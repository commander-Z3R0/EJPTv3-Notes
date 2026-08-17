# HTTP/S Protocol Fundamentals

## Protocole HTTP

**HTTP** signifie **Hypertext Transfer Protocol**. C’est un protocole de couche application utilisé pour transférer des ressources web, comme des pages HTML, des images, des fichiers JavaScript, CSS, des données d’API et du contenu d’applications web.

HTTP fonctionne au-dessus de **TCP** et a été conçu pour la communication entre navigateurs web et serveurs web.

HTTP utilise une architecture client-serveur :

- Le **client** est généralement un navigateur, une application mobile, un script ou un outil en ligne de commande.
- Le **serveur** est le serveur web qui reçoit les requêtes et renvoie les réponses.

Les ressources sont identifiées de manière unique par une **URL** ou une **URI**.

```text
Client / Navigateur                         Serveur Web
       |                                        |
       | -------- Requête HTTP --------------> |
       |                                        |
       | <------- Réponse HTTP --------------- |
       |                                        |
```

---

## Site web et serveur web

### Qu’est-ce qu’un site web ?

Un site web est un ensemble de pages web interconnectées et accessibles par Internet.

Il peut contenir :

- Du texte.
- Des images.
- Des vidéos.
- Des formulaires.
- Des liens.
- Des fichiers téléchargeables.
- Du contenu interactif.

### Qu’est-ce qu’un serveur web ?

Un serveur web est un logiciel ou un matériel qui reçoit des requêtes HTTP ou HTTPS et renvoie des ressources web aux clients.

Exemples de logiciels serveur web :

```text
Apache HTTP Server
Nginx
Microsoft IIS
```

### Types de serveurs

| Type de serveur | Fonction |
|---|---|
| Serveur HTTP/Web | Gère les requêtes HTTP et sert du contenu statique comme HTML, CSS, JavaScript, images et fichiers |
| Serveur d’applications | Exécute des applications, traite les données, gère les interactions utilisateur et génère du contenu dynamique |
| Serveur de base de données | Stocke et gère les données utilisées par les applications web, comme les utilisateurs, sessions et commandes |

### Off-Premise Hosting

L’**off-premise hosting**, aussi appelé cloud hosting, consiste à héberger un site ou une application sur une infrastructure distante plutôt que sur les serveurs locaux de l’organisation.

Exemples : plateformes cloud, VPS et fournisseurs d’hébergement géré.

---

## Versions HTTP

### HTTP/1.0

HTTP/1.0 est une version ancienne de HTTP.

Elle permet de demander des ressources à un serveur web, mais nécessite généralement une nouvelle connexion TCP pour chaque requête. Elle est donc moins efficace pour les applications web modernes.

### HTTP/1.1

HTTP/1.1 améliore HTTP/1.0 en permettant les connexions persistantes.

```text
HTTP/1.0  = crée généralement une nouvelle connexion pour chaque requête.
HTTP/1.1  = peut réutiliser la même connexion pour plusieurs requêtes.
```

HTTP/1.1 utilise `Connection: keep-alive` pour maintenir une connexion TCP ouverte et envoyer plusieurs requêtes par celle-ci.

### HTTP/2

HTTP/2 améliore les performances en permettant d’envoyer plusieurs requêtes et réponses simultanément sur une même connexion.

Améliorations importantes :

- Multiplexing.
- Compression des headers.
- Réduction de la latence.
- Chargement des ressources plus efficace.

### HTTP/3

HTTP/3 est conçu pour améliorer encore davantage les performances en utilisant le protocole de transport **QUIC**.

Il vise à réduire la latence et à accélérer l’établissement des connexions, surtout sur des réseaux peu fiables.

---

## HTTP est stateless

HTTP est un protocole **sans état** (*stateless*).

Cela signifie que chaque requête est indépendante par défaut. Le serveur ne mémorise pas automatiquement les requêtes précédentes du même utilisateur.

Les applications web utilisent des sessions, cookies et tokens pour maintenir l’état utilisateur.

```text
1. L’utilisateur se connecte.
2. Le serveur valide les identifiants.
3. Le serveur crée une session ou un token.
4. Le navigateur stocke un cookie ou token de session.
5. Le navigateur l’envoie avec les requêtes suivantes.
6. Le serveur reconnaît l’utilisateur authentifié.
```

