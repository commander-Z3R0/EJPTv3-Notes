# John the Ripper Cheat Sheet

## Overview

John the Ripper (John) est un cracker de mots de passe rapide utilisé pour :

- Cracker des hachages de mots de passe.
- Tester la force des mots de passe.
- Effectuer des attaques par dictionnaire.
- Exécuter des attaques par brute-force.
- Réaliser des évaluations de sécurité autorisées.

John fournit :

- Le support de nombreux types de hachages.
- De multiples modes d'attaque.
- Des attaques avec wordlist et basées sur des règles.
- Un mode incrémental (brute-force).
- La détection automatique du type de hachage.

```text
Utilisez John the Ripper uniquement contre des hachages que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Starting John

## Syntaxe de base

```bash
john [options] <hash-file>
```

## Afficher l'aide

```bash
john --help
```

## Afficher la version

```bash
john --version
```

## Mettre à jour John

```bash
git clone https://github.com/openwall/john.git
cd john/src
make
```

## Formats disponibles

```bash
john --list=formats
```

---

# 2. Hash Preparation

## Identifier le type de hachage

```bash
hashid <hash>
```

Ou utiliser des outils en ligne pour identifier le type de hachage.

## Préparer le fichier de hachage

```bash
echo "hash" > hashes.txt
```

Ou :

```bash
cat hashes.txt
```

## Formats de hachages communs

```bash
# MD5
hash

# SHA1
hash

# SHA256
hash

# SHA512
hash

# NTLM
hash

# WPA/WPA2
hash

# bcrypt
hash

# MD5crypt
$1$salt$hash

# SHA512crypt
$6$salt$hash
```

---

# 3. Basic Cracking

## Attaque par dictionnaire de base

```bash
john <hash-file>
```

Exemple :

```bash
john hashes.txt
```

## Attaque par dictionnaire avec wordlist

```bash
john --wordlist=<wordlist> <hash-file>
```

Exemple :

```bash
john --wordlist=rockyou.txt hashes.txt
```

## Spécifier le format de hachage

```bash
john --format=<format> --wordlist=<wordlist> <hash-file>
```

Exemple :

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## Afficher les mots de passe crackés

```bash
john --show <hash-file>
```

## Afficher les hachages restants

```bash
john --show <hash-file> | grep -v "password hashes cracked"
```

## Supprimer les hachages crackés

```bash
john --pot=<pot-file> <hash-file>
```

---

# 4. Attack Modes

## Attaque par dictionnaire

```bash
john --wordlist=<wordlist> <hash-file>
```

Exemple :

```bash
john --wordlist=rockyou.txt hashes.txt
```

## Incrémental (brute-force)

```bash
john --incremental <hash-file>
```

## Incrémental avec charset spécifique

```bash
john --incremental=lowercase <hash-file>
```

## Incrémental avec charset personnalisé

```bash
john --incremental=custom <hash-file>
```

## Mode externe

```bash
john --external=<mode> <hash-file>
```

## Attaque hybride

```bash
john --wordlist=<wordlist> --rules <hash-file>
```

## Attaque basée sur des règles

```bash
john --wordlist=<wordlist> --rules <hash-file>
```

---

# 5. Wordlist Options

## Utiliser rockyou.txt

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt <hash-file>
```

## Utiliser plusieurs wordlists

```bash
john --wordlist=wordlist1.txt,wordlist2.txt <hash-file>
```

## Utiliser stdin

```bash
cat wordlist.txt | john --stdin <hash-file>
```

## Générer une wordlist avec crunch

```bash
crunch 8 8 -t password@@@ | john --stdin <hash-file>
```

## Utiliser une wordlist personnalisée

```bash
john --wordlist=custom.txt <hash-file>
```

## Wordlist avec règles

```bash
john --wordlist=wordlist.txt --rules <hash-file>
```

---

# 6. Rules

## Utiliser les règles par défaut

```bash
john --wordlist=<wordlist> --rules <hash-file>
```

Exemple :

```bash
john --wordlist=rockyou.txt --rules hashes.txt
```

## Utiliser des règles spécifiques

```bash
john --wordlist=<wordlist> --rules=<rules> <hash-file>
```

## Lister les règles disponibles

```bash
john --list=rules
```

## Créer des règles personnalisées

Éditez `john.conf` et ajoutez la section de règles :

```text
[List.Rules:MyRules]
:
$1
$2
$3
^1
^2
^3
```

## Appliquer des règles personnalisées

```bash
john --wordlist=wordlist.txt --rules=MyRules <hash-file>
```

## Exemples de règles

```text
:          - Ne rien faire
$1         - Ajouter 1 à la fin
$2         - Ajouter 2 à la fin
^1         - Ajouter 1 au début
c          - Capitaliser la première lettre
C          - Capitaliser toutes les lettres
l          - Mettre en minuscules toutes les lettres
u          - Mettre en majuscules toutes les lettres
i          - Inverser la casse
d          - Dupliquer le mot
p          - Permuter les caractères
```

---

# 7. Hash Formats

## Raw MD5

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## Raw SHA1

```bash
john --format=raw-sha1 --wordlist=rockyou.txt hashes.txt
```

