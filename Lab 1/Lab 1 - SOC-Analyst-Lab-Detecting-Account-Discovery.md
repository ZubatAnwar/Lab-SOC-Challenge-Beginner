#**Lab ke-1 SOC-Analyst-Lab**

# 🔍 SOC Analyst Lab: Windows Event Log Analysis & PowerShell Execution Detection

## 🎯 Objective
Lab praktis ini bertujuan untuk melakukan konfigurasi keamanan pada lingkungan Windows (mengaktifkan Script Block Logging via Registry), mensimulasikan aktivitas mencurigakan, dan melakukan analisis *event log* (evtx) untuk mendeteksi anomali dan
utamanya dari lab ini adalah untuk memahami dasar-dasar log analysis dan mempraktikkan bagaimana seorang SOC (Security Operations Center) Analyst menggunakan catatan sistem (logs) untuk mendeteksi ancaman keamanan.

1. Pemahaman Konfigurasi Keamanan (Security Configuration)
Mempelajari cara meningkatkan visibilitas keamanan sistem operasi Windows dengan mengaktifkan fitur pencatatan tingkat lanjut, yaitu PowerShell Script Block Logging, meskipun tanpa menggunakan Group Policy Editor (gpedit).

2. Simulasi Taktik Penyerang (Adversary Simulation)
Mempraktikkan cara berpikir dan bertindak seperti seorang peretas dengan mensimulasikan fase Discovery (Pengintaian), yaitu mengeksekusi perintah untuk mencari tahu daftar pengguna di dalam sistem (Get-LocalUser).

3. Keterampilan Forensik Digital & Deteksi (Digital Forensics & Detection)
Melatih kemampuan untuk mencari, memfilter, dan menganalisis Windows Event Logs (secara spesifik menemukan Event ID 4104) guna mengidentifikasi artifact atau jejak digital dari aktivitas yang mencurigakan tersebut.

4. Pemetaan Standar Internasional (Framework Mapping)
Membiasakan diri dengan standar industri keamanan siber global dengan mengklasifikasikan temuan serangan menggunakan MITRE ATT&CK Framework (Tactic: Discovery TA0007, Technique: Account Discovery T1087.001).

## 🛠️ Tools & Environment
* OS: Windows 10/11 (Home Edition)
* Tools: PowerShell, Windows Event Viewer, Registry Editor (regedit)

## 🛡️ MITRE ATT&CK Mapping
Aktivitas yang disimulasikan dalam lab ini dipetakan ke dalam framework MITRE ATT&CK:
* **Tactic:** Discovery (TA0007)
* **Technique:** Account Discovery: Local Account (T1087.001)

## 📌 Pengertian Log
Log dalam konteks keamanan siber (security) adalah catatan digital yang terstruktur, kronologis, dan dihasilkan secara otomatis mengenai aktivitas, peristiwa, serta transaksi yang terjadi di dalam sistem komputer, jaringan, atau aplikasi bisa juga dikenal sebagai log peristiwa keamanan adalah catatan digital dari aktivitas dan peristiwa sistem, seperti upaya login, perubahan kebijakan, dan akses ke data sensitif. Log ini dikumpulkan dari berbagai sumber seperti basis data, server, workstation, dan firewall, dan memberikan gambaran kronologis tentang apa yang terjadi dalam sistem.

## 🧠 Fungsi Log

Log digunakan untuk:

1. Debugging (memperbaiki error)
Membantu developer mengetahui apa yang salah dalam program.
2. Monitoring sistem
Melihat apakah sistem berjalan normal atau tidak.
3. Audit (jejak aktivitas)
Mencatat aktivitas user atau sistem (siapa melakukan apa).
4. Analisis performa
Mengetahui bottleneck atau masalah performa.

## 📊 Contoh Format Log Windows (Umum)
🪟 Log pada Windows
```
Di Windows, log disimpan dalam Event Viewer.
```
Jenis log utama:

- `Application Log` → dari aplikasi
- `System Log` → dari sistem operasi
- `Security Log` → aktivitas login & keamanan

Lokasi akses:

`Event Viewer` → Windows Logs

