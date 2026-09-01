# Alternatives à django-jazzmin — Guide d'installation complet

Ce guide couvre l'installation détaillée des meilleures alternatives à `django-jazzmin` : **django-unfold**, **django-baton**, **django-jet-reboot**, **django-admin-interface** et **Kubi**.

---

## 1. django-unfold — Le plus moderne (Tailwind CSS)

**Prérequis :** Django 5.2+ et Python 3.12+

### Installation

```bash
pip install django-unfold
```

### Configuration

Dans `settings.py`, ajoute `unfold` **avant** `django.contrib.admin` :

```python
INSTALLED_APPS = [
    "unfold",  # doit être avant django.contrib.admin
    "unfold.contrib.filters",       # optionnel, filtres avancés
    "unfold.contrib.forms",         # optionnel, widgets de formulaire
    "unfold.contrib.import_export", # optionnel, si tu utilises django-import-export
    "unfold.contrib.guardian",      # optionnel, si tu utilises django-guardian
    "unfold.contrib.simple_history",# optionnel, si tu utilises simple-history

    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    # ... tes apps
]
```

### Personnalisation (optionnelle)

```python
UNFOLD = {
    "SITE_TITLE": "Mon Administration",
    "SITE_HEADER": "Mon Projet",
    "SITE_URL": "/",
    "SITE_ICON": {
        "light": lambda request: "/static/icon-light.svg",
        "dark": lambda request: "/static/icon-dark.svg",
    },
    "SHOW_HISTORY": True,
    "SHOW_VIEW_ON_SITE": True,
    "COLORS": {
        "primary": {
            "50": "250 245 255",
            "500": "168 85 247",
            "900": "88 28 135",
        },
    },
    "SIDEBAR": {
        "show_search": True,
        "show_all_applications": True,
    },
}
```

### Migration + lancement

```bash
python manage.py migrate
python manage.py collectstatic
python manage.py runserver
```

C'est tout — l'admin est automatiquement reskiné avec Tailwind, dark mode inclus.

---

## 2. django-baton — Le plus proche de Jazzmin (migration facile)

**Compatible :** toutes versions récentes de Django (y compris 4.2 LTS)

### Installation

```bash
pip install django-baton
```

### Configuration

Remplace `django.contrib.admin` par la config Baton dans `settings.py` :

```python
INSTALLED_APPS = [
    'baton',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # ... tes apps
    'baton.autodiscover',  # doit être en DERNIER
]
```

Dans `urls.py` :

```python
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('baton/', include('baton.urls')),  # nécessaire pour certaines fonctionnalités JS
]
```

### Personnalisation

```python
BATON = {
    'SITE_HEADER': 'Mon Projet',
    'SITE_TITLE': 'Mon Projet Admin',
    'INDEX_TITLE': 'Tableau de bord',
    'SUPPORT_HREF': 'https://github.com/mon-repo/issues',
    'COPYRIGHT': 'copyright © 2026 Mon Entreprise',
    'POWERED_BY': '<a href="https://monsite.com">Mon Entreprise</a>',
    'CONFIRM_UNSAVED_CHANGES': True,
    'SHOW_MULTIPART_UPLOADING': True,
    'ENABLE_IMAGES_PREVIEW': True,
    'CHANGELIST_FILTERS_IN_MODAL': True,
    'MENU_ALWAYS_COLLAPSED': False,
    'MENU_TITLE': 'Menu',
    'MESSAGES_TOASTS': False,
    'GRAVATAR_DEFAULT_IMG': 'retro',
    'MENU': (
        {'type': 'title', 'label': 'Général', 'apps': ('auth',)},
        {'type': 'model', 'label': 'Utilisateurs', 'name': 'user', 'app': 'auth'},
        {'type': 'model', 'label': 'Groupes', 'name': 'group', 'app': 'auth'},
        '-',
        {'type': 'free', 'label': 'Documentation', 'url': 'https://docs.monsite.com', 'perms': ('auth.view_user',)},
    ),
}
```

### Migration + lancement

```bash
python manage.py collectstatic
python manage.py runserver
```

Comme Jazzmin, tout se configure via un seul dictionnaire — la migration depuis Jazzmin est quasi directe.

---

## 3. django-jet-reboot — Fork actif de django-jet

### Installation

```bash
pip install django-jet-reboot
```

### Configuration

```python
INSTALLED_APPS = [
    'jet',                  # doit être avant django.contrib.admin
    'jet.dashboard',        # optionnel, pour le dashboard personnalisable
    'django.contrib.admin',
    # ... tes autres apps
]
```

Dans `urls.py` :

```python
from django.urls import path, include

urlpatterns = [
    path('jet/', include('jet.urls', 'jet')),
    path('jet/dashboard/', include('jet.dashboard.urls', 'jet-dashboard')),
    path('admin/', admin.site.urls),
]
```

### Migration + lancement

```bash
python manage.py migrate
python manage.py collectstatic
python manage.py runserver
```

Les utilisateurs peuvent choisir leur thème directement dans l'interface (icône en haut à droite).

---

## 4. django-admin-interface — Léger et personnalisable depuis l'admin

### Installation

```bash
pip install django-admin-interface
```

### Configuration

```python
INSTALLED_APPS = [
    'admin_interface',
    'colorfield',            # dépendance requise
    'django.contrib.admin',
    # ... tes autres apps
]

X_FRAME_OPTIONS = 'SAMEORIGIN'  # nécessaire pour la prévisualisation des thèmes
```

### Migration + lancement

```bash
python manage.py migrate
python manage.py collectstatic
python manage.py runserver
```

Ensuite, connecte-toi à `/admin/` → va dans **Admin Interface > Themes** pour personnaliser couleurs, logo, titre directement depuis l'UI, sans toucher au code.

---

## 5. Kubi — Bootstrap 5 + Sass

### Installation

```bash
pip install django-admin-kubi
```

### Configuration

```python
INSTALLED_APPS = [
    'kubi_admin',   # doit être avant django.contrib.admin
    'django.contrib.admin',
    # ... tes autres apps
]
```

### Migration + lancement

```bash
python manage.py collectstatic
python manage.py runserver
```

Compatible directement avec `django-modeltranslation`, `django-import-export`, `django-two-factor-auth`, `django-colorfield`.

---

## Tableau comparatif rapide

| Thème | Base UI | Config | Django requis | Points forts |
|---|---|---|---|---|
| **Unfold** | Tailwind CSS | Dict `UNFOLD` | 5.2+ / Python 3.12+ | Le plus moderne, dark mode natif |
| **Baton** | Bootstrap 5 | Dict `BATON` | Toutes versions récentes | Migration facile depuis Jazzmin, IA optionnelle |
| **Jet Reboot** | Custom | `INSTALLED_APPS` + urls | 3/4/5+ | Dashboard modulable, thèmes multiples |
| **Admin Interface** | Flat/responsive | Interface admin (UI) | Toutes versions | Zéro code, tout se fait dans l'admin |
| **Kubi** | Bootstrap 5 + Sass | `INSTALLED_APPS` | Toutes versions récentes | Compatible nombreux packages tiers |

---

## Recommandation finale

- **Django ≥ 5.2** → pars sur **Unfold**, c'est la référence 2026.
- **Django < 5.2 (LTS 4.2 par exemple)** → **Baton** est le meilleur compromis, migration quasi transparente depuis Jazzmin.
- **Besoin de personnalisation sans toucher au code** → **Admin Interface**.
