# Web Application Architecture & Components

## Architecture des applications web

L’architecture d’une application web désigne la structure, l’organisation et l’interaction des composants utilisés pour construire une application web.

Elle définit comment l’application :

- Gère les requêtes des utilisateurs.
- Traite la logique métier.
- Stocke et récupère les données.
- Communique avec des services externes.
- Envoie des réponses aux utilisateurs.

Une architecture bien conçue est importante pour assurer :

- La scalabilité.
- La maintenabilité.
- Les performances.
- La fiabilité.
- La sécurité.

Avant de réaliser une évaluation de sécurité, il est essentiel de comprendre l’architecture sous-jacente. Cela permet d’identifier les vulnérabilités, mauvaises configurations, flux de données non sécurisés et contrôles d’accès faibles.

---

## Modèle client-serveur

Les applications web utilisent généralement le **modèle client-serveur**.

L’application est divisée en deux parties principales :

- **Client :** Le front-end auquel l’utilisateur accède depuis son navigateur.
- **Serveur :** Le back-end qui traite les requêtes, applique la logique métier, communique avec les bases de données et renvoie les réponses.

### Architecture de base

```text
[ Utilisateur / Navigateur ]
           |
           | Requête HTTP / HTTPS
           v
[ Serveur Web ]
           |
           | Fichiers statiques ou requête transférée
           v
[ Serveur d’Applications / Backend ]
           |
           | Requêtes BD / Requêtes API
           v
[ Base de données / Services externes ]
           |
           | Réponse des données
           v
[ Serveur d’Applications / Backend ]
           |
           | Réponse HTTP / HTTPS
           v
[ Utilisateur / Navigateur ]
```

---

## Composants principaux

| Composant | Fonction |
|---|---|
| Interface utilisateur (UI) | Partie visuelle avec laquelle les utilisateurs interagissent : pages web, formulaires, menus, boutons et champs |
| Technologies client-side | Technologies exécutées dans le navigateur, comme HTML, CSS et JavaScript |
| Serveur web | Reçoit les requêtes HTTP et sert le contenu statique comme HTML, CSS, JavaScript, images et fichiers |
| Serveur d’applications | Exécute le code server-side, traite la logique métier, les requêtes dynamiques et communique avec les bases de données |
| Technologies server-side | Langages et frameworks utilisés pour traiter les requêtes, comme PHP, Python, Java, Ruby ou Node.js |
| Serveur de base de données | Stocke et gère les données : utilisateurs, produits, publications, configurations et enregistrements |
| Logique applicative | Règles qui définissent le fonctionnement : authentification, validation, autorisation et workflows |
| APIs | Interfaces permettant aux applications et services d’échanger des données de manière programmatique |

---

## Traitement côté client

Le traitement client-side s’exécute sur l’appareil de l’utilisateur, généralement dans le navigateur.

Il affiche l’interface et gère les interactions sans devoir toujours contacter le serveur.

### Technologies client-side principales

```text
HTML          = définit la structure et le contenu d’une page.
CSS           = définit l’apparence, la mise en page, les couleurs et les polices.
JavaScript    = ajoute l’interactivité et le comportement dynamique.
Cookies       = stockent de petites données dans le navigateur, souvent pour les sessions.
Local Storage = stocke des données pouvant persister après la fermeture du navigateur.
```

### Tâches courantes

- Afficher des pages web.
- Gérer les boutons, menus et formulaires.
- Effectuer une validation d’entrée basique.
- Mettre à jour les éléments de la page dynamiquement.
- Envoyer des requêtes asynchrones à des APIs.
- Stocker les préférences de l’utilisateur.
- Gérer l’état de l’application côté navigateur.

### Note de sécurité

Les contrôles client-side ne doivent jamais être considérés comme suffisants.

Un utilisateur ou un attaquant peut modifier :

- Le HTML.
- Le JavaScript.
- Les champs cachés des formulaires.
- Les requêtes du navigateur.
- Les cookies.
- Les valeurs de Local Storage.

Les validations critiques, l’autorisation et les contrôles de sécurité doivent donc toujours être appliqués côté serveur.

---

## Traitement côté serveur

Le traitement server-side s’exécute sur le serveur distant qui héberge l’application.

Le serveur reçoit les requêtes des clients, applique la logique métier, récupère les données, effectue les contrôles de sécurité et génère des réponses.

### Technologies courantes

```text
PHP                  = langage très utilisé pour les applications web dynamiques.
Python               = souvent utilisé avec Django ou Flask.
Java                 = utilisé avec des frameworks et serveurs d’entreprise.
Ruby                 = souvent utilisé avec Ruby on Rails.
JavaScript / Node.js = permet d’exécuter JavaScript sur le serveur.
```

