#Lab 11 - Incident Response Lab: RDP Brute Force Detection & Mitigation using Wazuh SIEM and Windows Event Logs

## 📘 **Bagian 1: Penjelasan Incident response**

Incident response adalah proses yang dirancang untuk menangani pelanggaran data atau serangan siber dengan tujuan meminimalkan dampaknya. 
Proses ini melibatkan langkah-langkah untuk mengendalikan insiden, memulihkan sistem, dan mencegah insiden serupa di masa depan. Dengan incident response yang efektif, organisasi dapat:

- Mengurangi kerugian finansial.
- Menutup celah kerentanan yang dimanfaatkan oleh penyerang.
- Memulihkan sistem dengan cepat.
- Menjaga reputasi organisasi di mata pelanggan dan mitra bisnis.
- 
Namun, untuk mencapai tujuan ini, organisasi harus memiliki incident response plan yang jelas dan terstruktur.

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
