#Lab 10 - Wireshark Traffic Analysis: Inspecting TLS Handshakes

## 📘 **Bagian 1: Penjelasan HTTP**

Transport Layer Security, atau TLS, adalah protokol keamanan yang diadopsi secara luas yang dirancang untuk memfasilitasi privasi dan keamanan data untuk komunikasi melalui Internet.
Penggunaan utama TLS adalah mengenkripsi komunikasi antara aplikasi web dan server, seperti browser web yang memuat situs web. 
TLS juga dapat digunakan untuk mengenkripsi komunikasi lain seperti email, pesan, dan suara melalui IP (VoIP) . Dalam artikel ini, kita akan fokus pada peran TLS dalam keamanan aplikasi web .

**Apa perbedaan antara TLS dan SSL?**

SSL (Secure Sockets Layer) adalah teknologi lama yang digunakan untuk mengenkripsi dan mengamankan komunikasi antara browser dan server.

TLS (Transport Layer Security) adalah penerus SSL yang lebih modern dan aman. TLS melindungi data yang dikirim melalui internet, seperti password dan informasi pribadi.

**Apa fungsi TLS?**

Ada tiga komponen utama yang dicapai oleh protokol TLS: Enkripsi , Otentikasi, dan Integritas.

1. **Enkripsi:** menyembunyikan data yang ditransfer dari pihak ketiga.

2. **Autentikasi:** memastikan bahwa pihak-pihak yang bertukar informasi adalah orang yang mereka klaim.

3. **Integritas:** memverifikasi bahwa data tersebut tidak dipalsukan atau dimanipulasi.

**Bagaimana cara kerja TLS?**

Koneksi TLS dimulai menggunakan urutan yang dikenal sebagai jabat tangan TLS . Ketika pengguna mengunjungi situs web yang menggunakan TLS, jabat tangan TLS dimulai antara perangkat pengguna (juga dikenal sebagai perangkat klien ) dan server web.

Selama proses jabat tangan TLS, perangkat pengguna dan server web:

- Tentukan versi TLS mana (TLS 1.0, 1.2, 1.3, dll.) yang akan mereka gunakan.

- Tentukan rangkaian sandi mana (lihat di bawah) yang akan mereka gunakan.

- Verifikasi identitas server menggunakan sertifikat TLS server.

- Menghasilkan kunci sesi untuk mengenkripsi pesan di antara mereka setelah proses jabat tangan selesai.

Proses jabat tangan TLS menetapkan rangkaian sandi (cipher suite) untuk setiap sesi komunikasi. Rangkaian sandi adalah sekumpulan algoritma yang menentukan detail seperti kunci enkripsi bersama mana , atau kunci sesi , yang akan digunakan untuk sesi tertentu. TLS mampu menetapkan kunci sesi yang cocok melalui saluran yang tidak terenkripsi berkat teknologi yang dikenal sebagai kriptografi kunci publik .

Proses jabat tangan juga menangani otentikasi, yang biasanya terdiri dari server yang membuktikan identitasnya kepada klien. Hal ini dilakukan menggunakan kunci publik. Kunci publik adalah kunci enkripsi yang menggunakan enkripsi satu arah, artinya siapa pun yang memiliki kunci publik dapat menguraikan data yang dienkripsi dengan kunci pribadi server untuk memastikan keasliannya, tetapi hanya pengirim asli yang dapat mengenkripsi data dengan kunci pribadi. Kunci publik server merupakan bagian dari sertifikat TLS-nya.

Setelah data dienkripsi dan diautentikasi, data tersebut kemudian ditandatangani dengan kode autentikasi pesan (MAC). Penerima kemudian dapat memverifikasi MAC untuk memastikan integritas data. Ini mirip dengan lapisan foil anti-perusakan yang terdapat pada botol aspirin; konsumen tahu bahwa tidak ada yang merusak obat mereka karena foil tersebut masih utuh saat mereka membelinya.

<img width="486" height="630" alt="image" src="https://github.com/user-attachments/assets/d683fd9a-ce37-4eca-9df8-d45d846be9da" />

