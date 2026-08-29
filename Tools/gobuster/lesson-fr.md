<<<<<<< HEAD
# Cheatsheet Gobuster
=======
# Gobuster Cheat Sheet
>>>>>>> ab4cfff (New lessons Tools)

## Aperçu

Gobuster est un outil de force brute haute performance écrit en Go, utilisé pour découvrir :

- Les répertoires et fichiers cachés sur les serveurs web.
- Les sous-domaines DNS (avec prise en charge des wildcard).
- Les hôtes virtuels (VHosts).
- Les paramètres et valeurs via le fuzzing.
- Les buckets de stockage cloud (S3, GCS).

Utilisez Gobuster uniquement contre des systèmes que vous possédez ou qui sont explicitement inclus dans une évaluation autorisée.

```text
Remplacez <URL>, <domain> et <wordlist> par les valeurs autorisées de la cible.
```

---

# 1. Commandes de base de Gobuster

## Installation rapide

```bash
# En utilisant Go (recommandé)
go install github.com/OJ/gobuster/v3@latest

# Sur Kali/Debian
sudo apt install gobuster
```

## Afficher l'aide générale

```bash
gobuster help
```

## Afficher l'aide pour un mode spécifique

```bash
gobuster dir help
```

---

# 2. Mode Directory (dir)

Le mode le plus couramment utilisé pour énumérer les répertoires et fichiers sur les serveurs web.

## Scan de répertoires de base

```bash
gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt
```

## Scan avec extensions

Recherche chaque entrée de la wordlist avec des extensions supplémentaires :

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js,txt,bak
```

## Spécifier les codes de statut à afficher

Affiche uniquement les réponses avec des codes de statut spécifiques :

```bash
gobuster dir -u https://example.com -w wordlist.txt -s 200,204,301,302,307,401,403
```

## Exclure des codes de statut

Masque les réponses avec des codes de statut spécifiques :

```bash
gobuster dir -u https://example.com -w wordlist.txt -b 404
```

## Augmenter les threads

Augmente le nombre de threads concurrents (par défaut : 10) :

```bash
gobuster dir -u https://example.com -w wordlist.txt -t 50
```

## Ajouter un délai entre les requêtes

Ajoute un délai pour réduire la charge sur la cible :

```bash
gobuster dir -u https://example.com -w wordlist.txt --delay 1500ms
```

## Utiliser un proxy HTTP

Envoie le trafic via un proxy (par ex. Burp Suite) :

```bash
gobuster dir -u https://example.com -w wordlist.txt -p http://127.0.0.1:8080
```

## Ajouter des en-têtes personnalisés

```bash
gobuster dir -u https://example.com -w wordlist.txt -H "Authorization: Bearer TOKEN"
```

## Utiliser des cookies de session

```bash
gobuster dir -u https://example.com -w wordlist.txt -c "session=123456;user=admin"
```

## Ignorer la vérification du certificat TLS

```bash
gobuster dir -u https://example.com -w wordlist.txt -k
```

## Enregistrer les résultats dans un fichier

```bash
gobuster dir -u https://example.com -w wordlist.txt -o results.txt
```

## Mode silencieux (sans bannière)

```bash
gobuster dir -u https://example.com -w wordlist.txt -q
```

## Afficher une sortie verbeuse

```bash
gobuster dir -u https://example.com -w wordlist.txt -v
```

## Exclure une longueur de réponse spécifique

Utile pour filtrer les réponses de taille constante (par ex. pages d'erreur personnalisées) :

```bash
gobuster dir -u https://example.com -w wordlist.txt --exclude-length 1587
```

## Scanner avec plusieurs extensions et chemins spécifiques

```bash
gobuster dir -u https://example.com/admin -w wordlist.txt -x php,html -t 40 -s 200,301,302,403
```

---

# 3. Mode DNS (dns)

Découvre les sous-domaines via la résolution DNS.

## Scan de sous-domaines de base

```bash
gobuster dns -d example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

## Utiliser un résolveur DNS personnalisé

```bash
gobuster dns -d example.com -w wordlist.txt -r 8.8.8.8
```

## Augmenter les threads

```bash
gobuster dns -d example.com -w wordlist.txt -t 50
```

## Afficher les résultats wildcard

Par défaut, Gobuster masque les résultats wildcard. Utilisez `-i` pour les afficher :

```bash
gobuster dns -d example.com -w wordlist.txt -i
```

## Enregistrer les résultats

```bash
gobuster dns -d example.com -w wordlist.txt -o dns-results.txt
```

## Scan avec domaine et résolveur spécifiques

```bash
gobuster dns -d target.com -w subdomains.txt -r 1.1.1.1 -t 100
```

---

# 4. Mode VHost (vhost)

Énumère les hôtes virtuels via le fuzzing de l'en-tête `Host`.

## Scan VHost de base

```bash
gobuster vhost -u https://example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
```

## Utiliser une IP comme cible de base

Utile lorsque le serveur utilise l'IP pour capturer tout le trafic :

```bash
gobuster vhost -u http://10.10.10.5 -w vhosts.txt
```

