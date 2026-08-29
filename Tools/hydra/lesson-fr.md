# Hydra Cheat Sheet

## Overview

Hydra est un outil rapide et flexible de cassage de mots de passe en ligne utilisé pour :

- Effectuer du brute-force de credentials de login.
- Tester des politiques de mots de passe.
- Exécuter des attaques par dictionnaire.
- Valider des mécanismes d'authentification.
- Réaliser des évaluations de sécurité autorisées.

Hydra prend en charge :

- De multiples protocoles (SSH, FTP, HTTP, SMB, MySQL, etc.).
- Des connexions parallèles pour plus de rapidité.
- Des wordlists et listes d'utilisateurs personnalisées.
- Le support de proxy.
- Sauvegarder et reprendre des attaques.

```text
Utilisez Hydra uniquement contre des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Starting Hydra

## Syntaxe de base

```bash
hydra [options] <target> <service>
```

## Afficher l'aide

```bash
hydra -h
```

## Afficher la version

```bash
hydra -V
```

## Afficher les protocoles supportés

```bash
hydra -h
```

Recherchez la liste des services supportés en bas de la sortie d'aide.

---

# 2. Target Specification

## Spécifier une seule cible

```bash
hydra <target-IP> <service>
```

Exemple :

```bash
hydra 192.168.1.10 ssh
```

## Spécifier plusieurs cibles

```bash
hydra -M targets.txt <service>
```

Où `targets.txt` contient une IP par ligne.

## Spécifier un port cible

```bash
hydra -s <port> <target-IP> <service>
```

Exemple :

```bash
hydra -s 2222 192.168.1.10 ssh
```

## Spécifier une cible via URL

Pour les services HTTP/HTTPS :

```bash
hydra <url> <service>
```

Exemple :

```bash
hydra http://192.168.1.10/login http-get-form
```

---

# 3. Username and Password Lists

## Spécifier un seul username

```bash
hydra -l <username> <target-IP> <service>
```

Exemple :

```bash
hydra -l admin 192.168.1.10 ssh
```

## Spécifier un seul mot de passe

```bash
hydra -p <password> <target-IP> <service>
```

Exemple :

```bash
hydra -p password123 192.168.1.10 ssh
```

## Spécifier une liste de usernames

```bash
hydra -L <userlist.txt> <target-IP> <service>
```

Exemple :

```bash
hydra -L users.txt 192.168.1.10 ssh
```

## Spécifier une liste de mots de passe

```bash
hydra -P <passlist.txt> <target-IP> <service>
```

Exemple :

```bash
hydra -P passwords.txt 192.168.1.10 ssh
```

## Spécifier les deux listes d'utilisateur et mot de passe

```bash
hydra -L users.txt -P passwords.txt <target-IP> <service>
```

## Utiliser une liste combinée (user:pass)

```bash
hydra -C combo.txt <target-IP> <service>
```

Où `combo.txt` contient des lignes au format `username:password`.

## Générer des usernames dynamiquement

Pour certains services, Hydra peut générer des usernames :

```bash
hydra -L users.txt -P passwords.txt <target-IP> <service>
```

---

# 4. Connection and Performance Options

## Définir le nombre de tâches parallèles

```bash
hydra -t <tasks> <target-IP> <service>
```

Exemple :

```bash
hydra -t 16 192.168.1.10 ssh
```

La valeur par défaut est 16 tâches.

## Définir le nombre de connexions parallèles par cible

```bash
hydra -c <connections> <target-IP> <service>
```

Exemple :

```bash
hydra -c 4 192.168.1.10 ssh
```

## Définir le timeout pour les connexions

```bash
hydra -w <seconds> <target-IP> <service>
```

Exemple :

```bash
hydra -w 30 192.168.1.10 ssh
```

## Définir le nombre maximum de tentatives

```bash
hydra -r <retries> <target-IP> <service>
```

Exemple :

```bash
hydra -r 3 192.168.1.10 ssh
```

## Temps d'attente entre les tentatives

```bash
hydra -d <delay> <target-IP> <service>
```

Exemple :

```bash
hydra -d 1 192.168.1.10 ssh
```

Ajoute un délai de 1 seconde entre les tentatives.

## Quitter après avoir trouvé le premier credential valide

```bash
hydra -f <target-IP> <service>
```

S'arrête après le premier login réussi.

## Quitter après avoir trouvé des credentials valides par utilisateur

```bash
hydra -F <target-IP> <service>
```

S'arrête après avoir trouvé un credential valide par username.

---

# 5. Output and Logging

## Afficher une sortie verbose

```bash
hydra -v <target-IP> <service>
```

## Afficher une sortie très verbose

```bash
hydra -d <target-IP> <service>
```

## Sauvegarder la sortie dans un fichier

```bash
hydra -o output.txt <target-IP> <service>
```

## Sauvegarder dans un format spécifique

```bash
hydra -o output.txt -oN <target-IP> <service>
```

Formats :

- `-oN` — Texte normal.
- `-oJ` — JSON.
- `-oX` — XML.

Exemple :

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh -oJ results.json
```

