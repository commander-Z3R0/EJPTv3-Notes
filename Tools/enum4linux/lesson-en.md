# Enum4Linux Cheat Sheet

## Overview

Enum4Linux is a tool for enumerating information from Windows and Samba systems. It's used for:

- Enumerating users and groups.
- Listing shares and permissions.
- Gathering system information.
- Identifying security misconfigurations.
- Conducting authorized security assessments.

Enum4Linux provides:

- Automated enumeration of SMB/CIFS services.
- User and group enumeration.
- Share enumeration and access testing.
- Password policy extraction.
- Integration with other pentesting tools.

```text
Use Enum4Linux only against systems you own or are explicitly authorized to test.
```

---

# 1. Starting Enum4Linux

## Basic syntax

```bash
enum4linux [options] <target>
```

## Show help

```bash
enum4linux -h
```

## Show version

```bash
enum4linux -V
```

## Update Enum4Linux

```bash
git clone https://github.com/CiscoCXSecurity/enum4linux.git
cd enum4linux
```

---

# 2. Basic Enumeration

## Enumerate all information

```bash
enum4linux <target>
```

Example:

```bash
enum4linux 192.168.1.10
```

## Enumerate with verbose output

```bash
enum4linux -v <target>
```

Example:

```bash
enum4linux -v 192.168.1.10
```

## Enumerate with machine-readable output

```bash
enum4linux -M <target>
```

Outputs in a format suitable for parsing.

## Enumerate specific target

```bash
enum4linux 192.168.1.10
```

## Enumerate with hostname

```bash
enum4linux hostname.local
```

---

# 3. User Enumeration

## Enumerate all users

```bash
enum4linux -U <target>
```

Example:

```bash
enum4linux -U 192.168.1.10
```

## Enumerate users with RID cycling

```bash
enum4linux -U -r <target>
```

RID cycling attempts to enumerate users by iterating through RIDs.

## Enumerate users with specific RID range

```bash
enum4linux -U -r <start-end> <target>
```

Example:

```bash
enum4linux -U -r 500-550 192.168.1.10
```

## List users with descriptions

```bash
enum4linux -U <target>
```

Includes user descriptions when available.

## Enumerate users with authentication

```bash
enum4linux -U -u <username> -p <password> <target>
```

Example:

```bash
enum4linux -U -u guest -p '' 192.168.1.10
```

---

# 4. Group Enumeration

## Enumerate all groups

```bash
enum4linux -G <target>
```

Example:

```bash
enum4linux -G 192.168.1.10
```

## Enumerate groups with members

```bash
enum4linux -G <target>
```

Includes group membership information.

## Enumerate specific group

```bash
enum4linux -G -g <groupname> <target>
```

Example:

```bash
enum4linux -G -g "Domain Admins" 192.168.1.10
```

## List group SIDs

```bash
enum4linux -G <target>
```

Shows Security Identifiers for groups.

---

# 5. Share Enumeration

## Enumerate all shares

```bash
enum4linux -S <target>
```

Example:

```bash
enum4linux -S 192.168.1.10
```

## Enumerate shares with permissions

```bash
enum4linux -S <target>
```

Shows share permissions and access rights.

## List accessible shares

```bash
enum4linux -S <target>
```

Identifies shares you can access.

## Enumerate shares with authentication

```bash
enum4linux -S -u <username> -p <password> <target>
```

Example:

```bash
enum4linux -S -u guest -p '' 192.168.1.10
```

## Check for null session shares

```bash
enum4linux -N <target>
```

Tests for shares accessible with null sessions.

---

# 6. System Information

## Get OS information

```bash
enum4linux -o <target>
```

Example:

```bash
enum4linux -o 192.168.1.10
```

## Get server information

```bash
enum4linux -S <target>
```

Includes server version and domain information.

## Get domain information

```bash
enum4linux -D <target>
```

Example:

```bash
enum4linux -D 192.168.1.10
```

## Get workstation information

```bash
enum4linux -W <target>
```

## Get password policy

```bash
enum4linux -P <target>
```

Example:

```bash
enum4linux -P 192.168.1.10
```

Shows password complexity, length, and expiration settings.

---

# 7. Advanced Enumeration

## Enumerate everything

```bash
enum4linux -a <target>
```

Example:

```bash
enum4linux -a 192.168.1.10
```

Performs all enumeration checks.

## Enumerate with RID cycling

```bash
enum4linux -r <target>
```

Example:

```bash
enum4linux -r 192.168.1.10
```

Attempts to enumerate users via RID cycling.

## Enumerate with specific RID range

```bash
enum4linux -r <start-end> <target>
```

Example:

