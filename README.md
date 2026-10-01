# Cowrie SSH Honeypot Project

## Overview

This project documents the deployment and analysis of a **Cowrie SSH honeypot** on an Ubuntu 24.04.5 virtual machine. The honeypot was configured to simulate a vulnerable SSH server, capturing brute-force login attempts, credential harvesting, and post-authentication attacker behavior.

## Objectives

- Deploy a realistic SSH honeypot to attract and monitor malicious activity
- Capture and analyze attacker credentials, commands, and sessions
- Identify attack patterns, source IPs, and common exploitation techniques
- Document findings for security research and awareness

## Architecture

```
┌─────────────┐         ┌──────────────────────┐
│  Attacker   │───SSH──▶│  Ubuntu 24.04.5 VM   │
│  (Internet) │         │                      │
└─────────────┘         │  ┌────────────────┐  │
                        │  │  Cowrie (2222)  │  │
                        │  │  ↕ Fake Shell   │  │
                        │  │  ↕ Logging      │  │
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

- Ubuntu 24.04.5 VM with internet access
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

## License

This project is licensed under the MIT License.
