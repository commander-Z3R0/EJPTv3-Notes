# WPScan Cheat Sheet

## Overview

WPScan is a WordPress security scanner used for:

- Enumerating WordPress installations.
- Identifying vulnerable plugins and themes.
- Detecting weak credentials.
- Finding configuration issues.
- Conducting authorized security assessments.

WPScan provides:

- Comprehensive WordPress vulnerability database.
- Plugin and theme enumeration.
- User enumeration capabilities.
- Password brute-forcing.
- API integration for vulnerability data.

```text
Use WPScan only against systems you own or are explicitly authorized to test.
```

---

# 1. Starting WPScan

## Basic syntax

```bash
wpscan [options] -u <URL>
```

## Show help

```bash
wpscan -h
```

## Show version

```bash
wpscan --version
```

## Update WPScan

```bash
wpscan --update
```

Updates the tool and vulnerability database.

## Update vulnerability database only

```bash
wpscan --update-only
```

---

# 2. Target Specification

## Specify target URL

```bash
wpscan -u <URL>
```

Example:

```bash
wpscan -u http://192.168.1.10/
```

## Specify target URL with port

```bash
wpscan -u http://192.168.1.10:8080/
```

## Specify target URL with path

```bash
wpscan -u http://192.168.1.10/wordpress/
```

## Scan multiple URLs

```bash
wpscan -u <URL1> -u <URL2> -u <URL3>
```

Example:

```bash
wpscan -u http://192.168.1.10/ -u http://192.168.1.11/
```

## Scan from file

```bash
wpscan --url <file>
```

Where file contains one URL per line.

---

# 3. Authentication

## Specify username

```bash
wpscan -u <URL> --username <username>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --username admin
```

## Specify password

```bash
wpscan -u <URL> --password <password>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --username admin --password password123
```

## Specify cookie

```bash
wpscan -u <URL> --cookie <cookie>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --cookie "wordpress_logged_in=abc123"
```

## Specify User-Agent

```bash
wpscan -u <URL> --user-agent <agent>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --user-agent "Mozilla/5.0"
```

## Specify proxy

```bash
wpscan -u <URL> --proxy <proxy>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080
```

## Specify proxy authentication

```bash
wpscan -u <URL> --proxy <proxy> --proxy-auth <credentials>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080 --proxy-auth user:pass
```

---

# 4. Enumeration Options

## Enumerate plugins

```bash
wpscan -u <URL> --enumerate p
```

## Enumerate themes

```bash
wpscan -u <URL> --enumerate t
```

## Enumerate users

```bash
wpscan -u <URL> --enumerate u
```

## Enumerate all

```bash
wpscan -u <URL> --enumerate a
```

Enumerates plugins, themes, users, and more.

## Enumerate vulnerable plugins only

```bash
wpscan -u <URL> --enumerate vp
```

## Enumerate vulnerable themes only

```bash
wpscan -u <URL> --enumerate vt
```

## Enumerate popular plugins

```bash
wpscan -u <URL> --enumerate ap
```

## Enumerate popular themes

```bash
wpscan -u <URL> --enumerate at
```

## Enumerate timthumbs

```bash
wpscan -u <URL> --enumerate tt
```

## Enumerate config backups

```bash
wpscan -u <URL> --enumerate cb
```

## Enumerate database exports

```bash
wpscan -u <URL> --enumerate db
```

## Limit enumeration results

```bash
wpscan -u <URL> --enumerate u[1-10]
```

Limits user enumeration to first 10 users.

---

# 5. Vulnerability Detection

## Detect all vulnerabilities

```bash
wpscan -u <URL>
```

Performs full vulnerability scan.

## Detect vulnerable plugins

```bash
wpscan -u <URL> --enumerate vp
```

## Detect vulnerable themes

```bash
wpscan -u <URL> --enumerate vt
```

## Detect vulnerable plugins and themes

```bash
wpscan -u <URL> --enumerate vp,vt
```

## Force detection of all vulnerabilities

```bash
wpscan -u <URL> --force
```

## Skip vulnerability check

```bash
wpscan -u <URL> --no-vulnerability-check
```

---

# 6. Password Attacks

## Brute-force passwords

```bash
wpscan -u <URL> --passwords <wordlist>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --passwords /usr/share/wordlists/rockyou.txt
```