```bash
enum4linux -r 1000-1100 192.168.1.10
```

## Enumerate printers

```bash
enum4linux -p <target>
```

Example:

```bash
enum4linux -p 192.168.1.10
```

## Enumerate shares and users

```bash
enum4linux -S -U <target>
```

Combines share and user enumeration.

---

# 8. Authentication Options

## Authenticate with username and password

```bash
enum4linux -u <username> -p <password> <target>
```

Example:

```bash
enum4linux -u admin -p password123 192.168.1.10
```

## Authenticate with username only

```bash
enum4linux -u <username> -p '' <target>
```

Example:

```bash
enum4linux -u guest -p '' 192.168.1.10
```

## Use null session

```bash
enum4linux -N <target>
```

Attempts to connect with null session.

## Use domain authentication

```bash
enum4linux -u <username> -p <password> -d <domain> <target>
```

Example:

```bash
enum4linux -u user -p pass -d DOMAIN 192.168.1.10
```

## Use hash authentication

```bash
enum4linux -u <username> -H <hash> <target>
```

Example:

```bash
enum4linux -u admin -H aad3b435b51404eeaad3b435b51404ee:hash 192.168.1.10
```

---

# 9. Output Options

## Save output to file

```bash
enum4linux <target> > output.txt
```

Example:

```bash
enum4linux -a 192.168.1.10 > enum_results.txt
```

## Verbose output

```bash
enum4linux -v <target>
```

Shows detailed enumeration progress.

## Machine-readable output

```bash
enum4linux -M <target>
```

Format suitable for parsing by scripts.

## Quiet mode

```bash
enum4linux -q <target>
```

Minimizes output verbosity.

## Debug mode

```bash
enum4linux -d <target>
```

Shows debug information for troubleshooting.

---

# 10. Common Attack Scenarios

## Basic enumeration

```bash
enum4linux 192.168.1.10
```

## Full enumeration

```bash
enum4linux -a 192.168.1.10
```

## User enumeration only

```bash
enum4linux -U 192.168.1.10
```

## Share enumeration only

```bash
enum4linux -S 192.168.1.10
```

## Group enumeration only

```bash
enum4linux -G 192.168.1.10
```

## RID cycling

```bash
enum4linux -r 192.168.1.10
```

## Password policy check

```bash
enum4linux -P 192.168.1.10
```

## Null session test

```bash
enum4linux -N 192.168.1.10
```

## Authenticated enumeration

```bash
enum4linux -u guest -p '' 192.168.1.10
```

## Verbose full scan

```bash
enum4linux -a -v 192.168.1.10
```

---

# 11. Practical Workflows

## Basic SMB enumeration workflow

```text
1. Identify Windows/Samba target.
2. Run enum4linux basic scan.
3. Enumerate users.
4. Enumerate shares.
5. Check password policy.
6. Document findings.
```

## Example: Full enumeration

```bash
# Basic scan
enum4linux 192.168.1.10

# Full enumeration
enum4linux -a 192.168.1.10

# Verbose output
enum4linux -a -v 192.168.1.10

# Save results
enum4linux -a 192.168.1.10 > results.txt
```

## Example: User enumeration

```bash
# Enumerate users
enum4linux -U 192.168.1.10

# RID cycling
enum4linux -U -r 192.168.1.10

# Specific RID range
enum4linux -U -r 500-550 192.168.1.10
```

## Example: Share enumeration

```bash
# Enumerate shares
enum4linux -S 192.168.1.10

# Check null session shares
enum4linux -N 192.168.1.10

# Authenticated share enum
enum4linux -S -u guest -p '' 192.168.1.10
```

## Example: Password policy

```bash
# Get password policy
enum4linux -P 192.168.1.10

# Full scan with policy
enum4linux -a -P 192.168.1.10
```

## Example: RID cycling attack

```bash
# Basic RID cycling
enum4linux -r 192.168.1.10

# Specific range
enum4linux -r 1000-1100 192.168.1.10

# With user enum
enum4linux -U -r 192.168.1.10
```

---

# 12. Common Commands Reference

| Command | Description |
|---|---|
| `enum4linux -h` | Show help |
| `enum4linux -V` | Show version |
| `enum4linux <target>` | Basic enumeration |
| `enum4linux -a <target>` | Enumerate all |
| `enum4linux -U <target>` | Enumerate users |
| `enum4linux -G <target>` | Enumerate groups |
| `enum4linux -S <target>` | Enumerate shares |
| `enum4linux -P <target>` | Get password policy |
| `enum4linux -o <target>` | Get OS information |
| `enum4linux -r <target>` | RID cycling |
| `enum4linux -N <target>` | Null session test |
| `enum4linux -v <target>` | Verbose output |
| `enum4linux -M <target>` | Machine-readable output |
| `enum4linux -u <user> -p <pass>` | Authenticate |
| `enum4linux -d <domain>` | Specify domain |
| `enum4linux -H <hash>` | Use hash authentication |
| `enum4linux -p <target>` | Enumerate printers |
| `enum4linux -W <target>` | Get workstation info |
| `enum4linux -D <target>` | Get domain info |
| `enum4linux -q <target>` | Quiet mode |

