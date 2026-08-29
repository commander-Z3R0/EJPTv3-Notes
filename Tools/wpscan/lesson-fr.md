# WPScan Cheat Sheet

## Overview

WPScan est un scanner de sécurité WordPress utilisé pour :

- Énumérer les installations WordPress.
- Identifier les plugins et thèmes vulnérables.
- Détecter les credentials faibles.
- Trouver des problèmes de configuration.
- Réaliser des évaluations de sécurité autorisées.

WPScan fournit :

- Une base de données complète de vulnérabilités WordPress.
- L'énumération de plugins et thèmes.
- Des capacités d'énumération d'utilisateurs.
- Du brute-force de mots de passe.
- L'intégration API pour les données de vulnérabilités.

```text
Utilisez WPScan uniquement contre des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Starting WPScan

## Syntaxe de base

```bash
wpscan [options] -u <URL>
```

## Afficher l'aide

```bash
wpscan -h
```

## Afficher la version

```bash
wpscan --version
```

## Mettre à jour WPScan

```bash
wpscan --update
```

Met à jour l'outil et la base de données de vulnérabilités.

## Mettre à jour uniquement la base de données de vulnérabilités

```bash
wpscan --update-only
```

---

# 2. Target Specification

## Spécifier l'URL cible

```bash
wpscan -u <URL>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/
```

## Spécifier l'URL cible avec port

```bash
wpscan -u http://192.168.1.10:8080/
```

## Spécifier l'URL cible avec chemin

```bash
wpscan -u http://192.168.1.10/wordpress/
```

## Scanner plusieurs URLs

```bash
wpscan -u <URL1> -u <URL2> -u <URL3>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ -u http://192.168.1.11/
```

## Scanner depuis un fichier

```bash
wpscan --url <file>
```

Où le fichier contient une URL par ligne.

---

# 3. Authentication

## Spécifier un username

```bash
wpscan -u <URL> --username <username>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --username admin
```

## Spécifier un mot de passe

```bash
wpscan -u <URL> --password <password>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --username admin --password password123
```

## Spécifier un cookie

```bash
wpscan -u <URL> --cookie <cookie>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --cookie "wordpress_logged_in=abc123"
```

## Spécifier un User-Agent

```bash
wpscan -u <URL> --user-agent <agent>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --user-agent "Mozilla/5.0"
```

## Spécifier un proxy

```bash
wpscan -u <URL> --proxy <proxy>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080
```

## Spécifier l'authentification du proxy

```bash
wpscan -u <URL> --proxy <proxy> --proxy-auth <credentials>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080 --proxy-auth user:pass
```

---

# 4. Enumeration Options

## Énumérer les plugins

```bash
wpscan -u <URL> --enumerate p
```

## Énumérer les thèmes

```bash
wpscan -u <URL> --enumerate t
```

## Énumérer les utilisateurs

```bash
wpscan -u <URL> --enumerate u
```

## Énumérer tout

```bash
wpscan -u <URL> --enumerate a
```

Énumère les plugins, thèmes, utilisateurs et plus.

## Énumérer uniquement les plugins vulnérables

```bash
wpscan -u <URL> --enumerate vp
```

## Énumérer uniquement les thèmes vulnérables

```bash
wpscan -u <URL> --enumerate vt
```

## Énumérer les plugins populaires

```bash
wpscan -u <URL> --enumerate ap
```

## Énumérer les thèmes populaires

```bash
wpscan -u <URL> --enumerate at
```

## Énumérer les timthumbs

```bash
wpscan -u <URL> --enumerate tt
```

## Énumérer les backups de configuration

```bash
wpscan -u <URL> --enumerate cb
```

## Énumérer les exports de base de données

```bash
wpscan -u <URL> --enumerate db
```

## Limiter les résultats d'énumération

```bash
wpscan -u <URL> --enumerate u[1-10]
```

Limite l'énumération des utilisateurs aux 10 premiers utilisateurs.

---

# 5. Vulnerability Detection

## Détecter toutes les vulnérabilités

```bash
wpscan -u <URL>
```

Effectue un scan complet de vulnérabilités.

## Détecter les plugins vulnérables

```bash
wpscan -u <URL> --enumerate vp
```

## Détecter les thèmes vulnérables

```bash
wpscan -u <URL> --enumerate vt
```

## Détecter les plugins et thèmes vulnérables

```bash
wpscan -u <URL> --enumerate vp,vt
```

## Forcer la détection de toutes les vulnérabilités

```bash
wpscan -u <URL> --force
```

## Ignorer la vérification de vulnérabilités

```bash
wpscan -u <URL> --no-vulnerability-check
```

---

# 6. Password Attacks

## Brute-force de mots de passe

```bash
wpscan -u <URL> --passwords <wordlist>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --passwords /usr/share/wordlists/rockyou.txt
```

## Brute-force d'un username spécifique

```bash
wpscan -u <URL> --username <username> --passwords <wordlist>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt
```

## Brute-force de plusieurs usernames

```bash
wpscan -u <URL> --usernames <userlist> --passwords <wordlist>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --usernames users.txt --passwords passwords.txt
```

## Limiter les tentatives de mot de passe

```bash
wpscan -u <URL> --passwords <wordlist> --max-threads <threads>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --passwords passwords.txt --max-threads 10
```

## Arrêter après le premier succès

```bash
wpscan -u <URL> --passwords <wordlist> --stop-on-success
```

---

# 7. API Integration

## Spécifier un token API

```bash
wpscan -u <URL> --api-token <token>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --api-token abc123xyz
```

## Utiliser un token API pour les données de vulnérabilités

```bash
wpscan -u <URL> --api-token <token> --enumerate vp
```

## Mettre à jour le token API

```bash
wpscan --update
```

## Vérifier le statut de l'API

```bash
wpscan --api-token <token>
```

---

# 8. Output and Logging

## Sauvegarder la sortie dans un fichier

```bash
wpscan -u <URL> -o <output-file>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ -o results.txt
```

## Sauvegarder au format JSON

```bash
wpscan -u <URL> -f json -o <output-file>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ -f json -o results.json
```

## Sauvegarder au format CSV

```bash
wpscan -u <URL> -f csv -o <output-file>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ -f csv -o results.csv
```

## Sortie verbose

```bash
wpscan -u <URL> -v
```

## Sortie très verbose

```bash
wpscan -u <URL> -vv
```

## Mode quiet

```bash
wpscan -u <URL> --quiet
```

## Pas de sortie couleur

```bash
wpscan -u <URL> --no-color
```

## Logger toutes les requêtes

```bash
wpscan -u <URL> --log <log-file>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --log scan.log
```

---

# 9. Advanced Options

## Spécifier le chemin d'installation WordPress

```bash
wpscan -u <URL> --wp-content-dir <path>
```

## Spécifier le chemin wp-includes

```bash
wpscan -u <URL> --wp-includes-dir <path>
```

## Spécifier le chemin des plugins

```bash
wpscan -u <URL> --plugins-dir <path>
```

## Spécifier le chemin des thèmes

```bash
wpscan -u <URL> --themes-dir <path>
```

## Forcer la détection WordPress

```bash
wpscan -u <URL> --force
```

## Ignorer la détection WordPress

```bash
wpscan -u <URL> --no-wp-content-dir-check
```

## Désactiver les vérifications TLS

```bash
wpscan -u <URL> --disable-tls-checks
```

## Spécifier le timeout

```bash
wpscan -u <URL> --timeout <seconds>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --timeout 30
```

## Spécifier le nombre maximum de threads

```bash
wpscan -u <URL> --max-threads <threads>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --max-threads 20
```

## Délai entre les requêtes

```bash
wpscan -u <URL> --throttle <milliseconds>
```

Exemple :

```bash
wpscan -u http://192.168.1.10/ --throttle 1000
```

---

# 10. Common Attack Scenarios

## Scan WordPress de base

```bash
wpscan -u http://192.168.1.10/
```

## Énumérer les plugins

```bash
wpscan -u http://192.168.1.10/ --enumerate p
```

## Énumérer les thèmes

```bash
wpscan -u http://192.168.1.10/ --enumerate t
```

## Énumérer les utilisateurs

```bash
wpscan -u http://192.168.1.10/ --enumerate u
```

## Énumérer tout

```bash
wpscan -u http://192.168.1.10/ --enumerate a
```

## Détecter les plugins vulnérables

```bash
wpscan -u http://192.168.1.10/ --enumerate vp
```

## Brute-force du mot de passe admin

```bash
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt
```

## Scanner avec un token API

```bash
wpscan -u http://192.168.1.10/ --api-token abc123xyz
```

## Sauvegarder les résultats en JSON

```bash
wpscan -u http://192.168.1.10/ -f json -o results.json
```

## Scanner avec un proxy

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080
```

