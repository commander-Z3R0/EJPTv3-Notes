# Hashcat Cheat Sheet

## Overview

Hashcat is the world's fastest and most advanced password recovery tool used for:

- Cracking password hashes.
- Testing password strength.
- Performing dictionary attacks.
- Executing brute-force attacks.
- Conducting authorized security assessments.

Hashcat provides:

- Support for 300+ hash types.
- Multiple attack modes.
- GPU acceleration.
- Rule-based attacks.
- Custom charset and mask attacks.

```text
Use Hashcat only against hashes you own or are explicitly authorized to test.
```

---

# 1. Starting Hashcat

## Basic syntax

```bash
hashcat [options] -m <hash-type> -a <attack-mode> <hash-file> <wordlist>
```

## Show help

```bash
hashcat -h
```

## Show version

```bash
hashcat --version
```

## Show supported hash types

```bash
hashcat --help | grep -A 100 "Hash modes"
```

## Show supported attack modes

```bash
hashcat --help | grep -A 10 "Attack modes"
```

## Update Hashcat

```bash
git clone https://github.com/hashcat/hashcat.git
cd hashcat
make
sudo make install
```

---

# 2. Hash Types

## Common hash types

```bash
# MD5
-m 0

# MD5 with salt
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

## List all hash types

```bash
hashcat --help
```

## Identify hash type

```bash
hashid <hash>
```

Or use online tools to identify hash type.

---

# 3. Attack Modes

## Dictionary attack

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist>
```

Example:

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt
```

## Combinator attack

```bash
hashcat -a 1 -m <hash-type> <hash-file> <wordlist1> <wordlist2>
```

Example:

```bash
hashcat -a 1 -m 0 hashes.txt wordlist1.txt wordlist2.txt
```

## Brute-force attack

```bash
hashcat -a 3 -m <hash-type> <hash-file> <mask>
```

Example:

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a
```

## Hybrid attack (wordlist + mask)

```bash
hashcat -a 6 -m <hash-type> <hash-file> <wordlist> <mask>
```

Example:

```bash
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d
```

## Hybrid attack (mask + wordlist)

```bash
hashcat -a 7 -m <hash-type> <hash-file> <mask> <wordlist>
```

Example:

```bash
hashcat -a 7 -m 0 hashes.txt ?d?d?d?d rockyou.txt
```

## Rule-based attack

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r <rules>
```

Example:

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

---

# 4. Dictionary Attacks

## Basic dictionary attack

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist>
```

Example:

```bash
hashcat -a 0 -m 1000 hashes.txt rockyou.txt
```

## Dictionary attack with multiple wordlists

```bash
hashcat -a 0 -m <hash-type> <hash-file> wordlist1.txt wordlist2.txt
```

## Dictionary attack with rules

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r <rules-file>
```

Example:

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Dictionary attack with multiple rules

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r rules1.rule -r rules2.rule
```

## Dictionary attack with custom rules

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r custom.rule
```

## Dictionary attack with stdout

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> --stdout
```

Shows generated passwords without cracking.

---

# 5. Brute-Force Attacks

## Basic brute-force

```bash
hashcat -a 3 -m <hash-type> <hash-file> <mask>
```

Example:

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a
```

## Numeric brute-force

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?d?d?d?d?d?d
```

Example:

```bash
hashcat -a 3 -m 0 hashes.txt ?d?d?d?d
```

## Lowercase brute-force

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?l?l?l?l?l?l
```

Example:

```bash
hashcat -a 3 -m 0 hashes.txt ?l?l?l?l
```

## Uppercase brute-force

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?u?u?u?u?u?u
```

Example:

```bash
hashcat -a 3 -m 0 hashes.txt ?u?u?u?u
```

## Mixed case brute-force

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?a?a?a?a?a?a
```

## Custom charset brute-force

```bash
hashcat -a 3 -m <hash-type> <hash-file> -1 <charset> <custom-mask>
```

Example:

```bash
hashcat -a 3 -m 0 hashes.txt -1 abcdef ?1?1?1?1
```

## Complex mask

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?u?l?l?l?d?d?d
```

Example:

```bash
hashcat -a 3 -m 0 hashes.txt ?u?l?l?l?d?d?d
```

---

# 6. Mask Attack

## Mask charset definitions

```text
?l = abcdefghijklmnopqrstuvwxyz (lowercase)
?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ (uppercase)
?d = 0123456789 (digits)
?h = 0123456789abcdef (lowercase hex)
?H = 0123456789ABCDEF (uppercase hex)
?s =  !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~ (special chars)
?a = ?l?u?d?s (all printable)
?b = 0x00 - 0xff (all bytes)
```

## Custom charset

```bash
hashcat -a 3 -m <hash-type> <hash-file> -1 <charset> <mask>
```

Example:

```bash
hashcat -a 3 -m 0 hashes.txt -1 custom ?1?1?1?1
```

## Multiple custom charsets

```bash
hashcat -a 3 -m <hash-type> <hash-file> -1 <charset1> -2 <charset2> <mask>
```

