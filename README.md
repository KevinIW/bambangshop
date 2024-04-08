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

Dalam pola desain Observer, Subscribers biasanya mewakili antarmuka atau trait pengamat, yang menentukan kontrak yang harus dipatuhi oleh kelas atau struktur pengamat konkret. Meskipun memungkinkan untuk menerapkan pola tersebut tanpa antarmuka atau trait eksplisit untuk subscriber, menggunakan satu memberikan beberapa keuntungan kunci. 

Pertama, abstraksi terhadap detail implementasi spesifik dari subscriber konkret, mempromosikan fleksibilitas dan perluasan dengan memungkinkan berbagai jenis subscriber diperlakukan secara seragam. Kedua, encapsulasi perilaku subscriber dalam sebuah antarmuka atau trait memungkinkan perubahan pada implementasi internal dari subscriber konkret tanpa memengaruhi bagian lain dari sistem, membantu dalam modularisasi kode dan manajemen kompleksitas. 

Selain itu, menggunakan antarmuka atau trait memfasilitasi perilaku polimorfik, memungkinkan beberapa implementasi subscriber konkret digunakan secara bergantian dan mempromosikan penggunaan kembali kode. Secara keseluruhan, meskipun memungkinkan untuk menerapkan pola Observer tanpa antarmuka atau trait subscriber eksplisit, menggunakan satu sesuai dengan prinsip-prinsip desain berorientasi objek dan meningkatkan kekokohan dan keberlanjutan dari kode.


2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?


Dalam situasi di mana identifikasi unik (seperti id dalam Program dan url dalam Subscriber) menjadi sangat penting, pemilihan antara Vec (daftar) dan DashMap (peta/kamus) tergantung pada persyaratan spesifik dari sistem tersebut. Meskipun Vec bisa mencukupi untuk hanya memelihara koleksi elemen, memastikan keunikan di dalamnya akan membutuhkan iterasi yang tidak efisien melalui seluruh daftar untuk memvalidasi identifikasi. Pendekatan ini menjadi tidak praktis seiring dengan bertumbuhnya dataset menjadi lebih besar. Di sisi lain, DashMap menawarkan operasi pencarian yang efisien berdasarkan kunci dan menjamin keunikan di dalam struktur data, menjadikannya cocok untuk situasi di mana mempertahankan identifikasi unik sangat penting.

Dengan kebutuhan akan identifikasi unik dan potensi kebutuhan akan operasi pencarian yang efisien, penggunaan DashMap muncul sebagai opsi yang lebih cocok. Dengan menggunakan DashMap, sistem memastikan integritas identifikasi unik dan efisiensi kinerja. Dengan DashMap, sistem dapat dengan cepat memvalidasi keunikan identifikasi dan mengambil elemen yang sesuai tanpa menimbulkan beban iterasi melalui daftar yang mungkin besar. Pilihan ini tidak hanya menjaga integritas data tetapi juga meningkatkan kinerja keseluruhan sistem, memastikan responsifnya bahkan saat dataset berkembang. Oleh karena itu, dalam situasi di mana identifikasi unik menjadi fundamental, memanfaatkan kemampuan DashMap menyediakan solusi yang kokoh yang menggabungkan integritas dan kinerja untuk memenuhi tuntutan sistem secara efektif.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread-safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Dalam Rust, penegakan keamanan thread oleh kompilator memang menjadi aspek penting dari desain bahasa tersebut. Ketika mempertimbangkan penggunaan daftar pelanggan (SUBSCRIBERS) sebagai variabel statis, beberapa pertimbangan desain menjadi penting, termasuk memastikan keamanan thread dan mengelola status global dengan efektif.

Pola Singleton adalah pola desain pembuatan yang memastikan sebuah kelas hanya memiliki satu instansi dan menyediakan titik akses global ke instansi tersebut. Meskipun Pola Singleton bisa digunakan untuk mengelola status global, namun tidak selalu menjamin keamanan thread, terutama dalam lingkungan konkuren atau multithread.

Menggunakan pustaka seperti DashMap, yang menyediakan implementasi HashMap yang aman untuk thread, memastikan bahwa akses konkuren ke daftar pelanggan disinkronkan dengan benar, sehingga menghindari perlombaan data dan memastikan keamanan thread. Ini sangat penting dalam Rust, di mana kompilator menerapkan aturan ketat seputar konkurensi data.

Menerapkan Pola Singleton saja mungkin tidak cukup untuk memastikan keamanan thread dalam konteks ini. Bahkan jika instansi Singleton diinisialisasi secara malas atau cepat, akses konkuren kepadanya tanpa sinkronisasi yang tepat bisa menyebabkan kondisi perlombaan.

