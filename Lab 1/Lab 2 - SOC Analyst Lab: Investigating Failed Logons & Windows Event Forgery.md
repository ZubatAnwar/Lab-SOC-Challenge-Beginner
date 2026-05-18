#**Lab ke-2 SOC-Analyst-Lab**

# 🔍 SOC Analyst Lab: Investigating Failed Logons & Windows Event Forgery

## 🎯 Objective
Lab ini memiliki tujuan yaitu mampu mendeteksi dan menganalisis anomali aktivitas login (terutama indikasi serangan Brute Force via Event ID 4624 & 4625) menggunakan Windows Event Viewer,
serta memetakan temuan tersebut ke dalam standar ancaman keamanan global (MITRE ATT&CK)

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

## 📌 Windows Security Logs
Windows Security logs adalah catatan aktivitas keamanan yang dibuat oleh sistem operasi Microsoft Windows. Log ini disimpan di Event Viewer dan dipakai untuk memantau kejadian penting terkait keamanan komputer, seperti:

- Successful or failed logins
- Password changes
- User account creation or deletion
- Access attempts to files or resources
- Administrator activity
- Programs requesting elevated privileges
- Antivirus or firewall security events

Windows menggunakan log ini untuk membantu:

- detect suspicious activity or attacks,
- troubleshoot security issues,
- audit user activity,
- and perform forensic investigations after incidents.

## Lokasi melihat Security Logs

Bisa dibuka lewat:

1. Tekan `Win + R`

2. Ketik:

`eventvwr.msc`

3. Masuk ke:

`Windows Logs > Security`

Atau lihat dokumentasi resmi Microsoft:
Windows Event Viewer documentation
(https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/basic-security-audit-policy-settings)

## Contoh Event ID umum di Windows Security Log
- 4624 → Login berhasil (successful logon)
- 4625 → Login gagal (failed logon)
- 4634 → User log off
- 4648 → Login menggunakan kredensial eksplisit
- 4719 → Perubahan kebijakan audit
- 4720 → Akun user baru dibuat
- 4726 → Akun user dihapus
- 4732 → User ditambahkan ke grup

Dokumentasi ID Windows:
https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx

## **Praktikum:**
Untuk lab ini perlu di siapkan yaitu:
1. **Command Prompt (CMD) / PowerShell**

Fungsi: Digunakan sebagai jalan pintas (workaround) pengganti lusrmgr.msc untuk membuat user percobaan menggunakan perintah net user. Selain itu, digunakan juga untuk mengeksekusi perintah simulasi serangan gagal login (net use).

2. **Windows Event Viewer (`eventvwr.msc`)**

Fungsi: Ini adalah tool utama dalam lab ini. Digunakan untuk membaca, memfilter (mencari Event ID 4624 & 4625), dan menganalisis rekam jejak keamanan (Security Logs) pada sistem operasi Windows.

## **Penjelasan**
Jadi kita menggunakan Windows Event Viewer yaitu alat yang digunakan Windows mencatat hampir setiap kejadian penting dalam sistem operasi ke dalam *logs*.

Fokus lab ini ada pada **Security Logs** yang merekam aktivitas terkait keamanan.
- **Event ID 4624 (Successful Logon):** Mencatat ketika seseorang (atau sebuah layanan sistem) berhasil masuk. Ini penting untuk memastikan bahwa yang masuk adalah pengguna yang sah.

- **Event ID 4625 (Failed Logon):** Mencatat ketika seseorang gagal masuk karena salah password atau username tidak ada. Jika log ini muncul puluhan atau ratusan kali dalam rentang waktu singkat, ini adalah indikator kuat adanya serangan Brute Force atau tebak password.

**Catatan:** jadi lab ini tidak menggunakan `lusrmgr.msc` karena menggunakan Windows 10/11 Home, kita akan membuat user secara manual menggunakan Command Prompt (CMD) dengak hak akses Administrator.

**Langkah 1: Membuat Test User (`haxuser1`) via CMD**
1. Klik tombol **Windows🪟**, ketik `cmd`.
2. Klik kanan pada **Command Prompt**, lalu pilih **Run as administrator**.
3. Setelah itu ketik di layar samping tanda `>`:
   `net user haxuser1 PasswordKuat123! /add`
   (*Perintah ini akan membuat user baru bernama haxuser1 dengan password PasswordKuat123!. Pesan "The command completed successfully" akan muncul jika berhasil*).

