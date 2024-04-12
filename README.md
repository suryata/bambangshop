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
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Dalam pola desain Observer, objek yang disebut Subscriber biasanya mewakili entitas yang menerima pembaruan dari subjek ketika terjadi perubahan. Penerapan pola ini dalam bahasa pemrograman seperti Rust sering kali menggunakan trait untuk mendefinisikan interface Subscriber.

Walaupun dalam kasus BambangShop hanya ada satu struct Model yang terlihat, keputusan untuk menggunakan trait atau cukup dengan satu struct Model bergantung pada beberapa faktor:

- Jika BambangShop hanya akan memiliki satu jenis subscriber yang akan menangani notifikasi dengan cara yang sama, maka menggunakan satu struct Model bisa saja cukup. Ini menyederhanakan desain karena tidak ada kebutuhan untuk polimorfisme.

- Namun, jika di masa depan BambangShop memerlukan fleksibilitas untuk mendukung berbagai jenis subscriber dengan perilaku yang berbeda-beda dalam menangani notifikasi, maka penggunaan trait untuk mendefinisikan interface Subscriber sangatlah disarankan. Dengan menggunakan trait, kita bisa mendefinisikan set metode yang harus diimplementasikan oleh setiap jenis subscriber dan memungkinkan sistem untuk mudah diperluas.

Secara umum, trait memberikan fleksibilitas dan skalabilitas dalam mendesain sistem yang bisa beradaptasi dengan perubahan atau penambahan fitur di masa depan. Jika BambangShop ingin mempertahankan kemungkinan tersebut, atau memiliki rencana untuk berbagai jenis subscriber, maka penggunaan trait akan lebih baik.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
Ketika kita memiliki elemen dengan identitas yang harus unik seperti id dalam Program atau url dalam Subscriber, pilihan struktur data untuk menyimpannya sangat penting terkait dengan efisiensi pencarian, penambahan, dan penghapusan elemen.

Menggunakan Vec atau list mungkin cukup untuk kasus-kasus dengan jumlah elemen yang sangat kecil dan operasi yang jarang terjadi, karena pencarian akan dilakukan secara linier dan efisiensinya rendah; setiap operasi untuk menemukan atau menghapus elemen memerlukan waktu proporsional terhadap jumlah elemen dalam list. Ini mungkin tidak masalah untuk sistem yang tidak membutuhkan skalabilitas atau kinerja tinggi.

Namun, dalam sistem yang memerlukan akses cepat dan sering ke elemen berdasarkan identitas uniknya, menggunakan struktur data seperti DashMap, yang merupakan implementasi thread-safe dari HashMap, sangatlah diperlukan. HashMap menyediakan operasi pencarian, penambahan, dan penghapusan dalam waktu yang konstan (O(1)) di rata-rata kasus, yang jauh lebih efisien dibandingkan dengan list.

Pertimbangan lainnya adalah keselamatan penggunaan data di lingkungan multithread. DashMap dirancang untuk digunakan dalam konteks multithread di Rust dan memastikan bahwa akses ke data dilakukan dengan aman tanpa mengorbankan kinerja. Ini penting ketika sistem perlu menangani banyak permintaan secara bersamaan atau ketika operasi pada data bersifat konkuren.

Jadi, jika id atau url perlu dijamin unik dan sistem memerlukan akses yang cepat dan aman di lingkungan multithread, maka menggunakan DashMap akan lebih tepat daripada menggunakan Vec.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Dalam pemrograman Rust, keselamatan thread merupakan aspek penting yang dipastikan oleh sistem jenisnya. Compiler Rust menegakkan kepemilikan dan peminjaman yang aman untuk mencegah kondisi balapan dan bug lainnya yang terkait dengan keselamatan memori, terutama ketika berhadapan dengan konteks multithread.

DashMap adalah pustaka eksternal yang memberikan implementasi HashMap yang aman untuk thread. Ini memungkinkan beberapa thread untuk membaca dan menulis ke map tanpa perlu mekanisme sinkronisasi yang kompleks, karena DashMap sudah mengurusnya di dalam implementasinya sendiri.

Pola Singleton, di sisi lain, adalah pola desain yang memastikan bahwa sebuah kelas hanya memiliki satu instans dan menyediakan titik akses global ke instans tersebut. Dalam konteks Rust, pola Singleton biasanya diimplementasikan menggunakan lazy_static untuk menjamin inisialisasi yang aman pada saat runtime.

Pertanyaannya adalah apakah DashMap masih diperlukan jika kita menerapkan pola Singleton? Jawabannya bergantung pada persyaratan konkurensi dari aplikasi BambangShop:

- Jika aplikasi membutuhkan akses konkuren ke list of subscribers:
Dalam hal ini, DashMap sangat berguna karena memungkinkan akses konkuren yang aman tanpa perlu mengelola locks secara manual. Jika kita hanya menggunakan pola Singleton dengan HashMap biasa, kita harus menambahkan mekanisme sinkronisasi sendiri, seperti menggunakan Mutex atau RwLock, yang bisa membuat kode lebih rumit dan berpotensi mengurangi kinerja.

- Jika aplikasi tidak membutuhkan akses konkuren atau hanya dijalankan dalam konteks single-thread:
Dalam skenario ini, pola Singleton dengan HashMap mungkin sudah cukup dan penggunaan DashMap mungkin terlalu berlebihan karena menambahkan overhead untuk keselamatan thread yang tidak diperlukan.

#### Reflection Publisher-2

#### Reflection Publisher-3
