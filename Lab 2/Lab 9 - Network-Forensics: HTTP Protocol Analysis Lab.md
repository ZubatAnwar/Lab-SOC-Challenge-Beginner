#Lab 9 - Network-Forensics: HTTP Protocol Analysis Lab

## 📘 **Bagian 1: Penjelasan HTTP**

Apa itu HTTP?

Hypertext Transfer Protocol (HTTP) adalah fondasi dari World Wide Web, dan digunakan untuk memuat halaman web menggunakan tautan hiperteks. HTTP adalah protokol lapisan aplikasi yang dirancang untuk mentransfer informasi antar perangkat yang terhubung ke jaringan dan berjalan di atas lapisan lain dari tumpukan protokol jaringan . 
Alur tipikal melalui HTTP melibatkan mesin klien yang membuat permintaan ke server, yang kemudian mengirimkan pesan respons.

HTTP biasanya berjalan tanpa enkripsi (teks biasa/ cleartext) melalui port TCP 80. Karena bentuknya teks biasa, siapa pun yang bisa "mendengarkan" jaringan (menggunakan alat seperti Wireshark) bisa membaca seluruh isinya. Inilah mengapa HTTP tidak aman dan sekarang digantikan oleh HTTPS (yang dienkripsi).

(*https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/*)

## 🌐 Key HTTP Fields

| Field Name         | Deskripsi |
|--------------------|-----------|
| **Request Method** | Metode HTTP yang digunakan oleh client untuk melakukan request, seperti `GET`, `POST`, `HEAD`, dan lainnya. |
| **Host**           | Nama domain atau website yang sedang diakses oleh client. |
| **User-Agent**     | Informasi mengenai client atau browser yang digunakan untuk mengirim request, seperti jenis browser dan sistem operasi. |
| **URI**             | Path atau alamat resource yang diminta dari server. |
| **Status Code**    | Kode status yang menunjukkan hasil dari response server, seperti `200 OK`, `404 Not Found`, atau `500 Internal Server Error`. |
| **Content-Type**   | Menunjukkan tipe atau format data yang dikirim oleh server, misalnya `text/html`, `application/json`, atau `image/png`. |
| **Cookie/Header**  | Informasi tambahan yang dikirim melalui HTTP header, termasuk cookie yang dapat digunakan untuk session, autentikasi, atau tracking. |

## 🔍 Most Common HTTP Display Filters

Berikut beberapa **Display Filter Wireshark** yang umum digunakan untuk menganalisis traffic HTTP:

| Display Filter | Deskripsi |
|----------------|-----------|
| `http` | Menampilkan seluruh traffic yang menggunakan protokol HTTP. |
| `tcp.port == 80` | Menampilkan traffic TCP yang menggunakan port `80`, yang secara default digunakan oleh HTTP. |
| `http.request.method == "GET"` | Menampilkan seluruh HTTP request dengan metode `GET`. |
| `http.request.uri` | Menampilkan informasi URI atau resource yang diminta oleh client. |
| `http.set_cookie` | Menampilkan cookie yang dikirim oleh server melalui HTTP response. |
| `ip.addr == 192.168.1.10` | Menampilkan traffic yang berasal dari atau menuju alamat IP `192.168.1.10`. |

## 📊 HTTP Status Codes

| Status Code | Name                  | Description                                      |
|-------------|-----------------------|--------------------------------------------------|
| **200**     | OK                    | Request berhasil diproses oleh server           |
| **201**     | Created               | Resource baru berhasil dibuat oleh server       |
| **204**     | No Content            | Request berhasil tanpa response body            |
| **301**     | Moved Permanently     | Resource telah dipindahkan secara permanen      |
| **302**     | Found                 | Resource sementara berada di URL lain           |
| **304**     | Not Modified          | Resource belum berubah dan dapat menggunakan cache |
| **400**     | Bad Request           | Request tidak valid atau tidak dapat diproses   |
| **401**     | Unauthorized          | Membutuhkan autentikasi yang valid              |
| **403**     | Forbidden             | Server menolak akses ke resource                |
| **404**     | Not Found             | Resource yang diminta tidak ditemukan           |
| **405**     | Method Not Allowed    | HTTP method tidak diperbolehkan                 |
| **408**     | Request Timeout       | Server terlalu lama menunggu request            |
| **429**     | Too Many Requests     | Terlalu banyak request dalam waktu tertentu     |
| **500**     | Internal Server Error | Terjadi kesalahan pada sisi server              |
| **502**     | Bad Gateway            | Gateway menerima response yang tidak valid      |
| **503**     | Service Unavailable   | Server sedang tidak tersedia atau overload      |
| **504**     | Gateway Timeout       | Gateway tidak menerima response tepat waktu     |



Bagaimana Cara Kerjanya?
Komunikasi HTTP terjadi seperti orang yang sedang bertanya dan menjawab:

1. Request (Permintaan): Komputer Anda meminta sesuatu ke server.

- GET: Meminta data (misal: "Tolong tampilkan halaman login.html").

- POST: Mengirim data (misal: "Ini lho username dan password saya, tolong login-kan").

2. Response (Balasan): Server merespons permintaan Anda dengan Status Code.

- 200 OK: Sukses! Halaman ditemukan dan dikirim.

- 404 Not Found: Gagal! Halamannya tidak ada.

- 500 Internal Server Error: Servernya sedang bermasalah/rusak.

(Adversarial Tactics, Techniques, and Common Knowledge) adalah basis pengetahuan global yang dapat diakses secara gratis, berisi kumpulan taktik, teknik, dan prosedur (TTP) perilaku penyerang siber berdasarkan pengamatan di dunia nyata. Kerangka kerja ini dikembangkan oleh MITRE Corporation untuk membantu praktisi keamanan siber memahami cara kerja hacker. (https://attack.mitre.org/)

Misalnya, dalam lab ini kita melihat ada traffic HTTP yang mencurigakan tanpa enkripsi. Kita bisa memetakannya ke MITRE ATT&CK:

- Tactic: TA0011 - Command and Control (Malware sedang berkomunikasi ke server peretas).

- Technique: T1071.001 - Application Layer Protocol: Web Protocols (Menggunakan HTTP port 80 untuk menyamar).

## 🧪 Eksekusi Lab

https://github.com/0xrajneesh/90-Days-SOC-Challenge-Beginner/raw/refs/heads/main/Protocol_Analysis_pcap.pcapng

1. Temukan Target (IP & Domain Asli)

- Buka file Protocol_Analysis_pcap.pcapng di Wireshark.

- Ketik filter ini di kolom atas: http.request.method == "GET"

- Pilih salah satu baris paket yang muncul.

- Lihat panel tengah (Packet Details), perluas bagian Internet Protocol Version 4.

- Catat Source IP (Ini adalah IP target/korban, masukkan ke bagian di laporan).

- Catat Destination IP (Ini adalah IP Attacker atau C2 Server, masukkan ke bagian Laporan).

2. Temukan IOC (Indicator of Compromise)

- Masih di paket yang sama, perluas bagian Hypertext Transfer Protocol di panel tengah.

- Cari baris Host:. (Ini adalah nama domain yang dituju oleh malware/attacker. Masukkan ke tabel IOC di laporan).

- Cari baris Request URI:. (Ini adalah nama file atau path yang diminta, misalnya /login.php atau /download/payload.exe. Masukkan juga ke tabel IOC).

- Cari baris User-Agent:. (Periksa apakah namanya aneh atau kosong. Jika aneh, ini bukti kuat malware beaconing).
