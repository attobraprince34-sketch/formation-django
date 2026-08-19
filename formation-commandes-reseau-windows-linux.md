# Formation Complète : Commandes Réseau pour Windows et Linux

> De débutant à administrateur réseau autonome : comprendre, diagnostiquer et configurer le réseau 
> depuis le terminal — Windows PowerShell, Bash, et les outils cross-platform.
>
> **Public visé** : débutant en networking, administrateur système junior, DevOps en formation.  
> **Prérequis** : connaître les bases du terminal (navigation dossiers, exécuter des commandes)

---

## 📚 Table des matières

1. [Avant de commencer](#avant-de-commencer)
2. [Module 0 — Fondamentaux réseau](#module-0--fondamentaux-réseau)
3. [Module 1 — Diagnostiquer la connexion réseau](#module-1--diagnostiquer-la-connexion-réseau)
4. [Module 2 — Résolution de noms (DNS)](#module-2--résolution-de-noms-dns)
5. [Module 3 — Ports, services et connexions](#module-3--ports-services-et-connexions)
6. [Module 4 — Configuration réseau (adressage, interfaces)](#module-4--configuration-réseau-adressage-interfaces)
7. [Module 5 — Routage et tracés (Route, Tracert/Traceroute)](#module-5--routage-et-tracés-route-tracerttraceroute)
8. [Module 6 — Firewall et filtrage](#module-6--firewall-et-filtrage)
9. [Module 7 — Monitoring réseau en temps réel](#module-7--monitoring-réseau-en-temps-réel)
10. [Module 8 — Transfert de fichiers et tunnel](#module-8--transfert-de-fichiers-et-tunnel)
11. [Module 9 — Diagnostic avancé (Packet capture, Wireshark)](#module-9--diagnostic-avancé-packet-capture-wireshark)
12. [Module 10 — Scripts réseau (Automation)](#module-10--scripts-réseau-automation)
13. [Projets pratiques](#projets-pratiques)
14. [Ressources](#ressources)

---

## 🎯 Comment utiliser cette formation

Cette formation suit le même principe que l'enseignement en présentiel : chaque concept est expliqué **avant** d'être utilisé, jamais l'inverse.

**Règles d'or pour bien apprendre** :

1. **Exécute chaque commande toi-même.** Ne copie-colle jamais sans comprendre pourquoi ça marche. Tes doigts doivent apprendre autant que ta tête.
2. **Casse volontairement tes commandes.** Oublie un flag, change un paramètre, vois l'erreur apparaître. Les erreurs sont tes professeurs.
3. **Comprendre la différence Windows/Linux** : ils font exactement la même chose, mais avec une syntaxe différente. Ce n'est pas une limite, c'est une force.
4. **Fais les exercices avant de lire la correction.** Même si tu bloques 30 minutes, c'est normal.
5. **À la fin de chaque module, teste sur TA machine réelle.** Lance une commande que tu as apprise sur ton propre réseau pour voir ce que tu maîtrises vraiment.

> **Règle d'or** : ne passe jamais au module suivant si tu ne peux pas expliquer le précédent à voix haute, sans notes. Si tu bloques, reviens en arrière — c'est normal et c'est même sain.

**Outil recommandé** : 
- Sur Windows : Terminal Windows ou PowerShell 7+ (plus moderne que PowerShell 5.1)
- Sur Linux : Terminal GNOME, Konsole, ou n'importe quel terminal — ils font tous la même chose

---

## Avant de commencer

### Ce dont tu as besoin

- Windows 10+ ou une distribution Linux (Ubuntu, CentOS, Debian...)
- Accès au terminal/PowerShell/Bash
- **Une machine de test** — ne lance pas ces commandes sur la machine de ta grand-mère ! 😉
- Connexion internet (pour tester les commandes vers l'extérieur)

### Qu'est-ce que tu vas apprendre

À la fin de cette formation, tu pourras :

✅ **Diagnostiquer** une connexion réseau défaillante (ping, trace, DNS)  
✅ **Configurer** une machine avec une IP fixe ou DHCP  
✅ **Tester** si un serveur est accessible (ports, services)  
✅ **Monitorer** le trafic réseau en temps réel  
✅ **Dépanner** des problèmes réseau complexes (perte de paquets, latence)  
✅ **Automatiser** des tâches réseau avec des scripts  
✅ **Analyser** le trafic au niveau des paquets  

---

## Module 0 — Fondamentaux Réseau (1-2 jours)

### 0.1 Les 7 couches OSI — ce que tu dois savoir

Ne te mets pas en tête de mémoriser tout. L'important, c'est de **savoir où chercher quand quelque chose ne marche pas**.

| Couche | Nom | Protocoles | Ce qui se passe | Nos commandes |
|--------|-----|-----------|------------------|---|
| 7 | **Application** | HTTP, SMTP, SSH, DNS, FTP | "Je veux accéder à google.com" | `nslookup`, `curl`, `ssh` |
| 6 | Présentation | Compression, Chiffrement | "Formatons les données" | (Rare) |
| 5 | Session | TCP/UDP sessions | "Établissons une connexion" | (Géré automatiquement) |
| 4 | **Transport** | TCP, UDP | "Comment envoyer les données ?" | `netstat`, `ss`, `lsof` |
| 3 | **Réseau** | IP, ICMP, ARP | "Par où va mon paquet ?" | `ping`, `route`, `tracert` |
| 2 | Liaison | Ethernet, MAC | "Vers quelle machine sur le LAN ?" | `arp`, `ipconfig` |
| 1 | Physique | Câbles, ondes Wi-Fi | Les vrais câbles, les vrais signaux | (Pas de commande) |

**Pourquoi c'est utile** : quand ton internet plante, tu dois savoir **à quelle couche** chercher. Exemple :

- Pas d'internet du tout → Couche 1 (câble débranché ?) ou Couche 2 (MAC ?)
- Connexion à ta box OK, mais pas à Google → Couche 3 (Routage) ou Couche 7 (DNS)
- Connexion lente → Couche 3 ou 4 (paquets perdus)

### 0.2 Adresse IP, masque de sous-réseau, gateway

Trois concepts clés, vraiment faciles à comprendre si tu les visualises bien.

**L'adresse IP** : c'est l'adresse de ta machine. Elle a 4 nombres (IPv4) ou 8 groupes (IPv6).

```
192.168.1.100   ← C'est MOI sur le réseau
```

**Le masque de sous-réseau** : c'est un filtre qui dit "tout ce qui match jusqu'ici est sur MON réseau local, le reste est loin".

```
Masque : 255.255.255.0

En binaire :
255.255.255.0 = 11111111.11111111.11111111.00000000
              = "Les 3 premiers nombres doivent être identiques"

Donc je peux parler directement à :
192.168.1.1
192.168.1.2
... jusqu'à
192.168.1.254

Mais 192.168.2.1 ? C'est trop loin, faut passer par la gateway.
```

**La gateway (passerelle)** : c'est la porte vers l'extérieur. Généralement, c'est ton routeur.

```
192.168.1.100    ← MOI
192.168.1.1      ← MA GATEWAY (le routeur)
8.8.8.8          ← Google (loin, faut passer par la gateway)
```

**Notation CIDR** : les gens modernes écrivent ça comme ça :

```
192.168.1.100/24

Le /24 signifie "les 24 premiers bits sont le réseau"
= 255.255.255.0
= "Le réseau est 192.168.1.0/24"
```

**Résumé en un tableau** :

| Concept | Valeur | Signification |
|---------|--------|---------------|
| Adresse IP | 192.168.1.100 | C'est MOI |
| Masque | 255.255.255.0 ou /24 | Mon réseau local est les 192.168.1.* |
| Gateway | 192.168.1.1 | C'est mon routeur |
| Réseau | 192.168.1.0/24 | L'ensemble des adresses possibles |

### 0.3 TCP vs UDP vs ICMP — tu as besoin des trois

Trois façons d'envoyer des données. Aucune n'est "meilleure", c'est selon le cas d'usage.

| Protocole | Orienté connexion | Fiabilité | Vitesse | Cas d'usage |
|-----------|-----------------|-----------|---------|---------|
| **TCP** | ✅ Oui | ✅ Garantie (aucun paquet perdu) | 🐢 Plus lent | Web (HTTP), SSH, Email, Base de données |
| **UDP** | ❌ Non | ❌ Pas de garantie | 🚀 Rapide | Streaming vidéo/audio, DNS, Jeux online, VoIP |
| **ICMP** | N/A | Signalisation | N/A | `ping`, `traceroute` (diagnostic) |

**Pourquoi la différence ?**

**TCP** (Transmission Control Protocol) :
- Avant d'envoyer les données, établit une connexion ("Salut, t'es là ?")
- Numérote chaque paquet
- Si un paquet est perdu, le renvoie
- **Lent, mais sûr**
- "Je veux que TOUS mes données arrivent, même si c'est lent"

**UDP** (User Datagram Protocol) :
- Envoie les données directement, sans demander permission
- Pas de numérotation, pas de resend
- **Rapide, mais risqué**
- "Je préfère la rapidité, même si un paquet par-ci par-là se perd"

**Exemple concret** :
- Regarder une vidéo YouTube en UDP : tu perds un paquet → tu vois un petit glitch d'une demi-seconde, tant pis
- Télécharger un fichier en TCP : tu perds un paquet → le serveur renvoie JUSTE ce paquet, puis continue

---

## Module 1 — Diagnostiquer la Connexion Réseau (2-3 jours)

### 1.1 Voir ta configuration IP actuelle

C'est ta première commande. À chaque fois que quelque chose ne marche pas, tu commences par là.

#### Sur Windows

```powershell
# Voir toutes les interfaces réseau
ipconfig

# Résultat typique :
# 
# Adaptateur Ethernet :
#    Adresse IPv4............. : 192.168.1.100
#    Masque de sous-réseau.... : 255.255.255.0
#    Passerelle par défaut.... : 192.168.1.1
#
# Adaptateur Loopback :
#    Adresse IPv4............. : 127.0.0.1
```

```powershell
# Version TRÈS détaillée (includes MAC, DNS, tout)
ipconfig /all

# Résultat typique :
#
# Adaptateur Ethernet :
#    Adresse MAC.............. : 00-1A-2B-3C-4D-5E
#    Adresse IPv4............. : 192.168.1.100
#    Serveurs DNS............ : 8.8.8.8
#                                8.8.4.4
#    Serveur DHCP............ : 192.168.1.1
#    Bail obtenu............. : jeudi 19 août 2026 10:00:00
#    Bail expirant........... : vendredi 26 août 2026 10:00:00
```

```powershell
# Voir juste l'IPv4
ipconfig | findstr "IPv4"

# Voir juste une interface spécifique
ipconfig | findstr "Ethernet" -A 5
```

#### Sur Linux/Mac

```bash
# Voir toutes les interfaces (moderne)
ip addr show

# Résultat :
# 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536
#    inet 127.0.0.1/8 scope host lo
#
# 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
#    inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0
#    inet6 fe80::1a2b:3c4d:5e6f:7890/64 scope link
```

```bash
# L'ancienne manière (mais ça marche encore)
ifconfig

# Résultat :
# eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>
#      inet 192.168.1.100  netmask 255.255.255.0  broadcast 192.168.1.255
#      inet6 fe80::1a2b:3c4d:5e6f:7890/64  prefixlen 64  scopeid 0x20<link>
```

```bash
# Voir statistiques détaillées
ip -s link

# Résultat :
# RX: bytes packets errors dropped
# TX: bytes packets errors dropped
```

### 1.2 Sous le capot — ce qui se passe vraiment

Quand tu tapes `ipconfig`, ton système :

1. Va regarder dans le registre Windows (ou `/proc/net/` sur Linux)
2. Lit la configuration de chaque interface réseau
3. Affiche DHCP si c'est activé ou IP fixe

**Sur Windows**, il y a plein d'interfaces réseau même si tu n'es connecté qu'à une seule chose. Pourquoi ?

- `Ethernet` : ta vraie connexion
- `Loopback` : pour parler à toi-même (toujours là, 127.0.0.1)
- `Hyper-V` : si tu as les vms activées
- `VirtualBox` : si tu utilises VirtualBox
- etc.

Tu n'as pas besoin de les configurer tous. Fouille pas où c'est pas nécessaire.

### 1.3 Pièges classiques

⚠️ **Piège 1** : Avoir plusieurs IP sur la même interface
```
Si tu vois 2 "Adresse IPv4" pour eth0, c'est rare, mais ça arrive.
La première est généralement l'IP "active".
```

⚠️ **Piège 2** : IPv6 vs IPv4
```
Les modernes ont les deux : fe80::1 (IPv6) et 192.168.1.1 (IPv4)
IPv6 commence avec "fe80::" généralement.
Pour cette formation, on oublie IPv6. 😉
```

⚠️ **Piège 3** : DHCP pas obtenu
```
Sur Windows :
"Adresse IPv4............ : 169.254.x.x"  ← C'est MAUVAIS. DHCP a échoué.

Sur Linux :
"inet 169.254.x.x/16"  ← Idem, DHCP a échoué.
```

---

### 1.4 Tester la connectivité basique : PING

`ping` est ta deuxième commande préférée. C'est le "T'es là ?" du réseau.

**Qu'est-ce que ping fait** :
1. Envoie un petit paquet (ICMP Echo Request)
2. Attend une réponse (ICMP Echo Reply)
3. Mesure le temps aller-retour

#### Sur Windows

```powershell
# Ping simple
ping google.com

# Résultat :
# Envoi d'une requête 'ping' sur google.com [142.250.80.46]
# Réponse de 142.250.80.46 : octets=32 temps=25ms TTL=57
# Réponse de 142.250.80.46 : octets=32 temps=24ms TTL=57
# Réponse de 142.250.80.46 : octets=32 temps=26ms TTL=57
# Réponse de 142.250.80.46 : octets=32 temps=25ms TTL=57
# 
# Statistiques Ping pour 142.250.80.46:
#   Paquets : envoyés = 4, reçus = 4, perdus = 0 (0% de perte)
```

**Interpréter les résultats** :

- `Réponse` = ✅ Accessible
- `Délai d'expiration dépassé` = ❌ Pas de réponse (firewall ?)
- `Impossible de résoudre le nom` = ❌ DNS ne marche pas
- `Perte 0%` = ✅ Pas de paquets perdus
- `TTL=57` = paquet a traversé ~7 routeurs (64-57=7)

```powershell
# Envoyer PLUS de paquets pour mieux voir
ping -n 20 google.com

# Envoyer MOINS de paquets (1 au lieu de 4)
ping -n 1 google.com

# Ping continu jusqu'à Ctrl+C
ping -t google.com
```

#### Sur Linux/Mac

```bash
# Ping simple
ping google.com

# Résultat :
# PING google.com (142.250.80.46) 56(84) bytes of data.
# 64 bytes from 142.250.80.46 (icmp_seq=1 time=25.2 ms
# 64 bytes from 142.250.80.46 (icmp_seq=2 time=24.8 ms
# 64 bytes from 142.250.80.46 (icmp_seq=3 time=25.5 ms
# 64 bytes from 142.250.80.46 (icmp_seq=4 time=26.1 ms
# --- google.com statistics ---
# 4 packets transmitted, 4 received, 0% packet loss, time 3001ms
```

```bash
# Envoyer exactement 5 paquets
ping -c 5 google.com

# Envoyer 1 paquet et arrête (utile en script)
ping -c 1 google.com

# Interval de 0.2 secondes entre les paquets (par défaut c'est 1s)
ping -i 0.2 google.com

# Timeout : si pas de réponse après 1 seconde, arrête
ping -W 1 -c 1 google.com
```

### 1.5 Sous le capot — comment fonctionne ping

1. Crée un paquet ICMP de type "Echo Request" (type 8)
2. Ajoute un timestamp "j'ai envoyé ça maintenant"
3. Envoie au destinataire
4. Destinataire répond avec "Echo Reply" (type 0), renvoie le timestamp
5. Ton ordi calcule "maintenant - timestamp" = temps aller-retour
6. Affiche le résultat

**TTL (Time To Live)** : Chaque routeur décrémente le TTL de 1. Si TTL atteint 0, le paquet est jeté.

```
TTL=64 au départ
    ↓ Routeur 1, TTL=63
    ↓ Routeur 2, TTL=62
    ↓ ...
    ↓ Routeur 7, TTL=57 (ça arrive chez Google)
    ↓ Routeur 8 répondrait avec TTL=56 si google répond
```

### 1.6 Pièges avec ping

⚠️ **Piège 1** : Firewall bloque ping
```
Certains firewalls disent "non, pas de ping ici"
Résultat : "Délai d'expiration dépassé"
Ça ne veut pas dire "serveur en panne", juste "pas le droit"
```

⚠️ **Piège 2** : Très haute latence
```
> 100ms = vraiment loin ou très saturé
```

⚠️ **Piège 3** : Perte de paquets intermittente
```
"0% de perte" une fois, puis "25% de perte" la fois d'après
= problème de réseau instable
```

### 1.7 Exercice

Ouvre ton terminal. Ping :
1. `ping google.com` — serveur lointain
2. `ping 8.8.8.8` — même serveur, mais par IP
3. `ping 192.168.1.1` — ton routeur local
4. `ping 127.0.0.1` — toi-même (loopback)

**Résultats attendus** :
- Les 3 premiers : latence, 0% perte
- Le dernier : 0-1ms de latence

Essaie avec `ping -c 1` (Linux) ou `ping -n 1` (Windows) pour économiser ton temps.

---

### 1.8 Tracer la route jusqu'à une destination : TRACERT/TRACEROUTE

`ping` te dit juste "c'est accessible". `tracert` te dit **par où** ça passe.

#### Sur Windows

```powershell
# Tracer la route vers Google
tracert google.com

# Résultat :
# Détermination de l'itinéraire vers google.com [142.250.80.46]
# avec un maximum de 30 sauts :
# 
#   1    <1 ms     1 ms    <1 ms  192.168.1.1          [TA GATEWAY]
#   2    15 ms    12 ms    14 ms  10.0.0.1             [ISP Router 1]
#   3    20 ms    19 ms    21 ms  195.154.1.1          [ISP Router 2]
#   4    22 ms    21 ms    23 ms  195.154.50.1         [Backbone]
#   5    25 ms    24 ms    26 ms  142.250.1.1          [Google border]
#   6    25 ms    24 ms    26 ms  142.250.80.46        [Google]
#
# Itinéraire établi.
```

```powershell
# Avec maximum 15 sauts (par défaut c'est 30)
tracert -h 15 google.com

# Avec port spécifique (TCP)
tracert -h 30 -p 80 google.com
```

#### Sur Linux/Mac

```bash
# Tracer la route (standard)
traceroute google.com

# Résultat :
# traceroute to google.com (142.250.80.46), 30 hops max, 60 byte packets
#  1  192.168.1.1 (192.168.1.1)  1.234 ms  1.111 ms  1.098 ms
#  2  10.0.0.1 (10.0.0.1)       15.432 ms  12.211 ms  14.098 ms
#  3  195.154.1.1 (195.154.1.1)  20.123 ms  19.211 ms  21.098 ms
#  4  * * *
#  5  * * *
#  6  142.250.1.1 (142.250.1.1)  25.432 ms  24.211 ms  26.098 ms
#  7  142.250.80.46 (142.250.80.46) 25.345 ms  24.211 ms  26.089 ms
```

**Les `* * *`** : ce routeur ne répond pas (peut être configuré pour ne pas répondre aux traceroutes)

```bash
# Version interactive (en continu)
mtr google.com

# Avec 10 paquets et arrête
mtr -c 10 google.com

# Résultat (Live) :
#                              My traceroute  [v0.91]
# google.com                                 --- Stats ---
#  1. 192.168.1.1                         1.2ms  0.9ms  1.5ms
#  2. 10.0.0.1                           15.2ms 12.1ms 14.9ms
#  3. 195.154.1.1                        20.1ms 19.2ms 21.0ms
```

`mtr` = c'est cool parce qu'il montre la latence et la perte EN CONTINU. Tu peux voir si un hop est responsable de la lenteur.

### 1.9 Sous le capot — comment fonctionne tracert

```
Envoi des paquets :
  Paquet 1 : TTL=1 (va mourir au premier routeur)
    → Routeur 1 dit "TTL=0, je l'envoie ailleurs", envoie un "ICMP Time Exceeded"
    
  Paquet 2 : TTL=2 (va mourir au 2e routeur)
    → Routeur 1 le laisse passer
    → Routeur 2 dit "TTL=1, je l'envoie ailleurs", envoie "ICMP Time Exceeded"
    
  Paquet 3 : TTL=3
    → Passe Routeur 1 et 2
    → Routeur 3 le tue et répond
    
  ... jusqu'à atteindre la destination
```

À chaque fois, ta machine mesure le temps et affiche la réponse.

### 1.10 Pièges avec tracert

⚠️ **Piège 1** : Routeurs qui ne répondent pas
```
Certains routeurs sont configurés pour NE PAS répondre aux requests ICMP Time Exceeded
Résultat : tu vois * * * à ce hop
C'est normal. C'est juste un routeur "silencieux"
```

⚠️ **Piège 2** : Chemins différents
```
Chaque hop du tracert peut être un paquet différent
Parfois tu vois un hop avec 3 latences TRÈS différentes :
1ms, 2ms, 450ms

C'est parce que le 3e paquet a pris une route différente.
C'est normal aussi. Le réseau rééquilibre les charges.
```

⚠️ **Piège 3** : Tracert très lent
```
Sur Windows, tracert attend 3 secondes par hop par défaut.
Si tu as 30 hops et qu'ils ne répondent pas, c'est 90 secondes d'attente.
Utilise -w 1 pour attendre que 1 seconde :
  tracert -w 1 google.com
```

---

### 1.11 Vérifier la latence et la perte de paquets

**Latence** = délai aller-retour en millisecondes  
**Perte** = pourcentage de paquets qui ne reviennent jamais

```powershell
# Windows — Ping répété et extraction de stats
ping -n 100 google.com | findstr "Lost"

# Résultat : "Statistiques Ping pour 142.250.80.46: ... perdus = 0 (0% de perte)"
```

```bash
# Linux — Ping standard affiche les stats à la fin
ping -c 100 google.com

# Résultat final :
# 100 packets transmitted, 100 received, 0.0% packet loss, time 103s
# rtt min/avg/max/stddev = 24.123/25.432/30.123/1.234 ms
```

**Interpréter les résultats** :

| Métrique | Bon | Acceptable | Mauvais |
|----------|-----|-----------|---------|
| Latence (ms) | < 50ms | 50-100ms | > 100ms |
| Perte | 0% | < 5% | > 10% |
| Jitter (variabilité) | < 5ms | 5-20ms | > 20ms |

---

## Module 2 — Résolution de Noms (DNS) (2 jours)

### 2.1 Qu'est-ce que DNS ?

DNS = **Domain Name System** = le botteur du réseau qui traduit les noms en adresses IP.

Sans DNS, tu devrais taper `http://142.250.80.46` au lieu de `http://google.com`. Facile pour les machines, impossible pour les humains.

```
Toi : Je veux aller sur google.com
DNS : C'est à quelle IP ?
Serveur DNS : 142.250.80.46
Toi : OK, je me connecte à 142.250.80.46
```

### 2.2 Configurer ton serveur DNS

#### Sur Windows

```powershell
# Voir ta config DNS
ipconfig /all | findstr "DNS"

# Résultat :
# Serveurs DNS. . . . . . . . . . . . . : 8.8.8.8
#                                         8.8.4.4
#                                         2001:4860:4860::8888
```

```powershell
# Changer le serveur DNS (méthode GUI : Settings > Network > Change adapter options)
# Ou en PowerShell (Admin required) :

# Voir tous les adaptateurs
Get-NetAdapter

# Configurer DNS pour Ethernet
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses ("8.8.8.8", "8.8.4.4")

# Revenir au DHCP (laisse le serveur DHCP choisir)
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ResetServerAddresses
```

#### Sur Linux

```bash
# Voir la config DNS (méthode moderne : systemd)
systemd-resolve --status

# Résultat :
#            LLMNR setting: yes
#  MulticastDNS setting: no
#  DNSSEC setting: no
#        DNSSEC validated: no
#
# Servers: 8.8.8.8 192.168.1.1 2001:4860:4860::8888
```

```bash
# Voir l'ancienne manière (toujours là sur beaucoup de systèmes)
cat /etc/resolv.conf

# Résultat :
# nameserver 8.8.8.8
# nameserver 8.8.4.4
```

```bash
# Sur Ubuntu/Debian moderne : modifier netplan
sudo nano /etc/netplan/00-installer-config.yaml

# Ajouter :
# network:
#   ethernets:
#     eth0:
#       dhcp4: no
#       nameservers:
#         addresses: [8.8.8.8, 8.8.4.4]

sudo netplan apply
```

### 2.3 Requête DNS simple : nslookup

`nslookup` = outil GUI/CLI pour interroger DNS. C'est le plus simple.

#### Sur Windows

```powershell
# Résoudre un domaine simple
nslookup google.com

# Résultat :
# Server:  8.8.8.8                        [TON SERVEUR DNS]
# Address: 8.8.8.8
# 
# Non-authoritative answer:               [NOT authoritative = cached, pas direct du server]
# Name:    google.com
# Address: 142.250.80.46
# Address: 142.250.80.47
# Address: 142.250.80.48
# Address: 142.250.80.49
```

Google a **plusieurs adresses IP** pour la résilience. Elles peuvent répartir la charge.

```powershell
# Interroger un serveur DNS spécifique (au lieu de celui par défaut)
nslookup google.com 8.8.8.8

# Chercher un type de record spécifique
nslookup -type=MX google.com    # Mail exchange
nslookup -type=NS google.com    # Nameservers
nslookup -type=A google.com     # IPv4
nslookup -type=AAAA google.com  # IPv6
nslookup -type=CNAME google.com # Alias
```

#### Sur Linux/Mac

```bash
# Linux/Mac n'a souvent pas nslookup (vieillot), mais a dig (modern et plus puissant)
dig google.com

# Résultat :
# ; <<>> DiG 9.16.1-Ubuntu
# ; <<>> google.com
# ;; QUESTION SECTION:
# ;google.com.                    IN    A
# 
# ;; ANSWER SECTION:
# google.com.             300    IN    A    142.250.80.46
# google.com.             300    IN    A    142.250.80.47
# google.com.             300    IN    A    142.250.80.48
# google.com.             300    IN    A    142.250.80.49
# 
# ;; Query time: 25 msec
# ;; SERVER: 8.8.8.8#53(8.8.8.8)
# ;; WHEN: Thu Aug 19 10:00:00 UTC 2026
# ;; MSG SIZE  rcvd: 160
```

```bash
# Afficher SEULEMENT l'IP (court)
dig +short google.com

# Interroger un serveur spécifique
dig @1.1.1.1 google.com

# Reverse lookup : quel domaine pour cette IP ?
dig -x 142.250.80.46

# Résultat :
# 46.80.250.142.in-addr.arpa. 3600 IN PTR bru06s03-in-f46.1e100.net.
```

### 2.4 Types de records DNS

| Record | Signification | Exemple |
|--------|--|--|
| **A** | Adresse IPv4 | google.com → 142.250.80.46 |
| **AAAA** | Adresse IPv6 | google.com → 2607:f8b0:4004:805::200e |
| **MX** | Mail exchange | google.com → 10 smtp.google.com |
| **NS** | Nameserver | google.com → ns1.google.com |
| **CNAME** | Alias/Canonical name | www.google.com → google.com |
| **TXT** | Texte libre (SPF, DKIM, etc.) | "v=spf1 include:..." |
| **SOA** | Start of Authority | Infos administratives |

```bash
# Voir tous les records MX pour un domaine
dig google.com MX +short

# Résultat :
# 10 smtp.google.com.
# 20 smtp2.google.com.
# 30 smtp3.google.com.

# C'est l'ordre de priorité : essayer smtp.google.com en premier, puis smtp2, etc.
```

### 2.5 Cache DNS — pourquoi les changements mettent du temps

Quand tu requêtes un domaine, ton système le **met en cache**.

```
Première requête : "google.com?" → Serveur DNS → met en cache (TTL=300 = 5 minutes)
2e requête (1 minute plus tard) : Pas d'interrogation, utilise le cache
3e requête (6 minutes plus tard) : Cache expiré, interroge à nouveau
```

**TTL (Time To Live)** = durée de vie du cache, en secondes.

```bash
dig google.com | grep "^google.com"

# Résultat :
# google.com.             272    IN    A    142.250.80.46
#                         ↑
#                    TTL = 272 secondes restantes

# Dans 272 secondes, le cache expire.
```

### 2.6 Vider le cache DNS

Parfois tu changes ta config et le cache te poursuit.

#### Sur Windows

```powershell
# Vider le cache DNS
ipconfig /flushdns

# Résultat :
# Vérification de la configuration TCP/IP
# Cache du système de résolution DNS vidé.

# Vérifier le cache
ipconfig /displaydns

# Résultat :
# Cache DNS
# -----------
# google.com
#     TTL : 300
#     Data Length : 4
#     A (Host) Record : 142.250.80.46
```

#### Sur Linux

```bash
# Beaucoup de systèmes Linux n'ont pas de cache DNS au niveau OS
# Mais systemd-resolved en a un :

sudo systemd-resolve --flush-caches

# Vérifier le cache
systemd-resolve --statistics

# Résultat :
# ...
# Current Transactions: 0
# ...
```

### 2.7 Exercice

1. Utilise `nslookup` ou `dig` pour voir toutes les IPs de Google
2. Change ton serveur DNS à `1.1.1.1` (Cloudflare) et refais la requête
3. Fais un `reverse lookup` sur `8.8.8.8`
4. Cherche les MX records d'un domaine
5. Regarde le TTL et attends que le cache expire

---

## Module 3 — Ports, Services et Connexions (2-3 jours)

### 3.1 Qu'est-ce qu'un port ?

Une adresse IP identifie une **machine**. Un port identifie un **service sur cette machine**.

```
192.168.1.100        = C'est TA machine
192.168.1.100:22     = Le service SSH sur ta machine
192.168.1.100:80     = Le service Web HTTP sur ta machine
192.168.1.100:443    = Le service Web HTTPS sur ta machine
192.168.1.100:3306   = Base de données MySQL sur ta machine
```

**Ports "bien connus"** (0-1023, réservés) :

| Port | Protocole | Service |
|------|-----------|---------|
| 20 | TCP | FTP Data |
| 21 | TCP | FTP Control |
| 22 | TCP | SSH (Shell sécurisé) |
| 23 | TCP | Telnet (Shell non-sécurisé) |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP (Web) |
| 443 | TCP | HTTPS (Web sécurisé) |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |
| 6379 | TCP | Redis |
| 8080 | TCP | Serveurs web alternatifs |
| 8443 | TCP | HTTPS alternatif |

**Ports éphémères** (49152-65535) : ton client les utilise quand tu **envoies** une requête. C'est temporaire.

### 3.2 Voir les connexions actives : netstat et ss

`netstat` = Network Statistics. Montre toutes les connexions actives (TCP/UDP) et les services qui "écoutent".

#### Sur Windows

```powershell
# Voir toutes les connexions (c'est BEAUCOUP)
netstat

# Résultat :
# Connexions actives
# Proto  Adresse Locale     Adresse Distante    État
# TCP    192.168.1.100:52342 142.250.80.46:443   ESTABLISHED
# TCP    192.168.1.100:52343 142.250.80.46:443   ESTABLISHED
# TCP    127.0.0.1:8080      127.0.0.1:52344     TIME_WAIT
```

```powershell
# Voir SEULEMENT les services qui "écoutent" (LISTENING)
netstat -an | findstr LISTENING

# Résultat :
# TCP    0.0.0.0:22           0.0.0.0:0           LISTENING
# TCP    0.0.0.0:80           0.0.0.0:0           LISTENING
# TCP    127.0.0.1:3306       0.0.0.0:0           LISTENING
# TCP    0.0.0.0:443          0.0.0.0:0           LISTENING
```

**Ici** :
- `0.0.0.0:22` = SSH écoute sur TOUS les IP (0.0.0.0 = "toutes les interfaces")
- `127.0.0.1:3306` = MySQL écoute SEULEMENT sur localhost (pas accessible de l'extérieur)

```powershell
# Ajouter le processus (qui lance ce service ?)
netstat -ano | findstr LISTENING

# Résultat :
# TCP    0.0.0.0:22    0.0.0.0:0    LISTENING    4521
#                                    ↑
#                           PID du processus
```

```powershell
# Voir le nom du processus par le PID
tasklist | findstr 4521

# Résultat :
# sshd.exe           4521
```

ou

```powershell
# Version plus facile (Windows 10+)
netstat -ano | findstr :22

# Puis directement voir le processus
Get-Process | Where-Object {$_.Id -eq 4521}

# Résultat :
# NPM(K)    PM(M)      WS(M)  CPU(s)     Id    SI ProcessName
#   20      2.5       3.8     0.02    4521     0 sshd.exe
```

#### Sur Linux

`ss` = Socket Statistics. C'est le remplaçant moderne de `netstat`.

```bash
# Voir SEULEMENT les sockets en LISTENING
ss -tlnp

# t = TCP, l = LISTENING, n = numérique (pas de noms), p = processus
# Résultat :
# State        Recv-Q Send-Q Local Address         Foreign Address      Process
# LISTEN       0      128    0.0.0.0:22            0.0.0.0:*            users:(("sshd",pid=1234,fd=3))
# LISTEN       0      128    :::22                 :::*                  users:(("sshd",pid=1234,fd=3))
# LISTEN       0      511    127.0.0.1:3306        0.0.0.0:*            users:(("mysqld",pid=5678,fd=15))
```

```bash
# Voir TOUTES les connexions (établies + sockets)
ss -an

# Voir les connexions avec latency/RTO
ss -i

# Statistiques résumées
ss -s

# Résultat :
# TCP:   14 (estab 2, closed 0, orphaned 0, synrecv 0, timewait 0), ports 0
#
# Transport Total IP IPv6
# *         20    -    -
# RAW       0     0    0
# UDP       4     2    2
# TCP       14    10   4
# INET      18    12   6
# FRAG      0     0    0
```

### 3.3 Quel processus utilise quel port ?

#### Sur Windows

```powershell
# Trouver qui écoute sur le port 8080
netstat -ano | findstr :8080

# Résultat :
# TCP    0.0.0.0:8080    0.0.0.0:0    LISTENING    5678

# Voir le processus
tasklist | findstr 5678

# Résultat :
# java.exe    5678
```

#### Sur Linux

```bash
# Directement, avec lsof
lsof -i :8080

# Résultat :
# COMMAND    PID  USER    FD   TYPE  DEVICE SIZE/OFF NODE NAME
# java    5678  user    15u  IPv4  123456  0t0     TCP  *:8080 (LISTEN)

# Ou avec ss et grep
ss -tlnp | grep :8080

# Résultat :
# LISTEN  0  128  0.0.0.0:8080  0.0.0.0:*  users:(("java",pid=5678,fd=15))
```

### 3.4 États de connexion TCP

Une connexion TCP peut être dans plusieurs états. Les plus courants :

| État | Signification |
|------|---|
| **LISTEN** | Serveur attend une connexion |
| **ESTABLISHED** | Connexion active |
| **TIME_WAIT** | Connexion fermée, mais l'OS attend avant de libérer le port |
| **CLOSE_WAIT** | L'autre bout a fermé, on attend qu'on ferme aussi |
| **SYN_SENT** | On essaie de se connecter (syn-flood ?) |
| **SYN_RECEIVED** | Serveur reçoit une tentative de connexion |
| **CLOSING** | Les deux côtés ferment simultanément |

**TIME_WAIT** = c'est normal d'en voir beaucoup. C'est juste des connexions qui se ferment. Ça disparaît après quelques secondes.

### 3.5 Tester la connexion à un port spécifique

#### Sur Windows

```powershell
# Test moderne et propre
Test-NetConnection -ComputerName google.com -Port 443

# Résultat :
# ComputerName     : google.com
# RemoteAddress    : 142.250.80.46
# RemotePort       : 443
# InterfaceAlias   : Ethernet
# SourceAddress    : 192.168.1.100
# TcpTestSucceeded : True
```

```powershell
# Vieux style : telnet (mais c'est ok pour les tests)
telnet google.com 443

# Résultat :
# Si le port est ouvert, une fenêtre se ferme, tu vois juste des trucs vides
# Si le port est fermé, tu vois "Could not open connection to the host on port 443"
```

#### Sur Linux

```bash
# Outil netcat (nc) — très common
nc -zv google.com 443

# z = vérifier juste (ne pas garder la connection)
# v = verbose
# Résultat :
# Connection to google.com 443 port [tcp/https] succeeded!
```

```bash
# Avec timeout (ne pas attendre 15 secondes si c'est fermé)
timeout 2 bash -c "echo >/dev/tcp/google.com/443" && echo "Open" || echo "Closed"

# Résultat :
# Open
```

### 3.6 Exercice

1. Vois les ports qui écoutent sur TA machine
2. Vérifie quel processus écoute sur le port 22 (SSH) ou 80 (Web)
3. Test si Google est accessible sur le port 443
4. Test si un service local écoute sur le port 3306

---

## Module 4 — Configuration Réseau (3 jours)

### 4.1 Configurer une IP manuellement (Windows)

#### Méthode 1 : Interface Graphique (GUI)

```
Settings → Network & Internet → Change adapter options
  → Droite-clic sur Ethernet → Properties
  → Internet Protocol Version 4 (TCP/IPv4) → Properties
  → Utiliser l'adresse IP suivante :
       IP Address : 192.168.1.50
       Subnet Mask : 255.255.255.0
       Default Gateway : 192.168.1.1
       DNS : 8.8.8.8
```

#### Méthode 2 : PowerShell (CLI)

```powershell
# Voir les adaptateurs
Get-NetAdapter

# Résultat :
# Name    Status MacAddress       LinkSpeed
# Ethernet Up    00-1A-2B-3C-4D-5E 1 Gbps
```

```powershell
# Supprimer la config DHCP actuelle (si elle existe)
Remove-NetIPAddress -InterfaceAlias "Ethernet" -Confirm:$false
Remove-NetRoute -InterfaceAlias "Ethernet" -Confirm:$false

# Ajouter une adresse IP fixe
New-NetIPAddress -InterfaceAlias "Ethernet" `
  -IPAddress 192.168.1.50 `
  -PrefixLength 24 `
  -DefaultGateway 192.168.1.1

# Configurer le serveur DNS
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" `
  -ServerAddresses ("8.8.8.8", "8.8.4.4")

# Vérifier
ipconfig
```

### 4.2 Revenir au DHCP (Windows)

```powershell
# DHCP = la box configure tout automatiquement
Set-NetIPInterface -InterfaceAlias "Ethernet" -DHCP Enabled

# Vérifier
ipconfig

# Tu devrais voir "DHCP Enabled : Yes"
```

### 4.3 Configurer une IP manuellement (Linux)

#### Ubuntu/Debian (Netplan)

```bash
# Fichier de config
sudo nano /etc/netplan/00-installer-config.yaml

# Ajouter ceci :
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: false          # Désactiver DHCP
      addresses:
        - 192.168.1.50/24   # IP + masque (CIDR)
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]

# Sauvegarder avec Ctrl+X, Y, Enter

# Appliquer les changements
sudo netplan apply

# Vérifier
ip addr show
```

#### CentOS/RHEL (nmtui ou nmcli)

```bash
# Voir les connexions
nmcli connection show

# Résultat :
# NAME          UUID                                  TYPE      DEVICE
# System eth0   5fb06bd0-1234-5678-90ab-cdef12345678 ethernet  eth0
```

```bash
# Modifier la connexion avec nmtui (interface graphique)
nmtui

# Ou en ligne de commande :
sudo nmcli connection modify "System eth0" \
  ipv4.addresses 192.168.1.50/24 \
  ipv4.gateway 192.168.1.1 \
  ipv4.dns 8.8.8.8 \
  ipv4.method manual

sudo nmcli connection down "System eth0"
sudo nmcli connection up "System eth0"

# Vérifier
ip addr show
```

### 4.4 Tester les changements

```powershell
# Windows
ipconfig

# Tu devrais voir :
# Adresse IPv4............. : 192.168.1.50
# Masque de sous-réseau.... : 255.255.255.0
# Passerelle par défaut.... : 192.168.1.1
```

```bash
# Linux
ip addr show

# Tu devrais voir :
# inet 192.168.1.50/24 brd 192.168.1.255 scope global eth0
```

```bash
# Test de connectivité
ping 192.168.1.1
ping 8.8.8.8
```

### 4.5 Configurer plusieurs IPs sur une interface

(C'est avancé, mais utile pour les tests)

#### Windows

```powershell
# Ajouter une 2e IP sur la même interface
New-NetIPAddress -InterfaceAlias "Ethernet" `
  -IPAddress 192.168.1.51 `
  -PrefixLength 24

# Maintenant, ta machine répond sur 192.168.1.50 ET 192.168.1.51

# Voir toutes les IPs
ipconfig
```

#### Linux

```bash
# Ajouter une 2e IP
sudo ip addr add 192.168.1.51/24 dev eth0

# Voir
ip addr show

# Supprimer
sudo ip addr del 192.168.1.51/24 dev eth0
```

### 4.6 Exercice

1. Configure une adresse IP fixe sur ta machine (pas localhost !)
2. Ping ton gateway (192.168.1.1 ou autre)
3. Reviens au DHCP
4. Ajoute une 2e IP et test la reachability

---

## Module 5 — Routage et Tracés (2-3 jours)

### 5.1 Voir la table de routage

La table de routage dit : "Pour aller vers X, passe par Y".

#### Sur Windows

```powershell
# Voir la table de routage
route print

# Résultat :
# Routes IPv4
# ===========================================================================
# Destination            Netmask          Gateway            Interface
# 0.0.0.0                0.0.0.0          192.168.1.1        192.168.1.100
# 127.0.0.0              255.0.0.0        On-link            127.0.0.1
# 192.168.1.0            255.255.255.0    On-link            192.168.1.100
# 224.0.0.0              240.0.0.0        On-link            192.168.1.100
```

**Interprétation** :

- `0.0.0.0/0 via 192.168.1.1` = Pour tout ce qui est pas dans mes sous-réseaux, passe par le gateway
- `127.0.0.0/8 via On-link` = Tout en 127.* est local (c'est moi)
- `192.168.1.0/24 via On-link` = Tout en 192.168.1.* est dans mon sous-réseau (pas besoin de gateway)

#### Sur Linux

```bash
# Version moderne
ip route show

# Résultat :
# default via 192.168.1.1 dev eth0 proto dhcp metric 100
# 192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.100

# Ancienne version
route -n

# Résultat :
# Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
# 0.0.0.0         192.168.1.1     0.0.0.0         UG    100    0      0   eth0
# 192.168.1.0     0.0.0.0         255.255.255.0   U     0      0      0   eth0
```

### 5.2 Ajouter une route personnalisée

Supposons que tu veux que tout le trafic vers `10.0.0.0/8` passe par un routeur spécial `192.168.1.254`.

#### Sur Windows

```powershell
# Ajouter la route
route add 10.0.0.0 mask 255.255.0.0 192.168.1.254

# Vérifier
route print | findstr "10.0.0.0"

# Résultat :
# 10.0.0.0            255.255.0.0      192.168.1.254      192.168.1.100

# Supprimer la route
route delete 10.0.0.0 mask 255.255.0.0

# Ajouter une route persistante (survit aux redémarrages)
route add 10.0.0.0 mask 255.255.0.0 192.168.1.254 -p
```

#### Sur Linux

```bash
# Ajouter la route
sudo ip route add 10.0.0.0/8 via 192.168.1.254

# Vérifier
ip route show

# Supprimer
sudo ip route del 10.0.0.0/8

# Ajouter une route persistante (modifie le fichier de config)
sudo nano /etc/netplan/00-installer-config.yaml

# Ajouter sous la section ethernets :
# eth0:
#   routes:
#     - to: 10.0.0.0/8
#       via: 192.168.1.254

sudo netplan apply
```

### 5.3 Cas d'usage : routage par VPN

Quand tu utilises un VPN, une route personnalisée redirige tout le trafic vers le VPN.

```
Sans VPN :
Toi (192.168.1.100) → Gateway (192.168.1.1) → Internet

Avec VPN :
Toi → VPN Client (local) → Tunnel chiffré → VPN Server → Internet
```

La table de routage change :

```bash
# Avant VPN
default via 192.168.1.1

# Après VPN
default via 10.8.0.1       # (tunnel VPN)
192.168.1.0/24 via 192.168.1.1   # (réseau local reste direct)
```

---

## Module 6 — Firewall et Filtrage (2-3 jours)

### 6.1 État du firewall

#### Sur Windows

```powershell
# Voir le statut du firewall
Get-NetFirewallProfile

# Résultat :
# Name    Enabled
# ----    -------
# Domain  True
# Private True
# Public  True

# Si tu veux juste "Enabled"
Get-NetFirewallProfile | Select-Object Name, Enabled
```

```powershell
# Désactiver le firewall (⚠️ ATTENTION : rend ta machine vulnérable !)
Set-NetFirewallProfile -All -Enabled $false

# Réactiver
Set-NetFirewallProfile -All -Enabled $true
```

#### Sur Linux (UFW)

```bash
# Voir le statut
sudo ufw status

# Résultat :
# Status: active
# 
# To              Action      From
# --              ------      ----
# 22              ALLOW       Anywhere
# 80              ALLOW       Anywhere
# 443             ALLOW       Anywhere

# Activer UFW
sudo ufw enable

# Désactiver (⚠️ attention)
sudo ufw disable

# Voir les règles "verbées"
sudo ufw show added

# Voir les règles raw (format importable)
sudo ufw show raw
```

### 6.2 Autoriser un port

#### Sur Windows

```powershell
# Autoriser le port 8080 en TCP
New-NetFirewallRule -DisplayName "Allow HTTP Alternative" `
  -Direction Inbound `
  -Action Allow `
  -Protocol TCP `
  -LocalPort 8080

# Vérifier
Get-NetFirewallRule -DisplayName "Allow HTTP Alternative" | Format-List

# Supprimer
Remove-NetFirewallRule -DisplayName "Allow HTTP Alternative"
```

```powershell
# Autoriser un intervalle de ports (8000-8100)
New-NetFirewallRule -DisplayName "Allow Dev Servers" `
  -Direction Inbound `
  -Action Allow `
  -Protocol TCP `
  -LocalPort 8000-8100
```

```powershell
# Lister TOUTES les règles ALLOW
Get-NetFirewallRule -Direction Inbound -Enabled $true | Select-Object DisplayName, Direction, Action
```

#### Sur Linux (UFW)

```bash
# Autoriser le port 8080
sudo ufw allow 8080

# Autoriser un port avec un protocole spécifique
sudo ufw allow 22/tcp      # SSH (TCP)
sudo ufw allow 53/udp      # DNS (UDP)

# Interdire un port
sudo ufw deny 3306

# Autoriser un intervalle
sudo ufw allow 8000:8100/tcp

# Autoriser un port pour une IP spécifique
sudo ufw allow from 192.168.1.50 to any port 22

# Supprimer une règle
sudo ufw delete allow 8080
```

### 6.3 Autoriser un processus

#### Sur Windows

```powershell
# Permettre à un programme d'être accessible de l'extérieur
New-NetFirewallRule -DisplayName "Allow My App" `
  -Direction Inbound `
  -Action Allow `
  -Program "C:\apps\myapp.exe"
```

#### Sur Linux

Sur Linux, le firewall ne connaît pas les "programmes". Tu vas autoriser les **ports** que le programme utilise.

```bash
# Exemple : si ma app écoute sur 8080
sudo ufw allow 8080/tcp
```

### 6.4 Bloquer tout et autoriser seulement ce que tu veux

C'est la meilleure pratique ("default deny").

#### Sur Windows

```powershell
# Changer la politique par défaut (inbound = deny, outbound = allow)
Set-NetFirewallProfile -DefaultInboundAction Block -DefaultOutboundAction Allow -All

# Puis authoriser seulement SSH et HTTP
New-NetFirewallRule -DisplayName "Allow SSH" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 22
New-NetFirewallRule -DisplayName "Allow HTTP" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 80
New-NetFirewallRule -DisplayName "Allow HTTPS" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 443
```

#### Sur Linux

```bash
# Par défaut, UFW bloque tout le trafic entrant
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Autoriser seulement SSH, HTTP, HTTPS
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Activer
sudo ufw enable
```

### 6.5 Exercice

1. Vois le statut de ton firewall
2. Ouvre le port 8080 pour un test local
3. Teste si tu peux te connecter à ce port
4. Ferme le port
5. Revérifie que c'est bien fermé

---

## Module 7 — Monitoring Réseau en Temps Réel (2 jours)

### 7.1 Bande passante utilisée par interface

#### Sur Windows

```powershell
# Voir l'utilisation par interface
Get-NetAdapterStatistics

# Résultat :
# Name    ReceivedBytes  ReceivedUnicastPackets  SentBytes  SentUnicastPackets
# Ethernet 1234567890    1023456                 987654321  654321
```

```powershell
# Voir en temps réel l'utilisation CPU/Réseau
Get-Counter -Counter "\Network Interface(*)\Bytes Received/sec", "\Network Interface(*)\Bytes Sent/sec" | Select-Object -ExpandProperty CounterSamples
```

#### Sur Linux

```bash
# Outil nload (affiche la bande passante en temps réel, genre un graph)
nload

# Outil iftop (montre quel IP envoie/reçoit le plus)
sudo iftop

# Outil nethogs (montre quel processus utilise le plus de bande)
sudo nethogs

# Simple : juste voir le trafic par interface
ip -s link show eth0

# Résultat :
# 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
#    RX: bytes  packets  errors  dropped overrun mcast
#    1234567890 123456   0       0       0       0
#    TX: bytes  packets  errors  dropped carrier collsns
#    987654321  654321   0       0       0       0
```

### 7.2 Monitoring continu — voir ce qui se passe en direct

#### Sur Linux

```bash
# Actualiser chaque 1 seconde : voir les stats TCP
watch -n 1 'ss -s'

# Résultat :
# Current SOCKETS:
# TCP:   14 (estab 2, closed 0, orphaned 0, synrecv 0, timewait 0), ports 0

# Voir les connexions qui se font en temps réel
watch -n 1 'ss -tnp'

# Résultat :
# LISTEN  0  128  0.0.0.0:22   0.0.0.0:*  users:(("sshd",pid=1234,fd=3))
# ESTAB   0  0    192.168.1.100:52342  142.250.80.46:443  users:(("chrome",pid=5678,fd=15))
```

```bash
# Voir juste les connexions "ESTABLISHED"
watch -n 1 'ss -tnp | grep ESTAB'
```

#### Sur Windows

```powershell
# PowerShell n'a pas d'équivalent "watch", mais on peut loop
while ($true) {
  Clear-Host
  Get-NetTCPConnection -State Established | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, OwningProcess
  Start-Sleep -Seconds 1
}
```

### 7.3 Analyzer quel processus utilise le plus de bande (Ressource Monitor)

#### Sur Windows

```powershell
# Lancer Resource Monitor
resmon

# Ou directement :
Get-Process | Where-Object {$_.WorkingSet -gt 100MB} | Select-Object Name, WorkingSet, CPU
```

#### Sur Linux

```bash
# nethogs montre le trafic par processus
sudo nethogs

# Résultat :
# RECEIVED   SENT       PID USER    COMMAND
# 1.2 MB     456 KB     5678 user   chrome
# 234 KB     123 KB     1234 user   firefox
# 45 KB      12 KB      9012 root   sshd
```

---

## Module 8 — Transfert de Fichiers et Tunnel (2 jours)

### 8.1 Copier des fichiers via le réseau : SCP et SFTP

#### SCP (Secure CoPy)

```bash
# Copier UN fichier depuis le serveur vers toi
scp user@192.168.1.100:/home/user/fichier.txt ./

# Copier UN fichier vers le serveur
scp fichier.txt user@192.168.1.100:/home/user/

# Copier un dossier ENTIER (-r = récursive)
scp -r user@192.168.1.100:/home/user/dossier ./

# Spécifier un port personnalisé (si SSH n'est pas sur 22)
scp -P 2222 user@192.168.1.100:/home/user/fichier.txt ./

# Afficher la progression
scp -v user@192.168.1.100:/home/user/fichier.txt ./
```

#### SFTP (SSH File Transfer Protocol)

SFTP c'est comme FTP mais sécurisé. C'est une session interactive.

```bash
# Ouvrir une session SFTP
sftp user@192.168.1.100

# Commandes à l'intérieur :
# get fichier.txt                   # Télécharger
# put fichier.txt                   # Uploader
# cd /home/user                     # Changer dossier distant
# ls                                # Lister distant
# !ls                               # Lister local
# exit                              # Quitter

# Exemple :
cd /tmp
get data.zip
cd /home/user/documents
put rapport.docx
exit
```

### 8.2 SSH Tunnel — accéder à un service distant comme s'il était local

Supposons : tu as une base de données MySQL sur `192.168.1.100:3306`, mais tu peux seulement accéder via SSH.

#### Cas 1 : Tunnel local (Forward)

Tu crées un tunnel : `localhost:3306` → `Serveur distant:3306`

```bash
# Créer le tunnel
ssh -L 3306:localhost:3306 user@192.168.1.100

# Le tunnel est maintenant actif
# Dans UN AUTRE TERMINAL, connecte-toi à MySQL :
mysql -h localhost -u root

# Tout ce que tu envoies à localhost:3306 passe par le tunnel SSH
# C'est chiffré et sécurisé !
```

#### Cas 2 : Tunnel distant (Reverse)

Pas courant, mais : tu es sur un serveur lointain et tu veux utiliser un service sur ta machine locale.

```bash
# Sur ta machine locale, crée un tunnel inverse
ssh -R 3306:localhost:3306 user@distant.server.com

# Maintenant, sur le serveur distant :
mysql -h localhost -u root
# Se connecte à TÀ machine locale
```

#### Cas 3 : Tunnel sur un port différent (parce que 3306 est occupé)

```bash
# Créer le tunnel sur le port local 13306
ssh -L 13306:192.168.1.101:3306 user@192.168.1.100

# Puis se connecter à ta machine :
mysql -h localhost -P 13306 -u root

# localhost:13306 → (tunnel) → 192.168.1.100:22 → 192.168.1.101:3306
```

### 8.3 Cas d'usage : Accéder à un Redis distant

```bash
# Redis écoute sur 192.168.1.100:6379
# Tu peux accéder via SSH sur 192.168.1.100:22

# Créer le tunnel
ssh -L 6379:192.168.1.100:6379 user@192.168.1.100

# Maintenant, dans un autre terminal :
redis-cli
> ping
PONG
```

### 8.4 Exercice

1. Copie un fichier de ta machine vers une autre (ou vice versa) avec SCP
2. Crée un tunnel SSH vers un port local
3. Accède au service via localhost

---

## Module 9 — Diagnostic Avancé : Packet Capture et Wireshark (2-3 jours)

### 9.1 Capture simple avec tcpdump

`tcpdump` = capture les paquets réseau au niveau bas. Tu vois EXACTEMENT ce qui passe sur le fil.

#### Sur Linux

```bash
# Capturer tout le trafic
sudo tcpdump

# Résultat :
# tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
# listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
# 10:00:00.123456 IP 192.168.1.100.52342 > 142.250.80.46.443: Flags [S], seq 1234567890, win 65535
# 10:00:00.124567 IP 142.250.80.46.443 > 192.168.1.100.52342: Flags [S.], seq 9876543210, ack 1234567891, win 65535
```

```bash
# Capturer SEULEMENT le trafic HTTP (port 80)
sudo tcpdump -i eth0 'tcp port 80'

# Capturer SEULEMENT le trafic HTTPS (port 443)
sudo tcpdump -i eth0 'tcp port 443'

# Capturer SEULEMENT le trafic DNS (port 53)
sudo tcpdump -i eth0 'udp port 53'

# Capturer le trafic vers/depuis une IP spécifique
sudo tcpdump -i eth0 'host 142.250.80.46'

# Capturer SEULEMENT les paquets entrants
sudo tcpdump -i eth0 'inbound'

# Capturer SEULEMENT les paquets sortants
sudo tcpdump -i eth0 'outbound'
```

```bash
# Sauvegarder dans un fichier (format pcap)
sudo tcpdump -i eth0 -w capture.pcap

# Lire le fichier après
tcpdump -r capture.pcap

# Lire avec plus de détails
tcpdump -r capture.pcap -v

# Lire et afficher les payloads (données brutes)
tcpdump -r capture.pcap -A

# Lire et filtrer (ex: HTTP seulement)
tcpdump -r capture.pcap -A 'tcp port 80'
```

### 9.2 Formules de filtre courants

```bash
# TCP (pas UDP)
sudo tcpdump 'tcp'

# UDP (pas TCP)
sudo tcpdump 'udp'

# Port spécifique
sudo tcpdump 'port 443'

# Plage de ports
sudo tcpdump 'portrange 8000-8100'

# IP spécifique
sudo tcpdump 'host 192.168.1.100'

# Sous-réseau
sudo tcpdump 'net 192.168.1.0/24'

# Combinaisons (ET = and, OU = or)
sudo tcpdump '(tcp port 80) or (tcp port 443)'

sudo tcpdump '(tcp) and (host 192.168.1.100)'

sudo tcpdump '(not host 127.0.0.1) and (tcp port 22)'
```

### 9.3 Wireshark (Interface Graphique)

`tcpdump` c'est puissant, mais pas très visuel. `Wireshark` est l'outil GUI.

```bash
# Installer
sudo apt install wireshark

# Lancer
wireshark &

# Ou directement une capture
sudo wireshark -i eth0
```

**Dans Wireshark** :
1. Sélectionne une interface réseau (eth0, wlan0, etc.)
2. Appuie sur le bouton "Play" (Capture → Start)
3. Tu vois les paquets arriver en temps réel
4. Tu peux filtrer, trier, analyser

**Filtres Wireshark** (même interface, différents noms) :

```
http                # Afficher seulement HTTP
dns                 # Afficher seulement DNS
tcp.port == 22      # Afficher seulement SSH
ip.src == 192.168.1.100    # De cette source
ip.dst == 8.8.8.8          # Vers cette destination
```

**Analyser une conversation TCP entière** :
```
Clic-droit sur un paquet → Follow TCP Stream
→ Tu vois toute la conversation (requête + réponse)
```

### 9.4 Cas d'usage : Voir le contenu d'une requête HTTP

```bash
# Capturer le trafic HTTP
sudo tcpdump -i eth0 'tcp port 80' -A

# Résultat :
# ...
# GET /path HTTP/1.1
# Host: example.com
# User-Agent: Mozilla/5.0
# ...
```

### 9.5 Exercice

1. Lance une capture tcpdump du trafic DNS (port 53)
2. Dans un autre terminal, fais un nslookup
3. Vois le paquet DNS dans tcpdump
4. Ouvre Wireshark et capture du trafic HTTP (visite un site web)
5. Analyser les paquets

---

## Module 10 — Scripts Réseau (Automation) (3-4 jours)

### 10.1 Script Windows PowerShell : Vérifier si les serveurs sont accessibles

```powershell
# Fichier : check_servers.ps1

$servers = @(
  "google.com",
  "8.8.8.8",
  "192.168.1.1",
  "github.com"
)

Write-Host "Vérification des serveurs..." -ForegroundColor Green

foreach ($server in $servers) {
  if (Test-Connection -ComputerName $server -Quiet -Count 1) {
    Write-Host "✓ $server : Accessible" -ForegroundColor Green
  } else {
    Write-Host "✗ $server : Inaccessible" -ForegroundColor Red
  }
}
```

**Lancer le script** :

```powershell
PowerShell -ExecutionPolicy Bypass -File C:\scripts\check_servers.ps1
```

### 10.2 Script PowerShell : Scanner de ports

```powershell
# Fichier : scan_ports.ps1

param(
  [string]$Target = "192.168.1.1",
  [array]$Ports = @(22, 80, 443, 3306, 5432, 8080)
)

Write-Host "Scanning $Target for open ports..." -ForegroundColor Green

foreach ($port in $Ports) {
  $connection = New-Object System.Net.Sockets.TCPClient
  $connection.SendTimeout = 1000
  
  try {
    $connection.Connect($Target, $port)
    Write-Host "✓ Port $port : OPEN" -ForegroundColor Green
    $connection.Close()
  } catch {
    Write-Host "✗ Port $port : CLOSED" -ForegroundColor Red
  }
}
```

**Lancer le script** :

```powershell
PowerShell -ExecutionPolicy Bypass -File C:\scripts\scan_ports.ps1 -Target 192.168.1.100
```

### 10.3 Script Bash : Tester plusieurs ports sur plusieurs serveurs

```bash
#!/bin/bash
# Fichier : check_servers.sh

# Make executable : chmod +x check_servers.sh

servers=("google.com" "8.8.8.8" "192.168.1.1")
ports=(22 80 443 3306 8080)

echo "Scanning servers..."

for server in "${servers[@]}"; do
  echo "--- Checking $server ---"
  
  for port in "${ports[@]}"; do
    timeout 1 bash -c "echo >/dev/tcp/$server/$port" 2>/dev/null && \
      echo "✓ $server:$port OPEN" || \
      echo "✗ $server:$port CLOSED"
  done
  
  echo ""
done
```

**Lancer le script** :

```bash
chmod +x check_servers.sh
./check_servers.sh
```

### 10.4 Script Bash : Monitoring continu avec alertes email

```bash
#!/bin/bash
# Fichier : monitor_service.sh

# Make executable : chmod +x monitor_service.sh

TARGET="192.168.1.100"
PORT=8080
EMAIL="admin@example.com"
MAX_FAILURES=3
FAIL_COUNT=0

while true; do
  if timeout 2 bash -c "echo >/dev/tcp/$TARGET/$PORT" 2>/dev/null; then
    echo "✓ $(date) : Service $TARGET:$PORT is UP"
    FAIL_COUNT=0
  else
    FAIL_COUNT=$((FAIL_COUNT+1))
    echo "✗ $(date) : Service DOWN (Attempt $FAIL_COUNT)"
    
    if [ $FAIL_COUNT -eq $MAX_FAILURES ]; then
      echo "Service DOWN for $MAX_FAILURES attempts. Sending alert..."
      echo "Service $TARGET:$PORT is DOWN!" | mail -s "ALERT: Service Down" $EMAIL
      FAIL_COUNT=0  # Reset pour ne pas envoyer trop d'emails
    fi
  fi
  
  sleep 30  # Check toutes les 30 secondes
done
```

**Lancer en background** :

```bash
nohup ./monitor_service.sh &
```

### 10.5 Script PowerShell : Monitoring avec Email

```powershell
# Fichier : monitor_service.ps1

param(
  [string]$Target = "192.168.1.100",
  [int]$Port = 8080,
  [string]$EmailTo = "admin@example.com",
  [string]$EmailFrom = "monitoring@example.com",
  [string]$SMTPServer = "smtp.gmail.com"
)

while ($true) {
  $connection = New-Object System.Net.Sockets.TCPClient
  $connection.SendTimeout = 2000
  
  try {
    $connection.Connect($Target, $Port)
    Write-Host "✓ $(Get-Date) : Service is UP"
    $connection.Close()
    $FailCount = 0
  } catch {
    $FailCount++
    Write-Host "✗ $(Get-Date) : Service DOWN (Attempt $FailCount)"
    
    if ($FailCount -eq 3) {
      $EmailParams = @{
        To = $EmailTo
        From = $EmailFrom
        Subject = "ALERT: Service Down"
        Body = "Service $Target`:$Port is DOWN!"
        SmtpServer = $SMTPServer
      }
      Send-MailMessage @EmailParams
      $FailCount = 0
    }
  }
  
  Start-Sleep -Seconds 30
}
```

### 10.6 Exercice

1. Modifie le script de vérification de serveurs pour tester TES serveurs
2. Crée un script pour scanner les ports 1-100 d'une machine locale
3. Ajoute du logging (sauvegarde les résultats dans un fichier)
4. Rends le script automatique (Scheduled Task sur Windows, cron sur Linux)

---

## Projets Pratiques

### Projet 1 : Diagnostiquer une Panne Réseau

**Scénario** : Tu vas chez un client. Internet ne marche pas. Go !

**Étapes** :
1. Lance `ipconfig` (Windows) ou `ip addr show` (Linux) — ta machine a-t-elle une IP ?
2. Ping la gateway — c'est accessible ?
3. Ping un serveur externe (8.8.8.8) — Internet OK ?
4. Fais un `nslookup` — le DNS marche ?
5. Fais un `tracert` vers google.com — où ça casse ?

**Diagnostics possibles** :
- Pas d'IP → problème DHCP (redémarre le modem/routeur)
- Gateway inaccessible → cable débranché ou routeur en panne
- Internet inaccessible mais gateway OK → problème FAI
- DNS ne marche pas → paramètres DNS incorrects

### Projet 2 : Configurer un Serveur Accessible

**Objectif** : avoir un serveur web qui écoute sur le port 8080 et accessible depuis l'extérieur.

**Étapes** :
1. Lance un serveur simple : `python -m http.server 8080` (sur port 8080)
2. Teste localement : `Test-NetConnection localhost -Port 8080` (Windows) ou `nc -zv localhost 8080` (Linux)
3. Ouvre le port dans le firewall
4. Teste depuis une autre machine : `Test-NetConnection 192.168.1.X -Port 8080`
5. Teste depuis l'extérieur en utilisant l'IP publique de ton routeur

### Projet 3 : Monitorer le Trafic Réseau

**Objectif** : capture le trafic, identifie quel processus utilise le plus de bande.

**Étapes** :
1. Ouvre Wireshark ou tcpdump
2. Lance quelque chose qui utilise la bande (stream vidéo, gros téléchargement)
3. Vois les paquets arriver
4. Utilise `nethogs` (Linux) ou Resource Monitor (Windows) pour voir quel processus
5. Filtre dans Wireshark pour voir seulement ce trafic

### Projet 4 : Créer un Script de Monitoring Automatisé

**Objectif** : surveiller la disponibilité de plusieurs services et recevoir une alerte si l'un est down.

**Étapes** :
1. Choisis une liste de serveurs/ports à monitorer
2. Crée un script (Bash ou PowerShell)
3. Lance le script en background
4. Il envoie un email si un service tombe
5. Teste en tuant volontairement un service

### Projet 5 : Créer un Tunnel SSH sécurisé

**Objectif** : accéder à une base de données distante en passant par SSH.

**Étapes** :
1. Configure un serveur SSH sur une machine (ou utilise ssh.example.com)
2. Crée un tunnel : `ssh -L 3306:localhost:3306 user@server`
3. Connecte-toi via le tunnel à la base de données
4. Vois que tout passe par SSH (chiffré)

---

## Ressources

### Documentation

- **Windows** : [Microsoft Docs - PowerShell](https://docs.microsoft.com/powershell)
- **Linux** : Man pages (`man ping`, `man ss`, `man tcpdump`)
- **Cross-platform** : [GNU Coreutils Docs](https://www.gnu.org/software/coreutils)

### Outils interactifs pour apprendre

- **TryHackMe** : [Networking Basics](https://tryhackme.com) (labs gratuits)
- **Cybrary** : [Networking Fundamentals](https://www.cybrary.it)
- **HackTheBox** : [Machines avec networking](https://www.hackthebox.eu)

### Articles et guides

- TCP/IP en détail : Stevens, "TCP/IP Illustrated"
- DNS expliqué : [DNS the Good Parts](https://www.aaaaaaaaaa.net)
- Firewall rules : [UFW Docs](https://help.ubuntu.com/community/UFW)
- Wireshark : [Official Guide](https://www.wireshark.org/docs/)

### Simulateurs/Labos gratuits

- **GNS3** : simulateur de réseaux (routeurs, switches)
- **Cisco Packet Tracer** : labo réseau Cisco (gratuit)
- **VirtualBox + Linux** : crée ta propre infra réseau

---

## Conclusion

Tu as couvert les **commandes réseau essentielles** pour diagnostiquer, configurer et monitorer un réseau.

**Récapitulatif** :

✅ Module 0 : Fondamentaux (OSI, IP, protocoles)  
✅ Module 1 : Diagnostiquer (`ipconfig`, `ping`, `tracert`)  
✅ Module 2 : DNS (`nslookup`, `dig`)  
✅ Module 3 : Ports/Services (`netstat`, `ss`)  
✅ Module 4 : Configuration (`ipconfig`, `ip addr`)  
✅ Module 5 : Routage (`route`, `ip route`)  
✅ Module 6 : Firewall (Windows Defender, UFW)  
✅ Module 7 : Monitoring (`netstat`, `nethogs`, Wireshark)  
✅ Module 8 : Transfert (`scp`, SSH tunnels)  
✅ Module 9 : Capture (`tcpdump`, Wireshark)  
✅ Module 10 : Automation (Scripts Bash/PowerShell)  

**Prochaines étapes** :

1. Pratique chaque commande sur TA machine
2. Fais les projets
3. Apprends Docker pour virtualiser les réseaux
4. Explore Kubernetes (orchestration de services réseau)
5. Certifications : CompTIA Network+, Cisco CCNA

> **Règle d'or finale** : Le vrai apprentissage, c'est de **casser volontairement** ton réseau et de le réparer. Les erreurs sont tes meilleures professeurs.

Bonne chance ! 🚀
