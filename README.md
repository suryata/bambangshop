# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [V] Commit: `Create Subscriber model struct.`
    -   [V] Commit: `Create Notification model struct.`
    -   [V] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [V] Commit: `Implement add function in Subscriber repository.`
    -   [V] Commit: `Implement list_all function in Subscriber repository.`
    -   [V] Commit: `Implement delete function in Subscriber repository.`
    -   [V] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [V] Commit: `Create Notification service struct skeleton.`
    -   [V] Commit: `Implement subscribe function in Notification service.`
    -   [V] Commit: `Implement subscribe function in Notification controller.`
    -   [V] Commit: `Implement unsubscribe function in Notification service.`
    -   [V] Commit: `Implement unsubscribe function in Notification controller.`
    -   [V] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [V] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [V] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [V] Commit: `Implement publish function in Program service and Program controller.`
    -   [V] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [V] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?<br/>

Dalam pola desain Observer, objek yang disebut Subscriber biasanya mewakili entitas yang menerima pembaruan dari subjek ketika terjadi perubahan. Penerapan pola ini dalam bahasa pemrograman seperti Rust sering kali menggunakan trait untuk mendefinisikan interface Subscriber.

Walaupun dalam kasus BambangShop hanya ada satu struct Model yang terlihat, keputusan untuk menggunakan trait atau cukup dengan satu struct Model bergantung pada beberapa faktor:

- Jika BambangShop hanya akan memiliki satu jenis subscriber yang akan menangani notifikasi dengan cara yang sama, maka menggunakan satu struct Model bisa saja cukup. Ini menyederhanakan desain karena tidak ada kebutuhan untuk polimorfisme.

- Namun, jika di masa depan BambangShop memerlukan fleksibilitas untuk mendukung berbagai jenis subscriber dengan perilaku yang berbeda-beda dalam menangani notifikasi, maka penggunaan trait untuk mendefinisikan interface Subscriber sangatlah disarankan. Dengan menggunakan trait, kita bisa mendefinisikan set metode yang harus diimplementasikan oleh setiap jenis subscriber dan memungkinkan sistem untuk mudah diperluas.

Secara umum, trait memberikan fleksibilitas dan skalabilitas dalam mendesain sistem yang bisa beradaptasi dengan perubahan atau penambahan fitur di masa depan. Jika BambangShop ingin mempertahankan kemungkinan tersebut, atau memiliki rencana untuk berbagai jenis subscriber, maka penggunaan trait akan lebih baik.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?<br/>
Ketika kita memiliki elemen dengan identitas yang harus unik seperti id dalam Program atau url dalam Subscriber, pilihan struktur data untuk menyimpannya sangat penting terkait dengan efisiensi pencarian, penambahan, dan penghapusan elemen.

Menggunakan Vec atau list mungkin cukup untuk kasus-kasus dengan jumlah elemen yang sangat kecil dan operasi yang jarang terjadi, karena pencarian akan dilakukan secara linier dan efisiensinya rendah; setiap operasi untuk menemukan atau menghapus elemen memerlukan waktu proporsional terhadap jumlah elemen dalam list. Ini mungkin tidak masalah untuk sistem yang tidak membutuhkan skalabilitas atau kinerja tinggi.

Namun, dalam sistem yang memerlukan akses cepat dan sering ke elemen berdasarkan identitas uniknya, menggunakan struktur data seperti DashMap, yang merupakan implementasi thread-safe dari HashMap, sangatlah diperlukan. HashMap menyediakan operasi pencarian, penambahan, dan penghapusan dalam waktu yang konstan (O(1)) di rata-rata kasus, yang jauh lebih efisien dibandingkan dengan list.

Pertimbangan lainnya adalah keselamatan penggunaan data di lingkungan multithread. DashMap dirancang untuk digunakan dalam konteks multithread di Rust dan memastikan bahwa akses ke data dilakukan dengan aman tanpa mengorbankan kinerja. Ini penting ketika sistem perlu menangani banyak permintaan secara bersamaan atau ketika operasi pada data bersifat konkuren.

Jadi, jika id atau url perlu dijamin unik dan sistem memerlukan akses yang cepat dan aman di lingkungan multithread, maka menggunakan DashMap akan lebih tepat daripada menggunakan Vec.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?<br/>

Dalam pemrograman Rust, keselamatan thread merupakan aspek penting yang dipastikan oleh sistem jenisnya. Compiler Rust menegakkan kepemilikan dan peminjaman yang aman untuk mencegah kondisi balapan dan bug lainnya yang terkait dengan keselamatan memori, terutama ketika berhadapan dengan konteks multithread.

