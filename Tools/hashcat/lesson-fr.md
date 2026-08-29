# Hashcat Cheat Sheet

## Overview

Hashcat est l'outil de récupération de mots de passe le plus rapide et le plus avancé au monde, utilisé pour :

- Cracker des hachages de mots de passe.
- Tester la force des mots de passe.
- Effectuer des attaques par dictionnaire.
- Exécuter des attaques par brute-force.
- Réaliser des évaluations de sécurité autorisées.

Hashcat fournit :

- Le support de plus de 300 types de hachages.
- De multiples modes d'attaque.
- L'accélération GPU.
- Des attaques basées sur des règles.
- Des attaques avec charset et masques personnalisés.

```text
Utilisez Hashcat uniquement contre des hachages que vous possédez ou pour lesquels vous avez une autorisation explicite de test.
```

---

# 1. Starting Hashcat

## Syntaxe de base

```bash
hashcat [options] -m <hash-type> -a <attack-mode> <hash-file> <wordlist>
```

## Afficher l'aide

```bash
hashcat -h
```

## Afficher la version

```bash
hashcat --version
```

## Afficher les types de hachages supportés

```bash
hashcat --help | grep -A 100 "Hash modes"
```

## Afficher les modes d'attaque supportés

```bash
hashcat --help | grep -A 10 "Attack modes"
```

## Mettre à jour Hashcat

```bash
git clone https://github.com/hashcat/hashcat.git
cd hashcat
make
sudo make install
```

---

# 2. Hash Types

## Types de hachages communs

```bash
# MD5
-m 0

# MD5 avec salt
-m 10

# SHA1
-m 100

# SHA256
-m 1400

# SHA512
-m 1700

# NTLM (Windows)
-m 1000

# WPA/WPA2
-m 2500

# bcrypt
-m 3200

# SHA512crypt
-m 1800

# MD5crypt
-m 500

# Kerberos TGS
-m 13100

# Bitcoin wallet
-m 11300
```

## Lister tous les types de hachages

```bash
hashcat --help
```

## Identifier le type de hachage

```bash
hashid <hash>
```

Ou utiliser des outils en ligne pour identifier le type de hachage.

---

# 3. Attack Modes

## Attaque par dictionnaire

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist>
```

Exemple :

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt
```

## Attaque combinatoire

```bash
hashcat -a 1 -m <hash-type> <hash-file> <wordlist1> <wordlist2>
```

Exemple :

```bash
hashcat -a 1 -m 0 hashes.txt wordlist1.txt wordlist2.txt
```

## Attaque par brute-force

```bash
hashcat -a 3 -m <hash-type> <hash-file> <mask>
```

Exemple :

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a
```

## Attaque hybride (wordlist + mask)

```bash
hashcat -a 6 -m <hash-type> <hash-file> <wordlist> <mask>
```

Exemple :

```bash
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d
```

## Attaque hybride (mask + wordlist)

```bash
hashcat -a 7 -m <hash-type> <hash-file> <mask> <wordlist>
```

Exemple :

```bash
hashcat -a 7 -m 0 hashes.txt ?d?d?d?d rockyou.txt
```

## Attaque basée sur des règles

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r <rules>
```

Exemple :

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

---

# 4. Dictionary Attacks

## Attaque par dictionnaire de base

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist>
```

Exemple :

```bash
hashcat -a 0 -m 1000 hashes.txt rockyou.txt
```

## Attaque par dictionnaire avec plusieurs wordlists

```bash
hashcat -a 0 -m <hash-type> <hash-file> wordlist1.txt wordlist2.txt
```

## Attaque par dictionnaire avec règles

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r <rules-file>
```

Exemple :

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Attaque par dictionnaire avec plusieurs règles

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r rules1.rule -r rules2.rule
```

## Attaque par dictionnaire avec règles personnalisées

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r custom.rule
```

## Attaque par dictionnaire avec stdout

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> --stdout
```

Affiche les mots de passe générés sans cracker.

---

# 5. Brute-Force Attacks

## Brute-force de base

```bash
hashcat -a 3 -m <hash-type> <hash-file> <mask>
```

Exemple :

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a
```

## Brute-force numérique

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?d?d?d?d?d?d
```

Exemple :

```bash
hashcat -a 3 -m 0 hashes.txt ?d?d?d?d
```

## Brute-force minuscules

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?l?l?l?l?l?l
```

Exemple :

```bash
hashcat -a 3 -m 0 hashes.txt ?l?l?l?l
```

## Brute-force majuscules

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?u?u?u?u?u?u
```

Exemple :

```bash
hashcat -a 3 -m 0 hashes.txt ?u?u?u?u
```

## Brute-force mixte

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?a?a?a?a?a?a
```

