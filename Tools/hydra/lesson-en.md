# Hydra Cheat Sheet

## Overview

Hydra is a fast and flexible online password cracking tool used for:

- Brute-forcing login credentials.
- Testing password policies.
- Performing dictionary attacks.
- Validating authentication mechanisms.
- Conducting authorized security assessments.

Hydra supports:

- Multiple protocols (SSH, FTP, HTTP, SMB, MySQL, etc.).
- Parallel connections for speed.
- Custom wordlists and username lists.
- Proxy support.
- Save and resume attacks.

```text
Use Hydra only against systems you own or are explicitly authorized to test.
```

---

# 1. Starting Hydra

## Basic syntax

```bash
hydra [options] <target> <service>
```

## Show help

```bash
hydra -h
```

## Show version

```bash
hydra -V
```

## Show supported protocols

```bash
hydra -h
```

Look for the list of supported services at the bottom of the help output.

---

# 2. Target Specification

## Specify a single target

```bash
hydra <target-IP> <service>
```

Example:

```bash
hydra 192.168.1.10 ssh
```

## Specify multiple targets

```bash
hydra -M targets.txt <service>
```

Where `targets.txt` contains one IP per line.

## Specify a target port

```bash
hydra -s <port> <target-IP> <service>
```

Example:

```bash
hydra -s 2222 192.168.1.10 ssh
```

## Specify target via URL

For HTTP/HTTPS services:

```bash
hydra <url> <service>
```

Example:

```bash
hydra http://192.168.1.10/login http-get-form
```

---

# 3. Username and Password Lists

## Specify a single username

```bash
hydra -l <username> <target-IP> <service>
```

Example:

```bash
hydra -l admin 192.168.1.10 ssh
```

## Specify a single password

```bash
hydra -p <password> <target-IP> <service>
```

Example:

```bash
hydra -p password123 192.168.1.10 ssh
```

## Specify a username list

```bash
hydra -L <userlist.txt> <target-IP> <service>
```

Example:

```bash
hydra -L users.txt 192.168.1.10 ssh
```

## Specify a password list

```bash
hydra -P <passlist.txt> <target-IP> <service>
```

Example:

```bash
hydra -P passwords.txt 192.168.1.10 ssh
```

## Specify both username and password lists

```bash
hydra -L users.txt -P passwords.txt <target-IP> <service>
```

## Use a combination list (user:pass)

```bash
hydra -C combo.txt <target-IP> <service>
```

Where `combo.txt` contains lines in the format `username:password`.

## Generate usernames dynamically

For some services, Hydra can generate usernames:

```bash
hydra -L users.txt -P passwords.txt <target-IP> <service>
```

---

# 4. Connection and Performance Options

## Set number of parallel tasks

```bash
hydra -t <tasks> <target-IP> <service>
```

Example:

```bash
hydra -t 16 192.168.1.10 ssh
```

Default is 16 tasks.

## Set number of parallel connections per target

```bash
hydra -c <connections> <target-IP> <service>
```

Example:

```bash
hydra -c 4 192.168.1.10 ssh
```

## Set timeout for connections

```bash
hydra -w <seconds> <target-IP> <service>
```

Example:

```bash
hydra -w 30 192.168.1.10 ssh
```

## Set maximum number of retries

```bash
hydra -r <retries> <target-IP> <service>
```

Example:

```bash
hydra -r 3 192.168.1.10 ssh
```

## Wait time between attempts

```bash
hydra -d <delay> <target-IP> <service>
```

Example:

```bash
hydra -d 1 192.168.1.10 ssh
```

Adds a 1-second delay between attempts.

## Exit after finding first valid credential

```bash
hydra -f <target-IP> <service>
```

Stops after the first successful login.

## Exit after finding valid credentials per user

```bash
hydra -F <target-IP> <service>
```

Stops after finding one valid credential per username.

---

# 5. Output and Logging

## Show verbose output

```bash
hydra -v <target-IP> <service>
```

## Show very verbose output

```bash
hydra -d <target-IP> <service>
```

## Save output to a file

```bash
hydra -o output.txt <target-IP> <service>
```

## Save in a specific format

```bash
hydra -o output.txt -oN <target-IP> <service>
```

Formats:

- `-oN` — Normal text.
- `-oJ` — JSON.
- `-oX` — XML.

Example:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh -oJ results.json
```

## Show found credentials in real-time

Hydra displays found credentials automatically in verbose mode.

---

# 6. Protocol-Specific Options

## SSH

```bash
hydra -L users.txt -P passwords.txt <target-IP> ssh
```

Example:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## FTP

```bash
hydra -L users.txt -P passwords.txt <target-IP> ftp
```

## HTTP Basic Auth

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-get
```