---

## Structure des messages HTTP

Pendant une communication HTTP, le client et le serveur échangent des messages.

Le client envoie une **requête HTTP** et le serveur renvoie une **réponse HTTP**.

```text
[ Navigateur / Client ] ---- Requête HTTP ----> [ Serveur Web ]
[ Navigateur / Client ] <--- Réponse HTTP ----- [ Serveur Web ]
```

Les messages HTTP utilisent :

```text
\r     = Carriage Return ; déplace le curseur au début de la ligne.
\n     = Line Feed ; déplace le curseur à la ligne suivante.
\r\n   = Carriage Return + Line Feed ; marque la fin d’une ligne.
```

Une ligne vide, représentée par `\r\n\r\n`, sépare les headers HTTP du corps optionnel du message.

---

# Requêtes HTTP

## Composants d’une requête HTTP

Une requête HTTP contient normalement :

1. Une request line.
2. Des request headers.
3. Une ligne vide.
4. Un request body optionnel.

### Structure d’une requête

```http
METHOD /path HTTP/version
Header-Name: Header-Value
Header-Name: Header-Value

Optional request body
```

---

## Request Line

La request line est la première ligne d’une requête HTTP.

Elle contient :

- La méthode HTTP.
- Le chemin URL demandé.
- La version HTTP.

### Exemple

```http
GET / HTTP/1.1
```

```text
GET       = méthode HTTP.
 /        = chemin demandé ; page racine.
HTTP/1.1  = version du protocole HTTP.
```

---

## Chemin de requête

Le chemin indique la ressource à laquelle le client souhaite accéder.

```http
GET / HTTP/1.1
```

Le chemin `/` représente la page d’accueil ou le répertoire racine du site web.

Autres exemples :

```http
GET /login HTTP/1.1
GET /downloads/index.php HTTP/1.1
GET /images/logo.png HTTP/1.1
GET /api/users/10 HTTP/1.1
```

Le header `Host` et le chemin sont combinés pour former l’URL complète.

```text
Host: example.com
Path: /login

URL complète : http://example.com/login
```

---

## Headers des requêtes HTTP

Les headers fournissent des informations supplémentaires concernant la requête HTTP.

Leur format de base est :

```text
Header-Name: Header-Value
```

Headers courants :

| Header | Fonction |
|---|---|
| Host | Spécifie le hostname du serveur demandé |
| User-Agent | Identifie le client, le navigateur, l’OS et parfois la langue |
| Accept | Définit les types de contenu acceptés par le client |
| Accept-Encoding | Définit les formats de compression acceptés, comme gzip ou deflate |
| Authorization | Envoie des identifiants ou tokens d’authentification |
| Cookie | Envoie des données stockées côté client, généralement des identifiants de session |
| Referer | Indique la page ayant lié vers la ressource demandée |
| Origin | Indique l’origine de la requête, important pour CORS |
| Content-Type | Spécifie le format des données envoyées dans le corps |
| Content-Length | Spécifie la taille du corps de la requête en octets |
| Connection | Contrôle si la connexion reste ouverte ou se ferme |

---

## Host Header

Le header `Host` indique à quel site web le client souhaite accéder.

```http
Host: www.example.com
```

Ce header permet à un seul serveur web avec une seule adresse IP d’héberger plusieurs sites web. Cela s’appelle le **virtual hosting**.

```text
Adresse IP : 192.168.1.10

Host: website-a.com
Host: website-b.com
Host: website-c.com
```

Le serveur utilise le header `Host` pour choisir quelle configuration de site ou quel virtual host doit traiter la requête.

---

## User-Agent Header

Le header `User-Agent` identifie le client qui effectue la requête.

```http
User-Agent: Mozilla/5.0 (X11; Linux x86_64) Firefox/120.0
```

Il peut révéler :

- Le type de navigateur.
- La version du navigateur.
- Le système d’exploitation.
- Le type d’appareil.
- La langue ou la plateforme.

Les serveurs peuvent utiliser ces informations pour retourner du contenu spécifique au navigateur, mais elles peuvent également exposer des détails inutiles sur l’utilisateur.