## Brute-force avec charset personnalisé

```bash
hashcat -a 3 -m <hash-type> <hash-file> -1 <charset> <custom-mask>
```

Exemple :

```bash
hashcat -a 3 -m 0 hashes.txt -1 abcdef ?1?1?1?1
```

## Masque complexe

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?u?l?l?l?d?d?d
```

Exemple :

```bash
hashcat -a 3 -m 0 hashes.txt ?u?l?l?l?d?d?d
```

---

# 6. Mask Attack

## Définitions de charset de masque

```text
?l = abcdefghijklmnopqrstuvwxyz (minuscules)
?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ (majuscules)
?d = 0123456789 (chiffres)
?h = 0123456789abcdef (hex minuscules)
?H = 0123456789ABCDEF (hex majuscules)
?s =  !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~ (caractères spéciaux)
?a = ?l?u?d?s (tous les imprimables)
?b = 0x00 - 0xff (tous les octets)
```

## Charset personnalisé

```bash
hashcat -a 3 -m <hash-type> <hash-file> -1 <charset> <mask>
```

Exemple :

```bash
hashcat -a 3 -m 0 hashes.txt -1 custom ?1?1?1?1
```

## Plusieurs charsets personnalisés

```bash
hashcat -a 3 -m <hash-type> <hash-file> -1 <charset1> -2 <charset2> <mask>
```

Exemple :

```bash
hashcat -a 3 -m 0 hashes.txt -1 abc -2 123 ?1?1?2?2
```

## Masque avec préfixe connu

```bash
hashcat -a 3 -m <hash-type> <hash-file> Password?d?d?d
```

## Masque avec suffixe connu

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?l?l?l?l123
```

## Exemple de masque complexe

```bash
hashcat -a 3 -m 0 hashes.txt ?u?l?l?l?l?l?d?d
```

---

# 7. Rule-Based Attacks

## Utiliser des règles intégrées

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r <rules-file>
```

Exemple :

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Fichiers de règles communs

```text
rules/best64.rule
rules/d3ad0ne.rule
rules/InsidePro-HashManager.rule
rules/OneRuleToRuleThemAll.rule
rules/T0XlC.rule
```

## Créer des règles personnalisées

Créez un fichier `custom.rule` :

```text
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
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r custom.rule
```

## Plusieurs fichiers de règles

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r rules1.rule -r rules2.rule
```

## Générer des règles avec stats

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r rules/best64.rule --stdout
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

# 8. Performance Options

## Spécifier le dispositif GPU

```bash
hashcat -d <device-id> -m <hash-type> <hash-file> <wordlist>
```

Exemple :

```bash
hashcat -d 1 -m 0 hashes.txt rockyou.txt
```

## Utiliser tous les GPUs

```bash
hashcat -d 1,2,3,4 -m <hash-type> <hash-file> <wordlist>
```

## Établir le profil de workload

```bash
hashcat -w <profile> -m <hash-type> <hash-file> <wordlist>
```

Profils :