Oleh karena itu, meskipun Pola Singleton bisa digunakan bersama dengan mekanisme sinkronisasi lain untuk mengelola status global, memanfaatkan struktur data yang aman untuk thread seperti DashMap memberikan solusi yang lebih kokoh dan andal, terutama dalam Rust, di mana keamanan thread ditegakkan pada saat kompilasi. Dengan menggunakan DashMap, para pengembang dapat memastikan bahwa akses konkuren ke daftar pelanggan tetap aman dan bahwa program mematuhi jaminan konkurensi Rust yang ketat.
#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Memisahkan "Service" dan "Repository" dari Model dalam pola Model-View-Controller (MVC) merupakan praktik yang sesuai dengan prinsip-prinsip desain, termasuk Prinsip Tanggung Jawab Tunggal (SRP), Pemisahan Concerns (SoC), dan Prinsip Inversi Ketergantungan (DIP). Dengan demikian, setiap komponen memiliki tanggung jawab yang jelas dan terpisah, meningkatkan pemeliharaan dan meminimalkan risiko konsekuensi tak diinginkan saat melakukan perubahan. Hal ini juga memungkinkan organisasi kode yang lebih baik, memperjelas struktur aplikasi, dan memudahkan pemahaman.

Prinsip SRP menekankan bahwa sebuah kelas seharusnya hanya memiliki satu alasan untuk berubah, dan memisahkan fungsi-fungsi seperti penyimpanan data, logika bisnis, dan interaksi dengan sistem eksternal menjadi komponen-komponen terpisah memungkinkan hal ini tercapai. Pemisahan concerns juga mendukung modularitas dan pemakaian kembali kode, dengan memungkinkan layer Service dan Repository digunakan kembali di berbagai bagian aplikasi.

Dengan memisahkan concerns dan merancang komponen dengan prinsip DIP, kita dapat membuat aplikasi yang lebih fleksibel dan mudah diuji. Abstraksi yang diberikan oleh Service dan Repository memungkinkan untuk implementasi yang berbeda-beda, tanpa memengaruhi Model. Dengan demikian, keseluruhan pendekatan ini meningkatkan pemeliharaan, modularitas, dan fleksibilitas aplikasi secara keseluruhan.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?


Jika hanya menggunakan Model tanpa memisahkan fungsi-fungsi ke dalam komponen terpisah seperti Service dan Repository, kompleksitas kode akan meningkat dan pemeliharaan menjadi lebih sulit. Dalam skenario ini, setiap Model (Program, Subscriber, Notification) harus menangani baik penyimpanan data maupun logika bisnis, menyebabkan kode menjadi rumit dan saling terkait. Misalnya, Program dan Subscriber akan berinteraksi langsung tanpa lapisan Service, meningkatkan risiko kesalahan dan membuat kode sulit dimengerti. Begitu pula, Program akan bertanggung jawab atas manajemen konten notifikasi tanpa melibatkan lapisan Service, meningkatkan kompleksitas dan mengurangi kemampuan pemakaian kembali kode.

Tidak adanya pemisahan antara Model, Service, dan Repository juga mengakibatkan redundansi kode dan pengulangan fungsi di dalam setiap Model. Hal ini mempersulit pemeliharaan dan meningkatkan risiko kesalahan. Dengan memisahkan fungsi-fungsi ke dalam lapisan terpisah, seperti Service untuk logika aplikasi dan Repository untuk akses data, kompleksitas kode dapat dikelola dengan lebih baik. Lapisan Service memungkinkan kode bisnis untuk dipisahkan dari operasi-operasi data, meningkatkan keterbacaan dan pemeliharaan kode.

Secara keseluruhan, memisahkan fungsi-fungsi ke dalam komponen terpisah seperti Service dan Repository memungkinkan untuk manajemen kode yang lebih terstruktur dan mudah dipelihara. Dengan pendekatan ini, kode menjadi lebih modular dan kemampuan pemakaian kembali kode meningkat, sehingga meningkatkan efisiensi dan kualitas pengembangan perangkat lunak secara keseluruhan.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. Maybe you want to also list which features in Postman you are interested in or feel like it’s helpful to help your Group Project or any of your future software engineering projects.

Postman adalah alat pengembangan API yang banyak digunakan untuk memudahkan pengujian, debugging, dan kolaborasi selama proses pengembangan. Dengan beragam fitur yang ditawarkannya, Postman memungkinkan pengguna untuk mengirim permintaan HTTP ke API, menjalankan pengujian otomatis, dan mendefinisikan lingkungan serta variabel untuk pengujian di berbagai lingkungan. Fitur kolaborasi Postman juga memfasilitasi berbagi koleksi, berkolaborasi dalam pengujian, dan menyinkronkan perubahan di antara anggota tim.

