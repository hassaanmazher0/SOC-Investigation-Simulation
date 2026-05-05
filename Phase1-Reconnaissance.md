> **Objective:** Map the target Windows 7 endpoint using Nmap,
> identify open ports and services, confirm vulnerability to MS17-010,
> and observe how Wazuh detects scanning activity.

**Steps:** 
- 1.1 → Host Discovery (is the target alive?) 
- 1.2 → SYN Scan (what ports are open?) 
- 1.3 → Service Detection (what is running on each port?) 
- 1.4 → Aggressive Scan (OS, scripts, traceroute) 
- 1.5 → Vulnerability Scan (is MS17-010 present?) 


## Environment at This Phase

| Role | Machine | IP |
|------|---------|-----|
| Attacker | Ubuntu 25.10 (Host) | 192.168.1.Y |
| Victim | Windows 7 SP1 (VM) | 192.168.1.X |
| SIEM | Wazuh (running on Host) | 127.0.0.1 |

> **Replace 192.168.1.X with your actual Windows 7 VM IP.**
> Find it on Windows 7 with: `ipconfig`
> Find your host IP with: `ip addr show`


## Step 1.1 — Host Discovery

**Goal:** Confirm the target is online before scanning.

**Simple ICMP ping sweep — just checking if host responds**
- nmap -sn 192.168.1.X

**What this does:**
- Sends ICMP echo requests (ping)
- Does NOT scan any ports
- Tells you if the host is reachable before committing to a full scan
- In a real engagement this is the first thing an attacker does

**What to note:**
- Latency (very low = same network segment)
- MAC address vendor (VirtualBox tells us it's a VM)
- A real attacker uses this to understand network topology


## Step 1.2 — SYN Scan (Stealth Scan)

**Goal: Identify all open TCP ports on the target.**

- -sS = SYN scan (half-open — never completes handshake)
- -oN = save output to file (always save your evidence)
- sudo nmap -sS 192.168.1.X -oN evidence/nmap_syn_scan.txt

**What this does:**
- Sends a TCP SYN packet to each port
- If port is open → target replies SYN-ACK
- Attacker sends RST immediately (never completes connection)
- Faster and slightly quieter than a full connect scan
- Still detected by Wazuh — the volume of SYN packets is the giveaway

**Key Finding:**
- Port 445 (SMB) is open.
- This is the attack surface for MS17-010 (EternalBlue).
- Document this immediately.


## Step 1.3 — Service and Version Detection

**Goal: Identify exactly what software is running on each open port.**

- -sV = version detection (banner grabbing + probing)
- -sC = default NSE scripts (extra enumeration)
- sudo nmap -sV -sC 192.168.1.X -oN evidence/nmap_service_scan.txt

**What this does:**
- Probes each open port with protocol-specific requests
- Grabs service banners where available
- Runs default Nmap scripts against each service
- Significantly noisier than a SYN scan — more packets, more time

**Key Finding:**
- SMB message signing is disabled.
- Windows 7 Ultimate SP1 — version confirmed.
- This is all the information an attacker needs to select the right exploit.









