#Lab ke-8 SOC Analyst Lab: TCP Protocol Deep Dive & Threat Telemetry Inspection

📚 **1. Penjelasan Materi: Cara Kerja TCP dan Wireshark**

Transmission Control Protocol adalah protokol komunikasi yang dirancang untuk memastikan data dapat dikirim secara andal dari satu perangkat ke perangkat lain melalui jaringan. TCP bekerja dengan cara membagi data berukuran besar menjadi paket-paket kecil agar lebih mudah dikirim melalui jaringan. Setiap paket tersebut kemudian diberi nomor urut sehingga perangkat penerima dapat menyusunnya kembali menjadi data yang utuh.

(*https://dqlab.id/apa-itu-tcp-dan-ip-penjelasan-dasar-protokol-yang-mengatur-komunikasi-internet*)

The 3-Way Handshake (Proses Jabat Tangan)
  Sebelum aplikasi bisa saling bertukar data, TCP mewajibkan kedua komputer melakukan "jabat tangan" untuk menyepakati koneksi.
  
  1. SYN (Synchronize): Komputer A mengirim paket dengan flag SYN ke Komputer B. (Artinya: "Halo, saya ingin membuka koneksi dengan Anda.")
  
  2. SYN-ACK (Synchronize-Acknowledge): Komputer B membalas dengan paket ber-flag SYN dan ACK. (Artinya: "Halo juga, saya menerima permintaanmu, dan saya juga siap membuka koneksi.")
  
  3. ACK (Acknowledge): Komputer A mengirim kembali paket ACK. (Artinya: "Baik, mari kita mulai bertukar data.")

TCP Flags (Bendera Kontrol)
Di dalam setiap paket TCP, terdapat sekumpulan bit yang berfungsi sebagai "bendera" untuk memberi tahu status paket tersebut:

- SYN (Synchronize): Memulai koneksi.

- ACK (Acknowledge): Mengonfirmasi penerimaan data.

- FIN (Finish): Menutup koneksi secara damai dan normal.

- RST (Reset): Memutus koneksi secara paksa (sering terjadi jika port tertutup atau ada gangguan firewall).

- PSH (Push): Meminta agar data segera dikirim ke aplikasi tanpa harus menunggu buffer penuh.
- 
(*https://www.cloudns.net/blog/tcp-transmission-control-protocol-what-is-it-and-how-does-it-work/*)
