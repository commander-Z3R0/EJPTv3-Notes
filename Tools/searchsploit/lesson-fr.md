# Searchsploit Cheat Sheet

## Overview

Searchsploit est un outil de recherche en ligne de commande pour Exploit-DB, utilisé pour :

- Rechercher des exploits, shellcodes et papers.
- Trouver des vulnérabilités par CVE, EDB-ID ou mot-clé.
- Copier des exploits dans le répertoire actuel.
- Afficher des informations détaillées sur les exploits.
- Réaliser des évaluations de sécurité autorisées.

Searchsploit fournit :

- Un accès offline au repository Exploit-DB.
- Des capacités de recherche rapides.
- L'intégration avec Metasploit.
- Le support de multiples filtres de recherche.
- La capacité de copier et visualiser le code source des exploits.

```text
Utilisez Searchsploit uniquement pour la recherche et contre des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Starting Searchsploit

## Syntaxe de base

```bash
searchsploit [options] <search-term>
```

## Afficher l'aide

```bash
searchsploit -h
```

## Afficher la version

```bash
searchsploit -V
```

## Afficher la version verbose

```bash
searchsploit -v
```

## Mettre à jour la base de données

```bash
searchsploit -u
```

Met à jour le repository local Exploit-DB.

---

# 2. Basic Search

## Rechercher par mot-clé

```bash
searchsploit <keyword>
```

Exemple :

```bash
searchsploit apache
```

## Rechercher par CVE

```bash
searchsploit <CVE>
```

Exemple :

```bash
searchsploit CVE-2017-0144
```

## Rechercher par EDB-ID

```bash
searchsploit <EDB-ID>
```

Exemple :

```bash
searchsploit 41937
```

## Rechercher par auteur

```bash
searchsploit --author <author-name>
```

Exemple :

```bash
searchsploit --author metasploit
```

## Rechercher par type

Les types incluent :

- `exploit`
- `shellcode`
- `papers`
- `webapps`
- `platform`
- `local`
- `remote`

```bash
searchsploit --type <type>
```

Exemple :

```bash
searchsploit --type exploit apache
```

## Rechercher par plateforme

```bash
searchsploit --platform <platform>
```

Exemple :

```bash
searchsploit --platform linux
```

## Rechercher par port

```bash
searchsploit --port <port>
```

Exemple :

```bash
searchsploit --port 445
```

## Combiner plusieurs filtres

```bash
searchsploit --type exploit --platform windows --port 445 smb
```

---

# 3. Advanced Search Options

## Recherche case-insensitive

```bash
searchsploit -i <keyword>
```

Exemple :

```bash
searchsploit -i apache
```

## Recherche de correspondance exacte

```bash
searchsploit -e <keyword>
```

Exemple :

```bash
searchsploit -e "apache 2.4.49"
```

## Afficher uniquement le titre

```bash
searchsploit -t <keyword>
```

Exemple :

```bash
searchsploit -t apache
```

## Afficher uniquement le chemin

```bash
searchsploit -p <keyword>
```

Exemple :

```bash
searchsploit -p apache
```

## Afficher uniquement l'EDB-ID

```bash
searchsploit -n <keyword>
```

Exemple :

```bash
searchsploit -n apache
```

## Rechercher dans la description

```bash
searchsploit --search <keyword>
```

Exemple :

```bash
searchsploit --search "remote code execution"
```

## Lister toutes les plateformes disponibles

```bash
searchsploit --platforms
```

## Lister tous les types disponibles

```bash
searchsploit --types
```

---

# 4. Viewing Exploit Details

## Afficher des informations détaillées

```bash
searchsploit -x <EDB-ID>
```

Exemple :

```bash
searchsploit -x 41937
```

## Afficher des informations détaillées par CVE

```bash
searchsploit -x <CVE>
```

Exemple :

```bash
searchsploit -x CVE-2017-0144
```

## Visualiser le code source de l'exploit

```bash
searchsploit -x <EDB-ID>
```

Cela affiche le code source complet de l'exploit.

## Afficher uniquement le chemin vers l'exploit

```bash
searchsploit -p <keyword>
```

Exemple :

```bash
searchsploit -p eternalblue
```

---

# 5. Copying Exploits

## Copier l'exploit dans le répertoire actuel

```bash
searchsploit -m <EDB-ID>
```

Exemple :

```bash
searchsploit -m 41937
```

## Copier l'exploit dans un répertoire spécifique

```bash
searchsploit -m <EDB-ID> -o /path/to/output
```

Exemple :

```bash
searchsploit -m 41937 -o /tmp/exploits
```

## Copier plusieurs exploits

```bash
searchsploit -m <EDB-ID-1> <EDB-ID-2> <EDB-ID-3>
```

Exemple :

```bash
searchsploit -m 41937 42000 42050
```

## Copier tous les exploits d'une recherche

```bash
searchsploit -m <search-term>
```

Exemple :

```bash
searchsploit -m eternalblue
```

Copie tous les exploits correspondants dans le répertoire actuel.

---

# 6. Metasploit Integration

## Rechercher des modules Metasploit

```bash
searchsploit --nmap <nmap-output.xml>
```

Analyse la sortie Nmap et suggère des modules Metasploit.

## Rechercher par nom de module Metasploit

```bash
searchsploit <module-name>
```

Exemple :

```bash
searchsploit ms17_010_eternalblue
```

## Afficher le chemin du module Metasploit

```bash
searchsploit -p <module-name>
```

Exemple :

```bash
searchsploit -p ms17_010_eternalblue
```

## Vérifier si l'exploit est disponible dans Metasploit

Searchsploit indique si un exploit est disponible dans Metasploit dans la vue détaillée.

---

# 7. Common Search Scenarios

## Rechercher des exploits Apache

```bash
searchsploit apache
```

## Rechercher des exploits du kernel Linux

```bash
searchsploit linux kernel
```

## Rechercher des exploits Windows SMB

```bash
searchsploit windows smb
```

## Rechercher EternalBlue

```bash
searchsploit eternalblue
```

## Rechercher par CVE-2017-0144

```bash
searchsploit CVE-2017-0144
```

## Rechercher du shellcode

```bash
searchsploit --type shellcode
```

## Rechercher des exploits d'applications web

```bash
searchsploit --type webapps
```

## Rechercher des escalades de privilèges locales

```bash
searchsploit --type local
```

## Rechercher des exploits distants

```bash
searchsploit --type remote
```

## Rechercher des exploits sur le port 445

```bash
searchsploit --port 445
```

## Rechercher des exploits WordPress

```bash
searchsploit wordpress
```

## Rechercher des exploits SSH

```bash
searchsploit ssh
```

## Rechercher des exploits FTP

```bash
searchsploit ftp
```

## Rechercher des exploits MySQL

```bash
searchsploit mysql
```

---

# 8. Practical Workflows

## Flux de travail de base de recherche de vulnérabilités

```text
1. Identifier le service ou logiciel cible.
2. Rechercher des exploits avec Searchsploit.
3. Examiner les détails et exigences de l'exploit.
4. Copier l'exploit dans votre répertoire de travail.
5. Analyser et modifier l'exploit si nécessaire.
6. Tester dans un environnement de laboratoire contrôlé.
7. Documenter les résultats.
```

## Exemple : Recherche de vulnérabilité Apache

```bash
# Rechercher des exploits Apache
searchsploit apache

