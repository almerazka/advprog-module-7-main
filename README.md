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
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
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
    
<details>
    <summary><strong> Reflection Publisher-2 💡 </strong></summary>
    
1. **In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?**
    > Memisahkan **Service** dan **Repository** dari _Model_ dalam arsitektur **MVC** merupakan langkah yang penting untuk menjaga keteraturan dan kemudahan pemeliharaan kode. Jika semua tanggung jawab, seperti representasi data, akses ke _database_, dan logika bisnis diletakkan dalam _Model_, maka kode akan menjadi sulit dipahami, sulit diuji, serta rentan terhadap perubahan yang tidak terkontrol. Dengan pemisahan ini, **Repository** hanya bertanggung jawab untuk menangani operasi **CRUD** _(Create, Read, Update, Delete)_ terhadap data, seperti membaca atau menulis ke dalam _database_.
    
    > Sementara itu, **Service** berfungsi untuk mengelola logika bisnis, termasuk bagaimana data diproses sebelum dikirim ke **controller**. Pendekatan ini sejalan dengan prinsip **Single Responsibility (SRS)** dalam **SOLID**, yang menekankan bahwa setiap komponen dalam sistem harus memiliki tanggung jawab yang jelas dan spesifik. Selain meningkatkan keteraturan kode, pemisahan ini juga membuat sistem lebih fleksibel. Jika suatu saat nanti diperlukan perubahan pada sistem penyimpanan data, misalnya mengganti _database_ yang digunakan, maka hanya bagian **Repository** yang perlu disesuaikan tanpa mengubah logika bisnis di **Service** atau bagian lainnya. Selain itu, pengujian kode juga menjadi lebih mudah karena setiap komponen dapat diuji secara terpisah tanpa saling memengaruhi. Dengan demikian, pendekatan ini menghasilkan kode yang lebih bersih, terstruktur, dan mudah dikembangkan di masa depan.
  
2.  **What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?**
    >  Jika kita hanya memakai Model seperti **Program**, **Subscriber**, dan **Notification** tanpa memisahkan **Service** dan **Repository**,  semua proses—mulai dari penyimpanan data sampai logika bisnis—akan tertumpuk dalam satu tempat. Akibatnya, kompleksitas kode meningkat, sulit dipahami, dan makin susah dipelihara. Bayangin aja, jika **Program**, **Subscriber**, dan **Notification** langsung saling terhubung tanpa pemisahan tugas, perubahan di satu bagian bisa membuat efek domino ke bagian lain yang mungkin tidak ada hubungannya.
    
    > Selain itu, Model akan memiliki terlalu banyak tanggung jawab, mulai dari menangani _query database_ hingga mengirim notifikasi ke API eksternal. Ini bertentangan dengan **Single Responsibility Principle (SRP)** karena satu komponen harus menangani berbagai tugas yang seharusnya dipisahkan. Akibatnya, kode menjadi sulit diuji, rentan terhadap duplikasi, serta kurang fleksibel ketika terjadi perubahan. Dengan demikian, tanpa pemisahan antara Model, **Service**, dan **Repository**, _maintainability_ akan berkurang sehingga sistem menjadi lebih sulit untuk dikembangkan di masa depan.

3. **Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects**
    > Selama perkuliahan, saya sudah beberapa kali menggunakan **Postman** untuk menguji API. Menurut saya,**Postman** sangat membantu karena memungkinkan saya mengirim request ke server tanpa perlu membuat tampilan terlebih dahulu. Dengan alat ini, saya bisa langsung menguji berbagai metode seperti **GET**, **POST**, **PUT**, dan **DELETE**, sehingga proses _debugging_ dan pengembangan menjadi lebih efisien. Beberapa fitur yang menurut saya sangat berguna adalah _Collections_ untuk menyimpan dan mengelola request, _Environment Variables_ untuk mempermudah konfigurasi API, serta _Automated Testing_ untuk memastikan API bekerja sesuai harapan.
    
    > Selain itu, _API Documentation_ di **Postman** memungkinkan tim bekerja lebih efektif dengan dokumentasi yang lebih terstruktur, sehingga semua anggota tim dapat memahami bagaimana API bekerja tanpa perlu komunikasi berulang. **Postman** juga memiliki fitur _Mock Server_ yang memungkinkan pengujian frontend tetap berjalan meskipun _backend_ belum sepenuhnya siap, sehingga pengembangan bisa dilakukan lebih cepat dan paralel. Dengan berbagai fitur ini, Postman menjadi alat yang sangat bermanfaat dalam proyek pengembangan perangkat lunak karena dapat mempercepat pengujian, mengurangi _error_, serta mempermudah kolaborasi dan integrasi antara _frontend_ dan _backend_.
