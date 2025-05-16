# subscriber-module9

**a. What is AMQP?**

AMQP (Advanced Message Queuing Protocol) adalah protokol komunikasi terbuka yang dirancang untuk sistem message broker seperti RabbitMQ. Protokol ini memungkinkan aplikasi yang berbeda untuk saling bertukar pesan secara andal dan terstruktur, tanpa harus langsung terhubung satu sama lain. AMQP mendukung fitur seperti antrian pesan, routing, dan manajemen antrian, yang sangat berguna dalam arsitektur sistem terdistribusi dan mikroservis.

**b. What does it mean? guest:guest@localhost:5672**

`guest:guest@localhost:5672` adalah bagian dari URL koneksi AMQP yang menunjukkan kredensial dan alamat server message broker. `guest:guest` berarti username dan password-nya adalah "guest", sedangkan `localhost` menunjukkan bahwa broker dijalankan di mesin lokal (komputer kita sendiri), dan `5672` adalah port default yang digunakan oleh RabbitMQ untuk komunikasi AMQP. Dengan demikian, kombinasi ini mengatur agar publisher atau subscriber bisa terhubung ke RabbitMQ lokal menggunakan akun bawaan.

**c. Slowdown Subscriber**
![pic4](images/pic4.png)

Saya menjalankan perintah cargo run sebanyak 10 kali secara cepat, yang menyebabkan hanya terbentuk satu antrian. Hal ini disebabkan karena publisher mengirim pesan dalam waktu yang sangat singkat dan berurutan, sementara subscriber membutuhkan waktu lebih lama untuk memproses setiap pesan. Akibatnya, subscriber tidak mampu mengejar kecepatan publisher, sehingga pesan-pesan tersebut menumpuk di antrian dan menyebabkan keterlambatan dalam penanganannya.

**d. Five Subscribers Console**
![pic5](images/pic5.png)
![pic6](images/pic6.png)

Sebelumnya, saat hanya ada satu subscriber, terjadi **bottleneck** karena satu subscriber harus memproses semua pesan yang dikirim sangat cepat oleh publisher. Hal ini menyebabkan pesan menumpuk di antrian dan menyebabkan delay.

Namun, ketika membuka 5 terminal dan menjalankan `cargo run` secara paralel untuk subscriber, saya melihat penurunan jumlah pesan dalam antrian terjadi lebih cepat karena sekarang setiap subscriber bisa menangani satu pesan secara paralel. Misalnya:

- Pesan untuk `2306221762-Amir` di-handle oleh terminal 1,
- `2306221762-Budi` oleh terminal 2,
dan seterusnya.

Dengan demikian, 1 subscriber menangani 1 user, mempercepat proses distribusi dan konsumsi pesan.