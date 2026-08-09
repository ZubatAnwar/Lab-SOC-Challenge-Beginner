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

## 🧠 **Apa itu Network Port Scanning?**

**Port scanning** adalah teknik mengirimkan paket data ke berbagai port jaringan pada komputer atau server untuk melihat port mana yang sedang terbuka, tertutup, atau disaring oleh firewall.

Gampangnya, bayangkan server seperti rumah:

- IP address = alamat rumah
- Port = pintu-pintu di rumah
- Port 80 = biasanya layanan HTTP
- Port 443 = biasanya HTTPS
- Port 22 = biasanya SSH

Port scanning seperti mengecek pintu mana yang bisa dibuka atau merespons.

Port scanning bisa digunakan untuk:

- 🔍 Mengetahui layanan yang sedang berjalan
- 🛡️ Audit keamanan server milik sendiri
- 🔧 Troubleshooting koneksi jaringan
- ⚠️ Dalam konteks serangan, mencari layanan yang mungkin punya kerentanan

### **Tapi Kenapa Port Scanning Berbahaya?**
Karena port scanning bisa menjadi langkah awal untuk menemukan celah keamanan, walaupun scanning itu sendiri belum tentu merupakan serangan.

1. **Mengetahui layanan yang terbuka**
Penyerang bisa melihat bahwa suatu server membuka SSH, HTTP, database, dan sebagainya.
2. **Mencari informasi tentang sistem**
Dari respons port, kadang bisa diketahui jenis layanan atau versinya.
3. **Menentukan target serangan**
Kalau ditemukan layanan yang memiliki kerentanan, penyerang bisa mencoba mengeksploitasinya.
4. **Membebani atau mengganggu sistem**
Scanning dalam jumlah sangat besar atau agresif dapat menghasilkan banyak trafik dan berpotensi mengganggu layanan.

---
Nah Contoh Alat yang terkenal ialah **Nmap**.
### Apa itu Nmap?
Nmap (Network Mapper) adalah alat untuk memeriksa dan memetakan jaringan. Salah satu fungsi terkenalnya adalah port scanning.

Dengan Nmap, bisa mengetahui misalnya:

- 🖥️ Perangkat apa yang aktif di jaringan
- 🚪 Port mana yang terbuka
- 🔧 Layanan apa yang berjalan pada port tersebut
- 🌐 Kadang bisa mengidentifikasi jenis/versi layanan
- 🛡️ Membantu audit keamanan jaringan
---

### Nmap Popular Scan Types
1. **TCP SYN Scan**
   Scan TCP yang populer dan relatif cepat. Nmap mengirim SYN untuk melihat apakah port merespons.
   Contoh Scan:
   (`nmap -sS <target>`)
2. **TCP Connect Scan**
   Membuat koneksi TCP secara penuh. Biasanya digunakan ketika SYN scan tidak tersedia.
   (`nmap -sT <target>`)
3. **UDP Scan**
   Mencari layanan yang berjalan melalui UDP, bukan TCP.
   (`nmap -sU <target>`)
4. **Service/version detection**
   Menggunakan paket TCP FIN untuk menguji respons port.
   (`nmap -sV <target>`)
5. **NULL Scan**
   Mengirim paket TCP tanpa flag. Berguna untuk mempelajari perilaku implementasi TCP tertentu.
   (`nmap -sN <target>`)
6. **Xmas Scan**
   Mengirim beberapa flag TCP sekaligus; namanya berasal dari flag yang dianggap “menyala” seperti lampu.
   (`nmap -sX <target>`)
7. **Ping/Host Discovery**
   Mencari apakah host aktif tanpa melakukan port scan biasa.
   (`nmap -sn <target>`)

### 🔐 Apa itu UFW?
UFW (Uncomplicated Firewall) adalah antarmuka sederhana untuk mengatur firewall pada sistem Linux. UFW dibuat agar pengguna tidak perlu berinteraksi langsung dengan konfigurasi firewall yang lebih kompleks seperti `iptables` atau `nftables`.

Sederhananya:

  **UFW menentukan koneksi jaringan mana yang boleh masuk atau ditolak oleh komputer.**

  🧱 1. UFW bekerja sebagai firewall

Firewall bisa dianggap seperti penjaga pintu.

Misalnya:

Internet
    ↓
┌─────────────┐
│     UFW     │
│  FIREWALL   │
└─────────────┘
    ↓
  Server

Ketika ada koneksi masuk, firewall dapat memeriksa aturan yang sudah dibuat.

Contohnya:

Ada koneksi ke port 22
        ↓
   UFW cek aturan
        ↓
    ALLOW ?
      /   \
    Ya    Tidak
    ↓       ↓
 Diterima  Ditolak


###🧾 Syntax UFW yang Penting & Sering Digunakan
1. **Melihat Status**
   Melihat status UFW dan aturan yang aktif.
   (`sudo ufw status`)
2. **Mengaktifkan UFW**
   Mengaktifkan firewall UFW dan menerapkan aturan yang sudah dibuat.
   (`sudo ufw enable`)
3. **Menonaktifkan UFW**
   Menonaktifkan firewall UFW.
   (`sudo ufw disable`)
4. **Mengizinkan Port**
   Mengizinkan koneksi masuk ke port tertentu.
   (`sudo ufw allow <port>`)
5. **Menolak Port**
   Menolak koneksi masuk ke port tertentu.
   (`sudo ufw deny <port>`)

**Catatan: Link Dokumentasi Nmap dan UFW**
*Nmap*
https://nmap.org/docs.html

*UFW*
https://help.ubuntu.com/community/UFW

## 🧪 **Lab Task: Explore and Analyze Linux Syslog for Network Scans**