## Scanner plusieurs cibles

```bash
wpscan -u http://192.168.1.10/ -u http://192.168.1.11/
```

## Scan verbose

```bash
wpscan -u http://192.168.1.10/ -v
```

---

# 11. Practical Workflows

## Flux de travail de base de scan de sécurité WordPress

```text
1. Identifier l'installation WordPress.
2. Exécuter WPScan contre la cible.
3. Énumérer les plugins et thèmes.
4. Vérifier les vulnérabilités.
5. Énumérer les utilisateurs.
6. Tester les credentials faibles.
7. Documenter les résultats.
```

## Exemple : Énumération complète

```bash
# Scan de base
wpscan -u http://192.168.1.10/

# Énumérer les plugins
wpscan -u http://192.168.1.10/ --enumerate p

# Énumérer les thèmes
wpscan -u http://192.168.1.10/ --enumerate t

# Énumérer les utilisateurs
wpscan -u http://192.168.1.10/ --enumerate u

# Énumérer tout
wpscan -u http://192.168.1.10/ --enumerate a
```

## Exemple : Détection de vulnérabilités

```bash
# Détecter les plugins vulnérables
wpscan -u http://192.168.1.10/ --enumerate vp

# Détecter les thèmes vulnérables
wpscan -u http://192.168.1.10/ --enumerate vt

# Scan complet de vulnérabilités
wpscan -u http://192.168.1.10/
```

