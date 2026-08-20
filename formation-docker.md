# 🐳 Formation Complète Docker — Du Débutant à l'Expert

![Docker Logo](https://commons.wikimedia.org/wiki/Special:FilePath/Docker%20(container%20engine)%20logo.svg)

> **Objectif de cette formation :** à la fin de ce parcours, tu seras capable de comprendre, expliquer et maîtriser Docker de A à Z — containeriser n'importe quelle application, écrire des `Dockerfile` robustes, orchestrer des architectures multi-services avec Docker Compose, sécuriser tes conteneurs en production, et même **containeriser des agents IA** (API LLM, RAG, bases vectorielles). Tu ne seras pas juste capable d'exécuter des commandes : tu sauras **pourquoi** tu les exécutes.

---

## 📋 Comment utiliser cette formation

Cette formation suit une pédagogie en 4 temps pour chaque notion :

1. **Le problème** — pourquoi cette notion existe, quelle douleur elle résout
2. **Le concept** — l'explication claire, avec schéma si nécessaire
3. **La pratique** — des commandes et du code que tu peux exécuter toi-même
4. **L'exercice** — pour vérifier que tu as compris avant de passer à la suite

**Règle d'or de cette formation : ne saute jamais une partie pratique.** Lire sur Docker sans taper les commandes, c'est comme lire sur la natation sans entrer dans l'eau. Ouvre un terminal maintenant et garde-le ouvert jusqu'à la fin.

### Sommaire

- [Partie 0 — Prérequis](#partie-0--prérequis)
- [Partie 1 — Comprendre Docker en profondeur](#partie-1--comprendre-docker-en-profondeur)
- [Partie 2 — Installation](#partie-2--installation)
- [Partie 3 — Premiers pas avec les conteneurs](#partie-3--premiers-pas-avec-les-conteneurs)
- [Partie 4 — Images Docker en profondeur](#partie-4--images-docker-en-profondeur)
- [Partie 5 — Les volumes et la persistance des données](#partie-5--les-volumes-et-la-persistance-des-données)
- [Partie 6 — Les réseaux Docker](#partie-6--les-réseaux-docker)
- [Partie 7 — Le Dockerfile en profondeur](#partie-7--le-dockerfile-en-profondeur)
- [Partie 8 — Docker Compose](#partie-8--docker-compose)
- [Partie 9 — Bonnes pratiques et sécurité](#partie-9--bonnes-pratiques-et-sécurité)
- [Partie 10 — Débogage et diagnostic](#partie-10--débogage-et-diagnostic)
- [Partie 11 — CI/CD et Docker en production](#partie-11--cicd-et-docker-en-production)
- [Partie 12 — Orchestration : Swarm et Kubernetes](#partie-12--orchestration--swarm-et-kubernetes)
- [Partie 13 — Containeriser des agents IA](#partie-13--containeriser-des-agents-ia)
- [Partie 14 — Projets pratiques](#partie-14--projets-pratiques)
- [Partie 15 — Glossaire et ressources](#partie-15--glossaire-et-ressources)

---

## Partie 0 — Prérequis

Avant de commencer, tu dois être à l'aise avec :

- Les commandes de base du **terminal** (`cd`, `ls`, `mkdir`, `cat`)
- La notion de **variables d'environnement**
- Les bases d'un langage de programmation (les exemples utiliseront principalement **Python** et **Node.js**, mais les concepts s'appliquent à tout langage)

Tu n'as **pas besoin** de connaître le réseau, la virtualisation ou l'administration système : tout sera expliqué depuis zéro.

---

## Partie 1 — Comprendre Docker en profondeur

### 1.1 Le problème que Docker résout

Imagine ce scénario, extrêmement courant avant Docker (et encore aujourd'hui chez ceux qui ne l'utilisent pas) :

> Un développeur écrit une application sur son ordinateur. Elle fonctionne parfaitement. Il l'envoie à un collègue, ou la déploie sur un serveur — et elle plante. Pourquoi ? Parce que son collègue a une version différente de Python, une bibliothèque système manquante, ou une variable de configuration différente.

C'est le fameux syndrome **"ça marche chez moi"** (*"works on my machine"*). Le code est identique, mais **l'environnement d'exécution** (version du langage, dépendances système, configuration, système de fichiers) diffère.

Docker résout ce problème avec une idée simple mais puissante : **au lieu de livrer juste le code, on livre le code ET tout son environnement d'exécution, ensemble, dans une unité portable et reproductible.**

### 1.2 Conteneur vs Machine Virtuelle : la confusion à éliminer

C'est LA confusion numéro 1 des débutants. Éclaircissons-la définitivement.

**Une machine virtuelle (VM)** virtualise du matériel. Un logiciel appelé *hyperviseur* (VMware, VirtualBox, Hyper-V) simule un ordinateur complet, sur lequel on installe un système d'exploitation complet (avec son propre noyau), puis l'application. Résultat : c'est lourd (plusieurs Go), lent à démarrer (minutes), mais totalement isolé.

**Un conteneur** ne virtualise pas le matériel. Il **partage le noyau du système d'exploitation hôte**, mais isole l'espace utilisateur (fichiers, processus, réseau) grâce à des fonctionnalités du noyau Linux (`namespaces` et `cgroups`). Résultat : c'est léger (quelques Mo à centaines de Mo), rapide à démarrer (millisecondes à secondes), et presque aussi isolé qu'une VM pour la plupart des usages.

![Comparaison conteneurs](https://commons.wikimedia.org/wiki/Special:FilePath/Containers.svg)

| Critère | Machine Virtuelle | Conteneur Docker |
|---|---|---|
| Ce qui est virtualisé | Le matériel | Le système d'exploitation (espace utilisateur) |
| Poids | Plusieurs Go | Quelques Mo à centaines de Mo |
| Démarrage | Minutes | Millisecondes à secondes |
| Isolation | Totale (noyau séparé) | Partagée (même noyau hôte) |
| Densité (combien sur une machine) | Faible (dizaines) | Élevée (centaines) |
| Cas d'usage typique | Isoler des OS différents, sécurité maximale | Déployer des applications, microservices |

> 💡 **À retenir :** un conteneur n'est PAS une mini-VM. C'est un **processus isolé** qui tourne directement sur le noyau de ta machine, mais qui "croit" être seul sur le système.

### 1.3 L'architecture de Docker

Docker fonctionne selon une architecture **client-serveur** :

- **Docker Client (`docker`)** : la commande en ligne que tu tapes. Elle envoie des ordres.
- **Docker Daemon (`dockerd`)** : le processus qui tourne en arrière-plan et qui exécute réellement les actions (créer un conteneur, télécharger une image, etc.). C'est le "moteur".
- **Docker Registry** : un serveur qui stocke des images Docker. Le plus connu est **Docker Hub** (public, gratuit pour les images publiques), mais on peut aussi avoir des registres privés (GitHub Container Registry, AWS ECR, GitLab Registry, etc.)

Voici le flux quand tu tapes `docker run nginx` :

```
┌─────────────┐        ┌──────────────┐        ┌──────────────────┐
│ Docker CLI  │──1───▶│ Docker Daemon │──2───▶│  Docker Registry  │
│ (client)    │        │  (dockerd)    │        │  (Docker Hub)     │
└─────────────┘        └──────┬───────┘        └──────────────────┘
                               │ 3. Télécharge l'image "nginx"
                               ▼
                        ┌──────────────┐
                        │  Conteneur    │
                        │  en cours     │
                        │  d'exécution  │
                        └──────────────┘
```

1. Le client envoie l'ordre "lance un conteneur à partir de l'image nginx"
2. Le daemon vérifie s'il a déjà l'image en local. Si non...
3. ...il la télécharge depuis le registre (Docker Hub par défaut)
4. Le daemon crée et démarre le conteneur à partir de cette image

### 1.4 Les concepts fondamentaux : vocabulaire indispensable

Avant d'aller plus loin, voici les 5 mots que tu dois connaître par cœur, car toute la formation repose dessus :

| Terme | Définition simple |
|---|---|
| **Image** | Un modèle en lecture seule contenant le code, les dépendances et la configuration d'une application. C'est une "photo" figée. |
| **Conteneur** | Une **instance en cours d'exécution** d'une image. Une image est comme une classe en programmation orientée objet ; un conteneur est comme un objet instancié à partir de cette classe. |
| **Dockerfile** | Un fichier texte qui décrit, étape par étape, **comment construire** une image. |
| **Registry** | Un serveur qui stocke et distribue des images (Docker Hub, GitHub Container Registry, etc.) |
| **Volume** | Un mécanisme pour **persister des données** au-delà de la durée de vie d'un conteneur. |

> 🎯 **Exercice de compréhension (à faire mentalement) :** Si `image nginx` = le plan d'une maison, `conteneur` = une maison réellement construite à partir de ce plan. Tu peux construire 10 maisons identiques (10 conteneurs) à partir du même plan (la même image). Vrai ou faux ?
>
> ✅ **Vrai.** C'est exactement le principe : une image peut donner naissance à autant de conteneurs que nécessaire, tous identiques au départ, mais qui évoluent ensuite indépendamment.

---

## Partie 2 — Installation

### 2.1 Choisir sa méthode d'installation

| Système | Méthode recommandée |
|---|---|
| Windows / macOS | **Docker Desktop** (interface graphique + CLI) |
| Linux (Ubuntu/Debian) | **Docker Engine** en ligne de commande (plus léger, pas besoin d'interface) |
| Serveur distant | **Docker Engine** via script officiel |

### 2.2 Installation sur Windows / macOS

1. Télécharge **Docker Desktop** depuis le site officiel : [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
2. Installe-le comme n'importe quelle application
3. Sur Windows, assure-toi que **WSL2** (Windows Subsystem for Linux) est activé — Docker Desktop te le proposera automatiquement
4. Lance Docker Desktop, attends que l'icône de la baleine 🐳 dans la barre des tâches indique "Running"

### 2.3 Installation sur Linux (Ubuntu/Debian)

```bash
# Mise à jour du système
sudo apt-get update

# Installation des dépendances nécessaires
sudo apt-get install -y ca-certificates curl gnupg

# Ajout de la clé GPG officielle de Docker
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Ajout du dépôt Docker
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Installation de Docker Engine
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# (Optionnel mais recommandé) Utiliser Docker sans "sudo"
sudo usermod -aG docker $USER
# Puis déconnecte-toi et reconnecte-toi (ou redémarre) pour que ça prenne effet
```

### 2.4 Vérifier l'installation

Peu importe ton système, vérifie que tout fonctionne avec ces deux commandes :

```bash
docker --version
docker run hello-world
```

Si tu vois un message de bienvenue expliquant que Docker fonctionne correctement, **félicitations, tu viens de faire tourner ton premier conteneur.**

> 🔍 **Ce qui vient de se passer en coulisses :** ta commande `docker run hello-world` a demandé au daemon de lancer un conteneur depuis l'image `hello-world`. Le daemon ne l'avait pas en local, donc il l'a téléchargée depuis Docker Hub, puis a créé et démarré un conteneur qui a affiché un message et s'est arrêté immédiatement (car son travail était terminé).

---

## Partie 3 — Premiers pas avec les conteneurs

### 3.1 Lancer ton premier vrai conteneur

```bash
docker run -d -p 8080:80 --name mon-nginx nginx
```

Décortiquons **chaque partie** de cette commande, car c'est le squelette que tu réutiliseras constamment :

- `docker run` : crée et démarre un conteneur
- `-d` : *detached* — le conteneur tourne en arrière-plan (sinon il "occuperait" ton terminal)
- `-p 8080:80` : *port mapping* — redirige le port **8080 de ta machine** vers le port **80 du conteneur** (où nginx écoute par défaut). Format : `-p <port_hôte>:<port_conteneur>`
- `--name mon-nginx` : donne un nom lisible à ton conteneur (sinon Docker en génère un aléatoire, du type `happy_einstein`)
- `nginx` : le nom de l'image à utiliser

Ouvre ton navigateur sur `http://localhost:8080` : tu verras la page d'accueil de nginx, servie **depuis ton conteneur**.

### 3.2 Les commandes essentielles pour manipuler les conteneurs

```bash
# Lister les conteneurs EN COURS d'exécution
docker ps

# Lister TOUS les conteneurs (y compris arrêtés)
docker ps -a

# Voir les logs d'un conteneur
docker logs mon-nginx

# Suivre les logs en temps réel (comme tail -f)
docker logs -f mon-nginx

# Arrêter un conteneur (arrêt propre)
docker stop mon-nginx

# Démarrer un conteneur déjà créé mais arrêté
docker start mon-nginx

# Redémarrer un conteneur
docker restart mon-nginx

# Supprimer un conteneur (doit être arrêté, sauf avec -f)
docker rm mon-nginx

# Arrêter ET supprimer en une commande
docker rm -f mon-nginx
```

### 3.3 Entrer DANS un conteneur (indispensable pour déboguer)

Un conteneur est un mini-environnement isolé — mais tu peux "entrer dedans" comme si c'était une machine à part, pour explorer, déboguer ou exécuter des commandes ponctuelles.

```bash
# Ouvre un terminal interactif DANS le conteneur en cours d'exécution
docker exec -it mon-nginx bash

# Une fois dedans, tu es "dans" le conteneur : essaie
ls /
cat /etc/nginx/nginx.conf
exit   # pour sortir
```

- `exec` : exécute une commande dans un conteneur déjà démarré
- `-it` : combinaison de `-i` (interactif, garde l'entrée standard ouverte) et `-t` (alloue un pseudo-terminal). C'est ce duo qui te donne un vrai terminal utilisable.
- `bash` : la commande à exécuter — ici, on lance un shell. Si l'image ne contient pas `bash` (images très légères comme Alpine), utilise `sh` à la place.

> ⚠️ **Piège classique du débutant :** `docker exec` ne fonctionne que sur un conteneur **déjà démarré**. Si le conteneur est arrêté, cette commande échouera avec une erreur. Utilise `docker start` d'abord.

### 3.4 Exécuter une commande unique sans "entrer" dans le conteneur

```bash
docker exec mon-nginx cat /etc/os-release
```

Ici, pas besoin de `-it` car on ne veut pas un terminal interactif, juste le résultat d'une commande unique.

### 3.5 Comprendre le cycle de vie d'un conteneur

```
   docker create          docker start           docker stop
        │                      │                       │
        ▼                      ▼                       ▼
   ┌─────────┐   docker run  ┌──────────┐          ┌──────────┐
   │  Créé   │ ────────────▶ │ En cours │ ────────▶│  Arrêté  │
   └─────────┘               └──────────┘          └──────────┘
                                                          │
                                                    docker rm
                                                          │
                                                          ▼
                                                    ┌──────────┐
                                                    │ Supprimé │
                                                    └──────────┘
```

- `docker run` = `docker create` + `docker start` en une seule commande (le cas le plus courant)
- Un conteneur arrêté **conserve son système de fichiers** (ses données à l'intérieur restent, sauf s'il est supprimé)
- Un conteneur supprimé (`docker rm`) est **définitivement perdu**, sauf ce qui a été mis dans un volume (voir Partie 5)

### 🎯 Exercice pratique n°1

1. Lance un conteneur Ubuntu en mode interactif : `docker run -it ubuntu bash`
2. À l'intérieur, crée un fichier : `echo "bonjour" > /test.txt`
3. Sors du conteneur (`exit`)
4. Relance un **nouveau** conteneur Ubuntu et vérifie que `/test.txt` n'existe pas
5. Retrouve avec `docker ps -a` le nom de ton **premier** conteneur, redémarre-le avec `docker start -ai <nom>`, et vérifie que `/test.txt` est toujours là

**Ce que cet exercice t'apprend :** chaque conteneur a son propre système de fichiers indépendant. Les données ne disparaissent que si le *conteneur* (pas l'image) est supprimé.

---

## Partie 4 — Images Docker en profondeur

### 4.1 Chercher et télécharger des images

```bash
# Chercher une image sur Docker Hub (depuis le terminal)
docker search postgres

# Télécharger une image sans la lancer
docker pull postgres:16

# Lister les images présentes en local
docker images

# Supprimer une image
docker rmi postgres:16

# Supprimer toutes les images non utilisées par un conteneur
docker image prune -a
```

### 4.2 Comprendre les tags

Une image s'identifie par `nom:tag`. Le tag précise la **version**.

```bash
docker pull node:20        # Node.js version 20
docker pull node:20-alpine # Node.js 20, sur une base Alpine (très légère)
docker pull node:latest    # La dernière version (À ÉVITER en production !)
```

> ⚠️ **Bonne pratique critique :** n'utilise **jamais** `latest` en production. Ce tag change avec le temps — ton déploiement d'aujourd'hui pourrait utiliser une version différente de celui de demain, cassant la reproductibilité que Docker est censé garantir. **Fixe toujours une version précise** (`node:20.11.0`) ou au moins une version majeure (`node:20`).

### 4.3 Le système de couches (layers) — le concept le plus important pour optimiser

Une image Docker n'est pas un bloc monolithique : c'est un **empilement de couches** (layers) en lecture seule, chacune représentant une modification par rapport à la précédente.

```
┌─────────────────────────────────┐
│  Couche 4 : COPY app.py .       │  ← ta dernière modification
├─────────────────────────────────┤
│  Couche 3 : RUN pip install     │
├─────────────────────────────────┤
│  Couche 2 : COPY requirements   │
├─────────────────────────────────┤
│  Couche 1 : FROM python:3.12    │  ← l'image de base
└─────────────────────────────────┘
```

**Pourquoi c'est fondamental :**

1. **Le cache de build** : si tu ne modifies que la couche 4, Docker réutilise les couches 1, 2 et 3 déjà construites — le build est instantané. C'est pour ça que **l'ordre des instructions dans un Dockerfile compte énormément** (voir Partie 7).
2. **Le partage entre images** : si deux images utilisent `python:3.12` comme base, cette couche n'est stockée **qu'une seule fois** sur le disque. C'est ce qui rend Docker si économe en espace.
3. **La couche du conteneur** : quand tu lances un conteneur, Docker ajoute une couche **inscriptible** au-dessus des couches en lecture seule de l'image. C'est là que vont tes fichiers temporaires, logs, etc. — et c'est cette couche qui disparaît quand tu supprimes le conteneur.

```bash
# Visualiser les couches d'une image
docker history nginx
```

### 4.4 Créer sa première image : `docker commit` (à éviter en pratique, mais utile pour comprendre)

```bash
# Lance un conteneur, modifie-le
docker run -it --name mon-ubuntu ubuntu bash
apt-get update && apt-get install -y curl
exit

# Transforme ce conteneur modifié en nouvelle image
docker commit mon-ubuntu ubuntu-avec-curl

# Vérifie
docker images
```

> ⚠️ **Pourquoi c'est déconseillé en pratique :** `docker commit` crée une image "à la main", sans trace de comment elle a été construite — impossible à reproduire ou à auditer. La méthode professionnelle est le **Dockerfile** (Partie 7), qui documente chaque étape de construction dans un fichier versionnable avec Git.

### 🎯 Exercice pratique n°2

1. Télécharge trois images différentes : `alpine`, `ubuntu`, `python:3.12-slim`
2. Compare leurs tailles avec `docker images` — observe l'écart entre `alpine` (~5 Mo) et les autres
3. Utilise `docker history python:3.12-slim` pour observer ses couches

---

## Partie 5 — Les volumes et la persistance des données

### 5.1 Le problème

Comme vu dans l'exercice de la Partie 3 : **les données d'un conteneur disparaissent quand il est supprimé.** C'est voulu (les conteneurs doivent être "jetables"), mais problématique pour une base de données, par exemple — tu ne veux pas perdre toutes tes données à chaque mise à jour de ton conteneur !

### 5.2 Les trois façons de gérer des données dans Docker

```
┌──────────────────────────────────────────────────────────┐
│                     MACHINE HÔTE                          │
│                                                            │
│   ┌──────────────┐         ┌────────────────────────┐    │
│   │ Volume Docker │         │  Dossier de ton projet  │    │
│   │ (géré par     │         │  /home/user/mon-projet │    │
│   │  Docker)      │         └───────────┬────────────┘    │
│   └───────┬───────┘                     │                 │
│           │                             │                 │
└───────────┼─────────────────────────────┼─────────────────┘
            │  volume nommé               │  bind mount
            ▼                             ▼
     ┌──────────────────────────────────────────┐
     │              CONTENEUR                     │
     │         /var/lib/postgresql/data           │
     └──────────────────────────────────────────┘
```

| Type | Description | Cas d'usage |
|---|---|---|
| **Volume nommé** | Géré entièrement par Docker, stocké dans un emplacement dédié | **Recommandé** pour les données persistantes (bases de données) |
| **Bind mount** | Relie un dossier précis de ta machine à un dossier du conteneur | Développement (voir tes modifications de code en temps réel) |
| **tmpfs** | Stocké uniquement en RAM, jamais sur le disque | Données temporaires sensibles (mots de passe en mémoire) |

### 5.3 Les volumes nommés (usage recommandé pour les données)

```bash
# Créer un volume
docker volume create data-postgres

# Lister les volumes
docker volume ls

# Utiliser le volume dans un conteneur PostgreSQL
docker run -d \
  --name ma-base \
  -e POSTGRES_PASSWORD=motdepasse \
  -v data-postgres:/var/lib/postgresql/data \
  postgres:16

# Même si tu supprimes le conteneur, les données restent dans le volume
docker rm -f ma-base

# Relance un nouveau conteneur avec le MÊME volume : les données sont toujours là
docker run -d --name ma-nouvelle-base -e POSTGRES_PASSWORD=motdepasse -v data-postgres:/var/lib/postgresql/data postgres:16
```

- `-v data-postgres:/var/lib/postgresql/data` : relie le volume nommé `data-postgres` au dossier `/var/lib/postgresql/data` **à l'intérieur** du conteneur (l'endroit où PostgreSQL stocke ses données)

### 5.4 Les bind mounts (usage recommandé pour le développement)

```bash
docker run -d \
  --name mon-app-dev \
  -p 3000:3000 \
  -v $(pwd):/app \
  -w /app \
  node:20 \
  npm run dev
```

- `-v $(pwd):/app` : relie le dossier courant de ta machine (`$(pwd)`) au dossier `/app` du conteneur. **Toute modification de code sur ta machine est immédiatement visible dans le conteneur** — idéal pour le développement avec rechargement automatique (hot-reload).
- `-w /app` : définit `/app` comme dossier de travail (working directory) dans le conteneur

> 💡 **Différence clé à retenir :** un **volume nommé**, c'est Docker qui décide où stocker les données (tu ne t'en soucies pas). Un **bind mount**, c'est TOI qui choisis un dossier précis de ta machine.

### 5.5 Commandes de gestion des volumes

```bash
docker volume inspect data-postgres   # Voir les détails d'un volume
docker volume rm data-postgres         # Supprimer un volume (doit être inutilisé)
docker volume prune                    # Supprimer tous les volumes inutilisés
```

### 🎯 Exercice pratique n°3

1. Crée un conteneur PostgreSQL avec un volume nommé
2. Connecte-toi dedans (`docker exec -it <nom> psql -U postgres`) et crée une table avec quelques lignes
3. Supprime le conteneur (pas le volume !)
4. Relance un nouveau conteneur avec le même volume
5. Vérifie que ta table et tes données sont toujours présentes

---

## Partie 6 — Les réseaux Docker

### 6.1 Le problème

Comment deux conteneurs (par exemple, une application web et sa base de données) peuvent-ils communiquer entre eux, alors qu'ils sont isolés l'un de l'autre par design ?

### 6.2 Les types de réseaux Docker

```bash
docker network ls
```

Tu verras au minimum trois réseaux par défaut :

| Réseau | Description |
|---|---|
| `bridge` | Le réseau par défaut. Les conteneurs peuvent communiquer entre eux via leur IP interne. |
| `host` | Le conteneur partage directement le réseau de la machine hôte (pas d'isolation réseau). |
| `none` | Aucun accès réseau. |

### 6.3 Créer un réseau personnalisé (la vraie bonne pratique)

Le réseau `bridge` par défaut ne permet **pas** de se joindre par nom de conteneur (seulement par IP, qui change à chaque redémarrage). La solution : créer ton propre réseau.

```bash
# Créer un réseau personnalisé
docker network create mon-reseau

# Lancer une base de données sur ce réseau
docker run -d --name ma-base --network mon-reseau -e POSTGRES_PASSWORD=secret postgres:16

# Lancer une application sur le MÊME réseau
docker run -d --name mon-app --network mon-reseau -p 3000:3000 mon-image-app
```

**Le résultat magique :** depuis `mon-app`, tu peux te connecter à la base de données en utilisant simplement le nom `ma-base` comme adresse (par exemple `postgresql://ma-base:5432/mabdd`), **sans jamais connaître son adresse IP**. Docker fournit un DNS interne automatique qui résout les noms de conteneurs.

```
┌─────────────────────────────────────────────────┐
│              Réseau "mon-reseau"                  │
│                                                    │
│   ┌─────────────┐         ┌─────────────────┐    │
│   │   mon-app    │────────▶│    ma-base       │    │
│   │  (port 3000) │  DNS:   │  (port 5432)     │    │
│   │              │ "ma-base"│                 │    │
│   └──────┬───────┘         └─────────────────┘    │
└──────────┼─────────────────────────────────────────┘
           │
           ▼ port 3000 exposé
    ┌─────────────┐
    │ Ta machine   │
    │ (hôte)       │
    └─────────────┘
```

> 💡 **À retenir :** deux conteneurs sur des réseaux différents ne peuvent **pas** communiquer entre eux, même s'ils tournent sur la même machine. C'est un mécanisme d'isolation volontaire — utile pour séparer par exemple un environnement "production" d'un environnement "test".

### 🎯 Exercice pratique n°4

1. Crée un réseau `test-net`
2. Lance un conteneur `redis` dessus (`docker run -d --name mon-redis --network test-net redis`)
3. Lance un conteneur Alpine sur le même réseau et installe `redis-cli` pour te connecter à `mon-redis` en utilisant son nom
4. Recommence, mais sans préciser `--network` cette fois : observe que la connexion par nom échoue

---

## Partie 7 — Le Dockerfile en profondeur

C'est **le cœur du métier**. Un `Dockerfile` est la recette qui permet de construire une image reproductible, versionnable avec Git, et auditable.

### 7.1 Anatomie d'un Dockerfile simple

Créons une image pour une application Node.js.

```dockerfile
# 1. Image de base : on part d'une image officielle Node.js
FROM node:20-alpine

# 2. Définit le dossier de travail à l'intérieur du conteneur
WORKDIR /app

# 3. Copie uniquement les fichiers de dépendances D'ABORD (voir explication cache plus bas)
COPY package.json package-lock.json ./

# 4. Installe les dépendances
RUN npm ci --omit=dev

# 5. Copie le reste du code de l'application
COPY . .

# 6. Documente le port utilisé par l'application (informatif seulement)
EXPOSE 3000

# 7. Variable d'environnement
ENV NODE_ENV=production

# 8. Commande exécutée au démarrage du conteneur
CMD ["node", "server.js"]
```

Construis et lance-la :

```bash
docker build -t mon-app:1.0 .
docker run -d -p 3000:3000 --name mon-app-conteneur mon-app:1.0
```

- `docker build -t mon-app:1.0 .` : construit une image nommée `mon-app` avec le tag `1.0`, à partir du Dockerfile trouvé dans le dossier courant (`.`)

### 7.2 Toutes les instructions essentielles du Dockerfile

| Instruction | Rôle | Exemple |
|---|---|---|
| `FROM` | Définit l'image de base (**toujours la première instruction**) | `FROM python:3.12-slim` |
| `WORKDIR` | Définit le dossier de travail (crée le dossier s'il n'existe pas) | `WORKDIR /app` |
| `COPY` | Copie des fichiers de ta machine vers l'image | `COPY . .` |
| `ADD` | Comme `COPY`, mais peut aussi extraire des archives et télécharger des URL (à éviter sauf besoin précis) | `ADD archive.tar.gz /app` |
| `RUN` | Exécute une commande **pendant la construction** de l'image (crée une nouvelle couche) | `RUN apt-get update` |
| `CMD` | Commande exécutée **au démarrage du conteneur**. Une seule par Dockerfile (la dernière écrase les précédentes) | `CMD ["python", "app.py"]` |
| `ENTRYPOINT` | Similaire à `CMD`, mais moins facilement remplaçable — souvent combiné avec `CMD` pour des arguments par défaut | `ENTRYPOINT ["python"]` |
| `ENV` | Définit une variable d'environnement, disponible au build ET à l'exécution | `ENV NODE_ENV=production` |
| `ARG` | Définit une variable disponible **uniquement pendant le build** | `ARG VERSION=1.0` |
| `EXPOSE` | Documente le port utilisé (n'ouvre rien tout seul — c'est informatif) | `EXPOSE 8080` |
| `VOLUME` | Déclare un point de montage destiné à un volume | `VOLUME /data` |
| `USER` | Change l'utilisateur d'exécution (sécurité — voir Partie 9) | `USER node` |
| `.dockerignore` | (pas une instruction, mais un fichier) exclut des fichiers du contexte de build | voir ci-dessous |

### 7.3 CMD vs ENTRYPOINT : la confusion classique

```dockerfile
# Avec CMD seul : entièrement remplaçable
CMD ["python", "app.py"]
# docker run mon-image  → exécute "python app.py"
# docker run mon-image echo salut → exécute "echo salut" (CMD est totalement ignoré)

# Avec ENTRYPOINT + CMD : ENTRYPOINT fixe, CMD fournit des arguments par défaut modifiables
ENTRYPOINT ["python"]
CMD ["app.py"]
# docker run mon-image  → exécute "python app.py"
# docker run mon-image other.py → exécute "python other.py" (seul CMD est remplacé)
```

> 💡 **Règle pratique :** utilise `CMD` seul pour la majorité des cas simples. Utilise `ENTRYPOINT` + `CMD` quand tu veux qu'un conteneur se comporte comme un exécutable avec des arguments par défaut modifiables (utile pour les images d'outils en ligne de commande).

### 7.4 Le cache de build : optimiser l'ordre de tes instructions

**Règle d'or : place les instructions qui changent le moins souvent en HAUT du Dockerfile, celles qui changent le plus souvent en BAS.**

```dockerfile
# ❌ MAUVAIS ORDRE : le cache est cassé à chaque modification de code
FROM node:20-alpine
WORKDIR /app
COPY . .                      # ← si TU changes 1 ligne de code, tout ce qui suit est reconstruit
RUN npm install                # ← réinstalle TOUTES les dépendances à chaque modif de code !
CMD ["node", "server.js"]

# ✅ BON ORDRE : le cache des dépendances est préservé
FROM node:20-alpine
WORKDIR /app
COPY package.json package-lock.json ./   # ← ne change que si tu ajoutes une dépendance
RUN npm ci                                # ← ce cache est réutilisé tant que package.json ne change pas
COPY . .                                  # ← seule cette étape est refaite si tu changes le code
CMD ["node", "server.js"]
```

**Pourquoi ça marche :** Docker compare chaque instruction à la couche précédente en cache. Si l'instruction et son contexte (les fichiers copiés) n'ont pas changé, Docker **réutilise** la couche déjà construite au lieu de la refaire. En séparant `COPY package.json` de `COPY . .`, tu permets à Docker de garder `npm install` en cache tant que tes dépendances ne changent pas — même si tu modifies ton code source cent fois par jour.

### 7.5 Le fichier `.dockerignore`

Comme `.gitignore`, il exclut des fichiers du **contexte de build** envoyé au daemon Docker.

```
# .dockerignore
node_modules
.git
.env
*.log
Dockerfile
.dockerignore
__pycache__
*.pyc
.venv
```

> ⚠️ **Pourquoi c'est important :** sans `.dockerignore`, `COPY . .` copie **tout**, y compris `node_modules` (potentiellement des centaines de Mo inutiles, car réinstallés dans l'image de toute façon) ou pire, ton fichier `.env` contenant des secrets !

### 7.6 Le multi-stage build : réduire drastiquement la taille des images

**Le problème :** pour compiler une application, tu as souvent besoin d'outils (compilateurs, dépendances de build) qui sont **inutiles à l'exécution**, mais qui gonflent inutilement ta couche finale.

**La solution :** utiliser plusieurs étapes (`stages`) dans un seul Dockerfile, où seule la dernière étape constitue l'image finale.

```dockerfile
# ─── Étape 1 : construction ───
FROM node:20 AS build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build   # génère un dossier /app/dist

# ─── Étape 2 : image finale, minimale ───
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Résultat : l'image finale contient **uniquement** nginx et les fichiers statiques compilés — pas Node.js, pas `node_modules`, pas les outils de build. On peut passer d'une image de 1,2 Go à une image de 25 Mo.

### 7.7 Exemple complet et robuste : une API Python (FastAPI)

```dockerfile
# Image de base légère et officielle
FROM python:3.12-slim AS base

# Empêche Python de générer des fichiers .pyc et force l'affichage immédiat des logs
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

# Dépendances système minimales (exemple : pour compiler certains paquets Python)
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Installation des dépendances Python (profite du cache)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copie du code applicatif
COPY . .

# Sécurité : créer un utilisateur non-root et l'utiliser (voir Partie 9)
RUN adduser --disabled-password --gecos "" appuser
USER appuser

EXPOSE 8000

# Vérification automatique de la bonne santé du conteneur
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')" || exit 1

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### 🎯 Exercice pratique n°5

1. Choisis une petite application que tu as déjà codée (ou crée un simple `app.py` Flask/FastAPI avec une route `/`)
2. Écris un `Dockerfile` complet pour elle, en appliquant : ordre optimisé pour le cache, `.dockerignore`, utilisateur non-root
3. Construis l'image et vérifie sa taille avec `docker images`
4. Modifie uniquement une ligne de ton code, reconstruis, et observe dans les logs de build que les étapes d'installation des dépendances utilisent le cache (`CACHED`)

---

## Partie 8 — Docker Compose

### 8.1 Le problème

Une application réelle n'est presque jamais un seul conteneur. Une architecture typique comprend une API, une base de données, un cache Redis, peut-être un frontend... Lancer chaque conteneur à la main avec de longues commandes `docker run` devient vite ingérable.

**Docker Compose** permet de décrire une architecture multi-conteneurs entière dans **un seul fichier YAML**, et de la démarrer/arrêter en une commande.

### 8.2 Structure d'un fichier `docker-compose.yml`

```yaml
services:
  api:
    build: ./api                  # construit l'image depuis un Dockerfile local
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/mabdd
    depends_on:
      - db
    networks:
      - mon-reseau

  db:
    image: postgres:16            # utilise une image existante
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mabdd
    volumes:
      - data-postgres:/var/lib/postgresql/data
    networks:
      - mon-reseau

  redis:
    image: redis:7-alpine
    networks:
      - mon-reseau

volumes:
  data-postgres:

networks:
  mon-reseau:
```

**Ce que Compose fait automatiquement pour toi :**

- Crée un réseau dédié pour que tous les services communiquent par leur nom (`api` peut joindre `db` juste avec le nom `db`)
- Gère l'ordre de démarrage approximatif avec `depends_on`
- Crée les volumes déclarés
- Permet de tout démarrer/arrêter/reconstruire en une seule commande

### 8.3 Les commandes essentielles de Compose

```bash
# Démarrer tous les services (en arrière-plan)
docker compose up -d

# Voir les logs de tous les services
docker compose logs -f

# Voir les logs d'un seul service
docker compose logs -f api

# Reconstruire les images puis démarrer
docker compose up -d --build

# Arrêter tous les services
docker compose down

# Arrêter ET supprimer les volumes (⚠️ perte de données)
docker compose down -v

# Voir l'état des services
docker compose ps

# Exécuter une commande dans un service en cours d'exécution
docker compose exec api bash
```

### 8.4 Gérer les variables d'environnement proprement avec `.env`

Ne mets **jamais** de secrets directement dans `docker-compose.yml` versionné sur Git. Utilise un fichier `.env` (ajouté à `.gitignore`) :

```
# .env (NE PAS COMMIT SUR GIT)
POSTGRES_PASSWORD=un-mot-de-passe-solide
API_SECRET_KEY=une-cle-secrete-longue-et-aleatoire
```

```yaml
services:
  db:
    image: postgres:16
    environment:
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
```

Compose charge automatiquement le fichier `.env` situé dans le même dossier que `docker-compose.yml`. Fournis un fichier `.env.example` (sans les vraies valeurs) pour documenter les variables attendues, et versionne **celui-là**.

### 8.5 Exemple complet réaliste : app Node.js + PostgreSQL + Redis + Nginx

```yaml
services:
  frontend:
    build: ./frontend
    depends_on:
      - api

  api:
    build: ./api
    environment:
      - DATABASE_URL=postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}@db:5432/${POSTGRES_DB}
      - REDIS_URL=redis://cache:6379
    depends_on:
      db:
        condition: service_healthy
      cache:
        condition: service_started

  db:
    image: postgres:16-alpine
    environment:
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
    volumes:
      - pg-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER}"]
      interval: 5s
      timeout: 5s
      retries: 5

  cache:
    image: redis:7-alpine

  reverse-proxy:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - frontend
      - api

volumes:
  pg-data:
```

> 💡 **`condition: service_healthy`** : force Compose à attendre que le `HEALTHCHECK` de la base de données réussisse avant de démarrer l'API — pas seulement que le conteneur soit "lancé", mais qu'il soit réellement **prêt** à recevoir des connexions.

### 🎯 Exercice pratique n°6

1. Crée un projet avec deux dossiers : `api/` (une API simple qui lit dans une base de données) et un `docker-compose.yml`
2. Décris un service `api` et un service `db` (PostgreSQL) dans le compose
3. Utilise un fichier `.env` pour les identifiants de la base
4. Démarre le tout avec `docker compose up -d --build` et vérifie que l'API arrive à joindre la base par son nom de service

---

## Partie 9 — Bonnes pratiques et sécurité

### 9.1 Utiliser des images de base minimales

Plus une image est petite, moins elle a de surface d'attaque (moins de paquets = moins de vulnérabilités potentielles) et plus elle est rapide à télécharger/déployer.

```dockerfile
# ❌ Lourd (~1 Go) et plus de surface d'attaque
FROM python:3.12

# ✅ Beaucoup plus léger (~150 Mo)
FROM python:3.12-slim

# ✅✅ Extrêmement léger (~50 Mo), mais syntaxe différente (musl vs glibc, peut poser des soucis de compatibilité)
FROM python:3.12-alpine
```

### 9.2 Ne jamais exécuter un conteneur en tant que `root`

Par défaut, un conteneur s'exécute en tant que `root`. Si un attaquant compromet ton application, il obtient les droits root **à l'intérieur du conteneur** — et selon la configuration, cela peut représenter un risque pour l'hôte.

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY . .
RUN npm ci --omit=dev

# Crée un utilisateur dédié et bascule dessus
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

CMD ["node", "server.js"]
```

> 💡 Certaines images officielles fournissent déjà un utilisateur non-root prêt à l'emploi (ex. `node` dans l'image `node`). Vérifie la documentation de l'image que tu utilises.

### 9.3 Ne jamais mettre de secrets dans une image

```dockerfile
# ❌ CATASTROPHIQUE : le secret reste dans l'historique des couches, visible avec "docker history"
ENV API_KEY=sk-abc123secret

# ✅ Fournir les secrets À L'EXÉCUTION, jamais au build
# docker run -e API_KEY=$API_KEY mon-image
```

Pour des secrets encore plus sensibles en production, utilise des solutions dédiées : **Docker Secrets** (avec Swarm), **variables d'environnement injectées par la CI/CD**, ou des gestionnaires externes (Vault, AWS Secrets Manager).

### 9.4 Scanner tes images pour les vulnérabilités

```bash
# Docker Scout, intégré à Docker Desktop
docker scout cves mon-image:1.0

# Alternative open-source populaire : Trivy
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image mon-image:1.0
```

### 9.5 Limiter les ressources d'un conteneur

Un conteneur mal configuré peut consommer toute la RAM ou le CPU de l'hôte. Toujours définir des limites en production.

```bash
docker run -d --memory=512m --cpus=1.0 mon-image
```

Ou en Compose :

```yaml
services:
  api:
    image: mon-image
    deploy:
      resources:
        limits:
          cpus: "1.0"
          memory: 512M
```

### 9.6 Checklist de sécurité et de qualité à cocher avant chaque mise en production

- [ ] Version d'image de base **fixée précisément** (pas de `latest`)
- [ ] Utilisateur **non-root**
- [ ] `.dockerignore` en place (pas de secrets ni de `.git` copiés)
- [ ] Aucun secret en dur dans le Dockerfile
- [ ] `HEALTHCHECK` défini
- [ ] Limites de ressources (CPU / RAM) définies
- [ ] Image scannée pour les vulnérabilités connues
- [ ] Multi-stage build utilisé si compilation nécessaire
- [ ] Logs envoyés vers la sortie standard (`stdout`/`stderr`), pas dans des fichiers internes au conteneur

---

## Partie 10 — Débogage et diagnostic

Ce sont les réflexes que tout développeur Docker doit avoir quand "ça ne marche pas".

```bash
# Le conteneur démarre puis s'arrête immédiatement ? Regarde ses logs
docker logs <nom-conteneur>

# Le conteneur ne démarre même pas ? Regarde les détails de l'erreur
docker inspect <nom-conteneur>

# Statistiques d'utilisation en temps réel (CPU, RAM, réseau)
docker stats

# Voir les processus en cours dans un conteneur
docker top <nom-conteneur>

# Inspecter le réseau d'un conteneur (son IP, ses réseaux)
docker inspect -f '{{json .NetworkSettings.Networks}}' <nom-conteneur>

# Voir l'espace disque utilisé par Docker (images, conteneurs, volumes, cache)
docker system df

# Nettoyer TOUT ce qui n'est pas utilisé (images, conteneurs arrêtés, réseaux, cache de build)
docker system prune -a --volumes
```

### 10.1 Méthode systématique de débogage

Quand un conteneur ne fonctionne pas comme prévu, suis cet ordre :

1. **`docker ps -a`** — le conteneur existe-t-il ? Dans quel état est-il (`Exited`, `Restarting`...) ?
2. **`docker logs <nom>`** — que dit l'application elle-même ? C'est la source d'information n°1.
3. **`docker inspect <nom>`** — quel code de sortie (`ExitCode`) ? Quelle configuration réseau/volumes réellement appliquée ?
4. **`docker exec -it <nom> sh`** (si le conteneur tourne) — entre dedans et vérifie manuellement : les fichiers sont-ils là où tu penses ? Les variables d'environnement sont-elles correctement définies (`env`) ?
5. Si le conteneur s'arrête **avant** que tu puisses `exec` dedans, lance-le en remplaçant sa commande par un shell pour explorer : `docker run -it --entrypoint sh mon-image`

### 🎯 Exercice pratique n°7

1. Écris volontairement un Dockerfile avec une erreur (par exemple une commande `CMD` qui pointe vers un fichier inexistant)
2. Construis et lance-le, observe qu'il s'arrête immédiatement
3. Utilise la méthode systématique ci-dessus pour identifier et corriger l'erreur

---

## Partie 11 — CI/CD et Docker en production

### 11.1 Le principe : automatiser la construction et le déploiement

Une fois ton Dockerfile prêt, l'objectif en production est qu'à chaque `git push`, une pipeline automatique :

1. Construit l'image
2. La teste
3. La pousse vers un registre (Docker Hub, GitHub Container Registry, etc.)
4. La déploie sur le serveur

### 11.2 Exemple avec GitHub Actions

```yaml
# .github/workflows/docker-build.yml
name: Build and Push Docker Image

on:
  push:
    branches: [main]

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Récupération du code
        uses: actions/checkout@v4

      - name: Connexion à GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Construction et publication de l'image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:latest
            ghcr.io/${{ github.repository }}:${{ github.sha }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

> 💡 Note l'usage de **deux tags** : `latest` pour la dernière version, et `${{ github.sha }}` (le hash du commit) pour une traçabilité précise — tu sais exactement quel commit a produit quelle image, et tu peux revenir en arrière (`rollback`) facilement.

### 11.3 Pousser et tirer des images depuis un registre

```bash
# Se connecter à Docker Hub
docker login

# Tagger une image locale pour un registre
docker tag mon-app:1.0 mon-utilisateur/mon-app:1.0

# Pousser l'image
docker push mon-utilisateur/mon-app:1.0

# Sur le serveur de production, tirer l'image
docker pull mon-utilisateur/mon-app:1.0
docker run -d mon-utilisateur/mon-app:1.0
```

### 11.4 Stratégie de tagging sémantique recommandée

```bash
docker build -t mon-app:1.4.2 -t mon-app:1.4 -t mon-app:1 -t mon-app:latest .
```

Cela permet à un utilisateur de choisir sa précision : `mon-app:1` (toujours la dernière version majeure 1.x compatible), `mon-app:1.4.2` (version figée exacte pour la reproductibilité totale).

---

## Partie 12 — Orchestration : Swarm et Kubernetes

Une fois que tu gères plusieurs conteneurs sur **plusieurs machines**, avec des besoins de haute disponibilité, de mise à l'échelle automatique et d'auto-réparation, un simple `docker-compose.yml` ne suffit plus. C'est le rôle des **orchestrateurs**.

| Outil | Description | Quand l'utiliser |
|---|---|---|
| **Docker Compose** | Orchestration simple, une seule machine | Développement, petits projets, un seul serveur |
| **Docker Swarm** | Orchestrateur natif de Docker, simple à apprendre | Petites/moyennes équipes voulant rester dans l'écosystème Docker |
| **Kubernetes (K8s)** | Le standard de l'industrie, très puissant mais complexe | Grandes équipes, besoins avancés (auto-scaling, multi-cloud) |

### 12.1 Un aperçu de Docker Swarm (car directement intégré à Docker)

```bash
# Initialiser un cluster Swarm
docker swarm init

# Déployer une stack (réutilise la syntaxe de docker-compose.yml !)
docker stack deploy -c docker-compose.yml ma-stack

# Mettre à l'échelle un service (5 instances)
docker service scale ma-stack_api=5

# Voir l'état des services
docker service ls
```

### 12.2 Pourquoi Kubernetes n'est pas couvert en détail ici

Kubernetes est un sujet de formation à part entière (souvent plusieurs semaines à lui seul). Ce que tu dois retenir à ce stade : **tous les concepts que tu as appris dans cette formation (images, conteneurs, réseaux, volumes) sont directement réutilisables dans Kubernetes** — K8s ajoute une couche d'orchestration au-dessus, mais ne remplace pas ta compréhension de Docker. Une fois cette formation terminée, tu seras prêt à aborder Kubernetes avec les bonnes bases.

---

## Partie 13 — Containeriser des agents IA

C'est ici que tout ce que tu as appris converge vers un cas d'usage moderne et extrêmement demandé : **déployer des applications d'intelligence artificielle** (agents LLM, RAG, chatbots) de façon robuste et reproductible.

### 13.1 Spécificités des applications IA en conteneur

Une application IA a des besoins particuliers par rapport à une API classique :

- **Clés API sensibles** (OpenAI, Anthropic, etc.) — jamais en dur dans l'image
- **Dépendances parfois lourdes** (bibliothèques ML) — attention à la taille de l'image
- **Composants externes fréquents** : base de données vectorielle (pour le RAG), cache, file de tâches asynchrones pour les traitements longs
- **Appels réseau sortants** vers les API de fournisseurs LLM — assure-toi que ton conteneur a bien accès à Internet

### 13.2 Exemple concret : un agent IA avec FastAPI + API Claude (Anthropic)

Structure du projet :

```
mon-agent-ia/
├── Dockerfile
├── .dockerignore
├── requirements.txt
├── main.py
└── .env.example
```

**`requirements.txt`**

```
fastapi==0.115.0
uvicorn[standard]==0.30.0
anthropic==0.40.0
python-dotenv==1.0.1
```

**`main.py`** (agent minimal utilisant l'API Claude)

```python
import os
from fastapi import FastAPI
from pydantic import BaseModel
from anthropic import Anthropic

app = FastAPI()

# La clé API est lue depuis l'environnement — JAMAIS écrite en dur
client = Anthropic(api_key=os.environ["ANTHROPIC_API_KEY"])

class Question(BaseModel):
    message: str

@app.get("/health")
def health():
    return {"status": "ok"}

@app.post("/chat")
def chat(question: Question):
    response = client.messages.create(
        model="claude-sonnet-4-5",
        max_tokens=1024,
        messages=[{"role": "user", "content": question.message}]
    )
    return {"reponse": response.content[0].text}
```

**`Dockerfile`**

```dockerfile
FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY main.py .

RUN adduser --disabled-password --gecos "" appuser
USER appuser

EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')" || exit 1

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**Construction et lancement — la clé API est injectée à l'exécution, jamais stockée dans l'image :**

```bash
docker build -t mon-agent-ia:1.0 .

docker run -d \
  --name agent-ia \
  -p 8000:8000 \
  -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \
  mon-agent-ia:1.0
```

> 🔒 **Point de sécurité crucial :** dans cet exemple, `$ANTHROPIC_API_KEY` doit exister comme variable d'environnement sur ta machine (ou dans ta CI/CD), jamais écrite directement dans un fichier versionné sur Git. Vérifie systématiquement avec `docker history mon-agent-ia:1.0` qu'aucune clé n'apparaît dans les couches de l'image.

### 13.3 Architecture complète : agent IA + base vectorielle (RAG) avec Docker Compose

Un agent RAG (*Retrieval-Augmented Generation*) a besoin de rechercher dans une base de connaissances avant de répondre. Voici une architecture typique :

```yaml
services:
  agent:
    build: ./agent
    ports:
      - "8000:8000"
    environment:
      - ANTHROPIC_API_KEY=${ANTHROPIC_API_KEY}
      - VECTOR_DB_URL=http://vector-db:6333
    depends_on:
      - vector-db

  vector-db:
    image: qdrant/qdrant:latest
    volumes:
      - qdrant-data:/qdrant/storage
    ports:
      - "6333:6333"

  worker:
    build: ./worker            # traitements asynchrones longs (indexation de documents, etc.)
    environment:
      - VECTOR_DB_URL=http://vector-db:6333
      - REDIS_URL=redis://queue:6379
    depends_on:
      - queue
      - vector-db

  queue:
    image: redis:7-alpine

volumes:
  qdrant-data:
```

```
┌──────────────────────────────────────────────────────────────┐
│                     docker-compose (réseau interne)            │
│                                                                  │
│   Utilisateur ──▶ ┌────────┐   recherche   ┌──────────────┐   │
│    (port 8000)     │ agent  │──────────────▶│  vector-db    │   │
│                     │(FastAPI)│◀─────────────│  (Qdrant)     │   │
│                     └───┬────┘   résultats   └──────────────┘   │
│                         │ appel API                              │
│                         ▼                                        │
│                  ┌──────────────┐                                │
│                  │  API Claude   │ (externe, via Internet)        │
│                  │  (Anthropic)  │                                │
│                  └──────────────┘                                │
│                                                                    │
│   ┌────────┐  tâches   ┌────────┐                                │
│   │ worker  │◀─────────│ queue   │  (indexation de documents)     │
│   │         │          │ (Redis) │                                │
│   └────┬───┘           └────────┘                                │
│        │ écrit dans                                              │
│        ▼                                                          │
│   ┌──────────────┐                                                │
│   │  vector-db    │                                                │
│   └──────────────┘                                                │
└──────────────────────────────────────────────────────────────────┘
```

**Pourquoi cette architecture est robuste :**

- Chaque composant est **remplaçable indépendamment** (changer de base vectorielle sans toucher à l'agent)
- Le **worker séparé** évite de bloquer les réponses de l'agent pendant l'indexation de nouveaux documents (traitement asynchrone via la file Redis)
- Tout est **reproductible** : n'importe quel développeur clone le repo, lance `docker compose up`, et obtient l'architecture complète en quelques minutes

### 13.4 Bonnes pratiques spécifiques aux conteneurs IA

- **Limite bien les ressources** (`--memory`, `--cpus`) : certains traitements IA (embeddings locaux, modèles téléchargés) peuvent consommer énormément de RAM
- **Ne mets jamais de modèles lourds directement dans l'image** si possible — préfère les monter en volume ou les télécharger au démarrage, sinon tes images pèseront des Go et seront lentes à distribuer
- **Ajoute des timeouts** sur tes appels aux API externes (les LLM peuvent être lents), pour éviter que ton conteneur ne reste bloqué indéfiniment
- **Journalise (logue) les appels aux API IA** (sans logger les clés !) pour pouvoir déboguer et suivre les coûts

### 🎯 Exercice pratique n°8

1. Reprends l'exemple de l'agent FastAPI + Claude ci-dessus
2. Ajoute un service Redis en Compose pour mettre en cache les réponses aux questions déjà posées (évite de rappeler l'API pour une question identique)
3. Ajoute un `HEALTHCHECK` complet
4. Teste que si tu coupes ta connexion Internet, ton agent renvoie une erreur propre plutôt que de planter silencieusement

---

## Partie 14 — Projets pratiques

Voici une liste de projets progressifs, du débutant à l'expert, à réaliser **sans avoir besoin de chercher les consignes ailleurs**. Fais-les dans l'ordre : chacun réutilise et approfondit les compétences du précédent.

### 🟢 Niveau débutant

**Projet 1 — Site statique containerisé**
Crée un site HTML/CSS simple (une page "à propos de toi"). Écris un Dockerfile utilisant `nginx:alpine` comme base, qui copie tes fichiers HTML dans `/usr/share/nginx/html`. Construis, lance, vérifie dans le navigateur.

**Projet 2 — API minimale avec base de données**
Crée une API (Flask, FastAPI ou Express) avec une seule route qui retourne une liste de tâches (*todo list*) stockées en mémoire. Containerise-la avec un Dockerfile propre (cache optimisé, `.dockerignore`).

**Projet 3 — Ajout de la persistance**
Reprends le Projet 2, mais remplace le stockage en mémoire par une vraie base PostgreSQL, lancée dans un second conteneur avec un volume nommé. Connecte les deux via un réseau Docker personnalisé (sans Compose pour l'instant — fais-le à la main avec `docker run` pour bien comprendre).

### 🟡 Niveau intermédiaire

**Projet 4 — Migration vers Docker Compose**
Reprends le Projet 3 et transforme-le en `docker-compose.yml` avec au moins 2 services (API + base de données), un fichier `.env` pour les secrets, et un volume nommé.

**Projet 5 — Application 3-tiers complète**
Construis une application avec : un frontend (React, Vue, ou HTML/JS simple), une API backend, et une base de données. Chaque composant a son propre Dockerfile. Orchestre le tout avec Docker Compose et un reverse proxy Nginx qui route les requêtes.

**Projet 6 — Optimisation multi-stage**
Prends le frontend du Projet 5 (typiquement une application React/Vue nécessitant une étape de build) et réécris son Dockerfile en multi-stage build pour réduire drastiquement sa taille finale. Compare la taille avant/après avec `docker images`.

### 🔴 Niveau avancé

**Projet 7 — Pipeline CI/CD complet**
Héberge le Projet 5 sur GitHub. Écris un workflow GitHub Actions qui, à chaque push sur `main` : construit les images, les teste (ajoute au moins un test automatisé simple), les publie sur GitHub Container Registry avec un tag basé sur le hash du commit.

**Projet 8 — Sécurisation complète**
Reprends n'importe quel projet précédent et applique la checklist complète de la Partie 9 : utilisateur non-root, `HEALTHCHECK`, limites de ressources, scan de vulnérabilités avec Trivy ou Docker Scout, aucune image utilisant `latest`.

### 🟣 Niveau expert — Intégration IA

**Projet 9 — Agent IA containerisé simple**
Réalise l'exemple complet de la Partie 13.2 : une API FastAPI qui interroge un modèle Claude via l'API Anthropic, entièrement containerisée, avec la clé API injectée de façon sécurisée.

**Projet 10 — Stack RAG complète**
Construis l'architecture de la Partie 13.3 : un agent IA + une base de données vectorielle (Qdrant ou ChromaDB) + un worker d'indexation asynchrone connecté via Redis. Objectif final : pouvoir uploader un document texte, qui est indexé par le worker, puis interroger l'agent qui utilise ce document pour répondre (RAG fonctionnel de bout en bout).

**Projet 11 — Portfolio final**
Combine tout ce que tu as appris : déploie la stack RAG du Projet 10 sur un vrai serveur (VPS type DigitalOcean, Hetzner, OVH...), avec un pipeline CI/CD qui déploie automatiquement à chaque push, un reverse proxy avec certificat HTTPS (Let's Encrypt via Traefik ou Nginx + Certbot), et un monitoring basique (`docker stats` ou un outil comme cAdvisor).

> 🏆 **Si tu termines le Projet 11, tu n'es plus un débutant Docker — tu es capable de concevoir, sécuriser et déployer des architectures conteneurisées complexes, y compris des systèmes d'IA en production.** C'est exactement le niveau attendu d'un développeur DevOps/Backend confirmé sur ce sujet.

---

## Partie 15 — Glossaire et ressources

### Glossaire rapide

- **Image** : modèle en lecture seule pour créer des conteneurs
- **Conteneur** : instance en cours d'exécution d'une image
- **Dockerfile** : recette de construction d'une image
- **Registry** : serveur stockant des images (ex. Docker Hub)
- **Volume** : mécanisme de persistance des données
- **Layer (couche)** : chaque instruction du Dockerfile crée une couche empilée
- **Bind mount** : lien direct entre un dossier de l'hôte et du conteneur
- **Compose** : outil pour orchestrer plusieurs conteneurs via un fichier YAML
- **Orchestrateur** : système gérant des conteneurs à grande échelle sur plusieurs machines (Swarm, Kubernetes)
- **Healthcheck** : commande vérifiant automatiquement que le conteneur fonctionne correctement
- **Multi-stage build** : Dockerfile utilisant plusieurs étapes pour réduire la taille finale de l'image

### Ressources officielles pour aller plus loin

- Documentation officielle Docker : [docs.docker.com](https://docs.docker.com)
- Référence complète du Dockerfile : [docs.docker.com/engine/reference/builder](https://docs.docker.com/engine/reference/builder/)
- Docker Hub (registre public d'images) : [hub.docker.com](https://hub.docker.com)
- Documentation Docker Compose : [docs.docker.com/compose](https://docs.docker.com/compose/)
- Documentation Kubernetes (pour la suite) : [kubernetes.io/docs](https://kubernetes.io/docs/home/)

---

## 🎓 Conclusion

Tu as maintenant parcouru l'intégralité du chemin : des concepts fondamentaux (conteneurs vs VM, images, couches) jusqu'à la containerisation d'agents IA en architecture RAG complète, en passant par la sécurité, le réseau, les volumes, Compose et la CI/CD.

**La clé pour transformer cette formation en vraie compétence : fais les 11 projets, dans l'ordre, sans copier-coller aveuglément.** Chaque erreur que tu rencontreras et résoudras toi-même vaudra plus que dix pages de cours lues passivement.

Bon courage, et bienvenue dans le monde des conteneurs 🐳