---

## Accept Header

Le header `Accept` spécifie les types de contenu acceptés par le client.

```http
Accept: text/html,application/xhtml+xml,application/json
```

Types de contenu courants :

```text
text/html          = page HTML.
application/json   = données JSON d’API.
application/xml    = données XML.
text/plain          = texte brut.
image/png          = image PNG.
image/jpeg         = image JPEG.
```

---

## Accept-Encoding Header

Le header `Accept-Encoding` définit les formats d’encodage ou compression acceptés par le client.

```http
Accept-Encoding: gzip, deflate, br
```

Valeurs courantes :

```text
gzip    = format de compression commun.
deflate = format de compression.
br      = compression Brotli.
```

La compression réduit la taille des réponses et améliore les performances.

---

## Connection Header

Le header `Connection` contrôle la gestion de la connexion réseau.

```http
Connection: keep-alive
```

Avec HTTP/1.1, `keep-alive` permet au navigateur de réutiliser la même connexion TCP pour plusieurs requêtes.

```text
Connection: close       = ferme la connexion après la réponse.
Connection: keep-alive  = maintient la connexion ouverte pour de futures requêtes.
```

---

## Authorization Header

Le header `Authorization` envoie des identifiants ou des tokens d’authentification.

Exemple avec HTTP Basic Authentication :

```http
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

La valeur après `Basic` est encodée en Base64 et ne doit pas être considérée comme chiffrée.

Exemple avec un bearer token :

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

Lors de tests de sécurité, vérifie que les tokens :

- Ne sont pas exposés dans les URLs.
- Expirent correctement.
- Ne peuvent pas être réutilisés après le logout.
- Sont validés côté serveur.
- Sont protégés par HTTPS.

---

## Cookie Header

Le header `Cookie` envoie les cookies enregistrés dans le navigateur au serveur.

```http
Cookie: session=abc123; theme=dark
```

Les cookies sont souvent utilisés pour :

- La gestion de sessions.
- L’authentification.
- Les préférences utilisateur.
- La sélection de langue.
- Les paniers.
- Le tracking.

### Note de sécurité

Les cookies de session sont sensibles, car ils peuvent permettre l’accès à des comptes authentifiés.

---

## Request Body

Certaines méthodes HTTP incluent un corps contenant des données envoyées au serveur.

Les méthodes utilisant souvent un corps sont :

- POST.
- PUT.
- PATCH.

### Exemple avec données de formulaire

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 31

username=student&password=test123
```

