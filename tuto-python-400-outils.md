# 🐍 Le grand tutoriel Python — 400+ outils expliqués

Un guide pratique regroupant des centaines d'outils Python : ce qu'ils font, comment les utiliser, un exemple de code, la doc officielle, et 5 idées de projets pour chacun.

> **Note sur les logos** : les outils qui ont une identité visuelle officielle référencée par [Simple Icons](https://simpleicons.org/) affichent leur vrai logo (CDN `cdn.simpleicons.org`, libre d'usage). Pour les bibliothèques plus obscures qui n'ont pas de logo officiel, un badge coloré généré via [shields.io](https://shields.io) fait office de repère visuel.

> **Statut** : ce fichier est construit progressivement, message par message (400+ outils = document massif). La table des matières ci-dessous s'enrichit à chaque livraison.

---

## 📑 Table des matières

### Lot 1 — Mix équilibré (livré)
1. [Requests](#1-requests)
2. [Flask](#2-flask)
3. [Django](#3-django)
4. [FastAPI](#4-fastapi)
5. [SQLAlchemy](#5-sqlalchemy)
6. [Pandas](#6-pandas)
7. [NumPy](#7-numpy)
8. [Pytest](#8-pytest)
9. [Selenium](#9-selenium)
10. [Scrapy](#10-scrapy)
11. [Celery](#11-celery)
12. [Redis-py](#12-redis-py)
13. [Cryptography](#13-cryptography)
14. [Scapy](#14-scapy)
15. [Paramiko](#15-paramiko)
16. [Click](#16-click)
17. [Docker SDK for Python](#17-docker-sdk-for-python)
18. [Rich](#18-rich)
19. [Pillow](#19-pillow)
20. [BeautifulSoup4](#20-beautifulsoup4)

### Lot 2 — Mix équilibré (livré)
21. [Pydantic](#21-pydantic)
22. [Loguru](#22-loguru)
23. [Typer](#23-typer)
24. [aiohttp](#24-aiohttp)
25. [Uvicorn](#25-uvicorn)
26. [Jinja2](#26-jinja2)
27. [Marshmallow](#27-marshmallow)
28. [Faker](#28-faker)
29. [Hypothesis](#29-hypothesis)
30. [PyJWT](#30-pyjwt)
31. [Passlib](#31-passlib)
32. [python-nmap](#32-python-nmap)
33. [Matplotlib](#33-matplotlib)
34. [Seaborn](#34-seaborn)
35. [OpenCV-Python](#35-opencv-python)
36. [scikit-learn](#36-scikit-learn)
37. [PyTorch](#37-pytorch)
38. [python-dotenv](#38-python-dotenv)
39. [Watchdog](#39-watchdog)
40. [PyInstaller](#40-pyinstaller)

*(Lots suivants à venir : Sécurité/Pentest avancé, Data Science & ML, DevOps, Async, GUI, Audio/Vidéo, NLP, Vision, etc. — dis-moi "continue" pour la suite.)*

---

## 1. Requests

![requests](https://img.shields.io/badge/-requests-000000?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Réseau / HTTP
**Description :** La bibliothèque HTTP la plus utilisée en Python. Simplifie radicalement l'envoi de requêtes GET/POST, la gestion des headers, cookies, sessions, et l'authentification.

**Installation :**
```bash
pip install requests
```

**Exemple de code :**
```python
import requests

# GET simple
response = requests.get("https://api.github.com/users/octocat")
print(response.status_code)
print(response.json()["login"])

# POST avec données JSON et headers custom
payload = {"username": "test", "password": "secret"}
headers = {"Content-Type": "application/json"}
r = requests.post("https://httpbin.org/post", json=payload, headers=headers)
print(r.json())

# Session persistante (cookies conservés entre requêtes)
session = requests.Session()
session.get("https://example.com/login")
session.post("https://example.com/login", data={"user": "admin", "pass": "1234"})
```

**Documentation :** https://requests.readthedocs.io/

**5 projets pertinents :**
1. Un scraper de prix qui surveille un produit e-commerce et t'alerte par email s'il baisse
2. Un client CLI pour consommer une API publique (météo, crypto, actualités)
3. Un outil de test automatisé d'endpoints API (santé, latence, codes de retour)
4. Un bot qui interagit avec l'API Telegram/Discord pour poster des messages automatiques
5. Un vérificateur de liens morts pour un site web (crawl + vérif de chaque URL)

---

## 2. Flask

![flask](https://cdn.simpleicons.org/flask/000000)

**Catégorie :** Framework web (micro-framework)
**Description :** Framework web minimaliste et extensible. Idéal pour des APIs REST ou des petites/moyennes applications web, avec un contrôle total sur l'architecture.

**Installation :**
```bash
pip install flask
```

**Exemple de code :**
```python
from flask import Flask, request, jsonify

app = Flask(__name__)

tasks = []

@app.route("/tasks", methods=["GET"])
def get_tasks():
    return jsonify(tasks)

@app.route("/tasks", methods=["POST"])
def add_task():
    data = request.get_json()
    task = {"id": len(tasks) + 1, "title": data["title"], "done": False}
    tasks.append(task)
    return jsonify(task), 201

if __name__ == "__main__":
    app.run(debug=True)
```

**Documentation :** https://flask.palletsprojects.com/

**5 projets pertinents :**
1. Une API REST de gestion de tâches (todo-list) avec authentification JWT
2. Un blog personnel avec système de commentaires
3. Un raccourcisseur d'URLs façon Bit.ly
4. Un dashboard de monitoring de serveurs affichant CPU/RAM en temps réel
5. Une API de vérification de mots de passe compromis (couplée à une base de hashes)

---

## 3. Django

![django](https://cdn.simpleicons.org/django/092E20)

**Catégorie :** Framework web (full-stack)
**Description :** Framework web "batteries incluses" : ORM, authentification, panel admin, gestion des formulaires, sécurité intégrée. Pensé pour des applications complètes et robustes.

**Installation :**
```bash
pip install django
```

**Exemple de code :**
```python
# models.py
from django.db import models

class Article(models.Model):
    titre = models.CharField(max_length=200)
    contenu = models.TextField()
    date_publication = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.titre

# views.py
from django.shortcuts import render
from .models import Article

def liste_articles(request):
    articles = Article.objects.order_by("-date_publication")
    return render(request, "blog/liste.html", {"articles": articles})
```

```bash
python manage.py startproject monsite
python manage.py startapp blog
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```

**Documentation :** https://docs.djangoproject.com/

**5 projets pertinents :**
1. Une plateforme e-learning (cours, quiz, suivi de progression) — comme ton projet NEXUS ACADEMY
2. Un réseau social minimaliste (profils, posts, likes, follow)
3. Une marketplace avec panier et paiement (Stripe)
4. Un système de gestion de bibliothèque (emprunts, retours, réservations)
5. Un CRM simplifié pour petites entreprises

---

## 4. FastAPI

![fastapi](https://cdn.simpleicons.org/fastapi/009688)

**Catégorie :** Framework web (API moderne, async)
**Description :** Framework pour créer des APIs rapides basées sur les type hints Python. Génère automatiquement une documentation interactive (Swagger/OpenAPI) et valide les données via Pydantic.

**Installation :**
```bash
pip install fastapi uvicorn
```

**Exemple de code :**
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Utilisateur(BaseModel):
    nom: str
    age: int

@app.get("/")
def accueil():
    return {"message": "API en ligne"}

@app.post("/utilisateurs")
def creer_utilisateur(user: Utilisateur):
    return {"cree": True, "utilisateur": user}
```

```bash
uvicorn main:app --reload
# Documentation auto générée sur http://127.0.0.1:8000/docs
```

**Documentation :** https://fastapi.tiangolo.com/

**5 projets pertinents :**
1. Une API de scan de vulnérabilités qui déclenche des scans Nmap et renvoie les résultats en JSON
2. Un backend pour une application mobile de gestion de dépenses
3. Une API de reconnaissance d'image (upload + prédiction via un modèle ML)
4. Un service de génération de rapports PDF à la demande
5. Une passerelle API qui agrège plusieurs microservices

---

## 5. SQLAlchemy

![sqlalchemy](https://img.shields.io/badge/-SQLAlchemy-D71F00?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** ORM / accès base de données
**Description :** Toolkit SQL et ORM le plus complet de l'écosystème Python. Permet de manipuler une base de données avec des objets Python plutôt que du SQL brut, tout en gardant un contrôle fin si besoin.

**Installation :**
```bash
pip install sqlalchemy
```

**Exemple de code :**
```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, sessionmaker

Base = declarative_base()

class Utilisateur(Base):
    __tablename__ = "utilisateurs"
    id = Column(Integer, primary_key=True)
    nom = Column(String(50))
    email = Column(String(100), unique=True)

engine = create_engine("sqlite:///app.db")
Base.metadata.create_all(engine)

Session = sessionmaker(bind=engine)
session = Session()

nouvel_user = Utilisateur(nom="Kouassi", email="kouassi@example.com")
session.add(nouvel_user)
session.commit()

resultats = session.query(Utilisateur).filter_by(nom="Kouassi").all()
print(resultats)
```

**Documentation :** https://docs.sqlalchemy.org/

**5 projets pertinents :**
1. Migrer un projet Flask/Django utilisant du SQL brut vers un modèle ORM propre
2. Un système de réservation (salles, rendez-vous) avec gestion des conflits d'horaires
3. Un outil d'export/import de données entre bases (migration SQLite → PostgreSQL)
4. Une API d'inventaire pour une boutique en ligne
5. Un générateur de rapports statistiques à partir d'une base transactionnelle

---

## 6. Pandas

![pandas](https://cdn.simpleicons.org/pandas/150458)

**Catégorie :** Analyse de données
**Description :** La bibliothèque de référence pour manipuler des données tabulaires (DataFrames) : nettoyage, filtrage, agrégation, jointures, export vers Excel/CSV/SQL.

**Installation :**
```bash
pip install pandas
```

**Exemple de code :**
```python
import pandas as pd

df = pd.read_csv("ventes.csv")

# Filtrage et agrégation
ventes_2024 = df[df["annee"] == 2024]
total_par_produit = ventes_2024.groupby("produit")["montant"].sum().sort_values(ascending=False)
print(total_par_produit)

# Nettoyage
df = df.dropna(subset=["montant"])
df["montant"] = df["montant"].astype(float)

df.to_excel("rapport_ventes.xlsx", index=False)
```

**Documentation :** https://pandas.pydata.org/docs/

**5 projets pertinents :**
1. Un tableau de bord d'analyse de dépenses personnelles à partir d'exports bancaires CSV
2. Un outil de nettoyage automatique de jeux de données bruts pour un projet ML
3. Un rapport hebdomadaire automatique de statistiques sur des logs serveur
4. Un comparateur de performance entre plusieurs campagnes marketing
5. Un système de détection d'anomalies dans des transactions financières

---

## 7. NumPy

![numpy](https://cdn.simpleicons.org/numpy/013243)

**Catégorie :** Calcul scientifique
**Description :** Fondation du calcul numérique en Python : tableaux multidimensionnels performants et opérations vectorisées, bien plus rapides que les listes natives.

**Installation :**
```bash
pip install numpy
```

**Exemple de code :**
```python
import numpy as np

matrice = np.array([[1, 2, 3], [4, 5, 6]])
print(matrice.shape)          # (2, 3)
print(matrice.T)              # transposée
print(matrice * 2)            # opération vectorisée

# Génération et statistiques
donnees = np.random.normal(loc=50, scale=10, size=1000)
print("Moyenne:", donnees.mean())
print("Écart-type:", donnees.std())
```

**Documentation :** https://numpy.org/doc/

**5 projets pertinents :**
1. Un simulateur de dés/probabilités pour vérifier des stratégies de jeu
2. Un moteur de traitement d'image basique (flou, contraste) en manipulant des tableaux de pixels
3. Un outil de calcul matriciel pour un cours d'algèbre linéaire
4. Une simulation Monte-Carlo pour estimer un risque financier
5. Un mini-moteur physique 2D (positions/vitesses de particules)

---

## 8. Pytest

![pytest](https://cdn.simpleicons.org/pytest/0A9EDC)

**Catégorie :** Tests
**Description :** Framework de test le plus utilisé en Python. Syntaxe simple (assert natif), fixtures puissantes, et un écosystème de plugins immense.

**Installation :**
```bash
pip install pytest
```

**Exemple de code :**
```python
# calculatrice.py
def diviser(a, b):
    if b == 0:
        raise ValueError("Division par zéro impossible")
    return a / b

# test_calculatrice.py
import pytest
from calculatrice import diviser

def test_division_normale():
    assert diviser(10, 2) == 5

def test_division_par_zero():
    with pytest.raises(ValueError):
        diviser(10, 0)

@pytest.fixture
def valeurs():
    return {"a": 20, "b": 4}

def test_avec_fixture(valeurs):
    assert diviser(valeurs["a"], valeurs["b"]) == 5
```

```bash
pytest -v
```

**Documentation :** https://docs.pytest.org/

**5 projets pertinents :**
1. Ajouter une suite de tests complète à ton projet CVCraft ou NEXUS ACADEMY
2. Un pipeline CI (GitHub Actions) qui lance les tests à chaque push
3. Un générateur de rapport de couverture de code pour un projet existant
4. Une suite de tests d'intégration pour une API Flask/FastAPI
5. Un plugin pytest personnalisé pour un besoin spécifique de ton projet

---

## 9. Selenium

![selenium](https://cdn.simpleicons.org/selenium/43B02A)

**Catégorie :** Automatisation web / scraping
**Description :** Pilote de vrais navigateurs (Chrome, Firefox) pour automatiser des interactions web : clics, formulaires, scraping de contenu généré en JavaScript.

**Installation :**
```bash
pip install selenium
```

**Exemple de code :**
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys

driver = webdriver.Chrome()
driver.get("https://www.google.com")

barre_recherche = driver.find_element(By.NAME, "q")
barre_recherche.send_keys("Python tutoriel")
barre_recherche.send_keys(Keys.RETURN)

resultats = driver.find_elements(By.CSS_SELECTOR, "h3")
for r in resultats[:5]:
    print(r.text)

driver.quit()
```

**Documentation :** https://www.selenium.dev/documentation/

**5 projets pertinents :**
1. Un bot de test automatique de formulaire d'inscription pour ton portfolio CVCraft
2. Un scraper de sites nécessitant du JavaScript (SPA React/Vue)
3. Un outil de remplissage automatique de candidatures en ligne
4. Un moniteur de disponibilité de créneaux (rendez-vous, billets) avec alerte
5. Un testeur automatisé de parcours utilisateur (login → action → logout)

---

## 10. Scrapy

![scrapy](https://cdn.simpleicons.org/scrapy/60A839)

**Catégorie :** Scraping / crawling
**Description :** Framework complet pour crawler des sites à grande échelle : gestion des requêtes concurrentes, pipelines de traitement, export automatique en JSON/CSV.

**Installation :**
```bash
pip install scrapy
```

**Exemple de code :**
```python
import scrapy

class CitationsSpider(scrapy.Spider):
    name = "citations"
    start_urls = ["https://quotes.toscrape.com/"]

    def parse(self, response):
        for citation in response.css("div.quote"):
            yield {
                "texte": citation.css("span.text::text").get(),
                "auteur": citation.css("small.author::text").get(),
            }
        page_suivante = response.css("li.next a::attr(href)").get()
        if page_suivante:
            yield response.follow(page_suivante, self.parse)
```

```bash
scrapy runspider citations.py -o citations.json
```

**Documentation :** https://docs.scrapy.org/

**5 projets pertinents :**
1. Un agrégateur d'offres d'emploi tech en Côte d'Ivoire depuis plusieurs sites
2. Un comparateur de prix multi-sites pour un produit donné
3. Une base de données d'articles de blogs tech pour alimenter du contenu TikTok
4. Un crawler de veille sur les CVE/vulnérabilités récentes
5. Un scraper de contacts d'entreprises pour une campagne de candidature ciblée

---

## 11. Celery

![celery](https://img.shields.io/badge/-Celery-37814A?style=for-the-badge&logo=celery&logoColor=white)

**Catégorie :** Tâches asynchrones / files d'attente
**Description :** Système de files de tâches distribuées. Permet d'exécuter du code en arrière-plan (hors du cycle requête/réponse) — envoi d'emails, traitement de fichiers lourds, tâches planifiées.

**Installation :**
```bash
pip install celery redis
```

**Exemple de code :**
```python
# tasks.py
from celery import Celery

app = Celery("tasks", broker="redis://localhost:6379/0")

@app.task
def envoyer_email(destinataire, message):
    # logique d'envoi réelle ici
    print(f"Email envoyé à {destinataire}: {message}")
    return True

# Dans ton app Flask/Django :
# envoyer_email.delay("user@example.com", "Bienvenue !")
```

```bash
celery -A tasks worker --loglevel=info
```

**Documentation :** https://docs.celeryq.dev/

**5 projets pertinents :**
1. Un système d'envoi d'emails de confirmation en arrière-plan pour ton portfolio
2. Un pipeline de traitement d'images (redimensionnement, filtres) asynchrone
3. Un scraper planifié qui tourne chaque nuit et met à jour une base de données
4. Un système de notifications différées (rappels, relances)
5. Un générateur de rapports lourds (PDF/Excel) sans bloquer l'utilisateur

---

## 12. Redis-py

![redis](https://cdn.simpleicons.org/redis/DC382D)

**Catégorie :** Base de données clé-valeur / cache
**Description :** Client officiel Redis pour Python. Utilisé pour le cache, les sessions, les files de messages, ou comme broker pour Celery.

**Installation :**
```bash
pip install redis
```

**Exemple de code :**
```python
import redis

r = redis.Redis(host="localhost", port=6379, decode_responses=True)

r.set("utilisateur:1:nom", "Kouassi")
r.expire("utilisateur:1:nom", 3600)  # expire après 1h
print(r.get("utilisateur:1:nom"))

# Compteur atomique (utile pour rate limiting)
r.incr("visites_page_accueil")
print(r.get("visites_page_accueil"))

# Liste (file simple)
r.lpush("file_taches", "tache1", "tache2")
print(r.lrange("file_taches", 0, -1))
```

**Documentation :** https://redis.readthedocs.io/

**5 projets pertinents :**
1. Un système de cache pour accélérer les endpoints les plus lourds de ton API Flask
2. Un rate limiter (limite de requêtes par IP) pour protéger une API publique
3. Un compteur de vues en temps réel pour des articles de blog
4. Un système de session partagée entre plusieurs instances d'une app web
5. Un leaderboard de jeu (classement en temps réel via sorted sets Redis)

---

## 13. Cryptography

![cryptography](https://img.shields.io/badge/-cryptography-4B8BBE?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Sécurité / chiffrement
**Description :** Bibliothèque crypto moderne et sûre (recommandée par la communauté sécurité) pour le chiffrement symétrique/asymétrique, les signatures et le hachage.

**Installation :**
```bash
pip install cryptography
```

**Exemple de code :**
```python
from cryptography.fernet import Fernet

# Chiffrement symétrique simple
cle = Fernet.generate_key()
f = Fernet(cle)

message = b"Donnee sensible a proteger"
message_chiffre = f.encrypt(message)
print(message_chiffre)

message_dechiffre = f.decrypt(message_chiffre)
print(message_dechiffre.decode())
```

**Documentation :** https://cryptography.io/

**5 projets pertinents :**
1. Un gestionnaire de mots de passe en ligne de commande (coffre-fort chiffré local)
2. Un outil de chiffrement/déchiffrement de fichiers avant upload cloud
3. Un système de tokens signés pour sécuriser des liens de réinitialisation de mot de passe
4. Un chat chiffré de bout en bout en ligne de commande (démonstration pédagogique)
5. Un outil d'audit vérifiant la force des algorithmes utilisés dans un projet existant

---

## 14. Scapy

![scapy](https://img.shields.io/badge/-Scapy-FF6600?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Réseau / Pentest
**Description :** Manipulation de paquets réseau bas niveau : création, envoi, sniffing et analyse de paquets. Un classique pour l'apprentissage réseau offensif/défensif.

**Installation :**
```bash
pip install scapy
```

**Exemple de code :**
```python
from scapy.all import sniff, IP, TCP, sr1

# Sniffing basique (nécessite les droits admin/root)
def analyser_paquet(paquet):
    if paquet.haslayer(IP):
        print(f"{paquet[IP].src} -> {paquet[IP].dst}")

sniff(prn=analyser_paquet, count=10)

# Envoi d'un paquet SYN (scan de port simplifié)
paquet = IP(dst="scanme.nmap.org") / TCP(dport=80, flags="S")
reponse = sr1(paquet, timeout=2)
if reponse:
    print(reponse.summary())
```

> ⚠️ À utiliser uniquement sur des réseaux/machines que tu es autorisé à tester.

**Documentation :** https://scapy.readthedocs.io/

**5 projets pertinents :**
1. Un scanner de ports personnalisé (dans un cadre de lab autorisé)
2. Un analyseur de trafic réseau local pour détecter des anomalies
3. Un outil pédagogique de visualisation du fonctionnement du handshake TCP
4. Un détecteur d'ARP spoofing sur un réseau local de test
5. Un générateur de rapports de capture réseau (statistiques par protocole)

---

## 15. Paramiko

![paramiko](https://img.shields.io/badge/-Paramiko-3776AB?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Réseau / SSH
**Description :** Implémentation du protocole SSH en Python pur. Permet d'automatiser des connexions distantes, transferts de fichiers (SFTP) et exécution de commandes.

**Installation :**
```bash
pip install paramiko
```

**Exemple de code :**
```python
import paramiko

client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
client.connect("192.168.1.10", username="admin", password="motdepasse")

stdin, stdout, stderr = client.exec_command("uptime")
print(stdout.read().decode())

# Transfert de fichier via SFTP
sftp = client.open_sftp()
sftp.put("rapport.txt", "/home/admin/rapport.txt")
sftp.close()

client.close()
```

**Documentation :** https://www.paramiko.org/

**5 projets pertinents :**
1. Un outil de déploiement automatique sur un serveur distant (pull + restart service)
2. Un script d'audit qui se connecte à plusieurs serveurs et vérifie leur configuration
3. Un gestionnaire de sauvegardes automatiques via SFTP
4. Un outil de vérification de mots de passe SSH faibles (dans un cadre autorisé)
5. Un tableau de bord centralisé de santé de plusieurs serveurs (CPU, disque, uptime)

---

## 16. Click

![click](https://img.shields.io/badge/-Click-000000?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** CLI
**Description :** Framework pour créer des interfaces en ligne de commande propres, avec gestion automatique des options, arguments, aide (`--help`) et sous-commandes.

**Installation :**
```bash
pip install click
```

**Exemple de code :**
```python
import click

@click.group()
def cli():
    pass

@cli.command()
@click.option("--nom", prompt="Ton nom", help="Nom à saluer")
@click.option("--fois", default=1, help="Nombre de répétitions")
def saluer(nom, fois):
    for _ in range(fois):
        click.echo(f"Bonjour {nom} !")

@cli.command()
def version():
    click.echo("v1.0.0")

if __name__ == "__main__":
    cli()
```

```bash
python app.py saluer --nom Franck --fois 3
```

**Documentation :** https://click.palletsprojects.com/

**5 projets pertinents :**
1. Un outil CLI perso pour automatiser ton setup de projet (structure de dossiers, git init, venv)
2. Une CLI de gestion de ton portfolio CVCraft (génération de PDF, envoi)
3. Un outil CLI de conversion de fichiers (CSV vers JSON, images, etc.)
4. Un client CLI pour interagir avec ta propre API
5. Un outil CLI de scan rapide (ports, sous-domaines) pour du pentest léger

---

## 17. Docker SDK for Python

![docker](https://cdn.simpleicons.org/docker/2496ED)

**Catégorie :** DevOps / conteneurisation
**Description :** Permet de piloter Docker (créer, lancer, arrêter des conteneurs, gérer des images) directement depuis du code Python plutôt que via la CLI `docker`.

**Installation :**
```bash
pip install docker
```

**Exemple de code :**
```python
import docker

client = docker.from_env()

# Lancer un conteneur
conteneur = client.containers.run(
    "nginx:latest",
    detach=True,
    ports={"80/tcp": 8080},
    name="mon_nginx"
)
print(conteneur.status)

# Lister les conteneurs actifs
for c in client.containers.list():
    print(c.name, c.status)

conteneur.stop()
conteneur.remove()
```

**Documentation :** https://docker-py.readthedocs.io/

**5 projets pertinents :**
1. Un outil d'auto-déploiement qui build et lance un conteneur à chaque push Git
2. Un environnement de test isolé qui lance/détruit des conteneurs à la volée
3. Un dashboard web affichant l'état de tous tes conteneurs Docker locaux
4. Un lab de pentest automatisé (conteneurs vulnérables lancés à la demande)
5. Un système de nettoyage automatique des images/conteneurs inutilisés

---

## 18. Rich

![rich](https://img.shields.io/badge/-Rich-FAE742?style=for-the-badge&logo=python&logoColor=black)

**Catégorie :** Terminal / affichage
**Description :** Rend le terminal beaucoup plus lisible et agréable : tables, barres de progression, coloration syntaxique, panels, et tracebacks détaillés.

**Installation :**
```bash
pip install rich
```

**Exemple de code :**
```python
from rich.console import Console
from rich.table import Table
from rich.progress import track
import time

console = Console()

table = Table(title="Résultats de scan")
table.add_column("Port", style="cyan")
table.add_column("État", style="green")
table.add_row("22", "Ouvert")
table.add_row("80", "Ouvert")
table.add_row("443", "Fermé")
console.print(table)

for i in track(range(5), description="Traitement..."):
    time.sleep(0.3)
```

**Documentation :** https://rich.readthedocs.io/

**5 projets pertinents :**
1. Un habillage visuel pro pour tous tes outils CLI existants (scanner, scraper, etc.)
2. Un tableau de bord terminal affichant des métriques système en direct
3. Un outil de visualisation de logs colorés et filtrables
4. Un rapport de pentest formaté proprement en sortie terminal
5. Un jeu textuel en terminal avec interface soignée (menus, tableaux de score)

---

## 19. Pillow

![pillow](https://img.shields.io/badge/-Pillow-3776AB?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Traitement d'image
**Description :** Fork moderne de PIL, la bibliothèque de référence pour ouvrir, modifier, convertir et sauvegarder des images en Python.

**Installation :**
```bash
pip install pillow
```

**Exemple de code :**
```python
from PIL import Image, ImageFilter, ImageDraw, ImageFont

image = Image.open("photo.jpg")

# Redimensionner et appliquer un filtre
image_reduite = image.resize((400, 300))
image_floue = image_reduite.filter(ImageFilter.GaussianBlur(2))

# Ajouter du texte (watermark)
dessin = ImageDraw.Draw(image_floue)
dessin.text((10, 10), "© Mon Portfolio", fill="white")

image_floue.save("photo_traitee.jpg")
```

**Documentation :** https://pillow.readthedocs.io/

**5 projets pertinents :**
1. Un générateur automatique de miniatures pour un site portfolio
2. Un outil de watermarking en masse pour protéger des images
3. Un générateur de visuels de citations pour du contenu TikTok/Instagram
4. Un compresseur/optimiseur d'images pour accélérer un site web
5. Un générateur de CAPTCHA simple à but pédagogique

---

## 20. BeautifulSoup4

![beautifulsoup](https://img.shields.io/badge/-BeautifulSoup4-4B8BBE?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Parsing HTML / scraping
**Description :** Parseur HTML/XML tolérant aux erreurs, très utilisé pour extraire des données de pages web, souvent couplé avec `requests`.

**Installation :**
```bash
pip install beautifulsoup4
```

**Exemple de code :**
```python
import requests
from bs4 import BeautifulSoup

reponse = requests.get("https://quotes.toscrape.com/")
soup = BeautifulSoup(reponse.text, "html.parser")

for citation in soup.select("div.quote"):
    texte = citation.select_one("span.text").get_text()
    auteur = citation.select_one("small.author").get_text()
    print(f"{auteur}: {texte}")
```

**Documentation :** https://www.crummy.com/software/BeautifulSoup/bs4/doc/

**5 projets pertinents :**
1. Un agrégateur d'actualités tech pour alimenter du contenu automatiquement
2. Un extracteur de tableaux de données depuis des pages Wikipedia
3. Un vérificateur de mentions de marque sur le web (veille simple)
4. Un outil d'extraction de métadonnées SEO d'un site (titres, meta descriptions)
5. Un comparateur d'annonces immobilières scrapées depuis plusieurs sites

---

## 21. Pydantic

![pydantic](https://img.shields.io/badge/-Pydantic-E92063?style=for-the-badge&logo=pydantic&logoColor=white)

**Catégorie :** Validation de données
**Description :** Validation et sérialisation de données basées sur les type hints Python. Devenu un standard, notamment au cœur de FastAPI.

**Installation :**
```bash
pip install pydantic
```

**Exemple de code :**
```python
from pydantic import BaseModel, EmailStr, ValidationError

class Inscription(BaseModel):
    nom: str
    email: EmailStr
    age: int

try:
    user = Inscription(nom="Franck", email="franck@mail.com", age=22)
    print(user)
except ValidationError as e:
    print(e.json())
```

**Documentation :** https://docs.pydantic.dev/

**5 projets pertinents :**
1. Valider proprement les payloads JSON de ton API Flask (via pydantic en dehors de FastAPI)
2. Un système de configuration d'application validé au démarrage
3. Un valideur de formulaires d'inscription pour CVCraft
4. Un parseur de fichiers de config (YAML/JSON) avec schéma strict
5. Un outil de validation de données avant import en base

---

## 22. Loguru

![loguru](https://img.shields.io/badge/-Loguru-4B8BBE?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Logging
**Description :** Remplace le module `logging` natif, souvent lourd à configurer. Fonctionne out-of-the-box avec rotation de fichiers, couleurs, et traces d'erreurs claires.

**Installation :**
```bash
pip install loguru
```

**Exemple de code :**
```python
from loguru import logger

logger.add("app.log", rotation="10 MB", retention="7 days", level="INFO")

logger.info("Serveur démarré")
logger.warning("Cache presque plein")

try:
    1 / 0
except ZeroDivisionError:
    logger.exception("Erreur critique")
```

**Documentation :** https://loguru.readthedocs.io/

**5 projets pertinents :**
1. Ajouter un logging propre et centralisé à tous tes projets Flask/Django existants
2. Un système de suivi d'erreurs pour une API en production
3. Un outil d'analyse de logs (recherche, filtrage par niveau)
4. Un logger structuré pour un pipeline de scraping longue durée
5. Un outil d'audit qui log toutes les actions sensibles d'une application

---

## 23. Typer

![typer](https://img.shields.io/badge/-Typer-000000?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** CLI
**Description :** Construction de CLI basée sur les type hints, par le créateur de FastAPI. Génère automatiquement l'aide et la validation des arguments.

**Installation :**
```bash
pip install typer
```

**Exemple de code :**
```python
import typer

app = typer.Typer()

@app.command()
def scanner(cible: str, port_debut: int = 1, port_fin: int = 1024):
    typer.echo(f"Scan de {cible} entre les ports {port_debut} et {port_fin}")

@app.command()
def rapport(nom_fichier: str):
    typer.echo(f"Génération du rapport : {nom_fichier}")

if __name__ == "__main__":
    app()
```

**Documentation :** https://typer.tiangolo.com/

**5 projets pertinents :**
1. Une CLI unifiée regroupant tous tes petits scripts de sécu perso
2. Un outil CLI d'administration de ta base de données NEXUS ACADEMY
3. Un générateur de projets (scaffolding) façon `create-react-app` mais pour Flask
4. Un outil CLI de gestion de sauvegardes automatisées
5. Un client CLI pour ton API CVCraft (upload CV, génération PDF)

---

## 24. aiohttp

![aiohttp](https://img.shields.io/badge/-aiohttp-2C5BB4?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Réseau asynchrone
**Description :** Client et serveur HTTP asynchrone. Permet de faire des centaines de requêtes en parallèle sans bloquer, contrairement à `requests`.

**Installation :**
```bash
pip install aiohttp
```

**Exemple de code :**
```python
import aiohttp
import asyncio

async def recuperer(session, url):
    async with session.get(url) as reponse:
        return await reponse.json()

async def main():
    urls = ["https://api.github.com/users/octocat"] * 5
    async with aiohttp.ClientSession() as session:
        resultats = await asyncio.gather(*[recuperer(session, u) for u in urls])
        print(len(resultats), "réponses reçues")

asyncio.run(main())
```

**Documentation :** https://docs.aiohttp.org/

**5 projets pertinents :**
1. Un scanner de sites qui vérifie la disponibilité de centaines d'URLs en parallèle
2. Un agrégateur d'API multiples (météo + news + crypto) chargé en simultané
3. Un serveur de webhook léger et rapide
4. Un outil de vérification en masse d'emails/comptes via APIs externes
5. Un proxy asynchrone simple pour rediriger et logguer des requêtes

---

## 25. Uvicorn

![uvicorn](https://img.shields.io/badge/-Uvicorn-499848?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Serveur ASGI
**Description :** Serveur ASGI ultra performant, utilisé pour exécuter des applications FastAPI/Starlette en développement comme en production (avec Gunicorn en façade).

**Installation :**
```bash
pip install uvicorn
```

**Exemple de code :**
```python
# main.py
from fastapi import FastAPI
app = FastAPI()

@app.get("/")
def racine():
    return {"status": "ok"}
```

```bash
# Lancement simple
uvicorn main:app --reload

# Production avec plusieurs workers
uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4
```

**Documentation :** https://www.uvicorn.org/

**5 projets pertinents :**
1. Déployer ton API FastAPI de portfolio en production avec plusieurs workers
2. Un serveur de développement rapide pour prototyper des APIs
3. Un benchmark comparatif Flask/Gunicorn vs FastAPI/Uvicorn
4. Un serveur WebSocket temps réel (chat, notifications)
5. Une architecture microservices avec plusieurs apps Uvicorn derrière un reverse proxy

---

## 26. Jinja2

![jinja](https://cdn.simpleicons.org/jinja/B41717)

**Catégorie :** Moteur de templates
**Description :** Moteur de templates utilisé par Flask (et bien d'autres). Permet de générer du HTML dynamique, mais aussi des fichiers texte, emails, ou configs.

**Installation :**
```bash
pip install jinja2
```

**Exemple de code :**
```python
from jinja2 import Environment, FileSystemLoader

env = Environment(loader=FileSystemLoader("templates"))
template = env.get_template("email.html")

rendu = template.render(nom="Franck", lien_activation="https://cvcraft.io/activer/xyz")
print(rendu)
```

```html
<!-- templates/email.html -->
<p>Bonjour {{ nom }},</p>
<p>Clique <a href="{{ lien_activation }}">ici</a> pour activer ton compte.</p>
```

**Documentation :** https://jinja.palletsprojects.com/

**5 projets pertinents :**
1. Un générateur d'emails HTML personnalisés pour les utilisateurs de NEXUS ACADEMY
2. Un générateur de fichiers de configuration (nginx, docker-compose) à partir de variables
3. Un générateur de rapports HTML/PDF stylés pour CVCraft
4. Un générateur de code source (scaffolding de projets) à partir de templates
5. Un système de newsletters personnalisées envoyées en masse

---

## 27. Marshmallow

![marshmallow](https://img.shields.io/badge/-Marshmallow-3776AB?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Sérialisation / validation
**Description :** Sérialise des objets complexes en types Python natifs (et vice versa) tout en validant les données — très utilisé avec Flask.

**Installation :**
```bash
pip install marshmallow
```

**Exemple de code :**
```python
from marshmallow import Schema, fields, ValidationError

class UtilisateurSchema(Schema):
    nom = fields.Str(required=True)
    email = fields.Email(required=True)
    age = fields.Int(validate=lambda n: n >= 18)

schema = UtilisateurSchema()
try:
    donnees = schema.load({"nom": "Franck", "email": "franck@mail.com", "age": 22})
    print(donnees)
except ValidationError as err:
    print(err.messages)
```

**Documentation :** https://marshmallow.readthedocs.io/

**5 projets pertinents :**
1. Valider et sérialiser les réponses d'API de ton projet Flask NEXUS ACADEMY
2. Un système de transformation de données entre un modèle DB et un format API public
3. Un valideur de fichiers d'import en masse (CSV d'étudiants, de produits...)
4. Un normalisateur de données provenant de plusieurs sources externes
5. Une couche de validation pour un système de paiement (montants, devises)

---

## 28. Faker

![faker](https://img.shields.io/badge/-Faker-4B8BBE?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Génération de données de test
**Description :** Génère de fausses données réalistes (noms, adresses, emails, textes) — indispensable pour peupler une base de test ou une démo.

**Installation :**
```bash
pip install faker
```

**Exemple de code :**
```python
from faker import Faker

fake = Faker("fr_FR")

for _ in range(5):
    print(fake.name(), "-", fake.email(), "-", fake.city())

print(fake.text(max_nb_chars=100))
```

**Documentation :** https://faker.readthedocs.io/

**5 projets pertinents :**
1. Peupler ta base NEXUS ACADEMY avec des centaines de faux étudiants/cours pour tester
2. Un générateur de jeux de données factices pour des démos client
3. Un outil de génération de faux profils pour tester un formulaire d'inscription
4. Un générateur de faux CV pour tester CVCraft à grande échelle
5. Un script de stress-test qui simule des centaines d'utilisateurs différents

---

## 29. Hypothesis

![hypothesis](https://img.shields.io/badge/-Hypothesis-773B7A?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Tests (property-based testing)
**Description :** Au lieu d'écrire des cas de test un par un, tu décris des propriétés que ton code doit toujours respecter — Hypothesis génère automatiquement des centaines de cas, y compris les cas limites.

**Installation :**
```bash
pip install hypothesis
```

**Exemple de code :**
```python
from hypothesis import given, strategies as st

def additionner(a, b):
    return a + b

@given(st.integers(), st.integers())
def test_addition_est_commutative(a, b):
    assert additionner(a, b) == additionner(b, a)

test_addition_est_commutative()
```

**Documentation :** https://hypothesis.readthedocs.io/

**5 projets pertinents :**
1. Renforcer les tests de ton moteur de validation de formulaire CVCraft
2. Tester une fonction de parsing pour détecter des cas limites imprévus
3. Un audit de robustesse d'une fonction de calcul de prix/remises
4. Des tests exhaustifs pour un algorithme de tri ou de recherche personnalisé
5. Un fuzzing léger d'une API pour détecter des crashs sur des entrées inattendues

---

## 30. PyJWT

![pyjwt](https://img.shields.io/badge/-PyJWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)

**Catégorie :** Sécurité / authentification
**Description :** Création et validation de JSON Web Tokens (JWT), le standard pour l'authentification stateless dans les APIs modernes.

**Installation :**
```bash
pip install pyjwt
```

**Exemple de code :**
```python
import jwt
import datetime

CLE_SECRETE = "change-moi-en-production"

payload = {
    "user_id": 42,
    "exp": datetime.datetime.utcnow() + datetime.timedelta(hours=1)
}
token = jwt.encode(payload, CLE_SECRETE, algorithm="HS256")
print(token)

try:
    donnees = jwt.decode(token, CLE_SECRETE, algorithms=["HS256"])
    print(donnees)
except jwt.ExpiredSignatureError:
    print("Token expiré")
```

**Documentation :** https://pyjwt.readthedocs.io/

**5 projets pertinents :**
1. Un système d'authentification JWT complet pour l'API de NEXUS ACADEMY
2. Un système de liens d'invitation à durée limitée signés en JWT
3. Un audit d'API : détecter des JWT mal configurés (alg=none, secrets faibles)
4. Un système de single sign-on (SSO) simplifié entre deux de tes projets
5. Un middleware de vérification de token réutilisable pour plusieurs APIs

---

## 31. Passlib

![passlib](https://img.shields.io/badge/-Passlib-4B8BBE?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Sécurité / mots de passe
**Description :** Bibliothèque de hachage de mots de passe supportant bcrypt, argon2, et bien d'autres, avec gestion automatique de la migration d'algorithmes.

**Installation :**
```bash
pip install passlib argon2-cffi
```

**Exemple de code :**
```python
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["argon2"], deprecated="auto")

hash_mdp = pwd_context.hash("MonSuperMotDePasse123!")
print(hash_mdp)

est_valide = pwd_context.verify("MonSuperMotDePasse123!", hash_mdp)
print("Mot de passe correct :", est_valide)
```

**Documentation :** https://passlib.readthedocs.io/

**5 projets pertinents :**
1. Sécuriser le stockage des mots de passe de NEXUS ACADEMY (migration vers argon2)
2. Un outil d'audit qui détecte des hashs de mots de passe faibles (MD5, SHA1) dans une base
3. Un système de connexion sécurisé complet (inscription, login, reset) pour CVCraft
4. Un vérificateur de politique de mot de passe (complexité, historique)
5. Un outil de migration de hash lors d'un changement d'algorithme de sécurité

---

## 32. python-nmap

![nmap](https://cdn.simpleicons.org/nmap/6E7C7C)

**Catégorie :** Réseau / Pentest
**Description :** Wrapper Python autour de l'outil Nmap, permettant de scripter des scans réseau et de traiter les résultats directement en Python.

**Installation :**
```bash
pip install python-nmap
# nécessite nmap installé sur le système
```

**Exemple de code :**
```python
import nmap

scanner = nmap.PortScanner()
scanner.scan("scanme.nmap.org", "22-443")

for host in scanner.all_hosts():
    print(f"Hôte : {host} ({scanner[host].hostname()})")
    for proto in scanner[host].all_protocols():
        ports = scanner[host][proto].keys()
        for port in ports:
            etat = scanner[host][proto][port]["state"]
            print(f"  Port {port}/{proto} : {etat}")
```

> ⚠️ À utiliser uniquement sur des cibles que tu es autorisé à scanner.

**Documentation :** https://xael.org/pages/python-nmap-en.html

**5 projets pertinents :**
1. Un scanner de réseau local qui liste tous les appareils connectés (dans un cadre de lab)
2. Un outil d'audit automatique de ports ouverts sur tes propres serveurs
3. Un tableau de bord de monitoring de la surface d'attaque de ton infra perso
4. Un générateur de rapport de scan formaté en PDF/HTML
5. Un outil pédagogique de démonstration des types de scans Nmap pour ta chaîne TikTok

---

## 33. Matplotlib

![matplotlib](https://img.shields.io/badge/-Matplotlib-11557C?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Visualisation de données
**Description :** La bibliothèque de visualisation la plus ancienne et la plus flexible de l'écosystème Python — base de nombreuses autres bibliothèques (Seaborn, Pandas.plot).

**Installation :**
```bash
pip install matplotlib
```

**Exemple de code :**
```python
import matplotlib.pyplot as plt

mois = ["Jan", "Fév", "Mar", "Avr", "Mai"]
visiteurs = [120, 340, 290, 500, 610]

plt.figure(figsize=(8, 4))
plt.plot(mois, visiteurs, marker="o", color="steelblue")
plt.title("Visiteurs du site par mois")
plt.xlabel("Mois")
plt.ylabel("Nombre de visiteurs")
plt.grid(True)
plt.savefig("visiteurs.png")
plt.show()
```

**Documentation :** https://matplotlib.org/stable/

**5 projets pertinents :**
1. Un tableau de bord de statistiques de visites pour NEXUS ACADEMY
2. Un générateur automatique de graphiques pour tes vidéos TikTok tech
3. Un outil de visualisation de résultats de scans réseau (répartition des ports)
4. Un rapport visuel mensuel de progression d'apprentissage (quiz réussis, temps passé)
5. Un visualiseur de tendances de prix scrapées (produits e-commerce)

---

## 34. Seaborn

![seaborn](https://img.shields.io/badge/-Seaborn-3776AB?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Visualisation de données statistique
**Description :** Construit par-dessus Matplotlib, avec des graphiques statistiques élégants par défaut (distributions, corrélations, heatmaps).

**Installation :**
```bash
pip install seaborn
```

**Exemple de code :**
```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd

df = pd.DataFrame({
    "note_quiz": [12, 15, 9, 18, 14, 11, 16],
    "temps_etude_h": [2, 4, 1, 6, 3, 2, 5]
})

sns.regplot(data=df, x="temps_etude_h", y="note_quiz")
plt.title("Relation temps d'étude / note")
plt.show()
```

**Documentation :** https://seaborn.pydata.org/

**5 projets pertinents :**
1. Analyser la corrélation entre temps d'étude et résultats sur NEXUS ACADEMY
2. Un rapport visuel de distribution des notes d'un cours
3. Une heatmap de trafic web par heure/jour de la semaine
4. Un comparateur visuel de performances entre plusieurs modèles ML
5. Un rapport de qualité de données (distributions, valeurs aberrantes)

---

## 35. OpenCV-Python

![opencv](https://cdn.simpleicons.org/opencv/5C3EE8)

**Catégorie :** Vision par ordinateur
**Description :** Bibliothèque de référence pour le traitement d'image et la vision par ordinateur en temps réel (détection de visages, contours, filtres, vidéo).

**Installation :**
```bash
pip install opencv-python
```

**Exemple de code :**
```python
import cv2

image = cv2.imread("photo.jpg")
gris = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

detecteur_visages = cv2.CascadeClassifier(
    cv2.data.haarcascades + "haarcascade_frontalface_default.xml"
)
visages = detecteur_visages.detectMultiScale(gris, 1.1, 4)

for (x, y, w, h) in visages:
    cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)

cv2.imwrite("photo_annotee.jpg", image)
```

**Documentation :** https://docs.opencv.org/

**5 projets pertinents :**
1. Un système de présence par reconnaissance faciale pour NEXUS ACADEMY
2. Un détecteur de triche pendant un quiz en ligne (suivi du regard/visage)
3. Un outil de floutage automatique de visages/plaques dans des photos
4. Un compteur d'objets/personnes à partir d'un flux vidéo
5. Un scanner de documents (redressement, amélioration de contraste avant OCR)

---

## 36. scikit-learn

![scikit-learn](https://cdn.simpleicons.org/scikitlearn/F7931E)

**Catégorie :** Machine Learning
**Description :** La bibliothèque ML généraliste la plus utilisée pour la classification, régression, clustering, et prétraitement de données.

**Installation :**
```bash
pip install scikit-learn
```

**Exemple de code :**
```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score
from sklearn.datasets import load_iris

X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

modele = RandomForestClassifier(n_estimators=100)
modele.fit(X_train, y_train)

predictions = modele.predict(X_test)
print("Précision :", accuracy_score(y_test, predictions))
```

**Documentation :** https://scikit-learn.org/stable/

**5 projets pertinents :**
1. Un système de recommandation de cours pour les étudiants de NEXUS ACADEMY
2. Un détecteur de spam pour les messages/commentaires de ta plateforme
3. Un modèle de prédiction de réussite d'étudiant basé sur ses habitudes d'étude
4. Un classifieur d'emails de phishing (projet sécurité + ML)
5. Un système de scoring de CV automatique pour CVCraft

---

## 37. PyTorch

![pytorch](https://cdn.simpleicons.org/pytorch/EE4C2C)

**Catégorie :** Deep Learning
**Description :** Framework de deep learning le plus utilisé en recherche, avec un graphe de calcul dynamique et un excellent support GPU.

**Installation :**
```bash
pip install torch
```

**Exemple de code :**
```python
import torch
import torch.nn as nn

class ReseauSimple(nn.Module):
    def __init__(self):
        super().__init__()
        self.couche1 = nn.Linear(4, 8)
        self.couche2 = nn.Linear(8, 3)

    def forward(self, x):
        x = torch.relu(self.couche1(x))
        return self.couche2(x)

modele = ReseauSimple()
entree = torch.randn(1, 4)
sortie = modele(entree)
print(sortie)
```

**Documentation :** https://pytorch.org/docs/

**5 projets pertinents :**
1. Un classifieur d'images simple (reconnaissance de type de document pour CVCraft)
2. Un modèle de détection de plagiat entre soumissions d'étudiants
3. Un générateur de résumés automatiques de cours (NLP)
4. Un détecteur d'anomalies réseau basé sur un autoencodeur (sécurité)
5. Un chatbot pédagogique simple entraîné sur le contenu de tes cours

---

## 38. python-dotenv

![dotenv](https://img.shields.io/badge/-python--dotenv-ECD53F?style=for-the-badge&logo=python&logoColor=black)

**Catégorie :** Configuration
**Description :** Charge des variables d'environnement depuis un fichier `.env`, pour garder les secrets (clés API, mots de passe DB) hors du code source.

**Installation :**
```bash
pip install python-dotenv
```

**Exemple de code :**
```python
# .env
DATABASE_URL=postgresql://user:pass@localhost/db
SECRET_KEY=une-cle-tres-secrete

# app.py
import os
from dotenv import load_dotenv

load_dotenv()

database_url = os.getenv("DATABASE_URL")
secret_key = os.getenv("SECRET_KEY")
print(database_url)
```

**Documentation :** https://saurabh-kumar.com/python-dotenv/

**5 projets pertinents :**
1. Sécuriser les clés API et secrets de tous tes projets avant de les pousser sur GitHub
2. Un système de configuration multi-environnements (dev/staging/prod)
3. Un template de projet Flask/Django prêt à l'emploi avec `.env` intégré
4. Un audit de repos GitHub à la recherche de secrets codés en dur (à but pédagogique)
5. Un gestionnaire de configuration centralisé pour plusieurs microservices

---

## 39. Watchdog

![watchdog](https://img.shields.io/badge/-Watchdog-4B8BBE?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Système de fichiers
**Description :** Surveille les changements sur le système de fichiers (création, modification, suppression) et déclenche des actions automatiquement.

**Installation :**
```bash
pip install watchdog
```

**Exemple de code :**
```python
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler
import time

class MonHandler(FileSystemEventHandler):
    def on_modified(self, event):
        print(f"Fichier modifié : {event.src_path}")

observer = Observer()
observer.schedule(MonHandler(), path=".", recursive=True)
observer.start()

try:
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    observer.stop()
observer.join()
```

**Documentation :** https://python-watchdog.readthedocs.io/

**5 projets pertinents :**
1. Un outil de rechargement automatique lors du développement d'un projet Flask
2. Un système d'ingestion automatique de fichiers déposés dans un dossier (upload CV)
3. Un antivirus basique qui alerte sur la création de fichiers suspects (projet sécu)
4. Un synchronisateur de dossiers local vers un stockage distant
5. Un outil de build automatique qui recompile des assets à chaque modification

---

## 40. PyInstaller

![pyinstaller](https://img.shields.io/badge/-PyInstaller-4B8BBE?style=for-the-badge&logo=python&logoColor=white)

**Catégorie :** Packaging
**Description :** Transforme un script Python en exécutable autonome (Windows/Mac/Linux), sans que l'utilisateur final ait besoin d'installer Python.

**Installation :**
```bash
pip install pyinstaller
```

**Exemple de code :**
```bash
# Génère un seul exécutable dans dist/
pyinstaller --onefile mon_script.py

# Avec icône personnalisée et sans console (app GUI)
pyinstaller --onefile --windowed --icon=logo.ico mon_app.py
```

**Documentation :** https://pyinstaller.org/

**5 projets pertinents :**
1. Distribuer un de tes outils CLI de pentest sous forme d'exécutable pour ton portfolio
2. Empaqueter un outil interne d'entreprise pour des collègues non-techniques
3. Créer un installeur simple pour une petite application desktop
4. Distribuer un outil de démonstration pour ta chaîne TikTok tech
5. Un générateur automatisé d'exécutables multi-plateformes via CI/CD

---

*Fin du lot 2 (40/400+). Dis "continue" pour la suite (prochain lot : Sécurité/Pentest avancé + DevOps + GUI + Audio/Vidéo).*
