# 🛡️ Obsidian-Stressor Suite

An advanced, high-performance multi-vector network stress-testing and packet injection framework engineered in Python 3. Designed specifically for measuring server resilience, evaluating security defenses, and performing authorized penetration testing labs.

---

### ⚠️ CRITICAL LEGAL & ETHICAL WARNING

* **AUTHORIZED TESTING ONLY:** This tool is strictly intended for educational purposes, security research, and stress testing networks or servers **for which you have explicit, written prior authorization and legal ownership.**
* **LAW ENFORCEMENT & ISP MONITORING:** Unauthorized deployment or malicious use against public servers, private infrastructures, or third-party networks constitutes a severe cybercrime. Internet Service Providers (ISPs), cybersecurity defense systems, and law enforcement agencies actively monitor abnormal volumetric traffic anomalies, proxy streams, and malicious disruptions.
* **LIABILITY:** Unauthorized use can result in immediate IP blacklisting, heavy criminal prosecution, severe fines, and legal penalties. The author and contributors assume **zero liability** and are not responsible for any misuse, damage, or legal consequences caused by this software.

---

### 🚀 Key Features

* **Multi-Protocol Vectors:** Supports UDP High-Volumetric Flood, TCP Connection Exhaustion, and Layer 7 HTTP Request Flooding.
* **Integrated Proxy Scraper:** Automated background proxy discovery and live socket validation.
* **Real-time Monitoring Live Dashboard:** Instant feedback tracking Packets Per Second (PPS), total volume, and active error rates.
* **Cross-Platform Compatibility:** Optimized natively for Android (Termux), Ubuntu, Debian, and Kali Linux environments.
Note: If there any bugs please report it. 
---

## 📥 Installation & Quick Start


```bash
pkg update && pkg install python git -y
git clone https://github.com/AmEeR159/Obsidian-DDoS.git
cd Obsidian-DDoS
python3 obsidian.py
