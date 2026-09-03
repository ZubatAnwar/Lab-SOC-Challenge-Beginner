#**Lab ke-7 SOC Analyst Investigation: ICMP Protocol Analysis & Reconnaissance Detection using Wireshark**

## **📚 Bagian 1: Penjelasan Materi (ICMP & Wireshark)**

Protokol Pesan Kontrol Internet (ICMP) adalah rangkaian aturan komunikasi yang digunakan perangkat untuk mengomunikasikan kesalahan transmisi data dalam jaringan. Dalam pertukaran pesan antara pengirim dan penerima, kesalahan yang tidak terduga dapat terjadi. Misalnya, pesan mungkin saja terlalu panjang atau paket data mungkin saja tiba secara tidak berurutan sehingga penerima tidak dapat menyusunnya.
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
