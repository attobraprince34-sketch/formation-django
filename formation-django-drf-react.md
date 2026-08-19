# De Zéro à Développeur Full-Stack : Django, DRF, React & IA

> Formation complète pour apprendre à construire des applications web robustes avec **Django**, **Django REST Framework** et **React**, jusqu'à l'intégration d'agents IA.
>
> Public visé : débutant motivé, à l'aise avec les bases de Python et de la programmation (variables, fonctions, boucles, un peu de HTML/CSS/JS).

![Django logo](https://commons.wikimedia.org/wiki/Special:FilePath/Django_logo.svg)
![React logo](https://commons.wikimedia.org/wiki/Special:FilePath/React-icon.svg)

---

## Table des matières

1. [Avant de commencer](#1-avant-de-commencer)
2. [Module 1 — Les fondations de Django](#module-1--les-fondations-de-django)
3. [Module 2 — Le modèle MVT et l'ORM en profondeur](#module-2--le-modèle-mvt-et-lorm-en-profondeur)
4. [Module 3 — L'interface d'administration Django (customisation avancée)](#module-3--linterface-dadministration-django-customisation-avancée)
5. [Module 4 — Django REST Framework](#module-4--django-rest-framework)
6. [Module 5 — Authentification & permissions avec DRF](#module-5--authentification--permissions-avec-drf)
7. [Module 6 — React, les fondations](#module-6--react-les-fondations)
8. [Module 7 — Connecter React à une API DRF](#module-7--connecter-react-à-une-api-drf)
9. [Module 8 — Vers des applications robustes (tests, structure, déploiement)](#module-8--vers-des-applications-robustes-tests-structure-déploiement)
10. [Module 9 — Intégrer des agents IA dans une stack Django + React](#module-9--intégrer-des-agents-ia-dans-une-stack-django--react)
11. [Module 10 — Compléments Django avancés (Celery, signals, cache, i18n)](#module-10--compléments-django-avancés-celery-signals-cache-i18n)
12. [Module 11 — Compléments DRF avancés (throttling, versioning, schema/OpenAPI)](#module-11--compléments-drf-avancés-throttling-versioning-schemaopenapi)
13. [Module 12 — Compléments React avancés (gestion d'état, styling, performance, TypeScript, tests)](#module-12--compléments-react-avancés-gestion-détat-styling-performance-typescript-tests)
14. [Projets pour s'exercer](#projets-pour-sexercer)
15. [Ressources complémentaires](#ressources-complémentaires)

---

## 1. Avant de commencer

### Ce dont tu as besoin

- Python 3.11+ installé
- Node.js 18+ et npm installés
- Un éditeur de code (VS Code recommandé)
- Git installé, et un compte GitHub
- Connaître les bases : variables, fonctions, listes/dictionnaires en Python ; balises HTML de base

### Comment utiliser cette formation

Chaque module suit toujours la même logique :

1. **Le concept** — pourquoi ça existe, quel problème ça résout
2. **La pratique** — du code que tu tapes et exécutes toi-même (ne copie-colle pas sans comprendre)
3. **Le "sous le capot"** — ce qui se passe réellement quand ton code s'exécute
4. **Les pièges classiques** — les erreurs que tout débutant rencontre

> **Règle d'or** : ne passe jamais au module suivant si tu ne peux pas expliquer le précédent à voix haute, sans notes. Si tu bloques, c'est normal — reviens en arrière plutôt que d'avancer en pilotage automatique.

---

## Module 1 — Les fondations de Django

### 1.1 Qu'est-ce que Django, concrètement ?

Django est un **framework web** : un ensemble d'outils qui prennent en charge tout ce qui est répétitif dans la création d'un site (connexion à la base de données, sécurité, routage des URLs, formulaires...) pour que tu te concentres sur la logique métier de ton application.

Django suit le patron **MVT (Model - View - Template)** :

- **Model** : la structure de tes données (ex : un article a un titre, un contenu, une date)
- **View** : la logique qui décide quoi faire quand une URL est appelée (aller chercher des données, les traiter)
- **Template** : ce qui s'affiche à l'écran (HTML dynamique)

![Schéma MVT Django](https://commons.wikimedia.org/wiki/Special:FilePath/MTV_Django.svg)

C'est très proche du célèbre patron **MVC**, à un détail près : dans Django, c'est le framework lui-même qui joue une partie du rôle du "Contrôleur" (le routage des URLs), donc on parle plutôt de MVT.

### 1.2 Installation et premier projet

```bash
# Créer un environnement virtuel (isole les dépendances du projet)
python -m venv env
source env/bin/activate   # Sur Windows : env\Scripts\activate

# Installer Django
pip install django

# Créer un projet
django-admin startproject config .
```

**Ce que tu obtiens** :

```
.
├── manage.py
└── config/
    ├── __init__.py
    ├── settings.py   # La configuration globale du projet
    ├── urls.py       # Le routeur principal
    ├── asgi.py
    └── wsgi.py
```

Lance le serveur :

```bash
python manage.py runserver
```

Va sur `http://127.0.0.1:8000` — la fusée de Django doit apparaître. **Comprends bien ce qui vient de se passer** : `manage.py` a démarré un serveur de développement qui écoute les requêtes HTTP et les fait transiter par `config/urls.py`.

### 1.3 Les "apps" : la brique de base de Django

Un projet Django est découpé en **apps** (applications) : des modules indépendants et réutilisables. Un projet "blog" pourrait avoir une app `articles`, une app `comments`, une app `utilisateurs`.

```bash
python manage.py startapp articles
```

**Piège classique du débutant** : oublier de déclarer l'app dans `settings.py`. Sans ça, Django ignore complètement ton app.

```python
# config/settings.py
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "articles",  # ← ton app
]
```

### 1.4 Le premier Model

```python
# articles/models.py
from django.db import models

class Article(models.Model):
    titre = models.CharField(max_length=200)
    contenu = models.TextField()
    date_creation = models.DateTimeField(auto_now_add=True)
    publie = models.BooleanField(default=False)

    def __str__(self):
        return self.titre
```

Chaque attribut est un **champ** (`CharField`, `TextField`, `BooleanField`, `DateTimeField`...) qui correspond à une colonne dans la base de données.

### 1.5 Les migrations : traduire tes Models en base de données

```bash
python manage.py makemigrations   # Génère le "plan" du changement
python manage.py migrate          # Applique réellement le changement en base
```

**Comprends bien la différence** :
- `makemigrations` regarde tes `models.py` et écrit un fichier Python qui décrit le changement (créer une table, ajouter une colonne...)
- `migrate` exécute ce fichier contre la base de données réelle

C'est un système à deux étapes volontairement : ça te permet de relire/versionner les changements avant de les appliquer.

### 1.6 Django Admin : ta première victoire visuelle

```python
# articles/admin.py
from django.contrib import admin
from .models import Article

admin.site.register(Article)
```

Crée un compte administrateur puis lance le serveur :

```bash
python manage.py createsuperuser
python manage.py runserver
```

Va sur `http://127.0.0.1:8000/admin` — tu peux maintenant créer/modifier/supprimer des articles **sans écrire une seule vue**. C'est l'un des plus gros atouts de Django : un CRUD complet en quelques lignes.

### 1.7 Ta première vue et ton premier template

```python
# articles/views.py
from django.shortcuts import render
from .models import Article

def liste_articles(request):
    articles = Article.objects.filter(publie=True)
    return render(request, "articles/liste.html", {"articles": articles})
```

```python
# articles/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path("", views.liste_articles, name="liste_articles"),
]
```

```python
# config/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("articles/", include("articles.urls")),
]
```

```html
<!-- articles/templates/articles/liste.html -->
<h1>Nos articles</h1>
<ul>
  {% for article in articles %}
    <li>{{ article.titre }} — {{ article.date_creation }}</li>
  {% endfor %}
</ul>
```

**Le cycle complet que tu viens de coder** :

```
Requête HTTP → config/urls.py → articles/urls.py → views.py → models.py (ORM)
                                                          ↓
Réponse HTML ← liste.html ← render() ←──────────────────┘
```

C'est LE cycle que tu vas répéter des centaines de fois. Une fois qu'il est automatique pour toi, tout Django devient plus simple.

### 1.8 Formulaires : gérer la création de données

```python
# articles/forms.py
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    class Meta:
        model = Article
        fields = ["titre", "contenu", "publie"]
```

```python
# articles/views.py
from django.shortcuts import render, redirect
from .forms import ArticleForm

def creer_article(request):
    if request.method == "POST":
        form = ArticleForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect("liste_articles")
    else:
        form = ArticleForm()
    return render(request, "articles/creer.html", {"form": form})
```

**À comprendre absolument** : le cycle **GET / POST**.
- **GET** : "montre-moi le formulaire vide"
- **POST** : "voici les données que je veux envoyer, traite-les"

C'est ce même cycle que tu retrouveras plus tard côté API (DRF) et côté React.

---

## Module 2 — Le modèle MVT et l'ORM en profondeur

### 2.1 Les relations entre Models

```python
# articles/models.py
from django.db import models
from django.contrib.auth.models import User

class Categorie(models.Model):
    nom = models.CharField(max_length=100)

    def __str__(self):
        return self.nom

class Article(models.Model):
    titre = models.CharField(max_length=200)
    contenu = models.TextField()
    auteur = models.ForeignKey(User, on_delete=models.CASCADE, related_name="articles")
    categorie = models.ForeignKey(Categorie, on_delete=models.SET_NULL, null=True, blank=True)
    tags = models.ManyToManyField("Tag", blank=True)
    date_creation = models.DateTimeField(auto_now_add=True)
    publie = models.BooleanField(default=False)

    def __str__(self):
        return self.titre

class Tag(models.Model):
    nom = models.CharField(max_length=50)

    def __str__(self):
        return self.nom
```

- **`ForeignKey`** : relation "plusieurs vers un" (plusieurs articles → un auteur)
- **`ManyToManyField`** : relation "plusieurs vers plusieurs" (un article peut avoir plusieurs tags, un tag peut appartenir à plusieurs articles)
- **`on_delete`** : que se passe-t-il si l'objet lié est supprimé ? (`CASCADE` = supprime aussi, `SET_NULL` = met à null)

### 2.2 Les requêtes ORM courantes

L'ORM (Object-Relational Mapping) te permet d'écrire du Python au lieu de SQL brut :

```python
# Tous les articles
Article.objects.all()

# Filtrer
Article.objects.filter(publie=True)
Article.objects.filter(auteur__username="king")          # traverse la relation
Article.objects.filter(date_creation__year=2026)          # lookup sur une date
Article.objects.exclude(categorie__isnull=True)

# Récupérer un seul objet
Article.objects.get(id=1)                                 # lève une erreur si 0 ou >1 résultat

# Trier
Article.objects.order_by("-date_creation")                # "-" = décroissant

# Compter, agréger
Article.objects.filter(publie=True).count()

# Créer
Article.objects.create(titre="Mon article", contenu="...", auteur=user)

# Mettre à jour
article.titre = "Nouveau titre"
article.save()

# Supprimer
article.delete()
```

**Piège classique** : enchaîner trop de requêtes dans une boucle (problème "N+1 queries"). Utilise `select_related()` (pour ForeignKey) et `prefetch_related()` (pour ManyToMany) pour optimiser :

```python
# Mauvais : une requête par article pour récupérer l'auteur
for article in Article.objects.all():
    print(article.auteur.username)

# Bon : une seule requête avec jointure
for article in Article.objects.select_related("auteur"):
    print(article.auteur.username)
```

### 2.3 Les vues basées sur les classes (CBV)

Une fois les vues fonction (FBV) maîtrisées, les **Class-Based Views** évitent de réécrire le même code CRUD partout :

```python
from django.views.generic import ListView, DetailView, CreateView

class ArticleListView(ListView):
    model = Article
    template_name = "articles/liste.html"
    context_object_name = "articles"
    queryset = Article.objects.filter(publie=True)

class ArticleDetailView(DetailView):
    model = Article
    template_name = "articles/detail.html"

class ArticleCreateView(CreateView):
    model = Article
    fields = ["titre", "contenu", "categorie", "tags"]
    success_url = "/articles/"
```

```python
# urls.py
from .views import ArticleListView, ArticleDetailView, ArticleCreateView

urlpatterns = [
    path("", ArticleListView.as_view(), name="liste_articles"),
    path("<int:pk>/", ArticleDetailView.as_view(), name="detail_article"),
    path("nouveau/", ArticleCreateView.as_view(), name="creer_article"),
]
```

> **Conseil pédagogique** : n'utilise les CBV qu'une fois que tu es capable de reproduire leur comportement avec des FBV. Sinon, tu utilises de la magie que tu ne comprends pas.

---

## Module 3 — L'interface d'administration Django (customisation avancée)

C'est un point que beaucoup de tutoriels survolent, alors qu'il est extrêmement puissant en production : bien customisé, l'admin Django peut remplacer un back-office entier.

### 3.1 ModelAdmin : personnaliser l'affichage

```python
# articles/admin.py
from django.contrib import admin
from .models import Article, Categorie, Tag

@admin.register(Article)
class ArticleAdmin(admin.ModelAdmin):
    list_display = ("titre", "auteur", "categorie", "publie", "date_creation")
    list_filter = ("publie", "categorie", "date_creation")
    search_fields = ("titre", "contenu")
    list_editable = ("publie",)                 # éditable directement dans la liste
    prepopulated_fields = {"slug": ("titre",)}   # si tu as un champ slug
    date_hierarchy = "date_creation"
    ordering = ("-date_creation",)
    filter_horizontal = ("tags",)                # meilleure UX pour ManyToMany
    autocomplete_fields = ("auteur",)             # recherche au lieu d'un <select> géant
```

- **`list_display`** : les colonnes affichées dans la liste
- **`list_filter`** : les filtres dans la barre latérale
- **`search_fields`** : active la barre de recherche
- **`list_editable`** : modifie une valeur sans ouvrir la fiche
- **`autocomplete_fields`** : essentiel quand tu as des milliers d'objets liés

### 3.2 Actions personnalisées

```python
@admin.register(Article)
class ArticleAdmin(admin.ModelAdmin):
    list_display = ("titre", "publie")
    actions = ["publier_articles", "depublier_articles"]

    @admin.action(description="Publier les articles sélectionnés")
    def publier_articles(self, request, queryset):
        queryset.update(publie=True)

    @admin.action(description="Dépublier les articles sélectionnés")
    def depublier_articles(self, request, queryset):
        queryset.update(publie=False)
```

Tu peux maintenant sélectionner plusieurs articles dans l'admin et les publier en un clic.

### 3.3 Inlines : éditer des objets liés depuis la même page

```python
class TagInline(admin.TabularInline):   # ou admin.StackedInline
    model = Article.tags.through
    extra = 1

@admin.register(Article)
class ArticleAdmin(admin.ModelAdmin):
    inlines = [TagInline]
```

Utile pour un modèle "Commande" avec ses "LignesDeCommande" par exemple : tu édites tout sur une seule page.

### 3.4 Champs calculés et méthodes custom

```python
@admin.register(Article)
class ArticleAdmin(admin.ModelAdmin):
    list_display = ("titre", "nombre_de_tags")

    @admin.display(description="Nb. tags")
    def nombre_de_tags(self, obj):
        return obj.tags.count()
```

### 3.5 Restreindre les permissions dans l'admin

```python
class ArticleAdmin(admin.ModelAdmin):
    def has_delete_permission(self, request, obj=None):
        return request.user.is_superuser   # seuls les superusers peuvent supprimer

    def get_queryset(self, request):
        qs = super().get_queryset(request)
        if request.user.is_superuser:
            return qs
        return qs.filter(auteur=request.user)   # chaque auteur ne voit que ses articles
```

### 3.6 Personnaliser le site admin lui-même (branding)

```python
# config/admin.py (ou dans articles/admin.py)
from django.contrib import admin

admin.site.site_header = "Administration — MonProjet"
admin.site.site_title = "MonProjet Admin"
admin.site.index_title = "Tableau de bord"
```

Pour aller plus loin visuellement, des packages comme **django-jazzmin** ou **django-unfold** transforment l'admin en interface moderne (thème sombre, sidebar repensée, dashboard) sans toucher à ta logique métier :

```bash
pip install django-jazzmin
```

```python
# settings.py
INSTALLED_APPS = [
    "jazzmin",   # doit être AVANT django.contrib.admin
    "django.contrib.admin",
    # ...
]
```

### 3.7 Vues admin personnalisées (dashboard custom)

Tu peux même ajouter des pages complètement custom dans l'admin (un tableau de bord avec des statistiques par exemple) :

```python
from django.urls import path
from django.template.response import TemplateResponse

class MonAdminSite(admin.AdminSite):
    def get_urls(self):
        urls = super().get_urls()
        custom_urls = [
            path("dashboard/", self.admin_view(self.dashboard_view)),
        ]
        return custom_urls + urls

    def dashboard_view(self, request):
        context = dict(
            self.each_context(request),
            total_articles=Article.objects.count(),
            total_publies=Article.objects.filter(publie=True).count(),
        )
        return TemplateResponse(request, "admin/dashboard.html", context)
```

> À ce stade, tu es capable de construire un back-office complet et professionnel sans écrire une seule ligne de frontend. C'est une compétence énorme en freelance ou en entreprise : beaucoup de projets internes n'ont besoin que de ça.

---

## Module 4 — Django REST Framework

### 4.1 Le déclic à comprendre

Jusqu'ici, tes vues renvoient du **HTML**. DRF sert à faire la même chose mais en renvoyant du **JSON**, pour qu'un frontend (React, une app mobile, un autre service) puisse consommer tes données.

```bash
pip install djangorestframework
```

```python
# settings.py
INSTALLED_APPS = [
    # ...
    "rest_framework",
]
```

### 4.2 Les Serializers : transformer un Model en JSON

```python
# articles/serializers.py
from rest_framework import serializers
from .models import Article

class ArticleSerializer(serializers.ModelSerializer):
    auteur_nom = serializers.CharField(source="auteur.username", read_only=True)

    class Meta:
        model = Article
        fields = ["id", "titre", "contenu", "auteur_nom", "categorie", "publie", "date_creation"]
        read_only_fields = ["date_creation"]
```

Un `ModelSerializer` déduit automatiquement les champs à partir du Model — c'est l'équivalent d'un `ModelForm` côté API.

### 4.3 Les vues DRF : de APIView aux ViewSets

**Niveau 1 — APIView (le plus explicite, pour bien comprendre)** :

```python
# articles/views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import Article
from .serializers import ArticleSerializer

class ArticleListAPIView(APIView):
    def get(self, request):
        articles = Article.objects.filter(publie=True)
        serializer = ArticleSerializer(articles, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save(auteur=request.user)
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

**Niveau 2 — ViewSet (regroupe tout le CRUD)**, une fois que le niveau 1 est clair :

```python
from rest_framework import viewsets, permissions

class ArticleViewSet(viewsets.ModelViewSet):
    queryset = Article.objects.all()
    serializer_class = ArticleSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]

    def perform_create(self, serializer):
        serializer.save(auteur=self.request.user)
```

Un seul `ModelViewSet` remplace `list`, `create`, `retrieve`, `update`, `partial_update`, `destroy`.

### 4.4 Les Routers : générer les URLs automatiquement

```python
# articles/urls.py
from rest_framework.routers import DefaultRouter
from .views import ArticleViewSet

router = DefaultRouter()
router.register("articles", ArticleViewSet, basename="article")

urlpatterns = router.urls
```

```python
# config/urls.py
urlpatterns = [
    path("admin/", admin.site.urls),
    path("api/", include("articles.urls")),
]
```

Le router génère automatiquement :

| Méthode | URL | Action |
|---|---|---|
| GET | `/api/articles/` | liste |
| POST | `/api/articles/` | création |
| GET | `/api/articles/{id}/` | détail |
| PUT/PATCH | `/api/articles/{id}/` | mise à jour |
| DELETE | `/api/articles/{id}/` | suppression |

### 4.5 Tester avec l'API Browsable

Va sur `http://127.0.0.1:8000/api/articles/` dans ton navigateur : DRF affiche une interface HTML interactive pour tester ton API directement, avec formulaires. C'est extrêmement pratique pour déboguer avant même de brancher React.

> **Astuce pédagogique importante** : teste toujours ton endpoint avec l'API Browsable (ou Postman/Thunder Client) **avant** de le consommer depuis React. Ça permet de savoir immédiatement si le bug vient du backend ou du frontend.

### 4.6 Pagination, filtres et recherche

```python
# settings.py
REST_FRAMEWORK = {
    "DEFAULT_PAGINATION_CLASS": "rest_framework.pagination.PageNumberPagination",
    "PAGE_SIZE": 10,
}
```

```python
pip install django-filter
```

```python
from django_filters.rest_framework import DjangoFilterBackend
from rest_framework import filters

class ArticleViewSet(viewsets.ModelViewSet):
    queryset = Article.objects.all()
    serializer_class = ArticleSerializer
    filter_backends = [DjangoFilterBackend, filters.SearchFilter, filters.OrderingFilter]
    filterset_fields = ["categorie", "publie"]
    search_fields = ["titre", "contenu"]
    ordering_fields = ["date_creation"]
```

Maintenant `/api/articles/?categorie=2&search=django&ordering=-date_creation` fonctionne directement.

---

## Module 5 — Authentification & permissions avec DRF

### 5.1 JWT (JSON Web Token) : le standard pour les API modernes

```bash
pip install djangorestframework-simplejwt
```

```python
# settings.py
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "rest_framework_simplejwt.authentication.JWTAuthentication",
    ],
}
```

```python
# config/urls.py
from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView

urlpatterns = [
    # ...
    path("api/token/", TokenObtainPairView.as_view(), name="token_obtain_pair"),
    path("api/token/refresh/", TokenRefreshView.as_view(), name="token_refresh"),
]
```

**Comment ça fonctionne, concrètement** :

```
1. Le client envoie username/password → /api/token/
2. Le serveur renvoie un access_token (courte durée) + un refresh_token (longue durée)
3. Le client envoie l'access_token dans le header : Authorization: Bearer <token>
4. Quand l'access_token expire, le client utilise le refresh_token pour en obtenir un nouveau
```

### 5.2 Permissions

```python
from rest_framework import permissions

class EstProprietaireOuLectureSeule(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        if request.method in permissions.SAFE_METHODS:   # GET, HEAD, OPTIONS
            return True
        return obj.auteur == request.user

class ArticleViewSet(viewsets.ModelViewSet):
    permission_classes = [permissions.IsAuthenticatedOrReadOnly, EstProprietaireOuLectureSeule]
```

### 5.3 CORS : autoriser React à appeler ton API

C'est **l'erreur numéro 1** que tout débutant rencontre en connectant React à Django. Par sécurité, un navigateur bloque par défaut les requêtes entre deux origines différentes (`localhost:3000` → `localhost:8000`).

```bash
pip install django-cors-headers
```

```python
# settings.py
INSTALLED_APPS = [
    # ...
    "corsheaders",
]

MIDDLEWARE = [
    "corsheaders.middleware.CorsMiddleware",   # tout en haut
    # ...
]

CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",
    "http://localhost:5173",   # si tu utilises Vite
]
```

> **Pédagogiquement** : ne préviens pas cette erreur à l'avance auprès d'un apprenant. Laisse-le la rencontrer (message "blocked by CORS policy" dans la console du navigateur), puis explique pourquoi ça arrive. L'erreur comprise reste gravée bien plus longtemps que l'explication préventive.

---

## Module 6 — React, les fondations

### 6.1 Créer un projet React (avec Vite, plus rapide que Create React App)

```bash
npm create vite@latest frontend -- --template react
cd frontend
npm install
npm run dev
```

### 6.2 JSX et composants : la brique de base

Un composant React est **une fonction qui retourne du JSX** (du HTML dans du JavaScript) :

```jsx
// src/components/Bonjour.jsx
function Bonjour() {
  return <h1>Bonjour, monde !</h1>;
}

export default Bonjour;
```

### 6.3 Props : faire communiquer des composants

```jsx
function CarteArticle({ titre, contenu }) {
  return (
    <div className="carte">
      <h2>{titre}</h2>
      <p>{contenu}</p>
    </div>
  );
}

// Utilisation
<CarteArticle titre="Mon article" contenu="Un super contenu" />
```

### 6.4 useState : le premier Hook

Avant de toucher à l'API, isole `useState` sur un exemple simple, sans API, pour bien comprendre la logique de l'état :

```jsx
import { useState } from "react";

function Compteur() {
  const [compte, setCompte] = useState(0);

  return (
    <div>
      <p>Compte : {compte}</p>
      <button onClick={() => setCompte(compte + 1)}>+1</button>
    </div>
  );
}
```

**À bien comprendre** : `setCompte` ne modifie pas juste une variable, il dit à React "relance le rendu de ce composant avec cette nouvelle valeur". C'est le cœur de la réactivité de React.

### 6.5 useEffect : réagir à un changement (et préparer les appels API)

```jsx
import { useState, useEffect } from "react";

function Horloge() {
  const [heure, setHeure] = useState(new Date());

  useEffect(() => {
    const timer = setInterval(() => setHeure(new Date()), 1000);
    return () => clearInterval(timer);   // nettoyage
  }, []);   // [] = exécuté une seule fois au montage

  return <p>{heure.toLocaleTimeString()}</p>;
}
```

`useEffect` est la porte d'entrée pour tout ce qui est "effet de bord" : appels API, timers, abonnements à un événement.

---

## Module 7 — Connecter React à une API DRF

### 7.1 Ton premier appel API depuis React

```bash
npm install axios
```

```jsx
// src/components/ListeArticles.jsx
import { useState, useEffect } from "react";
import axios from "axios";

function ListeArticles() {
  const [articles, setArticles] = useState([]);
  const [chargement, setChargement] = useState(true);

  useEffect(() => {
    axios.get("http://127.0.0.1:8000/api/articles/")
      .then((response) => setArticles(response.data.results))
      .catch((erreur) => console.error(erreur))
      .finally(() => setChargement(false));
  }, []);

  if (chargement) return <p>Chargement...</p>;

  return (
    <ul>
      {articles.map((article) => (
        <li key={article.id}>{article.titre}</li>
      ))}
    </ul>
  );
}

export default ListeArticles;
```

C'est le moment clé de toute la formation : ton **Django (données) → DRF (API) → React (affichage)** sont maintenant connectés bout en bout.

### 7.2 Envoyer des données (formulaire React → API DRF)

```jsx
function FormulaireArticle() {
  const [titre, setTitre] = useState("");
  const [contenu, setContenu] = useState("");

  const envoyer = async (e) => {
    e.preventDefault();
    try {
      await axios.post("http://127.0.0.1:8000/api/articles/", { titre, contenu });
      setTitre("");
      setContenu("");
    } catch (erreur) {
      console.error(erreur.response?.data);
    }
  };

  return (
    <form onSubmit={envoyer}>
      <input value={titre} onChange={(e) => setTitre(e.target.value)} placeholder="Titre" />
      <textarea value={contenu} onChange={(e) => setContenu(e.target.value)} placeholder="Contenu" />
      <button type="submit">Publier</button>
    </form>
  );
}
```

### 7.3 Gérer l'authentification JWT côté React

```jsx
// src/api/auth.js
import axios from "axios";

const API = "http://127.0.0.1:8000/api";

export async function connexion(username, password) {
  const { data } = await axios.post(`${API}/token/`, { username, password });
  localStorage.setItem("access", data.access);
  localStorage.setItem("refresh", data.refresh);
  return data;
}

// Un client axios qui ajoute automatiquement le token à chaque requête
export const apiClient = axios.create({ baseURL: API });

apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem("access");
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});
```

> Pour la production, préférer des cookies `httpOnly` à `localStorage` pour stocker les tokens (protection contre le XSS) — mais `localStorage` reste très bien pour apprendre.

### 7.4 React Router : plusieurs pages

```bash
npm install react-router-dom
```

```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";
import ListeArticles from "./components/ListeArticles";
import DetailArticle from "./components/DetailArticle";

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Accueil</Link>
      </nav>
      <Routes>
        <Route path="/" element={<ListeArticles />} />
        <Route path="/articles/:id" element={<DetailArticle />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### 7.5 React Query : la vraie manière de gérer les appels API en production

`useState` + `useEffect` pour les appels API devient vite répétitif (gestion du chargement, des erreurs, du cache...). **React Query** (TanStack Query) résout ça :

```bash
npm install @tanstack/react-query
```

```jsx
import { useQuery } from "@tanstack/react-query";
import { apiClient } from "../api/auth";

function ListeArticles() {
  const { data, isLoading, error } = useQuery({
    queryKey: ["articles"],
    queryFn: () => apiClient.get("/articles/").then((res) => res.data.results),
  });

  if (isLoading) return <p>Chargement...</p>;
  if (error) return <p>Erreur : {error.message}</p>;

  return (
    <ul>
      {data.map((article) => <li key={article.id}>{article.titre}</li>)}
    </ul>
  );
}
```

React Query gère automatiquement le cache, le rechargement, les états de chargement/erreur. C'est le standard en entreprise aujourd'hui.

---

## Module 8 — Vers des applications robustes (tests, structure, déploiement)

### 8.1 Structurer un vrai projet Django

```
mon_projet/
├── config/                # settings, urls racine
├── apps/
│   ├── articles/
│   ├── utilisateurs/
│   └── commentaires/
├── requirements/
│   ├── base.txt
│   ├── dev.txt
│   └── prod.txt
├── .env                    # variables d'environnement (jamais commit !)
└── manage.py
```

### 8.2 Variables d'environnement (ne jamais coder en dur une clé secrète)

```bash
pip install python-decouple
```

```python
# settings.py
from decouple import config

SECRET_KEY = config("SECRET_KEY")
DEBUG = config("DEBUG", default=False, cast=bool)
DATABASE_URL = config("DATABASE_URL")
```

### 8.3 Tests

```python
# articles/tests.py
from django.test import TestCase
from django.contrib.auth.models import User
from rest_framework.test import APIClient

class ArticleAPITest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(username="king", password="motdepasse123")
        self.client = APIClient()

    def test_liste_articles_accessible_sans_auth(self):
        response = self.client.get("/api/articles/")
        self.assertEqual(response.status_code, 200)

    def test_creation_article_necessite_auth(self):
        response = self.client.post("/api/articles/", {"titre": "Test", "contenu": "..."})
        self.assertEqual(response.status_code, 401)
```

```bash
python manage.py test
```

**Pourquoi c'est essentiel** : un projet "robuste" n'est pas un projet qui marche une fois — c'est un projet qui continue de marcher après que tu (ou quelqu'un d'autre) l'ait modifié. Les tests sont ton filet de sécurité.

### 8.4 Base de données de production

En développement, Django utilise SQLite par défaut. En production, passe à **PostgreSQL** :

```bash
pip install psycopg2-binary dj-database-url
```

```python
# settings.py
import dj_database_url

DATABASES = {
    "default": dj_database_url.config(default=config("DATABASE_URL"))
}
```

### 8.5 Déploiement (aperçu)

- **Backend Django** : Railway, Render, ou un VPS avec Gunicorn + Nginx
- **Frontend React** : Vercel, Netlify (build statique via `npm run build`)
- **Fichiers statiques Django** : `whitenoise` ou un bucket S3
- **Variables sensibles** : jamais dans le code, toujours dans les variables d'environnement de la plateforme d'hébergement

---

## Module 9 — Intégrer des agents IA dans une stack Django + React

C'est ici que ta formation devient réellement différenciante : savoir brancher un LLM (comme Claude) dans une application réelle.

### 9.1 Le principe

**Ne jamais appeler l'API du LLM directement depuis React.** Ta clé API doit toujours rester côté serveur (Django) — sinon n'importe qui peut la voler en ouvrant les outils développeur du navigateur.

```
React (interface chat) → Django/DRF (endpoint sécurisé) → API du LLM → réponse → React
```

### 9.2 Un endpoint DRF qui appelle un LLM

```bash
pip install anthropic
```

```python
# agents/views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated
import anthropic
from decouple import config

client = anthropic.Anthropic(api_key=config("ANTHROPIC_API_KEY"))

class AgentChatView(APIView):
    permission_classes = [IsAuthenticated]

    def post(self, request):
        message_utilisateur = request.data.get("message", "")

        reponse = client.messages.create(
            model="claude-sonnet-4-6",
            max_tokens=1024,
            system="Tu es un assistant qui aide les utilisateurs de cette application.",
            messages=[{"role": "user", "content": message_utilisateur}],
        )

        texte = "".join(bloc.text for bloc in reponse.content if bloc.type == "text")
        return Response({"reponse": texte})
```

```python
# agents/urls.py
from django.urls import path
from .views import AgentChatView

urlpatterns = [
    path("chat/", AgentChatView.as_view(), name="agent_chat"),
]
```

### 9.3 Donner des "outils" à l'agent (tool use / function calling)

Un agent devient réellement utile quand il peut **agir** sur ta base de données, pas juste discuter :

```python
class AgentAvecOutilsView(APIView):
    def post(self, request):
        message = request.data.get("message", "")

        outils = [{
            "name": "rechercher_articles",
            "description": "Recherche des articles publiés par mot-clé",
            "input_schema": {
                "type": "object",
                "properties": {"mot_cle": {"type": "string"}},
                "required": ["mot_cle"],
            },
        }]

        reponse = client.messages.create(
            model="claude-sonnet-4-6",
            max_tokens=1024,
            tools=outils,
            messages=[{"role": "user", "content": message}],
        )

        for bloc in reponse.content:
            if bloc.type == "tool_use" and bloc.name == "rechercher_articles":
                mot_cle = bloc.input["mot_cle"]
                resultats = Article.objects.filter(titre__icontains=mot_cle, publie=True)
                # Renvoyer les résultats au modèle pour qu'il formule une réponse finale
                # (nécessite un second appel avec le tool_result — voir la doc Anthropic)

        return Response({"reponse": reponse.content})
```

> C'est ici que l'agent arrête d'être un simple chatbot et devient capable d'interagir avec les données réelles de ton application (chercher, créer, filtrer...).

### 9.4 Consommer l'agent depuis React (avec streaming simple)

```jsx
import { useState } from "react";
import { apiClient } from "../api/auth";

function ChatAgent() {
  const [message, setMessage] = useState("");
  const [historique, setHistorique] = useState([]);

  const envoyerMessage = async () => {
    setHistorique((h) => [...h, { role: "user", texte: message }]);
    const { data } = await apiClient.post("/agents/chat/", { message });
    setHistorique((h) => [...h, { role: "agent", texte: data.reponse }]);
    setMessage("");
  };

  return (
    <div>
      <div className="messages">
        {historique.map((m, i) => (
          <p key={i}><strong>{m.role} :</strong> {m.texte}</p>
        ))}
      </div>
      <input value={message} onChange={(e) => setMessage(e.target.value)} />
      <button onClick={envoyerMessage}>Envoyer</button>
    </div>
  );
}
```

### 9.5 Bonnes pratiques pour des agents en production

- **Rate limiting** : limite le nombre d'appels par utilisateur (DRF Throttling) pour éviter des factures API imprévues
- **Logs** : enregistre les échanges pour déboguer et améliorer les prompts
- **Validation stricte** : ne fais jamais confiance aveuglément aux `tool_use` retournés par le modèle — valide toujours les entrées avant d'exécuter une action réelle (suppression, paiement...)
- **Coûts** : surveille l'usage de tokens, mets des `max_tokens` raisonnables

---

## Module 10 — Compléments Django avancés (Celery, signals, cache, i18n)

### 10.1 Signals : réagir automatiquement à un événement

Un **signal** permet d'exécuter du code quand un événement précis se produit (sauvegarde, suppression...), sans polluer ta vue ou ton modèle principal.

```python
# articles/signals.py
from django.db.models.signals import post_save
from django.dispatch import receiver
from .models import Article

@receiver(post_save, sender=Article)
def notifier_publication(sender, instance, created, **kwargs):
    if created and instance.publie:
        print(f"Nouvel article publié : {instance.titre}")
```

```python
# articles/apps.py
from django.apps import AppConfig

class ArticlesConfig(AppConfig):
    name = "articles"

    def ready(self):
        import articles.signals   # important : charge le fichier au démarrage
```

**Piège classique** : oublier d'importer `signals.py` dans `ready()` — le signal n'est alors jamais connecté.

### 10.2 Celery : exécuter des tâches en arrière-plan

Certaines actions (envoyer un email, générer un PDF, appeler une API externe lente) ne doivent **jamais** bloquer la réponse HTTP. Celery exécute ces tâches de façon asynchrone, dans un processus séparé.

```bash
pip install celery redis
```

```python
# config/celery.py
import os
from celery import Celery

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings")
app = Celery("config")
app.config_from_object("django.conf:settings", namespace="CELERY")
app.autodiscover_tasks()
```

```python
# articles/tasks.py
from celery import shared_task
from django.core.mail import send_mail

@shared_task
def envoyer_email_bienvenue(email_utilisateur):
    send_mail(
        "Bienvenue !",
        "Merci de vous être inscrit.",
        "contact@monsite.com",
        [email_utilisateur],
    )
```

```python
# Dans une vue
envoyer_email_bienvenue.delay(user.email)   # .delay() = exécution asynchrone
```

Lancer le worker Celery :

```bash
celery -A config worker -l info
```

### 10.3 Le cache : accélérer les pages coûteuses

```python
# settings.py
CACHES = {
    "default": {
        "BACKEND": "django.core.cache.backends.redis.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
    }
}
```

```python
from django.core.cache import cache
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)   # cache la vue pendant 15 minutes
def liste_articles(request):
    ...

# Ou manuellement
def statistiques_view(request):
    stats = cache.get("stats_articles")
    if stats is None:
        stats = Article.objects.count()
        cache.set("stats_articles", stats, timeout=3600)
    return JsonResponse({"total": stats})
```

### 10.4 Internationalisation (i18n) : plusieurs langues

```python
# settings.py
USE_I18N = True
LANGUAGES = [("fr", "Français"), ("en", "English")]
LOCALE_PATHS = [BASE_DIR / "locale"]
```

```python
# Dans le code Python
from django.utils.translation import gettext as _
message = _("Article publié avec succès")
```

```html
<!-- Dans un template -->
{% load i18n %}
<p>{% trans "Bienvenue" %}</p>
```

```bash
django-admin makemessages -l en   # génère les fichiers de traduction
django-admin compilemessages       # les compile
```

### 10.5 Sécurité essentielle (CSRF, XSS, clickjacking)

Django protège automatiquement contre la plupart des attaques courantes, mais il faut comprendre ce qui se passe :

- **CSRF** (Cross-Site Request Forgery) : chaque formulaire POST doit inclure `{% csrf_token %}` ; les API DRF utilisant des tokens JWT n'en ont pas besoin (l'authentification par token suffit)
- **XSS** (Cross-Site Scripting) : les templates Django échappent automatiquement le HTML (`{{ variable }}`) — n'utilise `{{ variable|safe }}` que si tu es sûr du contenu
- **Clickjacking** : le middleware `XFrameOptionsMiddleware` (activé par défaut) empêche ton site d'être affiché dans une `<iframe>` malveillante

---

## Module 11 — Compléments DRF avancés (throttling, versioning, schema/OpenAPI)

### 11.1 Throttling : limiter le nombre de requêtes

Essentiel pour éviter les abus (et les factures API imprévues si tu appelles un LLM derrière).

```python
# settings.py
REST_FRAMEWORK = {
    "DEFAULT_THROTTLE_CLASSES": [
        "rest_framework.throttling.UserRateThrottle",
        "rest_framework.throttling.AnonRateThrottle",
    ],
    "DEFAULT_THROTTLE_RATES": {
        "user": "100/hour",
        "anon": "20/hour",
    },
}
```

Pour un endpoint spécifique (ex : l'agent IA du module 9) :

```python
from rest_framework.throttling import UserRateThrottle

class ThrottleAgentIA(UserRateThrottle):
    rate = "10/minute"

class AgentChatView(APIView):
    throttle_classes = [ThrottleAgentIA]
```

### 11.2 Versioning : faire évoluer une API sans casser les clients existants

```python
# settings.py
REST_FRAMEWORK = {
    "DEFAULT_VERSIONING_CLASS": "rest_framework.versioning.URLPathVersioning",
}
```

```python
# urls.py
urlpatterns = [
    path("api/v1/articles/", ArticleListV1.as_view()),
    path("api/v2/articles/", ArticleListV2.as_view()),
]
```

Ça permet de sortir une v2 de ton API (nouveaux champs, nouveau comportement) sans casser les applications déjà branchées sur la v1.

### 11.3 Gestion des exceptions personnalisée

```python
# config/exceptions.py
from rest_framework.views import exception_handler

def gestionnaire_exceptions_custom(exc, context):
    response = exception_handler(exc, context)
    if response is not None:
        response.data["status_code"] = response.status_code
    return response
```

```python
# settings.py
REST_FRAMEWORK = {
    "EXCEPTION_HANDLER": "config.exceptions.gestionnaire_exceptions_custom",
}
```

### 11.4 Documentation automatique (OpenAPI / Swagger)

```bash
pip install drf-spectacular
```

```python
# settings.py
INSTALLED_APPS += ["drf_spectacular"]
REST_FRAMEWORK = {
    "DEFAULT_SCHEMA_CLASS": "drf_spectacular.openapi.AutoSchema",
}
```

```python
# urls.py
from drf_spectacular.views import SpectacularAPIView, SpectacularSwaggerView

urlpatterns = [
    path("api/schema/", SpectacularAPIView.as_view(), name="schema"),
    path("api/docs/", SpectacularSwaggerView.as_view(url_name="schema")),
]
```

Va sur `/api/docs/` : une documentation interactive de toute ton API est générée automatiquement, avec possibilité de tester chaque endpoint. Extrêmement utile quand une équipe frontend doit consommer ton API sans lire ton code.

### 11.5 Content negotiation

DRF peut répondre en JSON, XML, ou tout autre format selon le header `Accept` envoyé par le client :

```python
REST_FRAMEWORK = {
    "DEFAULT_RENDERER_CLASSES": [
        "rest_framework.renderers.JSONRenderer",
        "rest_framework.renderers.BrowsableAPIRenderer",   # désactive-le en prod si besoin
    ],
}
```

---

## Module 12 — Compléments React avancés (gestion d'état, styling, performance, TypeScript, tests)

### 12.1 Gestion d'état globale : Context API, Redux, Zustand

Quand plusieurs composants éloignés dans l'arbre ont besoin des mêmes données (ex : l'utilisateur connecté), passer les props manuellement devient vite ingérable ("prop drilling").

**Context API (intégré à React, pour des besoins simples)** :

```jsx
import { createContext, useContext, useState } from "react";

const AuthContext = createContext();

export function AuthProvider({ children }) {
  const [utilisateur, setUtilisateur] = useState(null);
  return (
    <AuthContext.Provider value={{ utilisateur, setUtilisateur }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  return useContext(AuthContext);
}

// Dans n'importe quel composant enfant
function Profil() {
  const { utilisateur } = useAuth();
  return <p>Connecté en tant que {utilisateur?.username}</p>;
}
```

**Zustand (plus simple que Redux, très utilisé en 2026)** :

```bash
npm install zustand
```

```jsx
import { create } from "zustand";

const useAuthStore = create((set) => ({
  utilisateur: null,
  connecter: (utilisateur) => set({ utilisateur }),
  deconnecter: () => set({ utilisateur: null }),
}));

// Utilisation directe, sans Provider
function Profil() {
  const utilisateur = useAuthStore((state) => state.utilisateur);
  return <p>{utilisateur?.username}</p>;
}
```

> **Conseil** : commence par Context API pour comprendre le problème qu'elle résout. Passe à Zustand ou Redux seulement quand ton état global devient complexe (beaucoup d'actions, de logique dérivée).

### 12.2 Styling : les options principales

- **CSS Modules** : `import styles from "./Bouton.module.css"` — classes scoppées automatiquement au composant
- **Tailwind CSS** : classes utilitaires directement dans le JSX (`className="flex items-center gap-2"`), très rapide en développement
- **styled-components** : CSS directement en JavaScript, avec des props dynamiques

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```jsx
function Bouton({ enfants }) {
  return (
    <button className="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700">
      {enfants}
    </button>
  );
}
```

### 12.3 Performance : lazy loading et Suspense

Pour ne pas charger tout le JavaScript de l'application d'un coup :

```jsx
import { lazy, Suspense } from "react";

const PageAdmin = lazy(() => import("./pages/PageAdmin"));

function App() {
  return (
    <Suspense fallback={<p>Chargement...</p>}>
      <PageAdmin />
    </Suspense>
  );
}
```

Utile pour des pages lourdes (dashboard avec graphiques) que tout le monde ne visite pas.

### 12.4 useRef et useMemo/useCallback

```jsx
import { useRef, useMemo, useCallback } from "react";

function Recherche({ articles }) {
  const inputRef = useRef(null);   // accéder directement à un élément du DOM

  const focus = () => inputRef.current.focus();

  // Recalculé seulement si `articles` change (évite un recalcul à chaque rendu)
  const articlesTries = useMemo(
    () => [...articles].sort((a, b) => a.titre.localeCompare(b.titre)),
    [articles]
  );

  // Évite de recréer la fonction à chaque rendu (utile avec React.memo)
  const gererClic = useCallback(() => console.log("clic"), []);

  return <input ref={inputRef} />;
}
```

> Ne les utilise pas partout par réflexe — `useMemo`/`useCallback` n'apportent un vrai gain que sur des calculs coûteux ou des listes longues. Sur un petit composant, ils ajoutent de la complexité pour rien.

### 12.5 TypeScript avec React

```bash
npm create vite@latest frontend -- --template react-ts
```

```tsx
type Article = {
  id: number;
  titre: string;
  contenu: string;
  publie: boolean;
};

function CarteArticle({ article }: { article: Article }) {
  return <h2>{article.titre}</h2>;
}
```

TypeScript détecte les erreurs de type **avant** l'exécution (ex : oublier un champ obligatoire) — précieux quand un projet grandit et que plusieurs personnes y contribuent.

### 12.6 Tester des composants React

```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom
```

```jsx
// Compteur.test.jsx
import { render, screen, fireEvent } from "@testing-library/react";
import { test, expect } from "vitest";
import Compteur from "./Compteur";

test("incrémente le compte au clic", () => {
  render(<Compteur />);
  const bouton = screen.getByText("+1");
  fireEvent.click(bouton);
  expect(screen.getByText("Compte : 1")).toBeInTheDocument();
});
```

### 12.7 Build tools : Vite vs Webpack vs Create React App

| Outil | Statut | Quand l'utiliser |
|---|---|---|
| **Vite** | Recommandé aujourd'hui | Démarrage quasi instantané, standard actuel — utilisé dans toute cette formation |
| **Webpack** | Toujours utilisé en entreprise (projets existants) | Projets legacy, configuration très fine nécessaire |
| **Create React App** | Déprécié officiellement | À éviter pour un nouveau projet |

---

## Projets pour s'exercer

Fais-les **dans l'ordre**. Ne passe pas au suivant tant que le précédent n'est pas fonctionnel de bout en bout (Django + DRF + React).

### Niveau débutant

1. **Gestionnaire de tâches (To-Do List)** — CRUD simple : créer, lister, marquer comme fait, supprimer. Admin customisé avec `list_editable` sur le statut.
2. **Carnet de contacts** — Model avec plusieurs champs (nom, téléphone, email, catégorie), recherche et filtres dans l'admin ET dans l'API (`SearchFilter`).
3. **Blog personnel** — Reprends le fil rouge de cette formation : articles, catégories, tags, authentification JWT pour la création.

### Niveau intermédiaire

4. **Plateforme de recettes de cuisine** — Relations ManyToMany (ingrédients), upload d'images, filtres avancés (temps de préparation, difficulté), pagination.
5. **Suivi de dépenses personnelles** — Graphiques côté React (librairie `recharts`), agrégations Django (`Sum`, `Avg` par mois/catégorie), export CSV.
6. **Petit réseau social (mini Twitter)** — Posts, likes, follow entre utilisateurs, fil d'actualité paginé, permissions fines (un utilisateur ne peut modifier que ses propres posts).

### Niveau avancé

7. **Plateforme e-commerce simplifiée** — Produits, panier, commandes, stock, dashboard admin avec statistiques de ventes (vues admin custom), paiement simulé.
8. **Système de réservation (ex : salle de sport, rendez-vous)** — Gestion de créneaux, contraintes de disponibilité, notifications par email (Django signals + Celery).
9. **Outil de gestion de projet type Trello** — Tableaux, colonnes, cartes, drag & drop côté React (`@dnd-kit`), WebSockets pour la mise à jour en temps réel (Django Channels).

### Niveau expert (avec IA)

10. **Assistant IA de support client** — Un agent Django/DRF qui répond aux questions des utilisateurs en s'appuyant sur une base de connaissances (FAQ en base de données), avec historique de conversation persistant.
11. **Générateur de contenu assisté par IA** — L'utilisateur décrit un sujet, l'agent génère un brouillon d'article stocké en base, l'utilisateur l'édite ensuite dans React avant publication.
12. **Agent avec accès aux données de l'application** — Un agent capable de répondre à "combien de ventes ce mois-ci ?" en interrogeant réellement ta base via des `tools` (function calling), avec limitation stricte de ce qu'il peut faire (lecture seule, jamais de suppression directe).

> Pour chacun de ces projets : commence toujours par les Models, puis l'admin, puis l'API DRF (testée via l'API Browsable), puis seulement à la fin le frontend React.

---

## Ressources complémentaires

- Documentation officielle Django : https://docs.djangoproject.com/
- Documentation officielle DRF : https://www.django-rest-framework.org/
- Documentation officielle React : https://react.dev/
- Documentation TanStack Query : https://tanstack.com/query/
- Documentation API Anthropic (Claude) : https://docs.claude.com/

---

*Formation rédigée pour un apprentissage progressif, projet par projet. N'hésite pas à casser volontairement ton code pour comprendre les messages d'erreur — c'est souvent là que l'apprentissage réel se produit.*