## Afficher les credentials trouvés en temps réel

Hydra affiche les credentials trouvés automatiquement en mode verbose.

---

# 6. Protocol-Specific Options

## SSH

```bash
hydra -L users.txt -P passwords.txt <target-IP> ssh
```

Exemple :

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## FTP

```bash
hydra -L users.txt -P passwords.txt <target-IP> ftp
```

## HTTP Basic Auth

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-get
```

## HTTP Form-based Auth

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "<path>:<parameters>:<fail_string>"
```

Exemple :

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

- `/login.php` — Chemin de la page de login.
- `user=^USER^&pass=^PASS^` — Paramètres POST.
- `Invalid` — Chaîne indiquant un échec.

## HTTPS

```bash
hydra -L users.txt -P passwords.txt <target-IP> https-get
```

## SMB

```bash
hydra -L users.txt -P passwords.txt <target-IP> smb
```

## MySQL

```bash
hydra -L users.txt -P passwords.txt <target-IP> mysql
```

## PostgreSQL

```bash
hydra -L users.txt -P passwords.txt <target-IP> postgres
```

## RDP

```bash
hydra -L users.txt -P passwords.txt <target-IP> rdp
```

## Telnet

```bash
hydra -L users.txt -P passwords.txt <target-IP> telnet
```

## SMTP

```bash
hydra -L users.txt -P passwords.txt <target-IP> smtp
```

## IMAP

```bash
hydra -L users.txt -P passwords.txt <target-IP> imap
```

## POP3

```bash
hydra -L users.txt -P passwords.txt <target-IP> pop3
```

## LDAP

```bash
hydra -L users.txt -P passwords.txt <target-IP> ldap
```

## VNC

```bash
hydra -L users.txt -P passwords.txt <target-IP> vnc
```

---

# 7. Advanced Options

## Utiliser un proxy

```bash
hydra -p <password> -P <passlist.txt> -X <proxy> <target-IP> <service>
```

Exemple :

```bash
hydra -L users.txt -P passwords.txt -X socks4://127.0.0.1:9050 192.168.1.10 ssh
```

## Utiliser SSL/TLS

Pour les services qui supportent SSL :

```bash
hydra -s <port> -S <target-IP> <service>
```

Exemple :

```bash
hydra -s 443 -S 192.168.1.10 https-get
```

## Spécifier des options de module

Certains modules supportent des options supplémentaires :

```bash
hydra -m <options> <target-IP> <service>
```

Exemple pour HTTP GET :

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-get -m /admin
```

## Reprendre une attaque précédente

```bash
hydra -r <session.restore>
```

Hydra crée automatiquement un fichier de session lorsqu'il est interrompu.

## Sauvegarder une session pour reprise ultérieure

Hydra sauvegarde la session automatiquement lors d'une interruption (Ctrl+C).

## Ignorer une session existante

```bash
hydra -I <target-IP> <service>
```

Ignore tout fichier de restore existant.

---

# 8. Common Attack Scenarios

## SSH brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## SSH avec un seul username

```bash
hydra -l admin -P passwords.txt 192.168.1.10 ssh
```

## SSH avec port personnalisé

```bash
hydra -s 2222 -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## FTP brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ftp
```

## HTTP Basic Auth brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-get
```