📌 Contoh Log Windows
```
Log Name: Security
Source: Microsoft-Windows-Security-Auditing
Date: 2026-04-29 10:20:15
Event ID: 4624
Task Category: Logon
Level: Information
Description:
An account was successfully logged on.

Account Name: user1
Source Network Address: 192.168.1.10
```
🧠 Penjelasan:
- `Event ID 4624` → login berhasil
- `Account Name` → user yang login
- `Source Network Address` → IP asal
- `Level: Information` → status (bukan error)

## 💻 Contoh Log di Linux (tambahan untuk perbandingan)
Contoh log linux ada di:
📁 /var/log/
Beberapa file log penting:

- `/var/log/syslog` → log umum sistem
- `/var/log/auth.log` → aktivitas login & autentikasi
- `/var/log/kern.log` → log kernel
- `/var/log/nginx/access.log` → log akses web server

```
Apr 28 10:15:32 server sshd[1234]: Failed password for root
Apr 28 10:16:10 server kernel: CPU temperature high
Apr 29 10:15:32 server sshd[1234]: Accepted password for user1 from 192.168.1.10 port 54321 ssh2
```
🧠 Penjelasan:
- `Apr 29 10:15:32` → waktu kejadian
- `server` → nama host/server
- `sshd[1234]` → service (SSH daemon) + PID
- `Accepted password` → aksi (login berhasil)
- `user1` → user yang login
- `192.168.1.10` → alamat IP asal

👉 Artinya: ada login berhasil ke server melalui SSH.

Tools Populer Untuk Analisis Log:

🔥 1. Log Management & Observability

🔹 ELK Stack

Terdiri dari:

- Elasticsearch
- Logstash
- Kibana

📌 Digunakan untuk:

- Centralized logging
- Search & filtering log
- Dashboard monitoring

🔹 Grafana + Loki

📌 Digunakan untuk:

- Monitoring modern (cloud, Kubernetes)
- Visualisasi log & metrics
- Alternatif ringan ELK
  
🔹 Graylog

📌 Digunakan untuk:

- Centralized log management
- Analisis log skala besar
- UI lebih sederhana dibanding ELK
  
⚡ 2. Enterprise & Commercial Tools
🔹 Splunk

📌 Digunakan untuk:

- Big data log analysis
- SIEM (Security Information and Event Management)
- Monitoring enterprise

🔹 Datadog

📌 Digunakan untuk:

- Monitoring cloud (AWS, Azure, GCP)
- Log + metrics + tracing
- Observability all-in-one
  
🔹 New Relic

📌 Digunakan untuk:

- Application performance monitoring (APM)
- Log & tracing
- Debugging aplikasi production
  
🛡️ 3. Security & SIEM Tools
🔹 Wazuh

📌 Digunakan untuk:

- Threat detection
- Intrusion detection (HIDS)
- Security log analysis
  
🔹 OSSEC

📌 Digunakan untuk:

- Monitoring keamanan host
- Deteksi perubahan file
- Analisis log security
  
🔹 IBM QRadar

📌 Digunakan untuk:

- SIEM enterprise
- Analisis ancaman
- Korelasi log keamanan
  
🔹 ArcSight

📌 Digunakan untuk:

- Security monitoring skala besar
- Compliance & audit
  
☁️ 4. Cloud-Native Logging
🔹 AWS CloudWatch

📌 Digunakan untuk:

- Monitoring AWS resources
- Log aplikasi cloud
  
🔹 Google Cloud Logging

📌 Digunakan untuk:

- Log di Google Cloud
- Integrasi dengan GCP services
  
🔹 Azure Monitor

📌 Digunakan untuk:

- Monitoring cloud Azure
-Log analytics

🧰 5. Tools Pendukung (Linux CLI)

Digunakan untuk analisis cepat:

- `grep` → cari log
- `tail` → lihat log real-time
- `awk` → parsing
- `sed` → manipulasi teks

## **Praktikum:**
Untuk lab ini perlu disiapkan beberapa alat untuk melakukan analisis, jadi lab ini **tidak menggunakan Group Policy Editor (gpedit.msc)**. tapi menggunakan jalan pintas/alternatif yaitu **Registry Editor**.

**Catatan**

🔧 Group Policy Editor (gpedit)
- Antarmuka berbasis GUI (lebih ramah pengguna)
- Digunakan untuk mengatur kebijakan sistem (policy) secara terstruktur
- Setting sudah dikelompokkan rapi (misalnya: keamanan, update, jaringan)
- Lebih aman karena opsi yang tersedia terbatas (tidak bisa sembarangan ubah)
- Biasanya tersedia di Windows Pro, Enterprise, Education (tidak ada di Home)