- 1 - Low (optimisé pour l'arrière-plan)
- 2 - Default (équilibré)
- 3 - High (optimisé pour la vitesse)
- 4 - Insane (performance maximale)

## Limiter le temps de kernel

```bash
hashcat --kernel-timeout <seconds> -m <hash-type> <hash-file> <wordlist>
```

## Établir le nombre de threads

```bash
hashcat -t <threads> -m <hash-type> <hash-file> <wordlist>
```

## Utiliser uniquement le CPU

```bash
hashcat --force -m <hash-type> <hash-file> <wordlist>
```

---

# 9. Output Options

## Spécifier le fichier de sortie

```bash
hashcat -o <output-file> -m <hash-type> <hash-file> <wordlist>
```

Exemple :

```bash
hashcat -o cracked.txt -m 0 hashes.txt rockyou.txt
```

## Afficher les mots de passe crackés

```bash
hashcat --show -m <hash-type> <hash-file>
```

## Afficher les hachages restants

```bash
hashcat --left -m <hash-type> <hash-file>
```

## Format de sortie

```bash
hashcat -o <output-file> --outfile-format <format> -m <hash-type> <hash-file> <wordlist>
```

Formats :

- 1 - Hash:Plain
- 2 - Plain
- 3 - Hex-Plain
- 4 - Crack-Pos
- 5 - Timestamp:Plain

## Sortie verbose

```bash
hashcat -v -m <hash-type> <hash-file> <wordlist>
```

## Mode quiet

```bash
hashcat -q -m <hash-type> <hash-file> <wordlist>
```

## Mode debug

```bash
hashcat --debug-mode <mode> -m <hash-type> <hash-file> <wordlist>
```

---

# 10. Session Management

## Spécifier le nom de session

```bash
hashcat --session <name> -m <hash-type> <hash-file> <wordlist>
```

Exemple :

```bash
hashcat --session mycrack -m 0 hashes.txt rockyou.txt
```

## Lister les sessions

```bash
hashcat --list
```

## Restaurer la session

```bash
hashcat --restore --session <name>
```

## Supprimer la session

```bash
hashcat --remove --session <name>
```

## Sauvegarder la progression

```bash
hashcat --checkpoint-disable -m <hash-type> <hash-file> <wordlist>
```

## Pause de session

Appuyez sur `p` pendant l'exécution pour mettre en pause.

## Reprendre la session

Appuyez sur `s` pendant l'exécution pour reprendre.

## Quitter la session

Appuyez sur `q` pendant l'exécution pour quitter.

---

# 11. Advanced Options

## Gestion du potfile

```bash
# Afficher le potfile
hashcat --show -m <hash-type> <hash-file>

# Désactiver le potfile
hashcat --potfile-disable -m <hash-type> <hash-file> <wordlist>

# Potfile personnalisé
hashcat --potfile-path <path> -m <hash-type> <hash-file> <wordlist>
```

## Attaque loopback

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> --loopback
```

## Désactiver le self-test

```bash
hashcat --self-test-disable -m <hash-type> <hash-file> <wordlist>
```

## Forcer l'exécution

```bash
hashcat --force -m <hash-type> <hash-file> <wordlist>
```

## Kernel optimisé

```bash
hashcat -O -m <hash-type> <hash-file> <wordlist>
```

## Spinup delay

```bash
hashcat --spinup-damp <percent> -m <hash-type> <hash-file> <wordlist>
```

## Status timer

```bash
hashcat --status-timer <seconds> -m <hash-type> <hash-file> <wordlist>
```

---

# 12. Common Attack Scenarios

## Crack MD5

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt
```

## Crack SHA256

```bash
hashcat -a 0 -m 1400 hashes.txt rockyou.txt
```

## Crack NTLM

```bash
hashcat -a 0 -m 1000 hashes.txt rockyou.txt
```

## Crack WPA/WPA2

```bash
hashcat -a 0 -m 2500 wifi.hccapx rockyou.txt
```

## Crack bcrypt

```bash
hashcat -a 0 -m 3200 hashes.txt rockyou.txt
```

## Brute-force 8 chars

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a?a?a
```

## Attaque basée sur des règles

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Attaque hybride

```bash
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d
```

## Masque personnalisé

```bash
hashcat -a 3 -m 0 hashes.txt -1 abcdef ?1?1?1?1?1?1
```

## Plusieurs wordlists

```bash
hashcat -a 0 -m 0 hashes.txt wordlist1.txt wordlist2.txt
```

---

# 13. Practical Workflows

## Flux de travail de base de crack de mots de passe

```text
1. Identifier le type de hachage.
2. Préparer le fichier de hachage.
3. Sélectionner le mode d'attaque.
4. Choisir la wordlist/les règles.
5. Exécuter hashcat.
6. Examiner les mots de passe crackés.
7. Documenter les résultats.
```

## Exemple : Crack NTLM

```bash
# Préparer les hachages
echo "hash1" > hashes.txt
echo "hash2" >> hashes.txt

# Cracker avec rockyou
hashcat -a 0 -m 1000 hashes.txt rockyou.txt

# Afficher les résultats
hashcat --show -m 1000 hashes.txt
```

## Exemple : Crack WPA/WPA2

```bash
# Convertir en hccapx
hcxpcapngtool -o wifi.hccapx capture.pcapng

# Cracker
hashcat -a 0 -m 2500 wifi.hccapx rockyou.txt

# Afficher les résultats
hashcat --show -m 2500 wifi.hccapx
```

## Exemple : Brute-force

```bash
# 6 caractères minuscules
hashcat -a 3 -m 0 hashes.txt ?l?l?l?l?l?l

# 8 caractères mixtes
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a?a?a
```

## Exemple : Basé sur des règles

```bash
# Avec règles best64
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule

# Avec règles personnalisées
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r custom.rule
```

## Exemple : Attaque hybride

```bash
# Wordlist + 4 chiffres
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d

# 4 chiffres + wordlist
hashcat -a 7 -m 0 hashes.txt ?d?d?d?d rockyou.txt
```

---

# 14. Common Commands Reference

| Command | Description |
|---|---|
| `hashcat -h` | Afficher l'aide |
| `hashcat --version` | Afficher la version |
| `hashcat -a 0` | Attaque par dictionnaire |
| `hashcat -a 1` | Attaque combinatoire |
| `hashcat -a 3` | Attaque par brute-force |
| `hashcat -a 6` | Attaque hybride (wordlist + mask) |
| `hashcat -a 7` | Attaque hybride (mask + wordlist) |
| `hashcat -m <type>` | Spécifier le type de hachage |
| `hashcat -o <file>` | Fichier de sortie |
| `hashcat -r <rules>` | Utiliser un fichier de règles |
| `hashcat -d <device>` | Spécifier le dispositif GPU |
| `hashcat -w <profile>` | Profil de workload |
| `hashcat --show` | Afficher les crackés |
| `hashcat --left` | Afficher les restants |
| `hashcat --restore` | Restaurer la session |
| `hashcat --session <name>` | Nom de session |
| `hashcat -O` | Kernel optimisé |
| `hashcat --force` | Forcer l'exécution |
| `hashcat -v` | Sortie verbose |
| `hashcat -q` | Mode quiet |
| `hashcat --stdout` | Sortie vers stdout |

---

# 15. Troubleshooting

## Aucun hachage chargé

- Vérifiez le format du fichier de hachage.
- Assurez-vous que le type de hachage est correct.
- Vérifiez que les hachages ne sont pas déjà crackés.
- Vérifiez les permissions du fichier.

## Dispositif non trouvé

- Mettez à jour les pilotes GPU.
- Vérifiez l'installation OpenCL.
- Assurez-vous que le dispositif est détecté : `hashcat -I`
- Essayez un ID de dispositif différent.

## Out of memory

- Réduisez le profil de workload.
- Utilisez des wordlists plus petites.
- Fermez les autres applications GPU.
- Utilisez le CPU à la place.

## Performance lente

- Augmentez le profil de workload.
- Utilisez le kernel optimisé `-O`.
- Vérifiez l'utilisation du GPU.
- Vérifiez le refroidissement et le thermal throttling.

## Hachage invalide

- Vérifiez le type de hachage.
- Vérifiez le format du hachage.
- Supprimez les hachages invalides.
- Utilisez `--force` si nécessaire.

---

# 16. Security Best Practices

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
- Profitez de l'accélération GPU.
- Surveillez les ressources système.

## Gérez les ressources

- Surveillez la température du GPU.
- Évitez la surchauffe.
- Utilisez des profils de workload appropriés.
- Prenez des pauses entre les sessions.

## Documentez tout

- Enregistrez toutes les commandes utilisées.
- Notez les types et sources de hachages.
- Suivez la progression du crack.
- Documentez les résultats et méthodes.

---

# 17. Important Reminders

- Obtenez toujours une autorisation explicite avant d'utiliser Hashcat.
- Testez d'abord dans un environnement de laboratoire contrôlé.
- Tous les hachages ne sont pas crackables en temps raisonnable.
- Certaines attaques peuvent prendre un temps significatif.
- Maintenez Hashcat à jour régulièrement.
- Validez les résultats manuellement.
- Documentez toutes les actions et commandes.
- Préservez les preuves originales et les logs.
- Comprenez les implications légales et éthiques.
- Respectez les politiques de mots de passe et de sécurité.

---

# 18. Quick Reference Examples

## Attaque par dictionnaire de base

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt
```

## Brute-force 6 chars

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a
```

## Attaque basée sur des règles

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Crack NTLM

```bash
hashcat -a 0 -m 1000 hashes.txt rockyou.txt
```

## Crack WPA/WPA2

```bash
hashcat -a 0 -m 2500 wifi.hccapx rockyou.txt
```

## Afficher les crackés

```bash
hashcat --show -m 0 hashes.txt
```

## Afficher les restants

```bash
hashcat --left -m 0 hashes.txt
```

## Masque personnalisé

```bash
hashcat -a 3 -m 0 hashes.txt -1 abcdef ?1?1?1?1?1?1
```

## Attaque hybride

```bash
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d
```

## Plusieurs wordlists

```bash
hashcat -a 0 -m 0 hashes.txt wordlist1.txt wordlist2.txt
```

## Kernel optimisé

```bash
hashcat -O -a 0 -m 0 hashes.txt rockyou.txt
```

---

# 19. Additional Resources

## Hashcat Official

```text
https://hashcat.net/hashcat/
```

## Hashcat GitHub

```text
https://github.com/hashcat/hashcat
```

## Hashcat Rules

```text
https://github.com/hashcat/hashcat/tree/master/rules
```

## Hashcat Wiki

```text
https://hashcat.net/wiki/
```

## Weakpass Wordlists

```text
https://weakpass.com/
```
