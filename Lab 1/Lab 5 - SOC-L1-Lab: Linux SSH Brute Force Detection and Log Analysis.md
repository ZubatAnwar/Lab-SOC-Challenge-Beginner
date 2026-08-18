#**SOC-L1-Lab: Linux SSH Brute Force Detection and Log Analysis**

---

## 🎯 **Objective**  
Lab ini berfokus pada **Attacker (Red Team)** dimana mencoba mendobrak pintu server menggunakan **Hydra**, dan sekaligus sebagai **Defender (Blue Team / Pertahanan)** yang melihat untuk mendeteksi pendobrakan tersebut dan menganalisisnya. 

- **SSH (Secure Shell):** SSH (Secure Shell) adalah protokol jaringan yang dipakai untuk mengontrol dan mengelola komputer atau server dari jarak jauh secara aman. Protokol ini menggunakan teknologi enkripsi agar data yang dikirim tidak bisa dibaca oleh orang lain di internet.
- **Brute Force Attack** Serangan dimana peretas mencoba menebak *username* dan *password* secara paksa dan terus-menerus menggunakan alat otomatis (Seperti Hydra) dikombinasikan dengan kamus dan memasukkan jutaan kombinasi huruf, angka, dan simbol secara berulang hingga menemukan kombinasi yang benar.
- **Auth Logs (`/var/log/auth.log`):** Adalah berkas catatan sistem pada sistem operasi Linux berbasis Debian atau Ubuntu yang merekam semua aktivitas otentikasi dan otorisasi, seperti keberhasilan atau kegagalan login, akses jarak jauh SSH, serta penggunaan hak akses tingkat tinggi via perintah sudo.

## 🛠️ **Lab Setup**

### **System Requirements**
- **Attacker Machine:**: Mint Linux
- **Defender Machine**: Debian Linux

### **Tools Needed**
- `Hydra` (on attacker machine)
- `openssh-server` (on target machine)
- `rsyslog` (default logging service)

### **Log Files**
- `/var/log/auth.log` – Authentication logs (Ubuntu/Debian)

## 🛡️ MITRE ATT&CK Mapping
Aktivitas yang disimulasikan dalam lab ini dipetakan ke dalam framework MITRE ATT&CK:

* **Tactic:** Credential Access (TA0006).

Penjelasan: Peretas ingin mencuri atau menebak kredensial/password.

* **Technique:** Brute Force (T1110).

Penjelasan: Mencoba melakukan menebak kata sandi secara paksa dengan berulang kali dan otomatis untuk mendapatkan akses ilegal ke suatu sistem.

* **Sub-Technique**: Password Guessing (T1110.001).

Penjelasan: Penyerang penyerang mencoba menebak kata sandi akun secara sistematis dan berulang tanpa memiliki pengetahuan awal tentang kredensial yang sah untuk satu akun (`root`).

## 🧪 **Eksekusi Lab**
💻 **Di Mesin Victim (Debian)**
Lakukan langkah-langkah ini untuk menginstal dan menyalakan SSH:

**1. Update repository dan instal OpenSSH Server:**
Buka terminal di Debian dan jalankan:
`sudo apt update`
`sudo apt install openssh-server -y`

**2. Jalankan layanan SSH:**
Setelah instalasi selesai, layanan SSH seharusnya bernama ssh. Mari kita nyalakan:

`sudo systemctl start ssh`

**3. Buat SSH otomatis menyala saat Debian restart (Opsional tapi direkomendasikan):**

`sudo systemctl enable ssh`

**4. Cek statusnya untuk memastikan sudah berjalan:**

`sudo systemctl status ssh`

**Catatan:**Cari tulisan berwarna hijau "Active (running)*.

**5. Periksa status firewall:**

`sudo ufw status`

Jika tulisannya "Status: active", maka Anda harus membuka jalur untuk SSH dengan perintah ini:

`sudo ufw allow ssh`

**.6 Cek IP Address Debian:**
Anda akan butuh IP ini untuk diserang. Ketik:

`ip a`

**.7 Uji ping dari Linux Mint ke Debian dan sebaliknya:**

`ping [IP_VICTIM/ATTACKER]`

pastikan sudah berhasil dan saling berkomunikasi.

🥷 **Di Mesin Attacker (Linux Mint)**
Karena Anda menggunakan Linux Mint (yang masih satu keluarga dengan Debian/Ubuntu/Kali), pastikan juga tool Hydra sudah siap di mesin penyerang Anda.

