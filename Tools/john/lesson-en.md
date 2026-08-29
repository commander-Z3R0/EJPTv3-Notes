# John the Ripper Cheat Sheet

## Overview

John the Ripper (John) is a fast password cracker used for:

- Cracking password hashes.
- Testing password strength.
- Performing dictionary attacks.
- Executing brute-force attacks.
- Conducting authorized security assessments.

John provides:

- Support for many hash types.
- Multiple attack modes.
- Wordlist and rule-based attacks.
- Incremental (brute-force) mode.
- Automatic hash type detection.

```text
Use John the Ripper only against hashes you own or are explicitly authorized to test.
```

---

# 1. Starting John

## Basic syntax

```bash
john [options] <hash-file>
```

## Show help

```bash
john --help
```

## Show version

```bash
john --version
```

## Update John

```bash
git clone https://github.com/openwall/john.git
cd john/src
make
```

## Available formats

```bash
john --list=formats
```

---

# 2. Hash Preparation

## Identify hash type

```bash
hashid <hash>
```

Or use online tools to identify hash type.

## Prepare hash file

```bash
echo "hash" > hashes.txt
```

Or:

```bash
cat hashes.txt
```

## Common hash formats

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

## Basic dictionary attack

```bash
john <hash-file>
```

Example:

```bash
john hashes.txt
```

## Dictionary attack with wordlist

```bash
john --wordlist=<wordlist> <hash-file>
```

Example:

```bash
john --wordlist=rockyou.txt hashes.txt
```

## Specify hash format

```bash
john --format=<format> --wordlist=<wordlist> <hash-file>
```

Example:

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## Show cracked passwords

```bash
john --show <hash-file>
```

## Show remaining hashes

```bash
john --show <hash-file> | grep -v "password hashes cracked"
```

## Remove cracked hashes

```bash
john --pot=<pot-file> <hash-file>
```

---

# 4. Attack Modes

## Dictionary attack

```bash
john --wordlist=<wordlist> <hash-file>
```

Example:

```bash
john --wordlist=rockyou.txt hashes.txt
```

## Incremental (brute-force)

```bash
john --incremental <hash-file>
```

## Incremental with specific charset

```bash
john --incremental=lowercase <hash-file>
```

## Incremental with custom charset

```bash
john --incremental=custom <hash-file>
```

## External mode

```bash
john --external=<mode> <hash-file>
```

## Hybrid attack

```bash
john --wordlist=<wordlist> --rules <hash-file>
```

## Rule-based attack

```bash
john --wordlist=<wordlist> --rules <hash-file>
```

---

# 5. Wordlist Options

## Use rockyou.txt

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt <hash-file>
```

## Use multiple wordlists

```bash
john --wordlist=wordlist1.txt,wordlist2.txt <hash-file>
```

## Use stdin

```bash
cat wordlist.txt | john --stdin <hash-file>
```

## Generate wordlist with crunch

```bash
crunch 8 8 -t password@@@ | john --stdin <hash-file>
```

## Use custom wordlist

```bash
john --wordlist=custom.txt <hash-file>
```

## Wordlist with rules

```bash
john --wordlist=wordlist.txt --rules <hash-file>
```

---

# 6. Rules

## Use default rules

```bash
john --wordlist=<wordlist> --rules <hash-file>
```

Example:

```bash
john --wordlist=rockyou.txt --rules hashes.txt
```

## Use specific rules

```bash
john --wordlist=<wordlist> --rules=<rules> <hash-file>
```

## List available rules

```bash
john --list=rules
```

## Create custom rules

Edit `john.conf` and add rules section:

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

## Apply custom rules

```bash
john --wordlist=wordlist.txt --rules=MyRules <hash-file>
```

## Rule examples

```text
:          - Do nothing
$1         - Append 1
$2         - Append 2
^1         - Prepend 1
c          - Capitalize first letter
C          - Capitalize all letters
l          - Lowercase all letters
u          - Uppercase all letters
i          - Invert case
d          - Duplicate word
p          - Permute characters
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
john --format=wpapsk --wordlist=rockyou.txt hashes.txt
```

## Kerberos

```bash
john --format=krb5-17 --wordlist=rockyou.txt hashes.txt
```

---

# 8. Performance Options

## Use all CPU cores

```bash
john --fork=<cores> <hash-file>
```

Example:

```bash
john --fork=4 hashes.txt
```

## Set session name

```bash
john --session=<name> <hash-file>
```

Example:

```bash
john --session=mycrack hashes.txt
```

## Restore session

```bash
john --restore=<name>
```

## Status of current session

```bash
john --status=<name>
```

## Limit runtime

```bash
john --max-run-time=<seconds> <hash-file>
```

## Limit password length

```bash
john --min-length=<min> --max-length=<max> <hash-file>
```

## Single crack mode

```bash
john --single <hash-file>
```

---

# 9. Output Options

## Show cracked passwords

```bash
john --show <hash-file>
```

## Show cracked with format

```bash
john --show --format=<format> <hash-file>
```

## Save output to file

```bash
john --show <hash-file> > cracked.txt
```

## Show only cracked

```bash
john --show <hash-file> | grep -v "password hashes cracked"
```

## Show statistics

```bash
john --show <hash-file> | tail
```

## Potfile location

```bash
~/.john/john.pot
```

## View potfile

```bash
cat ~/.john/john.pot
```

---

# 10. Advanced Options

## Generate wordlist from potfile

```bash
john --show <hash-file> | cut -d: -f2 > passwords.txt
```

## Make potfile readable

```bash
john --show <hash-file>
```

## Continue interrupted session

```bash
john --restore
```

## Stop session

Press `Ctrl+C` during execution.

## Save session state

John automatically saves session state.

## Clear potfile

```bash
rm ~/.john/john.pot
```

## Use specific potfile

```bash
john --pot=<pot-file> <hash-file>
```

## Disable potfile

```bash
john --pot=none <hash-file>
```

---

# 11. Common Attack Scenarios

## Basic dictionary attack

```bash
john --wordlist=rockyou.txt hashes.txt
```

## NTLM crack

```bash
john --format=NT --wordlist=rockyou.txt hashes.txt
```

## MD5 crack

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## SHA256 crack

```bash
john --format=raw-sha256 --wordlist=rockyou.txt hashes.txt
```

## WPA/WPA2 crack

```bash
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx
```

## Brute-force lowercase

```bash
john --incremental=lowercase hashes.txt
```

## Rule-based attack

```bash
john --wordlist=rockyou.txt --rules hashes.txt
```

## Single crack mode

```bash
john --single hashes.txt
```

## Multiple wordlists

```bash
john --wordlist=wordlist1.txt,wordlist2.txt hashes.txt
```

## Custom format

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

---

# 12. Practical Workflows

## Basic password cracking workflow

```text
1. Identify hash type.
2. Prepare hash file.
3. Select attack mode.
4. Choose wordlist/rules.
5. Execute John.
6. Review cracked passwords.
7. Document findings.
```

## Example: NTLM crack

```bash
# Prepare hashes
echo "hash1" > hashes.txt
echo "hash2" >> hashes.txt