Example:

```bash
hashcat -a 3 -m 0 hashes.txt -1 abc -2 123 ?1?1?2?2
```

## Known prefix mask

```bash
hashcat -a 3 -m <hash-type> <hash-file> Password?d?d?d
```

## Known suffix mask

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?l?l?l?l123
```

## Complex mask example

```bash
hashcat -a 3 -m 0 hashes.txt ?u?l?l?l?l?l?d?d
```

---

# 7. Rule-Based Attacks

## Use built-in rules

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r <rules-file>
```

Example:

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Common rule files

```text
rules/best64.rule
rules/d3ad0ne.rule
rules/InsidePro-HashManager.rule
rules/OneRuleToRuleThemAll.rule
rules/T0XlC.rule
```

## Create custom rules

Create a file `custom.rule`:

```text
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
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r custom.rule
```

## Multiple rule files

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r rules1.rule -r rules2.rule
```

## Generate rules with stats

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r rules/best64.rule --stdout
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

# 8. Performance Options

## Specify GPU device

```bash
hashcat -d <device-id> -m <hash-type> <hash-file> <wordlist>
```

Example:

```bash
hashcat -d 1 -m 0 hashes.txt rockyou.txt
```

## Use all GPUs

```bash
hashcat -d 1,2,3,4 -m <hash-type> <hash-file> <wordlist>
```

## Set workload profile

```bash
hashcat -w <profile> -m <hash-type> <hash-file> <wordlist>
```

Profiles:

- 1 - Low (optimized for background)
- 2 - Default (balanced)
- 3 - High (optimized for speed)
- 4 - Insane (maximum performance)

## Limit kernel runtime

```bash
hashcat --kernel-timeout <seconds> -m <hash-type> <hash-file> <wordlist>
```

## Set thread count

```bash
hashcat -t <threads> -m <hash-type> <hash-file> <wordlist>
```

## Use CPU only

```bash
hashcat --force -m <hash-type> <hash-file> <wordlist>
```

---

# 9. Output Options

## Specify output file

```bash
hashcat -o <output-file> -m <hash-type> <hash-file> <wordlist>
```

Example:

```bash
hashcat -o cracked.txt -m 0 hashes.txt rockyou.txt
```

## Show cracked passwords

```bash
hashcat --show -m <hash-type> <hash-file>
```

## Show remaining hashes

```bash
hashcat --left -m <hash-type> <hash-file>
```

## Output format

```bash
hashcat -o <output-file> --outfile-format <format> -m <hash-type> <hash-file> <wordlist>
```

Formats:

- 1 - Hash:Plain
- 2 - Plain
- 3 - Hex-Plain
- 4 - Crack-Pos
- 5 - Timestamp:Plain

## Verbose output

```bash
hashcat -v -m <hash-type> <hash-file> <wordlist>
```

## Quiet mode

```bash
hashcat -q -m <hash-type> <hash-file> <wordlist>
```

## Debug mode

```bash
hashcat --debug-mode <mode> -m <hash-type> <hash-file> <wordlist>
```

---

# 10. Session Management

## Specify session name

```bash
hashcat --session <name> -m <hash-type> <hash-file> <wordlist>
```

Example:

```bash
hashcat --session mycrack -m 0 hashes.txt rockyou.txt
```

## List sessions

```bash
hashcat --list
```

## Restore session

```bash
hashcat --restore --session <name>
```

## Remove session

```bash
hashcat --remove --session <name>
```

## Save progress

```bash
hashcat --checkpoint-disable -m <hash-type> <hash-file> <wordlist>
```

## Pause session

Press `p` during execution to pause.

## Resume session

Press `s` during execution to resume.

## Quit session

Press `q` during execution to quit.

---

# 11. Advanced Options

## Potfile management

```bash
# Show potfile
hashcat --show -m <hash-type> <hash-file>

# Disable potfile
hashcat --potfile-disable -m <hash-type> <hash-file> <wordlist>

# Custom potfile
hashcat --potfile-path <path> -m <hash-type> <hash-file> <wordlist>
```

## Loopback attack

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> --loopback
```

## Self-test disable

```bash
hashcat --self-test-disable -m <hash-type> <hash-file> <wordlist>
```

## Force execution

```bash
hashcat --force -m <hash-type> <hash-file> <wordlist>
```

## Optimized kernel

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

## MD5 crack

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt
```

## SHA256 crack

```bash
hashcat -a 0 -m 1400 hashes.txt rockyou.txt
```

## NTLM crack

```bash
hashcat -a 0 -m 1000 hashes.txt rockyou.txt
```

## WPA/WPA2 crack

```bash
hashcat -a 0 -m 2500 wifi.hccapx rockyou.txt
```

## bcrypt crack

```bash
hashcat -a 0 -m 3200 hashes.txt rockyou.txt
```

## Brute-force 8 chars

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a?a?a
```

## Rule-based attack

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Hybrid attack

```bash
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d
```

## Custom mask

```bash
hashcat -a 3 -m 0 hashes.txt -1 abcdef ?1?1?1?1?1?1
```

