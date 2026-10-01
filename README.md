# Cowrie SSH Honeypot Project

## Overview

A Cowrie SSH/Telnet honeypot was deployed on an Ubuntu virtual machine to simulate a vulnerable server environment. A separate Kali Linux machine was used to conduct controlled attacks against the honeypot, including brute-force SSH login attempts, port scanning, and shell interaction. The objective was to analyze how Cowrie captures, logs, and categorizes attacker behavior like authentication attempts, executed commands, and session recordings. 

## Objectives

- Deploy SSH honeypot to simulate attacks
- Capture and analyze attacker credentials, commands, and sessions
- Identify attack behavior, source IP, and common exploitation techniques
- Document findings for security research and awareness

## Architecture

```
┌──────────────────────┐         ┌──────────────────────┐
│  Kali Linux 2026.3   │         │  Ubuntu 24.04.5 VM   │
│  (Attack Machine)    │───SSH──▶│  (Honeypot Server)   │
│                      │         │                      │
│  • Hydra             │         │  ┌────────────────┐  │
│  • Nmap              │         │  │  Cowrie (2222)  │  │
│  • Cowrie Playlog    │         │  │  ↕ Fake Shell   │  │
└──────────────────────┘         │  │  ↕ Logging      │  │
                                 │  └────────────────┘  │
                                 └──────────────────────┘
```

## Tech Stack

| Component       | Details                          |
|-----------------|----------------------------------|
| **OS**          | Ubuntu 24.04.5 LTS               |
| **Honeypot**    | Cowrie SSH/Telnet Honeypot       |
| **Language**    | Python (Cowrie framework)        |
| **Logging**     | JSON logs, session transcripts   |
| **Attack OS**   | Kali Linux 2026.3                |
| **Attack Tools** | Hydra, Nmap, Cowrie Playlog     |
| **Deployment**  | Virtual Machine                  |

## Project Structure

```
├── README.md              # Project documentation
├── screenshots/           # Screenshots of setup, dashboards, attacks
├── logs/                  # Sample log files (sanitized)
├── data/                  # Parsed/analyzed attack data
├── configs/               # Cowrie configuration files (sanitized)
└── FINDINGS.md            # Analysis and key findings
```

## Setup & Deployment

### Prerequisites

- Ubuntu 24.04.5 VM 
- Python 3.x
- Git

### Installation Steps

1. **Update the system**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install dependencies**
   ```bash
   sudo apt install -y git python3-venv python3-pip libssl-dev libffi-dev build-essential
   ```

3. **Create a dedicated user**
   ```bash
   sudo adduser --disabled-password cowrie
   sudo su - cowrie
   ```

4. **Clone and configure Cowrie**
   ```bash
   git clone https://github.com/cowrie/cowrie.git
   cd cowrie
   python3 -m venv cowrie-env
   source cowrie-env/bin/activate
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

5. **Configure Cowrie**
   ```bash
   cp etc/cowrie.cfg.dist etc/cowrie.cfg
   # Edit etc/cowrie.cfg to customize hostname, SSH version, etc.
   ```

6. **Redirect port 22 to Cowrie (port 2222)**
   ```bash
   sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222
   ```

7. **Start Cowrie**
   ```bash
   bin/cowrie start
   ```

## Attack Simulation

A separate **Kali Linux 2026.3** machine was used to simulate realistic attacks against the honeypot.

### Nmap Port Scanning

Nmap was used to discover open ports and identify the SSH service running on the honeypot:

```bash
nmap -sV -p 22 <honeypot-ip>
```

### Hydra Brute-Force Attack

Hydra was used to perform SSH brute-force attacks with common username and password wordlists:

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://<honeypot-ip>
```

### Cowrie Playlog (Session Replay)

Cowrie's `playlog` utility was used to replay captured attacker sessions, allowing detailed review of commands executed inside the fake shell:

```bash
bin/playlog log/tty/<session-id>.log
```

This provided insight into what attackers attempted after gaining access, including reconnaissance commands, malware downloads, and privilege escalation attempts.

## Findings

See [FINDINGS.md](FINDINGS.md) for the full analysis of captured data, including:

- Top attempted usernames and passwords
- Geographic distribution of attack sources
- Common post-login commands executed by attackers
- Attack frequency and timing patterns

## Screenshots

Screenshots documenting the setup process, live attacks, and analysis dashboards are in the [`screenshots/`](screenshots/) directory.

## Disclaimer

This project was conducted in a controlled environment for **educational and research purposes only**. The honeypot was deployed on an isolated virtual machine. No real systems were compromised, and no offensive actions were taken against any attackers. All logged IP addresses and credentials in this repository have been sanitized.

## Author

**Amanda Bouddhara**