---

# 13. Integration with Other Tools

## Use with Nmap

```bash
# Nmap SMB scan
nmap -p 445 --script smb-enum-users 192.168.1.10

# Then enum4linux
enum4linux -a 192.168.1.10
```

## Use with Metasploit

```bash
# Metasploit SMB enumeration
use auxiliary/scanner/smb/smb_enumusers
set RHOSTS 192.168.1.10
run

# Then enum4linux
enum4linux -U 192.168.1.10
```

## Use with CrackMapExec

```bash
# CrackMapExec SMB enum
crackmapexec smb 192.168.1.10

# Then enum4linux
enum4linux -a 192.168.1.10
```

## Use with SMBClient

```bash
# List shares with smbclient
smbclient -L //192.168.1.10

# Then enum4linux
enum4linux -S 192.168.1.10
```

---

# 14. Troubleshooting

## Connection refused

- Check if SMB service is running.
- Verify port 445 or 139 is open.
- Ensure target is reachable.
- Check firewall rules.

## Access denied

- Try different credentials.
- Use null session if allowed.
- Check user permissions.
- Verify authentication method.

## No users found

- RID cycling may be blocked.
- Try different RID range.
- Use authenticated enumeration.
- Check if user enumeration is allowed.

## Slow enumeration

- Reduce RID range.
- Use specific enumeration options.
- Check network latency.
- Verify target responsiveness.

## Timeout errors

- Increase timeout settings.
- Check network connectivity.
- Verify target is online.
- Reduce enumeration scope.

---

# 15. Security Best Practices

## Always verify findings

- Cross-reference with other tools.
- Verify user accounts manually.
- Check share access manually.
- Document all findings.

## Respect legal boundaries

- Only test systems you own.
- Obtain explicit authorization.
- Follow responsible disclosure.
- Document all activities.

## Minimize impact

- Use appropriate enumeration options.
- Avoid aggressive RID cycling.
- Test during maintenance windows.
- Monitor target system.

## Keep tools updated

- Update enum4linux regularly.
- Stay informed about new techniques.
- Follow security advisories.
- Test in controlled environments.

## Document everything

- Record enumeration results.
- Note accessible shares.
- Track user accounts.
- Document security issues.

---

# 16. Important Reminders

- Always obtain explicit authorization before using Enum4Linux.
- Test in a controlled lab environment first.
- Not all enumerated information indicates vulnerabilities.
- Some enumeration may trigger security alerts.
- Keep Enum4Linux updated regularly.
- Validate findings manually; do not rely solely on automated results.
- Document all actions, commands, and results.
- Preserve original evidence and logs.
- Respect the scope and rules of engagement.
- Understand the legal and ethical implications of your actions.

---

# 17. Quick Reference Examples

## Basic scan

```bash
enum4linux 192.168.1.10
```

## Full enumeration

```bash
enum4linux -a 192.168.1.10
```

## User enumeration

```bash
enum4linux -U 192.168.1.10
```

## Share enumeration

```bash
enum4linux -S 192.168.1.10
```

## Group enumeration

```bash
enum4linux -G 192.168.1.10
```

## RID cycling

```bash
enum4linux -r 192.168.1.10
```

## Password policy

```bash
enum4linux -P 192.168.1.10
```

## Null session

```bash
enum4linux -N 192.168.1.10
```

## Authenticated

```bash
enum4linux -u guest -p '' 192.168.1.10
```

## Verbose

```bash
enum4linux -a -v 192.168.1.10
```

## Save output

```bash
enum4linux -a 192.168.1.10 > results.txt
```

---

# 18. Additional Resources

## Enum4Linux GitHub

```text
https://github.com/CiscoCXSecurity/enum4linux
```

## SMB Protocol Documentation

```text
https://docs.microsoft.com/en-us/openspecs/
```

## Samba Documentation

```text
https://www.samba.org/samba/docs/
```

## OWASP SMB Security

```text
https://owasp.org/www-project-web-security-testing-guide/
```

## Windows Security Baseline

```text
https://docs.microsoft.com/en-us/windows/security/
```