## Multiple wordlists

```bash
hashcat -a 0 -m 0 hashes.txt wordlist1.txt wordlist2.txt
```

---

# 13. Practical Workflows

## Basic password cracking workflow

```text
1. Identify hash type.
2. Prepare hash file.
3. Select attack mode.
4. Choose wordlist/rules.
5. Execute hashcat.
6. Review cracked passwords.
7. Document findings.
```

## Example: NTLM crack

```bash
# Prepare hashes
echo "hash1" > hashes.txt
echo "hash2" >> hashes.txt

# Crack with rockyou
hashcat -a 0 -m 1000 hashes.txt rockyou.txt

# Show results
hashcat --show -m 1000 hashes.txt
```

## Example: WPA/WPA2 crack

```bash
# Convert to hccapx
hcxpcapngtool -o wifi.hccapx capture.pcapng

# Crack
hashcat -a 0 -m 2500 wifi.hccapx rockyou.txt

# Show results
hashcat --show -m 2500 wifi.hccapx
```

## Example: Brute-force

```bash
# 6 character lowercase
hashcat -a 3 -m 0 hashes.txt ?l?l?l?l?l?l

# 8 character mixed
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a?a?a
```

## Example: Rule-based

```bash
# With best64 rules
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule

# With custom rules
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r custom.rule
```

## Example: Hybrid attack

```bash
# Wordlist + 4 digits
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d

# 4 digits + wordlist
hashcat -a 7 -m 0 hashes.txt ?d?d?d?d rockyou.txt
```

---

# 14. Common Commands Reference

| Command | Description |
|---|---|
| `hashcat -h` | Show help |
| `hashcat --version` | Show version |
| `hashcat -a 0` | Dictionary attack |
| `hashcat -a 1` | Combinator attack |
| `hashcat -a 3` | Brute-force attack |
| `hashcat -a 6` | Hybrid attack (wordlist + mask) |
| `hashcat -a 7` | Hybrid attack (mask + wordlist) |
| `hashcat -m <type>` | Specify hash type |
| `hashcat -o <file>` | Output file |
| `hashcat -r <rules>` | Use rules file |
| `hashcat -d <device>` | Specify GPU device |
| `hashcat -w <profile>` | Workload profile |
| `hashcat --show` | Show cracked |
| `hashcat --left` | Show remaining |
| `hashcat --restore` | Restore session |
| `hashcat --session <name>` | Session name |
| `hashcat -O` | Optimized kernel |
| `hashcat --force` | Force execution |
| `hashcat -v` | Verbose output |
| `hashcat -q` | Quiet mode |
| `hashcat --stdout` | Output to stdout |

---

# 15. Troubleshooting

## No hashes loaded

- Check hash file format.
- Verify hash type is correct.
- Ensure hashes are not already cracked.
- Check file permissions.

## Device not found

- Update GPU drivers.
- Check OpenCL installation.
- Verify device is detected: `hashcat -I`
- Try different device ID.

## Out of memory

- Reduce workload profile.
- Use smaller wordlists.
- Close other GPU applications.
- Use CPU instead.

## Slow performance

- Increase workload profile.
- Use optimized kernel `-O`.
- Check GPU utilization.
- Verify cooling and thermal throttling.

## Invalid hash

- Verify hash type.
- Check hash format.
- Remove invalid hashes.
- Use `--force` if necessary.

---

# 16. Security Best Practices

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
- Leverage GPU acceleration.
- Monitor system resources.

## Manage resources

- Monitor GPU temperature.
- Avoid overheating.
- Use appropriate workload profiles.
- Take breaks between sessions.

## Document everything

- Record all commands used.
- Note hash types and sources.
- Track cracking progress.
- Document findings and methods.

---

# 17. Important Reminders

- Always obtain explicit authorization before using Hashcat.
- Test in a controlled lab environment first.
- Not all hashes are crackable in reasonable time.
- Some attacks may take significant time.
- Keep Hashcat updated regularly.
- Validate findings manually.
- Document all actions and commands.
- Preserve original evidence and logs.
- Understand the legal and ethical implications.
- Respect password policies and security.

---

# 18. Quick Reference Examples

## Basic dictionary attack

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt
```

## Brute-force 6 chars

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a
```

## Rule-based attack

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## NTLM crack

```bash
hashcat -a 0 -m 1000 hashes.txt rockyou.txt
```

## WPA/WPA2 crack

```bash
hashcat -a 0 -m 2500 wifi.hccapx rockyou.txt
```

## Show cracked

```bash
hashcat --show -m 0 hashes.txt
```

## Show remaining

```bash
hashcat --left -m 0 hashes.txt
```

## Custom mask

```bash
hashcat -a 3 -m 0 hashes.txt -1 abcdef ?1?1?1?1?1?1
```

## Hybrid attack

```bash
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d
```

## Multiple wordlists

```bash
hashcat -a 0 -m 0 hashes.txt wordlist1.txt wordlist2.txt
```

## Optimized kernel

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
