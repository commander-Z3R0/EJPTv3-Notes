# File & Directory Enumeration

## Présentation Générale

Le brute-force de fichiers et de répertoires (souvent appelé découverte de contenu ou énumération de répertoires) est une technique utilisée pour découvrir des fichiers, des répertoires et des endpoints cachés ou non liés sur un serveur web.

Les applications web n'exposent souvent qu'une partie de leur structure réelle via des liens et des menus visibles. En coulisses, il peut y avoir :

- Panneaux d'administration (`/admin`, `/dashboard`).
- Fichiers de sauvegarde (`.bak`, `.old`, `.zip`).
- Endpoints de développement ou de test (`/test`, `/dev`, `/staging`).
- Routes d'API non référencées dans le frontend.
- Fichiers de configuration ou de débogage.

Le brute-force fonctionne en envoyant un grand nombre de requêtes HTTP pour des noms de fichiers et de répertoires courants (à partir de wordlists prédéfinies) et en analysant les réponses du serveur (codes de statut, taille de réponse, redirections, etc.) pour déduire ce qui existe.

---

## Pourquoi C'est Effectué

Le brute-force de fichiers et de répertoires est effectué car ce que vous ne pouvez pas voir peut toujours être exploitable. Ses principaux objectifs sont :

- **Élargir la surface d'attaque** : découvrir des fonctionnalités qui n'étaient pas destinées à être publiques.
- **Identifier des points d'entrée sensibles** : portails d'administration, outils internes, répertoires de téléchargement ou APIs.
- **Trouver des erreurs de configuration** : sauvegardes exposées, fichiers sources ou artefacts de développement oubliés.
- **Permettre des attaques ultérieures** : les endpoints découverts peuvent mener à :
  - Contournement d'authentification.
  - Vulnérabilités de téléchargement de fichiers.
  - Divulgation d'informations.
  - Injection SQL, XSS ou failles logiques.

Lors de pentests réels, l'énumération de répertoires agit fréquemment comme un point de pivot — un endpoint découvert débloque souvent plusieurs voies d'exploitation.

---

# Gobuster

**Gobuster** est un outil d'énumération rapide en ligne de commande écrit en Go, couramment utilisé lors de la phase de reconnaissance et d'énumération lors de tests de pénétration d'applications web.

Il effectue une découverte de type brute-force en utilisant des wordlists et est conçu pour être efficace, simple et scriptable.

Gobuster est populaire car il est :

- **Rapide** (basé sur Go, hautement performant).
- **Fiable** (logique simple, faux positifs minimes).
- **Flexible** (fonctionne avec de nombreuses wordlists et configurations).
- **Adapté à l'automatisation** dans les flux de travail de pentest.

En pratique, Gobuster est souvent l'un des premiers outils d'énumération active exécutés contre une cible web après la reconnaissance passive, aidant les testeurs à identifier rapidement les zones nécessitant des tests manuels plus approfondis.

---

## Modes de Gobuster

Gobuster prend en charge plusieurs modes d'énumération. Les plus pertinents pour le pentest d'applications web sont :

| Mode | Description |
|---|---|
| `dir` | Énumération de répertoires et de fichiers sur un serveur web |
| `vhost` | Énumération d'hôtes virtuels sur un domaine cible |
| `dns` | Énumération de sous-domaines par DNS |
| `fuzz` | Mode de fuzzing pour des modèles personnalisés |
| `gcs` | Énumération de buckets Google Cloud Storage |
| `s3` | Énumération de buckets AWS S3 |

### Fonctionnalité Principale (mode dir)

- Brute-force de répertoires et de fichiers sur un serveur web.
- Prend en charge les extensions de fichiers (ex. `.php`, `.js`, `.txt`).
- Filtre les résultats selon les codes de statut HTTP.
- Aide à découvrir des routes et des fonctionnalités cachées.

### Énumération d'Hôtes Virtuels (mode vhost)