## Ajouter un User-Agent personnalisé

```bash
gobuster vhost -u https://example.com -w wordlist.txt --useragent "PENTEST"
```

## Utiliser un domaine spécifique

```bash
gobuster vhost -u https://example.com -w wordlist.txt --domain example.com
```

## Ignorer la vérification TLS

```bash
gobuster vhost -u https://example.com -w wordlist.txt -k
```

---

# 5. Mode Fuzz (fuzz)

Remplace le mot-clé `FUZZ` n'importe où dans l'URL pour un fuzzing flexible.

## Fuzzing de paramètre

Remplace `FUZZ` par chaque entrée de la wordlist dans la valeur du paramètre :

```bash
gobuster fuzz -u "https://example.com/page.php?id=FUZZ" -w wordlist.txt
```

## Fuzzing de nom de paramètre

```bash
gobuster fuzz -u "https://example.com/page.php?FUZZ=value" -w wordlist.txt
```

## Fuzzing de chemin

```bash
gobuster fuzz -u "https://example.com/FUZZ/admin" -w wordlist.txt
```

## Fuzzing de l'en-tête Host

```bash
gobuster fuzz -u "https://example.com/" -w wordlist.txt -H "Host: FUZZ.example.com"
```

## Positions FUZZ multiples

```bash
gobuster fuzz -u "https://FUZZ.example.com/api/FUZZ" -w wordlist.txt
```

## Fuzzing avec méthode HTTP personnalisée

```bash
gobuster fuzz -u "https://example.com/api" -w wordlist.txt -m POST
```

---

# 6. Mode S3 (s3)

Énumère les buckets Amazon S3.

## Scan de bucket S3 de base

```bash
gobuster s3 -w bucket-names.txt
```

## Augmenter les threads

```bash
gobuster s3 -w bucket-names.txt -t 50
```

---

# 7. Mode GCS (gcs)

Énumère les buckets Google Cloud Storage.

## Scan de bucket GCS de base

```bash
gobuster gcs -w bucket-names.txt
```

---

# 8. Options globales

Ces options fonctionnent dans tous les modes.

## Nombre de threads

```bash
-t 50
```

## Wordlist

```bash
-w /chemin/vers/wordlist.txt
```

## Fichier de sortie

```bash
-o results.txt
```

## Mode silencieux

```bash
-q
```

## Sans couleurs

```bash
--no-color
```

## Sans barre de progression

```bash
-z
```

## Ne pas afficher les erreurs

```bash
--no-error
```

## Verbose (afficher les erreurs)

```bash
-v
```

## Proxy HTTP

```bash
-p http://127.0.0.1:8080
```

## Ignorer la vérification TLS

```bash
-k
```

## Ajouter un en-tête personnalisé

```bash
-H "Nom-En-tête: valeur"
```

## Cookies

```bash
-c "cookie1=valeur1;cookie2=valeur2"
```

## Délai entre les requêtes

```bash
--delay 1500ms
```

---

# 9. Flux de travail pratiques

## Flux de travail d'énumération web de base

### Étape 1 : Scan initial de répertoires

```bash
gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt -t 30
```

### Étape 2 : Scan avec extensions courantes

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js,txt,bak,zip,conf -t 40
```

### Étape 3 : Filtrer par codes de statut pertinents

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html -s 200,301,302,401,403 -t 50
```

### Étape 4 : Enregistrer les résultats

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js -o dir-results.txt -t 50
```

---

## Flux de travail de découverte de sous-domaines

### Étape 1 : Scan DNS de base

```bash
gobuster dns -d example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 50
```

### Étape 2 : Utiliser un résolveur personnalisé

```bash
gobuster dns -d example.com -w subdomains.txt -r 8.8.8.8 -o dns-results.txt
```

### Étape 3 : Énumérer les VHosts

```bash
gobuster vhost -u https://example.com -w subdomains.txt -t 50
```

---

## Flux de travail de fuzzing de paramètres

### Fuzzing d'ID numérique

```bash
gobuster fuzz -u "https://example.com/user.php?id=FUZZ" -w /usr/share/seclists/Fuzzing/numbers.txt
```

### Fuzzing de noms de paramètres

```bash
gobuster fuzz -u "https://example.com/login.php" -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -H "FUZZ: test"
```

---

## Scan complet autorisé

Un scan détaillé qui enregistre les résultats :

```bash
gobuster dir -u https://example.com \
  -w /usr/share/seclists/Discovery/Web-Content/common.txt \
  -x php,html,txt,js,bak,zip,conf \
  -t 50 \
  -s 200,204,301,302,307,401,403 \
  -o gobuster-full.txt \
  -k
