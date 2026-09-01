# Tutoriel : Implémenter l'OTP et l'authentification Google avec Django

Ce tutoriel couvre deux systèmes d'authentification complémentaires dans un projet Django :

1. **OTP (One-Time Password)** — vérification par code à usage unique (email ou SMS)
2. **Google Auth (OAuth 2.0)** — connexion via un compte Google

---

## Prérequis

- Python 3.10+
- Django installé (`pip install django`)
- Un projet Django déjà créé (`django-admin startproject core .`)
- Une app dédiée à l'authentification (`python manage.py startapp accounts`)

```bash
pip install django djangorestframework
```

Ajoutez `accounts` et `rest_framework` dans `INSTALLED_APPS` (settings.py) :

```python
INSTALLED_APPS = [
    # ...
    'rest_framework',
    'accounts',
]
```

---

## Partie 1 : Authentification par OTP

### 1.1 Principe

L'utilisateur saisit son email (ou numéro de téléphone), reçoit un code à 6 chiffres valide quelques minutes, puis le soumet pour être authentifié ou vérifié.

### 1.2 Modèle OTP

Dans `accounts/models.py` :

```python
import random
from datetime import timedelta
from django.conf import settings
from django.db import models
from django.utils import timezone


class OTP(models.Model):
    user = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
        related_name='otps'
    )
    code = models.CharField(max_length=6)
    created_at = models.DateTimeField(auto_now_add=True)
    is_used = models.BooleanField(default=False)

    @staticmethod
    def generate_code():
        return str(random.randint(100000, 999999))

    def is_expired(self):
        expiration = self.created_at + timedelta(minutes=5)
        return timezone.now() > expiration

    def __str__(self):
        return f"OTP({self.user}, {self.code})"
```

Migrations :

```bash
python manage.py makemigrations accounts
python manage.py migrate
```

### 1.3 Envoi du code par email

Configurez l'envoi d'emails dans `settings.py` (exemple avec Gmail SMTP, à adapter en prod avec un service comme SendGrid/Mailgun) :

```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'votre_email@gmail.com'
EMAIL_HOST_PASSWORD = 'votre_mot_de_passe_application'
```

Fonction utilitaire dans `accounts/utils.py` :

```python
from django.core.mail import send_mail
from .models import OTP


def send_otp(user):
    code = OTP.generate_code()
    OTP.objects.create(user=user, code=code)

    send_mail(
        subject="Votre code de vérification",
        message=f"Votre code OTP est : {code}. Il expire dans 5 minutes.",
        from_email=None,
        recipient_list=[user.email],
    )
    return code
```

### 1.4 Vues (avec Django REST Framework)

Dans `accounts/serializers.py` :

```python
from rest_framework import serializers
from django.contrib.auth import get_user_model

User = get_user_model()


class RequestOTPSerializer(serializers.Serializer):
    email = serializers.EmailField()


class VerifyOTPSerializer(serializers.Serializer):
    email = serializers.EmailField()
    code = serializers.CharField(max_length=6)
```

Dans `accounts/views.py` :

```python
from django.contrib.auth import get_user_model
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework_simplejwt.tokens import RefreshToken

from .models import OTP
from .serializers import RequestOTPSerializer, VerifyOTPSerializer
from .utils import send_otp

User = get_user_model()


class RequestOTPView(APIView):
    def post(self, request):
        serializer = RequestOTPSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        email = serializer.validated_data['email']

        user, _ = User.objects.get_or_create(
            email=email,
            defaults={'username': email}
        )
        send_otp(user)
        return Response({"detail": "Code envoyé par email."}, status=status.HTTP_200_OK)


class VerifyOTPView(APIView):
    def post(self, request):
        serializer = VerifyOTPSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        email = serializer.validated_data['email']
        code = serializer.validated_data['code']

        try:
            user = User.objects.get(email=email)
            otp = OTP.objects.filter(user=user, code=code, is_used=False).latest('created_at')
        except (User.DoesNotExist, OTP.DoesNotExist):
            return Response({"detail": "Code invalide."}, status=status.HTTP_400_BAD_REQUEST)

        if otp.is_expired():
            return Response({"detail": "Code expiré."}, status=status.HTTP_400_BAD_REQUEST)

        otp.is_used = True
        otp.save()

        refresh = RefreshToken.for_user(user)
        return Response({
            "access": str(refresh.access_token),
            "refresh": str(refresh),
        }, status=status.HTTP_200_OK)
```

> Nécessite `pip install djangorestframework-simplejwt` et l'ajout de `rest_framework_simplejwt` dans `INSTALLED_APPS`.

### 1.5 URLs

Dans `accounts/urls.py` :

```python
from django.urls import path
from .views import RequestOTPView, VerifyOTPView

urlpatterns = [
    path('otp/request/', RequestOTPView.as_view(), name='otp-request'),
    path('otp/verify/', VerifyOTPView.as_view(), name='otp-verify'),
]
```

Dans `core/urls.py` :