- Découvre des hôtes virtuels cachés sur un domaine cible.
- Utile lorsque les applications se comportent différemment selon le nom d'hôte.
- Modifie l'en-tête `Host` pour tester les hôtes virtuels configurés sur la cible.

### Énumération de Sous-domaines par DNS (mode dns)

- Brute-force de sous-domaines utilisant des requêtes DNS.
- Utile pour cartographier l'écosystème complet de l'application.
- Idéal en complément du mode vhost.

---

# Installation

Gobuster est généralement préinstallé sur Kali Linux.

## Vérifier la Version Installée

```bash
gobuster version  # affiche la version installée de Gobuster.
```

## Afficher l'Aide

```bash
gobuster --help  # affiche les options et modes disponibles de Gobuster.
gobuster dir --help  # affiche les options spécifiques au mode dir.
```

## Installer Gobuster sur les Systèmes Basés sur Debian

```bash
sudo apt update  # met à jour la liste locale des paquets.
sudo apt install gobuster -y  # installe Gobuster.
```

Alternativement, Gobuster peut être installé depuis ses releases GitHub ou compilé depuis le code source avec Go :

```bash
go install github.com/OJ/gobuster/v3@latest  # installe Gobuster avec Go.
```

---

# Flags et Options de Base

| Flag | Description | Exemple |
|---|---|---|
| `-u` | URL cible | `-u https://example.com` |
| `-w` | Wordlist pour le brute-force | `-w /path/to/wordlist.txt` |
| `-k` | Ignore les erreurs de certificat SSL/TLS (HTTPS) | `-k` |
| `-t` | Nombre de threads pour accélérer le scan | `-t 20` |
| `-o` | Enregistre les résultats dans un fichier | `-o results.txt` |
| `-x` | Extensions de fichiers à rechercher | `-x php,html,txt` |
| `-r` | Active le mode récursif | `-r` |
| `-s` | Filtre par codes de statut HTTP | `-s 200,204,301` |
| `-z` | Ignore la longueur de réponse (sans barre de progression par résultat) | `-z` |
| `-X` | Utilise des méthodes HTTP spécifiques | `-X GET,POST` |
| `-P` | Préfixe de chemin URL pour chaque requête | `-P /app/` |
| `--append-domain` | En mode vhost, ajoute le domaine de base à chaque mot (recommandé) | `--append-domain` |
| `-q` | Mode silencieux, supprime la bannière et la sortie supplémentaire | `-q` |
| `-e` | Sortie étendue, affiche les URLs complètes | `-e` |
| `-n` | N'affiche pas les codes de statut dans la sortie | `-n` |
| `--no-error` | N'affiche pas les erreurs | `--no-error` |
| `-c` | Spécifie les cookies HTTP pour l'authentification | `-c "session=abc123"` |
| `-H` | Spécifie les en-têtes HTTP personnalisés | `-H "Authorization: Bearer token"` |
| `-b` | Liste noire de codes de statut spécifiques | `-b 403,404` |
| `-l` | Affiche la longueur de réponse dans la sortie | `-l` |

---

# Exemples d'Énumération de Répertoires

## Énumération Basique de Répertoires

Énumère les répertoires en utilisant une wordlist courante :

```bash
gobuster dir -u https://www.example.com -w common.txt  # brute-force basique de répertoires.
```

## Wordlist Personnalisée et Extensions

Utilise une wordlist personnalisée et spécifie les extensions de fichiers à rechercher :

```bash
gobuster dir -u https://www.example.com -w custom.txt -x php,html  # recherche des fichiers .php et .html.
```

## Énumération Récursive de Répertoires

