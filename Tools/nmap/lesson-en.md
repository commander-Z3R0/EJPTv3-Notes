# Nmap Cheat Sheet

## Overview

Nmap is a network discovery and security auditing tool used to identify:

- Live hosts.
- Open, closed, or filtered ports.
- Running services.
- Service versions.
- Operating system information.
- Firewall and filtering behavior.
- Security-related information through NSE scripts.

Only scan systems that you own or that are explicitly included in an authorized assessment.

```text
Replace <target-IP> with the authorized target address.
```

---

# 1. Basic Nmap Commands

## Basic scan

Scan the most common TCP ports on a target:

```bash
nmap <target-IP>
```

## Verbose output

Display additional information during the scan:

```bash
nmap -v <target-IP>
```

Use double verbosity for more detailed output:

```bash
nmap -vv <target-IP>
```

## Disable DNS resolution

Avoid reverse DNS lookups to make scans faster and reduce unnecessary traffic:

```bash
nmap -n <target-IP>
```

## Skip host discovery

Treat the target as online and scan it directly:

```bash
nmap -Pn <target-IP>
```

This is useful when ICMP or normal discovery probes are blocked. It may also make the scan slower because Nmap scans the target even if it is offline.

## List targets without scanning

Display the targets that Nmap would scan without sending scan probes:

```bash
nmap -sL <target-IP>
```

## Host discovery only

Discover whether the target is online without performing a port scan:

```bash
nmap -sn <target-IP>
```

This is commonly used during the initial reconnaissance phase.

## Show scan reasons

Display the reason why Nmap assigned a particular state to a port or host:

```bash
nmap --reason <target-IP>
```

## Show only open ports

Hide ports that are not reported as open:

```bash
nmap --open <target-IP>
```

---

# 2. Host Discovery and Port Scanning

## Host discovery techniques

### ICMP echo request

Use ICMP echo discovery:

```bash
sudo nmap -PE <target-IP>
```

### ICMP timestamp request

Use ICMP timestamp discovery:

```bash
sudo nmap -PP <target-IP>
```

### TCP SYN ping

Send TCP SYN probes to specific ports:

```bash
sudo nmap -PS80,443 <target-IP>
```

### TCP ACK ping

Send TCP ACK probes to specific ports:

```bash
sudo nmap -PA80,443 <target-IP>
```

### UDP ping

Send UDP probes to selected ports:

```bash
sudo nmap -PU53,161 <target-IP>
```

### Disable ARP ping

Prevent Nmap from using ARP discovery on local networks:

```bash
sudo nmap --disable-arp-ping <target-IP>
```

---

## TCP SYN scan

The TCP SYN scan is one of the most commonly used TCP scan types.

```bash
sudo nmap -sS <target-IP>
```

Characteristics:

- Sends SYN packets.
- Usually requires elevated privileges.
- Does not complete the full TCP connection in the normal case.
- Provides a fast way to identify TCP port states.

Use it only against authorized targets.

## TCP Connect scan

Use the operating system's full TCP connection method:

```bash
nmap -sT <target-IP>
```

This is useful when raw-packet privileges are unavailable.

Characteristics:

- Completes the TCP connection.
- Usually creates more visible connections in application logs.
- Does not normally require root privileges.

## UDP scan

Scan UDP ports:

```bash
sudo nmap -sU <target-IP>
```

UDP scans are usually slower than TCP scans because UDP services may not respond consistently.

## Combined TCP and UDP scan

Scan selected TCP and UDP ports together:

```bash
sudo nmap -sS -sU -p T:22,80,443,U:53,161 <target-IP>
```

The syntax separates protocols:

```text
T: = TCP ports
U: = UDP ports
```

## TCP ACK scan

Use a TCP ACK scan to help analyze firewall rules:

```bash
sudo nmap -sA <target-IP>
```

This scan is mainly useful for determining whether ports are filtered by a firewall. It is not designed to identify open ports directly.

## TCP FIN scan

Send FIN packets to TCP ports:

```bash
sudo nmap -sF <target-IP>
```

## TCP NULL scan

Send TCP packets without standard TCP flags:

```bash
sudo nmap -sN <target-IP>
```

