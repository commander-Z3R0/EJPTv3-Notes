# Enum4Linux Cheat Sheet

## Overview

Enum4Linux est un outil pour énumérer des informations depuis des systèmes Windows et Samba. Il est utilisé pour :

- Énumérer les utilisateurs et groupes.
- Lister les partages et permissions.
- Recueillir des informations système.
- Identifier les mauvaises configurations de sécurité.
- Réaliser des évaluations de sécurité autorisées.

Enum4Linux fournit :

- L'énumération automatisée des services SMB/CIFS.
- L'énumération d'utilisateurs et de groupes.
- L'énumération de partages et le test d'accès.
- L'extraction de politiques de mots de passe.
- L'intégration avec d'autres outils de pentesting.

```text
Utilisez Enum4Linux uniquement contre des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Starting Enum4Linux

## Syntaxe de base

```bash
enum4linux [options] <target>
```

## Afficher l'aide

```bash
enum4linux -h
```

## Afficher la version

```bash
enum4linux -V
```

## Mettre à jour Enum4Linux

```bash
git clone https://github.com/CiscoCXSecurity/enum4linux.git
cd enum4linux
```

---

# 2. Basic Enumeration

## Énumérer toutes les informations

```bash
enum4linux <target>
```

Exemple :

```bash
enum4linux 192.168.1.10
```

## Énumérer avec sortie verbose

```bash
enum4linux -v <target>
```

Exemple :

```bash
enum4linux -v 192.168.1.10
```

## Énumérer avec sortie lisible par machine

```bash
enum4linux -M <target>
```

Sortie dans un format adapté au parsing.

## Énumérer une cible spécifique

```bash
enum4linux 192.168.1.10
```

## Énumérer avec un hostname

```bash
enum4linux hostname.local
```

---

# 3. User Enumeration

## Énumérer tous les utilisateurs

```bash
enum4linux -U <target>
```

Exemple :

```bash
enum4linux -U 192.168.1.10
```

## Énumérer les utilisateurs avec RID cycling

```bash
enum4linux -U -r <target>
```

Le RID cycling tente d'énumérer les utilisateurs en itérant à travers les RIDs.

## Énumérer les utilisateurs avec une plage RID spécifique

```bash
enum4linux -U -r <start-end> <target>
```

Exemple :

```bash
enum4linux -U -r 500-550 192.168.1.10
```

## Lister les utilisateurs avec descriptions

```bash
enum4linux -U <target>
```

Inclut les descriptions d'utilisateurs quand disponibles.

## Énumérer les utilisateurs avec authentification

```bash
enum4linux -U -u <username> -p <password> <target>
```

Exemple :

```bash
enum4linux -U -u guest -p '' 192.168.1.10
```

---

# 4. Group Enumeration

## Énumérer tous les groupes

```bash
enum4linux -G <target>
```

Exemple :

```bash
enum4linux -G 192.168.1.10
```

## Énumérer les groupes avec membres

```bash
enum4linux -G <target>
```

Inclut les informations d'appartenance aux groupes.

## Énumérer un groupe spécifique

```bash
enum4linux -G -g <groupname> <target>
```

Exemple :

```bash
enum4linux -G -g "Domain Admins" 192.168.1.10
```

## Lister les SIDs de groupes

```bash
enum4linux -G <target>
```

Affiche les Security Identifiers pour les groupes.

---

# 5. Share Enumeration

## Énumérer tous les partages

```bash
enum4linux -S <target>
```

Exemple :

```bash
enum4linux -S 192.168.1.10
```

## Énumérer les partages avec permissions

```bash
enum4linux -S <target>
```

Affiche les permissions de partage et les droits d'accès.

## Lister les partages accessibles

```bash
enum4linux -S <target>
```

Identifie les partages auxquels vous pouvez accéder.

## Énumérer les partages avec authentification

```bash
enum4linux -S -u <username> -p <password> <target>
```

Exemple :

```bash
enum4linux -S -u guest -p '' 192.168.1.10
```

## Vérifier les partages null session

```bash
enum4linux -N <target>
```

Teste les partages accessibles avec des null sessions.

---

# 6. System Information

## Obtenir les informations du système d'exploitation

```bash
enum4linux -o <target>
```

Exemple :

```bash
enum4linux -o 192.168.1.10
```

## Obtenir les informations du serveur

```bash
enum4linux -S <target>
```

Inclut la version du serveur et les informations de domaine.

## Obtenir les informations du domaine

```bash
enum4linux -D <target>
```

Exemple :

```bash
enum4linux -D 192.168.1.10
```

## Obtenir les informations de workstation

```bash
enum4linux -W <target>
```

## Obtenir la politique de mots de passe

```bash
enum4linux -P <target>
```

Exemple :

```bash
enum4linux -P 192.168.1.10
```

Affiche les paramètres de complexité, longueur et expiration des mots de passe.

---

# 7. Advanced Enumeration

## Énumérer tout

```bash
enum4linux -a <target>
```

Exemple :

```bash
enum4linux -a 192.168.1.10
```

Effectue tous les checks d'énumération.

## Énumérer avec RID cycling

```bash
enum4linux -r <target>
```

Exemple :

```bash
enum4linux -r 192.168.1.10
```

Tente d'énumérer les utilisateurs via RID cycling.

## Énumérer avec une plage RID spécifique

```bash
enum4linux -r <start-end> <target>
```

Exemple :

```bash
enum4linux -r 1000-1100 192.168.1.10
```

## Énumérer les imprimantes

```bash
enum4linux -p <target>
```

Exemple :

```bash
enum4linux -p 192.168.1.10
```

## Énumérer les partages et utilisateurs

```bash
enum4linux -S -U <target>
```

Combine l'énumération de partages et d'utilisateurs.

---

# 8. Authentication Options

## Authentifier avec username et mot de passe

```bash
enum4linux -u <username> -p <password> <target>
```

Exemple :

```bash
enum4linux -u admin -p password123 192.168.1.10
```

## Authentifier avec username uniquement

```bash
enum4linux -u <username> -p '' <target>
```

Exemple :

```bash
enum4linux -u guest -p '' 192.168.1.10
```

## Utiliser une null session

```bash
enum4linux -N <target>
```

Tente de se connecter avec une null session.

## Utiliser l'authentification de domaine

```bash
enum4linux -u <username> -p <password> -d <domain> <target>
```

Exemple :

```bash
enum4linux -u user -p pass -d DOMAIN 192.168.1.10
```

## Utiliser l'authentification par hash

```bash
enum4linux -u <username> -H <hash> <target>
```

Exemple :

```bash
enum4linux -u admin -H aad3b435b51404eeaad3b435b51404ee:hash 192.168.1.10
```

---

# 9. Output Options

## Sauvegarder la sortie dans un fichier

```bash
enum4linux <target> > output.txt
```

Exemple :

```bash
enum4linux -a 192.168.1.10 > enum_results.txt
```

## Sortie verbose

```bash
enum4linux -v <target>
```

Affiche la progression détaillée de l'énumération.

## Sortie lisible par machine

```bash
enum4linux -M <target>
```

Format adapté au parsing par des scripts.

## Mode quiet

```bash
enum4linux -q <target>
```

Minimise la verbosité de la sortie.

## Mode debug

```bash
enum4linux -d <target>
```

Affiche les informations debug pour le troubleshooting.

---

# 10. Common Attack Scenarios

## Énumération de base

```bash
enum4linux 192.168.1.10
```

## Énumération complète

```bash
enum4linux -a 192.168.1.10
```

## Énumération d'utilisateurs uniquement

```bash
enum4linux -U 192.168.1.10
```

## Énumération de partages uniquement

```bash
enum4linux -S 192.168.1.10
```

## Énumération de groupes uniquement

```bash
enum4linux -G 192.168.1.10
```

## RID cycling

```bash
enum4linux -r 192.168.1.10
```

## Check de politique de mots de passe

```bash
enum4linux -P 192.168.1.10
```

## Test de null session

```bash
enum4linux -N 192.168.1.10
```

## Énumération authentifiée

```bash
enum4linux -u guest -p '' 192.168.1.10
```

## Scan verbose complet

```bash
enum4linux -a -v 192.168.1.10
```

---

# 11. Practical Workflows

## Flux de travail de base d'énumération SMB

```text
1. Identifier la cible Windows/Samba.
2. Exécuter un scan de base avec enum4linux.
3. Énumérer les utilisateurs.
4. Énumérer les partages.
5. Vérifier la politique de mots de passe.
6. Documenter les résultats.
```

## Exemple : Énumération complète

```bash
# Scan de base
enum4linux 192.168.1.10

