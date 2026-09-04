#**Lab ke-7 SOC Analyst Investigation: ICMP Protocol Analysis & Reconnaissance Detection using Wireshark**

## **📚 Bagian 1: Penjelasan Materi (ICMP & Wireshark)**

Internet Control Message Protocol (ICMP) adalah rangkaian aturan komunikasi yang digunakan perangkat untuk mengomunikasikan kesalahan transmisi data dalam jaringan. Dalam pertukaran pesan antara pengirim dan penerima, kesalahan yang tidak terduga dapat terjadi. Misalnya, pesan mungkin saja terlalu panjang atau paket data mungkin saja tiba secara tidak berurutan sehingga penerima tidak dapat menyusunnya.
Dalam kasus tersebut, penerima menggunakan ICMP untuk memberi tahu pengirim melalui pesan kesalahan dan permintaan agar pesan dikirim ulang.

Contoh paling umum dari penggunaan ICMP adalah perintah ping. Ketika Anda melakukan `ping` ke sebuah komputer, inilah yang terjadi:

- Komputer Anda mengirim ICMP Echo Request (Type 8).

- Komputer tujuan menerima pesan itu dan membalas dengan ICMP Echo Reply (Type 0).

Struktur Data ICMP:

- Type: Menentukan jenis pesan (Misal: 8 = Request, 0 = Reply, 3 = Destination Unreachable).

- Code: Memberikan detail spesifik dari Type.

- Checksum: Digunakan untuk mengecek apakah paket rusak selama pengiriman.

- Identifier & Sequence No: Membantu mencocokkan Request mana yang dibalas oleh Reply mana (sangat berguna jika Anda mengirim banyak ping sekaligus).

(*https://aws.amazon.com/id/what-is/icmp/*)

## **🛠️ Bagian 2: Panduan Menyelesaikan Lab**

1. **Buka Wireshark:** Jalankan aplikasi Wireshark dan buka file Sample PCAP yang telah Anda unduh.

2. **Gunakan Filter:** Di kolom bar "Apply a display filter" (bagian atas), ketik icmp lalu tekan Enter. Ini akan menyembunyikan semua lalu lintas selain ICMP.

3. **Analisis Paket Echo Request:**

- Klik pada salah satu baris yang memiliki info Echo (ping) request.

- Di panel tengah (Packet Details), buka bagian Internet Control Message Protocol.

- Perhatikan nilai Type (8).

4. **Analisis Paket Echo Reply:**

- Klik pada baris di bawahnya yang berisi Echo (ping) reply.

- Di panel tengah, lihat nilai Type (0).

Perhatikan layar Wireshark Anda yang menunjukkan dengan jelas adanya baris Echo Request, Echo Reply, dan panel detail yang terbuka. Ini adalah hasil akhir (submission) Anda.
