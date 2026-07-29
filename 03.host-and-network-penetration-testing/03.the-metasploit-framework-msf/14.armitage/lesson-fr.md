# Armitage

## Armitage - GUI de MSF

Armitage est une interface graphique (GUI) pour le Metasploit Framework. Il rend Metasploit plus facile à utiliser en offrant une manière visuelle d’interagir avec les cibles, les services, les sessions et les exploits.

Armitage est particulièrement utile pour :

- Visualiser les hôtes et leurs relations réseau.
- Réaliser des scans et des tâches d’énumération.
- Lancer des exploits depuis une interface graphique.
- Gérer les sessions actives.
- Faciliter les tâches de post-exploitation comme le dump de hashes, la navigation dans les fichiers et le pivoting.

>  **Port Scanning & Enumeration With Armitage** — laboratoire de l’INE.

---

## Configuration D’Armitage

### Démarrer Les Services Requis

Avant d’ouvrir Armitage, Metasploit doit être connecté à PostgreSQL.

```bash
service postgresql start && msfconsole -q
db_status
```

Si la base de données est correctement connectée, Metasploit doit indiquer qu’il est connecté à PostgreSQL.

### Lancer Armitage

Ouvre Armitage depuis un nouveau terminal :

```bash
armitage
```

Lorsque le message apparaît, réponds **YES** pour démarrer le serveur RPC.

### Pourquoi C’est Important

Armitage dépend de la base de données Metasploit et des services RPC pour gérer correctement les hôtes, les services, le loot et les sessions.

---

## Scan Et Énumération Des Ports

### Ajouter Des Cibles

Une fois Armitage ouvert, ajoute manuellement l’hôte victime :

- Ouvre **Hosts**.
- Sélectionne **Add Hosts**.
- Ajoute l’IP de la victime.
- Donne un nom à l’hôte si nécessaire, par exemple `Victim 1`.

### Scanner L’Hôte

Fais un clic droit sur la cible et choisis **Scan**.  
Tu peux aussi lancer un scan Nmap depuis le menu **Hosts**.

Armitage affiche les services découverts et les ports ouverts sous forme visuelle.

### Flux Typique

```bash
# Ajouter l’hôte depuis l’interface graphique, puis le scanner
```

### Ce Que Tu Peux Voir

Après le scan, Armitage peut afficher :

- Les ports ouverts.
- Les services en cours d’exécution.
- Les versions détectées.
- Les relations entre services.

### Pourquoi C’est Important

Armitage rend l’énumération plus simple en présentant les résultats du scan visuellement, au lieu de devoir tout inspecter manuellement dans la console.

---

## Exploitation Avec Armitage

### Recherche D’Exploits

Armitage peut rechercher des modules liés à un service et les lancer directement depuis l’interface.

Par exemple, si une cible exécute Rejetto HFS, tu peux rechercher `rejetto` et lancer le module d’exploit correspondant.

### Exemple Typique

Un service HFS vulnérable peut être exploité depuis Armitage en sélectionnant l’hôte, en choisissant le service détecté, puis en lançant le module Metasploit correspondant.

### Pourquoi C’est Important

Ce flux réduit le besoin d’interactions manuelles avec Metasploit et accélère l’exploitation dans les labs et les évaluations.

---

## Post-Exploitation Avec Armitage

### Dump De Hashes

Armitage peut être utilisé pour lancer des actions de post-exploitation comme le dump de hashes Windows.

Un exemple consiste à utiliser la méthode du registre via le module `smart_hashdump` :

```bash
post/windows/gather/smart_hashdump
```

Les hashes enregistrés peuvent ensuite être retrouvés dans le menu **View > Loot**.

### Navigation Dans Les Fichiers

Après compromission, Armitage peut t’aider à parcourir les fichiers du système cible depuis le contexte de la session.

### Affichage Des Processus