### Exemple JSON

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "username": "student",
  "email": "student@example.com"
}
```

---

# Méthodes HTTP

Les méthodes HTTP, également appelées verbes HTTP, définissent l’action que le client souhaite effectuer sur une ressource.

| Méthode | Fonction | Sûre | Idempotente |
|---|---|---:|---:|
| GET | Récupère des données depuis le serveur | Oui | Oui |
| POST | Envoie des données pour traitement ou création | Non | Non |
| PUT | Crée ou remplace une ressource complète | Non | Généralement oui |
| DELETE | Supprime une ressource | Non | Généralement oui |
| PATCH | Applique une mise à jour partielle | Non | Pas toujours |
| HEAD | Récupère uniquement les headers, sans corps de réponse | Oui | Oui |
| OPTIONS | Récupère les options de communication et méthodes supportées | Oui | Oui |

### Méthodes sûres

Une méthode **sûre** ne devrait pas modifier l’état du serveur.

Exemples :

```text
GET
HEAD
OPTIONS
```

### Méthodes idempotentes

Une méthode **idempotente** devrait produire le même résultat même si elle est répétée plusieurs fois.

Par exemple, exécuter une requête `GET` dix fois ne devrait pas modifier les données du serveur.

---

## GET

`GET` récupère des données depuis le serveur.

```http
GET /products?id=10 HTTP/1.1
Host: example.com
```

GET ne doit pas modifier les données du serveur.

### Note de sécurité

Les données sensibles ne doivent pas être envoyées dans des paramètres GET, car les URLs peuvent être enregistrées dans :

- L’historique du navigateur.
- Les logs du serveur.
- Les logs de proxy.
- Les favoris.
- Les headers Referer.

Exemple incorrect :

```text
https://example.com/login?username=student&password=password123
```

---

## POST

`POST` envoie des données au serveur pour les traiter.

Il est souvent utilisé pour :

- Les formulaires de connexion.
- L’inscription des utilisateurs.
- L’upload de fichiers.
- La création de commandes.
- La création de commentaires.
- L’envoi de données de paiement.

```http
POST /register HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=student&password=securepassword
```

POST peut modifier l’état du serveur et n’est généralement pas idempotent.

---

## PUT

`PUT` crée ou remplace une ressource à une URL spécifique.

```http
PUT /api/users/10 HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "username": "student",
  "role": "user"
}
```

Si la ressource existe, PUT la remplace généralement entièrement. Si elle n’existe pas, PUT peut la créer.

---

## DELETE

`DELETE` demande la suppression d’une ressource.

```http
DELETE /api/users/10 HTTP/1.1
Host: example.com
```

Les requêtes DELETE doivent être correctement authentifiées et autorisées.

Si l’autorisation est faible, un attaquant peut supprimer les données d’autres utilisateurs.

---

## PATCH

`PATCH` applique des modifications partielles à une ressource.

```http
PATCH /api/users/10 HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "email": "new-email@example.com"
}
```

Contrairement à PUT, PATCH modifie généralement seulement les champs sélectionnés.

---

## HEAD

`HEAD` est similaire à GET, mais retourne uniquement les headers de réponse et non le corps.

```http
HEAD /backup.zip HTTP/1.1
Host: example.com
```

HEAD peut être utile pour vérifier :

- L’existence d’une ressource.
- Les headers de réponse.
- Le type de contenu.
- La taille du contenu.
- La date de modification.

---

## OPTIONS

`OPTIONS` récupère les options de communication disponibles pour une ressource.

```http
OPTIONS /api/users HTTP/1.1
Host: example.com
```

Le serveur peut retourner les méthodes autorisées dans le header `Allow`.

```http
Allow: GET, POST, OPTIONS
```

Pendant une évaluation de sécurité, OPTIONS peut aider à identifier des méthodes activées comme PUT, DELETE ou PATCH.

---

# Réponses HTTP

## Composants d’une réponse HTTP

Une réponse HTTP contient normalement :

1. Une status line.
2. Des response headers.
3. Une ligne vide.
4. Un response body optionnel.

### Structure d’une réponse

```http
HTTP/version status-code status-message
Header-Name: Header-Value
Header-Name: Header-Value

Response body
```

### Exemple

```http
HTTP/1.1 200 OK
Date: Fri, 13 Mar 2015 11:26:05 GMT
Cache-Control: private, max-age=0
Content-Type: text/html; charset=UTF-8
Content-Encoding: gzip
Server: nginx
Content-Length: 258

<html>
  <body>
    <h1>Hello World</h1>
  </body>
