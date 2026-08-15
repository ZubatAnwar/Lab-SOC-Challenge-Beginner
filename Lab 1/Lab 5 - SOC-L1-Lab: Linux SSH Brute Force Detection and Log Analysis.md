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