### Tâches courantes

- Traiter les connexions utilisateur.
- Valider les identifiants.
- Appliquer des règles d’autorisation.
- Interroger les bases de données.
- Traiter des paiements.
- Uploader et stocker des fichiers.
- Envoyer des e-mails.
- Appeler des APIs externes.
- Générer du HTML dynamique ou des réponses JSON.
- Enregistrer l’activité et les événements de sécurité.

### Avantage de sécurité

Le traitement côté serveur est plus sûr pour les opérations sensibles, car l’utilisateur ne peut pas modifier directement le code exécuté sur le serveur.

Cependant, le code server-side peut contenir des vulnérabilités telles que :

- SQL injection.
- Command injection.
- Server-Side Request Forgery.
- File inclusion.
- Désérialisation non sécurisée.
- Broken access control.
- Contournement d’authentification.

---

## Serveur web vs serveur d’applications

### Serveur web

Un serveur web reçoit les requêtes HTTP ou HTTPS et sert le contenu statique.

Exemples :

```text
Apache HTTP Server
Nginx
Microsoft IIS
```

Il sert généralement :

- Des fichiers HTML.
- Des fichiers CSS.
- Des fichiers JavaScript.
- Des images.
- Des polices.
- Des documents téléchargeables.
- Des ressources statiques.

### Serveur d’applications

Un serveur d’applications exécute le code server-side et gère les requêtes dynamiques.

Il :

- Traite la logique de l’application.
- Gère les actions utilisateur.
- Communique avec les bases de données.
- Génère les pages dynamiques.
- Renvoie les réponses API.
- Applique les règles métier.

Dans une petite application, les deux serveurs peuvent être sur la même machine. Dans les environnements plus importants, ils peuvent être séparés pour des raisons de performance et de sécurité.

---

## Bases de données

Les bases de données stockent et gèrent les informations utilisées par les applications web.

Données courantes :

- Comptes utilisateurs.
- Hash de mots de passe.
- Informations produits.
- Données clients.
- Commandes.
- Publications et commentaires.
- Configurations.
- Données de session.
- Logs d’audit.

### Technologies fréquentes

```text
MySQL
MariaDB
PostgreSQL
Microsoft SQL Server
Oracle Database
MongoDB
```

### Importance pour la sécurité

Les bases de données sont des cibles à forte valeur, car elles peuvent contenir des informations sensibles.

Risques fréquents :

- SQL injection.
- Identifiants de base de données faibles.
- Privilèges excessifs.
- Ports de base de données exposés.
- Données sensibles non chiffrées.
- Backups exposés.
- Messages d’erreur détaillés.

---

## Logique applicative

La logique applicative représente les règles et procédures contrôlant le fonctionnement de l’application.

Exemples :

- Inscription d’utilisateurs.
- Connexion et déconnexion.
- Réinitialisation de mot de passe.
- Contrôle d’accès basé sur les rôles.
- Calcul du panier.
- Validation des paiements.
- Règles d’upload de fichiers.
- Validation de données.
- Gestion de comptes.
- Workflows administratifs.

### Importance pour la sécurité

Les vulnérabilités de logique métier apparaissent lorsqu’un attaquant détourne une fonctionnalité légitime de manière imprévue.

Exemples :

- Modifier le prix d’un produit dans une requête.
- Réutiliser un code de réduction.
- Ignorer une étape de paiement.
- Accéder à la facture d’un autre utilisateur en modifiant un ID.
- Modifier des rôles de compte via des requêtes manipulées.
- Contourner un workflow d’approbation.

---

## Communication HTTP et flux de données

Les applications web communiquent sur Internet via **HTTP** ou **HTTPS**.

Quand un utilisateur clique sur un lien, soumet un formulaire ou charge une page, le navigateur envoie une requête HTTP au serveur.

Le serveur traite la requête, peut consulter une base de données ou une API externe, puis renvoie une réponse HTTP au navigateur.

### Flux de requête de base

```text
1. L’utilisateur saisit une URL ou clique sur un lien.
2. Le navigateur envoie une requête HTTP/HTTPS.
3. Le serveur web reçoit la requête.
4. Le serveur d’applications traite la requête.
5. Le backend consulte une base de données ou une API si nécessaire.
6. Le backend génère une réponse.
7. Le serveur envoie une réponse HTTP/HTTPS.
8. Le navigateur rend la réponse pour l’utilisateur.
```

### Exemple de requête HTTP

```http
GET /profile?id=10 HTTP/1.1
Host: example.com
Cookie: session=abc123
```

### Exemple de réponse HTTP

```http
HTTP/1.1 200 OK
Content-Type: text/html

<html>
  <body>
    <h1>User Profile</h1>
  </body>
</html>
```