🔍 Event Log yang muncul biasanya:

Kalau ini dijalankan, Windows akan mencatat:

4720 → A user account was created

**Langkah 2: Menyimulasikan Gagal Login**
1. Kamu bisa tetap menggunakan CMD atau buka PowerShell baru.
2. Masukan perintah ini untuk memaksa sistem mencoba login ke *local network* menggunakan akun tadi, tapi dengan *password* yang disengaja disalahkan:
   `net use \\127.0.0.1\IPC$ /user:haxuser1 WrongPassword`
   
🧩 Penjelasan bagian per bagian
🌐 net use

Tool Windows untuk:

- menghubungkan ke network share
- mapping drive
- akses resource jaringan (SMB)
  
🖥️ **\\127.0.0.1\IPC$**

Ini adalah target koneksi:

- `127.0.0.1` → komputer sendiri (localhost)
- `IPC$` → Inter-Process Communication share

📌 IPC$ adalah share khusus Windows untuk:

- autentikasi remote
- komunikasi admin (SMB/RPC)
- tidak berisi file biasa
👤 /user:haxuser1

Artinya:

mencoba login ke share menggunakan username haxuser1
🔑 WrongPassword

Ini adalah password yang dipakai untuk autentikasi.

artinya:
Menghubungkan ke IPC$ di komputer sendiri menggunakan akun haxuser1 dengan password yang diberikan.

Apa yang terjadi di sistem?

Biasanya hasilnya salah satu:

❌ Jika password salah:
- akses ditolak
- koneksi gagal
✅ Jika benar:
- koneksi SMB berhasil dibuat
- session login tercatat

🛡️ Event Log yang muncul

Di Windows Security Log biasanya:

4625 → Failed logon (kalau password salah)
4624 → Successful logon (kalau benar)

enapa IPC$ sering dipakai di dunia keamanan?

Perintah seperti ini sering muncul dalam konteks:

🔎 Penyerang (attacker) mencoba:
- brute force SMB login
- validasi username/password
- lateral movement di jaringan
- akses admin share (ADMIN$, C$ juga sering)

🧩 Jadi, apa itu IPC$?
🔐 IPC$ (Inter-Process Communication share)
- Ini adalah hidden administrative share
- Dipakai Windows untuk komunikasi antar proses lewat jaringan (SMB/RPC)
- Tidak digunakan untuk simpan file seperti folder biasa

📌 Disebut:

Special Share / Administrative Share / Null Session Interface (dalam konteks lama)

🧠 “Ada berapa jenis IPC$?”

👉 Jawaban yang benar:

❌ IPC$ tidak punya “jenis-jenis”
✅ IPC$ hanya 1 jenis share khusus
🧱 Tapi dalam praktik Windows ada beberapa “administrative shares” yang sering disalahartikan sebagai satu kelompok:
Share	Fungsi
- `IPC$`	-> komunikasi antar proses (SMB/RPC)
- `ADMIN$`	-> akses remote ke folder Windows system
- `C$`	-> akses drive C (hidden admin share)
- `D$ (dll)`	-> drive lain (kalau ada)

📌 Semua ini disebut:

Administrative Shares

🔥 Fungsi utama IPC$

IPC$ digunakan untuk:

- autentikasi remote (SMB login)
- RPC (Remote Procedure Call)
- enumerasi user & group
- koneksi admin tools (seperti net use, wmic, dll)
🚨 Kenapa IPC$ sering dibahas di security?

Karena sering dipakai dalam:

- brute force SMB login
- lateral movement (pindah antar mesin)
- enumerasi user/domain
- attack tools (PsExec, mimikatz, dll)

Nama resminya:
👉 Windows IPC Share (SMB Inter-Process Communication)
Termasuk kategori:
👉 Administrative Shares / Hidden Shares

3. Tekan Enter. Kamu akan melihat pesan error seperti "System error 86 has occurred. The specified network password is not correct." Ini berarti simulasi serangan gagal login kita sukses terekam.
