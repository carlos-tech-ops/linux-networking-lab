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