---

## Comment les pages web sont rendues

Lorsqu’un utilisateur visite une adresse web, le navigateur demande une ressource au serveur.

### Processus de rendu

```text
1. L’utilisateur visite une URL.
2. Le navigateur demande la page au serveur.
3. Le serveur retourne un document HTML.
4. Le navigateur analyse le HTML.
5. Le navigateur télécharge CSS, JavaScript, images et polices.
6. Le navigateur analyse le CSS et construit les styles.
7. Le navigateur exécute JavaScript.
8. JavaScript peut modifier la page et demander des données supplémentaires à une API.
9. Le navigateur rend la page finale.
```

### Modèle de rendu du navigateur

```text
[ Réponse HTML ]
       |
       v
[ Parser HTML ] ------> Construit le DOM
       |
       +-----> Télécharge les fichiers CSS
       |              |
       |              v
       |         [ Parser CSS ]
       |
       +-----> Télécharge les fichiers JavaScript
                      |
                      v
              [ Moteur JavaScript ]
                      |
                      v
                Modifie le DOM
                      |
                      v
             [ Page web rendue ]
```

### DOM

Le **DOM** (*Document Object Model*) est une représentation structurée de la page web créée par le navigateur.

JavaScript peut lire et modifier le DOM de manière dynamique.

Par exemple, JavaScript peut :

- Modifier le texte d’une page.
- Afficher ou masquer des éléments.
- Modifier des formulaires.
- Ajouter des boutons.
- Charger de nouvelles données.
- Envoyer des requêtes API.
- Modifier le comportement du navigateur.

---

## Échange de données

L’**échange de données** est le processus de transfert d’informations entre applications, systèmes ou services.

Il permet à des systèmes ayant des langages, structures de données, systèmes d’exploitation ou plateformes différents de communiquer.

Les applications web échangent souvent des données avec :

- Des bases de données.
- Des APIs externes.
- Des applications mobiles.
- Des passerelles de paiement.
- Des fournisseurs d’authentification.
- Des services cloud.
- Des systèmes internes.
- Des microservices.

---

## APIs

Une **API** (*Application Programming Interface*) permet à différents systèmes logiciels de communiquer et d’échanger des données.

Une application web peut utiliser une API pour :

- Traiter des paiements.
- Envoyer des e-mails.
- Obtenir des données météo.
- S’authentifier avec Google ou Microsoft.
- Accéder à des cartes.
- Obtenir des informations sur des produits.
- Connecter une application mobile au backend.

### Importance pour la sécurité

Les APIs peuvent exposer des données ou fonctionnalités sensibles si elles sont mal sécurisées.

Vérifications importantes :

- Authentification.
- Autorisation.
- Validation des tokens.
- Rate limiting.
- Validation des entrées.
- Gestion des erreurs.
- Exposition de données.
- Énumération des endpoints.
- Logging et monitoring.

---

## JSON

**JSON** (*JavaScript Object Notation*) est un format léger d’échange de données couramment utilisé par les applications web et APIs.

Il est facile à lire pour les humains et à traiter pour les applications.

### Exemple JSON

```json
{
  "username": "student",
  "role": "user",
  "email": "student@example.com"
}
```

JSON est habituellement utilisé lorsqu’un navigateur communique avec une API REST.

---

## XML

**XML** (*eXtensible Markup Language*) est un format d’échange de données utilisant des balises pour définir la structure.

Il est souvent utilisé pour :

- Les fichiers de configuration.
- Les systèmes hérités.
- Les services web SOAP.
- Les intégrations d’entreprise.

### Exemple XML

```xml
<user>
  <username>student</username>
  <role>user</role>
  <email>student@example.com</email>
</user>
```

### Importance pour la sécurité

Les applications traitant XML peuvent être vulnérables à XML External Entity si les parseurs XML sont configurés de manière non sécurisée.

---

## REST

**REST** (*Representational State Transfer*) est un style architectural couramment utilisé pour les APIs web.

Les APIs REST utilisent généralement les méthodes HTTP standards :

```text
GET     = récupère des données.
POST    = crée des données.
PUT     = remplace ou met à jour des données.
PATCH   = met partiellement à jour des données.
DELETE  = supprime des données.
```

### Exemple d’endpoints REST

```text
GET    /api/users          # récupère les utilisateurs.
GET    /api/users/10       # récupère l’utilisateur 10.
POST   /api/users          # crée un utilisateur.
PUT    /api/users/10       # met à jour l’utilisateur 10.
DELETE /api/users/10       # supprime l’utilisateur 10.
```

### Importance pour la sécurité

Lors des tests, vérifie que les contrôles d’autorisation sont appliqués à chaque endpoint et à chaque méthode HTTP.

---

## SOAP