## HTTP Form-based login

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## SMB brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 smb
```

## MySQL brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 mysql
```

## RDP brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 rdp
```

## Telnet brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 telnet
```

## SMTP brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 smtp
```

## VNC brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 vnc
```

---

# 9. HTTP Form Attacks

## HTTP POST form avec paramètres personnalisés

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "<path>:<parameters>:<fail_string>"
```

Exemple :

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:username=^USER^&password=^PASS^:Login failed"
```

## HTTP POST form avec cookies

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "<path>:<parameters>:<fail_string>:<headers>"
```

Exemple :

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid:Cookie: session=abc123"
```

## HTTP GET form

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-get-form "<path>:<parameters>:<fail_string>"
```

Exemple :

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-get-form "/login.php?user=^USER^&pass=^PASS^:Invalid"
```

## HTTP form avec chaîne de succès

Utilisez `F=` pour la chaîne d'échec ou `S=` pour la chaîne de succès :

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "/login.php:user=^USER^&pass=^PASS^:S=Welcome"
```

## HTTP form avec plusieurs conditions d'échec

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "/login.php:user=^USER^&pass=^PASS^:F=Invalid:F=Error"
```

---

# 10. Practical Workflows

## Flux de travail de base SSH brute force

```text
1. Préparer la liste de usernames (users.txt).
2. Préparer la liste de mots de passe (passwords.txt).
3. Exécuter Hydra contre SSH.
4. Examiner la sortie pour les credentials valides.
5. Valider les credentials manuellement.
6. Documenter les résultats.
```

## Exemple : Attaque SSH complète

```bash
hydra -L users.txt -P passwords.txt -vV 192.168.1.10 ssh
```

- `-vV` — Sortie très verbose.

## Exemple : Attaque HTTP form

```bash
hydra -L users.txt -P passwords.txt -vV 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## Exemple : Plusieurs cibles

```bash
hydra -M targets.txt -L users.txt -P passwords.txt ssh
```

Où `targets.txt` contient :

```text
192.168.1.10
192.168.1.11
192.168.1.12
```

## Exemple : Arrêter après le premier succès

```bash
hydra -L users.txt -P passwords.txt -f 192.168.1.10 ssh
```

## Exemple : Port personnalisé et timeout

```bash
hydra -s 2222 -w 30 -L users.txt -P passwords.txt 192.168.1.10 ssh
```

---

# 11. Common Commands Reference

| Command | Description |
|---|---|
| `hydra -h` | Afficher l'aide |
| `hydra -V` | Afficher la version |
| `hydra -l <user>` | Spécifier un seul username |
| `hydra -L <file>` | Spécifier une liste de usernames |
| `hydra -p <pass>` | Spécifier un seul mot de passe |
| `hydra -P <file>` | Spécifier une liste de mots de passe |
| `hydra -C <file>` | Spécifier un fichier combo (user:pass) |
| `hydra -t <tasks>` | Définir le nombre de tâches parallèles |
| `hydra -s <port>` | Spécifier un port cible |
| `hydra -M <file>` | Spécifier une liste de cibles |
| `hydra -o <file>` | Sauvegarder la sortie dans un fichier |
| `hydra -v` | Sortie verbose |
| `hydra -V` | Sortie très verbose |
| `hydra -f` | Quitter après le premier credential valide |
| `hydra -F` | Quitter après un credential valide par utilisateur |
| `hydra -w <seconds>` | Définir le timeout de connexion |
| `hydra -r <retries>` | Définir le nombre maximum de tentatives |
| `hydra -d <delay>` | Définir le délai entre les tentatives |
| `hydra -I` | Ignorer la session existante |
| `hydra -X <proxy>` | Utiliser un proxy |
| `hydra -S` | Utiliser SSL |
| `hydra -m <options>` | Options spécifiques au module |

---

# 12. Wordlist Tips

## Emplacements courants de wordlists

- `/usr/share/wordlists/`
- `/usr/share/seclists/`
- `rockyou.txt`
- `common-passwords.txt`

## Créer une wordlist personnalisée

```bash
echo "password123" >> passwords.txt
echo "admin123" >> passwords.txt
```

## Utiliser plusieurs wordlists

```bash
cat list1.txt list2.txt > combined.txt
```

## Supprimer les doublons

```bash
sort passwords.txt | uniq > passwords_unique.txt
```

## Générer des variations

Utilisez des outils comme `crunch` pour générer des wordlists personnalisées :

```bash
crunch 8 12 -t password@@@ -o generated.txt
```

---

# 13. Common Exploits and Scenarios

## SSH avec credentials par défaut

```bash
hydra -l root -P passwords.txt 192.168.1.10 ssh
```

## FTP login anonyme

```bash
hydra -l anonymous -p anonymous 192.168.1.10 ftp
```

## HTTP admin panel

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/admin/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## MySQL accès root

```bash
hydra -l root -P passwords.txt 192.168.1.10 mysql
```

## SMB accès guest

```bash
hydra -l guest -P passwords.txt 192.168.1.10 smb
```

## RDP administrator

```bash
hydra -l Administrator -P passwords.txt 192.168.1.10 rdp
```

## Telnet root

```bash
hydra -l root -P passwords.txt 192.168.1.10 telnet
```

## SMTP authentication

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 smtp
```

