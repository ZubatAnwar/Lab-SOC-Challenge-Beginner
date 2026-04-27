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