🧠 Registry Editor (regedit)
- Antarmuka berupa struktur database (tree/hierarki)
- Menyimpan semua konfigurasi Windows dan aplikasi
- Lebih fleksibel tapi juga lebih berisiko (salah edit bisa bikin sistem error)
- Tersedia di semua versi Windows (termasuk Home)

**🔍 Intinya**
- **gpedit = cara aman & terstruktur untuk setting kebijakan**
- **regedit = cara bebas & mendalam untuk modifikasi sistem**

## **Penjelasan Lab**
Sistem operasi selalu mencatat aktivitas penting dalam bentuk *log*. Di Windows dikelola oleh *Event Viewer*.
**Skenario**
Seorang hacker berhasil masuk ke dalam sebuah sistem, salah satu hal yang pertama dilalkukan adalah mencari tahu "Siapa saja yang ada dalam sistem ini?". Perintah `Get-LocalUser` di PowerShell digunakan untuk mendaftar semua akun pengguna dikomputer.
## **Tujuan Lab** 
Bertindak menjadi hacker untuk menjalankan `Get-LocalUser` dan juga menjadi sebagai Defender/SOC Analyst (Menemukan jejak perintah tersebut di log windows untuk membuktikan ada aktivitas mencurigakan yang terjadi). 

## **Windows**
1. **Mengaktifkan Logging via Registry Editor (regedit).**
   - Buka **Registry Editor** melalui pencarian atau bisa lewat Windows + R ketik **(regedit)** pilih yes jika ada UAC         (User Access Control).
   - Navigasi path (jalur) satu per satu di panel sebelah kiri.
     `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows`
   - Cek terlebih dahulu apakah ada folder **PowerShell**. jika tidak ada buat.
      - Klik kanan pada folder **Windows -> New -> Key**. Beri nama **PowerShell**.
   - Klik kanan pada folder **PowerShell -> New -> Key**. Beri nama **ScriptBlockLogging**.
   - Klik pada folder **ScriptBlockLogging** yang dibuat. Di panel sebelah kanan yang kosong, klik kanan **-> New ->          DWORD (32-bit)** Value.
   - Beri nama **EnableScriptBlockLogging**.
   - Klik ganda pada `EnableScriptBlockLogging`, ubah value data menjadi `1`, lalu OK.
   - Tutup Registry Editor dan *Restart*  Komputer agar efek berjalan.
     
2. **Menajalankan Pertintah (Simulasi).**
   - Buka **PowerShell** sebagai Administrator.
   - Jalankan perintah:
     `Get-LocalUser | Select-Object Name, Enabled`
     
     digunakan untuk menampilkan informasi akun pengguna lokal di Windows.
     
     1. `Get-LocalUser`
        - Ini adalah cmdlet PowerShell untuk mengambil daftar user lokal yang ada di komputer.
        - Output-nya biasanya berisi berbagai properti seperti nama akun, status, deskripsi, dll.
        
     3. `|`
        - Simbol ini disebut pipeline.
        - Fungsinya untuk mengirim hasil dari perintah di sebelah kiri ke perintah di sebelah kanan.
        - Jadi, output dari Get-LocalUser diteruskan ke Select-Object.
        
     5. `Select-Object Name, Enabled`
        - Digunakan untuk memilih hanya properti tertentu dari output.
        - Dalam hal ini:
        - Name → nama akun user
        - Enabled → status apakah akun aktif (True) atau nonaktif (False).
        
3. **Mendetksi Log Di Event Viewer.**
   - Buka **Even Viewer** melalui pencarian atau bisa lewat Windows + R ketik **(eventvwr.msc)** pilih yes jika ada UAC       (User Access Control).
   - Masuk ke panel sebelah kiri: **Applications and Services Logs -> Microsoft -> Windows -> PowerShell ->                   Operational**.
   - Di panel sebelah kanan, klik **Filter Current Log..**
   - Di kolonm Includes/Excludes Event IDs, ketik **4104** dan klik OK.
   - Cari Log terbaru (lihat jamnya). Dibagian *General* atau *Details*, kamu akan melihat ScriptBlockText yang berisi        perintah `Get-LocalUser | Select-Object Name, Enabled`.
