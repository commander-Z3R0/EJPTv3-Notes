# File & Directory Enumeration

## Overview

File and directory brute-forcing (often called content discovery or directory enumeration) is a technique used to discover hidden or unlinked files, directories, and endpoints on a web server.

Web applications often expose only a subset of their actual structure through visible links and menus. Behind the scenes, there may be:

- Admin panels (`/admin`, `/dashboard`).
- Backup files (`.bak`, `.old`, `.zip`).
- Development or test endpoints (`/test`, `/dev`, `/staging`).
- API routes not referenced in the frontend.
- Configuration or debug files.

Brute-forcing works by sending large numbers of HTTP requests for common filenames and directory names (from predefined wordlists) and analyzing the server's responses (status codes, response size, redirects, etc.) to infer what exists.

---

## Why It Is Performed

File and directory brute-forcing is performed because what you cannot see can still be exploitable. Its main goals are:

- **Expand the attack surface**: discover functionality that was not intended to be public.
- **Identify sensitive entry points**: admin portals, internal tools, upload directories, or APIs.
- **Find misconfigurations**: exposed backups, source files, or forgotten development artifacts.
- **Enable further attacks**: discovered endpoints may lead to:
  - Authentication bypass.
  - File upload vulnerabilities.
  - Information disclosure.
  - SQL injection, XSS, or logic flaws.

In real-world pentests, directory enumeration frequently acts as a pivot — one discovered endpoint often unlocks multiple exploitation paths.

---

# Gobuster

**Gobuster** is a fast, command-line enumeration tool written in Go that is commonly used during the reconnaissance and enumeration phase of web application penetration tests.

It performs brute-force-style discovery using wordlists and is designed to be efficient, simple, and scriptable.

Gobuster is popular because it is:

- **Fast** (Go-based, highly performant).
- **Reliable** (simple logic, minimal false positives).
- **Flexible** (works with many wordlists and configurations).
- **Well-suited for automation** in pentesting workflows.

In practice, Gobuster is often one of the first active enumeration tools run against a web target after passive recon, helping testers quickly identify areas worth deeper manual testing.

---

## Gobuster Modes

Gobuster supports multiple enumeration modes. The most relevant for web application pentesting are:

| Mode | Description |
|---|---|
| `dir` | Directory and file enumeration on a web server |
| `vhost` | Virtual host enumeration on a target domain |
| `dns` | DNS subdomain enumeration |
| `fuzz` | Fuzzing mode for custom patterns |
| `gcs` | Google Cloud Storage bucket enumeration |
| `s3` | AWS S3 bucket enumeration |

### Core Functionality (dir Mode)

- Brute-forces directories and files on a web server.
- Supports file extensions (e.g. `.php`, `.js`, `.txt`).
- Filters results based on HTTP status codes.
- Helps uncover hidden routes and functionality.

### Virtual Host Enumeration (vhost Mode)

- Discovers hidden virtual hosts on a target domain.
- Useful when applications behave differently per hostname.
- Changes the `Host` header to test for virtual hosts configured on the target.

### DNS Subdomain Enumeration (dns Mode)

- Brute-forces subdomains using DNS queries.
- Helpful for mapping the full application ecosystem.
- Ideal as a complement to vhost mode.

---

# Installation

Gobuster is commonly preinstalled on Kali Linux.

## Check the Installed Version

```bash
gobuster version  # displays the installed Gobuster version.
```

## Display Help

```bash
gobuster --help  # displays available Gobuster options and modes.
gobuster dir --help  # displays options specific to the dir mode.
```

## Install Gobuster on Debian-Based Systems

```bash
sudo apt update  # updates the local package list.
sudo apt install gobuster -y  # installs Gobuster.
```

Alternatively, Gobuster can be installed from its GitHub releases or built from source using Go:

```bash
go install github.com/OJ/gobuster/v3@latest  # installs Gobuster using Go.
```

---

# Basic Flags And Options

| Flag | Description | Example |
|---|---|---|
| `-u` | Target URL | `-u https://example.com` |
| `-w` | Wordlist for brute-forcing | `-w /path/to/wordlist.txt` |
| `-k` | Ignore SSL/TLS certificate errors (HTTPS) | `-k` |
| `-t` | Number of threads to speed up the scan | `-t 20` |
| `-o` | Save results to a file | `-o results.txt` |
| `-x` | File extensions to search for | `-x php,html,txt` |
| `-r` | Enable recursive mode | `-r` |
| `-s` | Filter by HTTP status codes | `-s 200,204,301` |
| `-z` | Ignore response length (no progress bar per result) | `-z` |
| `-X` | Use specific HTTP methods | `-X GET,POST` |
| `-P` | URL path prefix for each request | `-P /app/` |
| `--append-domain` | In vhost mode, appends the base domain to each word (recommended) | `--append-domain` |
| `-q` | Quiet mode, suppresses banner and extra output | `-q` |
| `-e` | Expanded output, shows full URLs | `-e` |
| `-n` | No status codes in output | `-n` |
| `--no-error` | Do not display errors | `--no-error` |
| `-c` | Specify HTTP cookies for authentication | `-c "session=abc123"` |
| `-H` | Specify custom HTTP headers | `-H "Authorization: Bearer token"` |
| `-b` | Blacklist specific status codes | `-b 403,404` |
| `-l` | Show response length in output | `-l` |

