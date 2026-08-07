#**Lab ke-3 Proactive Threat Detection: Port Scan Analysis using UFW on Debian Linux**

---

## 🎯 **Objective**  
Lab ini berfokus pada **Blue Teaming (Pertahanan)**, dimana ini kita akana memantau dan memblokir aktivitas mencurigakan di jaringan.

1. **Port Scanning:** ini adalah teknik pengintaian. Persis seperti seorang pencuri yang mengecek pinut, jendela mana yang tidak terkunci di sebuah rumah, *port scanning* mengecek layanan apa aja yang terbuka dan bisa diakses disebuah server.
2. **Nmap (Network Mapper):** adalah perangkat lunak sumber terbuka (open-source) gratis yang digunakan untuk pemetaan jaringan, eksplorasi, dan audit keamanan. Alat ini bekerja dengan mengirimkan paket data ke target untuk menemukan perangkat aktif, port terbuka, layanan yang berjalan, serta mendeteksi sistem operasi dan jenis firewall yang digunakan (misalnya port 80 untuk web, port 22 untuk SSH).
3. **UFW (Uncomplicated Firewall):**) adalah program antarmuka baris perintah yang dirancang untuk menyederhanakan pengelolaan sistem firewall Netfilter pada kernel Linux. UFW hadir sebagai solusi praktis untuk mengatur aturan penyaringan paket data tanpa harus menulis perintah `iptables` yang rumit.
4. **Log Analysis:** saat UFW memblokir lalu lintas, ia akan mencatatnya di sistem log (`/var/log/ufw.log/). Menganalisis log ini adalah langkah untuk mendeteksi bahwa sistem sedang diintai.


## 🛠️ **Lab Setup**

### **System Requirements**
- **Attacker Machine:**: Kali Linux
- **Target Machine**: Debian Linux

### **Tools Needed**
- `nmap` (on attacker machine)
- `ufw` or `iptables` (on target machine)

### **Log Files**
- `/var/log/ufw.log` on Debian Server– Captures system and network-related messages

## 🛡️ MITRE ATT&CK Mapping
Aktivitas yang disimulasikan dalam lab ini dipetakan ke dalam framework MITRE ATT&CK:

* **Tactic:** Reconnaissance (TA0043)

Penjelasan: Penyerang sedang mengumpulkan informasi tentang infrastruktur jaringanmu sebelum melancarkan serangan.

* **Technique:** Active Scanning (T1595)

Penjelasan: Penyerang secara aktif mengirimkan probe (paket) ke sistemmu.

* **Sub-Technique**: Scanning IP Blocks (T1595.001)

Penjelasan: Penyerang menggunakan Nmap untuk memindai IP Debian milikmu untuk mencari port layanan yang terbuka (seperti HTTP/80).

* **Mitigation (Pertahanan):** Network Intrusion Prevention (M1031)

Penjelasan: Menggunakan firewall (UFW) untuk mendrop koneksi dari IP yang melakukan pemindaian yang agresif.

## 🧠 **What is a Network Port Scan?**

A **port scan** is a technique used by attackers to probe a system for open ports and active services. Tools like `nmap` are commonly used to map a system’s network surface.

### **Why It’s Dangerous**
- Port scans are often a **precursor to exploitation**
- They help attackers identify vulnerable services like open SSH, FTP, or outdated web servers

---

### What is Nmap?
- Nmap (Network Mapper) is an open-source network scanning tool.
- Used to discover hosts and services on a network.
- Helps in identifying open ports, running services, and OS detection.
- Commonly used for network inventory and vulnerability scanning.

---

### Nmap Popular Scan Types
- SYN Scan (-sS): Fast and stealthy port scan.
- TCP Connect Scan (-sT): Full TCP connection, less stealthy.
- UDP Scan (-sU): Scans UDP ports for services.
- Ping Scan (-sn): Checks which hosts are up, no port scan.

### 🔐 What is UFW?
- UFW stands for Uncomplicated Firewall, a frontend for iptables.
- Simplifies firewall management for Linux users.
- Used to allow, deny, and manage traffic rules easily.
- Logs are stored in `/var/log/ufw.log`.
- Rule file `/etc/ufw/before.rules`
- To check ufw status `ufw status`
- To check the rule number `ufw status numbered`

###🧾 UFW Rule Syntax
- Basic allow rule: ufw allow <port>
- Deny a port: ufw deny <port>
- Allow by service: ufw allow <service> (e.g., ufw allow ssh)
- Allow by IP: ufw allow from <IP>
- Allow specific port from IP: ufw allow from <IP> to any port <port>
- Delete rule: ufw delete allow <port>



## 🧪 **Lab Task: Explore and Analyze Linux Syslog for Network Scans**
