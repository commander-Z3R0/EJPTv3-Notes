# Introduction to Web App Security Testing

## Que sont les applications web ?

Une **application web** est une application logicielle qui s’exécute sur un serveur web et à laquelle les utilisateurs accèdent via un navigateur.

Les applications web proposent des fonctionnalités dynamiques et interactives. Elles permettent notamment d’accéder à des informations, d’envoyer des données, de s’authentifier, d’effectuer des achats, de gérer des comptes et d’interagir avec des services en ligne.

Exemples courants :

- Réseaux sociaux comme Facebook et X.
- Services de messagerie comme Gmail et Outlook.
- Sites de commerce électronique comme Amazon et eBay.
- Plateformes cloud comme Google Workspace et Microsoft 365.
- Applications bancaires.
- Portails clients.
- Systèmes de gestion de contenu.
- APIs web et plateformes SaaS.

---

## Fonctionnement des applications web

Les applications web utilisent généralement une **architecture client-serveur**.

```text
[ Utilisateur / Navigateur ]
           |
           | Requête HTTP ou HTTPS
           v
[ Serveur web / Application web ]
           |
           | Requête ou consultation
           v
[ Base de données / API / Services internes ]
           |
           | Réponse
           v
[ Serveur web / Application web ]
           |
           | Réponse HTTP ou HTTPS
           v
[ Utilisateur / Navigateur ]
```

### Composants principaux

- **Client :** Le navigateur de l’utilisateur, tel que Firefox, Chrome ou Edge.
- **Serveur web :** Reçoit les requêtes HTTP et renvoie les réponses.
- **Application web :** Contient la logique métier de l’application.
- **Base de données :** Stocke les utilisateurs, mots de passe, commandes et données métier.
- **API :** Permet à des applications ou systèmes d’échanger des données de manière programmatique.

### Technologies d’interface

Les interfaces web sont généralement construites avec :

- **HTML :** Définit la structure et le contenu d’une page.
- **CSS :** Définit l’apparence et la mise en page.
- **JavaScript :** Ajoute un comportement dynamique et interactif au navigateur.

---

## HTTP et statelessness

Les navigateurs communiquent principalement avec les serveurs au moyen de **HTTP** ou **HTTPS**.

HTTP est un protocole **sans état** (*stateless*). Chaque requête est indépendante par défaut : le serveur ne mémorise pas automatiquement les requêtes précédentes.

Les applications utilisent donc des cookies, des identifiants de session et des tokens pour conserver l’état de l’utilisateur.

### Exemple

```text
1. L’utilisateur se connecte avec un nom d’utilisateur et un mot de passe.
2. Le serveur valide les identifiants.
3. Le serveur crée une session ou un token.
4. Le navigateur stocke le cookie ou le token.
5. Le navigateur l’envoie avec les requêtes suivantes.
6. Le serveur reconnaît l’utilisateur authentifié.
```

Si la gestion des sessions est faible, un attaquant peut tenter de détourner une session, de fixer une session ou de voler des tokens.

---

## Sécurité des applications web

La sécurité des applications web vise à les protéger contre les vulnérabilités, les attaques, les accès non autorisés, les fuites de données et les interruptions de service.

L’objectif principal est de préserver la **triade CIA** :

- **Confidentialité :** Les données sensibles ne sont accessibles qu’aux utilisateurs autorisés.
- **Intégrité :** Les données ne peuvent pas être modifiées sans autorisation.
- **Disponibilité :** L’application reste accessible aux utilisateurs légitimes.

Les applications web sont des cibles attractives parce qu’elles sont souvent publiques et traitent des données importantes :

- Noms d’utilisateur et mots de passe.
- Données personnelles.
- Informations de paiement.
- Données financières.
- Documents internes.
- Données métier.
- Propriété intellectuelle.
- Tokens d’authentification.

---

## Pourquoi la sécurité web est importante

Une application vulnérable peut provoquer :

- L’exposition de données sensibles.
- La compromission de comptes.
- Le vol financier.
- Une atteinte à la réputation.
- Des sanctions réglementaires.
- Le vol de propriété intellectuelle.
- Des interruptions de service.
- Une perte de confiance des clients.
- Le defacement d’un site web.
- La manipulation de données.

