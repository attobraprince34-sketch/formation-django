# Formation Complète : System Design

> Objectif de cette formation : comprendre en profondeur les bases du system design, être capable de les expliquer/enseigner à quelqu'un d'autre, et pouvoir justifier cette compétence en entretien ou sur un CV.

---

## Sommaire

1. [Qu'est-ce que le System Design ?](#1-quest-ce-que-le-system-design)
2. [Concepts fondamentaux](#2-concepts-fondamentaux)
3. [Architecture Client-Serveur et DNS](#3-architecture-client-serveur-et-dns)
4. [Load Balancing](#4-load-balancing)
5. [Bases de données](#5-bases-de-données)
6. [Caching](#6-caching)
7. [CDN (Content Delivery Network)](#7-cdn-content-delivery-network)
8. [Communication asynchrone et Message Queues](#8-communication-asynchrone-et-message-queues)
9. [Design d'API](#9-design-dapi)
10. [Monolithe vs Microservices](#10-monolithe-vs-microservices)
11. [CAP Theorem et modèles de cohérence](#11-cap-theorem-et-modèles-de-cohérence)
12. [Consistent Hashing](#12-consistent-hashing)
13. [Rate Limiting](#13-rate-limiting)
14. [Fiabilité et haute disponibilité](#14-fiabilité-et-haute-disponibilité)
15. [Observabilité (Monitoring & Logging)](#15-observabilité-monitoring--logging)
16. [Sécurité dans le system design](#16-sécurité-dans-le-system-design)
17. [Méthodologie pour résoudre un exercice de system design](#17-méthodologie-pour-résoudre-un-exercice-de-system-design)
18. [Études de cas pratiques](#18-études-de-cas-pratiques)
19. [Comment présenter cette compétence sur ton CV](#19-comment-présenter-cette-compétence-sur-ton-cv)
20. [Ressources pour aller plus loin](#20-ressources-pour-aller-plus-loin)

---

## 1. Qu'est-ce que le System Design ?

Le **system design** est la discipline qui consiste à concevoir l'architecture d'un système informatique — c'est-à-dire décider comment ses différents composants (serveurs, bases de données, caches, files d'attente, etc.) s'organisent et communiquent pour répondre à un besoin, à une échelle donnée.

Il y a deux niveaux principaux :

- **High-Level Design (HLD)** : la vue d'ensemble — quels services existent, comment ils communiquent, quelles bases de données on utilise, où sont les caches. C'est le niveau qu'on discute en entretien.
- **Low-Level Design (LLD)** : le détail — classes, structures de données, algorithmes, schémas de base de données précis.

Cette formation se concentre sur le **HLD**, car c'est la base pour comprendre tout système, et c'est ce qui est le plus demandé en entretien, y compris pour un premier poste.

**Pourquoi c'est important ?**

- Un bon développeur ne sait pas seulement écrire du code, il sait aussi anticiper : "Que se passe-t-il si j'ai 10 utilisateurs ? Et si j'en ai 10 millions ?"
- C'est une compétence très valorisée en entretien technique, même junior.
- Ça t'aide à faire de meilleurs choix techniques sur tes propres projets (comme CVCraft ou NEXUS ACADEMY).

---

## 2. Concepts fondamentaux

Avant de parler d'architecture, il faut maîtriser le vocabulaire et les métriques qu'on optimise.

### 2.1 Scalabilité (Scalability)

C'est la capacité d'un système à gérer une charge croissante (plus d'utilisateurs, plus de données, plus de requêtes) sans dégrader les performances.

Deux façons de scaler :

- **Scaling vertical (Scale Up)** : ajouter plus de ressources à une seule machine (plus de CPU, plus de RAM). Simple, mais limité physiquement et représente un point de défaillance unique.
- **Scaling horizontal (Scale Out)** : ajouter plus de machines qui travaillent ensemble. Plus complexe à mettre en place (il faut répartir la charge), mais quasi illimité et plus résilient.

> Analogie : le scaling vertical, c'est embaucher un employé plus rapide. Le scaling horizontal, c'est embaucher plusieurs employés qui travaillent en parallèle.

### 2.2 Latence vs Débit (Latency vs Throughput)

- **Latence** : le temps que met une seule requête pour être traitée (ex : 200 ms pour charger une page).
- **Débit (Throughput)** : le nombre de requêtes que le système peut traiter par unité de temps (ex : 5000 requêtes/seconde).

Ces deux métriques sont parfois en tension : optimiser l'une peut dégrader l'autre (ex : traiter les requêtes par lots augmente le débit mais peut augmenter la latence individuelle).

### 2.3 Disponibilité (Availability)

C'est le pourcentage de temps pendant lequel un système est opérationnel. On la mesure souvent en "9" :

| Disponibilité | Downtime/an |
|---|---|
| 99% | ~3,65 jours |
| 99,9% | ~8,76 heures |
| 99,99% | ~52 minutes |
| 99,999% | ~5 minutes |

### 2.4 Fiabilité (Reliability)

C'est la capacité du système à fonctionner correctement, même en cas de panne partielle (une machine tombe, le système continue de fonctionner grâce à la redondance).

### 2.5 Cohérence (Consistency)

Dans un système distribué (plusieurs machines/copies de données), la cohérence désigne le fait que toutes les copies des données reflètent la même valeur au même moment. On y reviendra en détail avec le CAP theorem (section 11).

### 2.6 Tolérance aux pannes (Fault Tolerance) & Redondance

- **Redondance** : dupliquer des composants critiques (serveurs, bases de données) pour qu'en cas de panne d'un composant, un autre prenne le relais.
- **Tolérance aux pannes** : la capacité globale du système à continuer de fonctionner malgré des pannes.

### 2.7 Single Point of Failure (SPOF)

Un composant dont la panne entraîne l'arrêt total du système. Un bon system design cherche à **éliminer les SPOF** grâce à la redondance.

---

## 3. Architecture Client-Serveur et DNS

### 3.1 Le modèle Client-Serveur

C'est la base de quasi tout système web :

- Le **client** (navigateur, app mobile) envoie une requête.
- Le **serveur** la traite et renvoie une réponse.

### 3.2 DNS (Domain Name System)

Le DNS traduit un nom de domaine (ex : `nexus-academy.com`) en adresse IP (ex : `192.0.2.1`), car les machines communiquent via IP, pas via des noms lisibles par l'humain.

Étapes simplifiées :

1. Le navigateur demande "quelle est l'IP de nexus-academy.com ?"
2. La requête passe par plusieurs serveurs DNS (résolveur → serveur racine → serveur TLD `.com` → serveur faisant autorité pour le domaine).
3. L'IP est renvoyée, souvent mise en cache pour accélérer les requêtes futures.

### 3.3 HTTP/HTTPS

- **HTTP** : protocole de communication client-serveur, basé sur des requêtes (GET, POST, PUT, DELETE...) et des réponses (codes 200, 404, 500...).
- **HTTPS** : HTTP sécurisé via TLS/SSL — chiffre les échanges pour éviter l'interception des données (important côté sécurité, ton domaine de prédilection).

---

## 4. Load Balancing

### 4.1 Pourquoi ?

Quand un seul serveur ne suffit plus (trop de trafic), on ajoute plusieurs serveurs identiques. Le **load balancer (répartiteur de charge)** distribue les requêtes entrantes entre eux.

### 4.2 Algorithmes courants

- **Round Robin** : distribue les requêtes à tour de rôle, un serveur après l'autre.
- **Least Connections** : envoie la requête au serveur ayant le moins de connexions actives.
- **IP Hash** : utilise l'IP du client pour toujours l'envoyer vers le même serveur (utile pour garder une session).
- **Weighted Round Robin** : comme round robin, mais certains serveurs plus puissants reçoivent plus de requêtes.

### 4.3 Niveaux de load balancing

- **Layer 4 (transport)** : répartit selon IP/port, plus rapide, moins de logique.
- **Layer 7 (application)** : répartit selon le contenu de la requête (URL, headers, cookies), plus flexible mais plus coûteux en calcul.

### 4.4 Exemples d'outils

Nginx, HAProxy, AWS ELB, Traefik.

---

## 5. Bases de données

### 5.1 SQL vs NoSQL

| | SQL (relationnel) | NoSQL (non relationnel) |
|---|---|---|
| Structure | Tables avec schéma fixe | Documents, clé-valeur, colonnes, graphes |
| Exemples | PostgreSQL, MySQL | MongoDB, Redis, Cassandra, DynamoDB |
| Relations | Fortes (jointures) | Faibles ou dénormalisées |
| Scalabilité | Verticale (historiquement) | Horizontale (nativement conçue pour) |
| Cas d'usage | Données structurées, transactions (banque, e-commerce) | Données massives, non structurées, évolution rapide du schéma |

En tant qu'utilisateur de Flask/Django, tu es probablement déjà à l'aise avec du SQL relationnel (via l'ORM). Comprendre quand basculer vers du NoSQL est une vraie compétence de system design.

### 5.2 Réplication

Copier les données sur plusieurs serveurs :

- **Réplication maître-esclave (Primary-Replica)** : le maître reçoit les écritures, les répliques ne servent que la lecture. Améliore la disponibilité en lecture et réduit la charge sur le maître.
- **Réplication multi-maître** : plusieurs nœuds peuvent recevoir des écritures — plus complexe (gestion des conflits) mais plus résilient.

### 5.3 Partitionnement (Sharding)

Quand une base de données devient trop grosse pour une seule machine, on la découpe en **shards** (fragments), chacun stocké sur une machine différente.

- **Sharding par plage (range-based)** : ex. utilisateurs A-M sur un shard, N-Z sur un autre.
- **Sharding par hachage (hash-based)** : on applique une fonction de hachage à une clé (ex : user_id) pour déterminer le shard — répartit mieux la charge, mais complique les requêtes qui touchent plusieurs shards.

### 5.4 Indexation

Un **index** est une structure de données (souvent un arbre B) qui accélère la recherche dans une table, au prix d'un espace de stockage supplémentaire et d'un ralentissement des écritures (l'index doit être mis à jour).

> Règle empirique : indexe les colonnes utilisées souvent dans les clauses `WHERE`, `JOIN`, `ORDER BY` — mais n'indexe pas tout, sous peine de ralentir les écritures inutilement.

### 5.5 Transactions et propriétés ACID

- **Atomicité** : une transaction est tout ou rien.
- **Cohérence** : la base passe d'un état valide à un autre état valide.
- **Isolation** : les transactions concurrentes ne s'interfèrent pas.
- **Durabilité** : une fois validée, une transaction persiste même en cas de panne.

Les bases NoSQL sacrifient souvent une partie d'ACID au profit de la scalabilité (modèle **BASE** : Basically Available, Soft state, Eventually consistent).

---

## 6. Caching

Le cache stocke temporairement des données fréquemment demandées dans un espace de lecture rapide (souvent en RAM), pour éviter de refaire un calcul coûteux ou une requête base de données à chaque fois.

### 6.1 Où placer le cache ?

- **Côté client** : cache navigateur.
- **CDN** : cache géographiquement distribué (voir section 7).
- **Cache applicatif** : ex. Redis, Memcached, entre l'application et la base de données.
- **Cache base de données** : certains SGBD ont leur propre cache de requêtes.

### 6.2 Stratégies de cache

- **Cache-Aside (Lazy Loading)** : l'application vérifie d'abord le cache ; si absent (cache miss), elle va chercher en base et met à jour le cache. Simple et le plus courant.
- **Write-Through** : chaque écriture va d'abord dans le cache, puis dans la base — garantit la cohérence mais ralentit les écritures.
- **Write-Back (Write-Behind)** : l'écriture se fait dans le cache, puis est propagée à la base de manière asynchrone — rapide, mais risque de perte de données si le cache tombe avant la synchronisation.

### 6.3 Politiques d'éviction

Quand le cache est plein, il faut décider quoi supprimer :

- **LRU (Least Recently Used)** : supprime l'élément le moins récemment utilisé.
- **LFU (Least Frequently Used)** : supprime l'élément le moins souvent utilisé.
- **FIFO** : supprime le plus ancien élément ajouté.

### 6.4 Risques

- **Cache invalidation** (l'un des problèmes les plus difficiles en informatique) : savoir quand une donnée en cache devient obsolète.
- **Cache stampede** : quand un cache expire et que des milliers de requêtes arrivent en même temps sur la base — on peut mitiger avec un verrou ou un réchauffement progressif.

---

## 7. CDN (Content Delivery Network)

Un CDN est un réseau de serveurs répartis géographiquement qui stockent des copies de contenu statique (images, vidéos, CSS, JS) proches des utilisateurs.

**Avantage** : réduit la latence (le contenu vient d'un serveur proche de l'utilisateur, pas du serveur d'origine à l'autre bout du monde) et réduit la charge sur le serveur principal.

Exemples : Cloudflare, Akamai, AWS CloudFront.

Pertinent pour NEXUS ACADEMY si le contenu (vidéos de cours, PDF) doit être livré rapidement à des utilisateurs dans différents pays francophones.

---

## 8. Communication asynchrone et Message Queues

### 8.1 Pourquoi de l'asynchrone ?

Dans une communication **synchrone**, le client attend la réponse du serveur avant de continuer. Pour des tâches longues (envoi d'email, traitement vidéo, génération de PDF), c'est inefficace : on préfère répondre immédiatement au client ("traitement en cours") et traiter la tâche en arrière-plan.

### 8.2 Message Queue (file d'attente de messages)

Un **producteur** (producer) envoie un message dans une file, et un ou plusieurs **consommateurs** (consumers) le traitent quand ils sont disponibles.

Avantages :
- Découple les composants (le producteur n'a pas besoin d'attendre le consommateur).
- Absorbe les pics de charge (buffer).
- Permet de réessayer en cas d'échec.

Exemples d'outils : RabbitMQ, Apache Kafka, AWS SQS, Celery (souvent utilisé avec Flask/Django, ce qui te concerne directement).

### 8.3 Pub/Sub (Publish-Subscribe)

Un modèle où un message publié sur un "topic" est reçu par tous les abonnés à ce topic — utile pour diffuser un événement à plusieurs services en même temps (ex : "nouvelle inscription" déclenche l'envoi d'un email ET la mise à jour d'un tableau de bord analytics).

---

## 9. Design d'API

### 9.1 REST

Style architectural basé sur les ressources, manipulées via les méthodes HTTP standards (GET, POST, PUT, PATCH, DELETE), avec des URLs représentant des ressources (`/users/42`).

Principes clés :
- **Stateless** : chaque requête contient toute l'information nécessaire, le serveur ne garde pas d'état entre les requêtes.
- Utilisation cohérente des codes HTTP (200, 201, 400, 401, 403, 404, 500...).

### 9.2 GraphQL

Le client décrit précisément les données dont il a besoin dans une seule requête, évitant l'**over-fetching** (recevoir trop de données) et l'**under-fetching** (devoir faire plusieurs requêtes pour assembler les données). Plus flexible que REST, mais plus complexe à mettre en cache côté serveur.

### 9.3 gRPC

Utilise Protocol Buffers (format binaire) plutôt que JSON, et HTTP/2. Très performant, souvent utilisé pour la communication **interne** entre microservices plutôt que pour des API publiques.

### 9.4 Versionnement d'API

Pour éviter de casser les clients existants quand l'API évolue : `/api/v1/users`, `/api/v2/users`, ou versionnement via headers.

---

## 10. Monolithe vs Microservices

### 10.1 Architecture monolithique

Toute l'application (interface, logique métier, accès aux données) est un seul bloc de code, déployé comme une seule unité.

**Avantages** : simple à développer, tester et déployer au début ; pas de complexité réseau interne.
**Inconvénients** : difficile à scaler sélectivement (on doit dupliquer tout le bloc même si seule une partie est sollicitée) ; une panne dans un module peut affecter tout le système ; le code devient difficile à maintenir quand il grossit.

### 10.2 Microservices

L'application est découpée en petits services indépendants, chacun responsable d'une fonctionnalité métier précise (ex : service utilisateurs, service paiement, service notifications), communiquant via API ou messages.

**Avantages** : chaque service peut être développé, déployé et scalé indépendamment ; une panne est isolée à un service ; équipes différentes peuvent travailler sur des services différents.
**Inconvénients** : complexité opérationnelle (déploiement, monitoring, communication réseau) ; gestion de la cohérence des données entre services plus difficile ; nécessite une bonne maturité DevOps.

> Conseil pratique : pour un projet personnel ou un premier poste, on démarre presque toujours en monolithe. Les microservices se justifient quand l'équipe et l'échelle grandissent.

---

## 11. CAP Theorem et modèles de cohérence

### 11.1 Le théorème CAP

Dans un système distribué, on ne peut garantir simultanément que **deux** des trois propriétés suivantes en cas de partition réseau :

- **Consistency (Cohérence)** : toutes les lectures renvoient la donnée la plus récente.
- **Availability (Disponibilité)** : chaque requête reçoit une réponse (pas d'erreur), même si elle n'est pas la donnée la plus récente.
- **Partition Tolerance (Tolérance au partitionnement)** : le système continue de fonctionner malgré une coupure de communication entre nœuds.

Puisqu'en pratique les partitions réseau **arrivent forcément**, le vrai choix se fait entre **CP** (cohérence + tolérance au partitionnement, on sacrifie la disponibilité) et **AP** (disponibilité + tolérance au partitionnement, on sacrifie la cohérence stricte).

- Exemple **CP** : une base bancaire où il vaut mieux refuser une opération que d'afficher un solde incorrect.
- Exemple **AP** : un réseau social où il vaut mieux afficher un like en retard plutôt que de bloquer l'utilisateur.

### 11.2 Cohérence forte vs cohérence à terme (Eventual Consistency)

- **Cohérence forte** : toute lecture après une écriture renvoie immédiatement la nouvelle valeur, partout.
- **Cohérence à terme** : les répliques finiront par converger vers la même valeur, mais pas instantanément — acceptable pour beaucoup de cas d'usage (compteurs de vues, réseaux sociaux) et permet une meilleure disponibilité/performance.

---

## 12. Consistent Hashing

Problème : quand on répartit des données sur N serveurs avec un simple `hash(clé) % N`, ajouter ou retirer un serveur oblige à redistribuer **presque toutes** les données (car N change).

**Consistent Hashing** résout ça en plaçant serveurs et clés sur un anneau circulaire de hachage. Quand un serveur est ajouté ou retiré, seule une petite portion des clés doit être redistribuée (celles situées juste avant lui sur l'anneau), pas l'ensemble.

Utilisé dans les bases de données distribuées (Cassandra, DynamoDB) et les caches distribués.

---

## 13. Rate Limiting

Le **rate limiting** (limitation de débit) protège un système contre une surcharge (volontaire — attaque — ou non) en limitant le nombre de requêtes qu'un client peut faire sur une période donnée.

Algorithmes courants :

- **Token Bucket** : un "seau" se remplit de jetons à intervalle régulier ; chaque requête consomme un jeton ; si le seau est vide, la requête est rejetée. Permet des pics courts (burst).
- **Leaky Bucket** : les requêtes sont traitées à un débit constant, comme l'eau qui s'écoule d'un seau percé ; lisse le trafic mais n'autorise pas de pics.
- **Fixed Window Counter** : compte les requêtes dans une fenêtre de temps fixe (ex : 100 requêtes/minute) — simple, mais peut laisser passer un double du quota à la frontière de deux fenêtres.
- **Sliding Window Log/Counter** : plus précis, évite le problème de la fenêtre fixe en glissant la fenêtre de comptage en continu.

C'est un sujet à la croisée du system design et de la sécurité (protection contre le brute-force, le scraping, le DDoS applicatif) — pertinent pour ton profil orienté cybersécurité.

---

## 14. Fiabilité et haute disponibilité

### 14.1 Redondance

Dupliquer les composants critiques (serveurs, bases de données, zones géographiques) pour éliminer les SPOF.

### 14.2 Failover

Bascule automatique vers un composant de secours quand le composant principal tombe en panne (ex : une base de données réplique devient le nouveau maître).

### 14.3 Health Checks

Des vérifications périodiques (souvent par le load balancer) pour détecter si un serveur est fonctionnel, et le retirer automatiquement de la rotation s'il ne répond plus correctement.

### 14.4 Circuit Breaker

Un mécanisme qui "coupe le circuit" vers un service défaillant après un certain nombre d'échecs, pour éviter de continuer à envoyer du trafic vers un service en panne (et éviter l'effet domino de pannes en cascade). Après un délai, il teste à nouveau si le service est revenu.

### 14.5 Retry avec Backoff exponentiel

Quand une requête échoue, on la réessaie, mais en augmentant progressivement le délai entre chaque tentative (ex : 1s, 2s, 4s, 8s...) pour ne pas surcharger un service déjà en difficulté.

---

## 15. Observabilité (Monitoring & Logging)

Un système bien conçu doit permettre de comprendre ce qu'il se passe en production :

- **Logging** : enregistrer les événements (erreurs, requêtes, actions importantes) pour pouvoir les analyser après coup.
- **Metrics (métriques)** : mesures numériques dans le temps (latence, taux d'erreur, nombre de requêtes) — souvent visualisées avec des outils comme Grafana/Prometheus.
- **Alerting** : déclencher une alerte automatique (email, SMS, Slack) quand une métrique dépasse un seuil critique.
- **Tracing distribué** : suivre une requête à travers plusieurs services (utile surtout en microservices) pour identifier où se situe un ralentissement ou une erreur.

---

## 16. Sécurité dans le system design

Vu ton focus cybersécurité, voici les points de jonction essentiels entre system design et sécurité :

- **Authentification vs Autorisation** : authentification = vérifier qui est l'utilisateur (login) ; autorisation = vérifier ce qu'il a le droit de faire.
- **Chiffrement en transit et au repos** : HTTPS/TLS pour les données en mouvement, chiffrement de la base de données pour les données stockées.
- **Rate limiting** (vu section 13) contre le brute-force et le DDoS applicatif.
- **Principe du moindre privilège** : chaque composant (service, utilisateur, clé API) ne doit avoir accès qu'à ce dont il a strictement besoin.
- **Défense en profondeur** : plusieurs couches de sécurité (WAF, validation des entrées, pare-feu réseau, authentification) plutôt qu'un seul point de protection.
- **Segmentation réseau** : isoler les composants sensibles (base de données) dans un réseau privé, non directement accessible depuis internet.

---

## 17. Méthodologie pour résoudre un exercice de system design

Voici une méthode en 5 étapes, utile aussi bien en entretien que pour enseigner à quelqu'un d'autre :

1. **Clarifier les exigences** : Quelles fonctionnalités sont indispensables ? Combien d'utilisateurs ? Lecture ou écriture intensive ? Cohérence stricte nécessaire ou pas ?
2. **Estimer l'échelle** : Nombre d'utilisateurs, requêtes par seconde, volume de stockage (calculs approximatifs, dits "back-of-the-envelope").
3. **Concevoir l'architecture générale (HLD)** : dessiner les composants principaux (client, serveur d'API, base de données, cache, load balancer) et leurs interactions.
4. **Approfondir les composants critiques** : par exemple, comment fonctionne le sharding de la base de données, ou l'algorithme de génération d'identifiants uniques.
5. **Discuter des compromis (trade-offs)** : chaque choix a un coût. Il n'y a pas de solution "parfaite", seulement des compromis adaptés au contexte (ex : cohérence forte vs disponibilité).

> Point clé à transmettre en enseignant : le system design n'a pas UNE bonne réponse. L'important est la capacité à justifier ses choix et leurs compromis.

---

## 18. Études de cas pratiques

### 18.1 Raccourcisseur d'URL (type bit.ly)

- **Besoin** : transformer une URL longue en URL courte, et rediriger vers l'originale.
- **Génération de l'ID court** : hachage de l'URL (encodage en base62) ou compteur incrémental encodé en base62.
- **Stockage** : base clé-valeur (ex : Redis ou DynamoDB) — lecture très fréquente (redirection), écriture rare (création de lien).
- **Cache** : les liens populaires sont mis en cache pour des redirections ultra-rapides.
- **Scalabilité** : la lecture domine largement l'écriture → on peut répliquer massivement en lecture.

### 18.2 Fil d'actualité (type Twitter/X)

- **Besoin** : afficher les posts des comptes suivis, dans l'ordre chronologique ou par pertinence.
- **Fan-out on write** : quand un utilisateur publie, on pré-calcule et distribue le post dans le fil de tous ses followers (rapide en lecture, coûteux en écriture — adapté si peu de followers).
- **Fan-out on read** : le fil est calculé à la demande, au moment où l'utilisateur consulte son feed (adapté pour les comptes ayant énormément de followers, pour éviter des millions d'écritures à chaque post).
- Beaucoup de systèmes réels combinent les deux approches selon le nombre de followers du compte.

### 18.3 Système de messagerie instantanée (type WhatsApp)

- **WebSockets** pour une communication bidirectionnelle en temps réel (plutôt que du HTTP classique).
- **Message Queue** pour garantir la livraison même si le destinataire est hors ligne (stockage temporaire du message jusqu'à sa remise).
- **Accusés de réception** (delivered/read) nécessitant un suivi d'état par message.

### 18.4 Plateforme e-learning (pertinent pour NEXUS ACADEMY)

- **Contenu statique** (vidéos, PDF de cours) → stockage objet (type S3) + CDN pour une diffusion rapide dans toute la zone francophone.
- **Base de données relationnelle** pour les utilisateurs, inscriptions, progressions de cours (données structurées, cohérence importante — ex : un paiement doit être fiable).
- **Cache** pour les pages de contenu de cours très consultées (listes de cours, descriptions).
- **File d'attente asynchrone** pour l'envoi d'emails (confirmation d'inscription, rappels) sans bloquer la réponse à l'utilisateur.
- **Scalabilité progressive** : commencer en architecture monolithique (Django/Flask), migrer certains modules en services séparés seulement si un besoin réel apparaît (ex : un module de traitement vidéo devient un service à part).

---

## 19. Comment présenter cette compétence sur ton CV

Quelques principes pour que ce soit crédible et bien perçu par un recruteur :

- **Ne mets pas juste "System Design"** comme mot-clé isolé — un recruteur ou un développeur senior peut te tester dessus en entretien. Assure-toi d'être capable d'expliquer les concepts de cette formation à l'oral.
- **Relie-le à un projet concret** : par exemple, dans la description de CVCraft ou NEXUS ACADEMY, mentionne les choix d'architecture que tu as faits (ex : "mise en place d'un cache pour réduire la charge sur la base de données", "conception d'une architecture modulaire facilitant l'ajout de nouveaux services").
- **Formulation possible dans une section Compétences** : *"Conception de systèmes : scalabilité, bases de données SQL/NoSQL, caching, API REST, principes de haute disponibilité"* — plus précis et crédible que juste "System Design".
- **Prépare-toi à un exercice pratique** : beaucoup d'entretiens, même juniors, incluent une question simplifiée de system design (ex : "comment concevrais-tu un système de like sur un post ?"). Entraîne-toi à appliquer la méthodologie de la section 17 à voix haute.

---

## 20. Ressources pour aller plus loin

Une fois cette base assimilée, voici des pistes pour approfondir (à explorer par toi-même) :

- Pratiquer des exercices de system design sur des plateformes dédiées à l'entraînement à l'entretien technique.
- Lire la documentation officielle des outils cités (Redis, Nginx, Kafka, PostgreSQL) pour voir comment les concepts théoriques s'implémentent concrètement.
- Étudier les articles d'ingénierie publiés par les grandes entreprises tech (souvent disponibles publiquement sur leurs blogs d'ingénierie), qui détaillent comment elles ont résolu des problèmes réels de scalabilité.
- Refaire les études de cas de la section 18 par toi-même, sans regarder la solution, puis comparer ta démarche.

---

## Comment utiliser ce document pour enseigner

Pour transmettre cette formation à quelqu'un d'autre efficacement :

1. Commence toujours par le **vocabulaire de base** (section 2) — sans lui, tout le reste est abstrait.
2. Utilise des **analogies simples** (comme celles proposées dans ce document) avant de plonger dans les détails techniques.
3. Illustre chaque concept avec un **exemple concret** issu des études de cas (section 18) plutôt que de rester dans la théorie pure.
4. Termine toujours par la **question des compromis** ("pourquoi pas l'inverse ?") — c'est ce qui distingue une vraie compréhension d'une simple mémorisation.
