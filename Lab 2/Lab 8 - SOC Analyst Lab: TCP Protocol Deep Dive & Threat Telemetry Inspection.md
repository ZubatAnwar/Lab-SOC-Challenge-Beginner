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

(*https://www.cloudns.net/blog/tcp-transmission-control-protocol-what-is-it-and-how-does-it-work/*)

### **Key TCP Fields:**

| Field Name           | Description                                  |
|----------------------|----------------------------------------------|
| **Source Port**      | Nomor port pengirim                          |
| **Destination Port** | Nomor port penerima                          |
| **Sequence Number**  | Nomor byte pertama dalam segmen              |
| **Acknowledgment No**| Mengonfirmasi data yang telah diterima       |
| **Flags**            | Bit kontrol (SYN, ACK, FIN, RST, PSH, URG)  |
| **Window Size**      | Ukuran buffer yang tersedia                  |
| **Checksum**         | Kolom untuk pemeriksaan kesalahan            |

## 🔍 **Most Common TCP Display Filters**

Gunakan filter berikut pada bar **Display Filter** di Wireshark:

| Filter                    | Deskripsi                                  |
|---------------------------|--------------------------------------------|
| `tcp`                     | Menampilkan semua paket TCP                |
| `tcp.flags.syn == 1`      | Menampilkan paket SYN (awal koneksi)       |
| `tcp.flags.fin == 1`      | Menampilkan paket FIN (akhir koneksi)      |
| `tcp.port == 80`          | Menampilkan paket TCP pada port 80         |
| `ip.addr == 192.168.1.1`  | Menampilkan lalu lintas TCP ke/dari host tertentu |


## 🧪 **Eksekusi Lab**

https://github.com/0xrajneesh/90-Days-SOC-Challenge-Beginner/raw/refs/heads/main/Protocol_Analysis_pcap.pcapng

Buka file PCAP sampel Anda menggunakan Wireshark. Untuk mengambil tangkapan layar (screenshot) yang diminta, ketikkan Display Filter berikut pada kolom filter di bagian atas Wireshark, lalu tekan Enter.

1.**Tampilkan Semua Paket TCP:** 

Ketikkan filter tcp di bar pencarian lalu Enter. Anda akan melihat semua lalu lintas yang murni menggunakan protokol TCP. Ambil screenshot.

2.**Tampilkan Paket SYN:** 

Ketikkan tcp.flags.syn == 1. Ini akan menyaring dan hanya menampilkan paket-paket yang sedang mencoba memulai koneksi. Ini sangat berguna untuk mendeteksi Port Scanning (seperti Nmap SYN Scan). Ambil screenshot.

3.**Tampilkan Paket FIN:** 

Ketikkan tcp.flags.fin == 1. Ini menampilkan paket-paket yang sedang dalam proses menutup koneksi secara normal. Ambil screenshot.

4.**Filter Berdasarkan IP Spesifik:** 

Pilih satu IP yang sering muncul di lab Anda (misalnya 192.168.1.1). Ketikkan ip.addr == 192.168.1.1 && tcp. Filter ini menggabungkan pencarian IP dan protokol TCP spesifik untuk host tersebut. Ambil screenshot.
