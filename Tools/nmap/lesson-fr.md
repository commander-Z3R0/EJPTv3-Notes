# Cheatsheet Nmap

## Aperçu

Nmap est un outil de découverte réseau et d'audit de sécurité utilisé pour identifier :

- Les hôtes actifs.
- Les ports ouverts, fermés ou filtrés.
- Les services en cours d'exécution.
- Les versions des services.
- Les informations sur le système d'exploitation.
- Le comportement des pare-feu et du filtrage.
- Les informations liées à la sécurité via les scripts NSE.

Scannez uniquement les systèmes que vous possédez ou qui sont explicitement inclus dans une évaluation autorisée.

```text
Remplacez <target-IP> par l'adresse autorisée de la cible.
```

---

# 1. Commandes Nmap de base

## Scan de base

Scanne les ports TCP les plus courants sur une cible :

```bash
nmap <target-IP>
```

## Sortie verbeuse

Affiche des informations supplémentaires pendant le scan :

```bash
nmap -v <target-IP>
```

Utilisez une double verbosité pour une sortie plus détaillée :

```bash
nmap -vv <target-IP>
```

## Désactiver la résolution DNS

Évite les recherches DNS inverses pour rendre les scans plus rapides et réduire le trafic inutile :

```bash
nmap -n <target-IP>
```

## Ignorer la découverte d'hôte

Traite la cible comme étant en ligne et la scanne directement :

```bash
nmap -Pn <target-IP>
```

Ceci est utile lorsque ICMP ou les sondes de découverte normales sont bloquées. Cela peut également rendre le scan plus lent car Nmap scanne la cible même si elle est hors ligne.

## Lister les cibles sans scanner

Affiche les cibles que Nmap scannerait sans envoyer de sondes de scan :

```bash
nmap -sL <target-IP>
```

## Découverte d'hôte uniquement

Découvre si la cible est en ligne sans effectuer de scan de port :

```bash
nmap -sn <target-IP>
```

Ceci est couramment utilisé lors de la phase de reconnaissance initiale.

## Afficher les raisons du scan

Affiche la raison pour laquelle Nmap a attribué un état particulier à un port ou un hôte :

```bash
nmap --reason <target-IP>
```

## Afficher uniquement les ports ouverts

Masque les ports qui ne sont pas signalés comme ouverts :

```bash
nmap --open <target-IP>
```

---

# 2. Découverte d'hôte et scan de ports

## Techniques de découverte d'hôte

### Requête d'écho ICMP

Utilise la découverte par écho ICMP :

```bash
sudo nmap -PE <target-IP>
```

### Requête d'horodatage ICMP

Utilise la découverte par horodatage ICMP :

```bash
sudo nmap -PP <target-IP>
```

### Ping TCP SYN

Envoie des sondes TCP SYN vers des ports spécifiques :

```bash
sudo nmap -PS80,443 <target-IP>
```

### Ping TCP ACK

Envoie des sondes TCP ACK vers des ports spécifiques :

```bash
sudo nmap -PA80,443 <target-IP>
```

### Ping UDP

Envoie des sondes UDP vers des ports sélectionnés :

```bash
sudo nmap -PU53,161 <target-IP>
```

### Désactiver le ping ARP

Empêche Nmap d'utiliser la découverte ARP sur les réseaux locaux :

```bash
sudo nmap --disable-arp-ping <target-IP>
```

---

## Scan TCP SYN

Le scan TCP SYN est l'un des types de scan TCP les plus couramment utilisés.

```bash
sudo nmap -sS <target-IP>
```

Caractéristiques :

- Envoie des paquets SYN.
- Nécessite généralement des privilèges élevés.
- Ne complète pas la connexion TCP complète dans le cas normal.
- Fournit un moyen rapide d'identifier les états des ports TCP.

Utilisez-le uniquement contre des cibles autorisées.

## Scan TCP Connect

Utilise la méthode de connexion TCP complète du système d'exploitation :

```bash
nmap -sT <target-IP>
```

Ceci est utile lorsque les privilèges de paquets raw ne sont pas disponibles.

Caractéristiques :

- Complète la connexion TCP.
- Crée généralement plus de connexions visibles dans les journaux d'application.
- Ne nécessite normalement pas de privilèges root.

