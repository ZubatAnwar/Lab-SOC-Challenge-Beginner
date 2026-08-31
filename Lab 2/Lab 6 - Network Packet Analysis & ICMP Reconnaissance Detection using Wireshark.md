#**Lab ke-6 Network Packet Analysis & ICMP Reconnaissance Detection using Wireshark**

## **1. Penjelasan Materi: Wireshark & MITRE ATT&CK**

**Apa itu Wireshark?**

Wireshark adalah penganalisis paket jaringan. Penganalisis paket jaringan menyajikan data paket yang ditangkap sedetail mungkin.
Anda dapat menganggap penganalisis paket jaringan sebagai alat ukur untuk memeriksa apa yang terjadi di dalam kabel jaringan, sama seperti seorang teknisi listrik menggunakan voltmeter untuk memeriksa apa yang terjadi di dalam kabel listrik (tetapi pada tingkat yang lebih tinggi, tentu saja).
Dahulu, perangkat lunak semacam itu sangat mahal, bersifat eksklusif, atau keduanya. Namun, dengan munculnya Wireshark, hal itu telah berubah. Wireshark tersedia secara gratis, bersifat open source, dan merupakan salah satu penganalisis paket terbaik yang tersedia saat ini.

<img width="755" height="406" alt="image" src="https://github.com/user-attachments/assets/1a892de3-2f33-4857-9559-e8d8c46912c8" />

*Antarmuka Utama Wireshark. Source: NetworkProGuide*

(*https://www.wireshark.org/docs/wsug_html_chunked/ChapterIntroduction.html*)

Apa bedanya Capture Filter dan Display Filter?

 - Capture Filter: Diterapkan sebelum Anda mulai menangkap lalu lintas jaringan. Tujuannya membatasi data apa saja yang disimpan ke memori/disk (menghemat ruang). Sintaksnya menggunakan format BPF, contoh: icmp.

 - Display Filter: Diterapkan setelah atau selama proses capture berjalan. Data aslinya tetap terekam semua, namun layar hanya menampilkan paket yang sesuai filter. Sintaksnya berbeda, contoh: icmp.

Apa itu MITRE ATT&CK?

MITRE ATT&CK merujuk pada sekelompok taktik yang disusun dalam bentuk matriks, yang menguraikan berbagai teknik yang digunakan oleh pemburu ancaman, pembela, dan tim merah untuk menilai risiko terhadap suatu organisasi dan mengklasifikasikan serangan. Pemburu ancaman mengidentifikasi, menilai, dan mengatasi ancaman, sementara tim merah bertindak seperti pelaku ancaman untuk menantang sistem keamanan TI.

Apa tujuan dari kerangka kerja ATT&CK?

Tujuan dari kerangka kerja MITRE ATTACK adalah untuk memperkuat langkah-langkah yang diambil setelah suatu organisasi mengalami pelanggaran keamanan. Dengan cara ini, tim keamanan siber dapat menjawab pertanyaan-pertanyaan penting mengenai bagaimana penyerang mampu menembus sistem dan apa yang mereka lakukan setelah berhasil masuk. Seiring waktu, informasi akan terkumpul dan terbentuk basis pengetahuan. Ini berfungsi sebagai alat yang terus berkembang yang dapat digunakan tim untuk memperkuat pertahanan mereka. Dengan menggunakan laporan yang dihasilkan oleh MITRE ATT&CK, suatu organisasi dapat mengetahui di mana arsitektur keamanannya memiliki kerentanan dan menentukan mana yang harus diperbaiki terlebih dahulu, sesuai dengan risiko yang ditimbulkannya.

(*https://www.fortinet.com/resources/cyberglossary/mitre-attck*)

Dalam skenario lab yang melibatkan pencarian lalu lintas ICMP (Ping), aktivitas ini biasanya dilakukan penyerang untuk memetakan jaringan (Network Reconnaissance). Dalam MITRE ATT&CK, hal ini masuk ke dalam:

 - Tactic: Discovery (TA0007)

 - Technique: Network Service Discovery (T1046)

## 🧪 **Eksekusi Lab**

**1. Buat Profile Baru "SOC Analyst"**

Lihat sudut kanan bawah jendela Wireshark, klik kanan terus ada tulisan **Profile: Default** lalu pilih **Manage Profiles**. Klik ikon **(+)**, ketik nama "SOC Analyst", lalu pilih OK. Fitur ini berguna untuk bisa menyimpan kustomisasi kolom (Misal: Menambahkan kolom *Source Port* dan *Dest Port*).

**2. Buat Capture Filter untuk ICMP"**

Sebelum memulai *Capture*, pada layar awal Wireshark tempat memilih *Interface* (Seperti Wi-FI atau Ethernet), ada kolom panjang di atasnya. Ketikan kata **icmp** dikolom tersebut hingga kotaknya berubah warna menjadi hijau.

**3. Buat Display Filter untuk ICMP"**

Mulai *Capture* dengan mengklik *Interface* anda. Buka Command Prompt (CMD) di komputer anda. lalu jalankan perintah ping 8.8.8.8 agar ada trafik ICMP yang lewat. di Wireshark, pada kolom filter di bagian atas, ketikan icmp lalu tekan Enter.

<img width="391" height="255" alt="image" src="https://github.com/user-attachments/assets/c938585d-e53c-4794-95fe-d9b28e3ec450" />

*Contoh Hasil Display Filter ICMP. Source: Wireshark Wiki*