</html>
```

---

## Status Line

La première ligne d’une réponse HTTP est appelée la **status line**.

```http
HTTP/1.1 200 OK
```

Elle contient :

```text
HTTP/1.1 = version du protocole.
200      = code de statut HTTP.
OK       = message de statut lisible.
```

---

## Codes HTTP courants

| Code | Signification |
|---|---|
| 200 OK | La requête a réussi et le serveur a retourné la ressource demandée |
| 201 Created | Une ressource a été créée correctement |
| 204 No Content | La requête a réussi, mais il n’y a pas de corps de réponse |
| 301 Moved Permanently | La ressource a été déplacée définitivement vers une autre URL |
| 302 Found | La ressource est temporairement disponible à une autre URL |
| 303 See Other | Le client doit récupérer une autre URL avec GET |
| 307 Temporary Redirect | Redirection temporaire ; la méthode HTTP doit être conservée |
| 400 Bad Request | Le serveur ne peut pas traiter la requête car elle est mal formée |
| 401 Unauthorized | Une authentification est requise ou les identifiants sont invalides |
| 403 Forbidden | Le serveur comprend la requête, mais refuse l’accès |
| 404 Not Found | La ressource demandée n’existe pas |
| 405 Method Not Allowed | La méthode HTTP n’est pas autorisée pour cette ressource |
| 429 Too Many Requests | Le client a envoyé trop de requêtes ; indique souvent un rate limiting |
| 500 Internal Server Error | Le serveur a rencontré une erreur inattendue |
| 502 Bad Gateway | Un gateway ou proxy a reçu une réponse invalide d’un serveur upstream |
| 503 Service Unavailable | Le service est temporairement indisponible ou surchargé |

---

## Response Headers

Les response headers fournissent des informations sur la réponse, le serveur, le caching, les cookies et le contenu.

| Header | Fonction |
|---|---|
| Date | Indique quand le serveur a généré la réponse |
| Content-Type | Spécifie le type de contenu de la réponse |
| Content-Length | Indique la taille du corps de réponse en octets |
| Content-Encoding | Spécifie la compression appliquée, comme gzip |
| Server | Identifie le logiciel ou banner du serveur web |
| Set-Cookie | Indique au navigateur d’enregistrer un cookie |
| Cache-Control | Définit les règles de cache |
| Location | Spécifie la destination d’une redirection |
| Strict-Transport-Security | Indique au navigateur d’utiliser HTTPS à l’avenir |
| Access-Control-Allow-Origin | Définit quelles origines peuvent accéder à une ressource via CORS |

---

## Date Header

Le header `Date` indique quand le serveur a généré la réponse.

```http
Date: Fri, 13 Mar 2015 11:26:05 GMT
```

Il aide les clients et systèmes intermédiaires à évaluer la fraîcheur d’une réponse et à synchroniser les informations liées au temps.

---

## Content-Type Header

Le header `Content-Type` indique le type de données renvoyé par le serveur.

```http
Content-Type: text/html; charset=UTF-8
```

Exemples :

```text
text/html                 = page HTML.
application/json          = données JSON.
application/xml           = données XML.
text/plain                = texte brut.
image/png                 = image PNG.
application/pdf           = document PDF.
application/javascript    = contenu JavaScript.
```

Les navigateurs utilisent ce header pour savoir comment traiter et afficher la réponse.

---

## Content-Length Header

Le header `Content-Length` indique la taille du corps de réponse en octets.

```http
Content-Length: 258
```

Il aide le navigateur à savoir quelle quantité de contenu attendre.

---

## Content-Encoding Header

Le header `Content-Encoding` spécifie la compression appliquée au corps de la réponse.

```http
Content-Encoding: gzip
```

Le navigateur utilise ce header pour décompresser correctement la réponse.

Valeurs courantes :

```text
gzip
deflate
br
```

---

## Server Header

Le header `Server` peut révéler le logiciel ou le banner du serveur web.

```http
Server: Apache/2.4.57
```

Autres exemples :

```text
Server: nginx
Server: Microsoft-IIS/10.0
Server: gws
```

### Note de sécurité

Des banners détaillés peuvent aider un attaquant à identifier des versions et vulnérabilités connues.

---

## Location Header

Le header `Location` est généralement utilisé avec les redirections.

```http
HTTP/1.1 302 Found
Location: https://example.com/login
```

Le navigateur suit la localisation et demande la nouvelle URL.

---

## Set-Cookie Header

Le header `Set-Cookie` indique au navigateur de stocker un cookie.

```http
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Lax
```

Il est souvent utilisé pour gérer les sessions après une connexion.

---

# Cache-Control

Le header `Cache-Control` définit comment le navigateur et les caches intermédiaires peuvent stocker et réutiliser une réponse.

```http
Cache-Control: private, max-age=0
```

Directives courantes :

| Directive | Signification |
|---|---|
| `public` | La réponse peut être mise en cache par les navigateurs et caches partagés |
| `private` | La réponse est destinée à un utilisateur et ne doit pas être stockée par des proxies partagés |
| `no-cache` | La réponse peut être stockée, mais doit être revalidée avec le serveur avant réutilisation |
| `no-store` | La réponse ne doit pas être stockée par les navigateurs ni intermédiaires |
| `max-age=<seconds>` | Définit combien de temps la réponse peut rester en cache |

### Note de sécurité

Les pages sensibles, comme les pages de profil ou de paiement, doivent utiliser des règles de cache restrictives.

```http
Cache-Control: no-store
```

---

# Cookies et sessions

## Qu’est-ce qu’une session ?

Une session permet à un site web de maintenir un état temporaire entre un utilisateur et le serveur.

Les sessions permettent au serveur de mémoriser des informations spécifiques à l’utilisateur pendant qu’il navigue entre différentes pages.

Elles sont souvent utilisées pour :

- L’authentification.
- Les paniers.
- Les préférences utilisateur.
- Les formulaires en plusieurs étapes.
- L’état d’accès temporaire.

## Qu’est-ce qu’un cookie ?

Un cookie est une petite donnée envoyée par un site web au navigateur.

Le navigateur le stocke et le renvoie au serveur dans les requêtes suivantes.

```http
Set-Cookie: session=abc123
```

Ensuite, le navigateur peut envoyer :

```http
Cookie: session=abc123
```

---

## Attributs des cookies

Les attributs définissent la portée, la durée de vie et le comportement de sécurité des cookies.

| Attribut | Fonction |
|---|---|
| Name | Identifiant unique du cookie |
| Value | Donnée stockée dans le cookie |
| Domain | Définit quel domaine peut recevoir le cookie |
| Path | Définit quel chemin URL peut recevoir le cookie |
| Expires / Max-Age | Définit la durée de vie du cookie |
| Secure | Envoie le cookie uniquement via HTTPS |
| HttpOnly | Empêche JavaScript d’accéder au cookie |
| SameSite | Contrôle le comportement du cookie lors des requêtes cross-site |
| Priority | Influence la priorité de suppression des cookies par le navigateur |
| Size | Taille maximale du nom, de la valeur et des métadonnées du cookie |

### Exemple de cookie sécurisé

```http
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Strict; Path=/
```

```text
Secure          = le cookie est envoyé uniquement par HTTPS.
HttpOnly        = JavaScript ne peut pas lire le cookie.
SameSite=Strict = le navigateur restreint l’envoi cross-site du cookie.
Path=/          = le cookie est disponible sur l’ensemble du site.
```

---

# Security Headers

Les security headers aident les navigateurs à appliquer des protections supplémentaires.

| Header | Objectif |
|---|---|
| Content-Security-Policy | Restreint les sources de contenu autorisées et aide à réduire l’injection de scripts |
| Strict-Transport-Security | Force l’utilisation de HTTPS pour les futures connexions |
| X-Frame-Options | Contrôle si une page peut être affichée dans un frame ou iframe ; aide contre le clickjacking |
| Referrer-Policy | Contrôle la quantité d’information URL envoyée dans le header Referer |
| X-Content-Type-Options | Empêche le MIME-type sniffing |
| Permissions-Policy | Contrôle l’accès aux fonctions du navigateur comme la caméra, le microphone et la géolocalisation |

### Content-Security-Policy

```http
Content-Security-Policy: default-src 'self'
```

Cela restreint le chargement de contenu à la même origine, sauf si d’autres sources sont explicitement autorisées.

### Strict-Transport-Security

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

Indique au navigateur d’utiliser HTTPS pour le domaine pendant la période indiquée.

### X-Frame-Options

```http
X-Frame-Options: DENY
```

Empêche la page d’être affichée dans des frames ou iframes.

### Referrer-Policy

```http
Referrer-Policy: strict-origin-when-cross-origin
```

Réduit l’exposition inutile d’informations URL lors de la navigation vers d’autres origines.

---

# HTTPS

## Qu’est-ce que HTTPS ?

**HTTPS** signifie **Hypertext Transfer Protocol Secure**.

HTTPS est la version sécurisée de HTTP et fournit une communication chiffrée entre un client et un serveur web.

HTTPS exécute HTTP sur **SSL/TLS**.

```text
Requête / Réponse HTTP
          |
          v