# Crack with rockyou
john --format=NT --wordlist=rockyou.txt hashes.txt

# Show results
john --show hashes.txt
```

## Example: WPA/WPA2 crack

```bash
# Convert to hccapx
hcxpcapngtool -o wifi.hccapx capture.pcapng

# Crack
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx

# Show results
john --show wifi.hccapx
```

## Example: Brute-force

```bash
# Lowercase only
john --incremental=lowercase hashes.txt

# Full brute-force
john --incremental hashes.txt
```

## Example: Rule-based

```bash
# With default rules
john --wordlist=rockyou.txt --rules hashes.txt

# With custom rules
john --wordlist=rockyou.txt --rules=MyRules hashes.txt
```

## Example: Multiple formats

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
| `john --help` | Show help |
| `john --version` | Show version |
| `john <hash-file>` | Basic crack |
| `john --wordlist=<file>` | Dictionary attack |
| `john --incremental` | Brute-force |
| `john --rules` | Rule-based attack |
| `john --format=<format>` | Specify format |
| `john --show` | Show cracked |
| `john --restore` | Restore session |
| `john --status` | Show status |
| `john --session=<name>` | Session name |
| `john --fork=<cores>` | Use multiple cores |
| `john --single` | Single crack mode |
| `john --stdin` | Read from stdin |
| `john --pot=<file>` | Specify potfile |
| `john --list=formats` | List formats |
| `john --list=rules` | List rules |
| `john --max-run-time=<sec>` | Limit runtime |
| `john --min-length=<min>` | Min password length |
| `john --max-length=<max>` | Max password length |

---

# 14. Troubleshooting

## No hashes loaded

- Check hash file format.
- Verify hash type is correct.
- Ensure hashes are not already cracked.
- Check file permissions.

## Unknown hash format

- Specify correct format with `--format`.
- Check hash syntax.
- Remove invalid hashes.
- Use `--list=formats` to see available formats.

## Slow performance

- Use appropriate attack mode.
- Select efficient wordlists.
- Use `--fork` for multiple cores.
- Consider using Hashcat for GPU acceleration.

## Session interrupted

- Use `--restore` to continue.
- Check session file exists.
- Verify potfile is intact.
- Restart with same parameters.

## Invalid hash

- Verify hash type.
- Check hash format.
- Remove invalid hashes.
- Use correct format specification.

---

# 15. Security Best Practices

## Always verify results

- Test cracked passwords manually.
- Verify hash type is correct.
- Cross-reference with other tools.
- Document all findings.

## Respect legal boundaries

- Only crack hashes you own.
- Obtain explicit authorization.
- Follow responsible disclosure.
- Document all activities.

## Optimize performance

- Use appropriate attack modes.
- Select efficient wordlists.
- Leverage multiple cores.
- Monitor system resources.

## Manage resources

- Monitor CPU usage.
- Avoid overheating.
- Use appropriate workload.
- Take breaks between sessions.

## Document everything

- Record all commands used.
- Note hash types and sources.
- Track cracking progress.
- Document findings and methods.

---

# 16. Important Reminders

- Always obtain explicit authorization before using John.
- Test in a controlled lab environment first.
- Not all hashes are crackable in reasonable time.
- Some attacks may take significant time.
- Keep John updated regularly.
- Validate findings manually.
- Document all actions and commands.
- Preserve original evidence and logs.
- Understand the legal and ethical implications.
- Respect password policies and security.

---

# 17. Quick Reference Examples

## Basic dictionary attack

```bash
john --wordlist=rockyou.txt hashes.txt
```

## NTLM crack

```bash
john --format=NT --wordlist=rockyou.txt hashes.txt
```

## MD5 crack

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## Show cracked

```bash
john --show hashes.txt
```

## Rule-based attack

```bash
john --wordlist=rockyou.txt --rules hashes.txt
```

## Brute-force

```bash
john --incremental hashes.txt
```

## WPA/WPA2 crack

```bash
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx
```

## Multiple cores

```bash
john --fork=4 --wordlist=rockyou.txt hashes.txt
```

## Restore session

```bash
john --restore
```

## Custom format

```bash
john --format=raw-sha256 --wordlist=rockyou.txt hashes.txt
```

## Single crack mode

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

## Hashcat (GPU alternative)

```text
https://hashcat.net/hashcat/
```