# Énumération complète
enum4linux -a 192.168.1.10

# Sortie verbose
enum4linux -a -v 192.168.1.10

# Sauvegarder les résultats
enum4linux -a 192.168.1.10 > results.txt
```

## Exemple : Énumération d'utilisateurs

```bash
# Énumérer les utilisateurs
enum4linux -U 192.168.1.10

# RID cycling
enum4linux -U -r 192.168.1.10

# Plage RID spécifique
enum4linux -U -r 500-550 192.168.1.10
```

## Exemple : Énumération de partages

```bash
# Énumérer les partages
enum4linux -S 192.168.1.10

# Vérifier les partages null session
enum4linux -N 192.168.1.10

# Énumération de partages authentifiée
enum4linux -S -u guest -p '' 192.168.1.10
```

## Exemple : Politique de mots de passe

```bash
# Obtenir la politique de mots de passe
enum4linux -P 192.168.1.10

# Scan complet avec politique
enum4linux -a -P 192.168.1.10
```

## Exemple : Attaque RID cycling

```bash
# RID cycling de base
enum4linux -r 192.168.1.10

# Plage spécifique
enum4linux -r 1000-1100 192.168.1.10

# Avec énumération d'utilisateurs
enum4linux -U -r 192.168.1.10
```

---

# 12. Common Commands Reference

| Command | Description |
|---|---|
| `enum4linux -h` | Afficher l'aide |
| `enum4linux -V` | Afficher la version |
| `enum4linux <target>` | Énumération de base |
| `enum4linux -a <target>` | Énumérer tout |
| `enum4linux -U <target>` | Énumérer les utilisateurs |
| `enum4linux -G <target>` | Énumérer les groupes |
| `enum4linux -S <target>` | Énumérer les partages |
| `enum4linux -P <target>` | Obtenir la politique de mots de passe |
| `enum4linux -o <target>` | Obtenir les informations du SE |
| `enum4linux -r <target>` | RID cycling |
| `enum4linux -N <target>` | Test de null session |
| `enum4linux -v <target>` | Sortie verbose |
| `enum4linux -M <target>` | Sortie lisible par machine |
| `enum4linux -u <user> -p <pass>` | Authentifier |
| `enum4linux -d <domain>` | Spécifier le domaine |
| `enum4linux -H <hash>` | Utiliser l'authentification par hash |
| `enum4linux -p <target>` | Énumérer les imprimantes |
| `enum4linux -W <target>` | Obtenir les informations de workstation |
| `enum4linux -D <target>` | Obtenir les informations du domaine |
| `enum4linux -q <target>` | Mode quiet |

---

# 13. Integration with Other Tools

## Utiliser avec Nmap

```bash
# Scan SMB avec Nmap
nmap -p 445 --script smb-enum-users 192.168.1.10