## Brute-force specific username

```bash
wpscan -u <URL> --username <username> --passwords <wordlist>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt
```

## Brute-force multiple usernames

```bash
wpscan -u <URL> --usernames <userlist> --passwords <wordlist>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --usernames users.txt --passwords passwords.txt
```

## Limit password attempts

```bash
wpscan -u <URL> --passwords <wordlist> --max-threads <threads>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --passwords passwords.txt --max-threads 10
```

## Stop after first success

```bash
wpscan -u <URL> --passwords <wordlist> --stop-on-success
```

---

# 7. API Integration

## Specify API token

```bash
wpscan -u <URL> --api-token <token>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --api-token abc123xyz
```

## Use API token for vulnerability data

```bash
wpscan -u <URL> --api-token <token> --enumerate vp
```

## Update API token

```bash
wpscan --update
```

## Check API status

```bash
wpscan --api-token <token>
```

---

# 8. Output and Logging

## Save output to file

```bash
wpscan -u <URL> -o <output-file>
```

Example:

```bash
wpscan -u http://192.168.1.10/ -o results.txt
```

## Save in JSON format

```bash
wpscan -u <URL> -f json -o <output-file>
```

Example:

```bash
wpscan -u http://192.168.1.10/ -f json -o results.json
```

## Save in CSV format

```bash
wpscan -u <URL> -f csv -o <output-file>
```

Example:

```bash
wpscan -u http://192.168.1.10/ -f csv -o results.csv
```

## Verbose output

```bash
wpscan -u <URL> -v
```

## Very verbose output

```bash
wpscan -u <URL> -vv
```

## Quiet mode

```bash
wpscan -u <URL> --quiet
```

## No color output

```bash
wpscan -u <URL> --no-color
```

## Log all requests

```bash
wpscan -u <URL> --log <log-file>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --log scan.log
```

---

# 9. Advanced Options

## Specify WordPress installation path

```bash
wpscan -u <URL> --wp-content-dir <path>
```

## Specify wp-includes path

```bash
wpscan -u <URL> --wp-includes-dir <path>
```

## Specify plugins path

```bash
wpscan -u <URL> --plugins-dir <path>
```

## Specify themes path

```bash
wpscan -u <URL> --themes-dir <path>
```

## Force WordPress detection

```bash
wpscan -u <URL> --force
```

## Skip WordPress detection

```bash
wpscan -u <URL> --no-wp-content-dir-check
```

## Disable TLS verification

```bash
wpscan -u <URL> --disable-tls-checks
```

## Specify timeout

```bash
wpscan -u <URL> --timeout <seconds>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --timeout 30
```

## Specify maximum threads

```bash
wpscan -u <URL> --max-threads <threads>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --max-threads 20
```

## Delay between requests

```bash
wpscan -u <URL> --throttle <milliseconds>
```

Example:

```bash
wpscan -u http://192.168.1.10/ --throttle 1000
```

---

# 10. Common Attack Scenarios

## Basic WordPress scan

```bash
wpscan -u http://192.168.1.10/
```

## Enumerate plugins

```bash
wpscan -u http://192.168.1.10/ --enumerate p
```

## Enumerate themes

```bash
wpscan -u http://192.168.1.10/ --enumerate t
```

## Enumerate users

```bash
wpscan -u http://192.168.1.10/ --enumerate u
```

## Enumerate all

```bash
wpscan -u http://192.168.1.10/ --enumerate a
```

## Detect vulnerable plugins

```bash
wpscan -u http://192.168.1.10/ --enumerate vp
```

## Brute-force admin password

```bash
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt
```

## Scan with API token

```bash
wpscan -u http://192.168.1.10/ --api-token abc123xyz
```

## Save results to JSON

```bash
wpscan -u http://192.168.1.10/ -f json -o results.json
```

## Scan with proxy

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080
```

## Scan multiple targets

```bash
wpscan -u http://192.168.1.10/ -u http://192.168.1.11/
```

## Verbose scan

```bash
wpscan -u http://192.168.1.10/ -v
```

---

# 11. Practical Workflows

## Basic WordPress security scan workflow

```text
1. Identify WordPress installation.
2. Run WPScan against target.
3. Enumerate plugins and themes.
4. Check for vulnerabilities.
5. Enumerate users.
6. Test for weak credentials.
7. Document findings.
```

## Example: Full enumeration

```bash
# Basic scan
wpscan -u http://192.168.1.10/

