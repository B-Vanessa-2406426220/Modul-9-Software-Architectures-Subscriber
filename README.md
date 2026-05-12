# Reflection

> What is amqp?

AMQP (Advanced Message Queuing Protocol) adalah protokol standar terbuka pada lapisan aplikasi (application layer) yang dirancang khusus untuk message-oriented middleware. Protokol ini berfungsi layaknya sebuah sistem pos digital yang bertugas mengatur, merutekan, dan mengantarkan data antar berbagai perangkat lunak. Sederhananya, AMQP menyediakan seperangkat aturan baku yang memungkinkan berbagai aplikasi untuk saling berkomunikasi secara efisien melalui message broker, seperti RabbitMQ. Sistem ini menjamin bahwa setiap pesan yang dikirim tidak akan hilang, melainkan dapat diantrekan dan dikirimkan dengan tingkat keandalan yang sangat tinggi. Selain itu, AMQP juga mendukung arsitektur komunikasi yang asinkron, sehingga pengirim pesan bisa tetap bekerja tanpa harus menunggu penerima siap memproses datanya. Keunggulan utamanya adalah fleksibilitas yang menjembatani komunikasi dengan mulus, meskipun aplikasi yang berinteraksi dibuat menggunakan bahasa pemrograman yang sama sekali berbeda. Bahkan, protokol ini tetap bekerja secara optimal untuk menghubungkan sistem-sistem yang berjalan pada infrastruktur server atau lingkungan operasi yang terpisah secara fisik.

> What does it mean? guest:guest@localhost:5672, what is the first guest, and what is the second guest, and what is localhost:5672 is for?

Ini adalah URI (Uniform Resource Identifier) koneksi yang memberi tahu aplikasi Rust secara spesifik bagaimana dan di mana harus terhubung dengan message broker.
- guest yang pertama: Ini adalah username (nama pengguna). "guest" adalah nama pengguna bawaan (default) yang otomatis dibuat saat RabbitMQ pertama kali diinstal.
- guest yang kedua: Ini adalah password untuk pengguna tersebut. "guest" juga merupakan kata sandi bawaan untuk akun "guest".
- localhost:5672: Ini menentukan alamat host dan nomor port dari server tempat message broker tersebut beroperasi.
    - localhost: Menandakan bahwa message broker (misalnya RabbitMQ) berjalan di komputer lokal yang sama persis dengan tempat aplikasi Rust yang dijalankan.

    - 5672: Ini adalah nomor port jaringan standar bawaan yang digunakan oleh server AMQP untuk menerima koneksi masuk (koneksi standar/TCP tanpa enkripsi).

## Simulation Slow Subscriber

![/slow-RabbitMQ](./assets/slow-subscriber.jpg)

Pada simulasi saya, jumlah pesan maksimum yang menumpuk di dalam queue adalah 10 pesan. Meskipun saya menjalankan program publisher sebanyak 7 kali secara berturut-turut (total 35 pesan), antrean tidak menumpuk hingga angka maksimal tersebut karena subscriber tetap aktif memproses pesan di latar belakang.

Angka 10 ini mencerminkan selisih kecepatan antara pengiriman pesan yang instan oleh publisher dan pemrosesan pesan oleh subscriber yang memiliki jeda waktu (thread::sleep). RabbitMQ menyimpan pesan-pesan yang belum sempat diproses ke dalam antrean agar tidak hilang. Begitu aktivitas pengiriman dari publisher berhenti, subscriber segera menghabiskan sisa antrean hingga kembali ke angka 0, yang ditandai dengan penurunan tajam pada grafik Queued Messages di akhir.