</details>

<details>
    <summary><strong> Reflection Publisher-3 💡 </strong></summary>
  
1. **Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?**
    > Dalam kasus tutorial ini, pola **Observer** yang digunakan adalah **Push Model**. Pada **Push Model**, _publisher_ secara aktif mengirimkan data ke _subscriber_ setiap kali terjadi perubahan data (**CRUD**). Dalam konteks tutorial ini, setelah pengguna berlangganan (_subscribe_) ke suatu jenis produk, perubahan ini memeicu **NotificationService** untuk mengiterasi list _subscriber_ dan program secara otomatis akan mengirim notifikasi kepada mereka setiap kali ada produk baru, promosi, atau penghapusan produk. **Subscriber** tidak perlu meminta (_pull_) informasi secara manual, mereka hanya menerima data yang dikirim oleh _publisher_. Data yang dikirim DISINI mencakup detail produk, status, dan informasi _subscriber_.
    
    > Sebaliknya, pada **Pull Model**, _subscriber_ hanya akan mendapatkan data ketika mereka secara aktif meminta (_request_) informasi dari _publisher_. Namun, dalam kasus ini, _subscriber_ tidak meminta informasi secara langsung, melainkan hanya menunggu notifikasi dikirimkan oleh sistem, sehingga lebih cocok dengan konsep **Push Model**.
  
2.  **What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)**
    >  Jika kita memakai **Pull Model**, _subscriber_ hanya akan mendapat notifikasi jika terdapat perubahan data, tetapi detail datanya baru diambil ketika memang dibutuhkan sehingga lebih fleksibel dan tidak menerima notifikasi yang tidak diperlukan. Beban _publisher_ juga berkurang karena tidak perlu terus-menerus mengirim data ke semua _subscriber_. Namun, kelemahannya adalah _subscriber_ harus aktif mengecek perubahan, yang bisa menambah beban kerja dan menyebabkan _delay_ karena informasi tidak langsung diterima secara _real-time_. Selain itu, hal ini bisa menjadi lebih boros _resource_ jika _subscriber_ terlalu sering melakukan pengecekan meskipun tidak ada update baru. Dalam kasus ini, **Push Model** lebih cocok karena memastikan _subscriber_ langsung mendapatkan informasi terbaru tanpa harus terus melakukan permintaan ke _publisher_.

3. **Explain what will happen to the program if we decide to not use multi-threading in the notification process.**
    > Jika program tidak menggunakan _multi-threading_ dalam proses notifikasi, semua notifikasi akan dikirim secara satu per satu dalam satu _thread_ utama. Hal ini menyebabkan beberapa masalah, terutama dalam hal efisiensi dan performa. Saat ada banyak _subscriber_, proses pengiriman notifikasi akan menjadi lebih lambat (_bottle-neck_) karena harus menunggu setiap notifikasi dikirim sebelum melanjutkan ke yang berikutnya. Akibatnya, _publisher_ bisa mengalami keterlambatan dalam menangani _request_ baru, terutama jika setiap notifikasi membutuhkan waktu lama untuk diproses.
    > Selain itu, tanpa _multi-threading_, program bisa menjadi kurang responsif, terutama jika ada proses lain yang berjalan bersamaan, seperti _update_ data atau penerimaan _request_ baru. Semua operasi akan berjalan secara berurutan (_synchronous_), sehingga jika ada satu _subscriber_ yang lambat dalam menerima notifikasi, seluruh sistem bisa ikut terhambat/_freeze_. Dengan _multi-threading_, notifikasi bisa dikirim secara paralel, sehingga _publisher_ tetap bisa menjalankan tugas lain tanpa harus menunggu setiap _subscriber_ selesai menerima notifikasi.

</details>  
