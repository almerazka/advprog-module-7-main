# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

- Nama : Muhammad Almerazka Yocendra
- NPM : 2306241745
- Kelas : Pemrograman Lanjut - A

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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
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

<details>
    <summary><strong> Reflection Publisher-1 💡 </strong></summary>
    
1. **In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?**
    > Dalam **Observer Pattern**, **Subscriber** biasanya didefinisikan sebagai _interface_ atau _trait_ (dalam Rust) agar berbagai jenis **subscriber** dapat memiliki metode yang sama meskipun perilakunya berbeda. Hal ini memungkinkan setiap jenis **subscriber** untuk merespons pembaruan dengan cara yang sesuai tanpa mengubah struktur utama. Namun, dalam kasus ***BambangShop***, saat ini hanya terdapat satu jenis Subscriber yang formatnya tetap, yaitu hanya menyimpan URL dan nama, tanpa adanya variasi dalam perilaku. Oleh karena itu, penggunaan _trait_ belum diperlukan, dan cukup menggunakan satu _struct model_ saja.
    
    > Meskipun demikian, jika di masa mendatang terdapat berbagai jenis subscriber dengan cara kerja yang berbeda—misalnya ada **subscriber** yang menerima notifikasi melalui email, SMS, atau _webhook_—maka penggunaan _trait_ akan lebih fleksibel. Dengan adanya _trait_, setiap **subscriber** dapat memiliki implementasi sendiri namun tetap konsisten dalam cara mereka menerima _update_.

2.  **id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?**
    >  Karena id di Program dan url di **Subscriber** dimaksudkan untuk unik, maka sebenarnya penggunaan **DashMap** lebih tepat dibandingkan **Vec**. Dalam **Vec**, keunikan tidak dijamin secara otomatis, sehingga untuk memastikan tidak ada duplikasi, kita harus melakukan iterasi manual setiap kali menambahkan atau mengecek data. Hal ini kurang efisien, terutama jika jumlah data terus bertambah. Sebaliknya, **DashMap** secara otomatis menjamin keunikan karena bekerja seperti _HashMap_, di mana setiap key harus unik. Dengan demikian, kita tidak perlu melakukan pengecekan manual untuk memastikan keunikan id dan url, yang membuat pengelolaan data menjadi lebih sederhana dan efisien.
    
    >  Jika dari segi performa, **DashMap** lebih unggul karena operasi pencarian, penyisipan, dan penghapusan data dilakukan dalam ***O(1)***, sedangkan **Vec** memerlukan ***(O(n))*** karena harus menelusuri elemen satu per satu. Selain itu, **DashMap** juga lebih aman dalam lingkungan  _multi-threading_, yang bisa berguna jika ada proses paralel yang mengakses data secara bersamaan. Jadi, meskipun **Vec** bisa digunakan untuk menyimpan data dalam kasus sederhana, **DashMap** lebih direkomendasikan karena menjamin keunikan data dan meningkatkan efisiensi dalam pengelolaan serta aksesnya.

3. **When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?**
    > Dalam kasus ini, penggunaan **DashMap** tetap diperlukan meskipun kita bisa menerapkan **Singleton** _pattern_. **Singleton** memang memastikan hanya ada satu _instance_ dari ***List of Subscribers*** di seluruh aplikasi, tetapi itu saja tidak cukup untuk menangani akses bersamaan oleh banyak _thread_ dalam kata lain tidak otomatis membuatnya _thread-safe_.
    
    > **Rust** sendiri mengutamakan _thread safety_, dan jika kita hanya menggunakan **Singleton** dengan _HashMap_ biasa, kita perlu tambahan _synchronization mechanism_ seperti **Mutex** atau **RwLock**. Masalahnya, ini bisa menyebabkan _bottleneck_, terutama jika ada banyak _thread_ yang ingin membaca atau menulis data secara bersamaan. Di sisi lain, **DashMap** sudah didesain untuk menangani _concurrency_ tanpa perlu _locking_ secara eksplisit. Artinya, kita tetap bisa baca dan tulis data dari banyak _thread_ tanpa khawatir mengalami _race condition_.
</details>
    
#### Reflection Publisher-2

#### Reflection Publisher-3
