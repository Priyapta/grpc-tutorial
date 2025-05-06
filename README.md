
1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
    - Unary RPC: Unary menyediakan service dimana klien akan mengirimkan satu request dan server juga membalas dengan satu response saja. Unary cocok digunakan untuk operasi sederhana seperti login, ambil data, dll.
    - Server Streaming: Server Streaming berarti klien mengirimkan satu request dan server mengirim banyak respons secara bertahap. Cocok digunakan untuk notifikasi.
    - Bi-directional Streaming RPC: RPC Method ini idealnya digunakan dalam aplikasi real-time seperti chat, game, dan lain lain karena klien dan server akan saling mengirim pesan secara terus-menerus selama koneksi aktif.
2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
    - Secara default Grpc sudah menyediakan Transport Layer Security. Sementara untuk otentikasi perlu aplikasi tambahan seperti JWT token, API Key , atau OAuth. Dan sama untuk otorisasi pengguna dapat mengimpelement Role Base Acces Control secara mandiri.
3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
    - Beberapa masalah yang dapat terjadi seperti ketika ada sistem dua arah stream aktif dan perlu handling menangani dua async stream secara bersamaan. solusinya dengan menggunakan `tokio::spawn` untuk memproses masing - masing stream secara pararel, dan saling kordinasikan dengan channel `tokio::sync::mpsc`

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
    - Kelebihan `tokio_stream::wrappers::ReceiverStream `adalah kemampuannya untuk menghubungkan channel `tokio::mpsc` dengan gRPC secara praktis, sehingga mempermudah integrasi logika asynchronous ke dalam stream respons. Namun, kelemahannya terletak pada penggunaan channel bounded atau unbounded, yang berpotensi menimbulkan bottleneck apabila ukuran buffer tidak dikendalikan dengan tepat.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
    - Tentunya dengan pemisahan layer yang jelas seperti pemisahan kode proto, service , handles, dll. Gunakan design pattern agar kode dapat dimantain lebih mudah dan mudah jika dilakukan penambahan.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
    - Untuk menangani logika yang lebih kompleks, perlu diterapkan beberapa aspek penting seperti validasi data masukan, misalnya memeriksa apakah pengguna valid, memverifikasi nilai nominal, dan sebagainya. Selain itu, logging dan audit trail sangat krusial untuk mencatat setiap transaksi, sehingga memudahkan pelacakan apabila terjadi kesalahan. Penanganan error juga tidak kalah penting, termasuk dalam menghadapi situasi seperti timeout, kegagalan koneksi, kesalahan autentikasi, dan jenis kesalahan lainnya.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
    - Adopsi gRPC meningkatkan efisiensi dan performa komunikasi antarlayanan dalam sistem terdistribusi, serta mendukung interoperabilitas lintas bahasa. Namun, penggunaannya memerlukan adaptasi tambahan untuk sistem legacy dan browser karena keterbatasan dukungan langsung.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
    - Dengan menggunakan HTTP/2 karena data bus lebih besar maka load data akan lebih cepat dibandingkan dengan HTTP/1.1

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
    - Model request-response pada REST API bersifat stateless dan sinkron, di mana klien harus menunggu respons dari server setiap kali mengirim permintaan, sehingga kurang ideal untuk komunikasi real-time. Sebaliknya, gRPC mendukung bidirectional streaming yang memungkinkan klien dan server saling mengirim data secara terus-menerus tanpa harus menunggu respons penuh, membuatnya jauh lebih responsif dan cocok untuk aplikasi seperti chat, notifikasi, atau sistem monitoring waktu nyata.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
    - Pendekatan berbasis skema pada gRPC dengan Protocol Buffers memberikan keuntungan berupa validasi data yang ketat, ukuran payload yang lebih kecil, dan efisiensi tinggi dalam serialisasi. Namun, ini mengharuskan semua pihak mengikuti definisi .proto yang telah ditentukan. Sebaliknya, JSON pada REST lebih fleksibel dan mudah dibaca manusia, tetapi cenderung menghasilkan payload yang lebih besar dan rawan inkonsistensi karena tidak memiliki skema formal bawaan.
