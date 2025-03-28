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
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Subscriber model struct.`
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [X] Commit: `Implement add function in Subscriber repository.`
    -   [X] Commit: `Implement list_all function in Subscriber repository.`
    -   [X] Commit: `Implement delete function in Subscriber repository.`
    -   [X] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [X] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [X] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [X] Commit: `Implement publish function in Program service and Program controller.`
    -   [X] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [X] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Pada kasus BambangShop, karena hanya ada satu jenis Subscriber yang tidak memiliki perilaku bervariasi, maka kita tidak memerlukan interface/trait, cukup satu Model Struct.

2. Penggunaan DashMap lebih baik daripada Vec karena id dan url bersifat unik, sehingga DashMap bisa mencegah duplikasi. Selain itu, DashMap lebih efisien karena memiliki kompleksitas rata-rata O(1) dibandingkan Vec yang membutuhkan operasi linear dengan kompleksitas O(n).

3.  Singleton pattern memang membantu agar ada satu instance global, tapi tidak cukup jika kita butuh akses data yang bisa ditulis oleh banyak thread. DashMap sudah mengatasi masalah ini dengan aman dan efisien. Jadi, penggunaan DashMap dalam variabel statis SUBSCRIBERS adalah pilihan yang tepat, baik dari sisi performa maupun keamanan.

#### Reflection Publisher-2
1. Kita memisahkan Service dan Repository dari Model agar setiap komponen dalam aplikasi memiliki tanggung jawab yang jelas dan terpisah. Service berfungsi untuk menangani logika bisnis, sehingga tidak tercampur dengan cara data disimpan atau diambil. Sementara itu, Repository fokus pada pengelolaan data, seperti menyimpan, mengambil, atau menghapus dari sumber data. Pendekatan ini membuat kode lebih modular, mudah diuji, dan lebih mudah dirawat dalam jangka panjang.

2. Jika kita hanya menggunakan Model tanpa memisahkan Service dan Repository, maka Model akan menanggung terlalu banyak tanggung jawab. Hal ini menyebabkan kode menjadi kompleks dan sulit dirawat. Dalam aplikasi BambangShop, Model seperti Program, Subscriber, dan Notification akan saling berinteraksi secara langsung, sehingga setiap perubahan kecil pada satu Model bisa berdampak pada seluruh alur aplikasi. Kompleksitas akan meningkat karena tidak ada Service dan Repository yang menjadi jembatan komunikasi.

3. Postman sangat membantu saya untuk mengetes proyek aplikasi, terutama dalam pengujian API. Dengan Postman, saya bisa dengan mudah mengirim request HTTP ke endpoint yang saya buat, mengatur header, body, dan melihat respons secara real-time. Tool ini sangat berguna untuk memverifikasi bahwa logika yang saya bangun di service dan controller bekerja sesuai ekspektasi, tanpa perlu membuat frontend terlebih dahulu.

#### Reflection Publisher-3
1. Pada kasus tutorial ini, pola Observer yang digunakan mengikuti model Push. Dalam model ini, publisher secara otomatis mengirimkan informasi atau pembaruan kepada subscriber begitu ada perubahan atau kejadian tertentu. Misalnya, ketika ada produk baru atau produk yang diubah, publisher akan langsung mengirimkan notifikasi ke semua subscriber tanpa harus menunggu permintaan dari mereka.

2. Jika kita menggunakan model Pull, maka subscriber tidak menerima notifikasi secara otomatis saat peristiwa terjadi, melainkan mereka harus secara aktif memeriksa publisher untuk mendapatkan pembaruan. Kelebihan dari model Pull adalah subscriber dapat mengontrol kapan mereka ingin mendapatkan pembaruan data, yang bisa mengurangi jumlah data yang diterima dan menghemat sumber daya. Kekurangannya adalah model ini bisa menyebabkan latensi dalam pengiriman data, karena pembaruan hanya akan diterima saat subscriber melakukan permintaan. 

3. Jika kita tidak menggunakan multi-threading dalam proses notifikasi, program bisa berjalan lambat, terutama ketika jumlah subscriber sangat banyak. Tanpa multi-threading, proses notifikasi akan dijalankan secara berurutan, sehingga setiap notifikasi harus selesai diproses sebelum notifikasi berikutnya bisa dikirim. Hal ini bisa menyebabkan masalah karena proses notifikasi akan melambat dan bahkan dapat menyebabkan time-out atau kegagalan.