# Puis enum4linux
enum4linux -a 192.168.1.10
```

## Utiliser avec Metasploit

```bash
# Énumération SMB avec Metasploit
use auxiliary/scanner/smb/smb_enumusers
set RHOSTS 192.168.1.10
run

# Puis enum4linux
enum4linux -U 192.168.1.10
```

## Utiliser avec CrackMapExec

```bash
# Énumération SMB avec CrackMapExec
crackmapexec smb 192.168.1.10

# Puis enum4linux
enum4linux -a 192.168.1.10
```

## Utiliser avec SMBClient

```bash
# Lister les partages avec smbclient
smbclient -L //192.168.1.10

# Puis enum4linux
enum4linux -S 192.168.1.10
```

---

# 14. Troubleshooting

## Connection refused

- Vérifiez si le service SMB est en cours d'exécution.
- Vérifiez que le port 445 ou 139 est ouvert.
- Assurez-vous que la cible est accessible.
- Vérifiez les règles du firewall.

## Access denied

- Essayez différents credentials.
- Utilisez une null session si autorisé.
- Vérifiez les permissions de l'utilisateur.
- Vérifiez la méthode d'authentification.

## Aucun utilisateur trouvé

- Le RID cycling peut être bloqué.
- Essayez une plage RID différente.
- Utilisez l'énumération authentifiée.
- Vérifiez si l'énumération d'utilisateurs est autorisée.

## Énumération lente

- Réduisez la plage RID.
- Utilisez des options d'énumération spécifiques.
- Vérifiez la latence réseau.
- Vérifiez la réactivité de la cible.

## Erreurs de timeout

- Augmentez les paramètres de timeout.
- Vérifiez la connectivité réseau.
- Assurez-vous que la cible est en ligne.
- Réduisez la portée de l'énumération.

---

# 15. Security Best Practices

## Vérifiez toujours les résultats

- Recoupez avec d'autres outils.
- Vérifiez les comptes utilisateurs manuellement.
- Vérifiez l'accès aux partages manuellement.
- Documentez tous les résultats.

## Respectez les limites légales

- Testez uniquement les systèmes que vous possédez.
- Obtenez une autorisation explicite.
- Suivez la divulgation responsable.
- Documentez toutes les activités.

## Minimisez l'impact

- Utilisez des options d'énumération appropriées.
- Évitez le RID cycling agressif.
- Testez pendant les fenêtres de maintenance.
- Surveillez le système cible.

## Maintenez les outils à jour

- Mettez régulièrement à jour enum4linux.
- Restez informé des nouvelles techniques.
- Suivez les advisories de sécurité.
- Testez dans des environnements contrôlés.

## Documentez tout

- Enregistrez les résultats d'énumération.
- Notez les partages accessibles.
- Suivez les comptes utilisateurs.
- Documentez les problèmes de sécurité.

---

# 16. Important Reminders

- Obtenez toujours une autorisation explicite avant d'utiliser Enum4Linux.
- Testez d'abord dans un environnement de laboratoire contrôlé.
- Toutes les informations énumérées n'indiquent pas des vulnérabilités.
- Certaines énumérations peuvent déclencher des alertes de sécurité.
- Maintenez Enum4Linux à jour régulièrement.
- Validez les résultats manuellement ; ne vous fiez pas uniquement aux résultats automatisés.
- Documentez toutes les actions, commandes et résultats.
- Préservez les preuves originales et les logs.
- Respectez le périmètre et les règles d'engagement.
- Comprenez les implications légales et éthiques de vos actions.

---

# 17. Quick Reference Examples

## Scan de base

```bash
enum4linux 192.168.1.10
```

## Énumération complète

```bash
enum4linux -a 192.168.1.10
```

## Énumération d'utilisateurs

```bash
enum4linux -U 192.168.1.10
```

## Énumération de partages

```bash
enum4linux -S 192.168.1.10
```

## Énumération de groupes

```bash
enum4linux -G 192.168.1.10
```

## RID cycling

```bash
enum4linux -r 192.168.1.10
```

## Politique de mots de passe

```bash
enum4linux -P 192.168.1.10
```

## Null session

```bash
enum4linux -N 192.168.1.10
```

## Authentifié

```bash
enum4linux -u guest -p '' 192.168.1.10
```

## Verbose

```bash
enum4linux -a -v 192.168.1.10
```

## Sauvegarder la sortie

```bash
enum4linux -a 192.168.1.10 > results.txt
```

---

# 18. Additional Resources

## Enum4Linux GitHub

```text
https://github.com/CiscoCXSecurity/enum4linux
```

## Documentation du protocole SMB

```text
https://docs.microsoft.com/en-us/openspecs/
```

## Documentation Samba

```text
https://www.samba.org/samba/docs/
```

## OWASP SMB Security

```text
https://owasp.org/www-project-web-security-testing-guide/
```

## Windows Security Baseline

```text
https://docs.microsoft.com/en-us/windows/security/
```
