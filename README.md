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
> In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Dalam kasus aplikasi BambangShop saat ini, subscriber hanya terdiri dari satu kelas, sehingga penggunaan interface atau trait belum diperlukan. Sebuah model tunggal sudah cukup untuk menangani kebutuhan yang ada. Namun, jika di masa depan BambangShop memiliki berbagai jenis subscriber dengan perilaku berbeda, maka penggunaan interface atau trait akan menjadi pilihan yang lebih baik. Dengan begitu, setiap subscriber dapat diimplementasikan secara fleksibel sambil tetap mengikuti kontrak yang telah ditetapkan, sehingga meningkatkan skalabilitas dan kemudahan pemeliharaan kode.

> id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Menurut saya, penggunaan DashMap dalam implementasi tutorial ini lebih optimal dibandingkan dengan Vec. Meskipun Vec bisa digunakan jika datanya masih kecil, DashMap lebih unggul dalam hal kinerja ketika mengelola subscriber. Hal ini disebabkan oleh perbedaan kompleksitas waktu, di mana DashMap memiliki pencarian O(1), sedangkan Vec membutuhkan O(n) dalam skenario terburuk. Jika jumlah subscriber meningkat di masa depan, penggunaan DashMap akan memastikan pencarian tetap cepat dan efisien, menjaga performa program tetap optimal.

>When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Dalam tutorial ini, Singleton telah diterapkan menggunakan lazy_static, dan di dalamnya digunakan DashMap. Dengan demikian, DashMap dan Singleton berjalan bersamaan dalam implementasi ini. Tujuan utama dari Singleton adalah memastikan hanya ada satu instance dari objek yang digunakan selama program berjalan, sedangkan DashMap adalah versi thread-safe dari HashMap yang memungkinkan akses bersamaan tanpa masalah race condition. Jika DashMap diganti dengan Singleton yang hanya menggunakan HashMap biasa, maka keamanan dalam lingkungan multithreading tidak akan terjamin. Oleh karena itu, mempertahankan DashMap adalah keputusan desain yang lebih baik untuk sistem yang membutuhkan kinerja tinggi dalam kondisi konkuren. Namun, jika aspek thread safety tidak menjadi prioritas, maka menggunakan Singleton dengan HashMap standar sudah cukup.
#### Reflection Publisher-2
> In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Pemisahan Service dan Repository mengacu pada prinsip Single Responsibility Principle (SRP) dalam pemrograman berorientasi objek (OOP). Jika Model menangani akses data sekaligus logika bisnis, maka kode akan menjadi lebih sulit dikelola, diperluas, dan diuji. Dengan memisahkan Repository dan Service, setiap bagian memiliki tanggung jawab spesifik: Repository berfokus pada interaksi dengan database, sedangkan Service menangani logika bisnis. Pemisahan ini membuat kode lebih modular, lebih mudah diuji, serta memungkinkan pengembangan tanpa harus mengubah banyak bagian dalam sistem.

> What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Jika Model menangani semuanya tanpa pemisahan, kompleksitas kode akan meningkat seiring bertambahnya fitur. Ketergantungan antar Model akan semakin besar, sehingga perubahan kecil bisa berdampak luas pada seluruh sistem. Selain itu, tanpa pemisahan yang jelas, akan ada banyak kode yang berulang, yang bertentangan dengan prinsip DRY (Don't Repeat Yourself). Akibatnya, proses refactoring menjadi lebih sulit, dan risiko kesalahan saat melakukan perubahan semakin tinggi. Dengan memisahkan Model, Service, dan Repository, pengembangan bisa lebih terstruktur dan mudah dipelihara.

> Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects

Saya sebelumnya telah menggunakan Postman dalam mata kuliah Pemrograman Berbasis Platform (PBP) untuk menguji API endpoints dengan lebih efisien. Postman membantu menampilkan data dari endpoint dengan jelas dan memungkinkan pengujian API tanpa harus menulis kode tambahan.

Fitur yang menurut saya menarik dan berguna dalam proyek ini maupun proyek mendatang adalah:
- Collections → Memungkinkan pengelompokkan request API yang berhubungan agar lebih mudah dikelola.
- Automated Testing → Memungkinkan penambahan skrip pengujian otomatis untuk memastikan setiap endpoint merespons sesuai ekspektasi.
- Environment Variables → Mempermudah pengelolaan konfigurasi API untuk berbagai lingkungan (misalnya, development dan production).
#### Reflection Publisher-3
> Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Dalam tutorial ini, pola Observer yang diterapkan adalah Push Model, di mana publisher (sistem) secara langsung mengirimkan notifikasi ke subscribers setiap kali terjadi perubahan status produk. Hal ini terlihat pada implementasi NotificationService::notify(), di mana notifikasi dikirim ke setiap subscriber menggunakan metode subscriber_clone.update(payload_clone). Dengan pendekatan ini, subscribers menerima update secara otomatis tanpa harus meminta data secara manual.

> What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Jika kita menggunakan Pull Model, salah satu keuntungannya adalah mengurangi beban publisher, karena publisher tidak perlu mengirimkan notifikasi secara langsung ke setiap subscriber. Sebaliknya, setiap subscriber dapat mengambil (pull) data sesuai dengan kebutuhan mereka.

Namun, kelemahan utama dari Pull Model adalah efisiensi yang lebih rendah, terutama jika jumlah subscribers banyak. Subscribers harus melakukan polling secara berkala untuk mengecek apakah ada perubahan status produk. Hal ini bisa menyebabkan overhead yang tidak perlu, karena mereka terus-menerus meminta data meskipun tidak ada perubahan. Dalam skenario ini, Push Model lebih efisien karena update dikirim hanya saat ada perubahan.

> Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Jika multithreading tidak digunakan, aplikasi akan menghadapi kesulitan dalam menangani notifikasi untuk banyak subscribers. Semua notifikasi harus dikirim satu per satu dalam satu thread, yang dapat menyebabkan bottleneck dan memperlambat eksekusi utama program.

Dengan menerapkan multithreading (misalnya dengan thread::spawn()), proses pengiriman notifikasi dapat dilakukan secara paralel, sehingga aplikasi tetap responsif dan dapat menangani banyak subscribers tanpa memperlambat proses utama. Hal ini sangat penting untuk meningkatkan kinerja dan skalabilitas sistem, terutama jika jumlah subscribers terus bertambah.