Par exemple, une vulnérabilité SQL injection peut permettre à un attaquant d’accéder à une base contenant des comptes utilisateurs, des adresses e-mail, des hash de mots de passe ou des données de paiement.

---

## Pratiques de sécurité web

### Authentification et autorisation

L’authentification vérifie **qui est l’utilisateur**.

L’autorisation détermine **à quelles ressources l’utilisateur authentifié peut accéder**.

```text
Authentification : « Qui es-tu ? »
Autorisation : « À quoi as-tu le droit d’accéder ? »
```

Bonnes pratiques :

- Imposer des mots de passe robustes.
- Utiliser l’authentification multifacteur.
- Mettre en place une réinitialisation de mot de passe sécurisée.
- Appliquer un contrôle d’accès basé sur les rôles.
- Vérifier l’autorisation pour chaque action sensible.
- Empêcher les utilisateurs d’accéder aux ressources d’autres utilisateurs.

---

### Validation des entrées et encodage des sorties

L’application doit valider toutes les entrées contrôlées par l’utilisateur.

Les sources d’entrée peuvent inclure :

- Les paramètres d’URL.
- Les champs de formulaire.
- Les headers HTTP.
- Les cookies.
- Les requêtes JSON.
- Les fichiers uploadés.
- Les requêtes API.

La validation des entrées aide à prévenir les attaques comme SQL injection et command injection.

L’encodage des sorties aide à prévenir les attaques comme cross-site scripting.

---

### Communication sécurisée

Les applications doivent utiliser HTTPS afin de chiffrer le trafic entre le navigateur et le serveur.

```text
HTTP   = communication non chiffrée.
HTTPS  = communication HTTP chiffrée avec TLS.
```

HTTPS protège les données en transit, comme :

- Les identifiants.
- Les cookies de session.
- Les informations de paiement.
- Les données personnelles.
- Les tokens API.

---

### Développement sécurisé

Les pratiques de développement sécurisé réduisent le risque d’introduire des vulnérabilités.

Pratiques importantes :

- Valider toutes les entrées.
- Utiliser des requêtes paramétrées.
- Éviter les fonctions dangereuses.
- Gérer les erreurs de façon sécurisée.
- Ne pas afficher les stack traces aux utilisateurs.
- Protéger les secrets et clés API.
- Utiliser des configurations sécurisées par défaut.
- Réviser le code avant le déploiement.

---

### Mises à jour régulières

Les applications web dépendent de nombreux composants :

- Serveurs web.
- Frameworks.
- Bibliothèques.
- CMS.
- Plugins.
- Bases de données.
- Systèmes d’exploitation.

Ces composants doivent être régulièrement mis à jour, car les logiciels obsolètes peuvent contenir des vulnérabilités connues.

---

### Principe du moindre privilège

Le **principe du moindre privilège** consiste à accorder aux utilisateurs, applications, services et processus uniquement les permissions strictement nécessaires.

Exemples :

- Un compte de base de données ne doit pas être administrateur sans nécessité.
- Un utilisateur standard ne doit pas accéder à une page d’administration.
- Une application web ne doit pas s’exécuter en tant que root ou Administrator.
- Les tokens API doivent avoir des permissions limitées et une date d’expiration.

---

### Web Application Firewall

Un **Web Application Firewall**, ou WAF, surveille et filtre le trafic HTTP entre les utilisateurs et l’application web.

Un WAF peut aider à détecter ou bloquer :

- Les tentatives courantes de SQL injection.
- Les payloads XSS.
- Les bots malveillants.
- Les schémas d’exploitation connus.
- Les requêtes excessives.
- Les requêtes HTTP suspectes.

Un WAF est utile, mais il ne remplace pas le développement sécurisé ni les tests de sécurité.

---

### Gestion des sessions

Une gestion correcte des sessions aide à prévenir le détournement de sessions authentifiées.

Bonnes pratiques :

