> **Objective:** Map the target Windows 7 endpoint using Nmap,
> identify open ports and services, confirm vulnerability to MS17-010,
> and observe how Wazuh detects scanning activity.

Steps: 
1.1 → Host Discovery (is the target alive?) 
1.2 → SYN Scan (what ports are open?) 
1.3 → Service Detection (what is running on each port?) 
1.4 → Aggressive Scan (OS, scripts, traceroute) 
1.5 → Vulnerability Scan (is MS17-010 present?) 


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




