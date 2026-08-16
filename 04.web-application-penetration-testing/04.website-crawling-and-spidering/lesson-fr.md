# Website Crawling & Spidering

## Vue d’ensemble

Le **website crawling** et le **spidering** sont des techniques utilisées pour découvrir et cartographier le contenu accessible d’une application web.

L’objectif est d’identifier la surface d’attaque visible de l’application, notamment :

- Les pages et répertoires.
- Les fichiers et ressources statiques.
- Les chemins URL.
- Les paramètres.
- Les formulaires.
- Les endpoints API.
- Les fichiers JavaScript.
- Les pages de connexion.
- Les panneaux d’administration.
- Les fonctionnalités d’upload de fichiers.
- Les rôles et workflows utilisateurs.

Le crawling est une étape importante au début d’une évaluation de sécurité web, car il est difficile de tester correctement une application sans comprendre les fonctionnalités existantes.

### Règle importante

Tu dois uniquement effectuer du crawling sur les sites et applications qui sont explicitement inclus dans le périmètre autorisé.

Définis le périmètre avant de commencer :

- Domaines cibles.
- Sous-domaines.
- Adresses IP.
- Ports.
- Comptes utilisateurs autorisés.
- Fonctionnalités exclues.
- Vitesse et limites de crawling.
- Exigences d’authentification.

---

## Crawling vs Spidering

Les termes sont souvent utilisés comme synonymes, mais peuvent être compris ainsi :

| Terme | Signification |
|---|---|
| Crawling | Découvrir du contenu en naviguant dans les pages, en suivant les liens, en traitant les formulaires et en observant les requêtes |
| Spidering | Suivre automatiquement les liens et URLs découverts pour identifier davantage de contenu |
| Crawling passif | Cartographier le contenu à partir du trafic et des références sans envoyer de payloads intrusifs |
| Crawling actif | Demander automatiquement les pages et ressources découvertes ; peut générer davantage de trafic |
| Crawling manuel | Le pentester navigue manuellement pendant qu’un proxy enregistre les requêtes |
| AJAX spidering | Parcourir des applications riches en JavaScript en rendant les pages et en interagissant avec des éléments dynamiques |

---

## Pourquoi le crawling est important

Le crawling aide à créer une carte de l’application cible.

Sans cartographie correcte, il est facile de manquer :

- Des répertoires cachés.
- Des pages non liées.
- Des endpoints API.
- Des paramètres.
- Des fonctions administratives.
- Des routes JavaScript.
- Des workflows d’authentification.
- Des fonctionnalités propres à certains utilisateurs.
- Des uploads de fichiers.
- Des fonctions de réinitialisation de mot de passe.
- Des ressources sensibles.

### Exemple de surface d’attaque

```text
https://target.local/
|
+-- /
+-- /login
+-- /register
+-- /forgot-password
+-- /dashboard
+-- /profile
+-- /admin
+-- /uploads
+-- /api/
|   +-- /api/users
|   +-- /api/orders
|   +-- /api/profile
|
+-- /static/
    +-- /static/js/app.js
    +-- /static/css/style.css
```

---

# Crawling passif avec Burp Suite

## Qu’est-ce que le crawling passif ?

Le crawling passif dans Burp Suite cartographie le contenu visible de l’application lorsque le trafic du navigateur passe par Burp Proxy.

Il ne nécessite pas d’exploitation active. Burp observe :

- Les URLs visitées dans le navigateur.
- Les liens présents dans les réponses.
- Les formulaires.
- Les scripts.
- Les ressources référencées.
- Les paramètres.
- Les cookies.
- Les méthodes HTTP.
- Les headers de requêtes et réponses.

Le contenu découvert est ajouté à **Target > Site map**.

---

## Configuration de Burp Suite

### 1. Configurer le périmètre

Avant de naviguer sur la cible, ajoute la cible autorisée au scope de Burp.

```text
Target > Scope > Add
```

Ajoute le domaine, l’URL, l’adresse IP ou le port cible.

Exemples :

```text
https://target.local
https://*.target.local
```

### Pourquoi le scope est important

Le scope permet de :

- Éviter l’envoi de trafic vers des sites non concernés.
- Garder le Site map organisé.
- Concentrer le crawling passif sur la cible.
- Éviter de tester accidentellement des systèmes hors périmètre.

---

## 2. Utiliser Burp Browser

Burp Suite inclut un navigateur intégré déjà configuré pour utiliser Burp Proxy.

```text
Proxy > Intercept > Open Browser
```

Pour le crawling passif, il est généralement plus pratique de désactiver l’interception pendant la navigation.

```text
Proxy > Intercept > Intercept is off
```