## TCP Xmas scan

Send packets with FIN, PSH, and URG flags:

```bash
sudo nmap -sX <target-IP>
```

These scan types may behave differently depending on the target operating system and firewall. Results should always be interpreted carefully.

---

## Port selection

### Scan a single port

```bash
nmap -p 22 <target-IP>
```

### Scan multiple ports

```bash
nmap -p 22,80,443 <target-IP>
```

### Scan a port range

```bash
nmap -p 1-1000 <target-IP>
```

### Scan all TCP ports

```bash
sudo nmap -p- <target-IP>
```

`-p-` means ports 1 through 65535.

### Scan the most common ports

```bash
nmap --top-ports 100 <target-IP>
```

Scan the 100 most common ports.

```bash
nmap --top-ports 1000 <target-IP>
```

Scan the 1,000 most common ports.

### Scan ports by protocol

```bash
sudo nmap -p T:80,443,U:53 <target-IP>
```

### Scan the default port list

```bash
nmap -p- <target-IP>
```

This checks all TCP ports. For UDP, use `-sU` explicitly.

---

# 3. Service, OS, and NSE Detection

## Service and version detection

Identify the application and version running on open ports:

```bash
nmap -sV <target-IP>
```

Nmap sends service probes to open ports and compares the responses with its service-detection database. [30]

## Service detection on selected ports

```bash
nmap -sV -p 22,80,443 <target-IP>
```

## Version intensity

Use a lower version-detection intensity:

```bash
nmap -sV --version-intensity 0 <target-IP>
```

Use a higher intensity:

```bash
nmap -sV --version-intensity 9 <target-IP>
```

The intensity ranges from 0 to 9:

- Lower values are faster and less comprehensive.
- Higher values perform more probes.
- Higher values may create more traffic and take longer.

## Operating system detection

Attempt to identify the target operating system:

```bash
sudo nmap -O <target-IP>
```

## OS detection with version detection

```bash
sudo nmap -O -sV <target-IP>
```

## Aggressive scan

Enable several advanced detection features:

```bash
sudo nmap -A <target-IP>
```

`-A` enables several features, including:

- OS detection.
- Service and version detection.
- Default NSE scripts.
- Traceroute.

Because it combines several detection techniques, it can generate more traffic than a basic scan.

## OS detection with more aggressive guessing

```bash
sudo nmap -O --osscan-guess <target-IP>
```

This asks Nmap to make a best-effort operating-system guess when the results are not conclusive.

---

## Nmap Scripting Engine

The Nmap Scripting Engine (NSE) allows scripts to perform additional discovery and security checks.

NSE scripts can help with:

- Service enumeration.
- Banner collection.
- HTTP information gathering.
- TLS configuration checks.
- SMB discovery.
- Authentication-related checks.
- Vulnerability detection.
- Enumeration of exposed resources.

NSE should only be used within the authorized scope.

## Run default scripts

```bash
nmap -sC <target-IP>
```

## Combine default scripts with version detection

```bash
nmap -sC -sV <target-IP>
```

This is a common enumeration command for identified web or network services.

## Run a specific script

```bash
nmap --script http-title -p 80,443 <target-IP>
```

## Check TLS cipher information

```bash
nmap --script ssl-enum-ciphers -p 443 <target-IP>
```

## Enumerate SMB operating-system information

```bash
nmap --script smb-os-discovery -p 445 <target-IP>
```

## Run safe scripts

```bash
nmap --script safe <target-IP>
```

The `safe` category contains scripts intended to be less intrusive, but they should still be reviewed and used only against authorized targets.

## Run default and safe scripts

```bash
nmap --script "default,safe" <target-IP>
```

## Run vulnerability scripts

```bash
nmap --script vuln <target-IP>
```

Vulnerability scripts may produce false positives and can generate significant traffic. Never treat their output as a confirmed vulnerability without manual validation.

## Run scripts against selected ports

```bash
nmap --script http-title,http-headers -p 80,443 <target-IP>
```

## Display information about a script

```bash
nmap --script-help http-title
```

## Display information about a script category

```bash
nmap --script-help safe
```

## List installed NSE scripts