---

# Directory Enumeration Examples

## Basic Directory Enumeration

Enumerate directories using a common wordlist:

```bash
gobuster dir -u https://www.example.com -w common.txt  # basic directory brute-force.
```

## Custom Wordlist And Extensions

Use a custom wordlist and specify file extensions to search for:

```bash
gobuster dir -u https://www.example.com -w custom.txt -x php,html  # searches for .php and .html files.
```

## Recursive Directory Enumeration

Enable recursive mode to explore subdirectories:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -r  # recursively enumerates discovered directories.
```

## Directory Enumeration From A Specific URL Path

Enumerate directories starting from a specific URL path:

```bash
gobuster dir -u https://www.example.com/subdir/ -w common.txt  # brute-forces paths under /subdir/.
```

## Filter By HTTP Status Codes

Specify which HTTP status codes to consider as found:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -x php,html -s 200,204  # only shows results with status 200 and 204.
```

## Use Different HTTP Methods

Use different HTTP methods during directory enumeration:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -x php -X GET,POST  # sends GET and POST requests.
```

## URL Path Prefix

Add a URL path prefix to each request:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -P /app/  # prefixes each word with /app/.
```

## Ignore Response Length

Ignore response length to quickly identify existing paths:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -z  # suppresses the response length column.
```

## Save Results To A File

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -o results.txt  # saves results to a file.
```

## Scan With Custom Cookies (Authenticated Enumeration)

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -c "session=abc123; role=admin"  # scans with authentication cookies.
```

## Scan With Custom Headers

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -H "Authorization: Bearer token123" -H "X-Custom-Header: test"  # sends custom headers with each request.
```

## Blacklist Status Codes

Exclude specific status codes from results:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -b 403,404  # hides 403 and 404 responses.
```

## Expanded Output With Full URLs

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -e -o full-urls.txt  # shows full URLs in output and saves them.
```

## Increase Threads For Faster Scanning

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -t 50  # uses 50 threads for faster enumeration.
```

## Ignore SSL/TLS Certificate Errors (HTTPS)

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -k  # ignores certificate validation errors.
```

---

# Virtual Host Enumeration (vhost Mode)

A web server may host multiple websites (virtual hosts) on the same IP address. Gobuster's vhost mode brute-forces virtual host names by changing the `Host` header of each request.

## Basic vhost Enumeration

```bash
gobuster vhost -u https://nunchucks.htb -k -w subdomains.txt --append-domain  # brute-forces virtual hosts, appending the base domain.
```

### Important Note On `--append-domain`

`--append-domain` is critical for vhost mode to work correctly. Without it, Gobuster only sends the raw word from the wordlist as the `Host` header (e.g. `admin` instead of `admin.nunchucks.htb`). With `--append-domain`, each word is combined with the base domain, producing valid virtual host names.

## vhost Enumeration With Threads And Output

```bash
gobuster vhost -u https://nunchucks.htb -k -w subdomains.txt --append-domain -t 30 -o vhost_results.txt  # scans with 30 threads and saves results.
```

vhost mode is useful when:

- Applications behave differently depending on the `Host` header.
- DNS is unavailable or does not resolve subdomains.
- You want to discover virtual hosts configured on the server but not publicly exposed.

---

# DNS Subdomain Enumeration (dns Mode)

Gobuster's dns mode brute-forces subdomains by sending DNS queries. It does not send HTTP requests; it only checks whether DNS records exist for each candidate subdomain.

## Basic DNS Enumeration

```bash
gobuster dns -d nunchucks.htb -w subdomains.txt -t 30 -o dns_result.txt  # brute-forces subdomains via DNS queries with 30 threads.
```

### When To Use dns Mode

- To map the full application ecosystem.
- As a complement to vhost mode (DNS finds resolvable subdomains; vhost finds virtual hosts configured on the server even without DNS).
- When you want to confirm whether discovered subdomains resolve to an IP address.

### Differences Between vhost And dns Mode

| Aspect | vhost Mode | dns Mode |
|---|---|---|
| What it tests | Virtual hosts by changing the `Host` header | Subdomains via DNS resolution |
| Requires DNS | No | Yes |
| Finds | Hosts configured on the server | Subdomains that resolve in DNS |
| Best used for | Finding hidden apps on a known IP | Mapping the domain's DNS footprint |

Using both modes together provides a more complete picture of the target's subdomain and virtual host landscape.

---

# Common Wordlists

Gobuster requires a wordlist to perform brute-forcing. Common wordlist locations on Kali Linux:

```text
/usr/share/wordlists/dirb/common.txt
/usr/share/wordlists/dirb/big.txt
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
/usr/share/wordlists/seclists/Discovery/Web-Content/
```

