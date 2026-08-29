# Searchsploit Cheat Sheet

## Overview

Searchsploit is a command-line search tool for Exploit-DB, used for:

- Searching for exploits, shellcodes, and papers.
- Finding vulnerabilities by CVE, EDB-ID, or keyword.
- Copying exploits to the current directory.
- Displaying detailed information about exploits.
- Conducting authorized security assessments.

Searchsploit provides:

- Offline access to the Exploit-DB repository.
- Fast search capabilities.
- Integration with Metasploit.
- Support for multiple search filters.
- Ability to copy and view exploit source code.

```text
Use Searchsploit only for research and against systems you own or are explicitly authorized to test.
```

---

# 1. Starting Searchsploit

## Basic syntax

```bash
searchsploit [options] <search-term>
```

## Show help

```bash
searchsploit -h
```

## Show version

```bash
searchsploit -V
```

## Show verbose version

```bash
searchsploit -v
```

## Update the database

```bash
searchsploit -u
```

Updates the local Exploit-DB repository.

---

# 2. Basic Search

## Search by keyword

```bash
searchsploit <keyword>
```

Example:

```bash
searchsploit apache
```

## Search by CVE

```bash
searchsploit <CVE>
```

Example:

```bash
searchsploit CVE-2017-0144
```

## Search by EDB-ID

```bash
searchsploit <EDB-ID>
```

Example:

```bash
searchsploit 41937
```

## Search by author

```bash
searchsploit --author <author-name>
```

Example:

```bash
searchsploit --author metasploit
```

## Search by type

Types include:

- `exploit`
- `shellcode`
- `papers`
- `webapps`
- `platform`
- `local`
- `remote`

```bash
searchsploit --type <type>
```

Example:

```bash
searchsploit --type exploit apache
```

## Search by platform

```bash
searchsploit --platform <platform>
```

Example:

```bash
searchsploit --platform linux
```

## Search by port

```bash
searchsploit --port <port>
```

Example:

```bash
searchsploit --port 445
```

## Combine multiple filters

```bash
searchsploit --type exploit --platform windows --port 445 smb
```

---

# 3. Advanced Search Options

## Case-insensitive search

```bash
searchsploit -i <keyword>
```

Example:

```bash
searchsploit -i apache
```

## Exact match search

```bash
searchsploit -e <keyword>
```

Example:

```bash
searchsploit -e "apache 2.4.49"
```

## Show only the title

```bash
searchsploit -t <keyword>
```

Example:

```bash
searchsploit -t apache
```

## Show only the path

```bash
searchsploit -p <keyword>
```

Example:

```bash
searchsploit -p apache
```

## Show only the EDB-ID

```bash
searchsploit -n <keyword>
```

Example:

```bash
searchsploit -n apache
```

## Search in description

```bash
searchsploit --search <keyword>
```

Example:

```bash
searchsploit --search "remote code execution"
```

## List all available platforms

```bash
searchsploit --platforms
```

## List all available types

```bash
searchsploit --types
```

---

# 4. Viewing Exploit Details

## Show detailed information

```bash
searchsploit -x <EDB-ID>
```

Example:

```bash
searchsploit -x 41937
```

## Show detailed information by CVE

```bash
searchsploit -x <CVE>
```

Example:

```bash
searchsploit -x CVE-2017-0144
```

## View exploit source code

```bash
searchsploit -x <EDB-ID>
```

This displays the full exploit source code.

## Show only the path to the exploit

```bash
searchsploit -p <keyword>
```

Example:

```bash
searchsploit -p eternalblue
```

---

# 5. Copying Exploits

## Copy exploit to current directory

```bash
searchsploit -m <EDB-ID>
```

Example:

```bash
searchsploit -m 41937
```

## Copy exploit to a specific directory

```bash
searchsploit -m <EDB-ID> -o /path/to/output
```

Example:

```bash
searchsploit -m 41937 -o /tmp/exploits
```

## Copy multiple exploits

```bash
searchsploit -m <EDB-ID-1> <EDB-ID-2> <EDB-ID-3>
```

Example:

```bash
searchsploit -m 41937 42000 42050
```

## Copy all exploits from a search

```bash
searchsploit -m <search-term>
```

Example:

```bash
searchsploit -m eternalblue
```

Copies all matching exploits to the current directory.

---

# 6. Metasploit Integration

## Search for Metasploit modules

```bash
searchsploit --nmap <nmap-output.xml>
```

Parses Nmap output and suggests Metasploit modules.

## Search by Metasploit module name

```bash
searchsploit <module-name>
```

Example:

```bash
searchsploit ms17_010_eternalblue
```

## Show Metasploit module path

```bash
searchsploit -p <module-name>
```

Example:

```bash
searchsploit -p ms17_010_eternalblue
```

## Check if exploit is available in Metasploit