Active le mode récursif pour explorer les sous-répertoires :

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -r  # énumère récursivement les répertoires découverts.
```

## Énumération de Répertoires depuis un Chemin URL Spécifique

Énumère les répertoires à partir d'un chemin URL spécifique :

```bash
gobuster dir -u https://www.example.com/subdir/ -w common.txt  # brute-force les chemins sous /subdir/.
```

## Filtrer par Codes de Statut HTTP

Spécifie quels codes de statut HTTP considérer comme trouvés :

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -x php,html -s 200,204  # affiche uniquement les résultats avec les statuts 200 et 204.
```

## Utiliser Différentes Méthodes HTTP

Utilise différentes méthodes HTTP lors de l'énumération de répertoires :

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -x php -X GET,POST  # envoie des requêtes GET et POST.
```

## Préfixe de Chemin URL

Ajoute un préfixe de chemin URL à chaque requête :

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -P /app/  # préfixe chaque mot avec /app/.
```

## Ignorer la Longueur de Réponse

Ignore la longueur de réponse pour identifier rapidement les chemins existants :

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -z  # supprime la colonne de longueur de réponse.
```

## Enregistrer les Résultats dans un Fichier

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -o results.txt  # enregistre les résultats dans un fichier.
```

## Scanner avec des Cookies Personnalisés (Énumération Authentifiée)

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -c "session=abc123; role=admin"  # scanne avec des cookies d'authentification.
```

## Scanner avec des En-têtes Personnalisés

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -H "Authorization: Bearer token123" -H "X-Custom-Header: test"  # envoie des en-têtes personnalisés avec chaque requête.
```

## Liste Noire de Codes de Statut

Exclut des codes de statut spécifiques des résultats :

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -b 403,404  # masque les réponses 403 et 404.
```

## Sortie Étendue avec URLs Complètes

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -e -o full-urls.txt  # affiche les URLs complètes dans la sortie et les enregistre.
```

## Augmenter les Threads pour un Scan Plus Rapide

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -t 50  # utilise 50 threads pour une énumération plus rapide.
```

## Ignorer les Erreurs de Certificat SSL/TLS (HTTPS)

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -k  # ignore les erreurs de validation de certificat.
```

---

# Énumération d'Hôtes Virtuels (mode vhost)

Un serveur web peut héberger plusieurs sites web (hôtes virtuels) sur la même adresse IP. Le mode vhost de Gobuster brute-force les noms d'hôtes virtuels en modifiant l'en-tête `Host` de chaque requête.

## Énumération Basique de vhost

```bash
gobuster vhost -u https://nunchucks.htb -k -w subdomains.txt --append-domain  # brute-force les hôtes virtuels, en ajoutant le domaine de base.
```

### Note Importante sur `--append-domain`

`--append-domain` est critique pour que le mode vhost fonctionne correctement. Sans lui, Gobuster envoie uniquement le mot brut de la wordlist comme en-tête `Host` (ex. `admin` au lieu de `admin.nunchucks.htb`). Avec `--append-domain`, chaque mot est combiné avec le domaine de base, produisant des noms d'hôtes virtuels valides.

## Énumération vhost avec Threads et Sortie

```bash
gobuster vhost -u https://nunchucks.htb -k -w subdomains.txt --append-domain -t 30 -o vhost_results.txt  # scanne avec 30 threads et enregistre les résultats.
```

Le mode vhost est utile lorsque :

- Les applications se comportent différemment selon l'en-tête `Host`.
- Le DNS n'est pas disponible ou ne résout pas les sous-domaines.
- Vous souhaitez découvrir des hôtes virtuels configurés sur le serveur mais non exposés publiquement.

---

# Énumération de Sous-domaines par DNS (mode dns)

Le mode dns de Gobuster brute-force les sous-domaines en envoyant des requêtes DNS. Il n'envoie pas de requêtes HTTP ; il vérifie uniquement si des enregistrements DNS existent pour chaque sous-domaine candidat.

## Énumération DNS Basique

```bash
gobuster dns -d nunchucks.htb -w subdomains.txt -t 30 -o dns_result.txt  # brute-force les sous-domaines via des requêtes DNS avec 30 threads.
```

