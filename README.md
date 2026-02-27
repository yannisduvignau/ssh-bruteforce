# 🔐 ssh-bruteforce

> A **Python-based SSH brute-force / credential stuffing tool** for educational purposes and authorized penetration testing — automates SSH login attempts using a CSV wordlist.

---

## ⚠️ Legal Disclaimer

> This tool is intended **for educational purposes and authorized security testing only**. Using this tool against any system without **explicit written permission** from the owner is **illegal** under the Computer Fraud and Abuse Act (CFAA), the Computer Misuse Act, and equivalent laws worldwide. The author assumes no responsibility for any misuse or damage caused. **Always obtain proper authorization before testing.**

---

## 📋 Description

**ssh-bruteforce** is a Python script that performs **credential stuffing** attacks against SSH services. It reads a list of username/password combinations from a CSV file (`passwords.csv`) and attempts to authenticate against a target SSH server using the **Paramiko** library.

This project is ideal for:
- Understanding how SSH brute-force attacks work in practice
- Testing the strength of your own SSH server's credentials
- Learning about the Paramiko SSH library in Python
- Practicing on isolated lab environments (CTF boxes, VMs, HackTheBox, TryHackMe…)

---

## 🗂️ Project Structure

```
ssh-bruteforce/
├── main.py           # Core script — SSH connection attempts & result reporting
├── passwords.csv     # Credential wordlist (username, password pairs)
├── requirements.txt  # Python dependencies
└── README.md
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Python 3.x |
| SSH Library | [Paramiko](https://www.paramiko.org/) |
| Wordlist Format | CSV (`username,password`) |
| Environment | Python virtual environment (`venv`) |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.x installed
- `pip` package manager
- Network access to the target SSH server (authorized only)

---

### 1. Clone the repository

```bash
git clone https://github.com/yannisduvignau/ssh-bruteforce.git
cd ssh-bruteforce
```

### 2. Create a virtual environment

```bash
# Windows
python -m venv venv_ssh

# macOS / Linux
python3 -m venv venv_ssh
```

### 3. Activate the virtual environment

```bash
# macOS / Linux
source venv_ssh/bin/activate

# Windows
venv_ssh\Scripts\activate
```

### 4. Install dependencies

```bash
pip install -r requirements.txt
```

### 5. Configure your target

Edit `main.py` to set the **target host**, **port**, and **timeout** values:

```python
HOST    = "192.168.1.100"   # Target IP or hostname
PORT    = 22                # SSH port (default: 22)
TIMEOUT = 5                 # Connection timeout in seconds
```

### 6. Prepare your wordlist

Edit `passwords.csv` — one `username,password` pair per line:

```csv
root,toor
admin,admin
admin,password
root,123456
ubuntu,ubuntu
pi,raspberry
```

### 7. Run the script

```bash
python main.py
```

### 8. Deactivate when done

```bash
deactivate
```

---

## ⚙️ How It Works

```
passwords.csv (username, password pairs)
           │
           │  read line by line
           ▼
        main.py
           │
           │  Paramiko SSH connect(host, port, username, password)
           ▼
    SSH Server Response
           │
           ├─ Auth success  → ✅ Valid credentials found! Print & stop
           └─ Auth failure  → ❌ Try next pair
```

1. The script reads each `username,password` pair from `passwords.csv`
2. For each pair, it attempts an **SSH connection** using Paramiko's `SSHClient`
3. On a successful authentication, it prints the valid credentials and stops
4. On failure (`AuthenticationException`), it moves on to the next pair
5. Network errors (timeout, refused connection) are caught and reported gracefully

---

## 📄 Wordlist Format

`passwords.csv` uses a simple two-column CSV format:

```
username,password
root,toor
admin,admin123
root,password
ubuntu,ubuntu
```

You can replace this file with any credential list. Popular sources for testing:

| Resource | Description |
|----------|-------------|
| [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Passwords) | Comprehensive security wordlists |
| `rockyou.txt` | 14M+ common passwords (Kali Linux: `/usr/share/wordlists/`) |
| Default credentials | Vendor default SSH credentials for routers, IoT, servers |

---

## 📦 Dependencies

| Package | Purpose |
|---------|---------|
| `paramiko` | SSH2 protocol implementation for Python — handles connections and authentication |

Install manually if needed:
```bash
pip install paramiko
```

---

## 🧪 Safe Testing Environments

**Only test on systems you own or have explicit authorization to test.** Recommended environments:

| Platform | Description |
|----------|-------------|
| Local VM | Spin up a Ubuntu/Debian VM with SSH enabled |
| [HackTheBox](https://www.hackthebox.com/) | Legal CTF machines with SSH services |
| [TryHackMe](https://tryhackme.com/) | Guided rooms with SSH brute-force challenges |
| [VulnHub](https://www.vulnhub.com/) | Downloadable vulnerable VMs for local practice |
| [PentesterLab](https://pentesterlab.com/) | Web and system security exercises |

---

## 🛡️ Defensive Countermeasures

Understanding this attack helps you harden your SSH server:

| Defense | Implementation |
|---------|---------------|
| **Disable password auth** | `PasswordAuthentication no` in `sshd_config` → use SSH keys only |
| **Change default port** | Move SSH away from port 22 to reduce automated scans |
| **Fail2ban** | Auto-ban IPs after N failed login attempts |
| **Rate limiting** | Use `iptables` or `ufw` to throttle SSH connection attempts |
| **AllowUsers / AllowGroups** | Restrict which users can authenticate via SSH |
| **MFA** | Add a second factor (e.g. Google Authenticator) for SSH logins |
| **IP whitelisting** | Only allow SSH from known trusted IP ranges |

**Example: harden `sshd_config`**
```bash
# /etc/ssh/sshd_config
PasswordAuthentication no
PermitRootLogin no
MaxAuthTries 3
LoginGraceTime 20
AllowUsers youruser
```

---

## 🧠 Concepts Covered

| Topic | Description |
|-------|-------------|
| SSH Protocol | How SSH authentication handshakes work |
| Paramiko API | `SSHClient`, `AutoAddPolicy`, `connect()`, `AuthenticationException` |
| Credential Stuffing | Systematic testing of known username/password pairs |
| Exception Handling | Gracefully catching auth failures and network errors |
| Virtual Environments | Isolating Python dependencies per project |

---

## 🤝 Contributing

1. Fork the project
2. Create your branch (`git checkout -b feature/threading-support`)
3. Commit your changes (`git commit -m 'Add multi-threaded support'`)
4. Push to the branch (`git push origin feature/threading-support`)
5. Open a Pull Request

---

## 👤 Author

**Yannis Duvignau**  
[GitHub](https://github.com/yannisduvignau)

---

## 📚 Resources

- 📖 [Paramiko Documentation](https://docs.paramiko.org/)
- 🛡️ [OWASP — Credential Stuffing](https://owasp.org/www-community/attacks/Credential_stuffing)
- 🔐 [SSH Hardening Guide](https://www.ssh.com/academy/ssh/sshd_config)
- 📋 [SecLists — Wordlists](https://github.com/danielmiessler/SecLists)
- 🔒 [Fail2ban Documentation](https://www.fail2ban.org/wiki/index.php/Main_Page)

---

## 📄 License

This project is distributed under an open license. See the `LICENSE` file for more details.
