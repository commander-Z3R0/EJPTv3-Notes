# Gobuster Command Cheat Sheet

## Overview

Gobuster is a high-performance brute-force tool written in Go, used to discover:

- Hidden directories and files on web servers.
- DNS subdomains (with wildcard support).
- Virtual hosts (VHosts).
- Parameters and values through fuzzing.
- Cloud storage buckets (S3, GCS).

Only use Gobuster against systems you own or that are explicitly included in an authorized assessment.

```text
Replace <URL>, <domain>, and <wordlist> with the authorized target values.
```

---

# 1. Basic Gobuster Commands

## Quick installation

```bash
# Using Go (recommended)
go install github.com/OJ/gobuster/v3@latest

# On Kali/Debian
sudo apt install gobuster
```

## View general help

```bash
gobuster help
```

## View help for a specific mode

```bash
gobuster dir help
```

---

# 2. Directory Mode (dir)

The most commonly used mode for enumerating directories and files on web servers.

## Basic directory scan

```bash
gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt
```

## Scan with extensions

Searches each wordlist entry with additional extensions:

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js,txt,bak
```

## Specify status codes to display

Shows only responses with specific status codes:

```bash
gobuster dir -u https://example.com -w wordlist.txt -s 200,204,301,302,307,401,403
```

## Exclude status codes

Hides responses with specific status codes:

```bash
gobuster dir -u https://example.com -w wordlist.txt -b 404
```

## Increase threads

Increases the number of concurrent threads (default: 10):

```bash
gobuster dir -u https://example.com -w wordlist.txt -t 50
```

## Add delay between requests

Adds a delay to reduce load on the target:

```bash
gobuster dir -u https://example.com -w wordlist.txt --delay 1500ms
```

## Use HTTP proxy

Sends traffic through a proxy (e.g., Burp Suite):

```bash
gobuster dir -u https://example.com -w wordlist.txt -p http://127.0.0.1:8080
```

## Add custom headers

```bash
gobuster dir -u https://example.com -w wordlist.txt -H "Authorization: Bearer TOKEN"
```

## Use session cookies

```bash
gobuster dir -u https://example.com -w wordlist.txt -c "session=123456;user=admin"
```

## Skip TLS certificate verification

```bash
gobuster dir -u https://example.com -w wordlist.txt -k
```

## Save results to file

```bash
gobuster dir -u https://example.com -w wordlist.txt -o results.txt
```

## Quiet mode (no banner)

```bash
gobuster dir -u https://example.com -w wordlist.txt -q
```

## Show verbose output

```bash
gobuster dir -u https://example.com -w wordlist.txt -v
```

## Exclude specific response length

Useful for filtering responses with constant size (e.g., custom error pages):

```bash
gobuster dir -u https://example.com -w wordlist.txt --exclude-length 1587
```

## Scan with multiple extensions and specific paths

```bash
gobuster dir -u https://example.com/admin -w wordlist.txt -x php,html -t 40 -s 200,301,302,403
```

---

# 3. DNS Mode (dns)

Discovers subdomains through DNS resolution.

## Basic subdomain scan

```bash
gobuster dns -d example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

## Use custom DNS resolver

```bash
gobuster dns -d example.com -w wordlist.txt -r 8.8.8.8
```

## Increase threads

```bash
gobuster dns -d example.com -w wordlist.txt -t 50
```

## Show wildcard results

By default, Gobuster hides wildcard results. Use `-i` to show them:

```bash
gobuster dns -d example.com -w wordlist.txt -i
```

## Save results

```bash
gobuster dns -d example.com -w wordlist.txt -o dns-results.txt
```

## Scan with specific domain and resolver

```bash
gobuster dns -d target.com -w subdomains.txt -r 1.1.1.1 -t 100
```

---

# 4. VHost Mode (vhost)

Enumerates virtual hosts by fuzzing the `Host` header.

## Basic VHost scan

```bash
gobuster vhost -u https://example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
```

## Use IP as base target

Useful when the server uses IP to capture all traffic:

```bash
gobuster vhost -u http://10.10.10.5 -w vhosts.txt
```

## Add custom User-Agent

```bash
gobuster vhost -u https://example.com -w wordlist.txt --useragent "PENTEST"
```

## Use specific domain