- Utiliser des identifiants de session longs et aléatoires.
- Définir des attributs de cookies sécurisés.
- Régénérer l’ID de session après la connexion.
- Expirer les sessions après inactivité.
- Invalider les sessions après déconnexion.
- Utiliser HTTPS sur toutes les pages authentifiées.
- Éviter d’exposer les tokens de session dans les URLs.

---

# Tests de sécurité des applications web

## Qu’est-ce que le Web Application Security Testing ?

Les tests de sécurité web sont le processus d’évaluation d’une application afin d’identifier les vulnérabilités, faiblesses, erreurs de configuration et risques de sécurité.

L’objectif principal est de trouver et corriger les failles avant qu’un attaquant ne les exploite.

Ces tests peuvent évaluer :

- L’authentification.
- L’autorisation.
- La validation des entrées.
- La gestion des sessions.
- La sécurité des APIs.
- L’upload de fichiers.
- La configuration du serveur.
- Les composants tiers.
- La logique métier.
- La protection des données.

---

## Types de tests de sécurité

### Vulnerability Scanning

Le scan de vulnérabilités utilise des outils automatisés afin d’identifier des faiblesses connues.

Un scanner peut détecter :

- Des logiciels obsolètes.
- Des headers de sécurité absents.
- Une configuration TLS faible.
- Des indicateurs de SQL injection ou XSS.
- Des répertoires exposés.
- Des erreurs de configuration.

Le scan automatisé offre une large couverture, mais ses résultats doivent être vérifiés manuellement en raison des faux positifs et faux négatifs.

---

### Penetration Testing

Le pentesting web simule des attaques réelles de manière contrôlée et autorisée.

Le pentester cherche à :

- Identifier des vulnérabilités.
- Vérifier si elles sont exploitables.
- Déterminer l’impact d’une exploitation.
- Évaluer les contrôles de sécurité existants.
- Identifier des chemins vers des données sensibles ou des fonctions privilégiées.

L’exploitation doit toujours rester dans le périmètre et respecter les règles d’engagement.

---

### Revue de code et analyse statique

La revue de code examine le code source afin de détecter les problèmes avant le déploiement.

Le **SAST** (*Static Application Security Testing*) analyse le code sans exécuter l’application.

Problèmes fréquents :

- Identifiants hardcodés.
- Requêtes de base de données non sécurisées.
- Cryptographie faible.
- Validation d’entrée absente.
- Gestion de fichiers non sécurisée.
- Fonctions dangereuses.
- Contrôles d’autorisation faibles.

---

### Analyse dynamique

Le **DAST** (*Dynamic Application Security Testing*) teste une application en fonctionnement depuis l’extérieur.

Les outils DAST interagissent avec l’application par des requêtes et réponses HTTP.

Ils peuvent détecter :

- XSS réfléchi.
- Indicateurs de SQL injection.
- Headers de sécurité manquants.
- Cookies non sécurisés.
- Informations serveur exposées.
- Méthodes HTTP faibles.
- Redirections non sécurisées.

---

### Analyse interactive

Le **IAST** (*Interactive Application Security Testing*) analyse une application pendant son exécution.

Il combine des éléments d’analyse statique et dynamique en surveillant le comportement de l’application pendant les tests.

---

### Software Composition Analysis

Le **SCA** (*Software Composition Analysis*) identifie les bibliothèques, dépendances et composants tiers présentant des vulnérabilités connues.

C’est important car les applications modernes dépendent fortement de packages open source et de bibliothèques externes.

---

## Tests d’authentification et d’autorisation

Les tests d’authentification évaluent si les utilisateurs peuvent prouver leur identité de manière sécurisée.

Les tests d’autorisation vérifient si les utilisateurs accèdent uniquement aux fonctions et données autorisées.

Points importants :

- Politiques de mots de passe faibles.
- Identifiants par défaut.
- Absence de MFA.
- Réinitialisation de mot de passe non sécurisée.
- Énumération d’utilisateurs.
- Protection contre la force brute.
- Élévation de privilèges.
- IDOR.
- Broken access control.
- Absence de validation des rôles.

---

## Tests des entrées et sorties

Ces tests évaluent comment l’application traite les données fournies par l’utilisateur.