## Scan UDP

Scanne les ports UDP :

```bash
sudo nmap -sU <target-IP>
```

Les scans UDP sont généralement plus lents que les scans TCP car les services UDP peuvent ne pas répondre de manière cohérente.

## Scan combiné TCP et UDP

Scanne les ports TCP et UDP sélectionnés ensemble :

```bash
sudo nmap -sS -sU -p T:22,80,443,U:53,161 <target-IP>
```

La syntaxe sépare les protocoles :

```text
T: = ports TCP
U: = ports UDP
```

## Scan TCP ACK

Utilise un scan TCP ACK pour aider à analyser les règles du pare-feu :

```bash
sudo nmap -sA <target-IP>
```

Ce scan est principalement utile pour déterminer si les ports sont filtrés par un pare-feu. Il n'est pas conçu pour identifier directement les ports ouverts.

## Scan TCP FIN

Envoie des paquets FIN vers les ports TCP :

```bash
sudo nmap -sF <target-IP>
```

## Scan TCP NULL

Envoie des paquets TCP sans les indicateurs TCP standard :

```bash
sudo nmap -sN <target-IP>
```

## Scan TCP Xmas

Envoie des paquets avec les indicateurs FIN, PSH et URG :

```bash
sudo nmap -sX <target-IP>
```

Ces types de scan peuvent se comporter différemment selon le système d'exploitation et le pare-feu de la cible. Les résultats doivent toujours être interprétés avec prudence.

---

## Sélection de ports

### Scanner un seul port

```bash
nmap -p 22 <target-IP>
```

### Scanner plusieurs ports

```bash
nmap -p 22,80,443 <target-IP>
```

### Scanner une plage de ports

```bash
nmap -p 1-1000 <target-IP>
```

### Scanner tous les ports TCP

```bash
sudo nmap -p- <target-IP>
```

`-p-` signifie les ports 1 à 65535.

### Scanner les ports les plus courants

```bash
nmap --top-ports 100 <target-IP>
```

Scanne les 100 ports les plus courants.

```bash
nmap --top-ports 1000 <target-IP>
```

Scanne les 1 000 ports les plus courants.

### Scanner des ports par protocole

```bash
sudo nmap -p T:80,443,U:53 <target-IP>
```

### Scanner la liste de ports par défaut

```bash
nmap -p- <target-IP>
```

Ceci vérifie tous les ports TCP. Pour UDP, utilisez `-sU` explicitement.

---

# 3. Détection de services, OS et NSE

## Détection de services et de versions

Identifie l'application et la version s'exécutant sur les ports ouverts :

```bash
nmap -sV <target-IP>
```

Nmap envoie des sondes de service aux ports ouverts et compare les réponses avec sa base de données de détection de services.

## Détection de services sur des ports sélectionnés

```bash
nmap -sV -p 22,80,443 <target-IP>
```

## Intensité de détection de version

Utilise une intensité de détection de version plus faible :

```bash
nmap -sV --version-intensity 0 <target-IP>
```

Utilise une intensité plus élevée :

```bash
nmap -sV --version-intensity 9 <target-IP>
```

L'intensité varie de 0 à 9 :

- Les valeurs plus faibles sont plus rapides et moins complètes.
- Les valeurs plus élevées effectuent plus de sondes.
- Les valeurs plus élevées peuvent générer plus de trafic et prendre plus de temps.

## Détection du système d'exploitation

Tente d'identifier le système d'exploitation de la cible :

```bash
sudo nmap -O <target-IP>
```

## Détection OS avec détection de version

```bash
sudo nmap -O -sV <target-IP>
```

## Scan agressif

Active plusieurs fonctionnalités de détection avancées :

```bash
sudo nmap -A <target-IP>
```

`-A` active plusieurs fonctionnalités, notamment :

- Détection OS.
- Détection de services et de versions.
- Scripts NSE par défaut.
- Traceroute.

Comme il combine plusieurs techniques de détection, il peut générer plus de trafic qu'un scan de base.

## Détection OS avec supposition plus agressive

```bash
sudo nmap -O --osscan-guess <target-IP>
```

Ceci demande à Nmap de faire une supposition de système d'exploitation au mieux effort lorsque les résultats ne sont pas concluants.