Searchsploit indicates if an exploit is available in Metasploit in the detailed view.

---

# 7. Common Search Scenarios

## Search for Apache exploits

```bash
searchsploit apache
```

## Search for Linux kernel exploits

```bash
searchsploit linux kernel
```

## Search for Windows SMB exploits

```bash
searchsploit windows smb
```

## Search for EternalBlue

```bash
searchsploit eternalblue
```

## Search by CVE-2017-0144

```bash
searchsploit CVE-2017-0144
```

## Search for shellcode

```bash
searchsploit --type shellcode
```

## Search for web application exploits

```bash
searchsploit --type webapps
```

## Search for local privilege escalation

```bash
searchsploit --type local
```

## Search for remote exploits

```bash
searchsploit --type remote
```

## Search for exploits on port 445

```bash
searchsploit --port 445
```

## Search for WordPress exploits

```bash
searchsploit wordpress
```

## Search for SSH exploits

```bash
searchsploit ssh
```

## Search for FTP exploits

```bash
searchsploit ftp
```

## Search for MySQL exploits

```bash
searchsploit mysql
```

---

# 8. Practical Workflows

## Basic vulnerability research workflow

```text
1. Identify the target service or software.
2. Search for exploits using Searchsploit.
3. Review exploit details and requirements.
4. Copy the exploit to your working directory.
5. Analyze and modify the exploit if needed.
6. Test in a controlled lab environment.
7. Document findings.
```

## Example: Research Apache vulnerability

```bash
# Search for Apache exploits
searchsploit apache

# View detailed information
searchsploit -x 41937

# Copy exploit to current directory
searchsploit -m 41937
```

## Example: CVE-based research

```bash
# Search by CVE
searchsploit CVE-2017-0144

# View details
searchsploit -x CVE-2017-0144

# Copy exploit
searchsploit -m 41937
```

## Example: Platform-specific search

```bash
# Search for Linux exploits
searchsploit --platform linux

# Search for Windows exploits
searchsploit --platform windows
```

## Example: Type-specific search

```bash
# Search for shellcode
searchsploit --type shellcode

# Search for webapps
searchsploit --type webapps
```

## Example: Nmap integration

```bash
# Parse Nmap output
searchsploit --nmap scan.xml
```

This suggests relevant exploits based on discovered services.

---

# 9. Common Commands Reference

| Command | Description |
|---|---|
| `searchsploit -h` | Show help |
| `searchsploit -V` | Show version |
| `searchsploit -v` | Show verbose version |
| `searchsploit -u` | Update database |
| `searchsploit <keyword>` | Search by keyword |
| `searchsploit -i <keyword>` | Case-insensitive search |
| `searchsploit -e <keyword>` | Exact match search |
| `searchsploit -t <keyword>` | Show only titles |
| `searchsploit -p <keyword>` | Show only paths |
| `searchsploit -n <keyword>` | Show only EDB-IDs |
| `searchsploit -x <EDB-ID>` | Show detailed information |
| `searchsploit -m <EDB-ID>` | Copy exploit to current directory |
| `searchsploit --author <name>` | Search by author |
| `searchsploit --type <type>` | Search by type |
| `searchsploit --platform <platform>` | Search by platform |
| `searchsploit --port <port>` | Search by port |
| `searchsploit --nmap <file>` | Parse Nmap output |
| `searchsploit --platforms` | List all platforms |
| `searchsploit --types` | List all types |
| `searchsploit --search <term>` | Search in description |

---

# 10. Advanced Usage

## Search with multiple filters

```bash
searchsploit --type exploit --platform linux --port 22 ssh
```

## Search for papers and documentation

```bash
searchsploit --type papers
```

## Search for local exploits only

```bash
searchsploit --type local
```

## Search for remote exploits only

```bash
searchsploit --type remote
```

## Search for specific software version

```bash
searchsploit "apache 2.4.49"
```

## Search for exploits by year

Include year in search term:

```bash
searchsploit "2021"
```

## Search for 0-day research

```bash
searchsploit --type exploit --platform linux
```

Review recent exploits for potential 0-day patterns.

## Combine search with grep

```bash
searchsploit apache | grep "2.4"
```

## Export search results

```bash
searchsploit apache > results.txt
```

---

# 11. Exploit-DB Integration

## Update Exploit-DB database

```bash
searchsploit -u
```

This downloads the latest exploits from Exploit-DB.

## Check database status

```bash
searchsploit -v
```

Shows database version and last update.

## Manually update Exploit-DB

```bash
cd /usr/share/exploitdb
git pull
```

## Search Exploit-DB online

Visit:

```text
https://www.exploit-db.com/
```

For web-based search and additional features.

---

# 12. Common Exploits and Scenarios

## MS17-010 EternalBlue

```bash
searchsploit eternalblue
searchsploit -x 41937
searchsploit -m 41937
```

## Apache Struts RCE

