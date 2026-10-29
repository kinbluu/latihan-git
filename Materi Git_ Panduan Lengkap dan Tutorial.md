

# **Panduan Komprehensif Git: Dari Konsep Fundamental hingga Alur Kerja Kolaboratif**

## **Bagian 1: Pengantar Git dan Kontrol Versi**

### **1.1 Apa Itu Kontrol Versi dan Mengapa Ini Penting?**

Dalam dunia pengembangan perangkat lunak modern, kompleksitas adalah keniscayaan. Proyek-proyek melibatkan ribuan hingga jutaan baris kode, dikerjakan oleh tim yang terdiri dari puluhan bahkan ratusan pengembang yang tersebar di berbagai lokasi geografis. Mengelola evolusi basis kode dalam lingkungan seperti ini tanpa alat yang tepat adalah resep untuk kekacauan. Di sinilah Sistem Kontrol Versi atau *Version Control System* (VCS) memainkan peran fundamental.

VCS adalah sebuah sistem yang bertugas merekam dan memantau setiap perubahan yang terjadi pada sekumpulan berkas dari waktu ke waktu.1 Bayangkan VCS sebagai "mesin waktu" untuk proyek Anda. Ia memungkinkan tim untuk:

1. **Melacak Riwayat Perubahan:** Setiap modifikasi, penambahan, atau penghapusan berkas dicatat secara permanen. Catatan ini mencakup informasi tentang siapa yang melakukan perubahan, kapan perubahan itu dibuat, dan apa saja yang diubah.2  
2. **Kembali ke Versi Sebelumnya:** Jika sebuah perubahan baru menimbulkan masalah atau *bug*, tim dapat dengan mudah kembali ke versi sebelumnya yang diketahui stabil, sebuah proses yang dikenal sebagai *rollback*.2  
3. **Memfasilitasi Kolaborasi Paralel:** Beberapa pengembang dapat bekerja pada bagian yang berbeda dari proyek yang sama secara bersamaan tanpa saling menimpa pekerjaan satu sama lain. VCS menyediakan mekanisme untuk menggabungkan perubahan-perubahan ini secara sistematis.3  
4. **Memahami Konteks Perubahan:** Dengan melihat riwayat, seorang pengembang dapat memahami mengapa sebuah baris kode tertentu ditulis atau diubah, memberikan konteks yang sangat berharga untuk pemeliharaan dan pengembangan lebih lanjut.

Tanpa VCS, tim pengembang akan terjebak dalam praktik yang tidak efisien dan rawan kesalahan, seperti menamai berkas secara manual (kode\_final\_v2\_benar.js, kode\_final\_v3\_revisi\_bos.js), saling mengirim potongan kode melalui email, atau yang terburuk, secara tidak sengaja menimpa pekerjaan rekan satu tim. VCS mengubah proses yang kacau ini menjadi alur kerja yang terstruktur dan aman. Oleh karena itu, penggunaan VCS bukan lagi sebuah pilihan, melainkan sebuah keharusan dan dianggap sebagai fondasi dari praktik rekayasa perangkat lunak modern, termasuk DevOps, arsitektur perangkat lunak, dan pola rilis berkelanjutan.3 Di antara berbagai VCS yang ada, Git telah muncul sebagai standar industri yang dominan, digunakan oleh jutaan pengembang di seluruh dunia untuk proyek pribadi hingga aplikasi perusahaan berskala besar.5

### **1.2 Sejarah dan Filosofi Git: Kelahiran dari Kebutuhan**

Kisah Git dimulai dari sebuah kontroversi dan kebutuhan mendesak dalam salah satu proyek perangkat lunak *open-source* terbesar di dunia: kernel Linux.6 Pada awal tahun 2000-an, pengembangan kernel Linux bergantung pada VCS komersial bernama BitKeeper. Namun, pada tahun 2005, hubungan antara komunitas kernel dan perusahaan di balik BitKeeper memburuk, yang menyebabkan akses gratis ke perangkat lunak tersebut dicabut.

Dihadapkan pada situasi ini, Linus Torvalds, pencipta Linux, memutuskan untuk membangun VCS-nya sendiri. Ia tidak puas dengan VCS yang ada pada saat itu, yang menurutnya terlalu lambat dan tidak cocok untuk skala dan sifat terdistribusi dari pengembangan kernel. Dari kebutuhan inilah, Git lahir.4

Torvalds merancang Git dengan serangkaian tujuan yang sangat spesifik, yang berakar dari pengalamannya mengelola proyek terdistribusi yang masif dan bergerak cepat. Filosofi desain ini menjadi DNA Git dan alasan utama kesuksesannya 7:

* **Kecepatan:** Setiap aspek Git dirancang untuk performa tinggi. Operasi seperti *commit*, *branching*, dan *merging* harus terasa instan, bahkan pada proyek-proyek yang sangat besar seperti kernel Linux.  
* **Desain Sederhana:** Meskipun kemampuannya kompleks, arsitektur inti Git didasarkan pada konsep-konsep sederhana yang dapat digabungkan untuk menciptakan alur kerja yang kuat.  
* **Dukungan Kuat untuk Pengembangan Non-Linear:** Pengembangan kernel melibatkan ratusan cabang (*branch*) yang berjalan secara paralel. Git harus unggul dalam membuat, mengelola, dan menggabungkan ribuan cabang ini dengan efisien.6  
* **Sepenuhnya Terdistribusi:** Tidak boleh ada ketergantungan pada server pusat. Setiap pengembang harus memiliki salinan lengkap dari seluruh riwayat proyek, memungkinkan mereka untuk bekerja secara independen dan *offline*.  
* **Mampu Menangani Proyek Besar secara Efisien:** Git harus mampu menangani basis kode sebesar kernel Linux tanpa penurunan kinerja yang signifikan, baik dari segi kecepatan maupun ukuran data.6

Sejak kelahirannya pada tahun 2005, Git telah berevolusi menjadi alat yang matang dan mudah digunakan, namun tetap mempertahankan kualitas-kualitas fundamental ini.6 Filosofi desain yang berfokus pada kecepatan, fleksibilitas, dan arsitektur terdistribusi inilah yang membedakan Git dari para pendahulunya dan menjadikannya alat yang sangat diperlukan dalam lanskap pengembangan perangkat lunak modern.

### **1.3 Arsitektur Terdistribusi (Git) vs. Terpusat (SVN): Pergeseran Paradigma**

Untuk memahami sepenuhnya inovasi yang dibawa Git, penting untuk membandingkan arsitekturnya dengan model dominan sebelumnya: *Centralized Version Control System* (CVCS), dengan Apache Subversion (SVN) sebagai contoh utamanya. Perbedaan mendasar antara keduanya terletak pada cara mereka menyimpan dan mengelola riwayat proyek.8

Model Terpusat (CVCS seperti SVN):  
Dalam model terpusat, terdapat satu server utama yang menyimpan seluruh riwayat versi proyek. Pengembang tidak memiliki salinan lengkap dari riwayat ini di mesin lokal mereka. Sebaliknya, mereka hanya "check out" versi terbaru dari berkas-berkas yang ingin mereka kerjakan. Untuk melakukan commit (menyimpan perubahan), melihat riwayat, atau berkolaborasi dengan orang lain, seorang pengembang harus memiliki koneksi jaringan ke server pusat tersebut.8 Arsitektur ini memiliki satu titik kebenaran (  
*single source of truth*), yang bisa menyederhanakan beberapa aspek administrasi, tetapi juga menciptakan beberapa kelemahan signifikan:

* **Satu Titik Kegagalan (*Single Point of Failure*):** Jika server pusat mengalami masalah atau tidak dapat diakses, tidak ada yang bisa berkolaborasi atau menyimpan perubahan versi mereka.10  
* **Ketergantungan Jaringan:** Hampir semua operasi (commit, diff, log) memerlukan komunikasi dengan server pusat, yang membuatnya lebih lambat dan tidak memungkinkan untuk bekerja secara efektif saat *offline*.9  
* ***Branching*** **yang Mahal:** Di SVN, membuat *branch* (cabang pengembangan) melibatkan penyalinan seluruh direktori di server. Ini adalah operasi yang berat dan memakan waktu, yang membuat pengembang enggan untuk sering membuat dan menggunakan *branch*.8

Model Terdistribusi (DVCS seperti Git):  
Git membalikkan model ini. Alih-alih hanya "check out" salinan kerja, setiap pengembang "mengkloning" (clone) seluruh repositori. Ini berarti setiap pengembang memiliki salinan lokal yang lengkap dari setiap berkas, setiap versi, dan setiap commit—seluruh riwayat proyek—di mesin mereka sendiri.1 Repositori lokal ini sepenuhnya fungsional. Server jarak jauh (seperti di GitHub atau GitLab) masih digunakan, tetapi perannya lebih sebagai titik sinkronisasi atau pusat kolaborasi, bukan sebagai "master" yang otoritatif.  
Pergeseran arsitektur ini adalah penyebab langsung dari hampir semua keunggulan yang dimiliki Git:

1. **Kecepatan Superior:** Karena seluruh riwayat ada di mesin lokal, operasi seperti melihat log (git log), membandingkan versi (git diff), dan melakukan *commit* menjadi sangat cepat. Tidak ada latensi jaringan yang terlibat.3  
2. **Kemampuan Kerja *Offline*:** Pengembang dapat melakukan *commit*, membuat *branch*, dan melihat riwayat proyek secara penuh bahkan tanpa koneksi internet. Mereka hanya perlu terhubung saat ingin menyinkronkan pekerjaan mereka dengan tim.3  
3. **Model *Branching* yang Revolusioner:** Di Git, sebuah *branch* hanyalah sebuah penunjuk (*pointer*) ringan ke sebuah *commit*. Membuat *branch* baru sama murahnya dengan membuat sebuah berkas kecil.12 Hal ini mendorong pengembang untuk menggunakan  
   *branch* secara ekstensif untuk setiap fitur, perbaikan, atau eksperimen, yang mengarah pada alur kerja yang lebih aman dan terorganisir.8  
4. **Redundansi Bawaan:** Karena setiap kloning adalah cadangan lengkap dari repositori, risiko kehilangan data akibat kerusakan server pusat secara drastis berkurang. Setiap salinan lokal dapat, pada prinsipnya, digunakan untuk memulihkan repositori pusat.3

Meskipun demikian, model terdistribusi juga memiliki beberapa pertimbangan. Git, secara desain, kurang efisien dalam menangani berkas biner yang sangat besar (misalnya, aset grafis atau video) karena setiap kloning akan mengunduh setiap versi dari berkas besar tersebut.8 Sebaliknya, SVN, yang hanya mengambil versi terbaru, seringkali lebih baik untuk proyek dengan banyak aset biner besar.8

Tabel berikut merangkum perbedaan paradigma antara kedua sistem ini.

**Tabel 1: Perbandingan Git (Terdistribusi) vs. SVN (Terpusat)**

