# ☸️ Formation Complète Kubernetes & Déploiement d'Applications — Du Débutant à l'Expert

![Kubernetes Logo](https://commons.wikimedia.org/wiki/Special:FilePath/Kubernetes%20logo%20without%20workmark.svg)

> **Objectif de cette formation :** à la fin de ce parcours, tu seras capable de comprendre, expliquer et maîtriser Kubernetes de A à Z — déployer des applications robustes et résilientes, gérer la mise à l'échelle automatique, sécuriser un cluster, mettre en place des stratégies de déploiement sans interruption de service (zero-downtime), et déployer de vraies applications (classiques et applications IA) en production. Cette formation part du principe que tu maîtrises déjà Docker (conteneurs, images, Dockerfile) — si ce n'est pas le cas, complète d'abord la formation Docker avant celle-ci.

---

## 📋 Comment utiliser cette formation

Même pédagogie que pour Docker : **problème → concept → pratique → exercice**, à chaque étape. Kubernetes est plus abstrait que Docker : il introduit énormément de vocabulaire nouveau. Ne te décourage pas si les 3 premières parties demandent plusieurs lectures — c'est normal, c'est la partie la plus dense de toute la formation. Une fois le vocabulaire acquis, tout devient plus fluide.

**Prérequis indispensable :** avoir un cluster Kubernetes local pour pratiquer. On installera **Minikube** ou **Kind** dès la Partie 2 — ne saute pas cette étape, tout le reste de la formation part du principe que tu as un cluster qui tourne devant toi.

### Sommaire

- [Partie 0 — Prérequis](#partie-0--prérequis)
- [Partie 1 — Pourquoi Kubernetes ? Comprendre le problème](#partie-1--pourquoi-kubernetes--comprendre-le-problème)
- [Partie 2 — Installation et premier cluster](#partie-2--installation-et-premier-cluster)
- [Partie 3 — L'architecture de Kubernetes en profondeur](#partie-3--larchitecture-de-kubernetes-en-profondeur)
- [Partie 4 — Le Pod : l'unité de base](#partie-4--le-pod--lunité-de-base)
- [Partie 5 — Le Deployment : déployer une application robuste](#partie-5--le-deployment--déployer-une-application-robuste)
- [Partie 6 — Le Service : exposer et joindre tes applications](#partie-6--le-service--exposer-et-joindre-tes-applications)
- [Partie 7 — Ingress : exposer proprement sur Internet](#partie-7--ingress--exposer-proprement-sur-internet)
- [Partie 8 — Configuration : ConfigMap et Secret](#partie-8--configuration--configmap-et-secret)
- [Partie 9 — Stockage : Volumes, PV et PVC](#partie-9--stockage--volumes-pv-et-pvc)
- [Partie 10 — Stratégies de déploiement d'applications](#partie-10--stratégies-de-déploiement-dapplications)
- [Partie 11 — Mise à l'échelle (Scaling)](#partie-11--mise-à-léchelle-scaling)
- [Partie 12 — Helm : le gestionnaire de paquets Kubernetes](#partie-12--helm--le-gestionnaire-de-paquets-kubernetes)
- [Partie 13 — Observabilité : logs, métriques, debug](#partie-13--observabilité--logs-métriques-debug)
- [Partie 14 — Sécurité en profondeur](#partie-14--sécurité-en-profondeur)
- [Partie 15 — CI/CD et GitOps : déploiement automatisé en production](#partie-15--cicd-et-gitops--déploiement-automatisé-en-production)
- [Partie 16 — Déployer des applications IA sur Kubernetes](#partie-16--déployer-des-applications-ia-sur-kubernetes)
- [Partie 17 — Déployer sur un vrai cloud (production réelle)](#partie-17--déployer-sur-un-vrai-cloud-production-réelle)
- [Partie 18 — Projets pratiques](#partie-18--projets-pratiques)
- [Partie 19 — Glossaire et ressources](#partie-19--glossaire-et-ressources)

---

## Partie 0 — Prérequis

- Maîtrise de **Docker** : images, conteneurs, Dockerfile, notions de réseau et de volumes
- Bases de **YAML** (indentation, listes, clés-valeurs) — Kubernetes s'écrit presque exclusivement en YAML
- Aisance avec le terminal

---

## Partie 1 — Pourquoi Kubernetes ? Comprendre le problème

### 1.1 Le problème que Docker seul ne résout pas

Docker Compose (vu dans la formation précédente) est parfait pour faire tourner une application multi-conteneurs **sur une seule machine**. Mais en production réelle, de nouvelles questions se posent, auxquelles Compose ne répond pas :

- Que se passe-t-il si le **serveur entier tombe en panne** ? Compose ne peut rien faire — tout était sur cette machine.
- Comment répartir la charge sur **plusieurs machines** automatiquement ?
- Si un conteneur plante à 3h du matin, qui le redémarre ? (Compose seul ne surveille pas activement.)
- Comment **augmenter automatiquement** le nombre d'instances d'une application quand le trafic explose, puis les réduire quand le trafic redescend ?
- Comment déployer une nouvelle version **sans jamais couper le service**, avec la possibilité d'annuler instantanément si quelque chose ne va pas ?

**Kubernetes (souvent abrégé "K8s"** — K, 8 lettres, S) est un **orchestrateur de conteneurs** : un système qui gère automatiquement le déploiement, la mise à l'échelle, la résilience et la mise à jour d'applications conteneurisées, réparties sur un ensemble de machines (appelé un **cluster**).

### 1.2 L'idée centrale : la réconciliation permanente

Le concept le plus important à comprendre avant tout le reste : **avec Kubernetes, tu ne donnes jamais d'ordres directs (comme "lance ce conteneur"). Tu décris un état désiré ("je veux toujours 3 instances de cette application qui tournent"), et Kubernetes travaille en permanence pour que la réalité corresponde à cet état désiré.**

```
┌─────────────────────┐         ┌──────────────────────┐
│   ÉTAT DÉSIRÉ         │        │    ÉTAT RÉEL           │
│  "Je veux 3 copies   │◀──────▶│  observé en continu    │
│   de mon app qui     │ compare │  par Kubernetes        │
│   tournent"           │        │                        │
└─────────────────────┘         └──────────────────────┘
           │                               │
           └───────────► Si écart détecté ─┘
                    (ex: 1 copie a planté)
                              │
                              ▼
              Kubernetes relance automatiquement
              une nouvelle copie, sans intervention humaine
```

C'est ce qu'on appelle une **boucle de contrôle** (*control loop*). Ce principe est LA raison pour laquelle Kubernetes est si résilient : si un conteneur meurt, un nœud tombe en panne, ou une configuration dérive, Kubernetes corrige automatiquement l'écart entre ce que tu as demandé et ce qui existe réellement.

> 💡 **Analogie :** c'est comme un thermostat. Tu ne dis pas "allume le chauffage maintenant" — tu dis "je veux 20°C dans la pièce". Le thermostat surveille en continu et ajuste automatiquement, sans que tu aies à intervenir à chaque variation de température.

### 1.3 Kubernetes vs Docker Compose : quand utiliser quoi

| Critère | Docker Compose | Kubernetes |
|---|---|---|
| Nombre de machines | Une seule | Un cluster (plusieurs machines) |
| Auto-réparation (self-healing) | Non (sauf `restart: always` basique) | Oui, native et sophistiquée |
| Mise à l'échelle automatique | Non | Oui (basée sur CPU/RAM/métriques custom) |
| Déploiements sans interruption | Manuel, limité | Natif (rolling updates, rollback) |
| Complexité d'apprentissage | Faible | Élevée |
| Cas d'usage idéal | Développement local, petits projets, un seul serveur | Production à l'échelle, haute disponibilité |

> 💡 **Ce n'est pas "l'un ou l'autre" en carrière :** un développeur professionnel utilise Compose au quotidien pour développer localement, et Kubernetes pour déployer en production. Les deux se complètent.

---

## Partie 2 — Installation et premier cluster

### 2.1 Choisir son outil de cluster local

Pour apprendre, tu n'as pas besoin d'un vrai cluster de plusieurs serveurs — tu peux en simuler un sur ta propre machine.

| Outil | Description |
|---|---|
| **Minikube** | Le plus populaire pour débuter, simule un cluster complet dans une VM ou un conteneur |
| **Kind** (*Kubernetes IN Docker*) | Simule un cluster en utilisant des conteneurs Docker comme "nœuds" — très rapide |
| **Docker Desktop** | Intègre une option "Enable Kubernetes" en un clic (pratique mais moins flexible) |

Cette formation utilise **Minikube**, le plus pédagogique.

### 2.2 Installation

```bash
# Sur macOS (avec Homebrew)
brew install minikube kubectl

# Sur Linux
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Sur Windows (avec Chocolatey, en PowerShell administrateur)
choco install minikube kubernetes-cli
```

`kubectl` (prononcé "kube-control" ou "kube-cee-tee-el") est **l'outil en ligne de commande** que tu utiliseras pour tout piloter — c'est l'équivalent de la commande `docker` pour Docker.

### 2.3 Démarrer ton premier cluster

```bash
minikube start

# Vérifie que le cluster est opérationnel
kubectl get nodes

# Ouvre le tableau de bord graphique (très utile pour visualiser au début)
minikube dashboard
```

Tu devrais voir un nœud unique (`minikube`) avec le statut `Ready`. **Félicitations, tu as ton premier cluster Kubernetes.**

### 2.4 La commande `kubectl` : le couteau suisse

Toute ta formation va tourner autour de `kubectl`. Sa syntaxe générale :

```bash
kubectl <action> <type-de-ressource> <nom> [options]
```

Exemples :

```bash
kubectl get pods              # liste les pods
kubectl describe pod mon-pod  # détails complets d'un pod
kubectl delete pod mon-pod    # supprime un pod
kubectl apply -f fichier.yaml # applique une configuration décrite dans un fichier YAML
```

> 💡 **Différence fondamentale avec Docker :** avec `docker run`, tu lances des commandes impératives ("fais ceci maintenant"). Avec Kubernetes, la pratique professionnelle standard est **déclarative** : tu écris des fichiers YAML décrivant l'état désiré, et tu les appliques avec `kubectl apply -f`. On y reviendra en détail dans chaque partie.

---

## Partie 3 — L'architecture de Kubernetes en profondeur

### 3.1 Vue d'ensemble : Control Plane et Worker Nodes

![Architecture Kubernetes](https://commons.wikimedia.org/wiki/Special:FilePath/Kubernetes.png)

Un cluster Kubernetes se divise en deux grands types de machines :

```
┌───────────────────────────────────────────────────────────────┐
│                        CONTROL PLANE                             │
│              (le "cerveau" du cluster)                           │
│                                                                    │
│  ┌───────────┐  ┌────────────┐  ┌─────────────┐  ┌───────────┐  │
│  │ API Server │  │ Scheduler   │  │ Controller   │  │   etcd     │  │
│  │ (le point  │  │ (décide où  │  │ Manager      │  │ (base de   │  │
│  │  d'entrée) │  │  placer les │  │ (fait tourner│  │  données   │  │
│  │            │  │  pods)      │  │  les boucles │  │  du cluster│  │
│  │            │  │             │  │  de contrôle)│  │            │  │
│  └───────────┘  └────────────┘  └─────────────┘  └───────────┘  │
└──────────────────────────┬────────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        ▼                    ▼                    ▼
┌───────────────┐  ┌───────────────┐   ┌───────────────┐
│  WORKER NODE 1  │  │  WORKER NODE 2  │   │  WORKER NODE 3  │
│                 │  │                 │   │                 │
│  ┌───────────┐  │  │  ┌───────────┐  │   │  ┌───────────┐  │
│  │  kubelet   │  │  │  │  kubelet   │  │   │  │  kubelet   │  │
│  └───────────┘  │  │  └───────────┘  │   │  └───────────┘  │
│  ┌───────────┐  │  │  ┌───────────┐  │   │  ┌───────────┐  │
│  │ kube-proxy │  │  │  │ kube-proxy │  │   │  │ kube-proxy │  │
│  └───────────┘  │  │  └───────────┘  │   │  └───────────┘  │
│  ┌───────────┐  │  │  ┌───────────┐  │   │  ┌───────────┐  │
│  │  Pods      │  │  │  │  Pods      │  │   │  │  Pods      │  │
│  │ (tes apps) │  │  │  │ (tes apps) │  │   │  │ (tes apps) │  │
│  └───────────┘  │  │  └───────────┘  │   │  └───────────┘  │
└───────────────┘  └───────────────┘   └───────────────┘
```

### 3.2 Les composants du Control Plane expliqués

| Composant | Rôle |
|---|---|
| **API Server** | Le point d'entrée unique du cluster. **Tout** passe par lui : `kubectl` lui parle, les composants internes lui parlent. C'est la seule porte d'entrée. |
| **etcd** | Une base de données clé-valeur qui stocke **l'état complet du cluster** (quelles ressources existent, leur configuration). C'est la "mémoire" du cluster. |
| **Scheduler** | Décide **sur quel nœud** placer chaque nouveau pod, en fonction des ressources disponibles, des contraintes définies, etc. |
| **Controller Manager** | Fait tourner en permanence les **boucles de contrôle** (vue en Partie 1.2) qui comparent l'état désiré et l'état réel, et corrigent les écarts. |

### 3.3 Les composants des Worker Nodes expliqués

| Composant | Rôle |
|---|---|
| **kubelet** | L'agent qui tourne sur chaque nœud, communique avec l'API Server, et s'assure que les conteneurs demandés tournent réellement sur cette machine |
| **kube-proxy** | Gère les règles réseau sur le nœud, permettant aux pods de communiquer entre eux et de recevoir du trafic externe |
| **Container runtime** | Le logiciel qui exécute réellement les conteneurs (containerd, généralement — pas directement Docker depuis Kubernetes 1.24+) |

### 3.4 Le flux complet : que se passe-t-il quand tu déploies une application ?

1. Tu envoies un fichier YAML avec `kubectl apply -f deployment.yaml`
2. `kubectl` transmet cette requête à l'**API Server**
3. L'API Server valide la requête et l'enregistre dans **etcd**
4. Le **Controller Manager** détecte qu'un nouvel état désiré existe (ex : "3 pods doivent tourner") et crée les objets nécessaires
5. Le **Scheduler** décide sur quel(s) nœud(s) placer ces pods
6. Le **kubelet** du nœud concerné reçoit l'instruction et demande au **container runtime** de démarrer réellement les conteneurs
7. En continu, le kubelet rapporte l'état réel à l'API Server, qui le compare à l'état désiré — et la boucle de réconciliation recommence indéfiniment

### 🎯 Exercice pratique n°1

1. Lance `kubectl get componentstatuses` (ou `kubectl get pods -n kube-system` sur les versions récentes) pour observer les composants du control plane tournant eux-mêmes... dans des pods !
2. Observe avec `kubectl cluster-info` l'adresse de ton API Server

---

## Partie 4 — Le Pod : l'unité de base

### 4.1 Pourquoi pas directement "conteneur" ?

En Kubernetes, tu ne déploies **jamais directement un conteneur** — l'unité de base est le **Pod**. Un Pod est une enveloppe qui contient **un ou plusieurs conteneurs** partageant le même réseau (même adresse IP) et pouvant partager du stockage.

> 💡 **Dans la grande majorité des cas, un Pod contient un seul conteneur.** Le cas multi-conteneurs (motif "sidecar") sert pour des besoins précis : un conteneur principal + un conteneur auxiliaire (ex : un agent de collecte de logs qui tourne à côté de ton application).

### 4.2 Créer un Pod (pour comprendre — pas la méthode recommandée en pratique)

**`pod.yaml`**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mon-premier-pod
  labels:
    app: demo
spec:
  containers:
    - name: nginx
      image: nginx:1.27-alpine
      ports:
        - containerPort: 80
```

Décortiquons la structure YAML, **commune à TOUTES les ressources Kubernetes** :

- `apiVersion` : la version de l'API Kubernetes utilisée pour ce type d'objet
- `kind` : le type de ressource (`Pod`, `Deployment`, `Service`, etc.)
- `metadata` : informations d'identification (nom, labels, annotations)
- `spec` : la spécification — **ce que tu désires** (le cœur de la configuration)

```bash
kubectl apply -f pod.yaml
kubectl get pods
kubectl describe pod mon-premier-pod
kubectl logs mon-premier-pod
kubectl exec -it mon-premier-pod -- sh
kubectl delete pod mon-premier-pod
```

Tu reconnais ces commandes ? Elles sont volontairement très proches de leurs équivalents `docker` (`logs`, `exec`, etc.) — Kubernetes réutilise le vocabulaire que tu connais déjà.

### 4.3 Pourquoi on ne crée (presque) jamais un Pod directement

**Un Pod créé seul (comme ci-dessus) n'est PAS auto-réparé.** S'il plante, personne ne le relance — Kubernetes considère qu'il a atteint son état désiré : "0 pod" puisque tu n'as jamais demandé "maintiens toujours 1 pod en vie".

```bash
# Supprime le pod que tu viens de créer manuellement
kubectl delete pod mon-premier-pod
# Il ne réapparaît PAS tout seul — car tu n'as jamais demandé une surveillance continue
```

C'est exactement le problème que le **Deployment** résout — l'objet que tu utiliseras réellement à 95% du temps.

### 🎯 Exercice pratique n°2

1. Crée un Pod manuellement (comme ci-dessus) contenant une image `busybox` qui exécute `sleep 3600`
2. Vérifie son état avec `kubectl get pods -w` (le flag `-w` = *watch*, observe les changements en temps réel)
3. Dans un second terminal, supprime le pod et observe qu'il **ne redémarre pas** automatiquement

---

## Partie 5 — Le Deployment : déployer une application robuste

### 5.1 Le concept

Un **Deployment** est une couche au-dessus du Pod qui déclare : *"je veux N copies (réplicas) de ce Pod, en permanence, et gère leurs mises à jour."* C'est **l'objet que tu utiliseras pour quasiment toutes tes applications sans état** (stateless).

```
┌───────────────────────────────────────────────┐
│                  Deployment                      │
│         "je veux 3 réplicas de mon-app"          │
└───────────────────────┬─────────────────────────┘
                         │ crée et gère
                         ▼
              ┌─────────────────────┐
              │      ReplicaSet       │
              │  (assure qu'il y a    │
              │   toujours 3 pods)     │
              └──────────┬───────────┘
                          │ crée
        ┌─────────────────┼─────────────────┐
        ▼                  ▼                  ▼
   ┌────────┐         ┌────────┐         ┌────────┐
   │  Pod 1  │         │  Pod 2  │         │  Pod 3  │
   └────────┘         └────────┘         └────────┘
```

> 💡 En pratique, tu manipuleras rarement le **ReplicaSet** directement — c'est un objet intermédiaire que le Deployment gère pour toi (notamment pour permettre les mises à jour progressives, voir Partie 10).

### 5.2 Créer ton premier Deployment

**`deployment.yaml`**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mon-app
  labels:
    app: mon-app
spec:
  replicas: 3                    # combien de copies je veux en permanence
  selector:
    matchLabels:
      app: mon-app                # comment le Deployment identifie SES pods
  template:                       # le modèle utilisé pour créer chaque pod
    metadata:
      labels:
        app: mon-app               # DOIT correspondre au selector ci-dessus
    spec:
      containers:
        - name: mon-app
          image: nginx:1.27-alpine
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: "100m"          # 0.1 CPU minimum garanti
              memory: "128Mi"
            limits:
              cpu: "500m"          # 0.5 CPU maximum autorisé
              memory: "256Mi"
```

> ⚠️ **Point souvent source d'erreur pour les débutants :** `selector.matchLabels` DOIT correspondre exactement aux `labels` définis dans `template.metadata.labels`. C'est ainsi que le Deployment "reconnaît" quels pods lui appartiennent.

```bash
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get pods -l app=mon-app
```

### 5.3 Observer l'auto-réparation en action

```bash
# Récupère le nom d'un des pods créés
kubectl get pods -l app=mon-app

# Supprime-le volontairement
kubectl delete pod <nom-du-pod>

# Regarde immédiatement l'état — un NOUVEAU pod apparaît automatiquement !
kubectl get pods -l app=mon-app
```

**C'est ça, la magie de Kubernetes.** Tu as supprimé un pod, mais le Deployment a immédiatement remarqué l'écart ("je veux 3 pods, il n'y en a que 2") et en a recréé un nouveau pour revenir à l'état désiré.

### 5.4 `requests` et `limits` : gérer les ressources correctement

- **`requests`** : la quantité de ressources **garantie** pour le conteneur. Le Scheduler s'en sert pour décider sur quel nœud placer le pod (il vérifie qu'il y a assez de place).
- **`limits`** : la quantité **maximale** que le conteneur peut consommer. S'il dépasse la limite mémoire, il est arrêté (`OOMKilled`). S'il dépasse la limite CPU, il est simplement ralenti (`throttled`), pas tué.

> ⚠️ **Ne jamais omettre `requests`/`limits` en production** : sans eux, un pod défaillant pourrait consommer toutes les ressources du nœud et affecter tous les autres pods qui s'y trouvent.

### 🎯 Exercice pratique n°3

1. Crée un Deployment avec 3 réplicas d'une image de ton choix
2. Supprime deux pods d'un coup et observe la vitesse de récupération
3. Modifie le Deployment pour passer à 5 réplicas (`kubectl edit deployment mon-app`, ou modifie le YAML et réapplique), observe les 2 nouveaux pods apparaître

---

## Partie 6 — Le Service : exposer et joindre tes applications

### 6.1 Le problème

Les pods sont **éphémères** : ils sont recréés en permanence par le Deployment (à chaque suppression, mise à jour, panne), et **chaque nouveau pod obtient une nouvelle adresse IP**. Comment un autre composant (ou un utilisateur) peut-il joindre ton application de façon fiable si son adresse change constamment ?

**Le Service** fournit une **adresse stable** (IP fixe + nom DNS) qui redirige automatiquement le trafic vers les pods actuellement vivants — quels qu'ils soient.

```
┌────────────────────────────────────────────┐
│                  Service                      │
│         mon-app-service (IP stable)           │
│    répartit le trafic entre les pods vivants  │
└───────────────────┬────────────────────────┘
                     │
       ┌──────────────┼──────────────┐
       ▼                ▼                ▼
  ┌────────┐       ┌────────┐       ┌────────┐
  │  Pod 1  │       │  Pod 2  │       │  Pod 3  │
  │(IP change│      │(IP change│      │(IP change│
  │ si recréé)│      │ si recréé)│      │ si recréé)│
  └────────┘       └────────┘       └────────┘
```

### 6.2 Les types de Service

| Type | Description | Cas d'usage |
|---|---|---|
| **ClusterIP** (par défaut) | Accessible uniquement **depuis l'intérieur** du cluster | Communication entre microservices internes (ex : API → base de données) |
| **NodePort** | Ouvre un port fixe sur **chaque nœud** du cluster, accessible depuis l'extérieur | Tests simples, environnements sans load balancer externe |
| **LoadBalancer** | Provisionne un load balancer chez ton fournisseur cloud (AWS, GCP, Azure...) | Exposer une application publiquement en production sur le cloud |

### 6.3 Créer un Service ClusterIP (communication interne)

**`service.yaml`**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mon-app-service
spec:
  type: ClusterIP
  selector:
    app: mon-app          # cible tous les pods portant ce label
  ports:
    - port: 80              # le port exposé PAR le Service
      targetPort: 80        # le port sur lequel le conteneur écoute réellement
```

```bash
kubectl apply -f service.yaml
kubectl get services
```

Depuis n'importe quel autre pod du cluster, tu peux maintenant joindre ton application via `http://mon-app-service` (résolution DNS automatique, exactement comme les noms de service en Docker Compose) — sans jamais te soucier des adresses IP changeantes des pods individuels.

### 6.4 Exposer temporairement en local pour tester

```bash
# Redirige un port de ta machine vers le Service, pour tester sans configuration complexe
kubectl port-forward service/mon-app-service 8080:80
# Ouvre http://localhost:8080 dans ton navigateur
```

### 6.5 NodePort (exposition simple pour l'apprentissage)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mon-app-nodeport
spec:
  type: NodePort
  selector:
    app: mon-app
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080     # doit être entre 30000-32767
```

```bash
minikube service mon-app-nodeport --url
```

### 🎯 Exercice pratique n°4

1. Crée un Deployment de 3 réplicas et un Service ClusterIP pointant dessus
2. Lance un pod temporaire de debug : `kubectl run debug --image=busybox -it --rm -- sh`
3. Depuis ce pod de debug, fais `wget -O- http://mon-app-service` plusieurs fois et observe (via les logs ou en modifiant temporairement chaque pod) que les requêtes sont réparties entre les différents pods

---

## Partie 7 — Ingress : exposer proprement sur Internet

### 7.1 Le problème avec NodePort et LoadBalancer seuls

`NodePort` n'est pas adapté à la production (ports non-standards, pas de nom de domaine). `LoadBalancer` fonctionne bien, mais **un LoadBalancer par service** devient rapidement coûteux et complexe si tu as 10 applications différentes à exposer.

**L'Ingress** est une couche supplémentaire qui permet de **router le trafic HTTP/HTTPS externe vers différents Services, selon le nom de domaine ou le chemin de l'URL**, avec un seul point d'entrée.

```
                    Internet
                        │
                        ▼
              ┌──────────────────┐
              │  Ingress Controller │  (un seul point d'entrée, un seul LoadBalancer)
              └─────────┬──────────┘
                         │ routage selon domaine/chemin
        ┌─────────────────┼─────────────────┐
        ▼                   ▼                   ▼
  api.monsite.com    app.monsite.com     monsite.com/blog
        │                   │                   │
        ▼                   ▼                   ▼
  Service "api"      Service "frontend"   Service "blog"
```

### 7.2 Installer un Ingress Controller (nécessaire — Ingress ne fonctionne pas sans lui)

```bash
minikube addons enable ingress
```

### 7.3 Créer une ressource Ingress

**`ingress.yaml`**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: mon-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: monapp.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: mon-app-service
                port:
                  number: 80
```

```bash
kubectl apply -f ingress.yaml

# En local, ajoute une entrée dans ton fichier hosts pointant vers l'IP de minikube
echo "$(minikube ip) monapp.local" | sudo tee -a /etc/hosts
```

Ouvre `http://monapp.local` dans ton navigateur — le trafic passe par l'Ingress Controller, qui le route vers ton Service.

### 🎯 Exercice pratique n°5

1. Déploie deux applications différentes (deux Deployments + deux Services distincts)
2. Crée un seul Ingress avec deux règles de chemin (`/app1` et `/app2`) routant vers chacune
3. Vérifie que les deux sont accessibles via le même nom de domaine, sur des chemins différents

---

## Partie 8 — Configuration : ConfigMap et Secret

### 8.1 Le problème

Comme pour Docker, tu ne dois **jamais** mettre de configuration ou de secrets en dur dans tes images. Kubernetes fournit deux objets dédiés.

### 8.2 ConfigMap : configuration non sensible

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mon-app-config
data:
  APP_ENV: "production"
  LOG_LEVEL: "info"
  MAX_CONNECTIONS: "100"
```

Utilisation dans un Deployment :

```yaml
spec:
  containers:
    - name: mon-app
      image: mon-image:1.0
      envFrom:
        - configMapRef:
            name: mon-app-config
```

### 8.3 Secret : données sensibles

```bash
# Créer un Secret directement en ligne de commande (évite de l'écrire en clair dans un fichier versionné)
kubectl create secret generic mon-app-secret \
  --from-literal=DATABASE_PASSWORD=motdepasse-solide \
  --from-literal=API_KEY=sk-abc123
```

Ou en YAML (les valeurs doivent être encodées en base64 — ce **n'est pas du chiffrement**, juste un encodage) :

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mon-app-secret
type: Opaque
data:
  DATABASE_PASSWORD: bW90ZGVwYXNzZS1zb2xpZGU=   # echo -n "motdepasse-solide" | base64
```

> ⚠️ **Piège de sécurité fréquent :** un Secret Kubernetes n'est **pas chiffré par défaut** dans `etcd` (juste encodé en base64, trivialement réversible). Pour une vraie sécurité en production, active le chiffrement au repos d'`etcd`, ou utilise un gestionnaire externe (HashiCorp Vault, AWS Secrets Manager, Sealed Secrets).

Utilisation dans un Deployment :

```yaml
spec:
  containers:
    - name: mon-app
      image: mon-image:1.0
      envFrom:
        - secretRef:
            name: mon-app-secret
```

> ⚠️ **Ne versionne jamais un fichier Secret YAML contenant de vraies valeurs sur Git.** Utilise `kubectl create secret` directement en ligne de commande, ou des outils dédiés (Sealed Secrets, External Secrets Operator) qui permettent de versionner des secrets chiffrés en toute sécurité.

### 🎯 Exercice pratique n°6

1. Crée un ConfigMap avec 3 variables de configuration
2. Crée un Secret avec un mot de passe factice
3. Modifie ton Deployment pour injecter les deux, puis vérifie avec `kubectl exec -it <pod> -- env` que les variables sont bien présentes dans le conteneur

---

## Partie 9 — Stockage : Volumes, PV et PVC

### 9.1 Le problème

Comme pour Docker, les données à l'intérieur d'un pod disparaissent quand il est supprimé. Kubernetes ajoute une couche d'abstraction supplémentaire pour gérer le stockage à l'échelle d'un cluster (potentiellement réparti sur plusieurs machines physiques).

### 9.2 Les trois niveaux d'abstraction

| Objet | Rôle |
|---|---|
| **PersistentVolume (PV)** | Un morceau de stockage réel, provisionné par un administrateur ou automatiquement par le cloud |
| **PersistentVolumeClaim (PVC)** | Une "demande" de stockage faite par une application, sans se soucier du détail technique du PV sous-jacent |
| **StorageClass** | Un modèle qui définit comment provisionner automatiquement des PV à la demande |

```
┌─────────────┐   demande    ┌──────────────────────┐   satisfait par   ┌─────────────┐
│  Pod         │ ───────────▶ │ PersistentVolumeClaim │ ─────────────────▶│ PersistentVolume│
│ (mon-app)    │              │  "je veux 5Go"          │                   │  (stockage réel)│
└─────────────┘              └──────────────────────┘                   └─────────────┘
```

### 9.3 Exemple concret : une base de données avec stockage persistant

**`pvc.yaml`**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-pvc
spec:
  accessModes:
    - ReadWriteOnce      # un seul nœud peut monter ce volume en lecture-écriture
  resources:
    requests:
      storage: 5Gi
```

**`postgres-deployment.yaml`**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres
spec:
  replicas: 1              # ⚠️ une base de données classique ne se réplique pas naïvement à plusieurs instances
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres:16-alpine
          env:
            - name: POSTGRES_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mon-app-secret
                  key: DATABASE_PASSWORD
          volumeMounts:
            - name: donnees
              mountPath: /var/lib/postgresql/data
      volumes:
        - name: donnees
          persistentVolumeClaim:
            claimName: postgres-pvc
```

> ⚠️ **Point d'attention important :** une base de données avec `replicas: 3` sur un Deployment classique créerait **3 instances indépendantes avec chacune leurs propres données** — ce qui casse la cohérence ! Les bases de données nécessitent des mécanismes de réplication spécifiques (souvent gérés via un objet appelé **StatefulSet**, hors du périmètre "débutant/intermédiaire" de cette formation, mais à connaître de nom).

### 🎯 Exercice pratique n°7

1. Crée un PVC de 2Gi
2. Déploie une base PostgreSQL utilisant ce PVC
3. Crée une table et insère des données
4. Supprime le pod PostgreSQL (pas le PVC), attends que le Deployment le recrée, et vérifie que les données sont toujours là

---

## Partie 10 — Stratégies de déploiement d'applications

C'est le cœur de la question **"comment déployer une nouvelle version sans casser la production"** — la compétence la plus recherchée chez un ingénieur DevOps.

### 10.1 Rolling Update (déploiement progressif) — le comportement par défaut

Quand tu changes l'image d'un Deployment, Kubernetes remplace les pods **progressivement**, jamais tous en même temps, pour éviter toute interruption de service.

```yaml
spec:
  replicas: 5
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1     # au maximum 1 pod indisponible pendant la mise à jour
      maxSurge: 1            # au maximum 1 pod supplémentaire créé temporairement
```

```bash
# Met à jour l'image d'un Deployment existant
kubectl set image deployment/mon-app mon-app=mon-image:2.0

# Observe le déploiement progressif en temps réel
kubectl rollout status deployment/mon-app
```

```
Avant :  [v1] [v1] [v1] [v1] [v1]
Étape 1: [v2] [v1] [v1] [v1] [v1]   ← 1 nouveau pod créé, 1 ancien retiré progressivement
Étape 2: [v2] [v2] [v1] [v1] [v1]
Étape 3: [v2] [v2] [v2] [v1] [v1]
Étape 4: [v2] [v2] [v2] [v2] [v1]
Après :  [v2] [v2] [v2] [v2] [v2]
```

À aucun moment il n'y a "zéro pod disponible" : le service reste accessible en continu pendant toute la mise à jour.

### 10.2 Rollback : annuler instantanément une mauvaise mise à jour

```bash
# Voir l'historique des déploiements
kubectl rollout history deployment/mon-app

# Revenir à la version précédente immédiatement
kubectl rollout undo deployment/mon-app

# Revenir à une révision précise
kubectl rollout undo deployment/mon-app --to-revision=2
```

> 💡 **C'est l'un des plus grands avantages de Kubernetes en production** : si une mise à jour casse quelque chose, un simple `rollout undo` restaure l'état précédent en quelques secondes, avec la même mécanique de rolling update progressif (donc sans coupure non plus).

### 10.3 Readiness Probe et Liveness Probe : la clé d'un déploiement vraiment sûr

Sans ces vérifications, Kubernetes pourrait considérer un pod "prêt" alors que ton application n'a même pas fini de démarrer — provoquant des erreurs pour les utilisateurs pendant un rolling update.

```yaml
spec:
  containers:
    - name: mon-app
      image: mon-image:2.0
      readinessProbe:            # "ce pod est-il prêt à recevoir du trafic ?"
        httpGet:
          path: /health
          port: 8080
        initialDelaySeconds: 5
        periodSeconds: 10
      livenessProbe:             # "ce pod est-il toujours vivant, ou faut-il le redémarrer ?"
        httpGet:
          path: /health
          port: 8080
        initialDelaySeconds: 15
        periodSeconds: 20
```

| Probe | Rôle | Que se passe-t-il en cas d'échec ? |
|---|---|---|
| **readinessProbe** | Vérifie si le pod est prêt à recevoir du trafic | Le pod est **retiré temporairement** du Service (plus de trafic envoyé), mais pas redémarré |
| **livenessProbe** | Vérifie si le pod fonctionne toujours correctement | Le pod est **redémarré** |

> ⚠️ **Sans `readinessProbe`, un rolling update peut envoyer du trafic à un pod pas encore prêt** (par exemple encore en train de se connecter à sa base de données), provoquant des erreurs 500 côté utilisateur — même si techniquement "aucune interruption" n'a eu lieu au niveau du Deployment. C'est LA cause n°1 des incidents lors de déploiements Kubernetes mal configurés.

### 10.4 Blue-Green Deployment (déploiement bleu-vert)

Une stratégie alternative : faire tourner **deux versions complètes en parallèle** (l'ancienne "bleue" et la nouvelle "verte"), puis basculer instantanément tout le trafic d'un coup en changeant simplement le `selector` du Service.

```
Étape 1 : le Service pointe vers "bleu" (v1), "vert" (v2) tourne en parallèle mais reçoit 0 trafic
Étape 2 : tests de validation sur "vert" en interne
Étape 3 : bascule INSTANTANÉE du Service vers "vert"
Étape 4 : si problème détecté, retour INSTANTANÉ vers "bleu"
```

```yaml
# Service pointant sur la version active — change juste le label ciblé pour basculer
apiVersion: v1
kind: Service
metadata:
  name: mon-app-service
spec:
  selector:
    app: mon-app
    version: bleu    # ← change ceci en "vert" pour basculer instantanément
  ports:
    - port: 80
      targetPort: 8080
```

**Avantage sur le Rolling Update :** bascule et retour en arrière **instantanés** (pas de période de transition avec deux versions mélangées). **Inconvénient :** nécessite deux fois plus de ressources pendant la transition (les deux versions tournent en parallèle).

### 10.5 Canary Deployment (déploiement canari)

Une troisième stratégie : envoyer **une petite fraction du trafic** (ex : 5%) vers la nouvelle version, observer si tout va bien, puis augmenter progressivement cette fraction jusqu'à 100%.

```
Trafic total : 100%
├── 95% → version stable (v1)
└── 5%  → version canari (v2)   ← si aucune erreur détectée, on augmente progressivement
```

En Kubernetes natif, cela se fait souvent en jouant sur le **nombre de réplicas relatifs** entre deux Deployments partageant le même `selector` de Service :

```yaml
# Deployment stable : 19 réplicas
# Deployment canari  : 1 réplique
# → environ 5% du trafic va vers la version canari (1 pod sur 20 au total)
```

> 💡 Pour un contrôle plus fin et automatisé (pourcentages précis, bascule automatique basée sur les métriques d'erreur), on utilise généralement des outils spécialisés comme **Argo Rollouts** ou un **service mesh** (Istio, Linkerd) — à explorer une fois les bases solidement acquises.

### 10.6 Comparatif des stratégies

| Stratégie | Ressources nécessaires | Vitesse de rollback | Complexité | Cas d'usage |
|---|---|---|---|---|
| **Rolling Update** | Normales (+1 pod temporaire) | Rapide (mais progressif) | Faible | Le choix par défaut pour la majorité des applications |
| **Blue-Green** | Doublées temporairement | Instantané | Moyenne | Applications critiques nécessitant un retour arrière immédiat |
| **Canary** | Normales | Rapide (réduire le trafic canari) | Élevée | Grandes applications, validation progressive sur du trafic réel |

### 🎯 Exercice pratique n°8

1. Déploie une application avec 5 réplicas et des probes de readiness/liveness correctement configurées
2. Effectue une mise à jour vers une nouvelle version de l'image en observant `kubectl rollout status`
3. Simule une mauvaise mise à jour (déploie une image invalide) et pratique un `kubectl rollout undo`
4. Bonus : implémente un déploiement blue-green manuel avec deux Deployments distincts et un Service dont tu changes le selector

---

## Partie 11 — Mise à l'échelle (Scaling)

### 11.1 Mise à l'échelle manuelle

```bash
kubectl scale deployment mon-app --replicas=10
```

### 11.2 Horizontal Pod Autoscaler (HPA) : mise à l'échelle automatique

Le HPA ajuste automatiquement le nombre de réplicas en fonction de métriques observées (CPU, RAM, ou métriques personnalisées).

```bash
# Nécessite le metrics-server (souvent à activer sur minikube)
minikube addons enable metrics-server
```

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: mon-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: mon-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70    # ajoute des réplicas si la moyenne dépasse 70% CPU
```

```bash
kubectl apply -f hpa.yaml
kubectl get hpa -w   # observe les ajustements automatiques en temps réel
```

> 💡 **Pour que le HPA fonctionne, `requests.cpu` doit obligatoirement être défini** sur le Deployment (vu en Partie 5.4) — c'est la valeur de référence sur laquelle le pourcentage d'utilisation est calculé.

### 11.3 Vertical Pod Autoscaler (VPA) — mention rapide

Alors que le HPA ajoute/retire des **pods**, le VPA ajuste automatiquement les `requests`/`limits` **d'un pod existant**. Moins utilisé en pratique (nécessite souvent un redémarrage du pod pour appliquer les changements), mais utile à connaître de nom.

### 🎯 Exercice pratique n°9

1. Déploie une application avec des `requests.cpu` définis et un HPA (min 2, max 8 réplicas, seuil 50%)
2. Génère de la charge artificielle (par exemple avec un pod exécutant une boucle de requêtes en continu vers ton Service)
3. Observe le nombre de réplicas augmenter automatiquement, puis redescendre une fois la charge arrêtée

---

## Partie 12 — Helm : le gestionnaire de paquets Kubernetes

### 12.1 Le problème

À mesure que ton application grandit, tu te retrouves avec des dizaines de fichiers YAML (Deployment, Service, Ingress, ConfigMap, Secret, HPA...) pour une seule application. Gérer différentes configurations pour différents environnements (dev, staging, production) devient vite répétitif et source d'erreurs.

**Helm** est le gestionnaire de paquets de Kubernetes : il permet de packager un ensemble de ressources Kubernetes en un **Chart** réutilisable, paramétrable via des variables.

### 12.2 Installation

```bash
brew install helm          # macOS
# ou
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash   # Linux
```

### 12.3 Installer une application existante avec Helm

Un des plus grands avantages de Helm : profiter de milliers de charts déjà écrits par la communauté pour des outils complexes.

```bash
# Ajouter un dépôt de charts (ici, celui de Bitnami, très complet)
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

# Installer PostgreSQL en une seule commande (au lieu d'écrire Deployment+Service+PVC+Secret à la main)
helm install ma-base bitnami/postgresql --set auth.password=motdepasse

# Voir ce qui a été installé
helm list
kubectl get pods
```

### 12.4 Créer ton propre Chart

```bash
helm create mon-app
```

Cela génère une structure de dossiers :

```
mon-app/
├── Chart.yaml           # métadonnées du chart
├── values.yaml           # valeurs par défaut (paramétrables)
└── templates/
    ├── deployment.yaml
    ├── service.yaml
    └── ingress.yaml
```

**`values.yaml`** (les paramètres modifiables)

```yaml
replicaCount: 3
image:
  repository: mon-utilisateur/mon-app
  tag: "1.0"
resources:
  requests:
    cpu: 100m
    memory: 128Mi
```

**`templates/deployment.yaml`** (utilise la syntaxe de templating Go)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-mon-app
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}
    spec:
      containers:
        - name: mon-app
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
```

```bash
# Déployer avec les valeurs par défaut
helm install ma-release ./mon-app

# Déployer avec des valeurs différentes pour un environnement de production
helm install ma-release ./mon-app --set replicaCount=10 --set image.tag=2.0

# Mettre à jour un déploiement existant
helm upgrade ma-release ./mon-app --set image.tag=2.1

# Revenir en arrière
helm rollback ma-release 1

# Désinstaller
helm uninstall ma-release
```

> 💡 **Analogie utile :** si un fichier YAML brut est comme une variable codée en dur, un Chart Helm est comme une **fonction avec des paramètres** — tu écris la logique une fois, et tu l'appelles avec des valeurs différentes selon le contexte (dev/staging/prod).

### 🎯 Exercice pratique n°10

1. Installe une application depuis un chart Helm public (ex : `bitnami/redis`)
2. Crée ton propre chart pour une application que tu as containerisée dans la formation Docker
3. Déploie-la deux fois avec des noms de release différents et des valeurs de `replicaCount` différentes, pour simuler un environnement "staging" et un environnement "production"

---

## Partie 13 — Observabilité : logs, métriques, debug

### 13.1 Consulter les logs

```bash
kubectl logs <nom-du-pod>
kubectl logs -f <nom-du-pod>                    # en temps réel
kubectl logs <nom-du-pod> -c <nom-du-conteneur>  # si le pod a plusieurs conteneurs
kubectl logs <nom-du-pod> --previous             # logs du conteneur AVANT son dernier redémarrage (essentiel pour déboguer un crash)

# Logs de TOUS les pods correspondant à un label
kubectl logs -l app=mon-app --all-containers --prefix
```

### 13.2 Diagnostiquer un pod qui ne démarre pas

```bash
kubectl describe pod <nom-du-pod>
```

Regarde particulièrement la section **`Events`** en bas de la sortie — c'est souvent là que se trouve la véritable cause du problème (image introuvable, ressources insuffisantes, probe qui échoue, volume qui ne peut pas être monté, etc.)

### 13.3 Les états d'un pod et leur signification

| État | Signification | Cause probable |
|---|---|---|
| `Pending` | Le pod n'a pas encore été placé sur un nœud | Ressources insuffisantes sur le cluster, ou PVC non satisfait |
| `ContainerCreating` | Le nœud prépare le conteneur | Téléchargement de l'image en cours, montage de volumes |
| `ImagePullBackOff` / `ErrImagePull` | Impossible de télécharger l'image | Nom d'image incorrect, tag inexistant, registre privé sans les bons identifiants |
| `CrashLoopBackOff` | Le conteneur démarre puis plante en boucle | Erreur dans l'application — regarde les logs avec `--previous` |
| `Running` | Tout va bien | — |
| `OOMKilled` (visible dans `describe`) | Le conteneur a dépassé sa limite mémoire | Augmente `limits.memory`, ou corrige une fuite mémoire dans l'application |

### 13.4 Méthode systématique de débogage Kubernetes

1. `kubectl get pods` — quel est l'état du pod ?
2. `kubectl describe pod <nom>` — que disent les `Events` ?
3. `kubectl logs <nom>` (et `--previous` si redémarré) — que dit l'application ?
4. `kubectl exec -it <nom> -- sh` (si le pod tourne) — explore manuellement
5. `kubectl get events --sort-by=.lastTimestamp` — vue globale des événements récents du cluster

### 13.5 Vers une observabilité de production : Prometheus et Grafana (aperçu)

En production, on ne surveille pas manuellement avec `kubectl` en continu. La stack standard de l'écosystème Kubernetes est :

- **Prometheus** : collecte et stocke des métriques dans le temps (CPU, RAM, latence, taux d'erreur...)
- **Grafana** : visualise ces métriques sous forme de tableaux de bord
- **Alertmanager** : envoie des alertes (Slack, email, PagerDuty...) quand un seuil critique est dépassé

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install monitoring prometheus-community/kube-prometheus-stack
```

Cette stack complète mérite une formation à part entière, mais sache qu'elle s'installe en une seule commande Helm grâce à ce que tu as appris en Partie 12.

### 🎯 Exercice pratique n°11

1. Crée volontairement un Deployment avec une image inexistante (`mon-image:version-qui-n-existe-pas`) et diagnostique l'erreur avec la méthode systématique
2. Crée un pod qui plante immédiatement au démarrage (ex : `CMD ["false"]` dans le Dockerfile) et observe le `CrashLoopBackOff`, puis consulte ses logs avec `--previous`

---

## Partie 14 — Sécurité en profondeur

### 14.1 SecurityContext : restreindre les privilèges d'un pod

```yaml
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
  containers:
    - name: mon-app
      image: mon-image:1.0
      securityContext:
        allowPrivilegeEscalation: false
        readOnlyRootFilesystem: true
        capabilities:
          drop:
            - ALL
```

- `runAsNonRoot: true` : refuse de démarrer le conteneur s'il tente de s'exécuter en tant que root
- `readOnlyRootFilesystem: true` : le système de fichiers du conteneur est en lecture seule (empêche un attaquant d'y écrire du code malveillant)
- `capabilities.drop: ALL` : retire toutes les capacités système Linux non essentielles

### 14.2 Network Policies : contrôler qui peut parler à qui

Par défaut, **tous les pods d'un cluster peuvent communiquer entre eux**, sans restriction. Une NetworkPolicy permet de restreindre ce trafic — un principe de sécurité essentiel appelé *least privilege* (privilège minimal).

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: autoriser-uniquement-api-vers-db
spec:
  podSelector:
    matchLabels:
      app: postgres
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: api          # SEULS les pods labellisés "app: api" peuvent joindre la base
      ports:
        - protocol: TCP
          port: 5432
```

### 14.3 RBAC : contrôler qui peut faire quoi sur le cluster

Le **Role-Based Access Control** définit précisément quelles actions un utilisateur ou un pod peut effectuer.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: lecteur-pods
  namespace: default
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]     # lecture seule, aucune suppression/modification possible
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: lier-lecteur-pods
  namespace: default
subjects:
  - kind: User
    name: nouveau-developpeur
roleRef:
  kind: Role
  name: lecteur-pods
  apiGroup: rbac.authorization.k8s.io
```

### 14.4 Namespaces : cloisonner logiquement le cluster

Un **Namespace** permet de séparer des ensembles de ressources au sein d'un même cluster physique (par exemple, séparer les environnements `dev`, `staging`, `production`).

```bash
kubectl create namespace production
kubectl apply -f deployment.yaml -n production
kubectl get pods -n production

# Voir tous les namespaces
kubectl get namespaces
```

> 💡 En combinant Namespaces + NetworkPolicies + RBAC, tu obtiens une isolation forte entre environnements ou équipes, même au sein d'un unique cluster physique.

### 14.5 Checklist de sécurité Kubernetes avant mise en production

- [ ] `runAsNonRoot: true` sur tous les pods
- [ ] `requests`/`limits` définis sur tous les conteneurs
- [ ] Secrets gérés via un outil dédié, jamais en clair dans Git
- [ ] NetworkPolicies limitant les communications au strict nécessaire
- [ ] RBAC configuré avec le principe du moindre privilège
- [ ] Images scannées pour les vulnérabilités (Trivy, Docker Scout)
- [ ] `readinessProbe` et `livenessProbe` définies sur tous les Deployments
- [ ] Namespaces séparant clairement les environnements
- [ ] Chiffrement d'`etcd` activé au repos (sur un cluster géré/production)

---

## Partie 15 — CI/CD et GitOps : déploiement automatisé en production

### 15.1 Pipeline CI/CD classique vers Kubernetes

```yaml
# .github/workflows/deploy.yml
name: Build, Push and Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Connexion au registre
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build et push de l'image
        uses: docker/build-push-action@v5
        with:
          push: true
          tags: ghcr.io/${{ github.repository }}:${{ github.sha }}

      - name: Configuration de kubectl
        uses: azure/setup-kubectl@v4

      - name: Déploiement sur le cluster
        run: |
          echo "${{ secrets.KUBE_CONFIG }}" > kubeconfig.yaml
          export KUBECONFIG=kubeconfig.yaml
          kubectl set image deployment/mon-app mon-app=ghcr.io/${{ github.repository }}:${{ github.sha }}
          kubectl rollout status deployment/mon-app
```

### 15.2 Le principe GitOps : Git comme unique source de vérité

L'approche **GitOps**, de plus en plus standard, pousse le principe déclaratif de Kubernetes encore plus loin : au lieu que ta CI/CD **exécute** des commandes `kubectl` vers le cluster, un agent (comme **ArgoCD** ou **Flux**) **tournant à l'intérieur du cluster** surveille en continu un dépôt Git, et synchronise automatiquement l'état du cluster avec ce qui est décrit dans Git.

```
┌──────────────┐   push   ┌─────────────────┐   surveille en continu   ┌──────────────┐
│ Développeur   │ ───────▶ │  Dépôt Git        │◀─────────────────────── │   ArgoCD       │
│  (git push)   │          │ (manifests YAML)  │                          │ (dans le cluster)│
└──────────────┘          └─────────────────┘                          └───────┬──────┘
                                                                                  │ applique automatiquement
                                                                                  ▼
                                                                          ┌──────────────┐
                                                                          │   Cluster K8s  │
                                                                          └──────────────┘
```

**Avantages du GitOps :**

- **Traçabilité totale** : chaque changement d'infrastructure est un commit Git, avec auteur, date, et possibilité de revert
- **Sécurité renforcée** : le cluster n'a pas besoin d'exposer d'accès entrant à la CI/CD — c'est l'agent interne qui va chercher les changements
- **Cohérence garantie** : impossible qu'un `kubectl apply` manuel fait "à la va-vite" désynchronise le cluster de ce qui est documenté

> 💡 GitOps mérite sa propre exploration approfondie une fois les bases de cette formation solidement acquises — retiens le principe : **Git décrit l'état désiré, un agent dans le cluster s'assure qu'il est respecté en permanence.**

---

## Partie 16 — Déployer des applications IA sur Kubernetes

### 16.1 Spécificités des charges de travail IA sur Kubernetes

Déployer des agents IA ou des modèles de machine learning sur Kubernetes ajoute des contraintes particulières par rapport à une API classique :

- **Besoins en GPU** parfois nécessaires (pour l'inférence de modèles locaux)
- **Démarrage plus lent** (chargement de modèles volumineux) — attention aux probes trop strictes
- **Coûts d'API externes** (OpenAI, Anthropic) à surveiller — chaque pod qui scale peut multiplier les appels
- **Composants additionnels fréquents** : base de données vectorielle, files de tâches asynchrones, cache de réponses

### 16.2 Exemple complet : déployer l'agent IA de la formation Docker sur Kubernetes

Reprenons l'agent FastAPI + API Claude construit dans la formation Docker (Partie 13), et déployons-le proprement sur Kubernetes.

**`agent-secret.yaml`** — la clé API, créée en ligne de commande plutôt que versionnée :

```bash
kubectl create secret generic agent-ia-secret \
  --from-literal=ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY
```

**`agent-deployment.yaml`**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: agent-ia
spec:
  replicas: 2
  selector:
    matchLabels:
      app: agent-ia
  template:
    metadata:
      labels:
        app: agent-ia
    spec:
      containers:
        - name: agent-ia
          image: ghcr.io/mon-utilisateur/mon-agent-ia:1.0
          ports:
            - containerPort: 8000
          envFrom:
            - secretRef:
                name: agent-ia-secret
          resources:
            requests:
              cpu: "250m"
              memory: "256Mi"
            limits:
              cpu: "1"
              memory: "512Mi"
          readinessProbe:
            httpGet:
              path: /health
              port: 8000
            initialDelaySeconds: 5
            periodSeconds: 10
          livenessProbe:
            httpGet:
              path: /health
              port: 8000
            initialDelaySeconds: 15
            periodSeconds: 20
---
apiVersion: v1
kind: Service
metadata:
  name: agent-ia-service
spec:
  selector:
    app: agent-ia
  ports:
    - port: 80
      targetPort: 8000
```

```bash
kubectl apply -f agent-deployment.yaml
kubectl port-forward service/agent-ia-service 8000:80
```

### 16.3 Architecture RAG complète sur Kubernetes

Reprenons l'architecture RAG de la formation Docker (agent + base vectorielle + worker + Redis), et transposons-la en manifests Kubernetes.

```
┌─────────────────────────────────────────────────────────────────┐
│                          Namespace: rag-app                        │
│                                                                     │
│   Ingress ──▶ Service(agent) ──▶ Deployment(agent, 2-5 replicas)   │
│                                        │ HPA basé sur CPU            │
│                                        ▼                             │
│                              Service(vector-db) ──▶ StatefulSet     │
│                                                     (Qdrant + PVC)   │
│                                        ▲                             │
│                                        │                             │
│   Service(queue) ──▶ Deployment(Redis)│                             │
│           ▲                            │                             │
│           │                            │                             │
│   Deployment(worker) ─────────────────┘                             │
│   (traite les tâches d'indexation)                                   │
└─────────────────────────────────────────────────────────────────┘
```

**`vector-db.yaml`** (base vectorielle avec stockage persistant)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: vector-db
  namespace: rag-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: vector-db
  template:
    metadata:
      labels:
        app: vector-db
    spec:
      containers:
        - name: qdrant
          image: qdrant/qdrant:latest
          ports:
            - containerPort: 6333
          volumeMounts:
            - name: qdrant-storage
              mountPath: /qdrant/storage
      volumes:
        - name: qdrant-storage
          persistentVolumeClaim:
            claimName: qdrant-pvc
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: qdrant-pvc
  namespace: rag-app
spec:
  accessModes: ["ReadWriteOnce"]
  resources:
    requests:
      storage: 10Gi
---
apiVersion: v1
kind: Service
metadata:
  name: vector-db-service
  namespace: rag-app
spec:
  selector:
    app: vector-db
  ports:
    - port: 6333
      targetPort: 6333
```

**`worker.yaml`** (traitement asynchrone, mise à l'échelle indépendante de l'agent)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: worker
  namespace: rag-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: worker
  template:
    metadata:
      labels:
        app: worker
    spec:
      containers:
        - name: worker
          image: ghcr.io/mon-utilisateur/mon-worker:1.0
          env:
            - name: VECTOR_DB_URL
              value: "http://vector-db-service:6333"
            - name: REDIS_URL
              value: "redis://queue-service:6379"
          resources:
            requests:
              cpu: "500m"
              memory: "512Mi"
            limits:
              cpu: "2"
              memory: "1Gi"
```

> 💡 **Pourquoi le worker est un Deployment séparé de l'agent :** cela permet de **mettre à l'échelle indépendamment** les deux composants. Si tu reçois beaucoup de documents à indexer mais peu de requêtes de chat, tu peux scaler `worker` sans toucher à `agent`, et inversement.

### 16.4 HPA spécifique pour l'agent IA

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: agent-ia-hpa
  namespace: rag-app
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: agent-ia
  minReplicas: 2
  maxReplicas: 6
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 60
```

> ⚠️ **Point d'attention spécifique à l'IA :** contrairement à une API classique, le coût d'un agent IA n'est pas seulement le CPU/RAM de ton pod — chaque requête déclenche un appel payant vers l'API du fournisseur LLM. **Scaler automatiquement le nombre de pods n'accélère pas les réponses individuelles de l'API externe** — cela permet seulement de traiter plus de requêtes en parallèle. Pense à définir un budget d'appels et un monitoring des coûts en parallèle du scaling technique.

### 🎯 Exercice pratique n°12

1. Containerise l'agent IA de la Partie 13 de la formation Docker, publie l'image sur un registre
2. Déploie-le sur ton cluster minikube avec un Secret pour la clé API, un Deployment avec probes, et un Service
3. Ajoute un Deployment Redis pour mettre en cache les réponses (comme dans l'exercice de la formation Docker), et modifie ton agent pour l'utiliser
4. Ajoute un HPA et observe le comportement sous charge simulée

---

## Partie 17 — Déployer sur un vrai cloud (production réelle)

### 17.1 Les principaux services Kubernetes managés

Un vrai cluster de production tourne rarement sur une machine que tu administres toi-même — on utilise des services **Kubernetes managés**, où le fournisseur cloud gère le Control Plane pour toi.

| Fournisseur | Service | Particularité |
|---|---|---|
| **Google Cloud** | GKE (Google Kubernetes Engine) | Créateur historique de Kubernetes, souvent considéré comme la référence |
| **Amazon Web Services** | EKS (Elastic Kubernetes Service) | Le plus répandu en entreprise, écosystème AWS très riche |
| **Microsoft Azure** | AKS (Azure Kubernetes Service) | Intégration native avec l'écosystème Microsoft |
| **DigitalOcean** | DOKS | Beaucoup plus simple et abordable, idéal pour apprendre/petits projets |

### 17.2 Ce qui change entre minikube et un cluster cloud

- Le **LoadBalancer** provisionne un vrai équilibreur de charge chez le fournisseur (avec un coût associé)
- Le **StorageClass** provisionne du vrai stockage cloud (disques SSD gérés)
- Tu gères l'authentification via le **kubeconfig** fourni par le fournisseur (`gcloud`, `aws eks update-kubeconfig`, `az aks get-credentials`...)
- Les coûts sont **réels** : chaque nœud, chaque LoadBalancer, chaque volume de stockage est facturé

### 17.3 Étapes générales pour un premier déploiement en production (exemple avec DigitalOcean, pour sa simplicité pédagogique)

```bash
# Installation du CLI DigitalOcean
brew install doctl
doctl auth init

# Création d'un cluster
doctl kubernetes cluster create mon-cluster --count 3 --size s-2vcpu-4gb

# Récupération automatique de la configuration kubectl
doctl kubernetes cluster kubeconfig save mon-cluster

# Vérification
kubectl get nodes
```

À partir de là, **tous les manifests YAML que tu as écrits pendant cette formation fonctionnent tels quels** — c'est toute la puissance de l'approche déclarative de Kubernetes : le même fichier `deployment.yaml` fonctionne sur minikube en local et sur un vrai cluster cloud en production.

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

### 17.4 HTTPS en production avec cert-manager

```bash
# Installe cert-manager, qui automatise l'obtention de certificats Let's Encrypt gratuits
helm repo add jetstack https://charts.jetstack.io
helm install cert-manager jetstack/cert-manager --set installCRDs=true
```

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: ton-email@exemple.com
    privateKeySecretRef:
      name: letsencrypt-key
    solvers:
      - http01:
          ingress:
            class: nginx
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: mon-ingress
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
    - hosts:
        - monapp.com
      secretName: monapp-tls
  rules:
    - host: monapp.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: mon-app-service
                port:
                  number: 80
```

Une fois appliqué, cert-manager obtient et renouvelle **automatiquement** un certificat HTTPS valide pour ton domaine — sans aucune intervention manuelle.

---

## Partie 18 — Projets pratiques

Comme pour Docker, voici une progression complète de projets à réaliser **dans l'ordre**, sans avoir besoin de chercher les consignes ailleurs.

### 🟢 Niveau débutant

**Projet 1 — Premier déploiement complet**
Déploie l'application statique nginx du Projet 1 Docker sur minikube : un Deployment avec 2 réplicas, un Service ClusterIP, testé via `kubectl port-forward`.

**Projet 2 — Exposition via Ingress**
Reprends le Projet 1 et ajoute un Ingress avec un nom de domaine local (`monsite.local`), en configurant ton fichier `hosts`.

**Projet 3 — Application avec base de données**
Déploie une API + PostgreSQL sur Kubernetes : un Deployment pour l'API, un Deployment pour PostgreSQL avec un PVC, un Secret pour le mot de passe, et deux Services distincts.

### 🟡 Niveau intermédiaire

**Projet 4 — Résilience et probes**
Ajoute des `readinessProbe` et `livenessProbe` correctement configurées à ton API du Projet 3. Provoque volontairement un crash de l'application et observe le `CrashLoopBackOff`, puis diagnostique et corrige.

**Projet 5 — Rolling Update et Rollback**
Fais évoluer ton API vers une version 2 (change une réponse d'API par exemple), déploie-la avec un rolling update, observe la transition sans coupure avec `kubectl rollout status`. Puis déploie volontairement une version cassée, et pratique un `rollout undo`.

**Projet 6 — Mise à l'échelle automatique**
Ajoute un HPA à ton API (min 2, max 8 réplicas). Génère de la charge artificielle avec un script simple envoyant des requêtes en boucle, observe le scaling automatique.

**Projet 7 — Packaging avec Helm**
Transforme le Projet 3 complet (API + base de données) en un Chart Helm personnalisé avec un `values.yaml` paramétrable (nombre de réplicas, tag d'image, ressources).

### 🔴 Niveau avancé

**Projet 8 — Sécurisation complète**
Applique la checklist de sécurité complète (Partie 14) au Projet 3 : `securityContext`, NetworkPolicy limitant l'accès à la base de données au seul service API, RBAC basique, Namespace dédié.

**Projet 9 — Pipeline CI/CD complet**
Écris un workflow GitHub Actions qui, à chaque push : construit et publie l'image, puis déploie automatiquement sur ton cluster (minikube en local via un runner self-hosted, ou un vrai cluster cloud) avec `kubectl set image` et vérification du `rollout status`.

**Projet 10 — Stratégie Blue-Green**
Implémente manuellement un déploiement blue-green pour ton API : deux Deployments (`app-bleu` et `app-vert`), un Service dont tu changes le `selector` pour basculer, avec un script de bascule automatisé.

### 🟣 Niveau expert — Intégration IA

**Projet 11 — Agent IA sur Kubernetes**
Déploie l'agent IA (FastAPI + API Claude) de la formation Docker sur ton cluster : Deployment avec probes, Secret pour la clé API, Service, et HPA basé sur le CPU.

**Projet 12 — Stack RAG complète sur Kubernetes**
Déploie l'architecture complète de la Partie 16.3 : agent + base vectorielle (avec PVC) + worker asynchrone + Redis, tous dans un Namespace dédié `rag-app`. Objectif : uploader un document via l'agent, le voir indexé par le worker, puis interroger l'agent qui utilise ce document pour répondre.

**Projet 13 — Portfolio final : production réelle**
Déploie la stack RAG complète sur un vrai cluster cloud (DigitalOcean DOKS recommandé pour son coût réduit), avec : un nom de domaine réel et HTTPS via cert-manager, un pipeline CI/CD GitOps (ArgoCD synchronisant automatiquement depuis ton dépôt Git), un monitoring basique avec la stack Prometheus/Grafana, et des NetworkPolicies limitant strictement les communications entre composants.

> 🏆 **Si tu termines le Projet 13, tu es capable de concevoir, sécuriser, déployer et opérer en production une architecture Kubernetes complète, y compris des systèmes d'IA à l'échelle.** C'est le niveau attendu d'un ingénieur DevOps/Platform Engineer confirmé.

---

## Partie 19 — Glossaire et ressources

### Glossaire rapide

- **Cluster** : un ensemble de machines (nœuds) gérées collectivement par Kubernetes
- **Node (nœud)** : une machine (physique ou virtuelle) faisant partie du cluster
- **Pod** : l'unité de base, une enveloppe contenant un ou plusieurs conteneurs
- **Deployment** : gère un ensemble de pods identiques, leur nombre, et leurs mises à jour
- **Service** : fournit une adresse réseau stable vers un ensemble de pods
- **Ingress** : route le trafic HTTP/HTTPS externe vers différents Services
- **ConfigMap** : stocke de la configuration non sensible
- **Secret** : stocke des données sensibles (mots de passe, clés API)
- **PersistentVolume / PersistentVolumeClaim** : gèrent le stockage persistant
- **Namespace** : cloisonnement logique de ressources au sein d'un cluster
- **HPA (Horizontal Pod Autoscaler)** : ajuste automatiquement le nombre de réplicas
- **Helm** : gestionnaire de paquets pour Kubernetes
- **kubectl** : l'outil en ligne de commande pour piloter un cluster
- **Control Plane** : les composants qui gèrent l'état et les décisions du cluster
- **Rolling Update / Blue-Green / Canary** : stratégies de déploiement sans interruption
- **GitOps** : approche où Git est la source de vérité unique, synchronisée automatiquement vers le cluster

### Ressources officielles pour aller plus loin

- Documentation officielle Kubernetes : [kubernetes.io/docs](https://kubernetes.io/docs/home/)
- Tutoriel interactif officiel : [kubernetes.io/docs/tutorials](https://kubernetes.io/docs/tutorials/)
- Documentation Helm : [helm.sh/docs](https://helm.sh/docs/)
- Documentation ArgoCD (GitOps) : [argo-cd.readthedocs.io](https://argo-cd.readthedocs.io/)
- Documentation Minikube : [minikube.sigs.k8s.io](https://minikube.sigs.k8s.io/docs/)

---

## 🎓 Conclusion

Tu as maintenant parcouru l'intégralité du chemin Kubernetes : de l'architecture interne du cluster jusqu'au déploiement d'une stack RAG complète en production réelle avec HTTPS, GitOps et monitoring, en passant par toutes les stratégies de déploiement professionnelles (rolling update, blue-green, canary), la sécurité, et la mise à l'échelle automatique.

**Combiné à la formation Docker, tu couvres maintenant l'intégralité de la chaîne** : containeriser une application, l'orchestrer à l'échelle, la déployer sans interruption de service, la sécuriser, et l'opérer en production — y compris pour des systèmes d'intelligence artificielle.

**La règle reste la même : fais les 13 projets, dans l'ordre.** Kubernetes se maîtrise par la pratique répétée bien plus que par la lecture — chaque `CrashLoopBackOff` que tu résoudras toi-même construira une compréhension que ce document seul ne peut pas te donner.

Bon courage, et bienvenue dans l'orchestration de conteneurs à l'échelle ☸️