Vulnérabilités courantes :

- SQL injection.
- Cross-site scripting.
- Command injection.
- Server-side template injection.
- Path traversal.
- XML external entity injection.
- File inclusion.
- Désérialisation non sécurisée.

---

## Tests de gestion de sessions

Ces tests évaluent la gestion des utilisateurs authentifiés et de leurs tokens de session.

Problèmes fréquents :

- IDs de session prévisibles.
- Session fixation.
- Détournement de session.
- Tokens exposés dans des URLs.
- Attributs de cookies absents.
- Sessions qui n’expirent pas.
- Sessions toujours actives après logout.
- Tokens de réinitialisation réutilisables.

Attributs utiles pour les cookies :

```text
Secure     = le cookie est envoyé uniquement via HTTPS.
HttpOnly   = JavaScript ne peut pas accéder au cookie.
SameSite   = aide à réduire les attaques cross-site.
```

---

## Tests de sécurité des APIs

Les APIs permettent aux applications web, mobiles et services d’échanger des données.

Les tests API doivent évaluer :

- Les mécanismes d’authentification.
- Les contrôles d’autorisation.
- La validation des tokens.
- Le rate limiting.
- La validation des entrées.
- L’exposition de données sensibles.
- L’exposition excessive de données.
- Les endpoints non sécurisés.
- Le versioning.
- Les messages d’erreur.
- Le logging et la supervision.

---

# Web App Pentesting vs Web App Security Testing

| Aspect | Tests de sécurité web | Pentesting web |
|---|---|---|
| Objectif | Identifier les vulnérabilités et faiblesses | Valider les vulnérabilités par exploitation contrôlée |
| Périmètre | Large : code, configuration, infrastructure, dépendances et application en exécution | Centré sur les chemins d’attaque réalistes |
| Méthodologie | Tests manuels et automatisés | Principalement manuel, soutenu par des outils |
| Exploitation | N’exploite généralement pas les failles | Utilise l’exploitation contrôlée si autorisée |
| Impact | Généralement non intrusif | Peut être intrusif et affecter la disponibilité |
| Rapport | Présente les problèmes et corrections | Documente l’exploitation, l’impact et les remédiations |
| But principal | Améliorer la posture de sécurité globale | Valider les défenses et les capacités de réponse |

---

# Menaces et risques

## Menace vs risque

Une **menace** est une source potentielle de dommage pouvant exploiter une vulnérabilité.

Exemples :

- Cybercriminels.
- Insiders malveillants.
- Campagnes de phishing.
- Malware.
- Attaques par déni de service.
- Catastrophes naturelles.
- Pannes de courant.

Un **risque** est la perte ou le dommage potentiel si une menace exploite avec succès une vulnérabilité.

```text
Risque = Probabilité × Impact
```

- **Probabilité :** La chance que l’événement se produise.
- **Impact :** La gravité des conséquences.

Une menace peut exister sans représenter un risque élevé si les contrôles de sécurité réduisent suffisamment sa probabilité ou son impact.

---

## Menaces web courantes

### Cross-Site Scripting

Le **XSS** survient lorsqu’un attaquant injecte du JavaScript malveillant dans une page consultée par d’autres utilisateurs.

Impacts possibles :

- Vol de cookies de session.
- Compromission de comptes.
- Manipulation du navigateur.
- Vol d’identifiants.
- Modification de contenu.
- Redirections malveillantes.

Types principaux :

- XSS réfléchi.
- XSS stocké.
- XSS basé sur le DOM.

---

### SQL Injection

La **SQL injection** survient lorsqu’un attaquant manipule l’entrée pour injecter des requêtes SQL malveillantes dans une base de données.

Impacts possibles :

- Accès non autorisé aux données.
- Fuite d’informations.
- Modification de données.
- Contournement de l’authentification.
- Compromission de la base.
- Suppression d’enregistrements.

Préventions principales :

- Requêtes paramétrées.
- Prepared statements.
- Validation d’entrée.
- Comptes de base de données avec minimum de privilèges.
- Gestion sécurisée des erreurs.

---

### Cross-Site Request Forgery