# Enumerate plugins
wpscan -u http://192.168.1.10/ --enumerate p

# Enumerate themes
wpscan -u http://192.168.1.10/ --enumerate t

# Enumerate users
wpscan -u http://192.168.1.10/ --enumerate u

# Enumerate all
wpscan -u http://192.168.1.10/ --enumerate a
```

## Example: Vulnerability detection

```bash
# Detect vulnerable plugins
wpscan -u http://192.168.1.10/ --enumerate vp

# Detect vulnerable themes
wpscan -u http://192.168.1.10/ --enumerate vt

# Full vulnerability scan
wpscan -u http://192.168.1.10/
```

## Example: Password brute-force

```bash
# Brute-force admin
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt

# Brute-force multiple users
wpscan -u http://192.168.1.10/ --usernames users.txt --passwords passwords.txt

# Stop on success
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt --stop-on-success
```

## Example: API integration

```bash
# Scan with API token
wpscan -u http://192.168.1.10/ --api-token abc123xyz

# Enumerate vulnerable plugins with API
wpscan -u http://192.168.1.10/ --api-token abc123xyz --enumerate vp
```

## Example: Output and logging

```bash
# Save to text file
wpscan -u http://192.168.1.10/ -o results.txt

# Save to JSON
wpscan -u http://192.168.1.10/ -f json -o results.json

# Save to CSV
wpscan -u http://192.168.1.10/ -f csv -o results.csv