---

## Nmap Scripting Engine (NSE)

Le Nmap Scripting Engine (NSE) permet aux scripts d'effectuer une découverte supplémentaire et des vérifications de sécurité.

Les scripts NSE peuvent aider avec :

- L'énumération de services.
- La collecte de bannières.
- La collecte d'informations HTTP.
- Les vérifications de configuration TLS.
- La découverte SMB.
- Les vérifications liées à l'authentification.
- La détection de vulnérabilités.
- L'énumération de ressources exposées.

NSE ne doit être utilisé que dans le cadre autorisé.

## Exécuter les scripts par défaut

```bash
nmap -sC <target-IP>
```

## Combiner les scripts par défaut avec la détection de version

```bash
nmap -sC -sV <target-IP>
```

Ceci est une commande d'énumération courante pour les services web ou réseau identifiés.

## Exécuter un script spécifique

```bash
nmap --script http-title -p 80,443 <target-IP>
```

## Vérifier les informations de chiffrement TLS

```bash
nmap --script ssl-enum-ciphers -p 443 <target-IP>
```

## Énumérer les informations OS SMB

```bash
nmap --script smb-os-discovery -p 445 <target-IP>
```

## Exécuter des scripts sûrs

```bash
nmap --script safe <target-IP>
```

La catégorie `safe` contient des scripts destinés à être moins intrusifs, mais ils doivent toujours être examinés et utilisés uniquement contre des cibles autorisées.

## Exécuter les scripts par défaut et sûrs

```bash
nmap --script "default,safe" <target-IP>
```

## Exécuter des scripts de vulnérabilité

```bash
nmap --script vuln <target-IP>
```

Les scripts de vulnérabilité peuvent produire des faux positifs et peuvent générer un trafic important. Ne traitez jamais leur sortie comme une vulnérabilité confirmée sans validation manuelle.

## Exécuter des scripts sur des ports sélectionnés

```bash
nmap --script http-title,http-headers -p 80,443 <target-IP>
```

## Afficher des informations sur un script

```bash
nmap --script-help http-title
```

## Afficher des informations sur une catégorie de scripts

```bash
nmap --script-help safe
```

## Lister les scripts NSE installés

```bash
ls /usr/share/nmap/scripts/
```

L'emplacement exact peut varier selon le système d'exploitation et la méthode d'installation.

---

# 4. Timing, performance et sortie

## Modèles de timing

Nmap fournit des modèles de timing de `-T0` à `-T5`.

```bash
nmap -T0 <target-IP>
```

Très lent et conçu pour réduire la vitesse de scan.

```bash
nmap -T1 <target-IP>
```

Profil de timing lent.

```bash
nmap -T2 <target-IP>
```

Profil de timing poli (polite).

```bash
nmap -T3 <target-IP>
```

Profil de timing par défaut.

```bash
nmap -T4 <target-IP>
```

Profil de timing plus rapide, couramment utilisé dans les laboratoires contrôlés ou les évaluations internes autorisées.

```bash
nmap -T5 <target-IP>
```

Timing très agressif. Cela peut augmenter la perte de paquets, les résultats inexacts et l'impact sur le service.

## Limiter la durée du scan

Définit un temps maximum pour scanner un hôte :

```bash
nmap --host-timeout 10m <target-IP>
```

## Limiter les nouvelles tentatives

Réduit le nombre de retransmissions :

```bash
nmap --max-retries 2 <target-IP>
```

Des valeurs de nouvelle tentative plus faibles peuvent rendre les scans plus rapides mais peuvent réduire la précision sur les réseaux peu fiables.

## Définir un taux de paquets maximum

```bash
sudo nmap --max-rate 100 <target-IP>
```

Ceci limite le nombre maximum de paquets envoyés par seconde.

## Définir un taux de paquets minimum

```bash
sudo nmap --min-rate 50 <target-IP>
```

Utilisez les contrôles de taux avec prudence. Un trafic excessif peut affecter les services et déclencher des systèmes de surveillance.

## Ajouter un délai entre les sondes

```bash
sudo nmap --scan-delay 100ms <target-IP>
```

Ceci peut réduire la vitesse de scan et la concentration du trafic.