Le **CSRF** trompe un utilisateur authentifié pour qu’il effectue une action non désirée avec sa session active.

Impacts possibles :

- Modification des données de compte.
- Changement d’adresse e-mail.
- Transactions non désirées.
- Changement de mot de passe.
- Actions privilégiées.

Protections courantes :

- Tokens anti-CSRF.
- Cookies SameSite.
- Réauthentification pour les actions sensibles.
- Validation de `Origin` et `Referer`.

---

### Security Misconfiguration

Une mauvaise configuration peut affecter les serveurs, applications, bases de données, services cloud ou frameworks.

Exemples :

- Identifiants par défaut.
- Mode debug activé.
- Directory listing activé.
- Panneaux d’administration exposés.
- Services inutiles.
- Messages d’erreur détaillés.
- Headers de sécurité absents.
- Permissions excessives.

---

### Exposition de données sensibles

L’exposition de données sensibles survient lorsque les informations confidentielles ne sont pas suffisamment protégées.

Exemples :

- Mots de passe stockés en clair.
- Chiffrement faible.
- Trafic HTTP non chiffré.
- Données sensibles dans les logs.
- Backups exposés.
- APIs renvoyant trop de données.
- Identifiants déposés dans des dépôts de code.

---

### Brute force et credential stuffing

Une attaque par force brute teste un grand nombre de mots de passe contre un compte.

Le credential stuffing utilise des combinaisons utilisateur-mot de passe déjà divulguées contre d’autres services, en profitant de la réutilisation des mots de passe.

Défenses :

- MFA.
- Rate limiting.
- Verrouillage de compte.
- CAPTCHA si approprié.
- Surveillance des échecs de connexion.
- Gestionnaires de mots de passe.
- Détection des connexions inhabituelles.

---

### Vulnérabilités d’upload de fichiers

Un upload de fichiers non sécurisé peut permettre d’envoyer des fichiers dangereux.

Impacts possibles :

- Exécution de code à distance.
- Distribution de malware.
- Compromission du serveur.
- XSS stocké.
- Path traversal.
- Déni de service.

Pratiques sûres :

- Valider le type et le contenu.
- Renommer les fichiers uploadés.
- Stocker les fichiers hors du web root.
- Empêcher les permissions d’exécution.
- Imposer des limites de taille.
- Scanner les fichiers.
- Utiliser des allowlists plutôt que des blocklists.

---

### DoS et DDoS

Une attaque **DoS** cherche à rendre une application indisponible en épuisant ses ressources.

Une attaque **DDoS** utilise de nombreux systèmes pour saturer la cible.

Défenses :

- Rate limiting.
- CDN.
- Équilibrage de charge.
- Services de protection DDoS.
- Supervision.
- Planification de capacité.

---

### Server-Side Request Forgery

Le **SSRF** permet à un attaquant de faire envoyer des requêtes par le serveur vers des ressources internes ou externes.

Impacts possibles :

- Accès à des services internes.
- Exposition de métadonnées cloud.
- Scan du réseau interne.
- Vol de données.
- Contournement de restrictions réseau.

Défenses :

- Allowlists strictes pour les requêtes sortantes.
- Blocage des plages IP privées si elles ne sont pas nécessaires.
- Validation des URLs et protocoles.
- Segmentation réseau.
- Restriction des permissions réseau du serveur web.

---

### Broken Access Control

Le **broken access control** se produit lorsque des utilisateurs accèdent à des fonctions ou données qui dépassent leurs permissions.

Exemples :

- Accéder au profil d’un autre utilisateur en modifiant un ID dans une URL.
- Voir des pages administratives comme utilisateur standard.
- Télécharger les documents d’un autre compte.
- Accéder à des APIs sans contrôle d’autorisation.
- Modifier des objets appartenant à d’autres utilisateurs.

C’est l’une des zones les plus importantes à tester pendant un pentest web.

---

### Composants vulnérables

L’utilisation de composants présentant des vulnérabilités connues introduit des risques.

Composants concernés :

- Frameworks.
- CMS.
- Plugins.
- Bibliothèques JavaScript.
- Serveurs.
- Dépendances API.
- Images de conteneurs.