Couche de chiffrement SSL / TLS
          |
          v
TCP
```

### Architecture HTTPS

```text
[ Navigateur ]
     |
     | Données HTTP protégées par TLS
     v
[ Internet ]
     |
     | Communication chiffrée
     v
[ Serveur Web ]
```

---

## Pourquoi HTTP est non sécurisé

Par défaut, le trafic HTTP est envoyé en clair.

Un attaquant capable d’intercepter le trafic HTTP peut lire ou modifier :

- Les noms d’utilisateur.
- Les mots de passe.
- Les cookies de session.
- Les données de formulaires.
- Les tokens API.
- Les informations personnelles.
- Le contenu des pages web.

HTTP ne fournit pas de chiffrement fort, de protection d’intégrité ni d’authentification robuste entre le navigateur et le serveur.

---

## SSL et TLS

SSL (*Secure Sockets Layer*) et TLS (*Transport Layer Security*) sont des protocoles cryptographiques utilisés pour fournir une communication sécurisée sur un réseau.

TLS est le protocole moderne. SSL est l’ancien terme souvent utilisé de façon informelle lorsqu’on parle de certificats HTTPS et de trafic web chiffré.

HTTPS fournit :

- **Confidentialité :** Un attaquant ne peut pas facilement lire le trafic chiffré.
- **Intégrité :** Un attaquant ne peut pas modifier les données en transit sans détection.
- **Authentification :** Les certificats aident le navigateur à vérifier l’identité du serveur.

---

## Avantages de HTTPS

### Chiffrement des données en transit

HTTPS chiffre les données transmises entre le navigateur et le serveur.

Même si un attaquant intercepte le trafic, il ne devrait pas pouvoir lire le contenu chiffré.

### Protection contre l’écoute

HTTPS aide à protéger les données sensibles contre l’interception, comme :

- Les identifiants.
- Les informations de cartes bancaires.
- Les données personnelles.
- Les tokens de session.
- Les messages privés.
- Les clés API.

### Protection contre la manipulation

HTTPS réduit le risque qu’un attaquant modifie le trafic HTTP pendant son transit.

Par exemple, cela aide à empêcher un attaquant sur le réseau d’injecter du JavaScript malveillant dans une réponse HTTP non chiffrée.

---

## HTTPS ne corrige pas les vulnérabilités web

HTTPS est essentiel, mais il ne protège pas contre les défauts présents dans l’application web.

Les vulnérabilités suivantes peuvent exister même avec HTTPS :

- SQL injection.
- Cross-site scripting.
- Broken access control.
- CSRF.
- SSRF.
- Vulnérabilités d’upload de fichiers.
- Authentification faible.
- Gestion de sessions non sécurisée.
- APIs non sécurisées.
- Failles de logique métier.

```text
HTTPS protège les données pendant leur trajet entre le client et le serveur.