## Exemple : Brute-force de mots de passe

```bash
# Brute-force admin
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt

# Brute-force plusieurs utilisateurs
wpscan -u http://192.168.1.10/ --usernames users.txt --passwords passwords.txt

# Arrêter en cas de succès
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt --stop-on-success
```

## Exemple : Intégration API

```bash
# Scanner avec un token API
wpscan -u http://192.168.1.10/ --api-token abc123xyz

# Énumérer les plugins vulnérables avec API
wpscan -u http://192.168.1.10/ --api-token abc123xyz --enumerate vp
```

## Exemple : Sortie et logging

```bash
# Sauvegarder dans un fichier texte
wpscan -u http://192.168.1.10/ -o results.txt

# Sauvegarder en JSON
wpscan -u http://192.168.1.10/ -f json -o results.json

# Sauvegarder en CSV
wpscan -u http://192.168.1.10/ -f csv -o results.csv

# Sortie verbose
wpscan -u http://192.168.1.10/ -v -o results.txt
```

---

# 12. Common Commands Reference

| Command | Description |
|---|---|
| `wpscan -h` | Afficher l'aide |
| `wpscan --version` | Afficher la version |
| `wpscan -u <URL>` | Spécifier l'URL cible |
| `wpscan --update` | Mettre à jour WPScan |
| `wpscan --enumerate p` | Énumérer les plugins |
| `wpscan --enumerate t` | Énumérer les thèmes |
| `wpscan --enumerate u` | Énumérer les utilisateurs |
| `wpscan --enumerate a` | Énumérer tout |
| `wpscan --enumerate vp` | Énumérer les plugins vulnérables |
| `wpscan --enumerate vt` | Énumérer les thèmes vulnérables |
| `wpscan --passwords <file>` | Spécifier une wordlist de mots de passe |
| `wpscan --username <user>` | Spécifier un username |
| `wpscan --usernames <file>` | Spécifier une liste de usernames |
| `wpscan --api-token <token>` | Spécifier un token API |
| `wpscan -o <file>` | Sauvegarder la sortie dans un fichier |
| `wpscan -f json` | Sortie au format JSON |
| `wpscan -f csv` | Sortie au format CSV |
| `wpscan -v` | Sortie verbose |
| `wpscan --proxy <proxy>` | Utiliser un proxy |
| `wpscan --cookie <cookie>` | Spécifier un cookie |
| `wpscan --user-agent <agent>` | Spécifier un User-Agent |
| `wpscan --timeout <seconds>` | Définir le timeout |
| `wpscan --max-threads <threads>` | Définir le nombre maximum de threads |
| `wpscan --force` | Forcer la détection WordPress |
| `wpscan --quiet` | Mode quiet |
| `wpscan --no-color` | Pas de sortie couleur |