### Quand Utiliser le Mode dns

- Pour cartographier l'écosystème complet de l'application.
- En complément du mode vhost (DNS trouve les sous-domaines résolvables ; vhost trouve les hôtes virtuels configurés sur le serveur même sans DNS).
- Lorsque vous souhaitez confirmer si les sous-domaines découverts se résolvent en une adresse IP.

### Différences entre le Mode vhost et le Mode dns

| Aspect | Mode vhost | Mode dns |
|---|---|---|
| Ce qu'il teste | Hôtes virtuels en modifiant l'en-tête `Host` | Sous-domaines via la résolution DNS |
| Requiert DNS | Non | Oui |
| Trouve | Hôtes configurés sur le serveur | Sous-domaines qui se résolvent en DNS |
| Mieux utilisé pour | Trouver des apps cachées sur une IP connue | Cartographier l'empreinte DNS du domaine |

Utiliser les deux modes ensemble fournit une image plus complète du panorama des sous-domaines et hôtes virtuels de la cible.

---

# Wordlists Courantes

Gobuster nécessite une wordlist pour effectuer le brute-force. Emplacements courants des wordlists sur Kali Linux :

```text
/usr/share/wordlists/dirb/common.txt
/usr/share/wordlists/dirb/big.txt
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
/usr/share/wordlists/seclists/Discovery/Web-Content/
```

[SecLists](https://github.com/danielmiessler/SecLists) est une collection complète de wordlists pour les tests de sécurité. Elle peut être installée sur Kali Linux :

```bash
sudo apt install seclists -y  # installe la collection SecLists.
```

Choisissez une wordlist appropriée pour la cible :

- `common.txt` pour des scans rapides et larges.
- `directory-list-2.3-medium.txt` pour une énumération plus approfondie.
- Wordlists personnalisées adaptées à la pile technologique de la cible.

---

# Comprendre les Codes de Statut HTTP dans les Résultats

Gobuster rapporte les résultats en fonction des codes de statut HTTP renvoyés par le serveur. Comprendre ces codes aide à interpréter les résultats :

| Code de Statut | Signification | Interprétation |
|---|---|---|
| `200` | OK | La ressource existe et est accessible |
| `204` | No Content | La ressource existe mais ne renvoie pas de corps |
| `301` | Moved Permanently | La ressource existe mais a été redirigée |
| `302` | Found | La ressource existe mais est temporairement redirigée |
| `401` | Unauthorized | La ressource existe mais nécessite une authentification |
| `403` | Forbidden | La ressource existe mais l'accès est refusé |
| `404` | Not Found | La ressource n'existe pas |
| `500` | Internal Server Error | La ressource existe mais a provoqué une erreur serveur |

Les codes de statut `401` et `403` sont particulièrement intéressants lors de l'énumération — ils confirment qu'une ressource existe même si l'accès est restreint, ce qui peut guider des attaques ultérieures (contournement d'authentification, escalade de privilèges, etc.).

---

# Commandes Pratiques Courantes

## Scan Rapide de Répertoires avec Wordlist Courante

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -t 30 -o gobuster-dir.txt  # scan rapide de répertoires, 30 threads, enregistre la sortie.
```

## Scan de Répertoires avec Extensions et HTTPS

```bash
gobuster dir -u https://target.local -w /usr/share/wordlists/dirb/common.txt -x php,html,txt -k -t 30 -o gobuster-ext.txt  # scanne les fichiers avec extensions en HTTPS en ignorant les erreurs de certificat.
```

## Scan Récursif avec Extensions

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -x php,bak,old -r -t 30 -o gobuster-recursive.txt  # scanne récursivement avec des extensions de fichiers de sauvegarde.
```

