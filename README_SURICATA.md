# 🛡️ Network Intrusion Detection System (NIDS) – Suricata Setup

## 📘 Project Overview
This project demonstrates the setup and configuration of a Network-Based Intrusion Detection System (NIDS) using Suricata, an open-source threat detection engine.  
The goal is to detect suspicious or malicious network activities, generate alerts, and optionally visualize them for easier analysis.

This implementation was created for **Task 2: Network Intrusion Detection System** as part of a cybersecurity coursework.

---

## 🎯 Objectives
- 🔹 Set up Suricata on a Linux environment (Ubuntu).  
- 🔹 Configure detection rules to identify network threats such as SSH connections and ICMP pings.  
- 🔹 Monitor live traffic from a chosen network interface (e.g., `eth0` or `enp0s3`).  
- 🔹 Generate alerts and analyze them using log files (`fast.log`, `eve.json`).  
- 🔹 Implement a simple response mechanism (e.g., blocking malicious IPs).  
- 🔹 Visualize detected events using JSON logs or dashboards.

---

## ⚙️ Installation & Setup

### 🧩 Step 1 — Install Suricata
```bash
sudo apt update
sudo apt install -y suricata
````

### 🧩 Step 2 — Run Suricata on your network interface

```bash
sudo suricata -c /etc/suricata/suricata.yaml -i eth0
```

> ⚠️ Replace `eth0` with your actual interface name (e.g., `enp0s3`, `wlan0`, etc.)

You should see output confirming:

* Suricata version and system mode
* Threads created and started
* Packets processed
* Drops = 0 ✅

---

## 🧱 Creating Custom Detection Rules

Rules are located in:

```
/etc/suricata/rules/local.rules
```
Restart Suricata to apply changes:

```bash
sudo systemctl restart suricata
```

---

## 🧪 Testing & Generating Alerts

Run safe, harmless commands inside your lab environment:

```bash
ping <target-ip>
ssh user@<target-ip>
nmap -sS -p 22 <target-ip>
```

Then view alerts in real time:

```bash
sudo tail -f /var/log/suricata/fast.log
```

Example alert:

```
10/05/2025-14:12:09.123456  [**] [1:1000002:1] LOCAL ICMP Echo request [**] [Priority: 0] {ICMP} 10.0.2.15 -> 10.0.2.5
```

---

## 📊 Logs & Visualization

Suricata stores logs in `/var/log/suricata/`:

| File           | Description                         |
| -------------- | ----------------------------------- |
| `fast.log`     | Readable alert summaries            |
| `eve.json`     | Detailed JSON alerts for dashboards |
| `stats.log`    | Packet and performance statistics   |
| `suricata.log` | Engine/system messages              |

View detailed JSON alerts:

```bash
sudo apt install jq -y
sudo jq '. | select(.event_type=="alert")' /var/log/suricata/eve.json | less
```

Optional visualization tools:

EveBox
ELK Stack (Elasticsearch + Kibana)
Export `eve.json` → CSV → graphs in Excel or Google Sheets

---

## 🧰 Automated Response (Optional)

You can block detected IPs manually using:

```bash
sudo iptables -I INPUT -s <ip-address> -j DROP
```

> ⚠️ Use only in a controlled lab.
> Do not block users or run scans on real networks.

---

## 🧠 Key Learnings

* How Intrusion Detection Systems monitor and analyze network traffic
* Writing and applying custom detection rules
* Interpreting IDS alerts and logs
* Automating basic defensive responses
* Using visualization for security monitoring

---

## 📁 Repository Structure

```
📂 Network-IDS-Suricata
│
├── README.md                # Project documentation (this file)
├── rules/
│   └── local.rules           # Custom detection rules
├── screenshots/              # Screenshots of logs and dashboards
├── logs/
│   ├── fast.log              # Example alert logs
│   ├── eve.json              # Detailed JSON logs
│   └── stats.log
└── scripts/
    └── auto_block.sh         # Example firewall automation script
```

---

## 🔍 Example Alert Explained

```
[1:1000002:1] LOCAL ICMP Echo request {ICMP} 10.0.2.15 -> 10.0.2.5
```

Explanation:

* `1:1000002:1` → Rule ID and revision
* `LOCAL ICMP Echo request` → Description message
* `{ICMP}` → Protocol
* `10.0.2.15 -> 10.0.2.5` → Source → Destination

---

## 🔒 Ethical Notice

> ⚠️ This project is for educational and defensive security purposes only.
> All experiments were conducted in an isolated lab environment.
> Do not perform scanning or intrusion testing on public or unauthorized networks.

---

## ✍️ Author

Zahra Bibi
Cybersecurity Student / Researcher
📅 *October 2025*
🔗 GitHub: [ZahraChaudhary](https://github.com/ZahraChaudhary)