## HTTP Form-based Auth

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "<path>:<parameters>:<fail_string>"
```

Example:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

- `/login.php` — Login page path.
- `user=^USER^&pass=^PASS^` — POST parameters.
- `Invalid` — String that indicates failure.

## HTTPS

```bash
hydra -L users.txt -P passwords.txt <target-IP> https-get
```

## SMB

```bash
hydra -L users.txt -P passwords.txt <target-IP> smb
```

## MySQL

```bash
hydra -L users.txt -P passwords.txt <target-IP> mysql
```

## PostgreSQL

```bash
hydra -L users.txt -P passwords.txt <target-IP> postgres
```

## RDP

```bash
hydra -L users.txt -P passwords.txt <target-IP> rdp
```

## Telnet

```bash
hydra -L users.txt -P passwords.txt <target-IP> telnet
```

## SMTP

```bash
hydra -L users.txt -P passwords.txt <target-IP> smtp
```

## IMAP

```bash
hydra -L users.txt -P passwords.txt <target-IP> imap
```

## POP3

```bash
hydra -L users.txt -P passwords.txt <target-IP> pop3
```

## LDAP

```bash
hydra -L users.txt -P passwords.txt <target-IP> ldap
```

## VNC

```bash
hydra -L users.txt -P passwords.txt <target-IP> vnc
```

---

# 7. Advanced Options

## Use a proxy

```bash
hydra -p <password> -P <passlist.txt> -X <proxy> <target-IP> <service>
```

Example:

```bash
hydra -L users.txt -P passwords.txt -X socks4://127.0.0.1:9050 192.168.1.10 ssh
```

## Use SSL/TLS

For services that support SSL:

```bash
hydra -s <port> -S <target-IP> <service>
```

Example:

```bash
hydra -s 443 -S 192.168.1.10 https-get
```

## Specify module options

Some modules support additional options:

```bash
hydra -m <options> <target-IP> <service>
```

Example for HTTP GET:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-get -m /admin
```

## Resume a previous attack

```bash
hydra -r <session.restore>
```

Hydra automatically creates a session file when interrupted.

## Save session for later resume

Hydra saves session automatically on interrupt (Ctrl+C).

## Ignore existing session

```bash
hydra -I <target-IP> <service>
```

Ignores any existing restore file.

---

# 8. Common Attack Scenarios

## SSH brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## SSH with single username

```bash
hydra -l admin -P passwords.txt 192.168.1.10 ssh
```

## SSH with custom port

```bash
hydra -s 2222 -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## FTP brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ftp
```

## HTTP Basic Auth brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-get
```

## HTTP Form-based login

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## SMB brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 smb
```

## MySQL brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 mysql
```

## RDP brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 rdp
```

## Telnet brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 telnet
```

## SMTP brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 smtp
```

## VNC brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 vnc
```

---

# 9. HTTP Form Attacks

## HTTP POST form with custom parameters

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "<path>:<parameters>:<fail_string>"
```

Example:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:username=^USER^&password=^PASS^:Login failed"
```

## HTTP POST form with cookies

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "<path>:<parameters>:<fail_string>:<headers>"
```

Example:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid:Cookie: session=abc123"
```

## HTTP GET form

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-get-form "<path>:<parameters>:<fail_string>"
```

Example:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-get-form "/login.php?user=^USER^&pass=^PASS^:Invalid"
```

## HTTP form with success string

Use `F=` for failure string or `S=` for success string:

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "/login.php:user=^USER^&pass=^PASS^:S=Welcome"
```

## HTTP form with multiple failure conditions

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "/login.php:user=^USER^&pass=^PASS^:F=Invalid:F=Error"
```

---

# 10. Practical Workflows

## Basic SSH brute force workflow

```text
1. Prepare username list (users.txt).
2. Prepare password list (passwords.txt).
3. Run Hydra against SSH.
4. Review output for valid credentials.
5. Validate credentials manually.
6. Document findings.
```

## Example: Full SSH attack

```bash
hydra -L users.txt -P passwords.txt -vV 192.168.1.10 ssh
```

- `-vV` — Very verbose output.

## Example: HTTP form attack

```bash
hydra -L users.txt -P passwords.txt -vV 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## Example: Multiple targets

```bash
hydra -M targets.txt -L users.txt -P passwords.txt ssh
```

Where `targets.txt` contains:

```text
192.168.1.10
192.168.1.11
192.168.1.12
```

## Example: Stop after first success

```bash
hydra -L users.txt -P passwords.txt -f 192.168.1.10 ssh
```

## Example: Custom port and timeout

```bash
hydra -s 2222 -w 30 -L users.txt -P passwords.txt 192.168.1.10 ssh
```

---

# 11. Common Commands Reference