**SOAP** (*Simple Object Access Protocol*) est un protocole permettant d’échanger des informations structurées entre systèmes.

SOAP utilise généralement XML et fournit une méthode standardisée de communication entre services web.

Il est souvent présent dans les environnements d’entreprise et les applications héritées.

---

## Technologies de sécurité

### Authentification et autorisation

L’authentification confirme l’identité d’un utilisateur.

L’autorisation détermine les zones, actions et données auxquelles il peut accéder.

Exemples :

- Authentification par nom d’utilisateur et mot de passe.
- Authentification multifacteur.
- Cookies de session.
- Tokens JWT.
- Contrôle d’accès basé sur les rôles.
- Listes de contrôle d’accès.

---

### SSL et TLS

**TLS** chiffre la communication entre le client et le serveur.

HTTPS utilise HTTP sur TLS.

```text
HTTP  = les données peuvent circuler sans chiffrement.
HTTPS = les données sont chiffrées pendant le transit.
```

TLS aide à protéger contre :

- L’interception d’identifiants.
- Le vol de cookies de session sur le réseau.
- Les attaques Man-in-the-Middle.
- L’exposition de données sensibles en transit.

---

## Technologies externes

### Content Delivery Networks

Un **Content Delivery Network**, ou CDN, distribue du contenu statique sur plusieurs serveurs situés dans différentes régions géographiques.

Les CDNs distribuent souvent :

- Images.
- Fichiers CSS.
- Bibliothèques JavaScript.
- Polices.
- Vidéos.
- Ressources téléchargeables.

Avantages :

- Chargement des pages plus rapide.
- Réduction de la charge sur le serveur d’origine.
- Meilleure disponibilité.
- Plus grande fiabilité.
- Une certaine protection contre les volumes de trafic élevés.

---

### Bibliothèques et frameworks tiers

Les applications web utilisent souvent des bibliothèques et frameworks tiers pour accélérer le développement et ajouter des fonctionnalités.

Exemples :

```text
Frameworks frontend : React, Angular, Vue.js
Frameworks backend  : Django, Flask, Laravel, Spring
Bibliothèques JS    : jQuery, Lodash
CMS                 : WordPress, Drupal, Joomla
```

### Importance pour la sécurité

Les composants tiers peuvent introduire des vulnérabilités s’ils sont obsolètes ou non sécurisés.

Lors d’une évaluation, identifie :

- Les versions de frameworks.
- Les technologies serveur.
- Les bibliothèques JavaScript.
- Les plugins CMS.
- Les dépendances.
- Les composants ayant des vulnérabilités connues.

---

## Vue de sécurité d’une application web

Lors d’une évaluation de sécurité, il est important de savoir quel composant traite chaque action.

```text
[ Navigateur ]
      |
      | Client-side : HTML, CSS, JavaScript, Cookies
      v
[ Serveur Web ]
      |
      | Apache, Nginx, IIS
      v
[ Serveur d’Applications ]
      |
      | PHP, Python, Java, Ruby, Node.js
      v
[ Base de données ]
      |
      | MySQL, PostgreSQL, MSSQL, Oracle
      v
[ APIs / Services externes ]
```

Cela aide à identifier les surfaces d’attaque possibles :

| Composant | Zones de sécurité |
|---|---|
| Navigateur / Client | XSS, manipulation du DOM, Local Storage non sécurisé, tokens exposés |
| Serveur web | Mauvaises configurations, fichiers exposés, TLS faible, directory listing |
| Serveur d’applications | Failles d’authentification, problèmes de contrôle d’accès, injections, erreurs de logique métier |
| Base de données | SQL injection, permissions faibles, exposition de données, backups non sécurisés |
| APIs | Broken object authorization, problèmes de tokens, exposition excessive de données, absence de rate limiting |
| Composants tiers | Bibliothèques obsolètes, plugins vulnérables, dépendances non sécurisées |

---

## Points clés

- L’architecture web définit comment les composants interagissent pour traiter les requêtes et gérer les données.
- Les applications web utilisent généralement un modèle client-serveur.
- Les technologies client-side comprennent HTML, CSS, JavaScript, les cookies et Local Storage.
- Les technologies server-side comprennent les serveurs web, serveurs d’applications, bases de données et langages côté serveur.
- La validation sensible, l’autorisation et la logique métier doivent toujours être appliquées côté serveur.
- HTTP et HTTPS servent à la communication entre clients et serveurs.
- Les APIs échangent souvent des données en JSON ou XML.
- Les APIs REST utilisent des méthodes comme GET, POST, PUT, PATCH et DELETE.
- Comprendre l’architecture permet au pentester d’identifier les surfaces d’attaque, vulnérabilités et mauvaises configurations.