`sudo apt update`
`sudo apt install hydra -y`

**2. Siapkan Wordlist (rockyou.txt):**
Biasanya di Kali Linux rockyou.txt sudah ada secara default. Namun di Linux Mint, Anda mungkin harus mengunduhnya atau mengekstraknya terlebih dahulu atau buat sendiri secara simple aja 10 baris dan disisipi password korban nya (Debian).
Cara cepat menginstal wordlist standar di Linux Mint:

`sudo apt install seclists wordlists -y`

atau jika ingin menggunakan worldlist sendiri secara sederhana:

`nano pass.txt`

**3. Mulai Serangan:**
Jalankan Hydra dengan target IP Debian Anda (ganti [OS VICTIM] dan [USERNAME_VICTIM] dengan yang sesuai):

`hydra -l USERNAME VICTIM  -P pass.txt/worldlist.txt ssh://IP VICTIM -t 4`

📺 **Langkah 1: Pasang "CCTV" di Mesin Victim (Debian)**
Sebelum Anda menembakkan serangan dari Linux Mint, Anda harus standby dulu di Debian.

1. Buka terminal di **Debian / OS VICTIM**
2. Ketik perintah dibawah ini:

   `sudo tail -f /var/log/auth.log`

   *Penjelasan:`tail` digunakan untuk melihat bagian bawah/terbaru dari sebuah file log, dan `-f` artinya "follow" atau ikuti terus. Jadi terminal Anda akan "menggantung" dan menunggu baris log baru masuk).*
   Biarkan terminal Debian tetap **terbuka dan jangan ditutup**.
   
**⚔️ Langkah 2: Mulai Serangan dari Mesin Attacker (Linux Mint)**
Sekarang, biarkan Debian menyala dengan terminal yang sedang memantau log, lalu pindah ke komputer penyerang.
1. Buka terminal **Linux Mint / OS Attacker**
2. Jalankan perintah hydra:

   `hydra -l USERNAME VICTIM  -P pass.txt/worldlist.txt ssh://IP VICTIM -t 4`
3. Enter

🚨 **Langkah 3: Lihat Hasilnya secara Real-Time!**
Segera setelah Anda menekan Enter di Linux Mint, alihkan pandangan Anda ke layar Debian.

Anda akan melihat teks bermunculan dan bergulir dengan sangat cepat di layar Debian. Teks-teks tersebut adalah rekaman log dari percobaan gagal (dan mungkin berhasil) yang dilakukan oleh Hydra. Akan ada banyak tulisan **"Failed password"**.

<img width="1739" height="566" alt="Cuplikan layar 2026-08-17 223317" src="https://github.com/user-attachments/assets/aabee204-38c9-44dc-9620-70e4563557d9" />

🥷 **Di Sisi Attacker (Linux Mint - Kanan)**
- **Serangan Berhasil:** Hydra menghentikan serangannya karena berhasil menemukan password yang valid.

- **Kredensial Ditemukan:** Perhatikan baris berwarna hijau: [22][ssh] host: 10.0.2.4 login: vboxuser password: user. Ini berarti Hydra berhasil menebak bahwa password untuk akun vboxuser adalah user.

- (Catatan: Dalam dunia nyata, ini adalah insiden kritis karena peretas berhasil mendapatkan akses masuk).

🛡️ **Di Sisi Target/Victim (Debian - Kiri)**
**Log Terekam:** Server Debian Anda mencatat rentetan serangan tersebut di /var/log/auth.log.

**Jejak IP Penyerang:** Anda bisa melihat dengan jelas tulisan Failed password for vboxuser from 10.0.2.6. Ini mengonfirmasi bahwa IP Linux Mint Anda setelah disetel ke NAT Network adalah 10.0.2.6.

**Peringatan PAM:** Terdapat log PAM 3 more authentication failures;. PAM (Pluggable Authentication Modules) adalah sistem keamanan Linux yang menyadari bahwa ada seseorang yang mencoba memasukkan password salah berkali-kali secara tidak wajar.

🚀 **Langkah Selanjutnya: Deteksi & Analisis Log**
1.**Menampilkan Semua Log Percobaan Gagal:**
Perintah ini akan menyaring jutaan teks di sistem dan hanya menampilkan baris yang mengandung kata "Failed password".

`sudo grep "Failed password" /var/log/auth.log`

-------------------------------------------------------------------------------------------------------------------------