```bash
gobuster vhost -u https://example.com -w wordlist.txt --domain example.com
```

## Skip TLS verification

```bash
gobuster vhost -u https://example.com -w wordlist.txt -k
```

---

# 5. Fuzz Mode (fuzz)

Replaces the `FUZZ` keyword anywhere in the URL for flexible fuzzing.

## Parameter fuzzing

Replaces `FUZZ` with each wordlist entry in the parameter value:

```bash
gobuster fuzz -u "https://example.com/page.php?id=FUZZ" -w wordlist.txt
```

## Parameter name fuzzing

```bash
gobuster fuzz -u "https://example.com/page.php?FUZZ=value" -w wordlist.txt
```

## Path fuzzing

```bash
gobuster fuzz -u "https://example.com/FUZZ/admin" -w wordlist.txt
```

## Host header fuzzing

```bash
gobuster fuzz -u "https://example.com/" -w wordlist.txt -H "Host: FUZZ.example.com"
```

## Multiple FUZZ positions

```bash
gobuster fuzz -u "https://FUZZ.example.com/api/FUZZ" -w wordlist.txt
```

## Fuzzing with custom HTTP method

```bash
gobuster fuzz -u "https://example.com/api" -w wordlist.txt -m POST
```

---

# 6. S3 Mode (s3)

Enumerates Amazon S3 buckets.

## Basic S3 bucket scan

```bash
gobuster s3 -w bucket-names.txt
```

## Increase threads

```bash
gobuster s3 -w bucket-names.txt -t 50
```

---

# 7. GCS Mode (gcs)

Enumerates Google Cloud Storage buckets.

## Basic GCS bucket scan

```bash
gobuster gcs -w bucket-names.txt
```

---

# 8. Global Options

These options work in all modes.

## Number of threads

```bash
-t 50
```

## Wordlist

```bash
-w /path/to/wordlist.txt
```

## Output file

```bash
-o results.txt
```

## Quiet mode

```bash
-q
```

## No colors

```bash
--no-color
```

## No progress bar

```bash
-z
```

## Do not show errors

```bash
--no-error
```

## Verbose (show errors)

```bash
-v
```

## HTTP proxy

```bash
-p http://127.0.0.1:8080
```

## Skip TLS verification

```bash
-k
```

## Add custom header

```bash
-H "Header-Name: value"
```

## Cookies

```bash
-c "cookie1=value1;cookie2=value2"
```

## Delay between requests

```bash
--delay 1500ms
```

---

# 9. Practical Workflows

## Basic web enumeration workflow

### Step 1: Initial directory scan

```bash
gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt -t 30
```

### Step 2: Scan with common extensions

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js,txt,bak,zip,conf -t 40
```

### Step 3: Filter by relevant status codes

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html -s 200,301,302,401,403 -t 50
```

### Step 4: Save results

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js -o dir-results.txt -t 50
```

---

## Subdomain discovery workflow

### Step 1: Basic DNS scan

```bash
gobuster dns -d example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 50
```

### Step 2: Use custom resolver

```bash
gobuster dns -d example.com -w subdomains.txt -r 8.8.8.8 -o dns-results.txt
```

### Step 3: Enumerate VHosts

```bash
gobuster vhost -u https://example.com -w subdomains.txt -t 50
```

---

## Parameter fuzzing workflow

### Numeric ID fuzzing

```bash
gobuster fuzz -u "https://example.com/user.php?id=FUZZ" -w /usr/share/seclists/Fuzzing/numbers.txt
```

### Parameter name fuzzing

```bash
gobuster fuzz -u "https://example.com/login.php" -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -H "FUZZ: test"
```

---

## Full authorized scan

A detailed scan that saves results:

```bash
gobuster dir -u https://example.com \
  -w /usr/share/seclists/Discovery/Web-Content/common.txt \
  -x php,html,txt,js,bak,zip,conf \
  -t 50 \
  -s 200,204,301,302,307,401,403 \
  -o gobuster-full.txt \
  -k