DashMap adalah pustaka eksternal yang memberikan implementasi HashMap yang aman untuk thread. Ini memungkinkan beberapa thread untuk membaca dan menulis ke map tanpa perlu mekanisme sinkronisasi yang kompleks, karena DashMap sudah mengurusnya di dalam implementasinya sendiri.

Pola Singleton, di sisi lain, adalah pola desain yang memastikan bahwa sebuah kelas hanya memiliki satu instans dan menyediakan titik akses global ke instans tersebut. Dalam konteks Rust, pola Singleton biasanya diimplementasikan menggunakan lazy_static untuk menjamin inisialisasi yang aman pada saat runtime.

Pertanyaannya adalah apakah DashMap masih diperlukan jika kita menerapkan pola Singleton? Jawabannya bergantung pada persyaratan konkurensi dari aplikasi BambangShop:

- Jika aplikasi membutuhkan akses konkuren ke list of subscribers:
Dalam hal ini, DashMap sangat berguna karena memungkinkan akses konkuren yang aman tanpa perlu mengelola locks secara manual. Jika kita hanya menggunakan pola Singleton dengan HashMap biasa, kita harus menambahkan mekanisme sinkronisasi sendiri, seperti menggunakan Mutex atau RwLock, yang bisa membuat kode lebih rumit dan berpotensi mengurangi kinerja.

- Jika aplikasi tidak membutuhkan akses konkuren atau hanya dijalankan dalam konteks single-thread:
Dalam skenario ini, pola Singleton dengan HashMap mungkin sudah cukup dan penggunaan DashMap mungkin terlalu berlebihan karena menambahkan overhead untuk keselamatan thread yang tidak diperlukan.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?<br/>

Dalam pola desain Model-View-Controller (MVC), Model secara tradisional memang menangani baik logika bisnis maupun penyimpanan data. Namun, seiring berkembangnya aplikasi yang kompleks, seringkali praktik terbaik menyarankan pemisahan tanggung jawab untuk mendukung skala yang lebih besar, pengujian yang lebih baik, dan pemeliharaan yang lebih mudah. Inilah alasan mengapa "Service" dan "Repository" sering kali dipisahkan dari Model:

- Single Responsibility Principle (SRP): Ini adalah prinsip SOLID yang menyatakan bahwa sebuah kelas harus memiliki satu, dan hanya satu, alasan untuk berubah. Dengan memisahkan "Service" (yang menangani logika bisnis) dan "Repository" (yang menangani operasi penyimpanan data) dari Model, setiap kelas dapat berfokus pada satu tanggung jawab.

- Pemisahan Logika Bisnis dari Logika Akses Data: Logika bisnis seringkali cukup rumit dan berubah-ubah, sementara logika akses data cenderung stabil dan berkaitan dengan kinerja operasi CRUD ke database. Dengan memisahkannya, kita bisa mengelola kompleksitas dan perubahan pada masing-masing aspek secara terpisah.

- Testing yang Lebih Mudah: Dengan memisahkan Service dan Repository, kita dapat menguji logika bisnis secara terisolasi tanpa perlu berinteraksi dengan database (atau sumber data lainnya). Hal ini memudahkan pengujian unit dan sering kali menghasilkan kode yang lebih bersih.

- Fleksibilitas dan Scalability: Ketika logika bisnis dan akses data dipisahkan, lebih mudah untuk mengubah salah satunya tanpa mempengaruhi yang lain. Misalnya, jika kita ingin mengganti sumber penyimpanan data, hanya Repository yang perlu diubah, bukan seluruh Model.

- Code Reusability: Kode yang bersih dan terorganisir dengan baik cenderung lebih bisa digunakan kembali. Service dan Repository yang terdefinisi dengan baik dapat digunakan kembali di bagian lain dari aplikasi atau bahkan di aplikasi yang berbeda.

- Decoupling: Pemisahan ini membantu mengurangi ketergantungan antar komponen. Dengan begitu, perubahan pada database atau perubahan pada aturan bisnis tidak menyebabkan efek domino yang memerlukan perubahan luas pada kode.