HTTPS ne protège pas automatiquement une application contre du code non sécurisé,
des contrôles d’accès faibles, de mauvaises configurations ou des composants vulnérables.
```

---

# Énumération des méthodes HTTP

Pendant une évaluation de sécurité autorisée, il peut être utile d’identifier les méthodes HTTP acceptées par un serveur ou une ressource.

## OPTIONS avec cURL

```bash
curl -X OPTIONS -i http://target.local/  # envoie une requête OPTIONS et affiche les headers complets de la réponse.
```

Recherche le header `Allow` :

```http
Allow: GET, POST, OPTIONS
```

## Script Nmap pour les méthodes HTTP

```bash
nmap -p 80,443 --script http-methods <TARGET_IP>  # vérifie les méthodes HTTP supportées sur les ports 80 et 443.
```

```text
-p 80,443              = scanne les ports HTTP et HTTPS communs.
--script http-methods  = utilise le script Nmap d’énumération des méthodes HTTP.
<TARGET_IP>            = adresse IP ou hostname cible.
```

Teste uniquement les systèmes inclus dans le périmètre autorisé.

---

# Commandes cURL utiles

`curl` est un outil en ligne de commande permettant d’envoyer des requêtes HTTP et d’afficher les réponses du serveur.

```bash
curl http://example.com  # récupère le contenu de la page.
curl -I http://example.com  # récupère uniquement les headers de réponse.
curl -i http://example.com  # récupère les headers et le corps de réponse.
curl -L http://example.com  # suit les redirections HTTP.
curl -A "Custom User Agent" http://example.com  # envoie un User-Agent personnalisé.
```

## Envoyer une requête POST

```bash
curl -X POST -d "param1=value1&param2=value2" http://example.com/api  # envoie des données de formulaire via une requête HTTP POST.
```

```text
-X POST = spécifie la méthode HTTP POST.
-d       = envoie des données dans le corps de la requête.
```

## Envoyer des données JSON

```bash
curl -X POST http://example.com/api \
  -H "Content-Type: application/json" \
  -d '{"username":"student","role":"user"}'  # envoie des données JSON dans une requête POST.