Tu peux aussi inspecter les processus en cours pour identifier des cibles utiles pour la migration ou l’élévation de privilèges.

### Pourquoi C’est Important

Armitage rend les tâches courantes de post-exploitation plus accessibles et t’aide à passer rapidement de l’accès initial à une découverte plus profonde du système.

---

## Pivoting Avec Armitage

### Ce Que Fait Le Pivoting

Le pivoting te permet d’utiliser un hôte compromis pour atteindre d’autres systèmes du réseau interne qui ne sont pas directement accessibles depuis ta machine d’attaque.

### Flux Typique

Après avoir compromis `Victim 1`, tu peux configurer le pivoting et ajouter une route vers la sous-réseau interne.

```bash
run autoroute -s <target_network>/24
```

Ensuite, tu peux scanner `Victim 2` à travers l’hôte pivot.

### Port Forwarding

Si un service est détecté sur le second hôte, tu peux rediriger le port via la machine compromise :

```bash
portfwd add -l 1234 -p 80 -r <target_ip>
db_nmap -sV -p 1234 localhost
```

Cela te permet d’inspecter le service distant comme s’il fonctionnait localement sur `127.0.0.1:1234`.

### Pourquoi C’est Important

Le pivoting est l’une des fonctionnalités les plus puissantes d’Armitage, car il permet d’étendre l’accès au-delà du premier hôte compromis et d’explorer le réseau plus en profondeur.

---

## Exploiter Un Second Hôte

### Flux D’Exemple

Après avoir découvert des services sur `Victim 2`, tu peux rechercher des exploits compatibles et les lancer depuis Armitage ou Metasploit.

Si la cible est vulnérable, tu peux aussi avoir besoin de :

- Migrer vers un processus stable.
- Renommer la session pour plus de clarté.
- Utiliser le bon type de payload selon l’architecture.

### Actions De Suivi Typiques

```bash
sessions -n victim-2 -i 2
```

### Pourquoi C’est Important

Renommer les sessions et organiser la chaîne de compromission devient très utile quand tu commences à travailler avec plusieurs systèmes en même temps.

---

## Armitage Sous Kali Linux

### Installation

Si Armitage n’est pas déjà disponible, tu peux l’installer sur Kali et préparer la base de données :

```bash
sudo apt install armitage -y
sudo msfdb init
sudo nano /etc/postgresql/15/main/pg_hba.conf
sudo systemctl enable postgresql
sudo systemctl restart postgresql
sudo armitage
```

### Note Sur L’Authentification De La Base De Données

Si Armitage ne parvient pas à se connecter correctement, il peut être nécessaire d’ajuster les paramètres d’authentification PostgreSQL dans `pg_hba.conf` afin que le client puisse se connecter localement.

### Pourquoi C’est Important

Armitage est étroitement lié aux services backend de Metasploit, donc la base de données et la configuration RPC doivent être correctes avant que l’interface graphique fonctionne correctement.

---

## Pourquoi C’Est Important

Armitage offre une manière visuelle de travailler avec Metasploit, ce qui facilite la gestion du scan, de l’exploitation, des sessions, de la revue du loot et du pivoting. Il est particulièrement utile dans les labs où tu veux passer rapidement de la découverte à la post-exploitation.

## Points Clés

- **Armitage** est une GUI pour Metasploit qui simplifie le scan, l’exploitation et la gestion des sessions.
- Il nécessite que **PostgreSQL** et Metasploit soient exécutés correctement.
- Tu peux ajouter des hôtes, les scanner et lancer des exploits directement depuis l’interface.
- **Loot**, la navigation dans les fichiers et l’affichage des processus sont disponibles après compromission.
- Le **pivoting** avec `autoroute` et `portfwd` permet d’atteindre des systèmes internes via un hôte compromis.
- Sous Kali, Armitage peut nécessiter une configuration de la base de données et un ajustement de l’authentification PostgreSQL avant de fonctionner correctement.
