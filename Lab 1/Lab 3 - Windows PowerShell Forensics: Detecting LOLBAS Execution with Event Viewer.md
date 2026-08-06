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

* **Tactic: Execution (TA0002)**

Penjelasan: Attacker mencoba menjalankan kode berbahaya di sistem target.

* **Technique: Command and Scripting Interpreter - PowerShell (T1059.001)**

Penjelasan: Attacker menggunakan PowerShell untuk mengeksekusi perintah (seperti perintah `Start-Process` di lab ini).

* **Tactic: Defense Evasion (TA0005)**

Penjelasan: Attacker berusaha menghindari deteksi keamanan

* **Technique: Masquerading / Obfuscated Files or Information (T1027) atau penggunaan LOLBAS**

Penjelasan: Menggunakan tools sah (seperti PowerShell atau certutil) agar terlihat seperti aktivitas sistem normal

* **Mitigation & Detection (Sisi Defender):**

**Data Source:** Command Execution (DS0017). Menganalisis Event ID 4104 (Script Block Logging) dan 4103 (Module Logging) untuk menginspeksi argumen command-line yang mencurigakan

## **Praktikum:**
Untuk lab ini perlu di siapkan yaitu:
1. **Command Prompt (CMD) / PowerShell**

Fungsi: Digunakan sebagai jalan pintas (workaround) pengganti lusrmgr.msc untuk membuat user percobaan menggunakan perintah net user. Selain itu, digunakan juga untuk mengeksekusi perintah simulasi serangan gagal login (net use).

2. **Windows Event Viewer (`eventvwr.msc`)**

Fungsi: Ini adalah tool utama dalam lab ini. Digunakan untuk membaca, memfilter (mencari Event ID 4624 & 4625), dan menganalisis rekam jejak keamanan (Security Logs) pada sistem operasi Windows.

## **Eksekusi Lab**

Jadi kita tidak menggunakan **Group Policy Editor (gpedit.msc)**, konfigurasi logging akan diaktifkan secara manual di **Registry Editor**.

Sekarang buka PowerShell di VM jalankan sebagai **Administrator** lalu ikuti perintah dibawah ini. Tapi kok pakai PowerShell jadi ini bisa jalankan di Registry Editor (versi GUI) atau (versi CLI) nanti hasilnya juga tetap sama.

**Cara 1: PowerShell**
1. Mengaktifkan Script Block Logging
   
   `New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" -Force`

   `Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" -Name "EnableScriptBlockLogging" -Value 1`

3. Mengaktifkan Module Logging
   
   `New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ModuleLogging" -Force`

   `Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ModuleLogging" -Name "EnableModuleLogging" -Value 1`

**Cara 2: Registry Editor**

1. Buka Registry Editor
2. Masuk ke path `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\PowerShell`
3. Lalu klik kanan pada folder PowerShell -> New -> Key -> namai **ScriptBlockLogging / ModuleLogging**
4. Pilih ScriptBlockLogging -> samping kanan ada ruang kosong klik kanan -> New -> **DWORD (32-bit) value** -> namai **EnableScriptBlockLogging / EnableModuleLogging** -> klik kanan pada lagi -> Modify -> Value nya diganti **1**
5. Buat lagi untuk **Module Logging** sama seperti **ScriptBlockLogging**.

*Setelah perintah ini dijalankan, sistem Windows 10 Home kamu sudah mulai merekam log PowerShell secara detail.*

**Panduan Eksekusi**

**1. Eksekusi Perintah (Simulasi Attacker):**
Di PowerShell (Admin), jalankan perintah lab-nya:

`Start-Process "notepad.exe" -ArgumentList "C:\Windows\System\drivers\etc\hosts"`

<img width="854" height="256" alt="image" src="https://github.com/user-attachments/assets/bce1c691-51f8-43b9-b454-30d34a4624e1" />