```bash
searchsploit apache struts
searchsploit -x <EDB-ID>
searchsploit -m <EDB-ID>
```

## Linux Kernel Privilege Escalation

```bash
searchsploit linux kernel privilege escalation
searchsploit --type local --platform linux
```

## WordPress Plugin Exploits

```bash
searchsploit wordpress plugin
searchsploit --type webapps wordpress
```

## SSH Vulnerabilities

```bash
searchsploit ssh
searchsploit --port 22 ssh
```

## FTP Vulnerabilities

```bash
searchsploit ftp
searchsploit --port 21 ftp
```

## SMB Vulnerabilities

```bash
searchsploit smb
searchsploit --port 445 smb
```

## MySQL Vulnerabilities

```bash
searchsploit mysql
searchsploit --port 3306 mysql
```

## RDP Vulnerabilities

```bash
searchsploit rdp
searchsploit --port 3389 rdp
```

## Web Application Exploits

```bash
searchsploit --type webapps
searchsploit "sql injection"
searchsploit "xss"
```

---

# 13. Troubleshooting

## No results found

- Try different keywords.
- Use case-insensitive search: `-i`
- Search by CVE or EDB-ID directly.
- Update the database: `searchsploit -u`

## Exploit not working

- Check the requirements and platform.
- Verify the target version matches.
- Review the exploit source code.
- Test in a controlled lab environment.

## Database out of date

```bash
searchsploit -u
```

## Permission denied

Run with appropriate permissions:

```bash
sudo searchsploit -m <EDB-ID>
```

## Exploit path not found

Verify the exploit exists:

```bash
searchsploit -p <keyword>
```

---

# 14. Security Best Practices

## Always verify exploit code

- Review the source code before running.
- Check for malicious payloads.
- Understand what the exploit does.
- Test in an isolated environment.

## Respect legal boundaries

- Only use exploits on systems you own.
- Obtain explicit authorization for testing.
- Follow responsible disclosure practices.
- Document all research activities.

## Maintain a lab environment

- Use virtual machines for testing.
- Isolate test networks.
- Keep snapshots of clean states.
- Document configurations and results.

## Keep tools updated

- Regularly update Searchsploit.
- Update Exploit-DB database.
- Stay informed about new vulnerabilities.
- Follow security news and advisories.

---

# 15. Important Reminders

- Always obtain explicit authorization before using exploits.
- Test exploits in a controlled lab environment first.
- Not all exploits are reliable; verify before use.
- Some exploits may crash services or systems.
- Keep Searchsploit and Exploit-DB updated regularly.
- Validate findings manually; do not rely solely on automated results.
- Document all actions, commands, and results.
- Preserve original evidence and logs.
- Respect the scope and rules of engagement.
- Understand the legal and ethical implications of your actions.

---

# 16. Quick Reference Examples

## Basic search

```bash
searchsploit apache
```

## Search by CVE

```bash
searchsploit CVE-2017-0144
```

## View exploit details

```bash
searchsploit -x 41937
```

## Copy exploit

```bash
searchsploit -m 41937
```

## Search for Linux exploits

```bash
searchsploit --platform linux
```

## Search for shellcode

```bash
searchsploit --type shellcode
```

## Update database

```bash
searchsploit -u
```

## Case-insensitive search

```bash
searchsploit -i apache
```

## Exact match search

```bash
searchsploit -e "apache 2.4.49"
```

## Search by author

```bash
searchsploit --author metasploit
```

## Parse Nmap output

```bash
searchsploit --nmap scan.xml
```

## Search with multiple filters

```bash
searchsploit --type exploit --platform windows --port 445 smb
```

---

# 17. Supported Search Filters

## By type

- `exploit`
- `shellcode`
- `papers`
- `webapps`
- `platform`
- `local`
- `remote`

## By platform

- `linux`
- `windows`
- `macos`
- `bsd`
- `android`
- `ios`
- `hardware`
- `php`
- `python`
- `ruby`
- `java`
- And many more...

## By port

Any valid port number:

- `21` (FTP)
- `22` (SSH)
- `80` (HTTP)
- `443` (HTTPS)
- `445` (SMB)
- `3306` (MySQL)
- `3389` (RDP)
- And many more...

## By CVE

Any valid CVE identifier:

- `CVE-2017-0144`
- `CVE-2021-44228`
- `CVE-2020-1472`

## By EDB-ID

Any valid Exploit-DB ID:

- `41937`
- `42000`
- `50000`

---

# 18. Additional Resources

## Exploit-DB

```text
https://www.exploit-db.com/
```

## Offensive Security

```text
https://www.offensive-security.com/
```

## Metasploit Framework

```text
https://www.metasploit.com/
```

## CVE Database

```text
https://cve.mitre.org/
```

## National Vulnerability Database

```text
https://nvd.nist.gov/
```