## Raw SHA256

```bash
john --format=raw-sha256 --wordlist=rockyou.txt hashes.txt
```

## Raw SHA512

```bash
john --format=raw-sha512 --wordlist=rockyou.txt hashes.txt
```

## NTLM

```bash
john --format=NT --wordlist=rockyou.txt hashes.txt
```

## MD5crypt

```bash
john --format=md5crypt --wordlist=rockyou.txt hashes.txt
```

## SHA512crypt

```bash
john --format=sha512crypt --wordlist=rockyou.txt hashes.txt
```

## bcrypt

```bash
john --format=bcrypt --wordlist=rockyou.txt hashes.txt
```

## WPA/WPA2

```bash
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx
```

## Kerberos

```bash
john --format=krb5-17 --wordlist=rockyou.txt hashes.txt
```

---

# 8. Performance Options

## Utiliser tous les cores CPU

```bash
john --fork=<cores> <hash-file>
```

Exemple :

```bash
john --fork=4 hashes.txt
```

## Établir le nom de session

```bash
john --session=<name> <hash-file>
```

Exemple :

```bash
john --session=mycrack hashes.txt
```

## Restaurer la session

```bash
john --restore=<name>
```

## Statut de la session actuelle

```bash
john --status=<name>
```

## Limiter le temps d'exécution

```bash
john --max-run-time=<seconds> <hash-file>
```

## Limiter la longueur du mot de passe

```bash
john --min-length=<min> --max-length=<max> <hash-file>
```

## Mode single crack

```bash
john --single <hash-file>
```

---

# 9. Output Options

## Afficher les mots de passe crackés

```bash
john --show <hash-file>
```

## Afficher les crackés avec format

```bash
john --show --format=<format> <hash-file>
```

## Sauvegarder la sortie dans un fichier

```bash
john --show <hash-file> > cracked.txt
```

## Afficher uniquement les crackés

```bash
john --show <hash-file> | grep -v "password hashes cracked"
```

## Afficher les statistiques

```bash
john --show <hash-file> | tail
```

## Emplacement du potfile

```bash
~/.john/john.pot
```

## Voir le potfile

```bash
cat ~/.john/john.pot
```

---

# 10. Advanced Options

## Générer une wordlist depuis le potfile

```bash
john --show <hash-file> | cut -d: -f2 > passwords.txt
```

## Rendre le potfile lisible

```bash
john --show <hash-file>
```

## Continuer une session interrompue

```bash
john --restore
```

## Arrêter la session

Appuyez sur `Ctrl+C` pendant l'exécution.

## Sauvegarder l'état de session

John sauvegarde automatiquement l'état de session.

## Nettoyer le potfile

```bash
rm ~/.john/john.pot
```

## Utiliser un potfile spécifique

```bash
john --pot=<pot-file> <hash-file>
```

## Désactiver le potfile

```bash
john --pot=none <hash-file>
```

---

# 11. Common Attack Scenarios

## Attaque par dictionnaire de base

```bash
john --wordlist=rockyou.txt hashes.txt
```

## Crack NTLM

```bash
john --format=NT --wordlist=rockyou.txt hashes.txt
```

## Crack MD5

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## Crack SHA256

```bash
john --format=raw-sha256 --wordlist=rockyou.txt hashes.txt
```

## Crack WPA/WPA2

```bash
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx
```

## Brute-force minuscules

```bash
john --incremental=lowercase hashes.txt
```

## Attaque basée sur des règles

```bash
john --wordlist=rockyou.txt --rules hashes.txt
```

## Mode single crack

```bash
john --single hashes.txt
```

## Plusieurs wordlists

```bash
john --wordlist=wordlist1.txt,wordlist2.txt hashes.txt
```

## Format personnalisé

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

---

# 12. Practical Workflows

## Flux de travail de base de crack de mots de passe

```text
1. Identifier le type de hachage.
2. Préparer le fichier de hachage.
3. Sélectionner le mode d'attaque.
4. Choisir la wordlist/les règles.
5. Exécuter John.
6. Examiner les mots de passe crackés.
7. Documenter les résultats.
```

## Exemple : Crack NTLM

```bash
# Préparer les hachages
echo "hash1" > hashes.txt
echo "hash2" >> hashes.txt

# Cracker avec rockyou
john --format=NT --wordlist=rockyou.txt hashes.txt

# Afficher les résultats
john --show hashes.txt
```

## Exemple : Crack WPA/WPA2

```bash
# Convertir en hccapx
hcxpcapngtool -o wifi.hccapx capture.pcapng

# Cracker
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx

# Afficher les résultats
john --show wifi.hccapx
```

## Exemple : Brute-force

```bash
# Minuscules uniquement
john --incremental=lowercase hashes.txt

# Brute-force complet
john --incremental hashes.txt
```

## Exemple : Basé sur des règles

```bash
# Avec règles par défaut
john --wordlist=rockyou.txt --rules hashes.txt

# Avec règles personnalisées
john --wordlist=rockyou.txt --rules=MyRules hashes.txt
```

## Exemple : Formats multiples

```bash
# MD5
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt

# SHA256
john --format=raw-sha256 --wordlist=rockyou.txt hashes.txt
```