**2. Investigasi (Simulasi Defender):**
- Buka Menu Start / Ikon Windows, ketik `Event Viewer`, lalu buka.
- Navigasi ke direktori: `Applications and services Logs -> Microsoft -> Windows -> PowerShell -> Operational.`
- Klik menu **"Filter Current Log"** disebelah kanan, dan masukan angka `4103` (atau `4104`) dikolom *Event ID*.
- Cari *log* terbaru. Kamu akan melihat detail *Script* persis ang baru saja kamu jalankan, lengkap dengan waktu dan nama user.

<img width="1023" height="708" alt="image" src="https://github.com/user-attachments/assets/20de92af-a3be-44f1-b392-851f6f2582d8" />

Tapi kok mengapa EventID 4104 yang muncul sedangkan 4103 tidak ini karean terletak ada cara kerja **Module Logging** di Windows.

Mengapa Event ID 4103 Tidak Muncul?
- **Event ID 4104 (Script Block Logging):** Sifatnya seperti saklar lampu. Begitu diaktifkan (seperti yang       kita lakukan dengan perintah registry sebelumnya), ia akan langsung mencatat semua script yang               dieksekusi.

- **Event ID 4103 (Module Logging):** Sifatnya lebih spesifik. Mengubah nilai `EnableModuleLogging` menjadi `1` saja   tidak cukup. Windows membutuhkan instruksi tambahan: "Modul PowerShell mana saja yang harus saya catat?".Karena kita belum mendefinisikan nama modulnya, Windows tidak mencatat apa-apa.

**Cara Memperbaiki agar Event 4103 Muncul (Tanpa gpedit.msc)**
Agar EventID 4103 mencatat semua eksekusi modul (*Pipeline Execution Details*), kita harus menambahkan *key* `ModuleNames` di dalam Registry dan memberinya nilai `*` (Karakter *wildcard* yang berarti "Semua Modul").

Buka kembali **PowerShell sebagai Administrator** dan jalankan dua baris perintah ini:

4. 1. Membuat folder/key khusus untuk daftar nama modul
   
   `New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ModuleLogging" -Force`
   
   2. Menambahkan tanda bintang (*) yang menginstruksikan Windows untuk mencatat SEMUA modul
   
   `Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ModuleLogging" -Name "*" -Value "*"`
   <img width="753" height="515" alt="image" src="https://github.com/user-attachments/assets/9a4e99ea-471b-4686-901c-3738dc3aa2a6" />


**Langkah Pengujian Ulang:**
1. **Tutup jendela PowerShell** yang sedang terbuka. (ini sangat penting agar PowerShell memuat ulang konfigurasi *registry* yang baru).
2. Buka jendela **PowerShell baru**.
3. Jalankan kembali perintah serangan lab kita:
   `Start-Process "notepad.exe" -ArgumentList "C:\Windows\System32\drivers\etc\hosts"`
4. Buka **Event Viewer** -> `Applications and services Logs -> Microsoft -> Windows -> PowerShell -> Operational.`
5. Klik **Refresh** di panel kanan.
Sekarang, jika mencari **EventID 4103**, *log* tersebut pasti muncul.

<img width="1021" height="711" alt="image" src="https://github.com/user-attachments/assets/6a08182a-59ac-4fe2-bc21-004a20ff36b0" />


**Catatan:**
Perbedaan Keduannya:
- *"Event 4104 menunjukkan apa kode asli yang dieksekusi (berguna jika attacker mencoba menyembunyikan kodenya)."* Artinya **Mencatat keseluruhan blok skrip PowerShell yang dijalankan. Sangat krusial untuk mendeteksi payload berbahaya karena fitur ini dapat mencatat script yang telah di-decode secara otomatis meskipun attacker menggunakan teknik obfuscation (seperti Base64).**

- *"Event 4103 menunjukkan bagaimana perintah itu diproses oleh Windows beserta parameternya (berguna untuk melihat hasil akhir dari eksekusi tersebut)."* artinya **Mencatat proses eksekusi perintah secara spesifik beserta argumen/parameter yang mengikutinya (Pipeline Execution Details). Sangat berguna bagi analis untuk melihat hasil evaluasi akhir dari sebuah variabel atau argumen yang dituju oleh attacker.**
