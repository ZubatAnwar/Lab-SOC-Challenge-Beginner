#Lab 12 - Practical Incident Response: Detecting and Remediating Linux Shell Payloads

## 📘 **Bagian 1: Penjelasan Incident response**

Incident response adalah proses yang dirancang untuk menangani pelanggaran data atau serangan siber dengan tujuan meminimalkan dampaknya. 
Proses ini melibatkan langkah-langkah untuk mengendalikan insiden, memulihkan sistem, dan mencegah insiden serupa di masa depan. Dengan incident response yang efektif, organisasi dapat:

- Mengurangi kerugian finansial.
- Menutup celah kerentanan yang dimanfaatkan oleh penyerang.
- Memulihkan sistem dengan cepat.
- Menjaga reputasi organisasi di mata pelanggan dan mitra bisnis.

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
- **Execution (TA0002)** - Command and Scripting Interpreter: Unix Shell (T1059.004): Penyerang mengeksekusi payload berbahaya menggunakan interpreter default /bin/bash atau .sh.

- **Defense Evasion (TA0005)** - Hide Artifacts: Hidden Window (T1564.003): Penyerang menjalankan script di background menggunakan argumen & agar jendela eksekusi terminal tidak muncul secara interaktif.

- **Persistence (TA0003)** - Scheduled Task/Job: Cron (T1053.003): Analis diwajibkan melakukan pengecekan crontab untuk memastikan penyerang tidak membuat tugas terjadwal (cron jobs) untuk menjalankan payload berulang kali.

🛠️ Topologi & Persyaratan Lab
- **Attacker Machine:** Ubuntu Server

(*Catatan: Disini saya pakai ubuntu server ini terserah kalian pakai OS apa aja*)

- **Target Machine:** Windows 10 / 11


🎯Simulasikan Serangan

**Tahap 1: Preparation (Persiapan Tool)**

`sudo apt update
sudo apt install curl lsof procps -y`

**Tahap 2: Simulasi Serangan (Attack Simulation)**

`mkdir ~/script-lab && cd ~/script-lab
nano fakebackup.sh`

Salin *script* berikut ke dalam editor nano.

`#!/bin/bash
echo "[*] Simulating backup operation..."
sleep 600`

Simpan file tersebut (tekan Ctrl+O, Enter, Ctrl+X). Lalu berikan hak eksekusi dan jalankan di background:

`chmod +x fakebackup.sh
./fakebackup.sh &`

**Tahap 3: Detection and Analysis (Deteksi & Analisis)**

Sekarang Anda berganti peran sebagai Analis SOC. Mari cari tahu script apa yang diam-diam berjalan di latar belakang:

`ps aux | grep fakebackup.sh`

Anda akan melihat output nama sistem, angka Process ID (PID), dan script-nya. Catat angka PID (biasanya di kolom kedua) dari script bash tersebut.

Kemudian, cari tahu di direktori mana persisnya file ini bersembunyi (jika file itu tertanam di server aslinya):

`find / -name "fakebackup.sh" 2>/dev/null`

*Note: 2>/dev/null akan menyembunyikan error "Permission denied" sehingga output jauh lebih rapi.*

**Tahap 4: Containment & Eradication (Isolasi & Pemusnahan)**

Containment: Matikan paksa (Terminasi) proses dari script yang menginfeksi menggunakan angka PID yang sudah Anda catat.

`kill -9 <GANTI_DENGAN_ANGKA_PID>`

(Verifikasi kembali dengan menjalankan ps aux | grep fakebackup.sh untuk memastikan sudah tidak ada lagi).

Eradication: Pemusnahan sumber masalah. Kita hapus file malicious tersebut!

`rm -f ~/script-lab/fakebackup.sh`

<img width="788" height="446" alt="Cuplikan layar 2026-10-07 205436" src="https://github.com/user-attachments/assets/b0267f06-6805-4369-bc35-20e4262e3cb3" />

**Tahap 5: Recovery (Verifikasi Persistence)**

Kita harus mengecek apakah penyerang menggunakan crontab agar malware bisa "hidup kembali" saat sistem direstart.

`crontab -l
sudo crontab -l`

<img width="788" height="446" alt="Cuplikan layar 2026-10-07 205436" src="https://github.com/user-attachments/assets/7ea42f0e-998f-4b63-b9ff-cd5072c2c508" />


**Referensi Lab:**

(*https://github.com/0xrajneesh/30-Days-SOC-Challenge-Beginner/blob/main/Challenge%233/Day%2311-%20Introduction%20to%20Incident%20Response.md*)