```

This command performs:

- Directory and file scanning.
- Testing with common extensions.
- 50 concurrent threads.
- Filtering by relevant status codes.
- Saving results to file.
- Skipping TLS verification.

Use this only when the scope authorizes the associated traffic.

---

# 10. Reference Tables

## Gobuster Modes

| Mode | Full name | Description |
|---|---|---|
| `dir` | Directory/file | Brute-forces directories and files on web servers. Most commonly used. |
| `dns` | DNS subdomain | Discovers subdomains through brute-force of DNS entries. |
| `vhost` | Virtual Host | Enumerates virtual hosts by fuzzing the Host header. |
| `fuzz` | General fuzzing | Replaces the `FUZZ` keyword anywhere in the URL. |
| `s3` | AWS S3 bucket | Enumerates Amazon S3 buckets. |
| `gcs` | Google Cloud Storage | Enumerates Google Cloud Storage buckets. |

## Common Gobuster Options

| Option | Description |
|---|---|
| `-u` | Target URL (dir, vhost, fuzz modes) |
| `-d` | Target domain (dns mode) |
| `-w` | Path to wordlist |
| `-x` | Extensions to append (dir mode) |
| `-s` | Show only these status codes |
| `-b` | Exclude these status codes |
| `-t` | Number of concurrent threads (default: 10) |
| `-o` | Output file |
| `-p` | HTTP proxy |
| `-H` | Custom header |
| `-c` | Cookies |
| `-k` | Skip TLS verification |
| `-q` | Quiet mode (no banner) |
| `-v` | Verbose output |
| `-z` | No progress bar |
| `--delay` | Delay between requests |
| `--exclude-length` | Exclude responses with this length |
| `-r` | Custom DNS resolver (dns mode) |
| `-i` | Show wildcard results (dns mode) |
| `--useragent` | Custom User-Agent (vhost mode) |
| `-m` | Custom HTTP method (fuzz mode) |

---

# 11. Recommended Wordlists

## Directories and files

```bash
/usr/share/wordlists/dirb/common.txt
/usr/share/seclists/Discovery/Web-Content/common.txt
/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

## Subdomains

```bash
/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
/usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
/usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
```

## Parameter fuzzing

```bash
/usr/share/seclists/Fuzzing/fuzz-Bo0oM.txt
/usr/share/seclists/Fuzzing/numbers.txt
/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt
```

## S3/GCS Buckets

```bash
/usr/share/seclists/Discovery/Cloud-Storage/bucket-names.txt
```

---

# 12. Important Reminders

- A found directory is not automatically a vulnerability.
- Results may include false positives. Validate manually.
- Aggressive fuzzing can generate significant traffic.
- Scans may trigger firewall, WAF, IDS, IPS, or SIEM alerts.
- Use `-k` only in test environments; in production, validate certificates.
- Adjust threads (`-t`) according to target capacity.
- Use `--delay` to reduce load on sensitive systems.
- Always save original output as evidence.
- Validate interesting results with browser, proxy, or HTTP client.
- Never scan systems outside the authorized scope.
- Gobuster is ideal for a quick first sweep; use tools like ffuf for more advanced fuzzing.

---

# 13. Quick Command Examples

| Task | Command |
|---|---|
| Basic directory scan | `gobuster dir -u https://target.com -w wordlist.txt` |
| Scan with extensions | `gobuster dir -u https://target.com -w wordlist.txt -x php,html,js` |
| Show except 404 | `gobuster dir -u https://target.com -w wordlist.txt -b 404` |
| DNS subdomain scan | `gobuster dns -d target.com -w subdomains.txt` |
| VHost scan | `gobuster vhost -u https://target.com -w vhosts.txt` |
| Parameter fuzzing | `gobuster fuzz -u "https://target.com?id=FUZZ" -w wordlist.txt` |
| Save results | `gobuster dir -u https://target.com -w wordlist.txt -o results.txt` |
| Use proxy | `gobuster dir -u https://target.com -w wordlist.txt -p http://127.0.0.1:8080` |
| 50 threads | `gobuster dir -u https://target.com -w wordlist.txt -t 50` |
| 1.5s delay | `gobuster dir -u https://target.com -w wordlist.txt --delay 1500ms` |

---

## Recommended Workflow

```text
1. Confirm authorization and scope.
2. Perform initial directory scan (dir mode).
3. Scan with common extensions.
4. Enumerate subdomains (dns mode).
5. Enumerate VHosts (vhost mode).
6. Perform parameter fuzzing (fuzz mode).
7. Save all results.
8. Manually validate interesting findings.
9. Eliminate false positives.
10. Document only confirmed findings.
```