---

# 13. Plugin Enumeration

## Lister tous les plugins

```bash
wpscan -u <URL> --enumerate p
```

## Lister les plugins vulnérables

```bash
wpscan -u <URL> --enumerate vp
```

## Lister les plugins populaires

```bash
wpscan -u <URL> --enumerate ap
```

## Lister les plugins avec une plage spécifique

```bash
wpscan -u <URL> --enumerate p[1-50]
```

## Détecter les versions de plugins

```bash
wpscan -u <URL> --enumerate p
```

WPScan détecte automatiquement les versions de plugins.

## Vérifier les vulnérabilités de plugins

```bash
wpscan -u <URL> --enumerate vp
```

Utilise la base de données de vulnérabilités WPScan.

---

# 14. Theme Enumeration

## Lister tous les thèmes

```bash
wpscan -u <URL> --enumerate t
```

## Lister les thèmes vulnérables

```bash
wpscan -u <URL> --enumerate vt
```

## Lister les thèmes populaires

```bash
wpscan -u <URL> --enumerate at
```

## Lister les thèmes avec une plage spécifique

```bash
wpscan -u <URL> --enumerate t[1-20]
```

## Détecter les versions de thèmes

```bash
wpscan -u <URL> --enumerate t
```

WPScan détecte automatiquement les versions de thèmes.

## Vérifier les vulnérabilités de thèmes

```bash
wpscan -u <URL> --enumerate vt
```

Utilise la base de données de vulnérabilités WPScan.

---

# 15. User Enumeration

## Lister tous les utilisateurs

```bash
wpscan -u <URL> --enumerate u
```

## Lister les utilisateurs avec une plage spécifique

```bash
wpscan -u <URL> --enumerate u[1-10]
```

## Lister uniquement les usernames

```bash
wpscan -u <URL> --enumerate u
```

## Détecter les rôles d'utilisateurs

```bash
wpscan -u <URL> --enumerate u
```

WPScan détecte automatiquement les rôles d'utilisateurs.

## Vérifier les mots de passe faibles

```bash
wpscan -u <URL> --username admin --passwords passwords.txt
```

## Brute-force de plusieurs utilisateurs

```bash
wpscan -u <URL> --usernames users.txt --passwords passwords.txt
```

---

# 16. Troubleshooting

## WordPress non détecté

- Vérifiez l'installation WordPress.
- Utilisez `--force` pour forcer la détection.
- Vérifiez si le site est vraiment WordPress.
- Vérifiez que l'URL est correcte.

## Aucun plugin trouvé

- Les plugins peuvent être cachés.
- Vérifiez le répertoire wp-content/plugins.
- Utilisez `--enumerate p` explicitement.
- Vérifiez les permissions.

## Erreurs API