## Afficher des statistiques périodiques

```bash
nmap --stats-every 10s <target-IP>
```

Ceci affiche la progression du scan à intervalles réguliers.

---

## Enregistrer la sortie en texte normal

```bash
nmap -oN nmap-result.txt <target-IP>
```

## Enregistrer la sortie en XML

```bash
nmap -oX nmap-result.xml <target-IP>
```

XML est utile pour importer les résultats dans d'autres outils.

## Enregistrer la sortie au format grepable

```bash
nmap -oG nmap-result.gnmap <target-IP>
```

Ce format est utile pour le traitement de texte simple.

## Enregistrer la sortie dans tous les formats principaux

```bash
nmap -oA nmap-result <target-IP>
```

Ceci crée des fichiers tels que :

```text
nmap-result.nmap
nmap-result.xml
nmap-result.gnmap
```

## Ajouter la sortie à un fichier existant

```bash
nmap --append-output -oN nmap-result.txt <target-IP>
```

## Reprendre un scan interrompu

```bash
nmap --resume nmap-result.nmap
```

Ceci nécessite un fichier de sortie au format normal du scan interrompu.

## Augmenter le détail de la sortie

```bash
nmap -v <target-IP>
```

## Activer le débogage

```bash
nmap -d <target-IP>
```

Utilisez des niveaux de débogage plus élevés uniquement lors du dépannage du comportement du scan :

```bash
nmap -dd <target-IP>
```

---

# 5. Flux de travail pratiques et interprétation

## Flux de travail de reconnaissance de base

### Étape 1 : Vérifier si l'hôte est en ligne

```bash
nmap -sn -n <target-IP>
```

### Étape 2 : Scanner les ports TCP courants

```bash
nmap -n --top-ports 1000 <target-IP>
```

### Étape 3 : Scanner tous les ports TCP

```bash
sudo nmap -n -sS -p- <target-IP>
```

### Étape 4 : Détecter les services et les versions

Utilisez les ports trouvés dans le scan précédent :

```bash
nmap -n -sV -p 22,80,443 <target-IP>
```

### Étape 5 : Exécuter les scripts par défaut

```bash
nmap -n -sC -sV -p 22,80,443 <target-IP>
```

### Étape 6 : Tenter la détection OS

```bash
sudo nmap -n -O -p 22,80,443 <target-IP>
```

### Étape 7 : Enregistrer les résultats finaux

```bash
sudo nmap -n -sC -sV -O -p 22,80,443 -oA nmap-final <target-IP>
```

---

## Flux de travail d'énumération de service web

Scanne les ports web courants et détecte les versions :

```bash
nmap -sV -p 80,443,8080,8443 <target-IP>
```

Collecte le titre HTTP :

```bash
nmap --script http-title -p 80,443,8080,8443 <target-IP>
```

Collecte les en-têtes HTTP :

```bash
nmap --script http-headers -p 80,443,8080,8443 <target-IP>
```

Exécute des scripts de découverte HTTP sélectionnés :

```bash
nmap --script http-title,http-headers -p 80,443 <target-IP>
```

Validez toujours manuellement les résultats intéressants avec un navigateur, un proxy ou un client HTTP autorisé.

---

## Flux de travail d'énumération de service TLS

Identifie les services sur les ports TLS courants :

```bash
nmap -sV -p 443,465,636,8443 <target-IP>
```

Énumère les chiffrements TLS pris en charge :

```bash
nmap --script ssl-enum-ciphers -p 443 <target-IP>
```

Obtient les informations de certificat :

```bash
nmap --script ssl-cert -p 443 <target-IP>
```

Exécute les deux scripts :

```bash
nmap --script ssl-cert,ssl-enum-ciphers -p 443 <target-IP>
```

---

## Flux de travail d'énumération UDP

Scanne les ports UDP courants :

```bash
sudo nmap -sU --top-ports 100 <target-IP>
```

Scanne les ports UDP sélectionnés :

```bash
sudo nmap -sU -p 53,67,68,123,161 <target-IP>
```

Combine les ports TCP et UDP sélectionnés :

```bash
sudo nmap -sS -sU -p T:22,80,443,U:53,123,161 <target-IP>
```