La mitigation inclut un inventaire des dépendances, la surveillance des avis de sécurité et l’application rapide des correctifs.

---

# Méthodologie de pentesting web

## 1. Définir le périmètre et les règles

Avant de tester, confirme :

- Les domaines et IPs cibles.
- Les comptes de test autorisés.
- Les dates et horaires.
- Les techniques autorisées.
- Les systèmes hors périmètre.
- Les fonctions sensibles à ne pas perturber.
- Les exigences de gestion des données.
- Les contacts d’urgence.
- Les exigences de reporting.

---

## 2. Collecte d’informations

Collecte des informations sur l’application avant les tests.

Identifie :

- Les domaines et sous-domaines.
- Les technologies web.
- Les frameworks.
- Les headers serveur.
- Les répertoires et endpoints.
- Les pages de connexion.
- Les APIs.
- Les fichiers JavaScript.
- Les paramètres.
- Les formulaires.
- Les cookies.
- Les rôles utilisateurs.
- Les intégrations tierces.

---

## 3. Cartographie de la surface d’attaque

Cartographie toutes les fonctionnalités accessibles.

Concentre-toi sur :

- Les pages d’authentification.
- L’inscription.
- La réinitialisation de mot de passe.
- Les paramètres de compte.
- Les uploads de fichiers.
- Les formulaires de recherche.
- Les paniers.
- Les fonctions d’administration.
- Les endpoints API.
- Les flux de paiement.
- Le contenu généré par les utilisateurs.
- Les méthodes HTTP.
- Les paramètres cachés.

---

## 4. Tests de vulnérabilités

Teste la surface d’attaque identifiée pour trouver des faiblesses communes :

- Failles d’authentification.
- Failles d’autorisation.
- Problèmes de session.
- Validation des entrées insuffisante.
- Upload de fichiers non sécurisé.
- Faiblesses API.
- Mauvaises configurations.
- Failles de logique métier.
- Exposition de données sensibles.
- Composants vulnérables.

---

## 5. Exploitation contrôlée

Lorsque les règles d’engagement l’autorisent, valide les vulnérabilités importantes par une exploitation contrôlée.

Le but est de démontrer l’impact tout en minimisant les risques.

Exemples :

- Confirmer l’accès aux données d’un autre utilisateur de test.
- Démontrer un contournement limité d’autorisation.
- Prouver qu’une donnée sensible est exposée.
- Vérifier une version vulnérable d’un composant.

Évite les actions destructrices sans autorisation spécifique.

---

## 6. Rapport

Un bon rapport de pentesting web doit inclure :

- Un résumé exécutif.
- Le périmètre et la méthodologie.
- Les vulnérabilités identifiées.
- La sévérité et l’impact métier.
- Les preuves.
- Les URLs, endpoints ou composants affectés.
- Les étapes de reproduction.
- Les recommandations de remédiation.
- Le niveau de risque.
- Les résultats du retest, si disponibles.

---

## 7. Remédiation et retest

Après correction des vulnérabilités, effectue de nouveaux tests afin de vérifier que :

- Le problème est résolu.
- La correction n’introduit pas de nouvelles vulnérabilités.
- Les contrôles de sécurité fonctionnent correctement.
- La preuve de concept initiale ne fonctionne plus.

---

# Points clés

- Les applications web sont des applications client-serveur accessibles par navigateur.
- La sécurité web protège la confidentialité, l’intégrité et la disponibilité.
- HTTP est sans état, donc les sessions doivent être gérées de manière sécurisée.
- Les tests de sécurité identifient les faiblesses, tandis que le pentesting valide les vulnérabilités par exploitation contrôlée.
- Les menaces courantes comprennent XSS, SQLi, CSRF, SSRF, broken access control, les uploads non sécurisés, les mauvaises configurations et DDoS.
- Le développement sécurisé, l’authentification robuste, la validation des entrées, les sessions sécurisées, le patching, le moindre privilège et les tests continus sont essentiels.
- La sécurité des applications web est un processus continu, pas une activité ponctuelle.