# Verbose output
wpscan -u http://192.168.1.10/ -v -o results.txt
```

---

# 12. Common Commands Reference

| Command | Description |
|---|---|
| `wpscan -h` | Show help |
| `wpscan --version` | Show version |
| `wpscan -u <URL>` | Specify target URL |
| `wpscan --update` | Update WPScan |
| `wpscan --enumerate p` | Enumerate plugins |
| `wpscan --enumerate t` | Enumerate themes |
| `wpscan --enumerate u` | Enumerate users |
| `wpscan --enumerate a` | Enumerate all |
| `wpscan --enumerate vp` | Enumerate vulnerable plugins |
| `wpscan --enumerate vt` | Enumerate vulnerable themes |
| `wpscan --passwords <file>` | Specify password wordlist |
| `wpscan --username <user>` | Specify username |
| `wpscan --usernames <file>` | Specify username list |
| `wpscan --api-token <token>` | Specify API token |
| `wpscan -o <file>` | Save output to file |
| `wpscan -f json` | Output in JSON format |
| `wpscan -f csv` | Output in CSV format |
| `wpscan -v` | Verbose output |
| `wpscan --proxy <proxy>` | Use proxy |
| `wpscan --cookie <cookie>` | Specify cookie |
| `wpscan --user-agent <agent>` | Specify User-Agent |
| `wpscan --timeout <seconds>` | Set timeout |
| `wpscan --max-threads <threads>` | Set max threads |
| `wpscan --force` | Force WordPress detection |
| `wpscan --quiet` | Quiet mode |
| `wpscan --no-color` | No color output |

---

# 13. Plugin Enumeration

## List all plugins

```bash
wpscan -u <URL> --enumerate p
```

## List vulnerable plugins

```bash
wpscan -u <URL> --enumerate vp
```

## List popular plugins

```bash
wpscan -u <URL> --enumerate ap
```

## List plugins with specific range

```bash
wpscan -u <URL> --enumerate p[1-50]
```

## Detect plugin versions

```bash
wpscan -u <URL> --enumerate p
```

WPScan automatically detects plugin versions.

## Check plugin vulnerabilities

```bash
wpscan -u <URL> --enumerate vp
```

Uses WPScan vulnerability database.

---

# 14. Theme Enumeration

## List all themes

```bash
wpscan -u <URL> --enumerate t
```

## List vulnerable themes

```bash
wpscan -u <URL> --enumerate vt
```

## List popular themes

```bash
wpscan -u <URL> --enumerate at
```

## List themes with specific range

```bash
wpscan -u <URL> --enumerate t[1-20]
```

## Detect theme versions

```bash
wpscan -u <URL> --enumerate t
```

WPScan automatically detects theme versions.

## Check theme vulnerabilities

```bash
wpscan -u <URL> --enumerate vt
```

Uses WPScan vulnerability database.

---

# 15. User Enumeration

## List all users

```bash
wpscan -u <URL> --enumerate u
```

## List users with specific range

```bash
wpscan -u <URL> --enumerate u[1-10]
```

## List usernames only

```bash
wpscan -u <URL> --enumerate u
```

## Detect user roles

```bash
wpscan -u <URL> --enumerate u
```

WPScan detects user roles automatically.

## Check for weak passwords

```bash
wpscan -u <URL> --username admin --passwords passwords.txt
```

## Brute-force multiple users

```bash
wpscan -u <URL> --usernames users.txt --passwords passwords.txt
```

---

# 16. Troubleshooting

## WordPress not detected

- Verify WordPress installation.
- Use `--force` to force detection.
- Check if site is actually WordPress.
- Verify URL is correct.

## No plugins found

- Plugins may be hidden.
- Check wp-content/plugins directory.
- Use `--enumerate p` explicitly.
- Verify permissions.

## API errors

- Check API token validity.
- Verify internet connectivity.
- Update WPScan database.
- Check API rate limits.

## Slow performance

- Reduce max threads: `--max-threads 10`
- Add throttle: `--throttle 1000`
- Use proxy for anonymity.
- Limit enumeration range.

## False positives

- Verify vulnerabilities manually.
- Check plugin/theme versions.
- Review WPScan output carefully.
- Cross-reference with other sources.

---

# 17. Security Best Practices

## Always verify findings

- Check vulnerabilities manually.
- Verify plugin/theme versions.
- Test in controlled environment.
- Document all findings.

## Respect legal boundaries

- Only test systems you own.
- Obtain explicit authorization.
- Follow responsible disclosure.
- Document all activities.

## Minimize impact

- Use appropriate throttle settings.
- Avoid aggressive scanning.
- Test during maintenance windows.
- Monitor target system.

## Keep tools updated

- Update WPScan regularly.
- Update vulnerability database.
- Stay informed about new vulnerabilities.
- Follow security advisories.

## Use API for accurate results

- Register for WPScan API.
- Use API token for scans.
- Get latest vulnerability data.
- Improve scan accuracy.

---

# 18. Important Reminders

- Always obtain explicit authorization before using WPScan.
- Test in a controlled lab environment first.
- Not all detected vulnerabilities are exploitable.
- Some scans may impact website performance.
- Keep WPScan updated regularly.
- Validate findings manually; do not rely solely on automated results.
- Document all actions, commands, and results.
- Preserve original evidence and logs.
- Respect the scope and rules of engagement.
- Understand the legal and ethical implications of your actions.

---

# 19. Quick Reference Examples

## Basic scan

```bash
wpscan -u http://192.168.1.10/
```

## Enumerate plugins

```bash
wpscan -u http://192.168.1.10/ --enumerate p
```

## Enumerate themes

```bash
wpscan -u http://192.168.1.10/ --enumerate t
```

## Enumerate users

```bash
wpscan -u http://192.168.1.10/ --enumerate u
```

## Enumerate all

```bash
wpscan -u http://192.168.1.10/ --enumerate a
```

## Detect vulnerable plugins

```bash
wpscan -u http://192.168.1.10/ --enumerate vp
```

## Brute-force password

```bash
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt
```

## Use API token

```bash
wpscan -u http://192.168.1.10/ --api-token abc123xyz
```

## Save to JSON

```bash
wpscan -u http://192.168.1.10/ -f json -o results.json
```

## Verbose scan

```bash
wpscan -u http://192.168.1.10/ -v
```

## Use proxy

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080
```

## Update WPScan

```bash
wpscan --update
```

---

# 20. Additional Resources

## WPScan Official Website

```text
https://wpscan.com/
```

## WPScan GitHub Repository

```text
https://github.com/wpscanteam/wpscan
```

## WPScan Vulnerability Database

```text
https://wpscan.com/vulnerability-db
```

## WordPress Security

```text
https://wordpress.org/support/article/hardening-wordpress/
```

## OWASP WordPress Security

```text
https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Enumerate_Applications_on_Web_Server
```