Cela permet aux requêtes de passer par Burp sans être arrêtées une par une.

---

## 3. Naviguer manuellement dans l’application

Utilise Burp Browser pour explorer l’application comme un utilisateur normal.

Navigue dans :

- La page d’accueil.
- Les pages de login et logout.
- Les pages d’inscription.
- Les fonctions de récupération de mot de passe.
- Les profils utilisateur.
- Les formulaires de recherche.
- Les menus de navigation.
- Les pages de paramètres.
- Les formulaires d’upload.
- Les liens de téléchargement.
- Les pages administratives, si elles sont autorisées.
- Les APIs utilisées par l’application.
- Les pages contenant du contenu généré par les utilisateurs.

### Flux de crawling manuel

```text
1. Ouvre la cible dans Burp Browser.
2. Navigue dans tous les menus visibles.
3. Clique sur les liens pertinents.
4. Soumets des formulaires non destructifs.
5. Connecte-toi avec des comptes de test autorisés.
6. Répète le processus avec d’autres rôles utilisateur.
7. Consulte le Site map.
```

Burp enregistre le trafic et remplit le Site map avec le contenu visité. Il peut aussi inférer des emplacements supplémentaires à partir des liens et formulaires présents dans les réponses.

---

## 4. Examiner Target Site Map

Ouvre :

```text
Target > Site map
```

Le Site map affiche la structure découverte de l’application.

Recherche :

- Des répertoires intéressants.
- Des endpoints API.
- Des fichiers JavaScript.
- Des paramètres.
- Des méthodes HTTP.
- Des codes de réponse.
- Des cookies.
- Des redirections.
- Des chemins cachés ou grisés.
- Des endpoints administratifs.
- Des emplacements d’upload.
- Du contenu différent selon les rôles.

### Éléments utiles dans le Site Map

```text
/login
/logout
/register
/admin
/api
/api/v1/users
/uploads
/download
/profile?id=10
/search?q=test
/robots.txt
/sitemap.xml
```

Les entrées grisées peuvent être des chemins inférés depuis les réponses, mais qui n’ont pas encore été demandés par Burp. Tu peux les ouvrir manuellement dans Burp Browser s’ils sont dans le périmètre.

---

## 5. Filtrer le Site Map

Utilise les filtres du Site map pour réduire le bruit.

Filtres utiles :

- Afficher uniquement les éléments dans le scope.
- Masquer les images.
- Masquer les fichiers CSS.
- Masquer les polices.
- Masquer les bibliothèques JavaScript statiques.
- Afficher uniquement le contenu dynamique.
- Afficher uniquement les éléments demandés.
- Afficher certains codes HTTP spécifiques.

Cela permet de se concentrer sur les endpoints contenant des fonctionnalités applicatives intéressantes.

---

## Checklist de crawling avec Burp Suite

- Ajouter la cible au scope.
- Ouvrir Burp Browser.
- Désactiver l’interception pendant la navigation normale.
- Naviguer sur toutes les pages visibles.
- Suivre les liens et menus.
- Soumettre des formulaires sûrs.
- Se connecter avec des comptes autorisés.
- Tester différents rôles si disponibles.
- Examiner `Target > Site map`.
- Identifier les APIs et paramètres.
- Inspecter les fichiers JavaScript.
- Examiner les endpoints cachés ou inférés.
- Documenter les fonctionnalités importantes.

---

# Crawling avec Burp Suite Professional

Burp Suite Professional inclut des fonctionnalités de crawling automatisé.

### Démarrer un crawling automatisé

```text
Target > Site map
Clic droit sur la racine de la cible
Scan
Sélectionner Crawl
Start Scan
```

Tu peux configurer des identifiants de connexion si l’application demande une authentification.

### Note importante

Burp Suite Community Edition ne possède pas les mêmes capacités de crawling automatisé que Burp Suite Professional.

Avec Burp Community Edition, utilise principalement la navigation manuelle via Burp Proxy pour remplir le Site map.

---

# Crawling passif avec OWASP ZAP

## Qu’est-ce que OWASP ZAP ?

OWASP ZAP, aussi appelé **Zed Attack Proxy**, est un outil open source de test de sécurité d’applications web.

ZAP peut aider à :

- Intercepter le trafic HTTP/S.
- Réaliser du passive scanning.
- Faire du spidering traditionnel.
- Faire de l’AJAX spidering.
- Explorer manuellement.
- Cartographier des sites.
- Inspecter les headers.
- Analyser les cookies.
- Découvrir des APIs.

---

## Passive Scanning dans ZAP

Le passive scanner de ZAP analyse les messages HTTP et WebSocket qui passent par ZAP sans les modifier.