| Fitur | Git (DVCS) | SVN (CVCS) | Implikasi |
| :---- | :---- | :---- | :---- |
| **Arsitektur Dasar** | Setiap pengembang memiliki salinan repositori lengkap (riwayat penuh). | Pengembang memiliki salinan kerja; riwayat penuh ada di server pusat. | Git memungkinkan otonomi penuh dan kerja *offline*; SVN bergantung pada server. |
| **Kinerja (Operasi Lokal)** | Sangat cepat. Operasi seperti commit, log, diff terjadi secara lokal. | Lebih lambat. Banyak operasi memerlukan komunikasi jaringan dengan server. | Alur kerja harian di Git terasa lebih responsif dan efisien. |
| **Kemampuan *Offline*** | Penuh. Semua operasi inti (kecuali sinkronisasi) dapat dilakukan *offline*. | Terbatas. Commit, log, dan kolaborasi memerlukan koneksi ke server. | Git sangat ideal untuk tim terdistribusi global dan pengembang yang sering bepergian. |
| **Model *Branching*** | Murah dan ringan. *Branch* adalah *pointer* sederhana. Mendorong penggunaan *branch* secara masif. | Mahal dan berat. *Branch* adalah salinan direktori di server. | Git memfasilitasi alur kerja paralel dan eksperimen yang aman; SVN tidak. |
| **Kolaborasi** | Fleksibel. Dapat mendukung alur kerja terpusat, terdistribusi, atau hibrida. | Terpusat. Semua perubahan harus melalui server pusat. | Git menawarkan lebih banyak model kolaborasi untuk disesuaikan dengan kebutuhan tim. |
| **Penanganan File Biner** | Kurang efisien. Kloning mengunduh semua versi dari file biner besar. | Lebih efisien. *Checkout* hanya mengunduh versi terbaru. | SVN seringkali lebih disukai untuk proyek dengan aset biner besar dan sering berubah. |
| **Titik Kegagalan** | Tidak ada titik kegagalan tunggal. Setiap klon adalah cadangan. | Server pusat adalah titik kegagalan tunggal. | Git secara inheren lebih tangguh terhadap kegagalan perangkat keras. |

## **Bagian 2: Konsep Fundamental dan Arsitektur Internal Git**

Untuk benar-benar menguasai Git, penting untuk memahami tidak hanya perintah-perintahnya, tetapi juga konsep-konsep dasar dan model data internal yang membuatnya bekerja. Arsitektur Git yang elegan adalah kunci dari kecepatan dan fleksibilitasnya.

### **2.1 Tiga Keadaan Inti: Siklus Hidup File**

Berbeda dengan banyak VCS lain yang beroperasi dengan model dua tahap (salinan kerja dan repositori), Git memperkenalkan arsitektur tiga tahap yang unik untuk melacak perubahan. Ketiga area ini adalah **Direktori Kerja** (*Working Directory*), **Area Staging** (*Staging Area* atau *Index*), dan **Repositori Git** (direktori .git).13 Memahami alur kerja di antara ketiga area ini adalah fundamental untuk menggunakan Git secara efektif.

1. **Direktori Kerja (*Working Directory*):** Ini adalah direktori proyek Anda di sistem berkas, tempat Anda secara aktif membuat, mengedit, dan menghapus berkas. Ini adalah satu *checkout* dari satu versi proyek.14 Setiap berkas di direktori kerja Anda dapat berada dalam salah satu dari dua keadaan:  
   *tracked* (dilacak oleh Git) atau *untracked* (tidak dilacak).15 Perubahan yang Anda buat di sini dianggap "dimodifikasi" (  
   *modified*) tetapi belum menjadi bagian dari riwayat proyek.13  
2. **Area Staging (*Staging Area* / *Index*):** Ini adalah area perantara yang sangat penting. *Staging Area* pada dasarnya adalah sebuah berkas di dalam direktori .git Anda yang menyimpan informasi tentang apa yang akan masuk ke dalam *commit* Anda berikutnya.14 Ia berfungsi sebagai "pratinjau" atau "keranjang belanja" untuk  
   *commit* Anda selanjutnya.17 Daripada melakukan  
   *commit* pada semua perubahan di direktori kerja Anda sekaligus, Anda dapat secara selektif "menambahkan" (*stage*) perubahan spesifik ke *Staging Area*.  
3. **Repositori Git (Direktori .git):** Ini adalah jantung dari proyek Git Anda. Direktori .git berisi semua metadata dan database objek (semua versi berkas Anda yang terkompresi) untuk proyek tersebut. Ketika Anda melakukan *commit*, Git mengambil *snapshot* dari berkas-berkas yang ada di *Staging Area* dan menyimpannya secara permanen di dalam direktori .git ini.13 Data di sini dianggap aman dan menjadi bagian dari riwayat permanen proyek Anda.

Siklus hidup dasar sebuah berkas dalam Git mengikuti alur ini:

* Anda **memodifikasi** sebuah berkas di **Direktori Kerja** Anda.  
* Anda memilih perubahan yang ingin Anda simpan dan memindahkannya ke **Area Staging** menggunakan perintah git add.  
* Anda mengambil *snapshot* permanen dari berkas-berkas di *Staging Area* dan menyimpannya di **Repositori Git** menggunakan perintah git commit.

Keberadaan *Staging Area* adalah lebih dari sekadar detail teknis; ia adalah sebuah fitur strategis yang memberdayakan pengembang. Tanpa area ini, setiap *commit* akan secara membabi buta merekam semua perubahan yang ada di direktori kerja. Hal ini sering kali akan mencampuradukkan beberapa tugas yang tidak terkait—seperti menambahkan fitur baru, memperbaiki *bug*, dan melakukan *refactoring*—ke dalam satu *commit* besar yang sulit dipahami.

Dengan *Staging Area*, seorang pengembang dapat memisahkan pekerjaannya. Misalnya, setelah mengubah tiga berkas untuk tiga tujuan berbeda, ia dapat menggunakan git add secara selektif untuk hanya men-stage perubahan yang terkait dengan perbaikan *bug*, lalu membuat *commit* untuk itu. Kemudian, ia dapat men-stage perubahan lain yang terkait dengan penambahan fitur dan membuat *commit* kedua yang terpisah. Kemampuan ini memungkinkan pembuatan **commit atomik**: unit perubahan terkecil yang logis, mandiri, dan dapat dijelaskan dengan satu pesan *commit*. Riwayat proyek yang dibangun dari *commit-commit* atomik jauh lebih mudah dibaca, ditinjau, dan dikelola, yang secara signifikan meningkatkan kualitas dan pemeliharaan basis kode dalam jangka panjang.17