Singkatnya, Service dan Repository membantu kita mengikuti prinsip-prinsip desain perangkat lunak yang baik dengan memberikan struktur yang lebih bersih, lebih modular, dan lebih mudah untuk dijaga dan dikembangkan. Ini mengarah pada sistem yang lebih robust dan maintainable.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?<br/>
Jika kita hanya menggunakan Model tanpa Service dan Repository, interaksi antara setiap model (Program, Subscriber, Notification) bisa meningkatkan kompleksitas kode secara signifikan. Berikut adalah beberapa kemungkinan dampaknya:

- Kompleksitas Model Tinggi: Setiap model akan harus menangani logika bisnis dan akses data yang spesifik. Ini berarti metode untuk operasi CRUD, validasi, dan logika bisnis lainnya akan bercampur dalam satu kelas Model, membuatnya besar dan sulit untuk dimengerti.

- Pengulangan Kode: Tanpa Repository, kode untuk akses data mungkin perlu diulangi di setiap Model. Ini bisa menyebabkan duplikasi yang berlebihan dan potensi inkonsistensi jika perubahan diperlukan.

- Tes yang Sulit: Mengujikan model yang juga menangani akses data akan sulit karena setiap tes akan memerlukan interaksi dengan database atau mock objects yang rumit, yang dapat meningkatkan waktu yang diperlukan untuk tes dan membuat tes lebih rentan terhadap kegagalan karena masalah luar yang tidak terkait.

- Perubahan Menjadi Berisiko: Jika bisnis memerlukan perubahan logika, ini mungkin memerlukan perubahan pada Model yang sama yang mengakses database, meningkatkan risiko bug dan efek samping yang tidak diinginkan karena perubahan tersebut mungkin mempengaruhi cara data disimpan atau diambil.

- Ketergantungan Berlebihan: Model yang berinteraksi langsung satu sama lain mungkin mengembangkan ketergantungan yang kuat. Misalnya, jika model Program perlu mengirim Notification kepada Subscriber, maka ada kemungkinan Program akan langsung berkomunikasi dengan Notification yang juga menangani akses data, membuat susunan ketergantungan yang kompleks dan sulit dipisahkan.

- Pembaruan Menjadi Sulit: Dalam sistem yang hanya menggunakan Model, melakukan pembaruan pada skema database atau aturan bisnis bisa menjadi sangat sulit. Karena logika terkait dengan database dan bisnis tersebar di seluruh Model, perubahan kecil bisa membutuhkan banyak perubahan di berbagai bagian kode.

Dengan hanya menggunakan Model, kita mengorbankan kejelasan, pemisahan tanggung jawab, dan fleksibilitas. Sementara ini mungkin berfungsi untuk aplikasi yang sangat sederhana, untuk aplikasi yang lebih kompleks dan berkembang ini akan menciptakan basis kode yang sulit untuk dikelola dan berisiko tinggi untuk error.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.<br/>
Saya telah menggunakan Postman untuk menguji API dalam proyek saya. Ini sangat membantu untuk memastikan bahwa API yang saya kembangkan berfungsi sesuai ekspektasi. Berikut beberapa fitur Postman yang saya temukan berguna:

- Pengujian API: Saya dapat dengan mudah membuat request ke API dan melihat responsnya, memastikan bahwa endpoint bekerja sebagaimana mestinya.

- Environment: Saya bisa menyiapkan environment yang berbeda untuk pengembangan dan produksi, memudahkan saya untuk beralih antara berbagai konfigurasi server tanpa harus mengubah request secara manual.

- Collections: Memungkinkan saya untuk menyimpan dan mengelompokkan request yang sering digunakan. Ini mempermudah saya untuk berbagi request dengan tim dan menjaga agar tetap terorganisir.

- Monitoring: Fitur ini memungkinkan saya untuk memantau API secara berkala untuk memeriksa kesehatan dan ketersediaannya, yang sangat penting untuk aplikasi produk

- Integration: Kemampuan untuk mengintegrasikan dengan sistem lain seperti GitHub, Jenkins, dan Slack sangat berguna untuk mempercepat workflow pengembangan dan berkolaborasi dengan tim

Fitur-fitur ini membuat Postman menjadi alat yang sangat berguna tidak hanya untuk proyek kelompok, tapi juga untuk pengembangan aplikasi kedepannya, hal ini karena Postman sangat membantu untuk memastikan bahwa API yang saya kembangkan bisa digunakan dan mudah untuk dikerjakan secara kolaboratif.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?<br/>

Dalam kasus tutorial ini, terutama melihat bagaimana Subscriber menerima pemberitahuan, kita menggunakan variasi Push model dari Observer Pattern. Dalam model ini, Publisher (dalam konteks kita, bisa jadi merupakan bagian dari NotificationService) bertanggung jawab untuk mengirimkan atau "mendorong" data ke Subscriber setiap kali ada pembaruan.