## Scan de Répertoires Authentifié

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -c "session=abc123" -o gobuster-auth.txt  # scanne avec un cookie de session.
```

## Énumération de vhost

```bash
gobuster vhost -u https://target.local -k -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain -t 30 -o gobuster-vhost.txt  # brute-force les hôtes virtuels avec domaine ajouté.
```

## Énumération de Sous-domaines par DNS

```bash
gobuster dns -d target.local -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 30 -o gobuster-dns.txt  # brute-force les sous-domaines via DNS.
```

## Scan avec Filtre de Code de Statut et Liste Noire

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -s 200,301,401,403 -b 404 -o gobuster-filtered.txt  # affiche uniquement les codes de statut intéressants et masque les 404.
```

---

# Mode Fuzzing (fuzz)

Gobuster inclut également un mode `fuzz` qui permet le brute-force de modèles d'URL personnalisés. Cela est utile lorsque vous connaissez une partie de la structure d'une URL et que vous voulez forcer un segment spécifique.

## Exemple Basique de Fuzzing

```bash
gobuster fuzz -u https://www.example.com/FUZZ?param=value -w wordlist.txt  # brute-force le marqueur FUZZ dans l'URL.
```

Le mot-clé `FUZZ` est remplacé par chaque mot de la wordlist. Ce mode est flexible et prend en charge tout modèle d'URL où vous souhaitez énumérer un segment variable.

---

# Liste de Contrôle pour une Énumération Sécurisée

Avant d'exécuter Gobuster :

- Confirmez que la cible est dans le périmètre.
- Confirmez l'URL, le port et le protocole de la cible.
- Identifiez si HTTPS est utilisé (utilisez `-k` si nécessaire).
- Choisissez une wordlist appropriée pour la cible.
- Décidez si une authentification est requise (utilisez `-c` ou `-H`).
- Définissez un nombre de threads raisonnable pour éviter de surcharger la cible.
- Envisagez si un scan récursif est nécessaire.
- Informez l'équipe concernée si le scan pourrait affecter les systèmes de surveillance ou de production.

Après avoir exécuté Gobuster :

- Enregistrez les résultats.
- Révisez chaque chemin et fichier découvert.
- Validez manuellement les résultats intéressants avec un navigateur, `curl` ou Burp Suite.
- Vérifiez les codes de statut `401` et `403` pour les problèmes d'authentification ou de contrôle d'accès.
- Investiguez les fichiers de sauvegarde (`.bak`, `.old`, `.zip`, `.tar.gz`).
- Testez les panneaux d'administration et les répertoires de téléchargement découverts.
- Documentez les preuves telles que les URL, les réponses HTTP et les captures d'écran.

---

# Points Clés

- Le brute-force de fichiers et de répertoires découvre le contenu caché ou non lié sur un serveur web.
- Gobuster est un outil d'énumération rapide basé sur Go avec plusieurs modes.
- Utilisez le mode `dir` pour l'énumération de répertoires et de fichiers.
- Utilisez le mode `vhost` pour l'énumération d'hôtes virtuels (utilisez toujours `--append-domain`).
- Utilisez le mode `dns` pour l'énumération de sous-domaines par DNS.
- Utilisez le mode `fuzz` pour le brute-force de modèles d'URL personnalisés.
- Utilisez `-u` pour définir l'URL cible.
- Utilisez `-w` pour définir la wordlist.
- Utilisez `-x` pour spécifier les extensions de fichiers.
- Utilisez `-r` pour l'énumération récursive.
- Utilisez `-s` pour filtrer par codes de statut et `-b` pour la liste noire.
- Utilisez `-k` pour ignorer les erreurs de certificat SSL/TLS.
- Utilisez `-t` pour contrôler le nombre de threads.
- Utilisez `-o` pour enregistrer les résultats dans un fichier.
- Utilisez `-c` ou `-H` pour les scans authentifiés.
- Les codes de statut `401` et `403` confirment qu'une ressource existe même si l'accès est restreint.
- Validez toujours les résultats manuellement avant de les signaler comme des vulnérabilités.
- Choisissez des wordlists appropriées à la pile technologique de la cible.