```bash
ls /usr/share/nmap/scripts/
```

The exact location may vary depending on the operating system and installation method.

---

# 4. Timing, Performance, and Output

## Timing templates

Nmap provides timing templates from `-T0` to `-T5`.

```bash
nmap -T0 <target-IP>
```

Very slow and designed to reduce scan speed.

```bash
nmap -T1 <target-IP>
```

Slow timing profile.

```bash
nmap -T2 <target-IP>
```

Polite timing profile.

```bash
nmap -T3 <target-IP>
```

Default timing profile.

```bash
nmap -T4 <target-IP>
```

Faster timing profile, commonly used in controlled labs or authorized internal assessments.

```bash
nmap -T5 <target-IP>
```

Very aggressive timing. It can increase packet loss, inaccurate results, and service impact.

## Limit scan duration

Set a maximum time for scanning a host:

```bash
nmap --host-timeout 10m <target-IP>
```

## Limit retries

Reduce the number of retransmissions:

```bash
nmap --max-retries 2 <target-IP>
```

Lower retry values can make scans faster but may reduce accuracy on unreliable networks.

## Set a maximum packet rate

```bash
sudo nmap --max-rate 100 <target-IP>
```

This limits the maximum number of packets sent per second.

## Set a minimum packet rate

```bash
sudo nmap --min-rate 50 <target-IP>
```

Use rate controls carefully. Excessive traffic can affect services and trigger monitoring systems.

## Add a delay between probes

```bash
sudo nmap --scan-delay 100ms <target-IP>
```

This can reduce scan speed and traffic concentration.

## Display periodic statistics

```bash
nmap --stats-every 10s <target-IP>
```

This displays scan progress at regular intervals.

---

## Save output as normal text

```bash
nmap -oN nmap-result.txt <target-IP>
```

## Save output as XML

```bash
nmap -oX nmap-result.xml <target-IP>
```

XML is useful for importing results into other tools.

## Save output as grepable format

```bash
nmap -oG nmap-result.gnmap <target-IP>
```

This format is useful for simple text processing.

## Save output in all major formats

```bash
nmap -oA nmap-result <target-IP>
```

This creates files such as:

```text
nmap-result.nmap
nmap-result.xml
nmap-result.gnmap
```

## Append output to an existing file

```bash
nmap --append-output -oN nmap-result.txt <target-IP>
```

## Resume an interrupted scan

```bash
nmap --resume nmap-result.nmap
```

This requires an existing normal-format output file from the interrupted scan.

## Increase output detail

```bash
nmap -v <target-IP>
```

## Enable debugging

```bash
nmap -d <target-IP>
```

Use higher debug levels only when troubleshooting scan behavior:

```bash
nmap -dd <target-IP>
```

---

# 5. Practical Workflows and Interpretation

## Basic reconnaissance workflow

### Step 1: Check whether the host is online

```bash
nmap -sn -n <target-IP>
```

### Step 2: Scan common TCP ports

```bash
nmap -n --top-ports 1000 <target-IP>
```

### Step 3: Scan all TCP ports

```bash
sudo nmap -n -sS -p- <target-IP>
```

### Step 4: Detect services and versions

Use the ports found in the previous scan:

```bash
nmap -n -sV -p 22,80,443 <target-IP>
```

### Step 5: Run default scripts

```bash
nmap -n -sC -sV -p 22,80,443 <target-IP>
```

### Step 6: Attempt OS detection

```bash
sudo nmap -n -O -p 22,80,443 <target-IP>
```

### Step 7: Save the final results

```bash
sudo nmap -n -sC -sV -O -p 22,80,443 -oA nmap-final <target-IP>
```

---

## Web service enumeration workflow

Scan common web ports and detect versions:

```bash
nmap -sV -p 80,443,8080,8443 <target-IP>
```

Collect the HTTP title:

```bash
nmap --script http-title -p 80,443,8080,8443 <target-IP>
```

Collect HTTP headers:

```bash
nmap --script http-headers -p 80,443,8080,8443 <target-IP>
```

Run selected HTTP discovery scripts:

```bash
nmap --script http-title,http-headers -p 80,443 <target-IP>
```

