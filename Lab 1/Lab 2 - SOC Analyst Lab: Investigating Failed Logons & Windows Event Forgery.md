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

Ketik:

`eventvwr.msc`

Masuk ke:

`Windows Logs > Security`

Atau lihat dokumentasi resmi Microsoft:
Windows Event Viewer documentation
(https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/basic-security-audit-policy-settings)