Selain itu, Postman juga memungkinkan pengguna untuk menghasilkan dokumentasi API berdasarkan permintaan dan respons mereka, serta menawarkan fungsionalitas server tiruan untuk mensimulasikan endpoint dan respons API. Fitur pemantauan yang disediakan oleh Postman memungkinkan pengembang untuk memantau kinerja dan perilaku API di lingkungan produksi, sehingga masalah dapat diidentifikasi dan ditangani secara proaktif. Secara keseluruhan, dengan berbagai fitur yang ditawarkannya, Postman adalah alat serbaguna yang dapat menjadi tambahan berharga dalam proses pengembangan dan pengujian API, baik untuk pengembang individu maupun tim pengembangan.





#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Menurut saya, kita mengadopsi model Push karena dalam fungsi notify, publisher (self) melakukan perulangan pada daftar subscribers yang diperoleh dari repository subscribers, dan untuk setiap subscribers, melakukan pengiriman pemberitahuan dengan memanggil metode update pada subscribers yang bersangkutan dengan menggunakan muatan yang sesuai. Proses pengiriman pemberitahuan secara langsung dari publisher ke subscribers ini sesuai dengan model Push yang merupakan bagian dari Pola Pengamat. Dalam model ini, publisher aktif mengirimkan pembaruan kepada subscribers tanpa menunggu permintaan atau polling dari subscribers. Hal ini memungkinkan pemberitahuan untuk langsung dikirimkan saat terjadi perubahan, meningkatkan responsifitas sistem dan mengurangi latensi.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Jika kita mempertimbangkan penggunaan variasi Pull daripada Push untuk kasus tutorial ini, terdapat kelebihan dan kekurangan yang perlu diperhatikan. Kelebihannya termasuk keterkaitan yang lebih rendah antara publishers dan subscribers, karena subscribers harus secara aktif meminta pembaruan. Mereka memiliki lebih banyak kontrol atas kapan mereka menerima pembaruan, sehingga dapat menghindari pembaruan yang terlewat. Namun, dalam implementasinya, kompleksitas sistem akan meningkat karena subscribers harus mengelola logika pembaruan dan menangani kondisi kesalahan.

Sementara itu, kekurangan variasi Pull meliputi penundaan dalam menerima pembaruan, karena subscribers harus secara aktif menarik pembaruan dari publishers, yang dapat mempengaruhi kinerja real-time sistem. Penggunaan sumber daya juga meningkat karena subscribers yang terus-menerus melakukan polling pada publishers untuk pembaruan dapat mengakibatkan peningkatan lalu lintas jaringan dan penggunaan CPU. Dalam memilih antara Push dan Pull, perlu dipertimbangkan persyaratan spesifik dari sistem dan kompromi yang mungkin terjadi.

Secara keseluruhan, meskipun variasi Pull mengurangi ketergantungan dan memberikan lebih banyak kontrol kepada subscribers atas pembaruan, hal ini juga memperkenalkan kompleksitas tambahan dan potensi penundaan dalam menerima pembaruan. Pilihan antara Push dan Pull akan tergantung pada kebutuhan sistem serta pertimbangan terkait kinerja dan efisiensi penggunaan sumber daya.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.


ika multi-threading tidak digunakan dalam proses notifikasi, program akan menjalankan notifikasi secara berurutan dalam satu thread yang sama. Ini berarti ketika fungsi notify dipanggil, publishers (atau komponen lain yang memulai notifikasi) akan mengulangi daftar subscribers dan memberi tahu setiap subscriber satu per satu, secara berurutan.

Tanpa multi-threading, setiap notifikasi akan menghentikan eksekusi notifikasi berikutnya sampai notifikasi saat ini selesai. Hal ini dapat menyebabkan penundaan yang signifikan, terutama jika ada sejumlah besar subscribers atau jika proses notifikasi untuk setiap subscriber memerlukan waktu yang lama. Akibatnya, responsivitas keseluruhan sistem dapat terpengaruh, dan risiko bottleneck menjadi lebih tinggi, terutama selama periode penggunaan puncak.

Selain itu, jika proses notifikasi mengalami kesalahan atau membutuhkan waktu yang lama untuk selesai, hal tersebut dapat menunda atau mencegah notifikasi berikutnya dari disampaikan kepada subscribers lainnya. Ini dapat menyebabkan inkonsistensi dalam proses pengiriman notifikasi dan potensi masalah dengan integritas data.

Secara ringkas, tanpa multi-threading, proses notifikasi akan dijalankan secara berurutan dalam satu thread yang sama, yang dapat menyebabkan penundaan, responsivitas yang menurun, dan peningkatan risiko bottleneck dan kesalahan, terutama dalam skenario dengan konkurensi tinggi atau proses notifikasi yang panjang.











