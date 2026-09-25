#Lab 11 - Incident Response Lab: RDP Brute Force Detection & Mitigation using Wazuh SIEM and Windows Event Logs

## 📘 **Bagian 1: Penjelasan Incident response**

Incident response adalah proses yang dirancang untuk menangani pelanggaran data atau serangan siber dengan tujuan meminimalkan dampaknya. 
Proses ini melibatkan langkah-langkah untuk mengendalikan insiden, memulihkan sistem, dan mencegah insiden serupa di masa depan. Dengan incident response yang efektif, organisasi dapat:

- Mengurangi kerugian finansial.
- Menutup celah kerentanan yang dimanfaatkan oleh penyerang.
- Memulihkan sistem dengan cepat.
- Menjaga reputasi organisasi di mata pelanggan dan mitra bisnis.
  
Namun, untuk mencapai tujuan ini, organisasi harus memiliki incident response plan yang jelas dan terstruktur.

Lab ini akan menyimulasikan serangan Brute Force pada layanan Remote Desktop Protocol (RDP) dan bagaimana seorang analis SOC merespons insiden tersebut menggunakan kerangka kerja Incident Response Plan (PICERL: Preparation, Identification, Containment, Eradication, Recovery, Lessons Learned) yang terintegrasi dengan SIEM Waz

## 🔑 **Tahapan dalam Incident Response Plan**
Sebuah incident response plan yang efektif biasanya terdiri dari tujuh tahapan berikut:

- **Deteksi Awal:** Tahap pertama adalah mendeteksi adanya insiden. Teknologi seperti Security Information and Event Management (SIEM) membantu mendeteksi aktivitas mencurigakan dan memberikan peringatan kepada tim keamanan.
- **Analisis:** Setelah insiden terdeteksi, tim analisis akan mengidentifikasi ancaman dan memverifikasi kebenarannya. Mereka juga menyelidiki indikator ancaman (Indicators of Compromise/IoC) untuk memahami skala masalah.
- **Prioritas:** Tidak semua insiden memiliki dampak yang sama. Tim harus menentukan prioritas berdasarkan seberapa besar insiden tersebut memengaruhi bisnis dan aset penting organisasi.
- **Pemberitahuan:** Tim keamanan harus segera memberitahu pihak-pihak yang relevan di dalam organisasi. Jika diperlukan, pelanggan, mitra bisnis, atau regulator juga harus diinformasikan, terutama jika insiden melibatkan pelanggaran data besar.
- **Penanganan dan Forensik:** Pada tahap ini, langkah-langkah diambil untuk menghentikan ancaman, mencegah penyebaran lebih lanjut, dan mengumpulkan bukti forensik untuk investigasi atau tindakan hukum.
- **Pemulihan:** Setelah ancaman teratasi, tim harus memulihkan sistem, baik dengan membersihkan malware, merestorasi data dari backup, atau memperbarui perangkat lunak agar sistem kembali berjalan normal.
- **Evaluasi Insiden:** Tahap terakhir adalah mengevaluasi seluruh proses penanganan. Tujuannya adalah untuk memahami apa yang berjalan dengan baik, apa yang perlu diperbaiki, dan bagaimana mencegah insiden serupa di masa depan.

(*https://csirt.or.id/pengetahuan-dasar/apa-itu-incident-response*)

🗺️ MITRE ATT&CK Framework Mapping
- **Tactic:** Credential Access (TA0006) | Initial Access (TA0001)

- **Technique:** Brute Force (T1110) | Valid Accounts (T1078)

- **Sub-Technique:** Password Guessing (T1110.001) | Local Accounts (T1078.003)

🛠️ Topologi & Persyaratan Lab
- **Attacker Machine:** Ubuntu Server (Dilengkapi Hydra & rockyou.txt)

- **Target Machine:** Windows 10 / 11 (RDP Aktif)

- **Monitoring/SIEM:** Wazuh Manager & Agent

📶Jaringan:
- Pastikan kedua mesin terhubung ke jaringan yang sama.

⚙️Langkah-langkah Persiapan
1. Pada Windows :
Aktifkan RDP :
`System Properties → Remote → Enable Remote Desktop`

2. Izinkan RDP di Firewall :
`Windows Defender Firewall → Advanced Settings → Inbound Rules → Remote Desktop (TCP-In) → Enable`

🎯Simulasikan Serangan



**Referensi Lab:**
(*https://github.com/0xrajneesh/30-Days-SOC-Challenge-Beginner/blob/main/Challenge%233/Day%2311-%20Introduction%20to%20Incident%20Response.md*)
