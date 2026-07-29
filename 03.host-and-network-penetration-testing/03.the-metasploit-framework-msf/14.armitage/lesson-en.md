# Armitage

## Armitage - MSF GUI

Armitage is a graphical user interface (GUI) for the Metasploit Framework. It makes Metasploit easier to use by giving you a visual way to interact with targets, services, sessions, and exploits.

Armitage is especially useful for:

- Visualizing hosts and network relationships.
- Running scans and enumeration tasks.
- Launching exploits from a GUI.
- Managing active sessions.
- Supporting post-exploitation tasks like dumping hashes, browsing files, and pivoting.

>  **Port Scanning & Enumeration With Armitage** — lab by INE.

---

## Armitage Setup

### Start the Required Services

Before opening Armitage, Metasploit must be connected to PostgreSQL.

```bash
service postgresql start && msfconsole -q
db_status
```

If the database is connected correctly, Metasploit should show that it is connected to PostgreSQL.

### Launch Armitage

Open Armitage from a new terminal:

```bash
armitage
```

When prompted, answer **YES** to start the RPC server.

### Why This Matters

Armitage depends on the Metasploit database and RPC services to manage hosts, services, loot, and sessions properly.

---

## Port Scanning And Enumeration

### Add Targets

Once Armitage is open, add the victim host manually:

- Open **Hosts**.
- Select **Add Hosts**.
- Add the victim IP.
- Name the host if needed, for example `Victim 1`.

### Scan The Host

Right-click the target and choose **Scan**.  
You can also perform an Nmap scan from the **Hosts** menu.

Armitage will display discovered services and open ports in a visual format.

### Typical Workflow

```bash
# Add host from the GUI, then scan it
```

### What You Can See

After scanning, Armitage can show:

- Open ports.
- Running services.
- Detected versions.
- Service relationships.

### Why This Matters

Armitage makes enumeration easier by presenting scan results visually instead of requiring you to inspect everything manually in the console.

---

## Exploitation With Armitage

### Searching For Exploits

Armitage can search for modules related to a service and launch them directly from the interface.

For example, if a target is running Rejetto HFS, you can search for `rejetto` and launch the corresponding exploit module.

### Typical Example

A vulnerable HFS service can be exploited from Armitage by selecting the host, choosing the detected service, and launching the matching Metasploit module.

### Why This Matters

This workflow reduces the amount of manual Metasploit interaction required and makes exploitation faster for lab and assessment work.

---

## Post Exploitation With Armitage

### Dumping Hashes

Armitage can be used to run post-exploitation actions such as dumping Windows hashes.

One example is using the registry method through the `smart_hashdump` module:

```bash
post/windows/gather/smart_hashdump
```

Saved hashes can then be found under the **View > Loot** menu.

### Browsing Files

After compromise, Armitage can help you browse files on the target system from the session context.

### Viewing Processes

You can also inspect running processes to identify useful targets for migration or privilege escalation.

### Why This Matters

Armitage makes common post-exploitation tasks more accessible and helps you move quickly from access to deeper system discovery.

---

## Pivoting With Armitage

### What Pivoting Does

Pivoting lets you use a compromised host to reach other systems on the internal network that are not directly accessible from your attack machine.

### Typical Workflow

After compromising `Victim 1`, you can set up pivoting and add a route to the internal subnet.

```bash
run autoroute -s <target_network>/24
```

Then you can scan `Victim 2` through the pivot host.

### Port Forwarding

If a service is detected on the second host, you can forward the port through the compromised machine:

```bash
portfwd add -l 1234 -p 80 -r <target_ip>
db_nmap -sV -p 1234 localhost
```

This lets you inspect the service on the remote host as if it were running locally on `127.0.0.1:1234`.

### Why This Matters

Pivoting is one of the strongest features in Armitage because it helps you extend access beyond the first compromised host and explore deeper into the network.

---

## Exploiting A Second Host

### Example Workflow

After discovering services on `Victim 2`, you can search for matching exploits and launch them from Armitage or Metasploit.

If the target is vulnerable, you may also need to:

- Migrate to a stable process.
- Rename the session for clarity.
- Use the correct payload type for the architecture.

### Typical Follow-Up Actions

```bash
sessions -n victim-2 -i 2
```

### Why This Matters

Renaming sessions and organizing the compromise chain becomes very useful once you start working with multiple systems at the same time.

---

## Armitage On Kali Linux

### Installation

If Armitage is not already available, you can install it on Kali and prepare the database:

```bash
sudo apt install armitage -y
sudo msfdb init
sudo nano /etc/postgresql/15/main/pg_hba.conf
sudo systemctl enable postgresql
sudo systemctl restart postgresql
sudo armitage
```

### Database Authentication Note

If Armitage cannot connect properly, you may need to adjust PostgreSQL authentication settings in `pg_hba.conf` so the client can connect locally.

### Why This Matters

Armitage is tightly tied to Metasploit’s backend services, so the database and RPC configuration must be correct before the GUI will work properly.

---

## Why This Matters

Armitage gives you a visual way to work with Metasploit, which makes scanning, exploitation, session handling, loot review, and pivoting easier to manage. It is especially helpful in labs where you want to move quickly between discovery and post-exploitation.

## Key Takeaways

- **Armitage** is a GUI for Metasploit that simplifies scanning, exploitation, and session management.
- It requires **PostgreSQL** and Metasploit to be running correctly.
- You can add hosts, scan them, and launch exploits directly from the GUI.
- **Loot**, file browsing, and process viewing are available after compromise.
- **Pivoting** with `autoroute` and `portfwd` lets you reach internal systems through a compromised host.
- On Kali, Armitage may require database setup and PostgreSQL authentication adjustment before it works properly.