```

## Utiliser Basic Authentication

```bash
curl -u username:password http://api.example.com/data  # envoie des identifiants via HTTP Basic Authentication.
```

## Télécharger un fichier

```bash
curl -O http://example.com/file.txt  # télécharge file.txt en utilisant son nom distant.
```

## Uploader un fichier

```bash
curl --upload-file test.txt http://example.com/upload/test.txt  # envoie test.txt avec une requête HTTP PUT si le serveur le permet.
```

---

# Outils de base pour l’évaluation web

## Nmap

Nmap aide à identifier les ports ouverts, services, versions et informations liées à HTTP.

```bash
nmap -sV -p 80,443 <TARGET_IP>  # détecte les versions de services sur les ports HTTP et HTTPS.
```

```bash
nmap -p 80,443 --script http-title,http-headers <TARGET_IP>  # récupère les titres de pages et headers HTTP.
```

## DIRB

DIRB est un outil d’énumération de répertoires et fichiers.

Il utilise des wordlists pour rechercher des répertoires et fichiers potentiellement cachés sur un serveur web.

```bash
dirb http://target.local  # commence une énumération de répertoires et fichiers contre le site web cible.
```

Exemples de résultats intéressants :

```text
/admin
/login
/uploads
/backups
/config
/robots.txt
/.git
```

## Burp Suite

Burp Suite est une plateforme de tests de sécurité pour applications web.

Elle permet de :

- Intercepter le trafic HTTP/S.
- Inspecter les requêtes et réponses.
- Modifier les requêtes manuellement.
- Renvoyer des requêtes.
- Tester des paramètres.
- Analyser les cookies et headers.
- Cartographier la surface d’attaque.

Composants utiles de Burp Suite :

```text
Proxy      = intercepte le trafic du navigateur.
Repeater   = modifie et renvoie manuellement les requêtes.
Intruder   = automatise des variations contrôlées de requêtes.
Decoder    = encode et décode des données.
Comparer   = compare les requêtes et réponses.
```

---

# Flux pratique HTTP/S

Un flux d’évaluation HTTP/S de base serait :

1. Identifier le serveur web et les ports ouverts.
2. Naviguer dans l’application et cartographier ses fonctionnalités.
3. Inspecter les requêtes et réponses avec les outils du navigateur ou Burp Suite.
4. Vérifier les headers HTTP, cookies, redirections et codes de statut.
5. Énumérer les méthodes HTTP supportées.
6. Examiner les security headers.
7. Vérifier si HTTPS est activé et imposé.
8. Vérifier les attributs de sécurité des cookies de session.
9. Identifier les répertoires, fichiers, APIs et paramètres.
10. Tester uniquement dans le périmètre approuvé.

---

# Points clés

- HTTP est un protocole de couche application sans état qui fonctionne sur TCP.
- HTTP utilise une architecture client-serveur : les clients envoient des requêtes et les serveurs retournent des réponses.
- Les requêtes HTTP contiennent une request line, des headers et un corps optionnel.
- Les réponses HTTP contiennent une status line, des headers et un corps optionnel.
- Les request headers importants comprennent `Host`, `User-Agent`, `Accept`, `Authorization` et `Cookie`.
- Les response headers importants comprennent `Content-Type`, `Content-Length`, `Set-Cookie`, `Cache-Control` et `Server`.
- Les méthodes HTTP définissent la manière dont les clients interagissent avec les ressources ; les principales sont GET, POST, PUT, DELETE, PATCH, HEAD et OPTIONS.
- Les cookies et sessions permettent aux applications web de maintenir l’état utilisateur.
- HTTPS utilise TLS pour fournir confidentialité, intégrité et authentification du serveur.
- HTTPS protège les données en transit, mais n’empêche pas les vulnérabilités web comme XSS, SQL injection ou broken access control.
- Des outils comme cURL, Nmap, DIRB et Burp Suite sont utiles pour comprendre et évaluer la communication HTTP/S.