Les résultats UDP peuvent nécessiter une validation supplémentaire car de nombreux services UDP ne répondent pas de manière cohérente.

---

## Commande complète d'évaluation autorisée

Un scan détaillé qui enregistre les résultats dans plusieurs formats :

```bash
sudo nmap -n -sS -sV -sC -O -p- -T3 -oA nmap-assessment <target-IP>
```

Cette commande effectue :

- Aucune résolution DNS.
- Scan TCP SYN.
- Détection de services et de versions.
- Scripts NSE par défaut.
- Détection OS.
- Scan de tous les ports TCP.
- Timing modéré.
- Sortie enregistrée dans les formats normal, XML et grepable.

Utilisez ceci uniquement lorsque la portée autorise le trafic associé et les techniques de détection.

---

## États de port courants

| État | Signification |
|---|---|
| `open` | Une application accepte activement les connexions sur le port |
| `closed` | Le port est accessible, mais aucune application n'écoute |
| `filtered` | Un pare-feu ou un dispositif de filtrage empêche Nmap de déterminer l'état |
| `open\|filtered` | Nmap ne peut pas déterminer si le port est ouvert ou filtré |
| `closed\|filtered` | Nmap ne peut pas déterminer si le port est fermé ou filtré |

L'état du port n'est pas automatiquement une vulnérabilité. C'est une observation qui nécessite une analyse supplémentaire.

---

## Options Nmap courantes

| Option | Description |
|---|---|
| `-sS` | Scan TCP SYN |
| `-sT` | Scan TCP connect |
| `-sU` | Scan UDP |
| `-sA` | Scan TCP ACK |
| `-sV` | Détection de services et de versions |
| `-O` | Détection du système d'exploitation |
| `-A` | Fonctionnalités de scan agressif |
| `-sC` | Exécuter les scripts NSE par défaut |
| `--script` | Exécuter des scripts ou catégories NSE sélectionnés |
| `-p` | Spécifier les ports |
| `-p-` | Scanner tous les ports TCP |
| `--top-ports` | Scanner les ports les plus courants |
| `-Pn` | Ignorer la découverte d'hôte |
| `-sn` | Découverte d'hôte sans scan de port |
| `-n` | Désactiver la résolution DNS |
| `-T0` à `-T5` | Modèles de timing |
| `-v` | Sortie verbeuse |
| `--reason` | Afficher les raisons des états de port |
| `--open` | Afficher uniquement les ports ouverts ou potentiellement ouverts |
| `-oN` | Enregistrer la sortie normale |
| `-oX` | Enregistrer la sortie XML |
| `-oG` | Enregistrer la sortie grepable |
| `-oA` | Enregistrer la sortie dans plusieurs formats |
| `--resume` | Reprendre un scan interrompu |
| `--host-timeout` | Limiter le temps de scan pour un hôte |
| `--max-retries` | Limiter les retransmissions |
| `--stats-every` | Afficher les informations de progression périodiques |

---

## Flux de travail recommandé

```text
1. Confirmer l'autorisation et la portée.
2. Effectuer la découverte d'hôte.
3. Scanner les ports courants.
4. Scanner tous les ports requis.
5. Détecter les services et les versions.
6. Exécuter des scripts NSE soigneusement sélectionnés.
7. Enregistrer la sortie.
8. Valider manuellement les résultats intéressants.
9. Éliminer les faux positifs.
10. Documenter uniquement les résultats confirmés.
```

## Rappels importants

- Un port ouvert n'est pas automatiquement une vulnérabilité.
- Une bannière de service peut être inexacte ou intentionnellement modifiée.
- La détection de version ne prouve pas qu'une vulnérabilité existe.
- Les résultats NSE peuvent contenir des faux positifs.
- Un timing agressif peut réduire la précision du scan.
- Le scan UDP est généralement plus lent et plus difficile à interpréter.
- Les scans volumineux peuvent générer un trafic important.
- Les scans peuvent déclencher des alertes de pare-feu, IDS, IPS ou SIEM.
- Conservez la sortie de scan originale comme preuve.
- Validez les résultats importants avec des outils supplémentaires et des tests manuels.
- Ne scannez jamais de systèmes en dehors de la portée autorisée.