(*https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/*)

## 🔑 **Pesan Utama TLS Handshake**

| Jenis Pesan       | Deskripsi                                                   |
|-------------------|-------------------------------------------------------------|
| **Client Hello**  | Klien memulai koneksi aman dan menawarkan pilihan cipher suite |
| **Server Hello**  | Server memilih cipher dan memberikan sertifikat             |
| **Certificate**   | Server memberikan sertifikat digital (X.509)                |
| **Key Exchange**  | Klien dan server melakukan pertukaran kunci untuk sesi      |
| **Finished**      | Proses handshake selesai dan sesi aman dimulai              |

---

## 🔍 **Filter Tampilan TLS yang Umum**

Gunakan filter berikut pada **Display Filter** di Wireshark:

| Filter                         | Deskripsi                              |
|--------------------------------|----------------------------------------|
| `tls`                          | Menampilkan seluruh lalu lintas TLS    |
| `tcp.port == 443`              | Menampilkan TLS melalui HTTPS          |
| `tls.handshake`                | Menampilkan seluruh TLS Handshake      |
| `tls.handshake.type == 1`      | Menampilkan pesan Client Hello         |
| `tls.handshake.type == 2`      | Menampilkan pesan Server Hello         |
| `tls.handshake.type == 11`     | Menampilkan pesan Certificate          |
| `tls.handshake.type == 16`     | Menampilkan pesan Key Exchange         |
| `tls.handshake.type == 20`     | Menampilkan pesan Finished             |
| `tls.record.version == 0x0303` | Menampilkan lalu lintas TLS 1.2         |
| `tls.record.version == 0x0304` | Menampilkan lalu lintas TLS 1.3         |
| `tls.alert`                    | Menampilkan pesan TLS Alert            |
| `ip.addr == 192.168.1.10`      | Menampilkan lalu lintas dari/ke IP tertentu |
| `tcp.stream == 0`              | Menampilkan satu sesi TCP tertentu     |

## 🧪 Eksekusi Lab

A. Show all TLS traffic
1. Ketik tls di kolom Display Filter Wireshark lalu tekan Enter.

2. Penjelasan untuk laporan: Filter ini menyingkirkan semua trafik latar belakang (seperti ARP atau DNS) dan hanya memfokuskan analisis pada paket yang dienkripsi menggunakan protokol SSL/TLS. Anda akan melihat baris dengan info seperti Application Data (data yang sudah dienkripsi) atau Handshake (proses negosiasi kunci).

3. Tangkapan Layar 1: Ambil screenshot layar Wireshark yang menunjukkan baris-baris berlabel "TLS" di kolom Protocol.

B. Show Client Hello messages
1. Ubah filter menjadi tls.handshake.type == 1 dan tekan Enter.

2. Penjelasan untuk laporan: Client Hello adalah pesan pertama yang dikirim komputer Anda saat meminta koneksi aman ke server. Jika Anda mengeklik salah satu paket dan melihat panel Packet Details di bawah, navigasikan ke Transport Layer Security > TLS Record Layer > Handshake Protocol: Client Hello > Extension: server_name. Di sini terdapat SNI (Server Name Indication) yang mengekspos nama domain asli yang dituju (misalnya: [www.google.com](https://www.google.com)), meskipun isi komunikasinya nanti dienkripsi. Ini adalah metadata krusial untuk mendeteksi apakah komputer berkomunikasi dengan domain malware.

3. Tangkapan Layar 2: Ambil screenshot dengan satu paket Client Hello terpilih, dan pastikan bagian Extension: server_name (SNI) terlihat di panel detail bawah.

C. Show TLS 1.2 traffic
1. Ubah filter menjadi tls.record.version == 0x0303 dan tekan Enter.

2. Penjelasan untuk laporan: Filter ini secara spesifik mencari sesi komunikasi yang menggunakan TLS versi 1.2. Menganalisis versi TLS sangat penting untuk audit keamanan. Penggunaan versi usang (seperti TLS 1.0 atau 1.1) sangat rentan terhadap serangan manipulasi jaringan (seperti serangan POODLE). Memastikan trafik dominan berada di TLS 1.2 atau TLS 1.3 (0x0304) mengonfirmasi postur keamanan jaringan yang baik.

3. Tangkapan Layar 3: Ambil screenshot daftar paket, pastikan kolom Info atau detail paket menunjukkan penggunaan TLS 1.2.