Ini berbeda dengan model Pull, di mana Subscriber secara periodik memeriksa atau "menarik" pembaruan dari Publisher. Dalam model Push, Subscriber tidak perlu mengetahui kapan harus memeriksa pembaruan, karena Publisher secara aktif menginformasikan kepada Subscriber ketika data tersedia.

Dalam kasus kita, ketika produk baru ditambahkan atau diperbarui, sistem akan "mendorong" pemberitahuan ke semua Subscriber yang relevan tanpa Subscriber perlu meminta pembaruan tersebut secara berkala. Ini membuat desain lebih efisien dalam hal lalu lintas jaringan, karena data hanya dikirimkan ketika benar-benar ada sesuatu yang baru atau telah berubah.

2.What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)<br/>

Jika kita menggunakan model Pull dalam kasus tutorial yang telah dibahas, berikut ini adalah kelebihan dan kekurangannya:

Kelebihan Model Pull<br/>

- Kontrol yang Lebih Besar untuk Subscriber<br/>
Subscriber memiliki kontrol penuh atas kapan mereka mengambil data. Ini bisa berguna jika Subscriber memiliki sumber daya yang terbatas atau jika ingin mengontrol beban kerja mereka sendiri.

- Efisiensi Data<br/>
Subscriber hanya meminta data ketika mereka siap untuk mengolahnya, yang berarti data tidak dikirimkan secara otomatis dan berpotensi tidak digunakan.

- Kebaruan Data<br/>
Ketika Subscriber meminta data, mereka dijamin mendapatkan data terbaru yang tersedia pada saat itu.

Kekurangan Model Pull<br/>

- Overhead Jaringan yang Tinggi<br/>
Jika Subscriber sering memeriksa pembaruan, hal ini dapat menyebabkan banyak permintaan yang tidak perlu ke server, yang bisa meningkatkan beban pada jaringan dan server.

- Pembaruan yang Lambat<br/>
Subscriber mungkin tidak segera menerima pembaruan karena mereka hanya akan menyadarinya setelah melakukan permintaan berikutnya, yang berarti ada latensi antara saat sebuah peristiwa terjadi dan saat Subscriber mengetahuinya.

- Penggunaan Sumber Daya yang Tidak Efisien<br/>
Model Pull bisa menyebabkan Subscriber menggunakan lebih banyak sumber daya karena mereka harus terus memeriksa pembaruan secara aktif, yang mungkin tidak perlu jika perubahan jarang terjadi.<br/>

Dalam konteks tutorial ini, menggunakan model Pull mungkin tidak sesuai jika tujuannya adalah untuk memberikan pemberitahuan secepat mungkin kepada Subscriber tentang perubahan produk. Model Push cenderung lebih sesuai dalam kasus di mana kita ingin memastikan Subscriber segera mengetahui perubahan tanpa harus memikirkan kapan harus membuat permintaan untuk informasi tersebut.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.<br/>

Jika kita memutuskan untuk tidak menggunakan multi-threading dalam proses notifikasi, berikut ini adalah beberapa dampak yang mungkin terjadi pada program:

- Penundaan dalam Pengiriman Notifikasi<br/>
Tanpa multi-threading, proses pengiriman notifikasi akan berjalan secara sekuensial. Artinya, notifikasi ke subscriber berikutnya hanya akan dikirim setelah notifikasi sebelumnya selesai diproses. Hal ini dapat menyebabkan penundaan signifikan jika ada banyak subscriber atau jika proses pengiriman notifikasi membutuhkan waktu yang lama.

- Responsivitas Aplikasi Menurun<br/>
Karena proses notifikasi dilakukan secara sekuensial dalam satu thread, aplikasi mungkin menjadi kurang responsif. Ini terutama terjadi jika aplikasi juga menangani permintaan lain atau tugas-tugas di thread yang sama. Pengguna aplikasi mungkin mengalami keterlambatan atau aplikasi tampak "membeku" selama proses notifikasi berlangsung.

- Skalabilitas Terbatas<br/>
Seiring dengan bertambahnya jumlah subscriber, beban pada proses notifikasi akan meningkat. Tanpa kemampuan untuk memproses notifikasi secara paralel, meningkatkan skalabilitas aplikasi akan menjadi tantangan. Aplikasi mungkin akan kesulitan menangani beban kerja yang lebih besar tanpa mengalami penurunan kinerja.