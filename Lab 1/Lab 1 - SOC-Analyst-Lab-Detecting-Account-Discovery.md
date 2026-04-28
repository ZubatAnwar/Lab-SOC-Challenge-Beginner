#**Lab ke-1 SOC-Analyst-Lab-Detecting-Account-Discovery**

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

## 📝 Methodology
1. **Configuration:** Mengaktifkan `EnableScriptBlockLogging` melalui Registry Editor untuk merekam eksekusi PowerShell di tingkat sistem.
2. **Simulation:** Menjalankan `Get-LocalUser` untuk mensimulasikan fase *Discovery* dari serangan.
3. **Detection:** Memfilter *PowerShell Operational Logs* mencari **Event ID 4104** untuk menemukan *ScriptBlockText* yang cocok dengan perintah simulasi.

## 📸 Artifacts / Proof of Concept
(Masukkan screenshot Event Viewer kamu di sini yang menampilkan Event ID 4104)

## 💡 Conclusion
Pemantauan *Event ID 4104* sangat krusial dalam *digital forensics* karena memungkinkan *Security Analyst* untuk melihat perintah PowerShell apa saja yang dieksekusi oleh pengguna atau proses latar belakang, bahkan ketika file *script* sudah dihapus.

##📌 Pengertian Log
Log dalam konteks keamanan siber (security) adalah catatan digital yang terstruktur, kronologis, dan dihasilkan secara otomatis mengenai aktivitas, peristiwa, serta transaksi yang terjadi di dalam sistem komputer, jaringan, atau aplikasi bisa juga dikenal sebagai log peristiwa keamanan adalah catatan digital dari aktivitas dan peristiwa sistem, seperti upaya login, perubahan kebijakan, dan akses ke data sensitif. Log ini dikumpulkan dari berbagai sumber seperti basis data, server, workstation, dan firewall, dan memberikan gambaran kronologis tentang apa yang terjadi dalam sistem.

##🧠 Fungsi Log

Log digunakan untuk:

1. Debugging (memperbaiki error)
Membantu developer mengetahui apa yang salah dalam program.
2. Monitoring sistem
Melihat apakah sistem berjalan normal atau tidak.
3. Audit (jejak aktivitas)
Mencatat aktivitas user atau sistem (siapa melakukan apa).
4. Analisis performa
Mengetahui bottleneck atau masalah performa.

##📊 Contoh Format Log Windows (Umum)
Biasanya log Windows memiliki struktur seperti ini:
```
Date: 28/04/2026
Time: 10:15:32
Level: Error / Information / Warning
Source: Service Name
Event ID: 1234
Message: Deskripsi kejadian
```
##💻 Contoh Log di Linux (tambahan untuk perbandingan)
Contoh log linux ada di:
📁 /var/log/
```
Apr 28 10:15:32 server sshd[1234]: Failed password for root
Apr 28 10:16:10 server kernel: CPU temperature high
```
