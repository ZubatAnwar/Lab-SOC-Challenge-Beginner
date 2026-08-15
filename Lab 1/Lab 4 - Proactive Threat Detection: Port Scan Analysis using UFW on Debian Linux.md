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
- **Attacker Machine:**: Mint Linux
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
Nah contoh alat yang terkenal ialah **Nmap**.
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
- https://nmap.org/docs.html

*UFW*
- https://help.ubuntu.com/community/UFW

## 🧪 **PRAKTIKUM**
*Catatan: Disini saya menggunakan Debian dan Linux Mint untuk melakukan Lab ini sebenernya bebas pakai OS Linux apa aja.*

**Tahap 1: Persiapan Pertahanan (Di Terminal Debian)**
1. Buka terminal Debian
2. Install UFW dengan menjalankan perintah:
   `sudo apt update && sudo apt install ufw -y`
3. Aktifkan UFW (Ketik 'y' jika diminta konfirmasi)
   `sudo ufw enable`
4. Aktifkan pencatatan log tingkat tinggi agar aktivitas *scanning* terekam detail:
   `sudo ufw logging high`
5. Buat aturan untuk menolak koneksi ke port 80 secara spesifik dari IP mesin penyerang:
   `sudo ufw deny from [IP_LINUX (attacker)] to any port 80 proto tcp
6. Terapkan aturan yang baru saja dibuat:
   `sudo ufw reload`
   
**Tahap 2: Monitoring Log secara Real-Time (Di Terminal Debian)**
1. Masih di terminal **Debian**, jalankan perintah ini:
   `sudo tail -f /var/log/ufw.log | grep "IP LINUX (attacker)"
2. Terminal akan diam dan menunggu. Biarkan terminal tetap terbuka dan jangan tertutup.

**Tahap 3: Simulasi Serangan Port Scan (Di Terminal Linux Mint)**
1. Buka terminal **Linux Mint**
2. Pastikan Nmap terinstal:
   `sudo apt update && sudo apt install nmap -y`
3. Luncurkan pemindaian *Stealt Scan (SYN Scan) ke port 80 milik target:
   `sudo nmap -sS -p80 -Pn [IP Target Machine]`

<img width="1340" height="234" alt="Cuplikan layar 2026-08-12 224000" src="https://github.com/user-attachments/assets/75371a2a-9366-499d-b996-48a20b10432e" />

📖 **Penjelasan Analisis Log & Simulasi Serangan**

**1. Sudut Pandang Penyerang (Terminal Kanan - Mesin Linux Mint)**
Di sisi kanan, kita bertindak sebagai penyerang yang mencoba mencari celah keamanan di server target.
 - Perintah: `sudo nmap -sS -p80 -Pn 10.0.2.4`
    - Penyerang mengirimkan pemindaian *Stealth SYN Scan (`-sS`) yang menargetkan secara spesifik layanan web atau port
      80 (-p80) pada IP target `10.0.2.4`
 - Hasil tangkapan Nmap: `80/tcp filtered http`
    - Perhatikan status `filtered` (disaring). Ini adalah tanda keberhasilan pertahanan! Jika port terbuka, statusnya          akan *open*, jika server mati statusnya closed. Status *filtered* berarti paket penyerang diabaikan atau dibuang         (drop) ditengah jalan oleh sebuah firewall, sehingga Nmap milik penyerang tidak bisa memastikan apakah port              tersebut sebenarnya terbuka atau tertutup.
      
2. **Sudut Pandang Pertahanan (Terminal Kiri - Mesin Debian)**
Di sisi kiri, kita melihat apa yang sebenarnya terjadi di belakang layar pada mesin target (Debian) saat serangan itu datang.
- Perintah: `sudo tail -f /var/log/ufw.log | grep "10.0.2.15"`
   - Administrator server sedang memantau log firewall secara langsung (real-time) khusus untuk aktivitas yang berasal        dari IP penyerang `(10.0.2.15)`.
     
- Bedah Log UFW (Analisis Forensik):
   - `2026-08-12T22:39:21` : **Timestamp (Waktu Kejadian)**. Menunjukkan kapan tepatnya paket jahat tersebut menyentuh         server.
   - `[UFW AUDIT]` / `[UFW BLOCK]` : **Aksi**. Menandakan bahwa aturan Firewall (UFW) telah terpicu dan sistem melakukan       audit/pemblokiran lalu lintas.
   - `IN=enp0s3` : **Interface Masuk**. Menunjukkan kartu jaringan mana yang menerima paket serangan tersebut.
   - `SRC=10.0.2.15` : **Source IP***. Ini adalah alamat IP pelaku/penyerang. Terbukti bahwa serangan berasal dari mesin       Linux Mint.
   - `DST=10.0.2.4` : **Destination IP**. Ini adalah IP mesin server kita (target).
   - `PROTO=TCP` : **Protokol**. Serangan menggunakan protokol komunikasi TCP.
   - `SPT=63175` : **Source Port (Port Asal)**. Port acak yang digunakan oleh Nmap penyerang untuk mengirim paket.
   - `DPT=80` : **Destination Port (Port Tujuan)**. Ini membuktikan bahwa penyerang secara spesifik mengincar port 80          (HTTP).
   - `SYN` : **TCP Flag**. Adanya label SYN di akhir log mengonfirmasi bahwa jenis serangan yang digunakan memang benar        SYN Scan, sesuai dengan argumen -sS yang diketik oleh penyerang.
     
**Kesimpulan:**
Bahwa konfigurasi *Uncomplicated Firewall (UFW)* berfungsi dengan sempurna. Firewall tidak hanya berhasil mencegah penyerang mengidentifikasi status port 80 (ditandai dengan status *filtered* pada Nmap), tetapi juga berhasil merekam jejak digital penyerang (IP, Port, dan Waktu) dengan sangat akurat di dalam sistem log untuk keperluan investigasi keamanan (*Threat Hunting*).