---

# 13. Common Commands Reference

| Command | Description |
|---|---|
| `john --help` | Afficher l'aide |
| `john --version` | Afficher la version |
| `john <hash-file>` | Crack de base |
| `john --wordlist=<file>` | Attaque par dictionnaire |
| `john --incremental` | Brute-force |
| `john --rules` | Attaque basée sur des règles |
| `john --format=<format>` | Spécifier le format |
| `john --show` | Afficher les crackés |
| `john --restore` | Restaurer la session |
| `john --status` | Afficher le statut |
| `john --session=<name>` | Nom de session |
| `john --fork=<cores>` | Utiliser plusieurs cores |
| `john --single` | Mode single crack |
| `john --stdin` | Lire depuis stdin |
| `john --pot=<file>` | Spécifier le potfile |
| `john --list=formats` | Lister les formats |
| `john --list=rules` | Lister les règles |
| `john --max-run-time=<sec>` | Limiter le temps d'exécution |
| `john --min-length=<min>` | Longueur minimale du mot de passe |
| `john --max-length=<max>` | Longueur maximale du mot de passe |

---

# 14. Troubleshooting

## Aucun hachage chargé

- Vérifiez le format du fichier de hachage.
- Assurez-vous que le type de hachage est correct.
- Vérifiez que les hachages ne sont pas déjà crackés.
- Vérifiez les permissions du fichier.

## Format de hachage inconnu

- Spécifiez le format correct avec `--format`.
- Vérifiez la syntaxe du hachage.
- Supprimez les hachages invalides.
- Utilisez `--list=formats` pour voir les formats disponibles.

## Performance lente

- Utilisez le mode d'attaque approprié.
- Sélectionnez des wordlists efficaces.
- Utilisez `--fork` pour plusieurs cores.
- Considérez utiliser Hashcat pour l'accélération GPU.

## Session interrompue

- Utilisez `--restore` pour continuer.
- Vérifiez que le fichier de session existe.
- Assurez-vous que le potfile est intact.
- Redémarrez avec les mêmes paramètres.

## Hachage invalide

- Vérifiez le type de hachage.
- Vérifiez le format du hachage.
- Supprimez les hachages invalides.
- Utilisez la spécification de format correcte.

---

# 15. Security Best Practices

## Vérifiez toujours les résultats

- Testez les mots de passe crackés manuellement.
- Vérifiez que le type de hachage est correct.
- Recoupez avec d'autres outils.
- Documentez tous les résultats.

## Respectez les limites légales

- Cracker uniquement les hachages que vous possédez.
- Obtenez une autorisation explicite.
- Suivez la divulgation responsable.
- Documentez toutes les activités.

## Optimisez la performance

- Utilisez des modes d'attaque appropriés.
- Sélectionnez des wordlists efficaces.
- Profitez des multiples cores.
- Surveillez les ressources système.

## Gérez les ressources

- Surveillez l'utilisation du CPU.
- Évitez la surchauffe.
- Utilisez une charge de travail appropriée.
- Prenez des pauses entre les sessions.

## Documentez tout

- Enregistrez toutes les commandes utilisées.
- Notez les types et sources de hachages.
- Suivez la progression du crack.
- Documentez les résultats et méthodes.

---

# 16. Important Reminders

- Obtenez toujours une autorisation explicite avant d'utiliser John.
- Testez d'abord dans un environnement de laboratoire contrôlé.
- Tous les hachages ne sont pas crackables en temps raisonnable.
- Certaines attaques peuvent prendre un temps significatif.
- Maintenez John à jour régulièrement.
- Validez les résultats manuellement.
- Documentez toutes les actions et commandes.
- Préservez les preuves originales et les logs.
- Comprenez les implications légales et éthiques.
- Respectez les politiques de mots de passe et de sécurité.

---

# 17. Quick Reference Examples

## Attaque par dictionnaire de base

```bash
john --wordlist=rockyou.txt hashes.txt
```

## Crack NTLM

```bash
john --format=NT --wordlist=rockyou.txt hashes.txt
```

## Crack MD5

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## Afficher les crackés

```bash
john --show hashes.txt
```

## Attaque basée sur des règles

```bash
john --wordlist=rockyou.txt --rules hashes.txt
```

## Brute-force

```bash
john --incremental hashes.txt
```

## Crack WPA/WPA2

```bash
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx
```

## Plusieurs cores

```bash
john --fork=4 --wordlist=rockyou.txt hashes.txt
```

## Restaurer la session

```bash
john --restore
```

## Format personnalisé

```bash
john --format=raw-sha256 --wordlist=rockyou.txt hashes.txt
```

## Mode single crack

```bash
john --single hashes.txt
```

---

# 18. Additional Resources

## John the Ripper Official

```text
http://www.openwall.com/john/
```

## John the Ripper GitHub

```text
https://github.com/openwall/john
```

## John the Ripper Wiki

```text
https://github.com/openwall/john/wiki
```

## Weakpass Wordlists

```text
https://weakpass.com/
```

## Hashcat (alternative GPU)

```text
https://hashcat.net/hashcat/
```