Il peut identifier des problèmes comme :

- Security headers absents.
- Cookies sans attributs de sécurité.
- Divulgation d’informations.
- Données sensibles dans les URLs.
- Exposition de banners serveur.
- Configuration Cache-Control faible.
- Absence de protections anti-clickjacking.
- Problèmes de configuration CORS.

### Différence importante

```text
Passive scan = analyse le trafic existant sans envoyer de payloads d’attaque.
Active scan  = envoie des payloads d’attaque et peut affecter la cible.
```

N’exécute pas d’active scan sauf s’il est explicitement autorisé.

---

## Configuration de ZAP

### 1. Créer une session

Démarre OWASP ZAP et crée ou ouvre une session.

```text
File > New Session
```

Une session stocke :

- Les URLs découvertes.
- L’historique HTTP.
- Les alertes.
- Les résultats des spiders.
- La configuration des contextes.
- La configuration de l’authentification.

---

## 2. Définir Context et Scope

Crée un contexte pour l’application cible.

```text
Sites > Clic droit sur la cible
Include in Context > New Context
```

Ajoute le domaine ou l’URL cible.

Exemples :

```text
https://target.local
https://.*\.target\.local.*
```

Ensuite, ajoute la cible au scope :

```text
Sites > Clic droit sur la cible
Include in Scope
```

### Pourquoi utiliser un Context ?

Un Context ZAP permet de définir :

- Les URLs dans le périmètre.
- Les règles d’authentification.
- Les règles de gestion de sessions.
- Les comptes utilisateur.
- Les chemins exclus.
- Les restrictions de spidering.

---

## 3. Utiliser Manual Explore

Utilise le navigateur intégré ou configure ton propre navigateur pour faire passer le trafic par ZAP.

```text
Quick Start > Manual Explore
```

Saisis l’URL cible et ouvre le navigateur.

Navigue normalement dans l’application :

- Ouvre des pages.
- Suis les liens de navigation.
- Soumets des formulaires non destructifs.
- Connecte-toi avec des comptes autorisés.
- Explore les profils et paramètres.
- Utilise les rôles disponibles.
- Déclenche des appels API.

ZAP enregistre les requêtes dans :

```text
History
Sites
```

La structure de l’application apparaît dans l’arborescence **Sites**.

---

# Traditional Spider dans OWASP ZAP

## Qu’est-ce que le Traditional Spider ?

Le Traditional Spider de ZAP demande automatiquement des pages et analyse le HTML retourné afin de découvrir :

- Des liens.
- Des formulaires.
- Des ressources.
- Des paramètres.
- Des chemins référencés.

Il est généralement rapide et fonctionne bien avec les sites HTML traditionnels.

### Démarrer Traditional Spider

```text
Sites > Clic droit sur la cible
Attack > Spider
```

Tu peux aussi utiliser :

```text
Tools > Spider
```

Configure :

- L’URL de départ.
- Le Context.
- La profondeur maximale.
- Le nombre maximal d’éléments enfants.
- Le nombre de threads.
- Les URLs exclues.
- Les options de récursivité.

Puis démarre le spider.

### Surveiller les résultats

Consulte les résultats dans :

```text
Spider tab
Sites tree
History tab
```

---

## Limites du Traditional Spider

Le Traditional Spider analyse principalement le HTML retourné par le serveur.

Il peut avoir des difficultés à découvrir le contenu des applications utilisant beaucoup :

- JavaScript.
- Les frameworks de single-page applications.
- Les modifications dynamiques du DOM.
- Le client-side routing.
- Les requêtes AJAX.
- Les boutons qui n’utilisent pas de liens traditionnels.

Pour les applications utilisant fortement JavaScript, utilise AJAX Spider en complément du Traditional Spider.

---

# AJAX Spider dans OWASP ZAP

## Qu’est-ce que AJAX Spider ?

L’**AJAX Spider** est conçu pour les applications web modernes utilisant JavaScript, AJAX, les éléments dynamiques et le rendu client-side.

Il utilise le crawler Crawljax afin de rendre les pages et d’interagir avec les applications riches en AJAX.

AJAX Spider peut découvrir des pages et états qu’un spider HTML traditionnel pourrait ne pas trouver.

### Quand l’utiliser

Utilise AJAX Spider lorsque l’application utilise :

- React.
- Angular.
- Vue.js.
- Les single-page applications.
- Les menus dynamiques.
- Les boutons JavaScript.
- Les requêtes AJAX.
- Le client-side routing.
- Le contenu chargé après la réponse initiale.

### Démarrer AJAX Spider

```text
Sites > Clic droit sur la cible
Attack > AJAX Spider
```

