# 🛡️ End-to-End SOC Investigation Simulation

> A complete adversary simulation and SOC investigation built on Wazuh SIEM —
> covering every phase from reconnaissance to incident reporting.


## 📌 Project Overview
This project simulates a **full cyberattack lifecycle** against a Windows 7 endpoint
and documents a complete **SOC analyst investigation** using Wazuh as the SIEM platform.

Every phase mirrors real-world SOC work:
- An attacker perspective (what was done and how)
- A defender perspective (what Wazuh caught and why it matters)
- A documentation perspective (incident report, timelines, evidence)


## 🏗️ Lab Architecture

┌─────────────────────────────────────────────────────┐
│          HOST: Ubuntu 25.10                         │
│  ┌─────────────────┐    ┌────────────────────────┐  │
│  │  Wazuh Manager  │    │  Attacker Tools        │  │
│  │  + Dashboard    │    │  (Nmap, Metasploit,    │  │
│  │  (Port 443)     │    │   Wireshark)           │  │
│  └────────┬────────┘    └──────────┬─────────────┘  │
│           │                        │                 │
│    ┌──────▼────────────────────────▼──────┐          │
│    │         VirtualBox Network           │          │
│    └──────────────────┬───────────────────┘          │
│                       │                              │
│              ┌────────▼────────┐                     │
│              │  Windows 7 VM   │                     │
│              │  (Wazuh Agent)  │                     │
│              │  VICTIM         │                     │
│              └─────────────────┘                     │
└─────────────────────────────────────────────────────┘


## ⚔️ Attack Phases Simulated

| Phase | Activity | Tools Used |
|-------|----------|------------|
| 1 — Reconnaissance | Host discovery, port scan, service fingerprint, vuln scan | Nmap |
| 2 — Exploitation | EternalBlue SMB exploit → SYSTEM shell | Metasploit |
| 3 — Post-Exploitation | Enumeration, backdoor account, data staging | Meterpreter, cmd.exe |
| 4 — Detection | SIEM triage, custom rule building, MITRE mapping | Wazuh |
| 5 — Investigation | Timeline reconstruction, IOC extraction, gap analysis | Wazuh, manual analysis |
| 6 — Incident Report | Formal documented report with recommendations | — |


## ⚠️ Legal Disclaimer

> This project was conducted entirely within an **isolated lab environment** on machines I own and control.
> All attack techniques were simulated for **educational purposes only**.
> No external systems were targeted at any point.


## 👤 Author

**Hassaan Mazhar**
www.linkedin.com/in/hassaan09 | hassan.mazher5@gmail.com
