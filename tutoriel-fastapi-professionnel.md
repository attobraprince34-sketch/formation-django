# Tutoriel FastAPI — De zéro à un projet professionnel

> Public visé : développeurs ayant déjà de l'expérience avec Flask/Django, qui veulent utiliser FastAPI en conditions réelles (API pro, prête pour la prod).

---

## Table des matières

1. [Pourquoi FastAPI ?](#1-pourquoi-fastapi-)
2. [Installation et premier projet](#2-installation-et-premier-projet)
3. [Les bases : routes, méthodes HTTP, paramètres](#3-les-bases--routes-méthodes-http-paramètres)
4. [Pydantic : modèles de données et validation](#4-pydantic--modèles-de-données-et-validation)
5. [Structurer un vrai projet (architecture)](#5-structurer-un-vrai-projet-architecture)
6. [Base de données avec SQLAlchemy async](#6-base-de-données-avec-sqlalchemy-async)
7. [Injection de dépendances](#7-injection-de-dépendances)
8. [Authentification & sécurité (JWT, OAuth2)](#8-authentification--sécurité-jwt-oauth2)
9. [Gestion des erreurs](#9-gestion-des-erreurs)
10. [Tests avec pytest](#10-tests-avec-pytest)
11. [Documentation automatique](#11-documentation-automatique)
12. [Async : bonnes pratiques et pièges](#12-async--bonnes-pratiques-et-pièges)
13. [Sécurité approfondie](#13-sécurité-approfondie)
14. [Déploiement en production](#14-déploiement-en-production)
15. [Checklist projet pro](#15-checklist-projet-pro)

---

## 1. Pourquoi FastAPI ?

FastAPI est un framework web Python basé sur les **type hints** standard du langage. Il combine :

- **Starlette** pour la partie web/ASGI (routing, requêtes, websockets)
- **Pydantic** pour la validation et la sérialisation des données

Avantages concrets par rapport à Flask/Django :

| Aspect | Flask | Django | FastAPI |
|---|---|---|---|
| Validation des données | manuelle ou via extensions | forms/serializers | automatique via Pydantic |
| Documentation API | manuelle (Swagger à ajouter) | manuelle | générée automatiquement |
| Async natif | non (WSGI) | partiel (depuis 3.1) | oui, natif (ASGI) |
| Performance brute | moyenne | moyenne | élevée |
| Verbosité | faible | élevée (beaucoup de conventions) | faible à moyenne |

FastAPI est particulièrement adapté aux **APIs REST**, microservices, backends pour SPA/mobile. Pour un site full-stack avec templates rendus côté serveur, Django ou Flask+Jinja restent parfois plus simples.

---

## 2. Installation et premier projet

```bash
mkdir mon-projet-api && cd mon-projet-api
python3 -m venv venv
source venv/bin/activate   # sous Linux/Mac
pip install fastapi uvicorn[standard]
```

- `fastapi` : le framework
- `uvicorn` : le serveur ASGI qui fait tourner l'app (équivalent de `gunicorn`/`runserver`)

Premier fichier `main.py` :

```python
from fastapi import FastAPI

app = FastAPI(title="Mon API", version="1.0.0")

@app.get("/")
def read_root():
    return {"message": "Hello world"}
```

Lancer le serveur :

```bash
uvicorn main:app --reload
```

- `main` = nom du fichier `main.py`
- `app` = nom de l'instance FastAPI
- `--reload` = rechargement automatique en dev (à **ne jamais** utiliser en prod)

Va ensuite sur `http://127.0.0.1:8000/docs` : tu as déjà une documentation Swagger interactive, générée automatiquement.

---

## 3. Les bases : routes, méthodes HTTP, paramètres

### Méthodes HTTP

```python
@app.get("/items")
def list_items():
    return []

@app.post("/items")
def create_item():
    return {"status": "created"}

@app.put("/items/{item_id}")
def update_item(item_id: int):
    return {"item_id": item_id}

@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    return {"deleted": item_id}
```

### Paramètres de chemin (path parameters)

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):  # FastAPI valide automatiquement que c'est un int
    return {"user_id": user_id}
```

Si tu passes `/users/abc`, FastAPI renvoie automatiquement une erreur 422 avec un message clair — sans que tu aies écrit une ligne de validation.

### Paramètres de requête (query parameters)

```python
@app.get("/items")
def list_items(skip: int = 0, limit: int = 10, q: str | None = None):
    return {"skip": skip, "limit": limit, "q": q}
```

Appel : `/items?skip=20&limit=5&q=phone`

### Corps de requête (body)

Vu en détail dans la section Pydantic ci-dessous.

---

## 4. Pydantic : modèles de données et validation

C'est le cœur de FastAPI. Un modèle Pydantic définit la forme des données attendues.

```python
from pydantic import BaseModel, EmailStr, Field

class UserCreate(BaseModel):
    username: str = Field(min_length=3, max_length=50)
    email: EmailStr
    age: int = Field(gt=0, lt=120)
    is_active: bool = True

@app.post("/users")
def create_user(user: UserCreate):
    # user est déjà validé ici : si les données sont invalides,
    # FastAPI renvoie une erreur 422 avant même d'exécuter cette fonction
    return {"username": user.username, "email": user.email}
```

### Séparer modèles d'entrée et de sortie

Bonne pratique pro : ne jamais réutiliser le même modèle pour l'entrée (ce que l'utilisateur envoie) et la sortie (ce que l'API renvoie), notamment pour ne pas exposer de champs sensibles (mot de passe, hash, etc.).

```python
class UserCreate(BaseModel):
    username: str
    email: EmailStr
    password: str

class UserOut(BaseModel):
    id: int
    username: str
    email: EmailStr
    # pas de password ici !

    class Config:
        from_attributes = True  # permet de créer ce modèle depuis un objet ORM
```

```python
@app.post("/users", response_model=UserOut)
def create_user(user: UserCreate):
    # ... logique de création ...
    return created_user  # FastAPI filtre automatiquement selon UserOut
```

`response_model` garantit que même si tu renvoies un objet contenant plus de champs, seuls ceux du modèle de sortie apparaîtront dans la réponse. C'est un vrai filet de sécurité contre les fuites de données.

### Validation personnalisée

```python
from pydantic import field_validator

class UserCreate(BaseModel):
    username: str
    password: str

    @field_validator("password")
    @classmethod
    def password_strength(cls, v: str) -> str:
        if len(v) < 8:
            raise ValueError("Le mot de passe doit faire au moins 8 caractères")
        return v
```

---

## 5. Structurer un vrai projet (architecture)

Pour un projet Flask/Django, tu as l'habitude d'une structure imposée. FastAPI ne l'impose pas — voici une architecture éprouvée en production, inspirée du pattern "par domaine" :

```
mon-projet/
├── app/
│   ├── __init__.py
│   ├── main.py                 # point d'entrée, création de l'app
│   ├── core/
│   │   ├── config.py            # configuration (variables d'env)
│   │   ├── security.py          # hash, JWT
│   │   └── database.py          # connexion DB
│   ├── models/                  # modèles SQLAlchemy (ORM)
│   │   ├── user.py
│   │   └── item.py
│   ├── schemas/                 # modèles Pydantic (validation/sérialisation)
│   │   ├── user.py
│   │   └── item.py
│   ├── api/
│   │   ├── deps.py              # dépendances communes (get_db, get_current_user)
│   │   └── routes/
│   │       ├── users.py
│   │       ├── items.py
│   │       └── auth.py
│   ├── services/                # logique métier
│   │   └── user_service.py
│   └── tests/
│       ├── test_users.py
│       └── conftest.py
├── alembic/                      # migrations DB
├── .env
├── requirements.txt
└── Dockerfile
```

Principe clé : **séparer les couches**.
- `models/` = structure de la base de données
- `schemas/` = ce qui transite dans les requêtes/réponses HTTP
- `api/routes/` = uniquement le routage, pas de logique métier lourde
- `services/` = la vraie logique métier, testable indépendamment du framework

`main.py` devient alors très léger :

```python
from fastapi import FastAPI
from app.api.routes import users, items, auth

app = FastAPI(title="Mon API")

app.include_router(auth.router, prefix="/auth", tags=["auth"])
app.include_router(users.router, prefix="/users", tags=["users"])
app.include_router(items.router, prefix="/items", tags=["items"])
```

Et une route dans `api/routes/users.py` :

```python
from fastapi import APIRouter, Depends
from app.api import deps
from app.schemas.user import UserOut

router = APIRouter()

@router.get("/{user_id}", response_model=UserOut)
def get_user(user_id: int, db=Depends(deps.get_db)):
    return db.query(User).filter(User.id == user_id).first()
```

---

## 6. Base de données avec SQLAlchemy async

En pro, l'ORM le plus courant avec FastAPI est **SQLAlchemy** (en mode async) ou **SQLModel** (créé par l'auteur de FastAPI, plus simple, fusionne Pydantic + SQLAlchemy).

### Avec SQLAlchemy async

```bash
pip install sqlalchemy[asyncio] asyncpg alembic
```

`core/database.py` :

```python
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession, async_sessionmaker
from sqlalchemy.orm import DeclarativeBase

DATABASE_URL = "postgresql+asyncpg://user:password@localhost/mydb"

engine = create_async_engine(DATABASE_URL, echo=False)
AsyncSessionLocal = async_sessionmaker(engine, expire_on_commit=False)

class Base(DeclarativeBase):
    pass

async def get_db():
    async with AsyncSessionLocal() as session:
        yield session
```

`models/user.py` :

```python
from sqlalchemy.orm import Mapped, mapped_column
from app.core.database import Base

class User(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True)
    username: Mapped[str] = mapped_column(unique=True, index=True)
    email: Mapped[str] = mapped_column(unique=True)
    hashed_password: Mapped[str]
```

Utilisation dans une route :

```python
from sqlalchemy import select
from sqlalchemy.ext.asyncio import AsyncSession

@router.get("/{user_id}")
async def get_user(user_id: int, db: AsyncSession = Depends(get_db)):
    result = await db.execute(select(User).where(User.id == user_id))
    user = result.scalar_one_or_none()
    return user
```

### Migrations avec Alembic

```bash
alembic init alembic
# configurer alembic.ini et env.py pour pointer vers ton Base.metadata
alembic revision --autogenerate -m "création table users"
alembic upgrade head
```

Ne jamais faire `Base.metadata.create_all()` en production — toujours passer par des migrations versionnées.

---

## 7. Injection de dépendances

C'est un des points forts de FastAPI, très différent de ce que tu connais en Flask.

```python
from fastapi import Depends

def get_query_params(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}

@app.get("/items")
def list_items(params: dict = Depends(get_query_params)):
    return params
```

Utilité concrète : factoriser tout ce qui est répétitif (connexion DB, utilisateur courant, vérification de permissions).

```python
from fastapi import HTTPException, status

def get_current_user(token: str = Depends(oauth2_scheme), db=Depends(get_db)):
    user = decode_token_and_get_user(token, db)
    if not user:
        raise HTTPException(status_code=status.HTTP_401_UNAUTHORIZED)
    return user

def get_current_active_admin(user=Depends(get_current_user)):
    if not user.is_admin:
        raise HTTPException(status_code=403, detail="Accès réservé aux admins")
    return user

@router.delete("/{item_id}")
def delete_item(item_id: int, admin=Depends(get_current_active_admin)):
    # seul un admin authentifié arrive ici
    ...
```

Les dépendances peuvent s'enchaîner (dépendance d'une dépendance), et FastAPI les met en cache par requête si besoin (`use_cache=True` par défaut).

---

## 8. Authentification & sécurité (JWT, OAuth2)

Vu ton intérêt pour la cybersécurité, cette section mérite d'être solide.

### Hash des mots de passe

```bash
pip install passlib[bcrypt]
```

```python
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def hash_password(password: str) -> str:
    return pwd_context.hash(password)

def verify_password(plain: str, hashed: str) -> bool:
    return pwd_context.verify(plain, hashed)
```

Ne jamais stocker un mot de passe en clair, ni utiliser MD5/SHA1 seuls (pas de sel adaptatif).

### JWT avec OAuth2PasswordBearer

```bash
pip install python-jose[cryptography]
```

```python
from datetime import datetime, timedelta, timezone
from jose import jwt

SECRET_KEY = "à charger depuis une variable d'environnement, jamais en dur"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

def create_access_token(data: dict) -> str:
    to_encode = data.copy()
    expire = datetime.now(timezone.utc) + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
```

Route de login :

```python
from fastapi.security import OAuth2PasswordRequestForm

@router.post("/login")
async def login(form_data: OAuth2PasswordRequestForm = Depends(), db=Depends(get_db)):
    user = await authenticate_user(db, form_data.username, form_data.password)
    if not user:
        raise HTTPException(status_code=401, detail="Identifiants invalides")
    token = create_access_token({"sub": user.username})
    return {"access_token": token, "token_type": "bearer"}
```

Dépendance pour protéger une route :

```python
from fastapi.security import OAuth2PasswordBearer
from jose import JWTError

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="auth/login")

async def get_current_user(token: str = Depends(oauth2_scheme), db=Depends(get_db)):
    credentials_exception = HTTPException(
        status_code=401, detail="Impossible de valider les identifiants"
    )
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username = payload.get("sub")
        if username is None:
            raise credentials_exception
    except JWTError:
        raise credentials_exception
    user = await get_user_by_username(db, username)
    if user is None:
        raise credentials_exception
    return user
```

```python
@router.get("/me")
async def read_current_user(current_user=Depends(get_current_user)):
    return current_user
```

### Points de vigilance sécurité liés à l'auth

- Toujours stocker `SECRET_KEY` dans une variable d'environnement (jamais dans le code versionné)
- Prévoir un refresh token séparé de l'access token, avec durée de vie plus longue mais révocable
- Limiter le nombre de tentatives de login (rate limiting, voir section 13)
- Ne jamais renvoyer un message différent entre "utilisateur inexistant" et "mot de passe faux" (évite l'énumération de comptes)

---

## 9. Gestion des erreurs

### Exceptions HTTP standard

```python
from fastapi import HTTPException

@router.get("/{item_id}")
def get_item(item_id: int):
    item = find_item(item_id)
    if not item:
        raise HTTPException(status_code=404, detail="Item non trouvé")
    return item
```

### Exceptions personnalisées globales

```python
class ItemNotFoundError(Exception):
    def __init__(self, item_id: int):
        self.item_id = item_id

@app.exception_handler(ItemNotFoundError)
async def item_not_found_handler(request, exc: ItemNotFoundError):
    return JSONResponse(
        status_code=404,
        content={"message": f"L'item {exc.item_id} n'existe pas"},
    )
```

Cela permet de lever `raise ItemNotFoundError(item_id)` n'importe où dans la couche `services/` sans dépendre de FastAPI dans cette couche — bonne séparation des responsabilités.

### Format d'erreur cohérent

En pro, définis un format d'erreur unique pour toute l'API (utile pour le frontend) :

```python
{
  "error": {
    "code": "ITEM_NOT_FOUND",
    "message": "L'item demandé n'existe pas",
    "details": null
  }
}
```

---

## 10. Tests avec pytest

```bash
pip install pytest httpx pytest-asyncio
```

```python
# tests/conftest.py
import pytest
from httpx import AsyncClient, ASGITransport
from app.main import app

@pytest.fixture
async def client():
    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as ac:
        yield ac
```

```python
# tests/test_users.py
import pytest

@pytest.mark.asyncio
async def test_create_user(client):
    response = await client.post("/users", json={
        "username": "julien",
        "email": "julien@example.com",
        "password": "motdepasse123"
    })
    assert response.status_code == 200
    assert response.json()["username"] == "julien"

@pytest.mark.asyncio
async def test_create_user_invalid_email(client):
    response = await client.post("/users", json={
        "username": "julien",
        "email": "pas-un-email",
        "password": "motdepasse123"
    })
    assert response.status_code == 422
```

Utilise une base de données de test séparée (SQLite en mémoire ou une DB PostgreSQL dédiée aux tests), jamais la base de prod ou de dev.

---

## 11. Documentation automatique

FastAPI génère `/docs` (Swagger UI) et `/redoc` automatiquement. Tu peux enrichir la doc :

```python
@app.get(
    "/items/{item_id}",
    summary="Récupère un item par son ID",
    description="Retourne les détails complets d'un item, ou 404 s'il n'existe pas.",
    response_description="Les détails de l'item",
)
def get_item(item_id: int):
    ...
```

```python
class UserCreate(BaseModel):
    username: str = Field(..., example="julien_dev", description="Nom d'utilisateur unique")
    email: EmailStr = Field(..., example="julien@example.com")
```

En prod, il est courant de **désactiver `/docs` et `/redoc`** ou de les protéger par authentification, pour ne pas exposer la structure interne de l'API publiquement :

```python
app = FastAPI(
    docs_url=None if settings.ENV == "production" else "/docs",
    redoc_url=None if settings.ENV == "production" else "/redoc",
)
```

---

## 12. Async : bonnes pratiques et pièges

### Quand utiliser `async def` vs `def`

- `async def` : pour les opérations I/O (appels DB async, requêtes HTTP externes, fichiers async)
- `def` (synchrone) : FastAPI l'exécute automatiquement dans un threadpool séparé, donc c'est safe pour du code bloquant (calculs CPU, libs sync)

**Piège classique** : ne jamais faire d'appel bloquant à l'intérieur d'une fonction `async def` sans `await` :

```python
# MAUVAIS : bloque tout le event loop
@app.get("/bad")
async def bad_route():
    time.sleep(5)  # bloque TOUTES les requêtes en cours
    return {}

# BON : soit vraiment async, soit fonction sync
@app.get("/good")
def good_route():
    time.sleep(5)  # FastAPI l'exécute dans un thread séparé
    return {}
```

### Appels externes concurrents

```python
import httpx
import asyncio

async def fetch_multiple():
    async with httpx.AsyncClient() as client:
        responses = await asyncio.gather(
            client.get("https://api1.example.com"),
            client.get("https://api2.example.com"),
        )
    return responses
```

---

## 13. Sécurité approfondie

Points essentiels pour un projet pro (et particulièrement pertinents vu ton profil pentest) :

### CORS

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://mon-frontend.com"],  # jamais "*" en prod avec credentials
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE"],
    allow_headers=["*"],
)
```

### Rate limiting

```bash
pip install slowapi
```

```python
from slowapi import Limiter
from slowapi.util import get_remote_address

limiter = Limiter(key_func=get_remote_address)
app.state.limiter = limiter

@router.post("/login")
@limiter.limit("5/minute")
async def login(request: Request, ...):
    ...
```

### Validation stricte des entrées

Pydantic protège déjà contre beaucoup d'injections de structure, mais reste vigilant sur :
- **Injection SQL** : toujours utiliser l'ORM ou des requêtes paramétrées, jamais de f-string dans du SQL brut
- **Injection de commande** : ne jamais passer une entrée utilisateur brute à `subprocess`, `os.system`, etc.
- **SSRF** : si l'API fait des requêtes sortantes vers des URLs fournies par l'utilisateur, valider strictement le domaine

### En-têtes de sécurité

```python
@app.middleware("http")
async def add_security_headers(request, call_next):
    response = await call_next(request)
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["X-Frame-Options"] = "DENY"
    response.headers["Strict-Transport-Security"] = "max-age=63072000"
    return response
```

### Variables d'environnement avec Pydantic Settings

```bash
pip install pydantic-settings
```

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: str
    env: str = "development"

    class Config:
        env_file = ".env"

settings = Settings()
```

Ne jamais committer le fichier `.env` (mets-le dans `.gitignore`).

---

## 14. Déploiement en production

### Serveur : Uvicorn + Gunicorn

En prod, on ne lance pas `uvicorn --reload`. On utilise Gunicorn comme process manager avec des workers Uvicorn :

```bash
pip install gunicorn
gunicorn app.main:app -w 4 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
```

`-w 4` = 4 workers (généralement `2 x nombre_de_cores + 1`)

### Dockerfile type

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["gunicorn", "app.main:app", "-w", "4", "-k", "uvicorn.workers.UvicornWorker", "--bind", "0.0.0.0:8000"]
```

### Reverse proxy

En prod, Uvicorn/Gunicorn est presque toujours derrière **Nginx** ou **Traefik**, qui gère :
- Le TLS/HTTPS (certificats Let's Encrypt)
- La compression gzip
- Le cache de fichiers statiques
- Un premier filtrage/rate-limiting

### Observabilité

- **Logs structurés** : utiliser `logging` avec un format JSON pour faciliter l'agrégation
- **Healthcheck** : exposer une route `/health` simple, utilisée par le load balancer / orchestrateur
- **Monitoring** : Prometheus + Grafana, ou Sentry pour le suivi des erreurs

```python
@app.get("/health")
def health_check():
    return {"status": "ok"}
```

---

## 15. Checklist projet pro

Avant de livrer/déployer une API FastAPI en conditions professionnelles :

- [ ] Architecture en couches (routes / services / modèles / schémas)
- [ ] `response_model` défini sur toutes les routes qui renvoient des données sensibles
- [ ] Mots de passe hashés avec bcrypt/argon2, jamais en clair
- [ ] JWT avec expiration courte + refresh token
- [ ] Rate limiting sur les routes sensibles (login, reset password)
- [ ] CORS configuré avec une liste blanche d'origines, pas `"*"`
- [ ] Variables sensibles dans `.env`, jamais dans le code versionné
- [ ] Migrations DB versionnées avec Alembic
- [ ] Tests automatisés (au minimum les routes critiques et l'auth)
- [ ] `/docs` désactivé ou protégé en production
- [ ] Logs structurés + monitoring des erreurs
- [ ] Healthcheck exposé pour l'orchestrateur/load balancer
- [ ] HTTPS géré en amont (Nginx/Traefik + Let's Encrypt)
- [ ] Gunicorn + workers Uvicorn en prod, jamais `--reload`

---

## Pour aller plus loin

- Documentation officielle : https://fastapi.tiangolo.com
- SQLModel (alternative simplifiée à SQLAlchemy + Pydantic) : https://sqlmodel.tiangolo.com
- Full Stack FastAPI Template (référence d'architecture officielle) : dépôt GitHub `tiangolo/full-stack-fastapi-template`