Tu peux aussi utiliser :

```text
Tools > AJAX Spider
```

Configure l’URL cible et le Context, puis démarre le crawling.

### Note importante

AJAX Spider est plus lent que Traditional Spider, car il rend les pages, exécute JavaScript et interagit avec du contenu dynamique.

Pour une meilleure couverture, utilise les deux :

```text
1. Navigation manuelle.
2. Traditional Spider.
3. AJAX Spider.
4. Passive scanning.
```

---

# Checklist de crawling avec ZAP

- Créer une nouvelle session ZAP.
- Définir le Context de la cible.
- Ajouter la cible au scope.
- Utiliser Manual Explore.
- Naviguer dans l’application avec des comptes autorisés.
- Examiner l’arborescence Sites.
- Exécuter Traditional Spider.
- Exécuter AJAX Spider pour les applications utilisant JavaScript.
- Attendre la fin du passive scanning.
- Examiner les alertes passives.
- Exporter ou documenter les endpoints découverts.
- Ne pas exécuter d’active scan sans autorisation.

---

# Burp Suite vs OWASP ZAP

| Fonctionnalité | Burp Suite | OWASP ZAP |
|---|---|---|
| Licence | Community Edition gratuite ; Professional Edition commerciale | Open source et gratuit |
| Crawling passif | Utilise le trafic Proxy et Site map | Utilise le trafic passant par proxy, l’arborescence Sites et le passive scanner |
| Navigation manuelle | Burp Browser et Proxy | Manual Explore et navigateur avec proxy |
| Crawling automatisé | Disponible dans Burp Suite Professional | Traditional Spider et AJAX Spider disponibles |
| Crawling JavaScript | Disponible via le crawler de Burp Professional | AJAX Spider disponible |
| Passive scanning | Analyse passive basique et Site map | Passive scanner intégré et règles passives |
| Active scanning | Fonctionnalité Professional | Disponible, mais doit être explicitement autorisé |
| Vue principale de cartographie | Target > Site map | Arborescence Sites et History |

---

# Cibles utiles à découvrir

Pendant le crawling et spidering passif, recherche :

```text
/login
/logout
/register
/forgot-password
/reset-password
/profile
/settings
/admin
/dashboard
/api
/api/v1/
/uploads
/downloads
/backups
/config
/robots.txt
/sitemap.xml
/.git
/.env
```

Identifie également :

- Les paramètres URL.
- Les champs de formulaires.
- Les cookies.
- Les tokens de session.
- Les requêtes API.
- Les endpoints JavaScript.
- Les rôles utilisateurs.
- Les méthodes HTTP.
- Les redirections.
- Les uploads de fichiers.
- Les pages d’erreur.
- Les intégrations tierces.

---

# Flux pratique

## Burp Suite Community Edition

```text
1. Ouvre Burp Suite.
2. Ajoute la cible dans Target > Scope.
3. Ouvre Burp Browser.
4. Désactive l’interception pour naviguer normalement.
5. Navigue sur toutes les fonctionnalités accessibles.
6. Connecte-toi avec des comptes autorisés.
7. Examine Target > Site map.
8. Analyse les chemins et paramètres intéressants.
9. Inspecte Proxy > HTTP history.
10. Documente les endpoints découverts.
```

## OWASP ZAP

```text
1. Ouvre OWASP ZAP.
2. Crée une nouvelle session.
3. Crée un Context pour la cible.
4. Ajoute la cible au scope.
5. Démarre Manual Explore.
6. Navigue normalement dans l’application.
7. Examine l’arborescence Sites et History.
8. Exécute Traditional Spider.
9. Exécute AJAX Spider si l’application utilise beaucoup JavaScript.
10. Attends les résultats du passive scan.
11. Examine les alertes et documente les résultats.
```

---

# Points clés

- Crawling et spidering aident à cartographier la surface d’attaque visible d’une application web.
- Le crawling passif observe le trafic et le contenu des réponses sans utiliser de payloads d’attaque.
- Burp Suite remplit `Target > Site map` lorsque le trafic passe par Burp Proxy.
- Burp Suite Community Edition dépend principalement de la navigation manuelle pour le crawling.
- OWASP ZAP fournit Manual Explore, Traditional Spider, AJAX Spider et passive scanning.
- Traditional Spider fonctionne bien avec les applications HTML classiques.
- AJAX Spider est utile pour les applications riches en JavaScript et les single-page applications.
- Le passive scanning peut identifier des problèmes de sécurité sans attaquer activement la cible.
- Définis toujours le scope, utilise des comptes autorisés et évite les active scans sans autorisation explicite.