\!([https\://git-scm.com/book/en/v2/images/areas.png](https://git-scm.com/book/en/v2/images/areas.png))

### **2.2 Anatomi Repositori Git: Objek Fundamental**

Di dalam direktori .git, Git tidak menyimpan berkas seperti yang kita lihat di direktori kerja. Sebaliknya, ia menggunakan database objek yang sangat efisien yang terdiri dari beberapa tipe objek fundamental. Memahami objek-objek ini memberikan wawasan tentang bagaimana Git mencapai kecepatan dan efisiensi penyimpanannya.7

Terdapat lima tipe objek utama dalam penyimpanan Git:

* **Blob (*Binary Large Object*):** Objek ini menyimpan konten mentah dari sebuah berkas. Sebuah blob tidak mengandung metadata apa pun seperti nama berkas, timestamp, atau izin; ia hanyalah data. Nama internal dari sebuah blob adalah hash SHA-1 dari kontennya. Ini berarti jika dua berkas di lokasi berbeda dalam proyek Anda memiliki konten yang identik, mereka akan menunjuk ke objek blob yang sama, sehingga menghemat ruang.7  
* **Tree:** Objek tree merepresentasikan satu direktori. Ia berisi daftar entri, di mana setiap entri terdiri dari mode berkas (misalnya, dapat dieksekusi), nama berkas/direktori, dan sebuah penunjuk (hash SHA-1) ke blob (jika entri tersebut adalah berkas) atau ke tree lain (jika entri tersebut adalah subdirektori). Sebuah tree secara efektif adalah *snapshot* dari struktur direktori pada satu titik waktu.7  
* **Commit:** Objek commit adalah yang mengikat semuanya menjadi sebuah riwayat yang koheren. Sebuah commit berisi:  
  * Sebuah penunjuk ke objek tree tingkat atas yang merepresentasikan *snapshot* lengkap dari proyek pada saat *commit* tersebut dibuat.  
  * Penunjuk ke satu atau lebih *commit* induk (*parent commits*). Biasanya ada satu induk. *Commit* pertama dalam sebuah repositori tidak memiliki induk, dan *merge commit* memiliki dua atau lebih induk.  
  * Metadata seperti nama dan email penulis, nama dan email *committer*, timestamp, dan pesan *commit* yang Anda tulis.7  
* **Tag:** Objek tag adalah cara untuk menandai sebuah *commit* spesifik, biasanya digunakan untuk menandai rilis (misalnya, v1.0.0). Sebuah *tag* berisi penunjuk ke sebuah *commit* dan dapat memiliki metadata tambahan, termasuk tanda tangan digital untuk verifikasi.7  
* **Packfile:** Untuk menghemat ruang dan meningkatkan efisiensi jaringan, Git tidak selalu menyimpan setiap objek sebagai berkas terpisah. Secara berkala, Git akan mengemas beberapa objek ke dalam satu berkas yang disebut packfile. Di dalam packfile, Git menyimpan hanya versi dasar dari sebuah berkas dan kemudian menyimpan *delta* (perbedaan) untuk versi-versi berikutnya, yang dikompresi bersama untuk mengurangi ukuran secara signifikan.7

Model data ini, di mana Git menyimpan *snapshot* lengkap (melalui tree dan blob) daripada perbedaan berkas, adalah kunci efisiensinya. Ketika Anda membuat *commit* baru dan sebuah berkas tidak berubah, objek tree yang baru hanya akan menunjuk kembali ke objek blob yang sudah ada, tanpa perlu menyimpan duplikat data.

### **2.3 Blok Bangunan Utama: Konsep yang Saling Terkait**

Dengan pemahaman tentang arsitektur internal, kita dapat mendefinisikan konsep-konsep tingkat tinggi yang digunakan dalam alur kerja sehari-hari. Konsep-konsep ini saling terkait erat dan membentuk fondasi operasional Git.

* **Repository:** Ini adalah wadah untuk seluruh proyek Anda. Ia menyimpan semua berkas, riwayat perubahan (dalam bentuk *commit*), dan konfigurasi. Terdapat dua jenis utama:  
  * **Repositori Lokal:** Salinan lengkap dari repositori yang ada di mesin Anda (di dalam direktori .git). Di sinilah Anda melakukan sebagian besar pekerjaan Anda.19  
  * **Repositori Remote:** Versi repositori Anda yang di-hosting di server (misalnya, di GitHub, GitLab, atau Bitbucket). Ini berfungsi sebagai titik pusat untuk kolaborasi dan sinkronisasi antar anggota tim.19  
* **Commit:** Sebuah *commit* adalah *snapshot* dari keadaan proyek Anda pada satu titik waktu tertentu. Setiap *commit* adalah unit dasar dari riwayat proyek. Ia memiliki pengidentifikasi unik (hash SHA-1) dan berisi pesan yang menjelaskan perubahan yang dibuat. Riwayat repositori Anda pada dasarnya adalah grafik dari *commit-commit* ini yang saling terhubung.19  
* **Branch:** Sebuah *branch* adalah jalur pengembangan yang independen. Secara teknis, ini hanyalah sebuah penunjuk (*pointer*) yang dapat dipindahkan yang menunjuk ke sebuah *commit*. Ketika Anda membuat *commit* baru, *branch* yang sedang aktif akan secara otomatis bergerak maju untuk menunjuk ke *commit* baru tersebut. *Branching* memungkinkan Anda untuk bekerja pada fitur baru atau perbaikan *bug* secara terisolasi tanpa mengganggu pekerjaan di *branch* lain (seperti *branch* utama main).12  
* **Merge:** *Merge* adalah proses menggabungkan kembali riwayat yang telah terpisah. Perintah git merge mengambil perubahan dari satu *branch* dan mengintegrasikannya ke dalam *branch* lain. Proses ini sering kali menghasilkan sebuah *merge commit* khusus yang memiliki dua *commit* induk, secara simbolis menyatukan dua jalur pengembangan yang berbeda.19

Hubungan antara konsep-konsep ini dapat diringkas sebagai berikut: Anda bekerja di dalam sebuah **Repository**. Anda merekam perubahan Anda dalam unit-unit atomik yang disebut **Commit**. Anda melakukan pekerjaan ini di sepanjang **Branch** untuk mengisolasi pengembangan fitur atau perbaikan. Akhirnya, setelah pekerjaan selesai dan diuji, Anda menggunakan **Merge** untuk mengintegrasikan perubahan dari *branch* Anda kembali ke *branch* utama.

## **Bagian 3: Alur Kerja Dasar dan Perintah Esensial**

Setelah memahami konsep-konsep fundamental, langkah selanjutnya adalah mempelajari perintah-perintah dasar yang membentuk alur kerja harian seorang pengembang menggunakan Git. Perintah-perintah ini adalah alat untuk berinteraksi dengan tiga keadaan inti dan mengelola riwayat proyek.

### **3.1 Memulai Proyek: git init dan git clone**

Ada dua cara utama untuk memulai sebuah proyek dengan Git: menginisialisasi repositori baru dari awal, atau mengkloning repositori yang sudah ada.

* git init  
  Fungsi perintah ini adalah untuk mengubah sebuah direktori yang ada (yang mungkin sudah berisi berkas proyek) menjadi sebuah repositori Git. Ketika Anda menjalankan git init di dalam direktori proyek Anda, Git akan membuat sebuah subdirektori tersembunyi bernama .git. Direktori ini berisi semua file kerangka yang diperlukan Git untuk mulai melacak versi.21  
  * **Kapan Digunakan:** Gunakan git init ketika Anda memulai proyek baru dari nol di komputer lokal Anda dan ingin menempatkannya di bawah kontrol versi.  
  * **Contoh Penggunaan:**  
    Bash  
    \# Navigasi ke direktori proyek Anda  
    cd /path/to/my-project

    \# Inisialisasi repositori Git  
    git init

    Git akan merespons dengan pesan seperti Initialized empty Git repository in /path/to/my-project/.git/. Pada titik ini, belum ada berkas yang dilacak.23  
* git clone \[url\]  
  Fungsi perintah ini adalah untuk membuat salinan lokal yang identik dari sebuah repositori yang sudah ada dari lokasi remote. Perintah clone tidak hanya mengunduh berkas-berkas terbaru; ia mengunduh seluruh riwayat proyek, termasuk semua commit dan branch yang ada di repositori remote tersebut.21  
  * **Kapan Digunakan:** Gunakan git clone ketika Anda ingin mulai bekerja pada proyek yang sudah ada yang di-hosting di platform seperti GitHub, GitLab, atau server Git pribadi.  
  * **Contoh Penggunaan:**  
    Bash  
    \# Kloning repositori dari URL remote  
    git clone https\://github.com/libgit2/libgit2

    \# Kloning dan menamainya dengan nama direktori yang berbeda  
    git clone https\://github.com/libgit2/libgit2 my-custom-name

    Perintah ini akan membuat direktori baru (libgit2 atau my-custom-name), menginisialisasi direktori .git di dalamnya, menarik semua data dari repositori *remote*, dan secara otomatis melakukan *checkout* versi terbaru untuk Anda.23

### **3.2 Siklus Kerja Harian: git add, git commit, git status**

Siklus kerja inti dalam Git berputar di sekitar modifikasi berkas, men-stage perubahan tersebut, dan kemudian melakukan *commit*. Tiga perintah ini adalah yang paling sering digunakan dalam pengembangan sehari-hari.

* git status  
  Ini adalah perintah diagnostik utama Anda. Ia memberitahu Anda tentang keadaan repositori Anda saat ini. git status akan menampilkan informasi tentang branch mana Anda berada, berkas mana yang telah dimodifikasi di direktori kerja, berkas mana yang ada di staging area (siap untuk di-commit), dan berkas mana yang tidak dilacak oleh Git.15  
  * **Kapan Digunakan:** Jalankan perintah ini sesering mungkin. Sebelum add, setelah add, sebelum commit—ini adalah cara terbaik untuk memastikan Anda memahami apa yang akan Anda lakukan.  
  * **Contoh Penggunaan:**  
    Bash  
    git status

* git add \[file\]  
  Perintah ini memindahkan perubahan dari direktori kerja ke staging area. Ini memberitahu Git, "Saya ingin menyertakan perubahan saat ini pada berkas ini dalam commit saya berikutnya." Perintah add dapat digunakan untuk berkas baru yang belum dilacak atau untuk men-stage modifikasi pada berkas yang sudah dilacak.15  
  * **Kapan Digunakan:** Setelah Anda selesai membuat serangkaian perubahan yang logis dan ingin menyiapkannya untuk di-*commit*.  
  * **Contoh Penggunaan:**  
    Bash  
    \# Stage satu berkas  
    git add README.md

    \# Stage semua perubahan di direktori saat ini dan subdirektori  
    git add.

* git commit \-m "pesan"  
  Perintah ini mengambil semua perubahan yang telah Anda stage dengan git add dan menyimpannya sebagai snapshot permanen di repositori lokal Anda. Setiap commit adalah sebuah titik simpan dalam riwayat proyek Anda. Opsi \-m memungkinkan Anda untuk menyediakan pesan commit langsung dari baris perintah. Pesan commit yang baik bersifat singkat namun deskriptif, menjelaskan apa dan mengapa perubahan itu dibuat.21  
  * **Kapan Digunakan:** Setelah Anda men-stage semua perubahan yang relevan untuk satu unit kerja logis.  
  * **Contoh Penggunaan:**  
    Bash  
    git commit \-m "Add user authentication feature"

Siklus modify \-\> add \-\> commit ini adalah detak jantung pengembangan dengan Git. git status berfungsi sebagai stetoskop Anda, memungkinkan Anda memantau keadaan proyek Anda di setiap langkah.

### **3.3 Menjelajahi Riwayat Proyek: git log dan git diff**

Setelah Anda memiliki riwayat *commit*, Anda memerlukan alat untuk menjelajahi dan memeriksanya.

* git log  
  Perintah ini menampilkan riwayat commit untuk branch yang sedang aktif. Secara default, ia menampilkan daftar commit dalam urutan kronologis terbalik (yang terbaru di atas), lengkap dengan hash SHA-1, penulis, tanggal, dan pesan commit untuk setiap commit.25  
  * **Kapan Digunakan:** Ketika Anda perlu melihat *commit-commit* sebelumnya, memahami urutan perubahan, atau menemukan hash *commit* tertentu.  
  * **Contoh Penggunaan:**  
    Bash  
    \# Menampilkan log standar  
    git log

    \# Menampilkan log yang lebih ringkas, satu baris per commit  
    git log \--oneline

    \# Menampilkan log dengan grafik ASCII dari branch dan merge  
    git log \--graph \--oneline \--decorate

* git diff  
  Perintah ini digunakan untuk melihat perbedaan. Penggunaannya bervariasi tergantung pada argumen yang diberikan:  
  * git diff: Menampilkan perbedaan antara direktori kerja Anda dan *staging area*. Ini menunjukkan perubahan yang telah Anda buat tetapi belum Anda *stage*.  
  * git diff \--staged (atau \--cached): Menampilkan perbedaan antara *staging area* dan *commit* terakhir (HEAD). Ini menunjukkan perubahan yang telah Anda *stage* dan akan masuk ke *commit* berikutnya.  
  * git diff \[commit1\]\[commit2\]: Menampilkan perbedaan antara dua commit tertentu.

    13  
  * **Kapan Digunakan:** Sebelum men-stage perubahan (untuk meninjau apa yang telah Anda ubah), sebelum melakukan *commit* (untuk meninjau apa yang akan Anda *commit*), atau kapan pun Anda ingin membandingkan dua titik dalam riwayat proyek Anda.  
  * **Contoh Penggunaan:**  
    Bash  
    \# Lihat perubahan yang belum di-stage  
    git diff

    \# Lihat perubahan yang sudah di-stage  
    git diff \--staged

git log adalah mesin waktu Anda untuk melihat ke masa lalu, sementara git diff adalah kaca pembesar Anda untuk memeriksa perubahan secara detail.

### **3.4 Bekerja dengan Repositori Jarak Jauh: git remote, git push, git pull**

Meskipun Anda dapat melakukan banyak hal secara lokal, kekuatan sebenarnya dari Git terletak pada kolaborasi. Ini difasilitasi melalui interaksi dengan repositori *remote*.

* git remote  
  Perintah ini digunakan untuk mengelola koneksi Anda ke repositori remote. Anda dapat menambahkan, mengganti nama, dan menghapus koneksi. Saat Anda mengkloning repositori, Git secara otomatis membuat koneksi remote bernama origin yang menunjuk ke URL tempat Anda mengkloning.25  
  * **Kapan Digunakan:** Ketika Anda perlu menghubungkan repositori lokal Anda ke repositori *remote* baru atau mengelola koneksi yang ada.  
  * **Contoh Penggunaan:**  
    Bash  
    \# Menambahkan remote baru bernama 'upstream'  
    git remote add upstream https\://github.com/original-creator/repo.git

    \# Menampilkan daftar semua remote yang terkonfigurasi  
    git remote \-v

* git push \[alias\]\[branch\]  
  Perintah ini digunakan untuk mengunggah konten branch lokal Anda ke repositori remote. Ini adalah cara Anda membagikan commit yang telah Anda buat secara lokal dengan tim Anda. Perintah ini mengambil commit dari \[branch\] lokal Anda dan mengirimkannya ke \[branch\] yang sesuai di \[alias\] remote.21  
  * **Kapan Digunakan:** Setelah Anda melakukan beberapa *commit* secara lokal dan siap untuk membagikan pekerjaan Anda dengan orang lain.  
  * **Contoh Penggunaan:**  
    Bash  
    \# Push branch 'main' lokal ke remote 'origin'  
    git push origin main

* git pull  
  Perintah ini digunakan untuk mengambil perubahan dari repositori remote dan secara otomatis menggabungkannya ke dalam branch lokal Anda saat ini. git pull sebenarnya adalah gabungan dari dua perintah lain: git fetch (yang mengunduh konten dari remote tanpa mengubah keadaan lokal Anda) dan git merge (yang menggabungkan perubahan yang diunduh).21  
  * **Kapan Digunakan:** Sebelum Anda mulai bekerja setiap hari, atau kapan pun Anda ingin memperbarui repositori lokal Anda dengan perubahan terbaru dari tim.  
  * **Contoh Penggunaan:**  
    Bash  
    \# Fetch dan merge perubahan dari remote yang dilacak  
    git pull

Memvisualisasikan push sebagai "mengunggah" dan pull sebagai "mengunduh" adalah model mental yang berguna. push mempublikasikan pekerjaan Anda ke dunia luar, sementara pull menyinkronkan dunia luar dengan pekerjaan Anda.

Tabel berikut menyajikan ringkasan perintah-perintah esensial ini sebagai referensi cepat.

**Tabel 2: Perintah Git Esensial untuk Alur Kerja Harian**

| Perintah | Fungsi Utama | Contoh Penggunaan | Kapan Digunakan |
| :---- | :---- | :---- | :---- |
| **git init** | Menginisialisasi direktori sebagai repositori Git baru. | git init | Saat memulai proyek baru dari awal di mesin lokal. |
| **git clone** | Membuat salinan lokal dari repositori *remote*. | git clone \[url\] | Saat ingin berkontribusi atau menggunakan proyek yang sudah ada. |
| **git status** | Menampilkan status direktori kerja dan *staging area*. | git status | Sering, untuk memeriksa keadaan repositori sebelum dan sesudah operasi. |
| **git add** | Menambahkan perubahan ke *staging area*. | git add \[file\] atau git add. | Setelah memodifikasi berkas dan sebelum melakukan *commit*. |
| **git commit** | Menyimpan *snapshot* dari *staging area* ke riwayat lokal. | git commit \-m "Pesan" | Setelah men-stage satu unit kerja logis yang lengkap. |
| **git log** | Menampilkan riwayat *commit*. | git log \--oneline | Untuk meninjau perubahan sebelumnya atau menemukan *commit* tertentu. |
| **git diff** | Menampilkan perbedaan antara berbagai keadaan (kerja, *stage*, *commit*). | git diff atau git diff \--staged | Untuk meninjau perubahan secara detail sebelum add atau commit. |
| **git push** | Mengirim *commit* lokal ke repositori *remote*. | git push origin main | Saat siap membagikan pekerjaan yang telah di-*commit* dengan tim. |
| **git pull** | Mengambil dan menggabungkan perubahan dari *remote*. | git pull | Untuk memperbarui repositori lokal dengan pekerjaan terbaru dari tim. |
| **git branch** | Membuat, mendaftar, atau menghapus *branch*. | git branch nama-fitur | Saat akan memulai pekerjaan pada fitur atau perbaikan baru. |
| **git checkout** | Berpindah antar *branch* atau memulihkan berkas. | git checkout nama-fitur | Untuk beralih konteks pekerjaan ke *branch* lain. |
| **git merge** | Menggabungkan riwayat dari *branch* lain ke *branch* saat ini. | git merge nama-fitur | Setelah pekerjaan pada sebuah *branch* selesai dan siap diintegrasikan. |

## **Bagian 4: Menguasai Branching dan Merging**

Jika *commit* atomik adalah detak jantung Git, maka *branching* dan *merging* adalah sistem peredaran darahnya. Kemampuan Git untuk menangani *branch* dengan sangat efisien adalah fitur yang membedakannya dari VCS lain dan memungkinkan alur kerja kolaboratif yang kuat dan fleksibel.

### **4.1 Kekuatan Branching: Bekerja secara Paralel**

Di Git, sebuah *branch* adalah jalur pengembangan yang terisolasi. Ketika Anda ingin mengerjakan fitur baru atau memperbaiki *bug*, Anda membuat *branch* baru. Tindakan ini secara efektif menciptakan salinan dari kode Anda pada saat itu, memungkinkan Anda untuk membuat perubahan, bereksperimen, dan melakukan *commit* tanpa memengaruhi basis kode utama (biasanya di *branch* main atau develop).20

Pergeseran mental yang krusial bagi pengguna yang beralih dari sistem seperti SVN adalah memahami betapa murahnya *branching* di Git. Di SVN, membuat *branch* adalah operasi penyalinan direktori yang berat di sisi server, sehingga jarang dilakukan. Di Git, sebuah *branch* hanyalah sebuah berkas ringan (sekitar 41 byte) yang berisi hash SHA-1 dari *commit* yang ditunjuknya.12 Akibatnya, membuat

*branch* baru hampir instan dan tidak memakan ruang.

Implikasi dari hal ini sangat besar. Alur kerja Git mendorong pembuatan *branch* untuk *setiap* unit pekerjaan, tidak peduli seberapa kecil. Perbaikan *bug* satu baris? Buat *branch*. Eksperimen kecil? Buat *branch*. Ini memberikan manfaat luar biasa:

* **Isolasi:** Pekerjaan yang sedang berlangsung pada fitur baru yang belum stabil tidak akan pernah mengganggu stabilitas *branch* utama.  
* **Organisasi:** Setiap *branch* memiliki tujuan tunggal yang jelas, membuat riwayat proyek lebih mudah dipahami.  
* **Kolaborasi Paralel:** Beberapa pengembang dapat bekerja pada fitur yang berbeda secara bersamaan, masing-masing di *branch* mereka sendiri, tanpa saling mengganggu.

Perintah-perintah utama untuk mengelola *branch* adalah:

* **git branch \[nama-branch\]**: Perintah ini membuat *branch* baru. Penting untuk dicatat bahwa perintah ini *hanya* membuat *branch*; ia tidak beralih ke *branch* tersebut.12  
  Bash  
  \# Membuat branch baru bernama 'feature/user-profile'  
  git branch feature/user-profile

* **git checkout \[nama-branch\]** atau **git switch \[nama-branch\]**: Perintah ini digunakan untuk berpindah ke *branch* yang sudah ada. checkout adalah perintah tradisional yang serbaguna, sedangkan switch (diperkenalkan di Git 2.23) dirancang khusus untuk beralih *branch*, membuatnya lebih intuitif dan aman dari penggunaan yang tidak disengaja.12  
  Bash  
  \# Beralih ke branch yang baru dibuat  
  git switch feature/user-profile

* **git checkout \-b \[nama-branch-baru\]** (atau **git switch \-c \[nama-branch-baru\]**): Ini adalah perintah pintas yang sangat umum digunakan. Ia menggabungkan dua langkah di atas: membuat *branch* baru dan langsung beralih ke dalamnya.12  
  Bash  
  \# Membuat dan beralih ke branch baru dalam satu perintah  
  git switch \-c hotfix/login-bug

### **4.2 Mengintegrasikan Perubahan: git merge**

Setelah pekerjaan pada sebuah *branch* fitur selesai, diuji, dan siap untuk diintegrasikan, langkah selanjutnya adalah menggabungkannya kembali ke *branch* utama (misalnya, develop atau main). Proses ini dilakukan dengan perintah git merge.

git merge \[nama-branch\] mengambil perubahan dari \[nama-branch\] dan mengaplikasikannya ke *branch* tempat Anda berada saat ini (HEAD).19 Alur kerja standar untuk menggabungkan

*branch* feature/user-profile ke main adalah sebagai berikut:

1. **Pastikan *branch* utama Anda mutakhir:**  
   Bash  
   git switch main  
   git pull origin main

2. **Jalankan perintah merge:**  
   Bash  
   git merge feature/user-profile

3. **(Opsional) Hapus *branch* fitur setelah digabungkan:** Setelah *merge* berhasil dan Anda yakin tidak lagi memerlukan *branch* fitur, adalah praktik yang baik untuk menghapusnya agar repositori tetap bersih.  
   Bash  
   git branch \-d feature/user-profile

### **4.3 Studi Kasus Merge: Fast-Forward vs. Three-Way Merge**

Git secara otomatis menggunakan salah satu dari dua strategi *merge* yang berbeda, tergantung pada struktur riwayat *commit* Anda.

* Fast-Forward Merge:  
  Ini adalah skenario yang paling sederhana. Fast-forward merge terjadi ketika branch tujuan (main) tidak memiliki commit baru sejak branch fitur dibuat. Dengan kata lain, ada jalur lurus dan tak terputus dari ujung main ke ujung feature. Dalam kasus ini, Git tidak perlu melakukan penggabungan yang sebenarnya. Ia hanya "memajukan" (fast-forwards) penunjuk main agar menunjuk ke commit yang sama dengan branch fitur. Tidak ada merge commit baru yang dibuat, menghasilkan riwayat yang linear dan bersih.19  
  Contoh visual:  
  Sebelum Merge:  
  A---B---C (main)  
  \\  
  D---E (feature)  
  Setelah git merge feature:  
  A---B---C---D---E (main, feature)  
* Three-Way Merge (Recursive Merge):  
  Ini terjadi ketika riwayat telah menyimpang. Artinya, setelah branch fitur dibuat, ada commit baru yang ditambahkan baik ke branch fitur maupun ke branch main. Sekarang tidak ada lagi jalur lurus antara keduanya. Untuk menggabungkan ini, Git harus melakukan lebih banyak pekerjaan. Ia akan:  
  1. Menemukan leluhur bersama terdekat dari kedua *branch* (misalnya, *commit* B dalam contoh di atas).  
  2. Membandingkan *snapshot* dari tiga titik: leluhur bersama, ujung main, dan ujung feature.  
  3. Membuat *snapshot* baru yang menggabungkan perubahan dari kedua sisi.  
  4. Membuat merge commit baru yang istimewa karena memiliki dua induk: satu menunjuk ke ujung main dan satu lagi ke ujung feature.

     19

Contoh visual:Sebelum Merge:A---B---C (main)\\D---E (feature)Setelah git merge feature:A---B---C---F (main)\\ /D---E (feature)Di sini, F adalah merge commit baru.

### **4.4 Resolusi Konflik: Ketika Penggabungan Otomatis Gagal**

Terkadang, proses *merge* tidak berjalan mulus. Jika dua *branch* yang berbeda memodifikasi baris yang sama dalam berkas yang sama, Git tidak dapat memutuskan secara otomatis versi mana yang harus dipertahankan. Ini disebut **merge conflict**.34

Ketika konflik terjadi, Git akan menghentikan proses *merge* dan menunggu Anda untuk menyelesaikannya secara manual. Berikut adalah proses resolusi konflik:

1. **Identifikasi Konflik:** Git akan memberitahu Anda berkas mana yang berkonflik. Menjalankan git status akan memberikan daftar berkas dalam keadaan "unmerged".34  
2. **Buka Berkas yang Berkonflik:** Di dalam berkas tersebut, Git akan menyisipkan penanda visual untuk menunjukkan bagian yang bertentangan:  
   \<\<\<\<\<\<\< HEAD  
   // Kode dari branch Anda saat ini (mis. main)  
   \=======  
   // Kode dari branch yang sedang Anda merge (mis. feature)  
   \>\>\>\>\>\>\> feature/user-profile

   Bagian antara \<\<\<\<\<\<\< HEAD dan \======= adalah versi dari *branch* tujuan. Bagian antara \======= dan \>\>\>\>\>\>\> adalah versi dari *branch* sumber.34  
3. **Selesaikan Konflik:** Tugas Anda adalah mengedit berkas ini secara manual. Anda dapat memilih untuk mempertahankan salah satu versi, atau menggabungkan keduanya, atau menulis sesuatu yang sama sekali baru. Yang terpenting adalah Anda harus menghapus penanda konflik (\<\<\<\<\<\<\<, \=======, \>\>\>\>\>\>\>) dan membuat kode dalam keadaan akhir yang Anda inginkan.  
4. **Tandai sebagai Terselesaikan:** Setelah Anda selesai mengedit dan menyimpan berkas, Anda harus memberitahu Git bahwa konflik telah diselesaikan dengan men-stage berkas tersebut:  
   Bash  
   git add nama-berkas-yang-berkonflik.js

5. **Selesaikan Merge:** Setelah semua konflik diselesaikan dan di-stage, Anda dapat menyelesaikan proses *merge* dengan membuat *commit*:  
   Bash  
   git commit

   Git akan secara otomatis membuat pesan *commit* default yang menyatakan bahwa ini adalah *merge commit*, yang dapat Anda edit jika perlu.35

Menyelesaikan konflik bisa menjadi hal yang menakutkan pada awalnya, tetapi ini adalah bagian normal dari alur kerja kolaboratif. Dengan memahami prosesnya, Anda dapat menanganinya dengan percaya diri.

## **Bagian 5: Teknik Lanjutan dan Membatalkan Perubahan**

Seiring dengan meningkatnya keahlian Anda dalam menggunakan Git, Anda akan menemukan kebutuhan untuk melakukan operasi yang lebih canggih daripada siklus add/commit/push dasar. Bagian ini membahas beberapa teknik lanjutan yang kuat untuk mengelola pekerjaan Anda, membatalkan kesalahan, dan membentuk riwayat proyek Anda.

### **5.1 Menyimpan Pekerjaan Sementara: git stash**

Seringkali, Anda berada di tengah-tengah pekerjaan pada sebuah fitur ketika permintaan mendesak muncul—misalnya, memperbaiki *bug* kritis di *branch* produksi. Perubahan Anda saat ini belum siap untuk di-*commit*, tetapi Anda tidak bisa membawanya ke *branch* lain. Inilah skenario sempurna untuk git stash.

Perintah git stash mengambil semua perubahan yang belum di-*commit* di direktori kerja Anda (baik yang sudah di-*stage* maupun yang belum) dan menyimpannya dalam sebuah "tumpukan" (*stack*) sementara. Setelah itu, ia mengembalikan direktori kerja Anda ke keadaan *commit* terakhir (HEAD), memberi Anda area kerja yang bersih untuk beralih *branch* atau memulai tugas baru.38

Alur kerja umum menggunakan git stash adalah sebagai berikut:

1. **Menyimpan Perubahan:** Saat Anda berada di tengah pekerjaan, jalankan git stash.  
   Bash  
   \# Anda sedang bekerja, \`git status\` menunjukkan perubahan  
   git stash  
   \# Pesan akan muncul: "Saved working directory and index state..."  
   \# Sekarang \`git status\` akan menunjukkan direktori kerja yang bersih

2. **Beralih Konteks dan Bekerja:** Sekarang Anda bebas untuk git switch ke *branch* lain, melakukan perbaikan, dan melakukan *commit* tanpa khawatir tentang pekerjaan Anda yang belum selesai.  
3. **Melihat Daftar Stash:** Anda dapat menyimpan beberapa *stash*. Untuk melihat daftar semua *stash* yang telah Anda simpan, gunakan git stash list.  
   Bash  
   git stash list  
   \# Output:  
   \# stash@{0}: WIP on feature/user-profile:...  
   \# stash@{1}: WIP on feature/user-profile:...

4. **Menerapkan Kembali Perubahan:** Setelah Anda selesai dengan tugas mendesak dan kembali ke *branch* asli Anda, Anda dapat menerapkan kembali perubahan yang Anda simpan. Ada dua cara utama untuk melakukan ini:  
   * **git stash pop**: Perintah ini menerapkan *stash* yang paling baru dan, jika berhasil, menghapusnya dari tumpukan. Ini adalah yang paling umum digunakan.39  
     Bash  
     git stash pop

   * **git stash apply**: Perintah ini melakukan hal yang sama seperti pop, tetapi *tidak* menghapus *stash* dari tumpukan. Ini berguna jika Anda ingin menerapkan *stash* yang sama ke beberapa *branch*.39  
     Bash  
     \# Menerapkan stash kedua dalam daftar  
     git stash apply stash@{1}

git stash adalah alat yang sangat berharga untuk manajemen konteks yang fleksibel dalam alur kerja pengembangan yang sibuk.

### **5.2 Membatalkan Perubahan: git reset vs. git revert**

Membuat kesalahan adalah hal yang manusiawi, dan Git menyediakan alat yang kuat untuk membatalkan perubahan. Dua perintah utama untuk ini adalah git reset dan git revert. Meskipun keduanya dapat membatalkan *commit*, mereka melakukannya dengan cara yang sangat berbeda, dan memilih yang salah dapat menyebabkan masalah besar dalam lingkungan kolaboratif.

* git revert \[commit\]  
  git revert adalah operasi yang aman dan tidak merusak. Ia tidak mengubah atau menghapus riwayat yang ada. Sebaliknya, ia membuat commit baru yang merupakan kebalikan persis dari commit yang ingin Anda batalkan. Jika commit asli menambahkan baris kode, revert commit akan menghapus baris kode tersebut.43  
  * **Efek pada Riwayat:** Menambahkan ke riwayat. Riwayat asli tetap utuh.  
  * **Kapan Digunakan:** Ini adalah metode yang disukai untuk membatalkan perubahan pada **branch publik atau bersama** (seperti main atau develop). Karena ia tidak menulis ulang sejarah, Anda dapat dengan aman mendorong (*push*) *revert commit* tanpa menyebabkan konflik bagi anggota tim lain yang mungkin telah menarik (*pull*) *commit* asli.43  
* git reset \[commit\]  
  git reset adalah operasi yang berpotensi merusak. Ia bekerja dengan memindahkan penunjuk HEAD dan penunjuk branch saat ini ke commit yang ditentukan. Akibatnya, semua commit yang datang setelah titik reset tersebut secara efektif dihapus dari riwayat branch tersebut. Ini menulis ulang sejarah.43  
  * **Efek pada Riwayat:** Mengubah riwayat yang ada dengan menghapus *commit*.  
  * **Kapan Digunakan:** Gunakan git reset hanya pada **branch lokal dan pribadi** yang belum Anda bagikan dengan orang lain. Ini sangat berguna untuk membersihkan kesalahan atau *commit* yang tidak diinginkan sebelum Anda mendorongnya. Jangan pernah menggunakan git reset pada *branch* yang sudah di-*push* dan digunakan oleh orang lain.

Perbedaan fundamental ini berakar pada pentingnya menjaga integritas riwayat bersama. Riwayat pada repositori *remote* publik adalah sumber kebenaran bersama tim. Menulis ulang sejarah itu (seperti yang dilakukan reset) setelah dibagikan sama dengan merobek halaman dari buku catatan bersama; itu akan menyebabkan kebingungan dan konflik bagi semua orang yang sudah memiliki salinan buku tersebut. git revert, di sisi lain, seperti menambahkan entri baru di buku yang mengatakan, "Abaikan entri di halaman X." Ini menjaga integritas catatan sambil tetap mengoreksi kesalahan.

**Tabel 3: Perbandingan git reset vs. git revert**

| Aspek | git reset | git revert |
| :---- | :---- | :---- |
| **Efek pada Riwayat** | Menulis ulang riwayat (menghapus *commit*). | Menambahkan ke riwayat (membuat *commit* baru). |
| **Hasil Akhir** | Riwayat terlihat seolah-olah *commit* yang salah tidak pernah ada. | Riwayat secara eksplisit menunjukkan bahwa sebuah *commit* telah dibatalkan. |
| **Keamanan (Branch Publik)** | **Tidak Aman.** Dapat menyebabkan konflik besar bagi kolaborator. | **Aman.** Dirancang untuk membatalkan perubahan pada *branch* bersama. |
| **Kasus Penggunaan Umum** | Membatalkan *commit* lokal pada *branch* pribadi sebelum di-*push*. | Membatalkan *commit* yang sudah ada di *branch* main atau develop. |

### **5.3 Analisis Mendalam git reset: Mode \--soft, \--mixed, dan \--hard**

Perintah git reset menjadi lebih kuat (dan lebih kompleks) dengan adanya tiga mode operasi utama: \--soft, \--mixed, dan \--hard. Mode-mode ini menentukan seberapa jauh reset akan "membersihkan" setelah memindahkan penunjuk HEAD, dengan memengaruhi ketiga pohon Git: HEAD, *Staging Area* (Index), dan *Working Directory*.43

Mari kita asumsikan kita ingin membatalkan *commit* terakhir dengan git reset HEAD\~1:

* git reset \--soft HEAD\~1  
  Mode ini adalah yang paling tidak "merusak". Ia hanya melakukan satu hal: memindahkan penunjuk HEAD (dan branch saat ini) kembali satu commit. Staging Area dan Working Directory tidak tersentuh. Hasilnya adalah commit terakhir dibatalkan, tetapi semua perubahan dari commit tersebut tetap berada di Staging Area, siap untuk di-commit kembali (mungkin dengan beberapa perubahan tambahan).49  
  * **Gunakan untuk:** Menggabungkan beberapa *commit* terakhir menjadi satu, atau mengedit pesan *commit* terakhir tanpa kehilangan perubahan yang sudah di-*stage*.  
* git reset \--mixed HEAD\~1 (Mode Default)  
  Mode ini melakukan dua hal: memindahkan penunjuk HEAD, dan kemudian mengatur ulang Staging Area agar sesuai dengan keadaan HEAD yang baru. Working Directory tidak tersentuh. Hasilnya adalah commit terakhir dibatalkan, dan semua perubahan dari commit tersebut dipindahkan dari Staging Area kembali ke Working Directory sebagai perubahan yang belum di-stage (unstaged).49  
  * **Gunakan untuk:** Membatalkan *commit* terakhir dan mengerjakan ulang perubahan tersebut dari awal sebelum men-stage dan melakukan *commit* lagi.  
* git reset \--hard HEAD\~1  
  Mode ini adalah yang paling kuat dan berbahaya. Ia melakukan tiga hal: memindahkan penunjuk HEAD, mengatur ulang Staging Area, dan mengatur ulang Working Directory agar sesuai dengan keadaan HEAD yang baru. Hasilnya adalah commit terakhir dan semua perubahan terkait (baik yang di-stage maupun yang belum) akan dihapus sepenuhnya. Perubahan di Working Directory Anda akan hilang selamanya jika belum di-commit di tempat lain.48  
  * **Gunakan dengan sangat hati-hati untuk:** Benar-benar membuang *commit* terakhir dan semua pekerjaan terkait, kembali ke keadaan bersih dari *commit* sebelumnya.

**Tabel 4: Dampak Mode git reset pada Tiga Pohon Git**

| Mode Reset | HEAD (Pointer Branch) | Staging Area (Index) | Working Directory |
| :---- | :---- | :---- | :---- |
| **git reset \--soft HEAD\~1** | Pindah ke *commit* sebelumnya. | Tidak berubah. Perubahan dari *commit* yang dibatalkan tetap di-*stage*. | Tidak berubah. |
| **git reset \--mixed HEAD\~1** | Pindah ke *commit* sebelumnya. | Direset. Perubahan dari *commit* yang dibatalkan menjadi *unstaged*. | Tidak berubah. |
| **git reset \--hard HEAD\~1** | Pindah ke *commit* sebelumnya. | Direset. | Direset. Perubahan dari *commit* yang dibatalkan **dihapus**. |

### **5.4 Menulis Ulang Riwayat: git rebase vs. git merge**

Ketika Anda ingin mengintegrasikan perubahan dari satu *branch* ke *branch* lain, git merge adalah pilihan yang jelas dan aman. Namun, ada alternatif lain yang kuat: git rebase. Pilihan antara keduanya sering kali merupakan preferensi alur kerja tim dan filosofi tentang bagaimana seharusnya riwayat proyek terlihat.

* **git merge:** Seperti yang dibahas sebelumnya, merge mengambil dua riwayat *branch* dan menyatukannya dengan membuat *merge commit* baru. Ini **mempertahankan riwayat persis seperti yang terjadi**, termasuk kapan dan di mana percabangan dan penggabungan terjadi. Hasilnya adalah grafik riwayat yang akurat secara historis, tetapi bisa menjadi rumit dan sulit dibaca (sering disebut "grafik kereta api") jika banyak *merge* terjadi.19  
* **git rebase:** rebase mengambil pendekatan yang berbeda. Alih-alih menyatukan dua riwayat, ia **menulis ulang riwayat**. git rebase \[base-branch\] mengambil semua *commit* pada *branch* Anda saat ini yang tidak ada di \[base-branch\], menyimpannya sementara, memindahkan *branch* Anda ke ujung \[base-branch\], dan kemudian menerapkan kembali *commit-commit* yang disimpan satu per satu. Hasilnya adalah **riwayat yang linear dan bersih**, seolah-olah semua pekerjaan Anda dilakukan secara berurutan di atas pekerjaan terbaru dari *base branch*.53

**Kapan Menggunakan Masing-masing?**

* Gunakan **merge** ketika Anda ingin mempertahankan konteks historis yang tepat dari pekerjaan Anda. Ini adalah operasi non-destruktif dan aman untuk *branch* bersama.  
* Gunakan **rebase** ketika Anda ingin menjaga riwayat proyek tetap bersih dan linear. Ini sangat berguna untuk "membersihkan" *commit-commit* pada *branch* fitur pribadi Anda *sebelum* menggabungkannya ke *branch* utama.

Ini membawa kita ke aturan paling penting dalam menggunakan rebase:

The Golden Rule of Rebasing:  
Jangan pernah me-rebase commit yang telah Anda dorong (push) ke branch publik atau bersama.  
Ketika Anda me-*rebase* sebuah *branch*, Anda meninggalkan *commit* lama dan membuat *commit* baru yang identik secara konten tetapi memiliki hash SHA-1 yang berbeda. Jika anggota tim lain telah menarik *commit* asli dan mendasarkan pekerjaan mereka padanya, *rebase* Anda akan membuat riwayat Anda menyimpang dari riwayat mereka. Ketika Anda mencoba menyinkronkan lagi, Git akan melihat dua set *commit* yang berbeda yang berisi perubahan yang sama, menyebabkan kebingungan besar dan riwayat yang sangat berantakan.53

rebase adalah alat untuk membersihkan riwayat pribadi, bukan untuk mengubah riwayat bersama.

## **Bagian 6: Alur Kerja Kolaboratif untuk Tim Modern**

Git adalah alat yang sangat fleksibel, tetapi fleksibilitas ini bisa menjadi bumerang tanpa adanya konvensi yang disepakati. Alur kerja Git (*Git workflow*) adalah resep atau serangkaian pedoman yang diadopsi oleh tim untuk menggunakan Git secara konsisten dan produktif. Memilih alur kerja yang tepat sangat penting untuk memfasilitasi kolaborasi yang efektif, menjaga kualitas kode, dan menyederhanakan proses rilis.57

### **6.1 Model Kolaborasi Terstruktur: Feature Branch, Gitflow, Trunk-Based**

Ada beberapa model alur kerja yang populer, masing-masing dengan kekuatan dan kelemahannya sendiri, cocok untuk berbagai jenis tim dan proyek.

* Feature Branch Workflow:  
  Ini adalah fondasi dari hampir semua alur kerja modern. Ide intinya adalah bahwa semua pengembangan, baik untuk fitur baru maupun perbaikan bug, harus dilakukan di dalam branch khusus yang terisolasi, bukan langsung di branch utama (main). Nama branch harus deskriptif (misalnya, feature/add-login-page atau bugfix/fix-user-auth). Setelah pekerjaan di branch fitur selesai, ia diintegrasikan kembali ke main (biasanya melalui Pull Request).  
  * **Kelebihan:** Mengisolasi pekerjaan yang sedang berlangsung, menjaga main tetap stabil, dan memfasilitasi tinjauan kode untuk setiap unit pekerjaan.59  
  * **Paling Cocok Untuk:** Hampir semua tim dan proyek.  
* Gitflow Workflow:  
  Gitflow adalah model branching yang lebih ketat dan terstruktur yang diperkenalkan oleh Vincent Driessen. Ini sangat cocok untuk proyek dengan siklus rilis yang terjadwal dan kebutuhan untuk memelihara beberapa versi produksi. Gitflow mendefinisikan peran spesifik untuk beberapa branch yang berumur panjang:  
  * main (atau master): Hanya berisi kode produksi yang siap dirilis. Setiap *commit* di main adalah rilis baru dan harus diberi *tag* dengan nomor versi.  
  * develop: *Branch* integrasi utama untuk fitur-fitur. Semua *branch* fitur digabungkan ke sini. Ini mewakili keadaan "malam" dari proyek.  
  * feature/\*: Dibuat dari develop untuk mengerjakan fitur baru. Digabungkan kembali ke develop setelah selesai.  
  * release/\*: Dibuat dari develop ketika mendekati waktu rilis. *Branch* ini digunakan untuk persiapan rilis akhir, pengujian, dan perbaikan *bug* kecil. Setelah siap, ia digabungkan ke main (dan diberi *tag*) dan juga kembali ke develop.  
  * hotfix/\*: Dibuat dari main untuk memperbaiki bug kritis di produksi. Setelah selesai, ia digabungkan ke main dan develop.

    61  
  * **Kelebihan:** Sangat terstruktur, menyediakan alur yang jelas untuk rilis dan perbaikan darurat, ideal untuk manajemen versi eksplisit.  
  * **Kekurangan:** Bisa menjadi terlalu rumit untuk proyek yang lebih kecil atau proyek yang mempraktikkan *Continuous Delivery*.  
  * **Paling Cocok Untuk:** Proyek dengan siklus rilis yang terdefinisi (misalnya, perangkat lunak desktop, aplikasi seluler) dan yang perlu mendukung beberapa versi di lapangan.  
* Trunk-Based Development (TBD):  
  Berbeda dengan Gitflow yang memiliki banyak branch berumur panjang, TBD menganjurkan agar semua pengembang bekerja dalam branch berumur sangat pendek yang berasal dari satu branch utama yang disebut "trunk" (biasanya main). Pengembang mengintegrasikan perubahan kecil mereka ke trunk sesering mungkin, setidaknya sekali sehari. Alur kerja ini sangat bergantung pada serangkaian pengujian otomatis yang komprehensif (Continuous Integration) untuk memastikan bahwa trunk selalu dalam keadaan stabil dan siap untuk dirilis.60  
  * **Kelebihan:** Mengurangi secara drastis kemungkinan "neraka penggabungan" (*merge hell*), mendorong integrasi berkelanjutan, dan sangat cocok untuk *Continuous Delivery* dan DevOps.  
  * **Kekurangan:** Memerlukan disiplin tinggi dan infrastruktur pengujian otomatis yang matang.  
  * **Paling Cocok Untuk:** Tim yang mempraktikkan CI/CD, aplikasi web, dan proyek yang bertujuan untuk rilis cepat dan sering.

### **6.2 Kolaborasi Skala Besar: Forking Workflow**

Untuk proyek *open-source* atau situasi di mana banyak kontributor eksternal terlibat, model repositori pusat tunggal menjadi tidak praktis dari segi manajemen akses. *Forking Workflow* memecahkan masalah ini dengan cara yang elegan.

Alih-alih satu repositori pusat, setiap pengembang mendapatkan repositori sisi server mereka sendiri, yang merupakan "fork" (salinan) dari repositori resmi. Alur kerjanya adalah sebagai berikut:

1. **Fork:** Seorang kontributor membuat salinan pribadi dari repositori resmi di platform hosting (misalnya, mengklik tombol "Fork" di GitHub). Ini menciptakan username/project-repo sebagai salinan dari official-org/project-repo.65  
2. **Clone:** Kontributor kemudian mengkloning *fork* mereka sendiri (bukan repositori resmi) ke mesin lokal mereka.  
3. **Develop:** Mereka membuat *branch* fitur, melakukan *commit*, dan mendorong perubahan tersebut ke *fork* mereka di server.  
4. **Pull Request:** Setelah pekerjaan selesai, mereka membuka *Pull Request* dari *branch* fitur di *fork* mereka ke *branch* main di repositori resmi. Ini adalah permintaan kepada pengelola proyek untuk meninjau dan mengintegrasikan perubahan mereka.65

*Forking Workflow* pada dasarnya adalah model kontrol akses terdistribusi yang dibangun di atas arsitektur Git. Ia membalikkan model izin: alih-alih pengelola proyek harus secara proaktif memberikan akses tulis kepada setiap kontributor, kontributor dapat bekerja secara bebas di ruang mereka sendiri dan secara proaktif "menawarkan" perubahan mereka untuk ditinjau. Ini menciptakan gerbang kualitas alami di mana pengelola proyek memiliki kontrol penuh atas apa yang masuk ke basis kode resmi, menjadikannya model yang ideal untuk kolaborasi *open-source* berskala besar.65

### **6.3 Mekanisme Review Kode: Pull Request (GitHub) vs. Merge Request (GitLab)**

*Pull Request* (PR) dan *Merge Request* (MR) adalah nama yang berbeda untuk konsep yang sama. Ini bukanlah fitur inti dari Git itu sendiri, melainkan fitur yang disediakan oleh platform hosting seperti GitHub, GitLab, dan Bitbucket untuk memfasilitasi kolaborasi dan tinjauan kode.68

* **Pull Request (GitHub & Bitbucket):** Dinamakan demikian karena kontributor meminta pengelola proyek untuk "menarik" (*pull*) perubahan dari *branch* mereka ke *branch* utama.69  
* **Merge Request (GitLab):** Dinamakan demikian karena tujuan akhirnya adalah untuk "menggabungkan" (*merge*) *branch* sumber ke *branch* tujuan.69

Terlepas dari namanya, fungsionalitasnya hampir identik. PR/MR adalah pusat dari alur kerja kolaboratif modern. Mereka menyediakan antarmuka terpusat untuk:

* **Diskusi Kode:** Anggota tim dapat memberikan komentar pada baris kode tertentu, mengajukan pertanyaan, dan mendiskusikan implementasi.  
* **Tinjauan Kode (*Code Review*):** Pengelola atau rekan tim dapat meninjau perubahan, meminta perbaikan, dan akhirnya menyetujui perubahan tersebut.  
* **Integrasi Otomatis (CI/CD):** PR/MR dapat secara otomatis memicu *pipeline* *Continuous Integration*, yang menjalankan pengujian, pemeriksaan *linting*, dan pemeriksaan kualitas lainnya. Hasilnya ditampilkan langsung di antarmuka PR/MR, memberikan umpan balik cepat tentang apakah perubahan tersebut aman untuk digabungkan.72  
* **Kontrol Penggabungan:** Mereka menyediakan gerbang akhir sebelum kode masuk ke *branch* utama, memastikan bahwa hanya kode yang telah ditinjau dan diuji yang diintegrasikan.

PR/MR adalah jembatan antara aspek teknis Git (percabangan dan penggabungan) dan aspek sosial pengembangan perangkat lunak (kolaborasi, jaminan kualitas, dan berbagi pengetahuan).

**Tabel 5: Ringkasan Alur Kerja Kolaboratif Git**

| Alur Kerja | Deskripsi Singkat | Paling Cocok Untuk | Pro | Kontra |
| :---- | :---- | :---- | :---- | :---- |
| **Centralized** | Semua pengembang melakukan *commit* ke satu *branch* utama (main). | Tim yang sangat kecil atau proyek solo yang bertransisi dari SVN. | Sederhana dan mudah dipahami. | Tidak terukur, rawan konflik, main sering tidak stabil. |
| **Feature Branch** | Setiap fitur/perbaikan dikembangkan di *branch* terpisah. | Hampir semua tim sebagai fondasi alur kerja. | Isolasi pekerjaan, main stabil, memfasilitasi *code review*. | Memerlukan disiplin dalam mengelola *branch*. |
| **Gitflow** | Model *branching* yang ketat dengan *branch* main, develop, feature, release, hotfix. | Proyek dengan siklus rilis terjadwal dan manajemen versi. | Sangat terstruktur, alur rilis yang jelas, manajemen *hotfix* yang baik. | Terlalu rumit untuk CI/CD atau tim kecil. |
| **Trunk-Based** | Pengembang mengintegrasikan perubahan kecil dan sering ke *branch* main tunggal. | Tim yang mempraktikkan CI/CD dan rilis cepat (misalnya, aplikasi web). | Meminimalkan konflik *merge*, mendorong integrasi berkelanjutan. | Memerlukan pengujian otomatis yang kuat dan disiplin tinggi. |
| **Forking** | Setiap kontributor memiliki salinan repositori (*fork*) mereka sendiri di server. | Proyek *open-source* dan kolaborasi dengan pihak eksternal. | Kontrol akses terdistribusi, aman untuk kontribusi publik. | Alur kerja bisa lebih kompleks untuk kontributor baru. |

## **Bagian 7: Kesimpulan dan Praktik Terbaik**

Git telah merevolusi cara tim perangkat lunak berkolaborasi dan mengelola kode. Dari akarnya sebagai solusi yang dibuat khusus untuk pengembangan kernel Linux, ia telah tumbuh menjadi standar de facto untuk kontrol versi di seluruh industri. Kekuatannya tidak hanya terletak pada serangkaian perintahnya, tetapi pada filosofi desainnya yang mendasar: kecepatan, fleksibilitas, dan model terdistribusi yang memberdayakan pengembang.

### **7.1 Rangkuman Prinsip-Prinsip Kunci Git**

Pemahaman yang mendalam tentang Git dibangun di atas beberapa pilar konseptual:

* **Sifat Terdistribusi adalah Segalanya:** Hampir setiap keunggulan Git—kecepatan operasi lokal, kemampuan kerja *offline*, model *branching* yang superior, dan ketahanan terhadap kegagalan—berasal dari arsitekturnya di mana setiap pengembang memiliki salinan lengkap dari riwayat proyek.  
* **Staging Area Mendorong Kualitas Riwayat:** *Staging area* bukan hanya langkah perantara; ini adalah alat strategis yang memungkinkan pengembang untuk menyusun *commit* atomik yang bersih dan logis, yang secara dramatis meningkatkan keterbacaan dan pemeliharaan riwayat proyek.  
* ***Branching*** **Murah, Gunakan Sering:** *Branch* di Git adalah penunjuk ringan, bukan salinan direktori yang mahal. Prinsip ini harus mendorong tim untuk membuat *branch* untuk setiap unit pekerjaan, tidak peduli seberapa kecil, untuk memastikan isolasi dan stabilitas.  
* **Hormati Riwayat Bersama:** Ada perbedaan mendasar antara operasi yang mempertahankan riwayat (seperti git merge dan git revert) dan yang menulis ulangnya (seperti git reset dan git rebase). Menulis ulang riwayat yang telah dibagikan adalah resep untuk kekacauan kolaboratif.

### **7.2 Rekomendasi Praktik Terbaik**

Untuk memanfaatkan kekuatan Git sepenuhnya dan menghindari jebakan umum, tim dan pengembang individu harus berusaha untuk mengadopsi praktik-praktik terbaik berikut:

1. **Commit Sering dan Kecil:** Buat *commit* atomik yang mewakili satu unit kerja logis. Tulis pesan *commit* yang jelas dan deskriptif yang menjelaskan "mengapa" perubahan itu dibuat, bukan hanya "apa" yang diubah.  
2. **Gunakan *Branch* untuk Segalanya:** Jangan pernah melakukan *commit* langsung ke *branch* main atau develop. Setiap fitur, perbaikan, atau eksperimen harus memiliki *branch*\-nya sendiri. Ini menjaga *branch* utama tetap bersih dan stabil.  
3. **Sinkronisasi Secara Teratur:** Sering-seringlah melakukan git pull (atau fetch dan rebase) dari *branch* utama untuk menjaga *branch* fitur Anda tetap mutakhir. Ini akan membuat konflik penggabungan lebih kecil dan lebih mudah dikelola ketika saatnya untuk mengintegrasikan pekerjaan Anda.  
4. **Pahami Alat Anda:** Ketahui perbedaan kritis antara reset dan revert, serta merge dan rebase. Gunakan alat yang tepat untuk pekerjaan yang tepat, dan selalu berhati-hati saat melakukan operasi yang menulis ulang sejarah.  
5. **Lakukan Tinjauan Kode Melalui *Pull/Merge Requests*:** Manfaatkan fitur kolaboratif platform hosting Anda. Setiap *branch* fitur harus melalui *Pull/Merge Request* untuk tinjauan kode sebelum digabungkan. Ini adalah salah satu praktik terbaik terpenting untuk memastikan kualitas kode dan berbagi pengetahuan tim.  
6. **Pilih Alur Kerja yang Sesuai dengan Konteks:** Tidak ada satu alur kerja yang cocok untuk semua. Evaluasi ukuran tim Anda, sifat proyek, dan siklus rilis Anda untuk memilih model yang paling sesuai—apakah itu Gitflow yang terstruktur, Trunk-Based Development yang cepat, atau sesuatu di antaranya. Jangan takut untuk mengadaptasi alur kerja seiring dengan berkembangnya tim dan proyek Anda.

Dengan menginternalisasi konsep-konsep ini dan menerapkan praktik-praktik terbaik ini, tim dapat mengubah Git dari sekadar alat kontrol versi menjadi pendorong produktivitas, kualitas, dan kolaborasi yang efektif dalam siklus hidup pengembangan perangkat lunak.

#### **Works cited**

> 1. Mengenal GIT, Definisi, Fungsi, hingga Manfaatnya Bagi Programmer \- Biznet Gio, accessed September 14, 2025, [https\://www\.biznetgio.com/news/apa-itu-git](https://www.biznetgio.com/news/apa-itu-git)  
> 2. Git : Sistem Kontrol Versi Revolusioner Tahun 2024 \- Web Wirausaha, accessed September 14, 2025, [https\://webwirausaha.com/git/](https://webwirausaha.com/git/)  
> 3. What is Git version control? \- GitLab, accessed September 14, 2025, [https\://about.gitlab.com/topics/version-control/what-is-git-version-control/](https://about.gitlab.com/topics/version-control/what-is-git-version-control/)  
> 4. Pengenalan Git: Konsep Dasar dan Manfaat \- Bagian 1 \- Rumah Coding, accessed September 14, 2025, [https\://rumahcoding.id/blog/pengenalan-git-konsep-dasar-dan-manfaat-bagian-1/](https://rumahcoding.id/blog/pengenalan-git-konsep-dasar-dan-manfaat-bagian-1/)  
> 5. Version Control System: Fungsi, 3 Jenisnya, dan Cara Kerja \- Sekawan Media, accessed September 14, 2025, [https\://www\.sekawanmedia.co.id/blog/version-control-system/](https://www.sekawanmedia.co.id/blog/version-control-system/)  
> 6. A Short History of Git \- Git, accessed September 14, 2025, [https\://git-scm.com/book/ms/v2/Getting-Started-A-Short-History-of-Git](https://git-scm.com/book/ms/v2/Getting-Started-A-Short-History-of-Git)  
> 7. Git \- Wikipedia, accessed September 14, 2025, [https\://en.wikipedia.org/wiki/Git](https://en.wikipedia.org/wiki/Git)  
> 8. Git vs. SVN: What's the Difference, and Which is Better for Your ..., accessed September 14, 2025, [https\://www\.perforce.com/blog/vcs/git-vs-svn-what-difference](https://www.perforce.com/blog/vcs/git-vs-svn-what-difference)  
> 9. Git vs SVN: A Comparative Analysis \| by Shijo \- Medium, accessed September 14, 2025, [https\://medium.com/@shijotck/git-vs-svn-a-comparative-analysis-1b8726c5cea1](https://medium.com/@shijotck/git-vs-svn-a-comparative-analysis-1b8726c5cea1)  
> 10. Git vs. SVN: Which version control system is right for you? \- Nulab, accessed September 14, 2025, [https\://nulab.com/learn/software-development/git-vs-svn-version-control-system/](https://nulab.com/learn/software-development/git-vs-svn-version-control-system/)  
> 11. Comparison between Centralized and Distributed Version Control Systems \- Stack Overflow, accessed September 14, 2025, [https\://stackoverflow.com/questions/111031/comparison-between-centralized-and-distributed-version-control-systems](https://stackoverflow.com/questions/111031/comparison-between-centralized-and-distributed-version-control-systems)  
> 12. Branches in a Nutshell \- Git, accessed September 14, 2025, [https\://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)  
> 13. How Git Works \- KodeKloud, accessed September 14, 2025, [https\://kodekloud.com/blog/how-git-works/](https://kodekloud.com/blog/how-git-works/)  
> 14. 1.3 Getting Started \- What is Git?, accessed September 14, 2025, [https\://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)  
> 15. Recording Changes to the Repository \- Git, accessed September 14, 2025, [https\://git-scm.com/book/ms/v2/Git-Basics-Recording-Changes-to-the-Repository](https://git-scm.com/book/ms/v2/Git-Basics-Recording-Changes-to-the-Repository)  
> 16. States of a File in Git Working Directory \- GeeksforGeeks, accessed September 14, 2025, [https\://www\.geeksforgeeks.org/git/states-of-a-file-in-git-working-directory/](https://www.geeksforgeeks.org/git/states-of-a-file-in-git-working-directory/)  
> 17. Understanding Git — Index \- Zvonimir Spajic \- Medium, accessed September 14, 2025, [https\://konrad126.medium.com/understanding-git-index-4821a0765cf](https://konrad126.medium.com/understanding-git-index-4821a0765cf)  
> 18. Git Internals Part 3: Understanding the staging area in Git \- Developer Nation, accessed September 14, 2025, [https\://www\.developernation.net/blog/git-internals-part-3-understanding-the-staging-area-in-git/](https://www.developernation.net/blog/git-internals-part-3-understanding-the-staging-area-in-git/)  
> 19. Git Merge \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/using-branches/git-merge](https://www.atlassian.com/git/tutorials/using-branches/git-merge)  
> 20. Git Branching and Merging: A Step-By-Step Guide \- Varonis, accessed September 14, 2025, [https\://www\.varonis.com/blog/git-branching](https://www.varonis.com/blog/git-branching)  
> 21. About Git \- GitHub Docs, accessed September 14, 2025, [https\://docs.github.com/en/get-started/using-git/about-git](https://docs.github.com/en/get-started/using-git/about-git)  
> 22. How to use the git init command \| TheServerSide, accessed September 14, 2025, [https\://www\.theserverside.com/video/How-to-create-a-local-repository-with-the-git-init-command](https://www.theserverside.com/video/How-to-create-a-local-repository-with-the-git-init-command)  
> 23. 2.1 Git Basics \- Getting a Git Repository, accessed September 14, 2025, [https\://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)  
> 24. Top 12 Git commands every developer must know \- The GitHub Blog, accessed September 14, 2025, [https\://github.blog/developer-skills/github/top-12-git-commands-every-developer-must-know/](https://github.blog/developer-skills/github/top-12-git-commands-every-developer-must-know/)  
> 25. GIT CHEAT SHEET \- GitHub Education, accessed September 14, 2025, [https\://education.github.com/git-cheat-sheet-education.pdf](https://education.github.com/git-cheat-sheet-education.pdf)  
> 26. Common Git commands \- GitLab Docs, accessed September 14, 2025, [https\://docs.gitlab.com/topics/git/commands/](https://docs.gitlab.com/topics/git/commands/)  
> 27. Git Commit \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/saving-changes/git-commit](https://www.atlassian.com/git/tutorials/saving-changes/git-commit)  
> 28. Learn Git with Bitbucket Cloud \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud)  
> 29. About \- Branching and Merging \- Git, accessed September 14, 2025, [https\://git-scm.com/about/branching-and-merging](https://git-scm.com/about/branching-and-merging)  
> 30. How to Create a Branch in Git? \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/using-branches](https://www.atlassian.com/git/tutorials/using-branches)  
> 31. git-switch Documentation \- Git, accessed September 14, 2025, [https\://git-scm.com/docs/git-switch](https://git-scm.com/docs/git-switch)  
> 32. Git Checkout \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/using-branches/git-checkout](https://www.atlassian.com/git/tutorials/using-branches/git-checkout)  
> 33. git-merge Documentation \- Git, accessed September 14, 2025, [https\://git-scm.com/docs/git-merge](https://git-scm.com/docs/git-merge)  
> 34. Basic Branching and Merging \- Git, accessed September 14, 2025, [https\://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)  
> 35. Resolving a merge conflict using the command line \- GitHub Docs, accessed September 14, 2025, [https\://docs.github.com/articles/resolving-a-merge-conflict-using-the-command-line](https://docs.github.com/articles/resolving-a-merge-conflict-using-the-command-line)  
> 36. Resolve Git merge conflicts \- Azure Repos \- Microsoft Learn, accessed September 14, 2025, [https\://learn.microsoft.com/en-us/azure/devops/repos/git/merging?view=azure-devops](https://learn.microsoft.com/en-us/azure/devops/repos/git/merging?view=azure-devops)  
> 37. How do I finish the merge after resolving my merge conflicts? \- Stack Overflow, accessed September 14, 2025, [https\://stackoverflow.com/questions/2474097/how-do-i-finish-the-merge-after-resolving-my-merge-conflicts](https://stackoverflow.com/questions/2474097/how-do-i-finish-the-merge-after-resolving-my-merge-conflicts)  
> 38. git-stash Documentation \- Git, accessed September 14, 2025, [https\://git-scm.com/docs/git-stash](https://git-scm.com/docs/git-stash)  
> 39. 7.3 Git Tools \- Stashing and Cleaning, accessed September 14, 2025, [https\://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)  
> 40. git stash \- Saving Changes \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/saving-changes/git-stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)  
> 41. What is the intended use-case for git stash? \- Stack Overflow, accessed September 14, 2025, [https\://stackoverflow.com/questions/20537223/what-is-the-intended-use-case-for-git-stash](https://stackoverflow.com/questions/20537223/what-is-the-intended-use-case-for-git-stash)  
> 42. Git \| Working with Stash \- GeeksforGeeks, accessed September 14, 2025, [https\://www\.geeksforgeeks.org/git/git-working-with-stash/](https://www.geeksforgeeks.org/git/git-working-with-stash/)  
> 43. Resetting, Checking Out & Reverting \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/resetting-checking-out-and-reverting](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting)  
> 44. Git Reset and Revert Tutorial for Beginners \- DataCamp, accessed September 14, 2025, [https\://www\.datacamp.com/tutorial/git-reset-revert-tutorial](https://www.datacamp.com/tutorial/git-reset-revert-tutorial)  
> 45. What's the difference between Git Revert, Checkout and Reset? \- Stack Overflow, accessed September 14, 2025, [https\://stackoverflow.com/questions/8358035/whats-the-difference-between-git-revert-checkout-and-reset](https://stackoverflow.com/questions/8358035/whats-the-difference-between-git-revert-checkout-and-reset)  
> 46. git reset vs git revert : r/programming \- Reddit, accessed September 14, 2025, [https\://www\.reddit.com/r/programming/comments/1bb7bss/git\_reset\_vs\_git\_revert/](https://www.reddit.com/r/programming/comments/1bb7bss/git_reset_vs_git_revert/)  
> 47. 9\. Git Revert and Git Reset made easy\! \- YouTube, accessed September 14, 2025, [https\://www\.youtube.com/watch?v=eXsJ837CQKQ](https://www.youtube.com/watch?v=eXsJ837CQKQ)  
> 48. Git Reset \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/undoing-changes/git-reset](https://www.atlassian.com/git/tutorials/undoing-changes/git-reset)  
> 49. Reset Demystified \- Git, accessed September 14, 2025, [https\://git-scm.com/book/ms/v2/Git-Tools-Reset-Demystified](https://git-scm.com/book/ms/v2/Git-Tools-Reset-Demystified)  
> 50. Is my understanding of Git Reset Correct? : r/git \- Reddit, accessed September 14, 2025, [https\://www\.reddit.com/r/git/comments/pbx637/is\_my\_understanding\_of\_git\_reset\_correct/](https://www.reddit.com/r/git/comments/pbx637/is_my_understanding_of_git_reset_correct/)  
> 51. What's the difference between git reset \--mixed, \--soft, and \--hard? \- Stack Overflow, accessed September 14, 2025, [https\://stackoverflow.com/questions/3528245/whats-the-difference-between-git-reset-mixed-soft-and-hard](https://stackoverflow.com/questions/3528245/whats-the-difference-between-git-reset-mixed-soft-and-hard)  
> 52. Git Reset \| Hard, Soft & Mixed \| Learn Git \- GitKraken, accessed September 14, 2025, [https\://www\.gitkraken.com/learn/git/git-reset](https://www.gitkraken.com/learn/git/git-reset)  
> 53. Merging vs. Rebasing \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/merging-vs-rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)  
> 54. What's the difference between 'git merge' and 'git rebase'? \- Stack Overflow, accessed September 14, 2025, [https\://stackoverflow.com/questions/16666089/whats-the-difference-between-git-merge-and-git-rebase](https://stackoverflow.com/questions/16666089/whats-the-difference-between-git-merge-and-git-rebase)  
> 55. Differences Between Git Merge and Rebase — and Why You Should Care, accessed September 14, 2025, [https\://blog.git-init.com/differences-between-git-merge-and-rebase-and-why-you-should-care/](https://blog.git-init.com/differences-between-git-merge-and-rebase-and-why-you-should-care/)  
> 56. The difference between merge and rebase in git. \- Reddit, accessed September 14, 2025, [https\://www\.reddit.com/r/git/comments/243ee6/the\_difference\_between\_merge\_and\_rebase\_in\_git/](https://www.reddit.com/r/git/comments/243ee6/the_difference_between_merge_and_rebase_in_git/)  
> 57. What is a Git workflow? \- GitLab, accessed September 14, 2025, [https\://about.gitlab.com/topics/version-control/what-is-git-workflow/](https://about.gitlab.com/topics/version-control/what-is-git-workflow/)  
> 58. Git Workflow \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/comparing-workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)  
> 59. Git Feature Branch Workflow \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)  
> 60. Git Workflow Explained: 5 Real-World Examples \+ Optimization Tips \- Axify, accessed September 14, 2025, [https\://axify.io/blog/git-workflow](https://axify.io/blog/git-workflow)  
> 61. Gitflow Workflow \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)  
> 62. Trunk-based Development \| Atlassian, accessed September 14, 2025, [https\://www\.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development)  
> 63. Smart branching with SourceTree and Git-flow \- Work Life by Atlassian, accessed September 14, 2025, [https\://www\.atlassian.com/blog/sourcetree/smart-branching-with-sourcetree-and-git-flow](https://www.atlassian.com/blog/sourcetree/smart-branching-with-sourcetree-and-git-flow)  
> 64. What is Git Flow \| How to use Git Flow \| Learn Git \- GitKraken, accessed September 14, 2025, [https\://www\.gitkraken.com/learn/git/git-flow](https://www.gitkraken.com/learn/git/git-flow)  
> 65. What Is a Git Fork? \| Atlassian Git Tutorial, accessed September 14, 2025, [https\://www\.atlassian.com/git/tutorials/comparing-workflows/forking-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)  
> 66. GitHub How to Fork \| Learn how to fork a GitHub repository \- GitKraken, accessed September 14, 2025, [https\://www\.gitkraken.com/learn/git/problems/github-how-to-fork](https://www.gitkraken.com/learn/git/problems/github-how-to-fork)  
> 67. Creating a pull request from a fork \- GitHub Docs, accessed September 14, 2025, [https\://docs.github.com/articles/creating-a-pull-request-from-a-fork](https://docs.github.com/articles/creating-a-pull-request-from-a-fork)  
> 68. Pull request vs merge request: Understanding the difference \- Graphite, accessed September 14, 2025, [https\://graphite.dev/guides/pull-request-vs-merge-request](https://graphite.dev/guides/pull-request-vs-merge-request)  
> 69. Pull Request vs. Merge Request: Definition, Differences, and More \- Simplilearn.com, accessed September 14, 2025, [https\://www\.simplilearn.com/pull-vs-merge-request-definition-differences-benefits-article](https://www.simplilearn.com/pull-vs-merge-request-definition-differences-benefits-article)  
> 70. git \- Pull request vs Merge request \- Stack Overflow, accessed September 14, 2025, [https\://stackoverflow.com/questions/22199432/pull-request-vs-merge-request](https://stackoverflow.com/questions/22199432/pull-request-vs-merge-request)  
> 71. About pull requests \- GitHub Docs, accessed September 14, 2025, [https\://docs.github.com/articles/about-pull-requests](https://docs.github.com/articles/about-pull-requests)  
> 72. Merge requests \| GitLab Docs, accessed September 14, 2025, [https\://docs.gitlab.com/user/project/merge\_requests/](https://docs.gitlab.com/user/project/merge_requests/)