[SecLists](https://github.com/danielmiessler/SecLists) is a comprehensive collection of wordlists for security testing. It can be installed on Kali Linux:

```bash
sudo apt install seclists -y  # installs the SecLists collection.
```

Choose a wordlist appropriate for the target:

- `common.txt` for quick, broad scans.
- `directory-list-2.3-medium.txt` for more thorough enumeration.
- Custom wordlists tailored to the target's technology stack.

---

# Understanding HTTP Status Codes In Results

Gobuster reports results based on HTTP status codes returned by the server. Understanding these codes helps interpret findings:

| Status Code | Meaning | Interpretation |
|---|---|---|
| `200` | OK | The resource exists and is accessible |
| `204` | No Content | The resource exists but returns no body |
| `301` | Moved Permanently | The resource exists but has been redirected |
| `302` | Found | The resource exists but is temporarily redirected |
| `401` | Unauthorized | The resource exists but requires authentication |
| `403` | Forbidden | The resource exists but access is denied |
| `404` | Not Found | The resource does not exist |
| `500` | Internal Server Error | The resource exists but caused a server error |

Status codes `401` and `403` are particularly interesting during enumeration — they confirm that a resource exists even though access is restricted, which can guide further attacks (authentication bypass, privilege escalation, etc.).

---

# Common Practical Commands

## Quick Directory Scan With Common Wordlist

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -t 30 -o gobuster-dir.txt  # quick directory scan, 30 threads, saves output.
```

## Directory Scan With Extensions And HTTPS

```bash
gobuster dir -u https://target.local -w /usr/share/wordlists/dirb/common.txt -x php,html,txt -k -t 30 -o gobuster-ext.txt  # scans for files with extensions over HTTPS ignoring cert errors.
```

## Recursive Scan With Extensions

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -x php,bak,old -r -t 30 -o gobuster-recursive.txt  # recursively scans with backup file extensions.
```

## Authenticated Directory Scan

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -c "session=abc123" -o gobuster-auth.txt  # scans with a session cookie.
```

## vhost Enumeration

```bash
gobuster vhost -u https://target.local -k -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain -t 30 -o gobuster-vhost.txt  # brute-forces virtual hosts with appended domain.
```

## DNS Subdomain Enumeration

```bash
gobuster dns -d target.local -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 30 -o gobuster-dns.txt  # brute-forces subdomains via DNS.
```

## Scan With Status Code Filter And Blacklist

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -s 200,301,401,403 -b 404 -o gobuster-filtered.txt  # shows only interesting status codes and hides 404.
```

---

# Fuzzing Mode (fuzz)

Gobuster also includes a `fuzz` mode that allows brute-forcing custom URL patterns. This is useful when you know part of a URL structure and want to brute-force a specific segment.

## Basic Fuzzing Example

```bash
gobuster fuzz -u https://www.example.com/FUZZ?param=value -w wordlist.txt  # brute-forces the FUZZ placeholder in the URL.
```

The `FUZZ` keyword is replaced by each word in the wordlist. This mode is flexible and supports any URL pattern where you want to enumerate a variable segment.

---

# Safe Enumeration Checklist

Before running Gobuster:

- Confirm the target is in scope.
- Confirm the target URL, port, and protocol.
- Identify whether HTTPS is used (use `-k` if needed).
- Choose an appropriate wordlist for the target.
- Decide whether authentication is required (use `-c` or `-H`).
- Set a reasonable thread count to avoid overwhelming the target.
- Consider whether recursive scanning is needed.
- Inform the relevant team if scanning could affect monitoring or production systems.

After running Gobuster:

- Save the results.
- Review each discovered path and file.
- Manually verify interesting findings with a browser, `curl`, or Burp Suite.
- Check status codes `401` and `403` for authentication or access control issues.
- Investigate backup files (`.bak`, `.old`, `.zip`, `.tar.gz`).
- Test discovered admin panels and upload directories.
- Document evidence such as URLs, HTTP responses, and screenshots.

---

# Key Takeaways

- File and directory brute-forcing discovers hidden or unlinked content on a web server.
- Gobuster is a fast, Go-based enumeration tool with multiple modes.
- Use `dir` mode for directory and file enumeration.
- Use `vhost` mode for virtual host enumeration (always use `--append-domain`).
- Use `dns` mode for DNS subdomain enumeration.
- Use `fuzz` mode for custom URL pattern brute-forcing.
- Use `-u` to define the target URL.
- Use `-w` to define the wordlist.
- Use `-x` to specify file extensions.
- Use `-r` for recursive enumeration.
- Use `-s` to filter by status codes and `-b` to blacklist them.
- Use `-k` to ignore SSL/TLS certificate errors.
- Use `-t` to control the number of threads.
- Use `-o` to save results to a file.
- Use `-c` or `-H` for authenticated scans.
- Status codes `401` and `403` confirm a resource exists even if access is restricted.
- Always validate findings manually before reporting them as vulnerabilities.
- Choose wordlists appropriate to the target's technology stack.