# Afficher des informations détaillées
searchsploit -x 41937

# Copier l'exploit dans le répertoire actuel
searchsploit -m 41937
```

## Exemple : Recherche basée sur CVE

```bash
# Rechercher par CVE
searchsploit CVE-2017-0144

# Afficher les détails
searchsploit -x CVE-2017-0144

# Copier l'exploit
searchsploit -m 41937
```

## Exemple : Recherche spécifique à une plateforme

```bash
# Rechercher des exploits Linux
searchsploit --platform linux

# Rechercher des exploits Windows
searchsploit --platform windows
```

## Exemple : Recherche spécifique à un type

```bash
# Rechercher du shellcode
searchsploit --type shellcode

# Rechercher des webapps
searchsploit --type webapps
```

## Exemple : Intégration Nmap

```bash
# Analyser la sortie Nmap
searchsploit --nmap scan.xml
```

Cela suggère des exploits pertinents basés sur les services découverts.

---

# 9. Common Commands Reference

| Command | Description |
|---|---|
| `searchsploit -h` | Afficher l'aide |
| `searchsploit -V` | Afficher la version |
| `searchsploit -v` | Afficher la version verbose |
| `searchsploit -u` | Mettre à jour la base de données |
| `searchsploit <keyword>` | Rechercher par mot-clé |
| `searchsploit -i <keyword>` | Recherche case-insensitive |
| `searchsploit -e <keyword>` | Recherche de correspondance exacte |
| `searchsploit -t <keyword>` | Afficher uniquement les titres |
| `searchsploit -p <keyword>` | Afficher uniquement les chemins |
| `searchsploit -n <keyword>` | Afficher uniquement les EDB-IDs |
| `searchsploit -x <EDB-ID>` | Afficher des informations détaillées |
| `searchsploit -m <EDB-ID>` | Copier l'exploit dans le répertoire actuel |
| `searchsploit --author <name>` | Rechercher par auteur |
| `searchsploit --type <type>` | Rechercher par type |
| `searchsploit --platform <platform>` | Rechercher par plateforme |
| `searchsploit --port <port>` | Rechercher par port |
| `searchsploit --nmap <file>` | Analyser la sortie Nmap |
| `searchsploit --platforms` | Lister toutes les plateformes |
| `searchsploit --types` | Lister tous les types |
| `searchsploit --search <term>` | Rechercher dans la description |

---

# 10. Advanced Usage

## Rechercher avec plusieurs filtres

```bash
searchsploit --type exploit --platform linux --port 22 ssh
```

## Rechercher des papers et documentation

```bash
searchsploit --type papers
```

## Rechercher uniquement des exploits locaux

```bash
searchsploit --type local
```

## Rechercher uniquement des exploits distants

```bash
searchsploit --type remote
```

## Rechercher par version spécifique de logiciel

```bash
searchsploit "apache 2.4.49"
```

## Rechercher des exploits par année

Inclure l'année dans le terme de recherche :

```bash
searchsploit "2021"
```

## Rechercher pour du 0-day

```bash
searchsploit --type exploit --platform linux
```

Examiner les exploits récents pour des patterns potentiels de 0-day.

## Combiner la recherche avec grep

```bash
searchsploit apache | grep "2.4"
```

## Exporter les résultats de recherche

```bash
searchsploit apache > results.txt
```

---

# 11. Exploit-DB Integration

## Mettre à jour la base de données Exploit-DB

```bash
searchsploit -u
```

Cela télécharge les derniers exploits d'Exploit-DB.

## Vérifier le statut de la base de données

```bash
searchsploit -v
```

Affiche la version de la base de données et la dernière mise à jour.

## Mettre à jour manuellement Exploit-DB

```bash
cd /usr/share/exploitdb
git pull
```

## Rechercher dans Exploit-DB en ligne

Visitez :

```text
https://www.exploit-db.com/
```

Pour la recherche web et des fonctionnalités supplémentaires.

---

# 12. Common Exploits and Scenarios

## MS17-010 EternalBlue

```bash
searchsploit eternalblue
searchsploit -x 41937
searchsploit -m 41937
```

## Apache Struts RCE

```bash
searchsploit apache struts
searchsploit -x <EDB-ID>
searchsploit -m <EDB-ID>
```

## Escalade de privilèges du kernel Linux

```bash
searchsploit linux kernel privilege escalation
searchsploit --type local --platform linux
```

## Exploits de plugins WordPress

```bash
searchsploit wordpress plugin
searchsploit --type webapps wordpress
```

## Vulnérabilités SSH

```bash
searchsploit ssh
searchsploit --port 22 ssh
```

## Vulnérabilités FTP

```bash
searchsploit ftp
searchsploit --port 21 ftp
```

## Vulnérabilités SMB

```bash
searchsploit smb
searchsploit --port 445 smb
```

## Vulnérabilités MySQL

```bash
searchsploit mysql
searchsploit --port 3306 mysql
```

## Vulnérabilités RDP

```bash
searchsploit rdp
searchsploit --port 3389 rdp
```

## Exploits d'applications web

```bash
searchsploit --type webapps
searchsploit "sql injection"
searchsploit "xss"
```

---

# 13. Troubleshooting

## Aucun résultat trouvé

- Essayez différents mots-clés.
- Utilisez la recherche case-insensitive : `-i`
- Recherchez directement par CVE ou EDB-ID.
- Mettez à jour la base de données : `searchsploit -u`

## L'exploit ne fonctionne pas

- Vérifiez les exigences et la plateforme.
- Assurez-vous que la version cible correspond.
- Examinez le code source de l'exploit.
- Testez dans un environnement de laboratoire contrôlé.

## Base de données obsolète

```bash
searchsploit -u
```

## Permission denied

Exécutez avec les permissions appropriées :

```bash
sudo searchsploit -m <EDB-ID>
```

## Chemin de l'exploit introuvable

Vérifiez que l'exploit existe :

```bash
searchsploit -p <keyword>
```

---

# 14. Security Best Practices

## Vérifiez toujours le code de l'exploit

- Examinez le code source avant d'exécuter.
- Vérifiez la présence de payloads malveillants.
- Comprenez ce que fait l'exploit.
- Testez dans un environnement isolé.

## Respectez les limites légales

- Utilisez des exploits uniquement sur des systèmes que vous possédez.
- Obtenez une autorisation explicite pour les tests.
- Suivez des pratiques de divulgation responsable.
- Documentez toutes les activités de recherche.

## Maintenez un environnement de laboratoire

- Utilisez des machines virtuelles pour les tests.
- Isolez les réseaux de test.
- Conservez des snapshots d'états propres.
- Documentez les configurations et résultats.

## Maintenez les outils à jour

- Mettez régulièrement à jour Searchsploit.
- Mettez à jour la base de données Exploit-DB.
- Restez informé des nouvelles vulnérabilités.
- Suivez les actualités et advisories de sécurité.

---

# 15. Important Reminders

- Obtenez toujours une autorisation explicite avant d'utiliser des exploits.
- Testez les exploits d'abord dans un environnement de laboratoire contrôlé.
- Tous les exploits ne sont pas fiables ; vérifiez avant utilisation.
- Certains exploits peuvent faire planter des services ou des systèmes.
- Maintenez Searchsploit et Exploit-DB à jour régulièrement.
- Validez les résultats manuellement ; ne vous fiez pas uniquement aux résultats automatisés.
- Documentez toutes les actions, commandes et résultats.
- Préservez les preuves originales et les logs.
- Respectez le périmètre et les règles d'engagement.
- Comprenez les implications légales et éthiques de vos actions.

---

# 16. Quick Reference Examples

## Recherche de base

```bash
searchsploit apache
```

## Rechercher par CVE

```bash
searchsploit CVE-2017-0144
```

## Afficher les détails de l'exploit

```bash
searchsploit -x 41937
```

## Copier l'exploit

```bash
searchsploit -m 41937
```

## Rechercher des exploits Linux

```bash
searchsploit --platform linux
```

## Rechercher du shellcode

```bash
searchsploit --type shellcode
```

## Mettre à jour la base de données

```bash
searchsploit -u
```

## Recherche case-insensitive

```bash
searchsploit -i apache
```

## Recherche de correspondance exacte

```bash
searchsploit -e "apache 2.4.49"
```

## Rechercher par auteur

```bash
searchsploit --author metasploit
```

## Analyser la sortie Nmap

```bash
searchsploit --nmap scan.xml
```

## Rechercher avec plusieurs filtres

```bash
searchsploit --type exploit --platform windows --port 445 smb
```

---

# 17. Supported Search Filters

## Par type

- `exploit`
- `shellcode`
- `papers`
- `webapps`
- `platform`
- `local`
- `remote`

## Par plateforme

- `linux`
- `windows`
- `macos`
- `bsd`
- `android`
- `ios`
- `hardware`
- `php`
- `python`
- `ruby`
- `java`
- Et bien d'autres...

## Par port

Tout numéro de port valide :

- `21` (FTP)
- `22` (SSH)
- `80` (HTTP)
- `443` (HTTPS)
- `445` (SMB)
- `3306` (MySQL)
- `3389` (RDP)
- Et bien d'autres...

## Par CVE

Tout identifiant CVE valide :

- `CVE-2017-0144`
- `CVE-2021-44228`
- `CVE-2020-1472`

## Par EDB-ID

Tout ID Exploit-DB valide :

- `41937`
- `42000`
- `50000`

---

# 18. Additional Resources

## Exploit-DB

```text
https://www.exploit-db.com/
```

## Offensive Security

```text
https://www.offensive-security.com/
```

## Metasploit Framework

```text
https://www.metasploit.com/
```

## CVE Database

```text
https://cve.mitre.org/
```

## National Vulnerability Database

```text
https://nvd.nist.gov/
```