```python
from django.urls import path, include

urlpatterns = [
    # ...
    path('api/auth/', include('accounts.urls')),
]
```

### 1.6 Test rapide

```bash
curl -X POST http://localhost:8000/api/auth/otp/request/ -d "email=test@example.com"
curl -X POST http://localhost:8000/api/auth/otp/verify/ -d "email=test@example.com&code=123456"
```

---

## Partie 2 : Authentification Google (OAuth 2.0)

### 2.1 Installation

La bibliothèque la plus simple pour Django + DRF est `django-allauth` ou `google-auth` combiné à un flux manuel. Voici l'approche avec `google-auth` (contrôle total, adaptée à une API DRF) :

```bash
pip install google-auth
```

### 2.2 Créer les identifiants Google

1. Rendez-vous sur [Google Cloud Console](https://console.cloud.google.com/)
2. Créez un projet, puis allez dans **APIs & Services > Identifiants**
3. Créez un **ID client OAuth 2.0** (type "Application Web")
4. Ajoutez vos origines autorisées (ex : `http://localhost:3000` pour le frontend)
5. Récupérez le **Client ID**

Ajoutez-le dans `settings.py` :

```python
GOOGLE_CLIENT_ID = "VOTRE_CLIENT_ID.apps.googleusercontent.com"
```

### 2.3 Flux d'authentification

Le frontend (React, Flutter, etc.) utilise le SDK Google pour obtenir un **ID Token**, puis l'envoie à votre API Django, qui le vérifie côté serveur.

### 2.4 Vue de vérification du token Google

Dans `accounts/views.py` :

```python
from google.oauth2 import id_token
from google.auth.transport import requests as google_requests
from django.conf import settings
from django.contrib.auth import get_user_model
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework_simplejwt.tokens import RefreshToken

User = get_user_model()


class GoogleAuthView(APIView):
    def post(self, request):
        token = request.data.get('id_token')
        if not token:
            return Response({"detail": "Token manquant."}, status=status.HTTP_400_BAD_REQUEST)

        try:
            idinfo = id_token.verify_oauth2_token(
                token,
                google_requests.Request(),
                settings.GOOGLE_CLIENT_ID
            )
        except ValueError:
            return Response({"detail": "Token Google invalide."}, status=status.HTTP_400_BAD_REQUEST)

        email = idinfo.get('email')
        name = idinfo.get('name', '')

        if not email:
            return Response({"detail": "Email introuvable dans le token."}, status=status.HTTP_400_BAD_REQUEST)

        user, created = User.objects.get_or_create(
            email=email,
            defaults={'username': email, 'first_name': name}
        )

        refresh = RefreshToken.for_user(user)
        return Response({
            "access": str(refresh.access_token),
            "refresh": str(refresh),
            "created": created,
        }, status=status.HTTP_200_OK)
```

### 2.5 URL

```python
from .views import GoogleAuthView

urlpatterns = [
    # ...
    path('google/', GoogleAuthView.as_view(), name='google-auth'),
]
```

### 2.6 Exemple côté frontend (JavaScript)

```html
<script src="https://accounts.google.com/gsi/client" async defer></script>

<div id="g_id_onload"
     data-client_id="VOTRE_CLIENT_ID.apps.googleusercontent.com"
     data-callback="handleCredentialResponse">
</div>
<div class="g_id_signin" data-type="standard"></div>

<script>
function handleCredentialResponse(response) {
    fetch("http://localhost:8000/api/auth/google/", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ id_token: response.credential })
    })
    .then(res => res.json())
    .then(data => console.log(data));
}
</script>
```

### 2.7 Test avec curl (une fois l'ID token récupéré côté client)

```bash
curl -X POST http://localhost:8000/api/auth/google/ \
  -H "Content-Type: application/json" \
  -d '{"id_token": "VOTRE_ID_TOKEN"}'
```

---

## Bonnes pratiques

- **OTP** : limitez le nombre de tentatives (throttling DRF), invalidez les anciens codes à chaque nouvelle demande, et supprimez les OTP expirés via une tâche périodique (Celery ou `django-crontab`).
- **Google Auth** : ne faites jamais confiance à un token non vérifié côté serveur ; utilisez toujours `id_token.verify_oauth2_token`.
- Stockez les secrets (`EMAIL_HOST_PASSWORD`, `GOOGLE_CLIENT_ID`) dans des variables d'environnement (`django-environ` ou `python-decouple`), jamais en dur dans le code.
- Combinez les deux méthodes : Google Auth pour la connexion rapide, OTP comme second facteur ou vérification email/téléphone.

---

## Résumé

| Fonctionnalité | Package clé | Ce qu'il fait |
|---|---|---|
| OTP | Django natif + `send_mail` | Génère et vérifie un code à usage unique |
| Google Auth | `google-auth` | Vérifie un ID Token émis par Google |
| Sessions | `djangorestframework-simplejwt` | Génère les tokens JWT après authentification réussie |