Always manually validate interesting results with an authorized browser, proxy, or HTTP client.

---

## TLS service enumeration workflow

Identify services on common TLS ports:

```bash
nmap -sV -p 443,465,636,8443 <target-IP>
```

Enumerate supported TLS ciphers:

```bash
nmap --script ssl-enum-ciphers -p 443 <target-IP>
```

Obtain certificate information:

```bash
nmap --script ssl-cert -p 443 <target-IP>
```

Run both scripts:

```bash
nmap --script ssl-cert,ssl-enum-ciphers -p 443 <target-IP>
```

---

## UDP enumeration workflow

Scan common UDP ports:

```bash
sudo nmap -sU --top-ports 100 <target-IP>
```

Scan selected UDP ports:

```bash
sudo nmap -sU -p 53,67,68,123,161 <target-IP>
```

Combine selected TCP and UDP ports:

```bash
sudo nmap -sS -sU -p T:22,80,443,U:53,123,161 <target-IP>
```

UDP results may require additional validation because many UDP services do not respond consistently.

---

## Full authorized assessment command

A detailed scan that saves results in multiple formats:

```bash
sudo nmap -n -sS -sV -sC -O -p- -T3 -oA nmap-assessment <target-IP>
```

This command performs:

- No DNS resolution.
- TCP SYN scanning.
- Service and version detection.
- Default NSE scripts.
- OS detection.
- Scanning of all TCP ports.
- Moderate timing.
- Output saved in normal, XML, and grepable formats.

Use this only when the scope allows the associated traffic and detection techniques.

---

## Common port states

| State | Meaning |
|---|---|
| `open` | An application is actively accepting connections on the port |
| `closed` | The port is reachable, but no application is listening |
| `filtered` | A firewall or filtering device prevents Nmap from determining the state |
| `open\|filtered` | Nmap cannot determine whether the port is open or filtered |
| `closed\|filtered` | Nmap cannot determine whether the port is closed or filtered |

The port state is not automatically a vulnerability. It is an observation that requires further analysis.

---

## Common Nmap options

| Option | Description |
|---|---|
| `-sS` | TCP SYN scan |
| `-sT` | TCP connect scan |
| `-sU` | UDP scan |
| `-sA` | TCP ACK scan |
| `-sV` | Service and version detection |
| `-O` | Operating-system detection |
| `-A` | Aggressive scan features |
| `-sC` | Run default NSE scripts |
| `--script` | Run selected NSE scripts or categories |
| `-p` | Specify ports |
| `-p-` | Scan all TCP ports |
| `--top-ports` | Scan the most common ports |
| `-Pn` | Skip host discovery |
| `-sn` | Host discovery without port scanning |
| `-n` | Disable DNS resolution |
| `-T0` to `-T5` | Timing templates |
| `-v` | Verbose output |
| `--reason` | Display reasons for port states |
| `--open` | Show only open or potentially open ports |
| `-oN` | Save normal output |
| `-oX` | Save XML output |
| `-oG` | Save grepable output |
| `-oA` | Save output in multiple formats |
| `--resume` | Resume an interrupted scan |
| `--host-timeout` | Limit scan time for a host |
| `--max-retries` | Limit retransmissions |
| `--stats-every` | Display periodic progress information |

---

## Recommended workflow

```text
1. Confirm authorization and scope.
2. Perform host discovery.
3. Scan common ports.
4. Scan all required ports.
5. Detect services and versions.
6. Run carefully selected NSE scripts.
7. Save the output.
8. Validate interesting results manually.
9. Remove false positives.
10. Document only confirmed findings.
```

## Important reminders

- An open port is not automatically a vulnerability.
- A service banner may be inaccurate or intentionally modified.
- Version detection does not prove that a vulnerability exists.
- NSE results may contain false positives.
- Aggressive timing can reduce scan accuracy.
- UDP scanning is usually slower and more difficult to interpret.
- Large scans can generate significant traffic.
- Scans may trigger firewall, IDS, IPS, or SIEM alerts.
- Keep original scan output as evidence.
- Validate important results with additional tools and manual testing.
- Never scan systems outside the authorized scope.

