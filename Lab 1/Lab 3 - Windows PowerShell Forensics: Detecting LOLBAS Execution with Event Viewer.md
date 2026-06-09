#**Lab ke-3 SOC-Analyst-Lab**

# 🔍 Windows PowerShell Forensics: Detecting LOLBAS Execution with Event Viewer

## 🎯 Objective
Lab ini memiliki tujuan yaitu Mencari tahu apa yang sebenarnya sedang diketik dan dijalankan oleh seorang *attacker* di **PowerShell**. tapi secara default **Windows** tidak mencatat setiap perintah di PowerShell yang diketik secara detail. oleh karena itu kita akan mengaktifkan **Script Block Logging** (sama seperti Lab 1) dan **Module Logging**, jadi *script* yang dijalankan/dieksekusi akan tercatat di **Event Viewer** pada EventID **4104** dan **4103**. Lab ini berfokus pada teknik **Blue Team (Defender)** jadi **attacker** sering menyalahgunakan program bawaan **Windows** yang sah seperti **PowerShell** untuk menjalankan perintah berbahaya, mengunduh malware atau mencuri data. Teknik ini disebut **LOLBAS(Living Off The Land Binaries)** Keutungan dari *attacker* ini tidak perlu mengunduh *tools* tambahan yang mudah terdeteksi oleh **Antivirus**

**EventID:4104**
1. Disebut PowerShell Script Block Logging
2. Mencatat isi script PowerShell yang benar-benar dieksekusi
3. Ini lebih detail dibanding 4103
Bisa menampilkan:
- script satu baris (one-liner)
- script obfuscated (setelah didekode)
- command download payload (misalnya DownloadString, IEX)

👉 Contoh yang sering terlihat di kasus serangan:

- IEX (New-Object Net.WebClient).DownloadString(...)
- script encoding / Base64
- perintah bypass security

**EventID:4103**
1. Disebut PowerShell Module Logging
2. Mencatat modul atau command PowerShell yang dijalankan
3. Fokusnya: apa perintah/modul yang dipanggil
4. Biasanya dipakai untuk monitoring aktivitas PowerShell tingkat pipeline

👉 Contoh:

- `Get-Process`
- `Invoke-Command`
- pemanggilan modul tertentu dalam script

## 🛠️ Tools & Environment
* OS: Windows 10/11 (Home Edition)
* Tools: Cmd (Command Prompt) atau PowerShell dan Windows Event Viewer

## 🛡️ MITRE ATT&CK Mapping
Aktivitas yang disimulasikan dalam lab ini dipetakan ke dalam framework MITRE ATT&CK:

* **Tactic:** Credential Access (TA0006)

Penjelasan: Penyerang mencoba mendapatkan username dan password yang sah untuk masuk ke dalam sistem.

* **Technique:** Brute Force (T1110)

Sub-Technique: Password Guessing (T1110.001)

Penjelasan: Dalam lab ini, mencoba login berulang kali dengan password yang salah (WrongPassword) adalah representasi dari teknik password guessing.

## **Praktikum:**
Untuk lab ini perlu di siapkan yaitu:
1. **Command Prompt (CMD) / PowerShell**

Fungsi: Digunakan sebagai jalan pintas (workaround) pengganti lusrmgr.msc untuk membuat user percobaan menggunakan perintah net user. Selain itu, digunakan juga untuk mengeksekusi perintah simulasi serangan gagal login (net use).

2. **Windows Event Viewer (`eventvwr.msc`)**

Fungsi: Ini adalah tool utama dalam lab ini. Digunakan untuk membaca, memfilter (mencari Event ID 4624 & 4625), dan menganalisis rekam jejak keamanan (Security Logs) pada sistem operasi Windows.

## **Penjelasan**