```

Cette commande effectue :

- Scan de répertoires et fichiers.
- Test avec des extensions courantes.
- 50 threads concurrents.
- Filtrage par codes de statut pertinents.
- Enregistrement des résultats dans un fichier.
- Ignorance de la vérification TLS.

Utilisez ceci uniquement lorsque la portée autorise le trafic associé.

---

# 10. Tableaux de référence

## Modes de Gobuster

| Mode | Nom complet | Description |
|---|---|---|
| `dir` | Directory/file | Force brute de répertoires et fichiers sur les serveurs web. Le plus couramment utilisé. |
| `dns` | DNS subdomain | Découvre les sous-domaines via force brute d'entrées DNS. |
| `vhost` | Virtual Host | Énumère les hôtes virtuels via fuzzing de l'en-tête Host. |
| `fuzz` | Fuzzing général | Remplace le mot-clé `FUZZ` n'importe où dans l'URL. |
| `s3` | AWS S3 bucket | Énumère les buckets Amazon S3. |
| `gcs` | Google Cloud Storage | Énumère les buckets Google Cloud Storage. |

## Options courantes de Gobuster

| Option | Description |
|---|---|
| `-u` | URL cible (modes dir, vhost, fuzz) |
| `-d` | Domaine cible (mode dns) |
| `-w` | Chemin vers la wordlist |
| `-x` | Extensions à ajouter (mode dir) |
| `-s` | Afficher uniquement ces codes de statut |
| `-b` | Exclure ces codes de statut |
| `-t` | Nombre de threads concurrents (par défaut : 10) |
| `-o` | Fichier de sortie |
| `-p` | Proxy HTTP |
| `-H` | En-tête personnalisé |
| `-c` | Cookies |
| `-k` | Ignorer la vérification TLS |
| `-q` | Mode silencieux (sans bannière) |
| `-v` | Sortie verbeuse |
| `-z` | Sans barre de progression |
| `--delay` | Délai entre les requêtes |
| `--exclude-length` | Exclure les réponses avec cette longueur |
| `-r` | Résolveur DNS personnalisé (mode dns) |
| `-i` | Afficher les résultats wildcard (mode dns) |
| `--useragent` | User-Agent personnalisé (mode vhost) |
| `-m` | Méthode HTTP personnalisée (mode fuzz) |

---

# 11. Wordlists recommandées

## Répertoires et fichiers

```bash
/usr/share/wordlists/dirb/common.txt
/usr/share/seclists/Discovery/Web-Content/common.txt
/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

## Sous-domaines

```bash
/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
/usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
/usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
```

## Fuzzing de paramètres

```bash
/usr/share/seclists/Fuzzing/fuzz-Bo0oM.txt
/usr/share/seclists/Fuzzing/numbers.txt
/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt
```

## Buckets S3/GCS

```bash
/usr/share/seclists/Discovery/Cloud-Storage/bucket-names.txt
```

---

# 12. Rappels importants

- Un répertoire trouvé n'est pas automatiquement une vulnérabilité.
- Les résultats peuvent inclure des faux positifs. Validez manuellement.
- Le fuzzing agressif peut générer un trafic important.
- Les scans peuvent déclencher des alertes de pare-feu, WAF, IDS, IPS ou SIEM.
- Utilisez `-k` uniquement dans des environnements de test ; en production, validez les certificats.
- Ajustez les threads (`-t`) selon la capacité de la cible.
- Utilisez `--delay` pour réduire la charge sur les systèmes sensibles.
- Enregistrez toujours la sortie originale comme preuve.
- Validez les résultats intéressants avec un navigateur, un proxy ou un client HTTP.
- Ne scannez jamais de systèmes en dehors de la portée autorisée.
- Gobuster est idéal pour un premier balayage rapide ; utilisez des outils comme ffuf pour un fuzzing plus avancé.

---

# 13. Exemples de commandes rapides

| Tâche | Commande |
|---|---|
| Scan de répertoires de base | `gobuster dir -u https://target.com -w wordlist.txt` |
| Scan avec extensions | `gobuster dir -u https://target.com -w wordlist.txt -x php,html,js` |
| Afficher sauf 404 | `gobuster dir -u https://target.com -w wordlist.txt -b 404` |
| Scan DNS de sous-domaines | `gobuster dns -d target.com -w subdomains.txt` |
| Scan VHost | `gobuster vhost -u https://target.com -w vhosts.txt` |
| Fuzzing de paramètre | `gobuster fuzz -u "https://target.com?id=FUZZ" -w wordlist.txt` |
| Enregistrer les résultats | `gobuster dir -u https://target.com -w wordlist.txt -o results.txt` |
| Utiliser un proxy | `gobuster dir -u https://target.com -w wordlist.txt -p http://127.0.0.1:8080` |
| 50 threads | `gobuster dir -u https://target.com -w wordlist.txt -t 50` |
| Délai 1,5s | `gobuster dir -u https://target.com -w wordlist.txt --delay 1500ms` |

---

## Flux de travail recommandé

```text
1. Confirmer l'autorisation et la portée.
2. Effectuer un scan initial de répertoires (mode dir).
3. Scanner avec des extensions courantes.
4. Énumérer les sous-domaines (mode dns).
5. Énumérer les VHosts (mode vhost).
6. Effectuer un fuzzing de paramètres (mode fuzz).
7. Enregistrer tous les résultats.
8. Valider manuellement les découvertes intéressantes.
9. Éliminer les faux positifs.
10. Documenter uniquement les découvertes confirmées.
```