| Command | Description |
|---|---|
| `hydra -h` | Show help |
| `hydra -V` | Show version |
| `hydra -l <user>` | Specify single username |
| `hydra -L <file>` | Specify username list |
| `hydra -p <pass>` | Specify single password |
| `hydra -P <file>` | Specify password list |
| `hydra -C <file>` | Specify combo file (user:pass) |
| `hydra -t <tasks>` | Set number of parallel tasks |
| `hydra -s <port>` | Specify target port |
| `hydra -M <file>` | Specify target list |
| `hydra -o <file>` | Save output to file |
| `hydra -v` | Verbose output |
| `hydra -V` | Very verbose output |
| `hydra -f` | Exit after first valid credential |
| `hydra -F` | Exit after valid credential per user |
| `hydra -w <seconds>` | Set connection timeout |
| `hydra -r <retries>` | Set maximum retries |
| `hydra -d <delay>` | Set delay between attempts |
| `hydra -I` | Ignore existing session |
| `hydra -X <proxy>` | Use proxy |
| `hydra -S` | Use SSL |
| `hydra -m <options>` | Module-specific options |

---

# 12. Wordlist Tips

## Common wordlist locations

- `/usr/share/wordlists/`
- `/usr/share/seclists/`
- `rockyou.txt`
- `common-passwords.txt`

## Create a custom wordlist

```bash
echo "password123" >> passwords.txt
echo "admin123" >> passwords.txt
```

## Use multiple wordlists

```bash
cat list1.txt list2.txt > combined.txt
```

## Remove duplicates

```bash
sort passwords.txt | uniq > passwords_unique.txt
```

## Generate variations

Use tools like `crunch` to generate custom wordlists:

```bash
crunch 8 12 -t password@@@ -o generated.txt
```

---

# 13. Common Exploits and Scenarios

## SSH with default credentials

```bash
hydra -l root -P passwords.txt 192.168.1.10 ssh
```

## FTP anonymous login

```bash
hydra -l anonymous -p anonymous 192.168.1.10 ftp
```

## HTTP admin panel

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/admin/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## MySQL root access

```bash
hydra -l root -P passwords.txt 192.168.1.10 mysql
```

## SMB guest access

```bash
hydra -l guest -P passwords.txt 192.168.1.10 smb
```

## RDP administrator

```bash
hydra -l Administrator -P passwords.txt 192.168.1.10 rdp
```

## Telnet root

```bash
hydra -l root -P passwords.txt 192.168.1.10 telnet
```

## SMTP authentication

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 smtp
```

---

# 14. Troubleshooting

## Connection refused

- Check if the service is running.
- Verify the port number.
- Ensure firewall allows connections.

## Too many errors

- Reduce parallel tasks: `-t 4`
- Increase timeout: `-w 30`
- Add delay: `-d 1`

## No valid credentials found

- Try a different wordlist.
- Check if the service requires special parameters.
- Verify the failure string for HTTP forms.

## Service not supported

- Check supported protocols: `hydra -h`
- Some services may require specific modules.

## Session restore issues

- Ignore existing session: `hydra -I`
- Delete restore file manually.

---

# 15. Important Reminders

- Always obtain explicit authorization before using Hydra.
- Test in a controlled lab environment first.
- Brute-forcing can lock accounts or trigger alerts.
- Respect rate limits and account lockout policies.
- Some services may detect and block brute-force attempts.
- Document all actions, commands, and results.
- Validate findings manually; do not rely solely on automated results.
- Preserve original evidence and logs.
- Respect the scope and rules of engagement.
- Understand the legal and ethical implications of your actions.

---

# 16. Quick Reference Examples

## SSH brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## HTTP form attack

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## FTP with single user

```bash
hydra -l admin -P passwords.txt 192.168.1.10 ftp
```

## Multiple targets

```bash
hydra -M targets.txt -L users.txt -P passwords.txt ssh
```

## Stop after first success

```bash
hydra -L users.txt -P passwords.txt -f 192.168.1.10 ssh
```

## Custom port and verbose

```bash
hydra -s 2222 -vV -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## Save results to JSON

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh -oJ results.json
```

## Use proxy

```bash
hydra -L users.txt -P passwords.txt -X socks4://127.0.0.1:9050 192.168.1.10 ssh
```

---

# 17. Supported Protocols (Partial List)

- `ssh`
- `ftp`
- `http-get`
- `http-post-form`
- `https-get`
- `https-post-form`
- `smb`
- `mysql`
- `postgres`
- `rdp`
- `telnet`
- `smtp`
- `imap`
- `pop3`
- `ldap`
- `vnc`
- `snmp`
- `cisco`
- `oracle`
- `mssql`

For a full list:

```bash
hydra -h
```
