## ⚙️ Phase 3: Linux Networking Lab

This phase focuses on core Linux networking diagnostics, SSH configuration, and firewall validation. All tasks were performed on a Fedora lab system, remotely accessed via a hardened SSH setup from a MacBook Terminal.

---

### 🎯 Objectives

- Display and interpret IP address configuration
- Inspect routing tables and interface stats
- Assign static IP and map hostname
- Configure DNS manually and test resolution
- Run connectivity diagnostics (`ping`, `dig`, `traceroute`)
- Validate SSH setup and secure configuration
- Test firewall rules and remote login

---

### 🛠️ Tools Used

- `ip`, `ss`, `netstat`, `nmcli`, `dig`, `ping`, `traceroute`
- `hostnamectl`, `/etc/hosts`, `firewalld`, `journalctl`
- `sshd_config`, SSH over port 2222
- Fedora Linux (remote target), macOS (control)

---

## 🧱 Phase 3A – Interfaces & Static IP Setup

Manually assign a static IP to improve hostname resolution and SSH reliability.

#### 🔧 Commands Used

```bash
sudo nmcli con mod SHELL_5C4F_5G ipv4.addresses 192.168.1.50/24
sudo nmcli con mod SHELL_5C4F_5G ipv4.gateway 192.168.1.1
sudo nmcli con mod SHELL_5C4F_5G ipv4.dns "1.1.1.1 8.8.8.8"
sudo nmcli con mod SHELL_5C4F_5G ipv4.method manual
sudo nmcli con down SHELL_5C4F_5G && sudo nmcli con up SHELL_5C4F_5G
ping -c 3 8.8.8.8
sudo hostname fedora-lab
sudo nano /etc/hosts   # Add: 192.168.1.50 fedora-lab
ping -c 3 fedora-lab
```

#### 🧪 Output Validation

| Checkpoint                  | Result                              |
|----------------------------|-------------------------------------|
| Static IP                  | `192.168.1.50/24` applied           |
| Ping 8.8.8.8               | Success                             |
| Hostname (hostname)        | `fedora-lab` set                    |
| /etc/hosts entry           | IP maps to hostname                 |
| ping fedora-lab            | Success <1ms                        |

#### 📸 Screenshots

![Static IP Config + Ping](screenshots/05-static-ip-ping-success.png)
![Hostname Set](screenshots/06-hostnamectl-fedora-lab.png)
![Ping Hostname](screenshots/07-ping-fedora-lab-success.png)

---

## 🧠 Phase 3B – DNS Configuration & Testing

#### 📝 DNS Setup

```bash
sudo nano /etc/resolv.conf
# Add:
nameserver 1.1.1.1
nameserver 8.8.8.8

cat /etc/resolv.conf
```

#### 🧪 DNS Validation

```bash
dig github.com
ping -c 3 github.com
```

#### 📸 Screenshots

![resolv.conf](screenshots/08-resolv-conf-dns-set.png)
![dig github.com](screenshots/09-dig-github-success.png)
![ping github.com](screenshots/10-ping-github-success.png)

---

## 🛰️ Phase 3C – Network Diagnostics Tools

#### 🧪 Commands Used

```bash
ping -c 3 google.com
dig google.com
ss -tuln
netstat -tuln
nmcli dev show
journalctl -u NetworkManager
ip route
traceroute google.com
```

#### 📸 Screenshots

![Ping Google](screenshots/11-ping-google-success.png)
![Dig Google](screenshots/12-dig-google-response.png)
![SS Listening](screenshots/13-ss-listening-ports.png)
![Netstat Listening](screenshots/14-netstat-listening-ports.png)
![nmcli Device Info](screenshots/15-nmcli-dev-show.png)
![NetworkManager Logs](screenshots/16-journalctl-networkmanager.png)
![IP Route](screenshots/17-ip-route-summary.png)
![Traceroute](screenshots/18-traceroute-google.png)

---

## 🔐 Phase 3D – SSH & Remote Connectivity

#### 🔍 SSH Listening Port Check

```bash
sudo ss -tuln | grep 2222
```

✅ Port 2222 is listening

![Port Listening](screenshots/19-sshd-port-listening.png)

---

#### 🔧 SSHD Configuration (`/etc/ssh/sshd_config`)

```conf
Port 2222
PasswordAuthentication no
PermitRootLogin no
AllowUsers sysops
Protocol 2
```

✅ SSH securely configured

![sshd config 1](screenshots/20-sshd-config-0.png)  
![sshd config 2](screenshots/20-sshd-config-1.png)

---

#### 📜 SSH Service Logs

```bash
sudo journalctl -u sshd --since "15 minutes ago"
```

✅ SSH service restarted correctly

![SSHD journalctl](screenshots/20-sshd-journalctl-output.png)

---

#### ✅ Remote SSH Login Test

```bash
ssh sysops@192.168.1.50 -p 2222
```

✅ Key-based login successful

![SSH Success](screenshots/21-ssh-remote-login-success.png)

---

## 🔥 Phase 3E – Firewall Setup & Port Rules

```bash
sudo firewall-cmd --state
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-all --zone=public
sudo firewall-cmd --add-port=2222/tcp --permanent
sudo firewall-cmd --reload
sudo ss -tuln | grep 2222
nmap -p 2222 192.168.1.50
```

✅ SSH port added and confirmed via `ss` and `nmap`

#### 📸 Screenshots

![Firewall State](screenshots/22-firewall-state.png)  
![Active Zones](screenshots/23-firewall-active-zones.png)  
![Zone Config](screenshots/24-firewall-zone-drop-list-all.png)  
![Port Rule Added](screenshots/25-firewall-port-2222-added.png)  
![Firewall Reloaded](screenshots/26-firewall-reload-success.png)  
![ss port listening](screenshots/27-ss-port-2222-listening.png)  
![nmap result](screenshots/28-nmap-2222-port-open.png)  
![Final SSH Login](screenshots/29-ssh-success-2222.png)

---

## ✅ Phase 3F – Final Recap & Validation Checklist

| Task                                     | Status  |
|------------------------------------------|----------|
| Static IP Assigned                        | ✅        |
| Hostname + /etc/hosts Updated             | ✅        |
| DNS Resolvers Set                         | ✅        |
| SSHD Config Hardened (port, root, password) | ✅        |
| Remote SSH via Key (MacBook → Fedora)     | ✅        |
| Firewall Rule for Port 2222               | ✅        |
| Connectivity Validated (ping, dig, traceroute) | ✅    |

---

## 📦 Project Context

This project is part of the `linux-networking-lab`, a hands-on reinforcement exercise aligned with the CompTIA Linux+ certification objective: **"System Management – Networking Configuration & Troubleshooting."** All actions were tested, documented, and version-controlled via GitHub.
