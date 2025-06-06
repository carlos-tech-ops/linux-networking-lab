## ⚙️ Phase 3: Linux Networking Lab

This phase focuses on essential networking concepts and commands used to inspect and troubleshoot network configurations on a Linux system. The commands were executed on a remote Fedora host accessed via hardened SSH from a MacBook terminal.

### ✅ Objectives

- Display and interpret IP address configuration
- Inspect routing table entries
- Check device status with `nmcli`
- Explore network interfaces using `ip link`

### 🛠️ Tools Used

- `ip a` – shows IP address assignment
- `ip route` – displays routing table
- `nmcli device status` – checks network manager status
- `ip link show` – inspects interface state and stats
- Fedora Linux (target node)
- macOS Terminal (control node)

### 📸 Screenshots

**1. IP Address Output**
  
![01-ip-address-output](screenshots/01-ip-address-output.png)

**2. Routing Table**

![02-routing-table](screenshots/02-routing-table.png)

**3. NMCLI Device Status**

![03-nmcli-device-status](screenshots/03-nmcli-device-status.png)

**4. IP Link Summary**

![04-ip-link-summary](screenshots/04-ip-link-summary.png)

---

> All outputs were captured during real command-line sessions as part of the `linux-networking-lab`. This project is part of a Linux+ certification training path and documents validated system behavior on a Fedora server.

---

## 🌐 Static IP Configuration + Hostname + /etc/hosts

### 🎯 Objective

Manually assign a static IP to the Fedora laptop and map it to a custom hostname using `/etc/hosts`. This setup improves SSH stability, scripting, and internal network recognition — all critical for sysadmin and DevOps tasks.

---

### 🔧 Steps Performed

```bash
# 1. Set static IP, gateway, and DNS via nmcli
sudo nmcli con mod SHELL_5C4F_5G ipv4.addresses 192.168.1.50/24
sudo nmcli con mod SHELL_5C4F_5G ipv4.gateway 192.168.1.1
sudo nmcli con mod SHELL_5C4F_5G ipv4.dns "1.1.1.1 8.8.8.8"
sudo nmcli con mod SHELL_5C4F_5G ipv4.method manual
sudo nmcli con down SHELL_5C4F_5G && sudo nmcli con up SHELL_5C4F_5G

# 2. Confirm IP works with ping
ping -c 3 8.8.8.8

# 3. Set custom hostname
sudo hostname fedora-lab
# (hostnamectl set-hostname failed due to PolicyKit auth timeout)

# 4. Update /etc/hosts
sudo nano /etc/hosts
# → Add line: 192.168.1.50    fedora-lab

# 5. Validate hostname resolution
ping -c 3 fedora-lab
```

---

### 🧪 Output Validation

| ✅ Checkpoint                         | Result                                   |
|--------------------------------------|------------------------------------------|
| Static IP applied via `nmcli`        | `192.168.1.50/24` assigned to Wi-Fi      |
| Internet test with ping              | Successful: packets returned from 8.8.8.8 |
| `hostname` command output            | Transient hostname set to `fedora-lab`   |
| `/etc/hosts` manually updated        | Maps `192.168.1.50` to `fedora-lab`      |
| `ping fedora-lab`                    | <1 ms latency — local resolution working |

---

### 📸 Screenshots

| Step | Description                             | Screenshot                              |
|------|-----------------------------------------|------------------------------------------|
| 1️⃣  | `nmcli` static IP config + ping test     | ![05](screenshots/05-static-ip-ping-success.png) |
| 2️⃣  | Hostname set via `hostnamectl` attempt   | ![06](screenshots/06-hostnamectl-fedora-lab.png) |
| 3️⃣  | `ping fedora-lab` successful             | ![07](screenshots/07-ping-fedora-lab-success.png) |

---

## 📡 Phase 3B – DNS Configuration & Testing

In this section, we configure DNS resolvers manually and validate DNS resolution using diagnostic tools.

---

### 🛠️ DNS Configuration

| Step | Command | Purpose |
|------|---------|---------|
| 1️⃣  | `sudo nano /etc/resolv.conf` | Edit DNS configuration file manually |
| 2️⃣  | Add the following lines: <br> `nameserver 1.1.1.1` <br> `nameserver 8.8.8.8` | Set Cloudflare and Google as DNS resolvers |
| 3️⃣  | Save and exit (`CTRL + O`, `Enter`, `CTRL + X`) | Apply the DNS changes |
| 4️⃣  | `cat /etc/resolv.conf` | Confirm resolvers are correctly set |

## 📸 Screenshot:  
![08](screenshots/08-resolv-conf-dns-set.png)_

---

### 🧪 DNS Lookup Test (dig)

| Step | Command | Description |
|------|---------|-------------|
| ✅   | `dig github.com` | Query DNS to resolve GitHub’s IP address |

Expected Output:
- You should see an **ANSWER SECTION** with GitHub's IP addresses.
- `Query time`, `SERVER`, and `WHEN` values validate DNS resolution is working.

## 📸 Screenshot:  
![09](screenshots/09-dig-github-success.png)_

---

### 🧪 DNS Test (ping)

| Step | Command | Description |
|------|---------|-------------|
| ✅   | `ping -c 3 github.com` | Validate name resolution + connectivity |

Expected Output:
- 3 replies from GitHub’s IP (usually a `20.x.x.x` address).
- **0% packet loss**, and time in milliseconds.

## 📸 Screenshot:  
![10](screenshots/10-ping-github-success.png)_

---

---

## 🧪 Phase 3C – Network Diagnostics Tools

In this phase, we tested and validated network configurations, DNS functionality, service routes, and port availability using key diagnostic tools.

---

### 🛠️ Tools Used

| Command                | Purpose                                      |
|------------------------|----------------------------------------------|
| `ping`                 | Test basic IP connectivity                   |
| `dig`                  | Perform DNS lookup                           |
| `ss -tuln`             | Show listening ports (TCP/UDP)               |
| `netstat -tuln`        | Legacy alternative to `ss`                   |
| `nmcli dev show`       | Display detailed device connection info      |
| `journalctl -u NetworkManager` | View network-related logs          |
| `ip route`             | View current routing table                   |
| `traceroute`           | Show packet path to destination              |

---

### 📸 Screenshots

| Step | Description                             | Screenshot |
|------|-----------------------------------------|------------|
| 1️⃣  | `ping google.com` successful            | ![11](screenshots/11-ping-google-success.png) |
| 2️⃣  | `dig google.com` DNS resolution         | ![12](screenshots/12-dig-google-response.png) |
| 3️⃣  | `ss -tuln` showing listening ports       | ![13](screenshots/13-ss-listening-ports.png) |
| 4️⃣  | `netstat -tuln` legacy output            | ![14](screenshots/14-netstat-listening-ports.png) |
| 5️⃣  | `nmcli dev show` detailed info           | ![15](screenshots/15-nmcli-dev-show.png) |
| 6️⃣  | `journalctl -u NetworkManager` log check | ![16](screenshots/16-journalctl-networkmanager.png) |
| 7️⃣  | `ip route` routing summary               | ![17](screenshots/17-ip-route-summary.png) |
| 8️⃣  | `traceroute google.com` multi-hop test   | ![18](screenshots/18-traceroute-google.png) |

---

---

✅ **Summary:**  
We configured DNS resolvers by editing `/etc/resolv.conf`, validated them with `dig`, and confirmed hostname resolution via `ping`. These are core diagnostics for sysadmins and network troubleshooting.