- Vérifiez la validité du token API.
- Vérifiez la connectivité internet.
- Mettez à jour la base de données WPScan.
- Vérifiez les limites de débit de l'API.

## Performance lente

- Réduisez le nombre maximum de threads : `--max-threads 10`
- Ajoutez du throttle : `--throttle 1000`
- Utilisez un proxy pour l'anonymat.
- Limitez la plage d'énumération.

## Faux positifs

- Vérifiez les vulnérabilités manuellement.
- Vérifiez les versions de plugins/thèmes.
- Examinez la sortie WPScan attentivement.
- Recoupez avec d'autres sources.

---

# 17. Security Best Practices

## Vérifiez toujours les résultats

- Vérifiez les vulnérabilités manuellement.
- Vérifiez les versions de plugins/thèmes.
- Testez dans un environnement contrôlé.
- Documentez tous les résultats.

## Respectez les limites légales

- Testez uniquement les systèmes que vous possédez.
- Obtenez une autorisation explicite.
- Suivez la divulgation responsable.
- Documentez toutes les activités.

## Minimisez l'impact

- Utilisez des paramètres de throttle appropriés.
- Évitez les scans agressifs.
- Testez pendant les fenêtres de maintenance.
- Surveillez le système cible.

## Maintenez les outils à jour

- Mettez régulièrement à jour WPScan.
- Mettez à jour la base de données de vulnérabilités.
- Restez informé des nouvelles vulnérabilités.
- Suivez les advisories de sécurité.

## Utilisez l'API pour des résultats précis

- Inscrivez-vous pour l'API WPScan.
- Utilisez un token API pour les scans.
- Obtenez les dernières données de vulnérabilités.
- Améliorez la précision des scans.

---

# 18. Important Reminders

- Obtenez toujours une autorisation explicite avant d'utiliser WPScan.
- Testez d'abord dans un environnement de laboratoire contrôlé.
- Toutes les vulnérabilités détectées ne sont pas exploitables.
- Certains scans peuvent impacter les performances du site web.
- Maintenez WPScan à jour régulièrement.
- Validez les résultats manuellement ; ne vous fiez pas uniquement aux résultats automatisés.
- Documentez toutes les actions, commandes et résultats.
- Préservez les preuves originales et les logs.
- Respectez le périmètre et les règles d'engagement.
- Comprenez les implications légales et éthiques de vos actions.

---

# 19. Quick Reference Examples

## Scan de base

```bash
wpscan -u http://192.168.1.10/
```

## Énumérer les plugins

```bash
wpscan -u http://192.168.1.10/ --enumerate p
```

## Énumérer les thèmes

```bash
wpscan -u http://192.168.1.10/ --enumerate t
```

## Énumérer les utilisateurs

```bash
wpscan -u http://192.168.1.10/ --enumerate u
```

## Énumérer tout

```bash
wpscan -u http://192.168.1.10/ --enumerate a
```

## Détecter les plugins vulnérables

```bash
wpscan -u http://192.168.1.10/ --enumerate vp
```

## Brute-force de mot de passe

```bash
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt
```

## Utiliser un token API

```bash
wpscan -u http://192.168.1.10/ --api-token abc123xyz
```

## Sauvegarder en JSON

```bash
wpscan -u http://192.168.1.10/ -f json -o results.json
```

## Scan verbose

```bash
wpscan -u http://192.168.1.10/ -v
```

## Utiliser un proxy

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080
```

## Mettre à jour WPScan

```bash
wpscan --update
```

---

# 20. Additional Resources

## Site web officiel WPScan

```text
https://wpscan.com/
```

## Repository GitHub WPScan

```text
https://github.com/wpscanteam/wpscan
```

## Base de données de vulnérabilités WPScan

```text
https://wpscan.com/vulnerability-db
```

## Sécurité WordPress

```text
https://wordpress.org/support/article/hardening-wordpress/
```

## OWASP WordPress Security

```text
https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Enumerate_Applications_on_Web_Server
```