---

# 14. Troubleshooting

## Connection refused

- Vérifiez si le service est en cours d'exécution.
- Vérifiez le numéro de port.
- Assurez-vous que le firewall autorise les connexions.

## Too many errors

- Réduisez les tâches parallèles : `-t 4`
- Augmentez le timeout : `-w 30`
- Ajoutez un délai : `-d 1`

## No valid credentials found

- Essayez une wordlist différente.
- Vérifiez si le service nécessite des paramètres spéciaux.
- Vérifiez la chaîne d'échec pour les formulaires HTTP.

## Service not supported

- Vérifiez les protocoles supportés : `hydra -h`
- Certains services peuvent nécessiter des modules spécifiques.

## Session restore issues

- Ignorez la session existante : `hydra -I`
- Supprimez le fichier de restore manuellement.

---

# 15. Important Reminders

- Obtenez toujours une autorisation explicite avant d'utiliser Hydra.
- Testez d'abord dans un environnement de laboratoire contrôlé.
- Le brute-forcing peut verrouiller des comptes ou déclencher des alertes.
- Respectez les limites de taux et les politiques de verrouillage de compte.
- Certains services peuvent détecter et bloquer les tentatives de brute-force.
- Documentez toutes les actions, commandes et résultats.
- Validez les résultats manuellement ; ne vous fiez pas uniquement aux résultats automatisés.
- Préservez les preuves originales et les logs.
- Respectez le périmètre et les règles d'engagement.
- Comprenez les implications légales et éthiques de vos actions.

---

# 16. Quick Reference Examples

## SSH brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## HTTP form attack

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## FTP avec un seul utilisateur

```bash
hydra -l admin -P passwords.txt 192.168.1.10 ftp
```

## Plusieurs cibles

```bash
hydra -M targets.txt -L users.txt -P passwords.txt ssh
```

## Arrêter après le premier succès

```bash
hydra -L users.txt -P passwords.txt -f 192.168.1.10 ssh
```

## Port personnalisé et verbose

```bash
hydra -s 2222 -vV -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## Sauvegarder les résultats en JSON

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh -oJ results.json
```

## Utiliser un proxy

```bash
hydra -L users.txt -P passwords.txt -X socks4://127.0.0.1:9050 192.168.1.10 ssh
```

---

# 17. Supported Protocols (Partial List)

- `ssh`
- `ftp`
- `http-get`
- `http-post-form`
- `https-get`
- `https-post-form`
- `smb`
- `mysql`
- `postgres`
- `rdp`
- `telnet`
- `smtp`
- `imap`
- `pop3`
- `ldap`
- `vnc`
- `snmp`
- `cisco`
- `oracle`
- `mssql`

Pour une liste complète :

```bash
hydra -h
```
