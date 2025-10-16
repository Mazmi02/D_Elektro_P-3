<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Data Lake?", "id": "Penyimpanan Data Mentah Skala Besar." },
  { "en": "Apa Beda Data Warehouse Dan Data Lake?", "id": "Warehouse Terstruktur, Lake Tidak Terstruktur." },
  { "en": "Apa Itu Database NoSQL?", "id": "Database Non-relasional Fleksibel." },
  { "en": "Sebutkan Jenis Database NoSQL?", "id": "Dokumen, Key-Value, Kolom, Grafik." },
  { "en": "Apa Itu MongoDB?", "id": "Contoh Database NoSQL Berbasis Dokumen." },
  { "en": "Apa Itu Redis?", "id": "Penyimpanan Key-Value Dalam Memori." },
  { "en": "Apa Itu Cassandra?", "id": "Database NoSQL Wide-Column." },
  { "en": "Apa Itu Neo4j?", "id": "Contoh Database Grafik." },
  { "en": "Apa Itu Teorema CAP?", "id": "Konsistensi, Ketersediaan, Toleransi Partisi." },
  { "en": "Apa Arti Teorema CAP?", "id": "Sistem Terdistribusi Hanya Dapat Memilih Dua." },
  { "en": "Apa Itu Konsistensi Akhirnya (Eventual Consistency)?", "id": "Data Akan Konsisten Seiring Waktu." },
  { "en": "Apa Itu Sharding Database?", "id": "Membagi Database Secara Horizontal." },
  { "en": "Apa Itu Replikasi Database?", "id": "Menyalin Data Ke Beberapa Server." },
  { "en": "Apa Beda Replikasi Master-Slave Dan Master-Master?", "id": "Arah Penulisan Data Antar Server." },
  { "en": "Apa Itu Indeks Database?", "id": "Struktur Data Mempercepat Pengambilan Data." },
  { "en": "Apa Itu Rencana Eksekusi Kueri?", "id": "Langkah-langkah Database Untuk Menjalankan Kueri." },
  { "en": "Apa Itu Transaksi Database?", "id": "Satu Unit Kerja Yang Atomik." },
  { "en": "Apa Itu Properti ACID?", "id": "Atomicity, Consistency, Isolation, Durability." },
  { "en": "Apa Itu Atomicity?", "id": "Transaksi Berhasil Sepenuhnya Atau Gagal Total." },
  { "en": "Apa Itu Consistency?", "id": "Transaksi Membawa Database Ke Keadaan Valid." },
  { "en": "Apa Itu Isolation?", "id": "Transaksi Bersamaan Tidak Saling Mengganggu." },
  { "en": "Apa Itu Durability?", "id": "Perubahan Transaksi Berhasil Akan Permanen." },
  { "en": "Apa Itu Tingkat Isolasi (Isolation Level)?", "id": "Mengontrol Tingkat Isolasi Antar Transaksi." },
  { "en": "Apa Itu Deadlock Database?", "id": "Dua Transaksi Saling Menunggu Sumber Daya." },
  { "en": "Apa Itu Object-Relational Mapping (ORM)?", "id": "Memetakan Objek Kode Ke Tabel Database." },
  { "en": "Apa Itu Arsitektur Perangkat Lunak?", "id": "Struktur Tingkat Tinggi Sistem Perangkat Lunak." },
  { "en": "Apa Itu Arsitektur Monolitik?", "id": "Aplikasi Dibangun Sebagai Satu Unit Tunggal." },
  { "en": "Apa Itu Arsitektur Layanan Mikro (Microservices)?", "id": "Aplikasi Dibangun Dari Layanan Kecil Independen." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Pemrograman Aplikasi." },
  { "en": "Apa Itu REST (Representational State Transfer)?", "id": "Gaya Arsitektur Untuk API Web." },
  { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Pertukaran Data Ringan." },
  { "en": "Apa Itu XML (Extensible Markup Language)?", "id": "Bahasa Markup Untuk Pertukaran Data." },
  { "en": "Apa Itu SOAP (Simple Object Access Protocol)?", "id": "Protokol API Berbasis XML." },
  { "en": "Apa Itu GraphQL?", "id": "Bahasa Kueri Untuk API." },
  { "en": "Apa Itu Pola Desain (Design Pattern)?", "id": "Solusi Umum Masalah Desain Perangkat Lunak." },
  { "en": "Sebutkan Contoh Pola Desain Creational?", "id": "Singleton, Factory, Builder." },
  { "en": "Sebutkan Contoh Pola Desain Structural?", "id": "Adapter, Decorator, Facade." },
  { "en": "Sebutkan Contoh Pola Desain Behavioral?", "id": "Observer, Strategy, Command." },
  { "en": "Apa Itu Prinsip SOLID?", "id": "Lima Prinsip Desain Berorientasi Objek." },
  { "en": "Apa Itu Single Responsibility Principle (SRP)?", "id": "Setiap Kelas Punya Satu Tanggung Jawab." },
  { "en": "Apa Itu Open/Closed Principle (OCP)?", "id": "Terbuka Untuk Ekstensi, Tertutup Modifikasi." },
  { "en": "Apa Itu Liskov Substitution Principle (LSP)?", "id": "Objek Turunan Harus Dapat Menggantikan Induk." },
  { "en": "Apa Itu Interface Segregation Principle (ISP)?", "id": "Klien Tidak Boleh Tergantung Antarmuka." },
  { "en": "Apa Itu Dependency Inversion Principle (DIP)?", "id": "Bergantung Pada Abstraksi, Bukan Implementasi." },
  { "en": "Apa Itu Kode Terkompilasi?", "id": "Diterjemahkan Menjadi Kode Mesin Sebelum Dijalankan." },
  { "en": "Apa Itu Kode Terinterpretasi?", "id": "Diterjemahkan Baris Demi Baris Saat Dijalankan." },
  { "en": "Sebutkan Contoh Bahasa Terkompilasi?", "id": "C++, Java (Ke Bytecode), Go." },
  { "en": "Sebutkan Contoh Bahasa Terinterpretasi?", "id": "Python, JavaScript, Ruby." },
  { "en": "Apa Itu Just-In-Time (JIT) Compilation?", "id": "Mengkompilasi Kode Saat Dijalankan." },
  { "en": "Apa Itu Garbage Collection?", "id": "Manajemen Memori Otomatis." },
  { "en": "Apa Itu Stack (Memori)?", "id": "Area Memori Untuk Variabel Lokal." },
  { "en": "Apa Itu Heap (Memori)?", "id": "Area Memori Untuk Alokasi Dinamis." },
  { "en": "Apa Itu Stack Overflow Error?", "id": "Stack Kehabisan Ruang." },
  { "en": "Apa Itu утечка памяти (Memory Leak)?", "id": "Memori Dialokasikan Tidak Pernah Dibebaskan." },
  { "en": "Apa Itu Concurrency?", "id": "Menjalankan Beberapa Tugas Secara Tumpang Tindih." },
  { "en": "Apa Itu Parallelism?", "id": "Menjalankan Beberapa Tugas Secara Bersamaan." },
  { "en": "Apa Itu Thread?", "id": "Unit Eksekusi Terkecil." },
  { "en": "Apa Itu Proses?", "id": "Program Yang Sedang Dijalankan." },
  { "en": "Apa Itu Multithreading?", "id": "Menjalankan Beberapa Thread Dalam Satu Proses." },
  { "en": "Apa Itu Race Condition?", "id": "Hasil Bergantung Urutan Eksekusi Thread." },
  { "en": "Apa Itu Deadlock?", "id": "Dua Thread Saling Menunggu." },
  { "en": "Apa Itu Mutex?", "id": "Mekanisme Kunci Untuk Mencegah Race Condition." },
  { "en": "Apa Itu Semaphore?", "id": "Mengontrol Akses Ke Kumpulan Sumber Daya." },
  { "en": "Apa Itu Pemrograman Asinkron?", "id": "Eksekusi Non-blocking, Tidak Menunggu Tugas." },
  { "en": "Apa Itu Callback Function?", "id": "Fungsi Dijalankan Setelah Tugas Selesai." },
  { "en": "Apa Itu Promise Dalam JavaScript?", "id": "Objek Mewakili Penyelesaian Operasi Asinkron." },
  { "en": "Apa Itu Async/Await?", "id": "Sintaksis Untuk Pemrograman Asinkron." },
  { "en": "Apa Itu Version Control System (VCS)?", "id": "Sistem Mengelola Perubahan Kode." },
  { "en": "Apa Itu Git?", "id": "Sistem Kontrol Versi Terdistribusi Populer." },
  { "en": "Apa Itu Repository?", "id": "Penyimpanan Proyek Git." },
  { "en": "Apa Itu Commit?", "id": "Menyimpan Perubahan Ke Repository." },
  { "en": "Apa Itu Branch?", "id": "Garis Pengembangan Independen." },
  { "en": "Apa Itu Merge?", "id": "Menggabungkan Perubahan Dari Branch Berbeda." },
  { "en": "Apa Itu Merge Conflict?", "id": "Terjadi Saat Perubahan Bertentangan." },
  { "en": "Apa Itu Pull Request (Merge Request)?", "id": "Proposal Untuk Menggabungkan Perubahan." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Mengintegrasikan Perubahan Kode Secara Otomatis." },
  { "en": "Apa Itu Continuous Delivery/Deployment (CD)?", "id": "Merilis Perangkat Lunak Secara Otomatis." },
  { "en": "Apa Itu DevOps?", "id": "Kultur Menggabungkan Pengembangan, Operasi." },
  { "en": "Apa Itu Infrastructure as Code (IaC)?", "id": "Mengelola Infrastruktur Menggunakan Kode." },
  { "en": "Apa Itu Docker?", "id": "Platform Untuk Membangun, Menjalankan Aplikasi Kontainer." },
  { "en": "Apa Itu Kontainer (Container)?", "id": "Unit Perangkat Lunak Ringan, Standar." },
  { "en": "Apa Beda Kontainer Dan Mesin Virtual?", "id": "Kontainer Berbagi Kernel OS." },
  { "en": "Apa Itu Kubernetes?", "id": "Platform Orkestrasi Kontainer." },
  { "en": "Apa Itu Orkestrasi?", "id": "Manajemen Otomatis Sistem Kompleks." },
  { "en": "Apa Itu Skalabilitas (Scalability)?", "id": "Kemampuan Sistem Menangani Peningkatan Beban." },
  { "en": "Apa Itu Skala Vertikal (Scaling Up)?", "id": "Menambah Sumber Daya Ke Server Tunggal." },
  { "en": "Apa Itu Skala Horizontal (Scaling Out)?", "id": "Menambah Lebih Banyak Server." },
  { "en": "Apa Itu Load Balancer?", "id": "Mendistribusikan Lalu Lintas Ke Beberapa Server." },
  { "en": "Apa Itu Caching?", "id": "Menyimpan Data Sementara Untuk Akses Cepat." },
  { "en": "Apa Itu Content Delivery Network (CDN)?", "id": "Mendistribusikan Konten Dekat Pengguna." },
  { "en": "Apa Itu Algoritma?", "id": "Serangkaian Langkah Untuk Menyelesaikan Masalah." },
  { "en": "Apa Itu Struktur Data?", "id": "Cara Mengorganisir Data Di Komputer." },
  { "en": "Apa Itu Array?", "id": "Kumpulan Elemen Tipe Sama." },
  { "en": "Apa Itu Linked List?", "id": "Struktur Data Elemen Terhubung Pointer." },
  { "en": "Apa Itu Stack?", "id": "Struktur Data LIFO (Last-In, First-Out)." },
  { "en": "Apa Itu Queue?", "id": "Struktur Data FIFO (First-In, First-Out)." },
  { "en": "Apa Itu Pohon Biner (Binary Tree)?", "id": "Setiap Simpul Punya Maksimal Dua Anak." },
  { "en": "Apa Itu Pohon Pencarian Biner (BST)?", "id": "Pohon Biner Terurut." },
  { "en": "Apa Itu Hash Table?", "id": "Struktur Data Pemetaan Kunci Ke Nilai." },
  { "en": "Apa Itu Kompleksitas Waktu?", "id": "Ukuran Waktu Eksekusi Algoritma." },
  { "en": "Apa Itu Kompleksitas Ruang?", "id": "Ukuran Memori Digunakan Algoritma." },
  { "en": "Apa Itu Notasi Big O?", "id": "Mendeskripsikan Perilaku Batas Atas Algoritma." },
  { "en": "Apa Itu Notasi Big Omega (Ω)?", "id": "Mendeskripsikan Batas Bawah Kinerja Algoritma." },
  { "en": "Apa Itu Notasi Big Theta (Θ)?", "id": "Mendeskripsikan Batas Ketat Kinerja Algoritma." },
  { "en": "Apa Itu Paradigma Algoritma Divide and Conquer?", "id": "Memecah Masalah Menjadi Sub-masalah Kecil." },
  { "en": "Apa Itu Algoritma Serakah (Greedy)?", "id": "Membuat Pilihan Optimal Lokal Setiap Langkah." },
  { "en": "Apa Itu Pemrograman Dinamis?", "id": "Memecahkan Masalah Kompleks Secara Rekursif." },
  { "en": "Apa Prinsip Dasar Algoritma Bubble Sort?", "id": "Membandingkan Dan Menukar Pasangan Elemen Berdekatan." },
  { "en": "Apa Prinsip Dasar Algoritma Selection Sort?", "id": "Memilih Elemen Terkecil, Menempatkannya Di Depan." },
  { "en": "Apa Prinsip Dasar Algoritma Insertion Sort?", "id": "Membangun Larik Terurut Satu Elemen Sekaligus." },
  { "en": "Bagaimana Cara Kerja Algoritma Merge Sort?", "id": "Membagi, Mengurutkan, Menggabungkan Kembali Sub-larik." },
  { "en": "Bagaimana Cara Kerja Algoritma Quick Sort?", "id": "Memilih Pivot, Mempartisi Larik Sekitarnya." },
  { "en": "Apa Struktur Data Digunakan Heap Sort?", "id": "Struktur Data Heap Biner." },
  { "en": "Apa Kompleksitas Waktu Rata-rata Pencarian Linear?", "id": "O(n)." },
  { "en": "Apa Syarat Utama Pencarian Biner?", "id": "Larik (Array) Harus Sudah Terurut." },
  { "en": "Apa Itu Rekursi (Recursion)?", "id": "Fungsi Yang Memanggil Dirinya Sendiri." },
  { "en": "Apa Itu Kasus Dasar (Base Case) Rekursi?", "id": "Kondisi Untuk Menghentikan Proses Rekursi." },
  { "en": "Apa Itu Mesin Turing?", "id": "Model Matematis Abstrak Komputasi." },
  { "en": "Apa Itu Halting Problem?", "id": "Masalah Penentuan Apakah Program Akan Berhenti." },
  { "en": "Apa Itu Komputabilitas?", "id": "Mempelajari Masalah Mana Yang Dapat Dipecahkan." },
  { "en": "Apa Itu Kalkulus Lambda?", "id": "Sistem Formal Berbasis Fungsi." },
  { "en": "Apa Prinsip Utama Pemrograman Fungsional?", "id": "Menggunakan Fungsi Murni Tanpa Efek Samping." },
  { "en": "Apa Itu Pemrograman Berorientasi Objek (OOP)?", "id": "Paradigma Pemrograman Berbasis Objek." },
  { "en": "Apa Itu Kelas Dalam OOP (Object-Oriented Programming)?", "id": "Cetakan Biru Untuk Membuat Objek." },
  { "en": "Apa Itu Objek Dalam OOP (Object-Oriented Programming)?", "id": "Instansiasi Dari Sebuah Kelas." },
  { "en": "Apa Itu Pewarisan (Inheritance) Dalam OOP?", "id": "Kelas Mendapatkan Properti Dari Kelas Lain." },
  { "en": "Apa Itu Polimorfisme (Polymorphism) Dalam OOP?", "id": "Objek Dapat Mengambil Banyak Bentuk." },
  { "en": "Apa Itu Enkapsulasi (Encapsulation) Dalam OOP?", "id": "Membungkus Data Dan Metode Bersama." },
  { "en": "Apa Itu Abstraksi (Abstraction) Dalam OOP?", "id": "Menyembunyikan Detail Implementasi Yang Kompleks." },
  { "en": "Apa Itu Kompiler (Compiler)?", "id": "Menerjemahkan Seluruh Kode Sumber Sekaligus." },
  { "en": "Apa Itu Interpreter?", "id": "Menerjemahkan Kode Sumber Baris Demi Baris." },
  { "en": "Apa Fungsi Linker?", "id": "Menggabungkan Berkas Objek Menjadi Berkas Eksekusi." },
  { "en": "Apa Fungsi Loader?", "id": "Memuat Program Ke Memori Utama." },
  { "en": "Apa Itu Sistem Operasi (OS)?", "id": "Mengelola Perangkat Keras Dan Lunak Komputer." },
  { "en": "Apa Itu Kernel?", "id": "Komponen Inti Dari Sistem Operasi." },
  { "en": "Apa Itu Penjadwalan Proses?", "id": "Memutuskan Proses Mana Yang Akan Dijalankan." },
  { "en": "Apa Itu Manajemen Memori?", "id": "Mengalokasikan Dan Mendealokasikan Ruang Memori." },
  { "en": "Apa Itu Paging Dalam Manajemen Memori?", "id": "Membagi Memori Menjadi Blok Ukuran Tetap." },
  { "en": "Apa Itu Segmentasi Dalam Manajemen Memori?", "id": "Membagi Memori Menjadi Segmen Ukuran Variabel." },
  { "en": "Apa Itu Sistem Berkas (File System)?", "id": "Mengatur Berkas Di Perangkat Penyimpanan." },
  { "en": "Apa Itu DBMS (Database Management System)?", "id": "Perangkat Lunak Pengelola Database." },
  { "en": "Apa Kepanjangan SQL (Structured Query Language)?", "id": "Bahasa Kueri Terstruktur." },
  { "en": "Apa Itu Kunci Primer (Primary Key)?", "id": "Kolom Unik Pengidentifikasi Setiap Baris." },
  { "en": "Apa Itu Kunci Asing (Foreign Key)?", "id": "Kunci Penghubung Dua Tabel Berbeda." },
  { "en": "Apa Tujuan Normalisasi Database?", "id": "Mengurangi Redundansi Dan Inkonsistensi Data." },
  { "en": "Apa Tujuan Denormalisasi Database?", "id": "Meningkatkan Kinerja Baca Dengan Menambah Redundansi." },
  { "en": "Apa Itu Grafika Raster?", "id": "Gambar Yang Dibuat Dari Jaringan Piksel." },
  { "en": "Apa Itu Grafika Vektor?", "id": "Gambar Dibuat Dari Objek Geometris." },
  { "en": "Apa Itu Proses Rendering?", "id": "Proses Menghasilkan Gambar Dari Model." },
  { "en": "Apa Itu Ray Tracing?", "id": "Teknik Rendering Menelusuri Jalur Cahaya." },
  { "en": "Apa Itu Shading?", "id": "Proses Menentukan Warna Objek Tiga Dimensi." },
  { "en": "Apa Itu Pemetaan Tekstur (Texture Mapping)?", "id": "Menerapkan Gambar Ke Permukaan Objek." },
  { "en": "Apa Kepanjangan GUI (Graphical User Interface)?", "id": "Antarmuka Pengguna Grafis." },
  { "en": "Apa Kepanjangan CLI (Command-Line Interface)?", "id": "Antarmuka Baris Perintah." },
  { "en": "Apa Kepanjangan IDE (Integrated Development Environment)?", "id": "Lingkungan Pengembangan Terintegrasi." },
  { "en": "Apa Itu Debugger?", "id": "Alat Perangkat Lunak Untuk Mencari Bug." },
  { "en": "Apa Kepanjangan SDLC (Software Development Life Cycle)?", "id": "Siklus Hidup Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu Model Air Terjun (Waterfall)?", "id": "Model Pengembangan Perangkat Lunak Sekuensial." },
  { "en": "Apa Itu Metodologi Agile?", "id": "Pendekatan Iteratif Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu Scrum?", "id": "Kerangka Kerja Agile Untuk Manajemen Proyek." },
  { "en": "Apa Itu Kanban?", "id": "Metode Agile Memvisualisasikan Alur Kerja." },
  { "en": "Apa Itu Pengujian Unit (Unit Testing)?", "id": "Menguji Komponen Perangkat Lunak Secara Individual." },
  { "en": "Apa Itu Pengujian Integrasi?", "id": "Menguji Interaksi Antara Komponen Perangkat Lunak." },
  { "en": "Apa Itu Pengujian Sistem?", "id": "Menguji Sistem Terintegrasi Secara Keseluruhan." },
  { "en": "Apa Itu Pengujian Penerimaan (Acceptance Testing)?", "id": "Memverifikasi Sistem Terhadap Kebutuhan Pengguna." },
  { "en": "Apa Itu Refactoring?", "id": "Merestrukturisasi Kode Tanpa Mengubah Perilakunya." },
  { "en": "Apa Itu Utang Teknis (Technical Debt)?", "id": "Biaya Implisit Pengerjaan Ulang Di Masa Depan." },
  { "en": "Apa Kepanjangan HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Transfer Hiperteks." },
  { "en": "Apa Kepanjangan HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Protokol Transfer Hiperteks Aman." },
  { "en": "Apa Kepanjangan URL (Uniform Resource Locator)?", "id": "Penentu Lokasi Sumber Seragam." },
  { "en": "Apa Kepanjangan HTML (Hypertext Markup Language)?", "id": "Bahasa Markah Hiperteks." },
  { "en": "Apa Kepanjangan CSS (Cascading Style Sheets)?", "id": "Lembar Gaya Bertingkat." },
  { "en": "Apa Kepanjangan DOM (Document Object Model)?", "id": "Model Objek Dokumen." },
  { "en": "Apa Kepanjangan AJAX (Asynchronous JavaScript and XML)?", "id": "JavaScript Dan XML Asinkron." },
  { "en": "Apa Itu Server Web?", "id": "Menyajikan Konten Situs Web Ke Pengguna." },
  { "en": "Apa Itu Browser Web?", "id": "Aplikasi Untuk Mengakses Informasi Di Internet." },
  { "en": "Apa Itu Cookie Web?", "id": "Data Kecil Dikirim Situs Web Ke Browser." },
  { "en": "Apa Itu Sesi Web (Web Session)?", "id": "Menyimpan Informasi Pengguna Di Sisi Server." },
  { "en": "Apa Itu Otentikasi?", "id": "Proses Memverifikasi Identitas Pengguna." },
  { "en": "Apa Itu Otorisasi?", "id": "Proses Memberi Izin Akses Sumber Daya." },
  { "en": "Apa Kepanjangan JWT (JSON Web Token)?", "id": "Token Web JSON." },
  { "en": "Apa Kepanjangan IaaS (Infrastructure as a Service)?", "id": "Infrastruktur Sebagai Layanan." },
  { "en": "Apa Kepanjangan PaaS (Platform as a Service)?", "id": "Platform Sebagai Layanan." },
  { "en": "Apa Kepanjangan SaaS (Software as a Service)?", "id": "Perangkat Lunak Sebagai Layanan." },
  { "en": "Apa Itu Virtualisasi?", "id": "Membuat Versi Virtual Sumber Daya Komputasi." },
  { "en": "Apa Itu Hypervisor?", "id": "Membuat Dan Menjalankan Mesin Virtual." },
  { "en": "Apa Itu Komputasi Tanpa Server (Serverless)?", "id": "Model Eksekusi Cloud Tanpa Mengelola Server." },
  { "en": "Apa Itu API Gateway?", "id": "Mengelola Panggilan API Antara Klien, Layanan." },
  { "en": "Apa Itu Service Mesh?", "id": "Lapisan Infrastruktur Mengelola Komunikasi Antar Layanan." },
  { "en": "Apa Itu Komputasi Kuantum?", "id": "Menggunakan Fenomena Kuantum Untuk Komputasi." },
  { "en": "Apa Itu Qubit?", "id": "Unit Dasar Informasi Kuantum." },
  { "en": "Apa Itu Superposisi?", "id": "Qubit Berada Dalam Beberapa Keadaan Sekaligus." },
  { "en": "Apa Itu Keterikatan (Entanglement)?", "id": "Koneksi Kuantum Antara Beberapa Qubit." },
  { "en": "Apa Itu Gerbang Kuantum?", "id": "Operasi Dasar Pada Satu Atau Lebih Qubit." },
  { "en": "Apa Itu Blockchain?", "id": "Buku Besar Digital Terdistribusi, Tak Terubah." },
  { "en": "Apa Itu Penambangan (Mining) Cryptocurrency?", "id": "Proses Memverifikasi Transaksi, Menambah Ke Blockchain." },
  { "en": "Apa Itu Bukti Kerja (Proof of Work)?", "id": "Mekanisme Konsensus Digunakan Oleh Bitcoin." },
  { "en": "Apa Itu Bukti Kepemilikan (Proof of Stake)?", "id": "Mekanisme Konsensus Alternatif." },
  { "en": "Apa Itu Kontrak Pintar (Smart Contract)?", "id": "Program Dijalankan Otomatis Di Blockchain." },
  { "en": "Apa Itu Aplikasi Terdesentralisasi (DApp)?", "id": "Aplikasi Berjalan Di Jaringan Peer-to-Peer." },
  { "en": "Apa Itu Organisasi Otonom Terdesentralisasi (DAO)?", "id": "Organisasi Dikelola Oleh Aturan Kode." },
  { "en": "Apa Itu Non-Fungible Token (NFT)?", "id": "Aset Digital Unik Di Blockchain." },
  { "en": "Apa Itu Web3?", "id": "Visi Internet Terdesentralisasi Generasi Berikutnya." },
  { "en": "Apa Itu Pengontrol Proporsional (P)?", "id": "Output Proporsional Terhadap Sinyal Error." },
  { "en": "Apa Itu Pengontrol Integral (I)?", "id": "Menghilangkan Error Keadaan Tunak (Steady-State)." },
  { "en": "Apa Itu Pengontrol Derivatif (D)?", "id": "Mengantisipasi Perubahan Error, Meredam Osilasi." },
  { "en": "Apa Itu Sistem Minimum Phase?", "id": "Sistem Stabil Dengan Invers Stabil." },
  { "en": "Apa Itu Sistem Non-Minimum Phase?", "id": "Sistem Dengan Nol Di Setengah Kanan Bidang-S." },
  { "en": "Apa Itu Teori Grafik Aliran Sinyal?", "id": "Metode Grafis Menganalisis Sistem Linear." },
  { "en": "Apa Itu Rumus Penguatan Mason?", "id": "Menghitung Penguatan Total Grafik Aliran Sinyal." },
  { "en": "Apa Itu Deadbeat Control?", "id": "Respon Sistem Mencapai Setpoint Secepat Mungkin." },
  { "en": "Apa Itu Model Internal Control (IMC)?", "id": "Strategi Desain Kontroler Berbasis Model." },
  { "en": "Apa Itu Matriks Keterkontrolan?", "id": "Matriks Menguji Keterkontrolan Sistem Keadaan." },
  { "en": "Apa Itu Matriks Keteramatan?", "id": "Matriks Menguji Keteramatan Sistem Keadaan." },
  { "en": "Apa Itu Eigenvalue Placement?", "id": "Nama Lain Untuk Penempatan Kutub." },
  { "en": "Apa Itu Kriteria Stabilitas Lingkaran?", "id": "Metode Stabilitas Grafis Untuk Sistem Non-linear." },
  { "en": "Apa Itu Fungsi Deskripsi?", "id": "Menganalisis Osilasi Batas Sistem Non-linear." },
  { "en": "Apa Itu Pesawat Fasa (Phase Plane)?", "id": "Plot Grafis Perilaku Sistem Orde Dua." },
  { "en": "Apa Itu Titik Ekuilibrium?", "id": "Titik Di Mana Keadaan Sistem Tetap." },
  { "en": "Apa Itu Siklus Batas (Limit Cycle)?", "id": "Trajektori Periodik Tertutup Dalam Pesawat Fasa." },
  { "en": "Apa Itu Kontrol Bang-Bang?", "id": "Kontroler Beralih Antara Dua Keadaan Ekstrem." },
  { "en": "Apa Itu Kontrol Variabel Struktur?", "id": "Nama Lain Untuk Sliding Mode Control." },
  { "en": "Apa Itu Permukaan Geser (Sliding Surface)?", "id": "Permukaan Di Ruang Keadaan Dirancang." },
  { "en": "Apa Itu Fenomena Chattering?", "id": "Osilasi Frekuensi Tinggi Dalam Sliding Mode." },
  { "en": "Apa Itu Lengan Robot?", "id": "Mekanisme Rantai Kinematik Robot." },
  { "en": "Apa Itu Sendi Prismatic?", "id": "Sendi Robot Yang Bergerak Linear." },
  { "en": "Apa Itu Sendi Revolute?", "id": "Sendi Robot Yang Berotasi." },
  { "en": "Apa Itu Konvensi Denavit-Hartenberg (DH)?", "id": "Metodologi Standar Pemodelan Kinematika Robot." },
  { "en": "Apa Itu Perangkat Lunak Middleware Robot?", "id": "Contoh: ROS (Robot Operating System)." },
  { "en": "Apa Itu Odometri Dalam Robotika?", "id": "Estimasi Posisi Berdasarkan Data Gerakan Internal." },
  { "en": "Apa Itu Pemfilteran Partikel (Monte Carlo Localization)?", "id": "Algoritma Lokalisasi Robot Probabilistik." },
  { "en": "Apa Itu Pemodelan Grid Okupansi?", "id": "Representasi Peta Lingkungan Robot." },
  { "en": "Apa Itu Wavefront Planner?", "id": "Algoritma Perencanaan Jalur Berbasis Grid." },
  { "en": "Apa Itu Algoritma A* (A-Star)?", "id": "Algoritma Pencarian Jalur Heuristik." },
  { "en": "Apa Itu Bidang Potensial Buatan?", "id": "Metode Perencanaan Jalur Menghindari Hambatan." },
  { "en": "Apa Itu Robotika Kawanan (Swarm Robotics)?", "id": "Koordinasi Sejumlah Besar Robot Sederhana." },
  { "en": "Apa Itu Bahasa Assembly?", "id": "Bahasa Pemrograman Tingkat Rendah." },
  { "en": "Apa Itu Assembler?", "id": "Menerjemahkan Bahasa Assembly Ke Kode Mesin." },
  { "en": "Apa Itu Opcode (Operation Code)?", "id": "Bagian Instruksi Penentu Operasi." },
  { "en": "Apa Itu Operand?", "id": "Bagian Instruksi Penentu Data Operasi." },
  { "en": "Apa Itu Mode Pengalamatan?", "id": "Cara Instruksi Menentukan Operandnya." },
  { "en": "Apa Itu Pengalamatan Langsung (Direct Addressing)?", "id": "Operand Adalah Alamat Memori." },
  { "en": "Apa Itu Pengalamatan Segera (Immediate Addressing)?", "id": "Operand Adalah Nilai Konstan." },
  { "en": "Apa Itu Pengalamatan Register?", "id": "Operand Berada Dalam Register CPU." },
  { "en": "Apa Itu Pengalamatan Tidak Langsung?", "id": "Alamat Operand Disimpan Di Memori/Register." },
  { "en": "Apa Itu Siklus Instruksi?", "id": "Urutan Langkah Eksekusi Instruksi." },
  { "en": "Apa Saja Tahap Siklus Instruksi?", "id": "Fetch, Decode, Execute." },
  { "en": "Apa Itu Fetch Cycle?", "id": "Mengambil Instruksi Dari Memori." },
  { "en": "Apa Itu Decode Cycle?", "id": "Menerjemahkan Opcode Instruksi." },
  { "en": "Apa Itu Execute Cycle?", "id": "Menjalankan Operasi Yang Ditentukan." },
  { "en": "Apa Itu Arsitektur Set Instruksi (ISA)?", "id": "Bagian Prosesor Terlihat Oleh Pemrogram." },
  { "en": "Apa Itu Little's Law?", "id": "L = λW." },
  { "en": "Apa Itu Kode ASCII (American Standard Code for Information Interchange)?", "id": "Standar Pengkodean Karakter." },
  { "en": "Apa Itu Unicode?", "id": "Standar Pengkodean Karakter Internasional." },
  { "en": "Apa Itu UTF-8?", "id": "Pengkodean Unicode Lebar Variabel Paling Umum." },
  { "en": "Apa Itu Komputasi Paralel?", "id": "Menjalankan Banyak Komputasi Secara Bersamaan." },
  { "en": "Apa Itu Hukum Amdahl?", "id": "Formula Batas Peningkatan Kinerja Paralel." },
  { "en": "Apa Itu Taksonomi Flynn?", "id": "Klasifikasi Arsitektur Komputer Paralel." },
  { "en": "Apa Itu SISD (Single Instruction, Single Data)?", "id": "Komputer Serial Tradisional." },
  { "en": "Apa Itu SIMD (Single Instruction, Multiple Data)?", "id": "Satu Instruksi Pada Banyak Data." },
  { "en": "Apa Itu MISD (Multiple Instruction, Single Data)?", "id": "Beberapa Instruksi Pada Satu Data (Jarang)." },
  { "en": "Apa Itu MIMD (Multiple Instruction, Multiple Data)?", "id": "Beberapa Instruksi Pada Banyak Data." },
  { "en": "Apa Itu Shared Memory Architecture?", "id": "Beberapa Prosesor Berbagi Ruang Memori Sama." },
  { "en": "Apa Itu Distributed Memory Architecture?", "id": "Setiap Prosesor Memiliki Memori Pribadi." },
  { "en": "Apa Itu Message Passing Interface (MPI)?", "id": "Standar Komunikasi Antar Prosesor Memori Terdistribusi." },
  { "en": "Apa Itu OpenMP?", "id": "API Untuk Pemrograman Paralel Memori Bersama." },
  { "en": "Apa Itu GPGPU (General-Purpose computing on Graphics Processing Units)?", "id": "Menggunakan GPU Untuk Komputasi Umum." },
  { "en": "Apa Itu CUDA (Compute Unified Device Architecture)?", "id": "Platform Komputasi Paralel NVIDIA." },
  { "en": "Apa Itu OpenCL?", "id": "Standar Terbuka Komputasi Paralel Heterogen." },
  { "en": "Apa Itu Klaster Komputer (Computer Cluster)?", "id": "Sekelompok Komputer Bekerja Bersama." },
  { "en": "Apa Itu Komputasi Grid?", "id": "Komputasi Terdistribusi Skala Sangat Besar." },
  { "en": "Apa Itu Superkomputer?", "id": "Komputer Dengan Kinerja Sangat Tinggi." },
  { "en": "Apa Itu FLOPS (Floating-point Operations Per Second)?", "id": "Ukuran Kinerja Komputer." },
  { "en": "Apa Itu Petaflop?", "id": "Seribu Triliun FLOPS." },
  { "en": "Apa Itu Exaflop?", "id": "Satu Kuintiliun FLOPS." },
  { "en": "Apa Itu Benchmarking?", "id": "Proses Mengukur Kinerja Sistem Komputer." },
  { "en": "Apa Itu Benchmark SPEC?", "id": "Kumpulan Standar Benchmark Kinerja CPU." },
  { "en": "Apa Itu Benchmark LINPACK?", "id": "Benchmark Aljabar Linear, Daftar TOP500." },
  { "en": "Apa Itu Pipelining Instruksi?", "id": "Tumpang Tindih Eksekusi Beberapa Instruksi." },
  { "en": "Apa Itu Latensi Pipeline?", "id": "Waktu Total Satu Instruksi Melewati Pipeline." },
  { "en": "Apa Itu Throughput Pipeline?", "id": "Tingkat Penyelesaian Instruksi Pipeline." },
  { "en": "Apa Itu Bahaya Struktural?", "id": "Konflik Sumber Daya Perangkat Keras." },
  { "en": "Apa Itu Bahaya Data?", "id": "Instruksi Bergantung Hasil Instruksi Sebelumnya." },
  { "en": "Apa Itu Penerusan Data (Data Forwarding)?", "id": "Teknik Mengatasi Bahaya Data." },
  { "en": "Apa Itu Bahaya Kontrol?", "id": "Masalah Akibat Instruksi Percabangan." },
  { "en": "Apa Itu Penundaan Percabangan (Branch Delay Slot)?", "id": "Instruksi Setelah Percabangan Selalu Dieksekusi." },
  { "en": "Apa Itu Kompleksitas Siklomatik?", "id": "Ukuran Kuantitatif Kompleksitas Program." },
  { "en": "Apa Itu Kohesi (Cohesion) Perangkat Lunak?", "id": "Ukuran Seberapa Terkait Fungsi Modul." },
  { "en": "Apa Itu Penggandengan (Coupling) Perangkat Lunak?", "id": "Ukuran Ketergantungan Antar Modul." },
  { "en": "Kohesi Tinggi, Penggandengan Rendah Baik Atau Buruk?", "id": "Sangat Baik Untuk Desain Modular." },
  { "en": "Apa Itu YAGNI (You Ain't Gonna Need It)?", "id": "Prinsip Tidak Menambah Fungsionalitas Tak Perlu." },
  { "en": "Apa Itu DRY (Don't Repeat Yourself)?", "id": "Prinsip Menghindari Duplikasi Informasi, Kode." },
  { "en": "Apa Itu KISS (Keep It Simple, Stupid)?", "id": "Prinsip Desain Menjaga Kesederhanaan." },
  { "en": "Apa Itu Test-Driven Development (TDD)?", "id": "Menulis Uji Dulu, Lalu Kode." },
  { "en": "Apa Itu Behavior-Driven Development (BDD)?", "id": "Pengembangan Berbasis Perilaku Sistem." },
  { "en": "Apa Itu Analisis Persyaratan?", "id": "Proses Menentukan Kebutuhan Pengguna." },
  { "en": "Apa Itu Desain Perangkat Lunak?", "id": "Proses Merencanakan Solusi Perangkat Lunak." },
  { "en": "Apa Itu Pemeliharaan Perangkat Lunak?", "id": "Memodifikasi Perangkat Lunak Setelah Rilis." },
  { "en": "Apa Itu Pemeliharaan Korektif?", "id": "Memperbaiki Bug Yang Ditemukan." },
  { "en": "Apa Itu Pemeliharaan Adaptif?", "id": "Mengubah Perangkat Lunak Sesuai Lingkungan Baru." },
  { "en": "Apa Itu Pemeliharaan Perfektif?", "id": "Menambah Fitur Baru, Meningkatkan Kinerja." },
  { "en": "Apa Itu Pemeliharaan Preventif?", "id": "Meningkatkan Maintainability Mencegah Masalah." },
  { "en": "Apa Itu Rekayasa Ulang Perangkat Lunak?", "id": "Menganalisis, Mendesain Ulang Sistem." },
  { "en": "Apa Itu Rekayasa Terbalik Perangkat Lunak?", "id": "Mengekstrak Pengetahuan, Desain Dari Produk." },
  { "en": "Apa Itu Kualitas Perangkat Lunak?", "id": "Derajat Sistem Memenuhi Kebutuhan." },
  { "en": "Apa Itu Software Quality Assurance (SQA)?", "id": "Proses Menjamin Kualitas Perangkat Lunak." },
  { "en": "Apa Itu Metrik Perangkat Lunak?", "id": "Ukuran Kuantitatif Properti Perangkat Lunak." },
  { "en": "Apa Itu Kompleksitas Siklomatik McCabe?", "id": "Metrik Mengukur Kompleksitas Logika Program." },
  { "en": "Apa Itu Poin Fungsi (Function Points)?", "id": "Metrik Mengukur Fungsionalitas Diberikan Pengguna." },
  { "en": "Apa Itu Lines of Code (LOC)?", "id": "Metrik Paling Sederhana Mengukur Ukuran Program." },
  { "en": "Apa Itu Halstead Complexity Measures?", "id": "Metrik Berdasarkan Jumlah Operator, Operand." },
  { "en": "Apa Itu Indeks Maintainability?", "id": "Metrik Mengukur Kemudahan Perawatan Kode." },
  { "en": "Apa Itu Churn Kode (Code Churn)?", "id": "Ukuran Seberapa Sering Kode Diubah." },
  { "en": "Apa Itu Analisis Leksikal?", "id": "Tahap Pertama Kompilasi, Memecah Kode." },
  { "en": "Apa Itu Token Dalam Kompilasi?", "id": "Unit Leksikal Terkecil (Kata Kunci)." },
  { "en": "Apa Itu Lexer (Scanner)?", "id": "Program Yang Melakukan Analisis Leksikal." },
  { "en": "Apa Itu Analisis Sintaks (Parsing)?", "id": "Menganalisis Struktur Tata Bahasa Kode." },
  { "en": "Apa Itu Parser?", "id": "Program Yang Melakukan Analisis Sintaks." },
  { "en": "Apa Itu Pohon Urai (Parse Tree)?", "id": "Representasi Grafis Struktur Sintaks." },
  { "en": "Apa Itu Pohon Sintaks Abstrak (AST)?", "id": "Representasi Pohon Ringkas Struktur Kode." },
  { "en": "Apa Itu Analisis Semantik?", "id": "Memeriksa Makna, Aturan Tipe Kode." },
  { "en": "Apa Itu Tabel Simbol?", "id": "Struktur Data Penyimpan Informasi Pengenal." },
  { "en": "Apa Itu Pembangkitan Kode Antara?", "id": "Menghasilkan Representasi Kode Tingkat Menengah." },
  { "en": "Apa Itu Optimisasi Kode?", "id": "Memperbaiki Kode Antara Agar Lebih Efisien." },
  { "en": "Apa Itu Pembangkitan Kode (Code Generation)?", "id": "Menghasilkan Kode Mesin Target." },
  { "en": "Apa Itu Tata Bahasa Bebas Konteks?", "id": "Aturan Formal Mendefinisikan Sintaks Bahasa." },
  { "en": "Apa Itu Backus-Naur Form (BNF)?", "id": "Notasi Standar Tata Bahasa Bebas Konteks." },
  { "en": "Apa Itu Parser LL?", "id": "Parser Top-Down, Left-to-Right, Leftmost Derivation." },
  { "en": "Apa Itu Parser LR?", "id": "Parser Bottom-Up, Left-to-Right, Rightmost Derivation." },
  { "en": "Apa Itu Penjadwalan Preemptif?", "id": "OS Dapat Menginterupsi Proses Berjalan." },
  { "en": "Apa Itu Penjadwalan Non-Preemptif?", "id": "Proses Berjalan Sampai Selesai Atau Blokir." },
  { "en": "Apa Itu Algoritma Penjadwalan FCFS?", "id": "First-Come, First-Served, Non-Preemptif." },
  { "en": "Apa Itu Algoritma Penjadwalan SJF?", "id": "Shortest-Job-First, Bisa Preemptif, Non-Preemptif." },
  { "en": "Apa Itu Starvation Dalam Penjadwalan?", "id": "Proses Tidak Pernah Mendapat Jatah CPU." },
  { "en": "Apa Itu Penjadwalan Prioritas?", "id": "Proses Dengan Prioritas Lebih Tinggi Dijalankan." },
  { "en": "Apa Itu Aging Dalam Penjadwalan?", "id": "Meningkatkan Prioritas Proses Seiring Waktu." },
  { "en": "Apa Itu Penjadwalan Round-Robin (RR)?", "id": "Setiap Proses Mendapat Jatah Waktu Tetap." },
  { "en": "Apa Itu Quantum Waktu (Time Slice)?", "id": "Jatah Waktu Dalam Algoritma Round-Robin." },
  { "en": "Apa Itu Penjadwalan Antrian Multilevel?", "id": "Memisahkan Proses Ke Dalam Antrian Berbeda." },
  { "en": "Apa Itu Fragmentasi Eksternal?", "id": "Lubang Memori Kosong Tersebar, Tak Terpakai." },
  { "en": "Apa Itu Fragmentasi Internal?", "id": "Memori Dialokasikan Lebih Besar Dari Permintaan." },
  { "en": "Apa Itu Compaction Dalam Manajemen Memori?", "id": "Memindahkan Proses Untuk Menyatukan Memori Kosong." },
  { "en": "Apa Itu Buddy System?", "id": "Algoritma Alokasi Memori." },
  { "en": "Apa Itu Page Fault?", "id": "Terjadi Saat Halaman Diminta Tidak Di Memori." },
  { "en": "Apa Itu Thrashing?", "id": "Aktivitas Paging Berlebihan, Kinerja Menurun." },
  { "en": "Apa Itu Working Set Model?", "id": "Kumpulan Halaman Yang Aktif Digunakan Proses." },
  { "en": "Apa Itu Algoritma Penggantian Halaman FIFO?", "id": "First-In, First-Out, Mengganti Halaman Terlama." },
  { "en": "Apa Itu Algoritma Penggantian Halaman Optimal?", "id": "Mengganti Halaman Paling Lama Tak Digunakan." },
  { "en": "Apa Itu Algoritma Penggantian Halaman LRU?", "id": "Least Recently Used, Mengganti Halaman Jarang Digunakan." },
  { "en": "Apa Itu Anomali Belady?", "id": "Menambah Frame Malah Menaikkan Page Fault." },
  { "en": "Apa Itu Inode Dalam Sistem Berkas?", "id": "Struktur Data Penyimpan Atribut Berkas." },
  { "en": "Apa Itu Journaling File System?", "id": "Menjaga Integritas Sistem Berkas." },
  { "en": "Apa Itu RAID (Redundant Array of Independent Disks)?", "id": "Menggabungkan Beberapa Disk Untuk Kinerja, Redundansi." },
  { "en": "Apa Itu RAID 0?", "id": "Striping, Tanpa Redundansi." },
  { "en": "Apa Itu RAID 1?", "id": "Mirroring, Redundansi Penuh." },
  { "en": "Apa Itu RAID 5?", "id": "Striping Dengan Paritas Terdistribusi." },
  { "en": "Apa Itu RAID 6?", "id": "Striping Dengan Paritas Ganda." },
  { "en": "Apa Itu Hot Swapping?", "id": "Mengganti Komponen Tanpa Mematikan Sistem." },
  { "en": "Apa Itu Blok Boot (Boot Block)?", "id": "Blok Pertama Disk Berisi Bootloader." },
  { "en": "Apa Itu Master Boot Record (MBR)?", "id": "Jenis Sektor Boot Khusus." },
  { "en": "Apa Itu GUID Partition Table (GPT)?", "id": "Standar Partisi Disk Modern." },
  { "en": "Apa Itu Panggilan Sistem (System Call)?", "id": "Antarmuka Terprogram Ke Layanan Sistem Operasi." },
  { "en": "Apa Itu Mode Pengguna (User Mode)?", "id": "Mode Eksekusi Terbatas Untuk Aplikasi." },
  { "en": "Apa Itu Mode Kernel (Kernel Mode)?", "id": "Mode Eksekusi Dengan Akses Penuh." },
  { "en": "Apa Itu Context Switch?", "id": "Menyimpan, Memulihkan Keadaan CPU Antar Proses." },
  { "en": "Apa Itu Interprocess Communication (IPC)?", "id": "Mekanisme Komunikasi Antar Proses." },
  { "en": "Apa Itu Memori Bersama (Shared Memory)?", "id": "Metode IPC (Interprocess Communication) Efisien." },
  { "en": "Apa Itu Antrian Pesan (Message Queue)?", "id": "Metode IPC (Interprocess Communication) Asinkron." },
  { "en": "Apa Itu Pipa (Pipe)?", "id": "Saluran Komunikasi Antar Proses Terkait." },
  { "en": "Apa Itu Soket (Socket)?", "id": "Titik Akhir Komunikasi Jaringan Antar Proses." },
  { "en": "Apa Itu Remote Procedure Call (RPC)?", "id": "Memanggil Prosedur Di Komputer Jarak Jauh." },
  { "en": "Apa Itu Monolithic Kernel?", "id": "Seluruh Layanan OS Berjalan Di Ruang Kernel." },
  { "en": "Apa Itu Microkernel?", "id": "Hanya Layanan Paling Dasar Di Ruang Kernel." },
  { "en": "Apa Itu Kernel Hibrida?", "id": "Gabungan Arsitektur Monolitik Dan Microkernel." },
  { "en": "Apa Itu Hypervisor?", "id": "Membuat Dan Menjalankan Mesin Virtual." },
  { "en": "Apa Itu Mesin Virtual (VM)?", "id": "Emulasi Sistem Komputer." },
  { "en": "Apa Itu Tipe 1 Hypervisor (Bare-Metal)?", "id": "Berjalan Langsung Di Atas Perangkat Keras." },
  { "en": "Apa Itu Tipe 2 Hypervisor (Hosted)?", "id": "Berjalan Di Atas Sistem Operasi." },
  { "en": "Apa Itu Emulasi?", "id": "Meniru Perangkat Keras Berbeda Sepenuhnya." },
  { "en": "Apa Itu Virtualisasi?", "id": "Abstraksi Sumber Daya Perangkat Keras Sama." },
  { "en": "Apa Itu Paravirtualisasi?", "id": "Sistem Operasi Tamu Diubah Bekerja Sama." },
  { "en": "Apa Itu VT-x/AMD-V?", "id": "Ekstensi Perangkat Keras CPU Untuk Virtualisasi." },
  { "en": "Apa Itu Live Migration?", "id": "Memindahkan Mesin Virtual Berjalan Antar Host." },
  { "en": "Apa Itu Snapshot Mesin Virtual?", "id": "Menyimpan Keadaan Mesin Virtual." },
  { "en": "Apa Itu Kontainer?", "id": "Virtualisasi Tingkat Sistem Operasi." },
  { "en": "Apa Beda Kontainer Dan VM?", "id": "Kontainer Berbagi Kernel OS Host." },
  { "en": "Apa Itu Docker Image?", "id": "Template Read-only Untuk Membuat Kontainer." },
  { "en": "Apa Itu Dockerfile?", "id": "Berkas Teks Instruksi Membangun Docker Image." },
  { "en": "Apa Itu Docker Hub?", "id": "Registri Repositori Untuk Docker Image." },
  { "en": "Apa Itu Orkestrasi Kontainer?", "id": "Manajemen Otomatis Siklus Hidup Kontainer." },
  { "en": "Apa Itu Kubernetes?", "id": "Platform Orkestrasi Kontainer Open-Source." },
  { "en": "Apa Itu Pod Dalam Kubernetes?", "id": "Unit Penerapan Terkecil Dalam Kubernetes." },
  { "en": "Apa Itu Service Dalam Kubernetes?", "id": "Abstraksi Jaringan Untuk Sekelompok Pod." },
  { "en": "Apa Itu Kubelet?", "id": "Agen Yang Berjalan Di Setiap Node." },
  { "en": "Apa Itu Kubectl?", "id": "Alat Baris Perintah Untuk Kubernetes." },
  { "en": "Apa Itu Helm Dalam Kubernetes?", "id": "Manajer Paket Untuk Aplikasi Kubernetes." },
  { "en": "Apa Itu Pemrograman Reaktif?", "id": "Paradigma Pemrograman Aliran Data, Perubahan." },
  { "en": "Apa Itu Observable?", "id": "Mewakili Aliran Data Asinkron." },
  { "en": "Apa Itu Observer?", "id": "Mengkonsumsi Nilai Dari Observable." },
  { "en": "Apa Itu Operator Dalam Pemrograman Reaktif?", "id": "Fungsi Murni Memanipulasi Aliran Data." },
  { "en": "Apa Itu Subscription?", "id": "Menghubungkan Observable Dengan Observer." },
  { "en": "Apa Itu Hot Observable?", "id": "Mulai Memancarkan Nilai Tanpa Menunggu." },
  { "en": "Apa Itu Cold Observable?", "id": "Mulai Memancarkan Nilai Saat Ada Subscriber." },
  { "en": "Apa Itu Backpressure?", "id": "Mekanisme Menangani Observable Lebih Cepat." },
  { "en": "Apa Itu RxJS (Reactive Extensions for JavaScript)?", "id": "Pustaka Populer Pemrograman Reaktif JavaScript." },
  { "en": "Apa Itu Mekanika Kuantum?", "id": "Teori Fisika Mendeskripsikan Alam Skala Atom." },
  { "en": "Apa Itu Kuantisasi?", "id": "Energi, Momentum Bersifat Diskret." },
  { "en": "Apa Itu Dualitas Gelombang-Partikel?", "id": "Partikel Dapat Menunjukkan Sifat Gelombang." },
  { "en": "Apa Itu Prinsip Ketidakpastian Heisenberg?", "id": "Tidak Mungkin Mengetahui Posisi, Momentum Tepat." },
  { "en": "Apa Itu Fungsi Gelombang?", "id": "Deskripsi Matematis Keadaan Kuantum." },
  { "en": "Apa Itu Persamaan Schrödinger?", "id": "Menggambarkan Evolusi Fungsi Gelombang." },
  { "en": "Apa Itu Runtuhnya Fungsi Gelombang?", "id": "Proses Pengukuran Memaksa Keadaan Tertentu." },
  { "en": "Apa Itu Kucing Schrödinger?", "id": "Eksperimen Pikiran Tentang Superposisi Kuantum." },
  { "en": "Apa Itu Interpretasi Kopenhagen?", "id": "Interpretasi Standar Mekanika Kuantum." },
  { "en": "Apa Itu Interpretasi Banyak-Dunia?", "id": "Setiap Pengukuran Menciptakan Alam Semesta Paralel." },
  { "en": "Apa Itu Dekoerensi Kuantum?", "id": "Hilangnya Sifat Kuantum Akibat Interaksi." },
  { "en": "Apa Itu Komputasi Kuantum?", "id": "Menggunakan Fenomena Kuantum Untuk Komputasi." },
  { "en": "Apa Itu Qubit?", "id": "Unit Dasar Informasi Dalam Komputer Kuantum." },
  { "en": "Apa Itu Gerbang Hadamard?", "id": "Gerbang Kuantum Menciptakan Superposisi." },
  { "en": "Apa Itu Gerbang CNOT?", "id": "Gerbang Kuantum Dua-Qubit Terkendali." },
  { "en": "Apa Itu Algoritma Shor?", "id": "Algoritma Kuantum Untuk Faktorisasi Bilangan." },
  { "en": "Apa Itu Algoritma Grover?", "id": "Algoritma Kuantum Untuk Pencarian Database." },
  { "en": "Apa Itu Supremasi Kuantum?", "id": "Komputer Kuantum Menyelesaikan Masalah Mustahil." },
  { "en": "Apa Itu Annealing Kuantum?", "id": "Metode Komputasi Kuantum Untuk Optimisasi." },
  { "en": "Apa Itu Kriptografi Kuantum?", "id": "Menggunakan Mekanika Kuantum Untuk Komunikasi Aman." },
  { "en": "Apa Itu Distribusi Kunci Kuantum (QKD)?", "id": "Protokol Berbagi Kunci Aman." },
  { "en": "Apa Itu Protokol BB84?", "id": "Protokol Distribusi Kunci Kuantum Pertama." },
  { "en": "Apa Itu Teleportasi Kuantum?", "id": "Memindahkan Keadaan Kuantum Dari Satu Tempat." },
  { "en": "Apa Itu Relativitas Khusus?", "id": "Teori Einstein Tentang Ruang, Waktu, Gerak." },
  { "en": "Apa Postulat Pertama Relativitas Khusus?", "id": "Hukum Fisika Sama Untuk Semua Pengamat." },
  { "en": "Apa Postulat Kedua Relativitas Khusus?", "id": "Kecepatan Cahaya Konstan Untuk Semua Pengamat." },
  { "en": "Apa Itu Dilatasi Waktu?", "id": "Waktu Berjalan Lebih Lambat Bagi Pengamat Bergerak." },
  { "en": "Apa Itu Kontraksi Panjang?", "id": "Objek Bergerak Tampak Lebih Pendek." },
  { "en": "Apa Itu E=mc²?", "id": "Kesetaraan Massa Dan Energi." },
  { "en": "Apa Itu Ruang-Waktu (Spacetime)?", "id": "Gabungan Tiga Dimensi Ruang, Satu Waktu." },
  { "en": "Apa Itu Diagram Minkowski?", "id": "Representasi Grafis Ruang-Waktu." },
  { "en": "Apa Itu Relativitas Umum?", "id": "Teori Gravitasi Einstein." },
  { "en": "Apa Itu Prinsip Ekuivalensi?", "id": "Gravitasi Tak Dapat Dibedakan Dari Percepatan." },
  { "en": "Bagaimana Relativitas Umum Menjelaskan Gravitasi?", "id": "Kelengkungan Ruang-Waktu Akibat Massa, Energi." },
  { "en": "Apa Itu Lensa Gravitasi?", "id": "Pembelokan Cahaya Akibat Gravitasi." },
  { "en": "Apa Itu Pergeseran Merah Gravitasi?", "id": "Cahaya Kehilangan Energi Saat Keluar Medan Gravitasi." },
  { "en": "Apa Itu Lubang Hitam (Black Hole)?", "id": "Wilayah Ruang-Waktu Gravitasi Sangat Kuat." },
  { "en": "Apa Itu Horison Peristiwa (Event Horizon)?", "id": "Batas Lubang Hitam Tak Dapat Kembali." },
  { "en": "Apa Itu Singularitas?", "id": "Titik Kepadatan Tak Terhingga Di Pusat." },
  { "en": "Apa Itu Radiasi Hawking?", "id": "Radiasi Termal Dipancarkan Lubang Hitam." },
  { "en": "Apa Itu Gelombang Gravitasi?", "id": "Riak Dalam Ruang-Waktu." },
  { "en": "Apa Itu Kosmologi?", "id": "Studi Asal Usul, Evolusi Alam Semesta." },
  { "en": "Apa Itu Big Bang?", "id": "Teori Awal Mula Alam Semesta." },
  { "en": "Apa Itu Pergeseran Merah Kosmologis?", "id": "Peregangan Cahaya Akibat Ekspansi Alam Semesta." },
  { "en": "Apa Itu Hukum Hubble?", "id": "Galaksi Saling Menjauh Dengan Kecepatan Proporsional." },
  { "en": "Apa Itu Radiasi Latar Gelombang Mikro Kosmik (CMB)?", "id": "Sisa Cahaya Dari Alam Semesta Awal." },
  { "en": "Apa Itu Materi Gelap (Dark Matter)?", "id": "Materi Tak Terlihat, Berinteraksi Gravitasi." },
  { "en": "Apa Itu Energi Gelap (Dark Energy)?", "id": "Penyebab Percepatan Ekspansi Alam Semesta." },
  { "en": "Apa Itu Inflasi Kosmik?", "id": "Periode Ekspansi Sangat Cepat." },
  { "en": "Apa Itu Model Standar Fisika Partikel?", "id": "Teori Partikel Elementer, Interaksinya." },
  { "en": "Apa Itu Kuark (Quark)?", "id": "Partikel Elementer Penyusun Proton, Neutron." },
  { "en": "Apa Itu Lepton?", "id": "Kelas Partikel Elementer (Elektron, Neutrino)." },
  { "en": "Apa Itu Gaya Kuat?", "id": "Mengikat Kuark Bersama Dalam Proton, Neutron." },
  { "en": "Apa Itu Gaya Lemah?", "id": "Bertanggung Jawab Atas Peluruhan Radioaktif." },
  { "en": "Apa Itu Gaya Elektromagnetik?", "id": "Gaya Antara Partikel Bermuatan." },
  { "en": "Apa Itu Foton?", "id": "Partikel Pembawa Gaya Elektromagnetik." },
  { "en": "Apa Itu Gluon?", "id": "Partikel Pembawa Gaya Kuat." },
  { "en": "Apa Itu Boson W Dan Z?", "id": "Partikel Pembawa Gaya Lemah." },
  { "en": "Apa Itu Boson Higgs?", "id": "Partikel Terkait Mekanisme Pemberian Massa." },
  { "en": "Apa Itu Antimateri?", "id": "Materi Terdiri Dari Antipartikel." },
  { "en": "Apa Itu Anihilasi?", "id": "Partikel, Antipartikel Bertemu, Menjadi Energi." },
  { "en": "Apa Itu Neutrino?", "id": "Partikel Netral, Sangat Ringan." },
  { "en": "Apa Itu Kromodinamika Kuantum (QCD)?", "id": "Teori Gaya Nuklir Kuat." },
  { "en": "Apa Itu Elektrodinamika Kuantum (QED)?", "id": "Teori Kuantum Gaya Elektromagnetik." },
  { "en": "Apa Itu Teori Elektrolemah?", "id": "Penyatuan Gaya Elektromagnetik Dan Lemah." },
  { "en": "Apa Itu Teori Segalanya?", "id": "Teori Hipotetis Penyatuan Semua Gaya." },
  { "en": "Apa Itu Teori Senar (String Theory)?", "id": "Partikel Adalah Getaran Senar Kecil." },
  { "en": "Apa Itu Supersimetri (SUSY)?", "id": "Simetri Hipotetis Antara Fermion, Boson." },
  { "en": "Apa Itu Pemercepat Partikel?", "id": "Mesin Mempercepat Partikel Ke Energi Tinggi." },
  { "en": "Apa Itu Large Hadron Collider (LHC)?", "id": "Pemercepat Partikel Terbesar Di Dunia." },
  { "en": "Apa Itu Termodinamika?", "id": "Studi Panas, Kerja, Energi." },
  { "en": "Apa Itu Hukum Pertama Termodinamika?", "id": "Kekekalan Energi." },
  { "en": "Apa Itu Hukum Kedua Termodinamika?", "id": "Entropi Total Selalu Meningkat." },
  { "en": "Apa Itu Entropi?", "id": "Ukuran Ketidakteraturan Atau Keacakan." },
  { "en": "Apa Itu Hukum Ketiga Termodinamika?", "id": "Entropi Mendekati Nol Saat Suhu Nol Mutlak." },
  { "en": "Apa Itu Nol Mutlak?", "id": "Suhu Terendah Yang Mungkin (0 Kelvin)." },
  { "en": "Apa Itu Siklus Carnot?", "id": "Siklus Termodinamika Paling Efisien." },
  { "en": "Apa Itu Efisiensi Carnot?", "id": "Efisiensi Maksimum Teoritis Mesin Panas." },
  { "en": "Apa Itu Entalpi?", "id": "Ukuran Total Panas Sistem." },
  { "en": "Apa Itu Energi Bebas Gibbs?", "id": "Energi Tersedia Melakukan Kerja." },
  { "en": "Apa Itu Mekanika Statistik?", "id": "Menghubungkan Sifat Mikroskopis Dengan Termodinamika." },
  { "en": "Apa Itu Ensemble Statistik?", "id": "Kumpulan Sistem Hipotetis." },
  { "en": "Apa Itu Optik?", "id": "Studi Perilaku Dan Sifat Cahaya." },
  { "en": "Apa Itu Refleksi?", "id": "Pemantulan Cahaya." },
  { "en": "Apa Itu Refraksi?", "id": "Pembiasan Cahaya." },
  { "en": "Apa Itu Hukum Snell?", "id": "Mendeskripsikan Sudut Pembiasan Cahaya." },
  { "en": "Apa Itu Indeks Bias?", "id": "Ukuran Seberapa Cepat Cahaya Merambat." },
  { "en": "Apa Itu Difraksi?", "id": "Penyebaran Gelombang Saat Melewati Celah." },
  { "en": "Apa Itu Interferensi?", "id": "Superposisi Gelombang (Konstruktif, Destruktif)." },
  { "en": "Apa Itu Eksperimen Celah Ganda Young?", "id": "Menunjukkan Sifat Gelombang Cahaya." },
  { "en": "Apa Itu Polarisasi Cahaya?", "id": "Orientasi Osilasi Medan Listrik." },
  { "en": "Apa Itu Spektrum Elektromagnetik?", "id": "Rentang Semua Jenis Radiasi Elektromagnetik." },
  { "en": "Apa Itu Panjang Gelombang?", "id": "Jarak Antara Puncak Gelombang Berurutan." },
  { "en": "Apa Itu Frekuensi?", "id": "Jumlah Siklus Gelombang Per Detik." },
  { "en": "Apa Itu Foton?", "id": "Kuantum Cahaya Atau Radiasi Elektromagnetik." },
  { "en": "Apa Itu Efek Doppler?", "id": "Perubahan Frekuensi Akibat Gerakan Relatif." },
  { "en": "Apa Itu Aberasi Sferis?", "id": "Cacat Lensa Di Mana Sinar Paralel." },
  { "en": "Apa Itu Aberasi Kromatik?", "id": "Cacat Lensa Di Mana Warna Berbeda." },
  { "en": "Apa Itu Holografi?", "id": "Teknik Merekam, Merekonstruksi Gambar Tiga Dimensi." },
  { "en": "Apa Itu Laser?", "id": "Sumber Cahaya Koheren, Monokromatik." },
  { "en": "Apa Kepanjangan LASER?", "id": "Light Amplification by Stimulated Emission of Radiation." },
  { "en": "Apa Itu Emisi Spontan?", "id": "Emisi Foton Tanpa Stimulasi Eksternal." },
  { "en": "Apa Itu Emisi Terstimulasi?", "id": "Emisi Foton Dipicu Foton Masuk." },
  { "en": "Apa Itu Inversi Populasi?", "id": "Kondisi Diperlukan Untuk Aksi Laser." },
  { "en": "Apa Itu Koherensi?", "id": "Hubungan Fasa Konstan Antar Gelombang." },
  { "en": "Apa Itu Monokromatisitas?", "id": "Terdiri Dari Satu Warna Tunggal." },
  { "en": "Apa Itu Mekanika Klasik?", "id": "Fisika Benda Makroskopis (Newtonian)." },
  { "en": "Apa Itu Hukum Gerak Pertama Newton?", "id": "Inersia." },
  { "en": "Apa Itu Hukum Gerak Kedua Newton?", "id": "F = ma." },
  { "en": "Apa Itu Hukum Gerak Ketiga Newton?", "id": "Aksi Dan Reaksi." },
  { "en": "Apa Itu Momentum Linear?", "id": "Ukuran Kesulitan Menghentikan Benda Bergerak." },
  { "en": "Apa Rumus Momentum Linear?", "id": "Massa Dikalikan Dengan Kecepatan (p=mv)." },
  { "en": "Apa Itu Impuls?", "id": "Perubahan Momentum Suatu Benda." },
  { "en": "Apa Itu Hukum Kekekalan Momentum?", "id": "Total Momentum Sistem Terisolasi Tetap Konstan." },
  { "en": "Apa Itu Tumbukan Lenting Sempurna?", "id": "Energi Kinetik Total Tetap Terjaga." },
  { "en": "Apa Itu Tumbukan Tidak Lenting Sempurna?", "id": "Benda Menempel Bersama Setelah Tumbukan." },
  { "en": "Apa Itu Momentum Sudut?", "id": "Ukuran Momentum Rotasi Suatu Benda." },
  { "en": "Apa Itu Torsi?", "id": "Gaya Yang Menyebabkan Rotasi." },
  { "en": "Apa Itu Momen Inersia?", "id": "Ukuran Kelembaman Rotasi Suatu Benda." },
  { "en": "Apa Itu Hukum Kekekalan Momentum Sudut?", "id": "Total Momentum Sudut Sistem Tetap Konstan." },
  { "en": "Apa Itu Energi Kinetik?", "id": "Energi Yang Dimiliki Benda Akibat Geraknya." },
  { "en": "Apa Itu Energi Potensial?", "id": "Energi Yang Disimpan Benda Akibat Posisinya." },
  { "en": "Apa Itu Energi Mekanik?", "id": "Jumlah Energi Kinetik Dan Energi Potensial." },
  { "en": "Apa Itu Kerja Dalam Fisika?", "id": "Energi Ditransfer Oleh Gaya." },
  { "en": "Apa Itu Daya Dalam Fisika?", "id": "Laju Di Mana Kerja Dilakukan." },
  { "en": "Apa Itu Gerak Harmonik Sederhana (GHS)?", "id": "Gerak Osilasi Periodik." },
  { "en": "Apa Itu Amplitudo?", "id": "Simpangan Maksimum Dari Posisi Setimbang." },
  { "en": "Apa Itu Periode?", "id": "Waktu Untuk Menyelesaikan Satu Siklus Penuh." },
  { "en": "Apa Itu Frekuensi?", "id": "Jumlah Siklus Per Satuan Waktu." },
  { "en": "Apa Itu Bandul Sederhana?", "id": "Massa Tergantung Tali, Berayun." },
  { "en": "Apa Itu Resonansi?", "id": "Amplitudo Osilasi Maksimum Akibat Gaya Eksternal." },
  { "en": "Apa Itu Efek Redaman (Damping)?", "id": "Hilangnya Energi Dalam Sistem Osilasi." },
  { "en": "Apa Itu Gelombang Mekanik?", "id": "Getaran Merambat Melalui Medium." },
  { "en": "Apa Itu Gelombang Transversal?", "id": "Getaran Tegak Lurus Arah Rambat." },
  { "en": "Apa Itu Gelombang Longitudinal?", "id": "Getaran Sejajar Arah Rambat." },
  { "en": "Sebutkan Contoh Gelombang Transversal?", "id": "Gelombang Cahaya, Gelombang Pada Tali." },
  { "en": "Sebutkan Contoh Gelombang Longitudinal?", "id": "Gelombang Suara." },
  { "en": "Apa Itu Superposisi Gelombang?", "id": "Penjumlahan Amplitudo Gelombang Yang Bertemu." },
  { "en": "Apa Itu Interferensi Konstruktif?", "id": "Gelombang Saling Menguatkan." },
  { "en": "Apa Itu Interferensi Destruktif?", "id": "Gelombang Saling Melemahkan." },
  { "en": "Apa Itu Gelombang Berdiri?", "id": "Hasil Interferensi Dua Gelombang Berlawanan." },
  { "en": "Apa Itu Simpul (Node) Gelombang Berdiri?", "id": "Titik Dengan Amplitudo Nol." },
  { "en": "Apa Itu Perut (Antinode) Gelombang Berdiri?", "id": "Titik Dengan Amplitudo Maksimum." },
  { "en": "Apa Itu Efek Doppler?", "id": "Perubahan Frekuensi Akibat Gerakan Sumber/Pengamat." },
  { "en": "Apa Itu Gelombang Kejut (Shock Wave)?", "id": "Gelombang Tekanan Bergerak Cepat." },
  { "en": "Apa Itu Bilangan Mach?", "id": "Rasio Kecepatan Objek Terhadap Kecepatan Suara." },
  { "en": "Apa Itu Fluida?", "id": "Zat Yang Dapat Mengalir (Cairan, Gas)." },
  { "en": "Apa Itu Tekanan?", "id": "Gaya Per Satuan Luas." },
  { "en": "Apa Itu Hukum Pascal?", "id": "Tekanan Dalam Fluida Tertutup Diteruskan Merata." },
  { "en": "Apa Itu Prinsip Archimedes?", "id": "Gaya Apung Sama Dengan Berat Fluida." },
  { "en": "Apa Itu Tegangan Permukaan?", "id": "Kecenderungan Permukaan Cairan Mengerut." },
  { "en": "Apa Itu Viskositas?", "id": "Ukuran Ketahanan Fluida Terhadap Aliran." },
  { "en": "Apa Itu Aliran Laminar?", "id": "Aliran Fluida Halus Dan Teratur." },
  { "en": "Apa Itu Aliran Turbulen?", "id": "Aliran Fluida Kacau Dan Tidak Teratur." },
  { "en": "Apa Itu Persamaan Bernoulli?", "id": "Menghubungkan Tekanan, Kecepatan, Ketinggian Fluida." },
  { "en": "Apa Itu Konduksi Panas?", "id": "Transfer Panas Melalui Kontak Langsung." },
  { "en": "Apa Itu Konveksi Panas?", "id": "Transfer Panas Melalui Gerakan Fluida." },
  { "en": "Apa Itu Radiasi Panas?", "id": "Transfer Panas Melalui Gelombang Elektromagnetik." },
  { "en": "Apa Itu Kalor Jenis?", "id": "Jumlah Panas Menaikkan Suhu Zat." },
  { "en": "Apa Itu Kalor Laten?", "id": "Panas Diserap/Dilepaskan Selama Perubahan Fasa." },
  { "en": "Apa Itu Kisi Kristal?", "id": "Susunan Atom Teratur Dalam Benda Padat." },
  { "en": "Apa Itu Sel Satuan (Unit Cell)?", "id": "Unit Berulang Terkecil Dari Kisi Kristal." },
  { "en": "Apa Itu Cacat Titik (Point Defect)?", "id": "Ketidaksempurnaan Kisi Melibatkan Satu Atom." },
  { "en": "Apa Itu Dislokasi Dalam Kristal?", "id": "Cacat Garis Dalam Struktur Kristal." },
  { "en": "Apa Itu Ikatan Ionik?", "id": "Ikatan Kimia Transfer Elektron." },
  { "en": "Apa Itu Ikatan Kovalen?", "id": "Ikatan Kimia Berbagi Pasangan Elektron." },
  { "en": "Apa Itu Ikatan Logam?", "id": "Ikatan Antar Atom Logam." },
  { "en": "Apa Itu Elektron Valensi?", "id": "Elektron Di Kulit Terluar Atom." },
  { "en": "Apa Itu Aturan Oktet?", "id": "Kecenderungan Atom Memiliki Delapan Elektron Valensi." },
  { "en": "Apa Itu Bilangan Oksidasi?", "id": "Ukuran Derajat Oksidasi Atom." },
  { "en": "Apa Itu Reaksi Redoks?", "id": "Reaksi Kimia Melibatkan Perubahan Oksidasi." },
  { "en": "Apa Itu Oksidasi?", "id": "Kehilangan Elektron." },
  { "en": "Apa Itu Reduksi?", "id": "Penerimaan Elektron." },
  { "en": "Apa Itu Sel Elektrokimia?", "id": "Perangkat Menghasilkan Listrik Dari Reaksi Kimia." },
  { "en": "Apa Itu Anoda?", "id": "Elektroda Tempat Terjadinya Oksidasi." },
  { "en": "Apa Itu Katoda?", "id": "Elektroda Tempat Terjadinya Reduksi." },
  { "en": "Apa Itu Jembatan Garam?", "id": "Menjaga Kenetralan Muatan Sel Elektrokimia." },
  { "en": "Apa Itu Potensial Elektroda Standar?", "id": "Ukuran Kecenderungan Elektroda Mengalami Reduksi." },
  { "en": "Apa Itu Deret Volta?", "id": "Urutan Logam Berdasarkan Potensial Elektroda." },
  { "en": "Apa Itu Korosi?", "id": "Kerusakan Logam Akibat Reaksi Elektrokimia." },
  { "en": "Apa Itu Pelapisan Listrik (Electroplating)?", "id": "Melapisi Logam Dengan Logam Lain." },
  { "en": "Apa Itu Mekanika Lagrangian?", "id": "Formulasi Ulang Mekanika Klasik." },
  { "en": "Apa Itu Lagrangian?", "id": "Selisih Antara Energi Kinetik Dan Potensial." },
  { "en": "Apa Itu Mekanika Hamiltonian?", "id": "Formulasi Lain Mekanika Klasik." },
  { "en": "Apa Itu Hamiltonian?", "id": "Jumlah Total Energi Kinetik Dan Potensial." },
  { "en": "Apa Itu Ruang Fasa?", "id": "Ruang Abstrak Koordinat Posisi, Momentum." },
  { "en": "Apa Itu Teorema Liouville?", "id": "Kepadatan Sistem Di Ruang Fasa Konstan." },
  { "en": "Apa Itu Kurung Poisson (Poisson Bracket)?", "id": "Operasi Biner Penting Mekanika Hamiltonian." },
  { "en": "Apa Itu Prinsip Variasi?", "id": "Prinsip Fisika Berbasis Meminimalkan Suatu Kuantitas." },
  { "en": "Apa Itu Prinsip Aksi Terkecil?", "id": "Jalur Sebenarnya Meminimalkan Aksi." },
  { "en": "Apa Itu Tensor?", "id": "Objek Geometris Generalisasi Skalar, Vektor." },
  { "en": "Apa Itu Persamaan Diferensial Biasa (ODE)?", "id": "Persamaan Diferensial Satu Variabel Independen." },
  { "en": "Apa Itu Persamaan Diferensial Parsial (PDE)?", "id": "Persamaan Diferensial Beberapa Variabel Independen." },
  { "en": "Apa Itu Orde Persamaan Diferensial?", "id": "Orde Turunan Tertinggi Dalam Persamaan." },
  { "en": "Apa Itu Persamaan Diferensial Linear?", "id": "Variabel Dependen, Turunannya Muncul Linear." },
  { "en": "Apa Itu Kondisi Awal?", "id": "Kondisi Diberikan Pada Titik Awal." },
  { "en": "Apa Itu Kondisi Batas?", "id": "Kondisi Diberikan Pada Batas Domain." },
  { "en": "Apa Itu Persamaan Karakteristik?", "id": "Persamaan Aljabar Terkait ODE Linear." },
  { "en": "Apa Itu Metode Pemisahan Variabel?", "id": "Teknik Menyelesaikan Beberapa PDE." },
  { "en": "Apa Itu Medan Skalar?", "id": "Memberi Nilai Skalar Setiap Titik Ruang." },
  { "en": "Apa Itu Medan Vektor?", "id": "Memberi Vektor Setiap Titik Ruang." },
  { "en": "Apa Itu Operator Laplace?", "id": "Operator Diferensial Divergensi Dari Gradien." },
  { "en": "Apa Itu Fungsi Green?", "id": "Solusi Persamaan Diferensial Untuk Sumber Titik." },
  { "en": "Apa Itu Ruang Hilbert?", "id": "Generalisasi Ruang Euklides." },
  { "en": "Apa Itu Operator Hermit?", "id": "Operator Sama Dengan Konjugat Adjoinnya." },
  { "en": "Apa Itu Teori Grup?", "id": "Studi Struktur Aljabar Grup." },
  { "en": "Apa Itu Grup Dalam Matematika?", "id": "Himpunan Dengan Operasi Biner." },
  { "en": "Apa Itu Simetri?", "id": "Sifat Sistem Tidak Berubah Transformasi." },
  { "en": "Apa Itu Teorema Noether?", "id": "Menghubungkan Simetri Dengan Hukum Kekekalan." },
  { "en": "Apa Itu Sistem LTI (Linear Time-Invariant)?", "id": "Sistem Linear Yang Sifatnya Tak Berubah Waktu." },
  { "en": "Apa Itu Respon Impuls Suatu Sistem?", "id": "Output Sistem Saat Diberi Input Impuls." },
  { "en": "Apa Saja Sifat Operasi Konvolusi?", "id": "Komutatif, Asosiatif, Dan Distributif." },
  { "en": "Apa Fungsi Utama Transformasi-Z?", "id": "Menganalisis Sinyal Dan Sistem Waktu Diskret." },
  { "en": "Apa Itu Daerah Konvergensi (ROC)?", "id": "Daerah Di Mana Nilai Z-Transform Konvergen." },
  { "en": "Apa Hubungan Stabilitas Dengan Kutub Domain-Z?", "id": "Stabil Jika Semua Kutub Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Diagram Blok Sistem Kontrol?", "id": "Representasi Grafis Komponen Dan Aliran Sinyal." },
  { "en": "Apa Fungsi Titik Penjumlahan (Summing Point)?", "id": "Menjumlahkan Atau Mengurangkan Beberapa Sinyal." },
  { "en": "Apa Fungsi Titik Cabang (Takeoff Point)?", "id": "Menggunakan Satu Sinyal Untuk Beberapa Input." },
  { "en": "Apa Itu Fungsi Alih Loop Terbuka?", "id": "Rasio Output Terhadap Input Tanpa Umpan Balik." },
  { "en": "Apa Itu Inverter Sumber Impedansi (Z-Source)?", "id": "Topologi Inverter Yang Dapat Menaikkan Tegangan." },
  { "en": "Apa Itu Konverter Modular Multilevel (MMC)?", "id": "Topologi Inverter Untuk Aplikasi Tegangan Tinggi." },
  { "en": "Apa Itu Kontrol Mode Arus (Current Mode Control)?", "id": "Loop Kontrol Berdasarkan Pengukuran Arus Induktor." },
  { "en": "Apa Keuntungan Kontrol Mode Arus?", "id": "Respon Cepat Dan Perlindungan Arus Alami." },
  { "en": "Apa Itu Kompensasi Kemiringan (Slope Compensation)?", "id": "Mencegah Osilasi Sub-harmonik Dalam Kontrol Arus." },
  { "en": "Apa Fungsi Utama Penggerak Gerbang (Gate Driver)?", "id": "Menguatkan Sinyal Untuk Mengemudikan Transistor Daya." },
  { "en": "Mengapa Penggerak Gerbang (Gate Driver) Butuh Isolasi?", "id": "Melindungi Sirkuit Kontrol Dari Tegangan Tinggi." },
  { "en": "Apa Itu Catu Daya Bootstrap?", "id": "Sirkuit Sederhana Pemasok Daya Gate Driver Atas." },
  { "en": "Apa Itu Efek Miller Pada Transistor?", "id": "Peningkatan Kapasitansi Input Akibat Penguatan." },
  { "en": "Apa Itu Plateau Miller?", "id": "Bagian Datar Tegangan Gerbang Selama Switching." },
  { "en": "Apa Itu Mesin Keadaan Hingga (FSM)?", "id": "Model Komputasi Dengan Jumlah Keadaan Terbatas." },
  { "en": "Apa Ciri Khas Mesin Moore?", "id": "Output Hanya Bergantung Pada Keadaan Saat Ini." },
  { "en": "Apa Ciri Khas Mesin Mealy?", "id": "Output Bergantung Keadaan Saat Ini Dan Input." },
  { "en": "Apa Itu Diagram Transisi Keadaan?", "id": "Representasi Grafis Dari Mesin Keadaan Hingga." },
  { "en": "Apa Itu Tabel Transisi Keadaan?", "id": "Representasi Tabular Dari Mesin Keadaan Hingga." },
  { "en": "Apa Itu Penetapan Keadaan (State Assignment)?", "id": "Memberikan Kode Biner Unik Untuk Setiap Keadaan." },
  { "en": "Apa Itu Pengkodean Keadaan Biner?", "id": "Metode Penetapan Keadaan Paling Ringkas." },
  { "en": "Apa Itu Pengkodean Keadaan One-Hot?", "id": "Satu Bit Aktif Untuk Setiap Keadaan." },
  { "en": "Apa Itu Kebocoran Osilator Lokal (LO Leakage)?", "id": "Sinyal Osilator Lokal Bocor Ke Port Lain." },
  { "en": "Apa Itu Desensitisasi?", "id": "Penurunan Sensitivitas Penerima Akibat Sinyal Pengganggu." },
  { "en": "Apa Itu Mixer Frekuensi?", "id": "Sirkuit Pengubah Frekuensi Sinyal Radio." },
  { "en": "Apa Itu Frekuensi Gambar (Image Frequency)?", "id": "Frekuensi Tak Diinginkan Dalam Penerima Superheterodyne." },
  { "en": "Apa Fungsi Filter Penolak Gambar?", "id": "Menekan Sinyal Pada Frekuensi Gambar." },
  { "en": "Apa Itu Konversi Naik (Upconversion)?", "id": "Mengubah Sinyal Ke Frekuensi Lebih Tinggi." },
  { "en": "Apa Itu Konversi Turun (Downconversion)?", "id": "Mengubah Sinyal Ke Frekuensi Lebih Rendah." },
  { "en": "Apa Itu Osilator Lokal (LO)?", "id": "Sumber Frekuensi Referensi Untuk Proses Konversi." },
  { "en": "Apa Itu Kebocoran LO-RF?", "id": "Sinyal Osilator Lokal Bocor Ke Port RF." },
  { "en": "Apa Itu Linearitas Dalam Konteks Mixer?", "id": "Kemampuan Menghasilkan Frekuensi Tanpa Distorsi." },
  { "en": "Apa Itu Titik Intersep Orde Ketiga (IIP3)?", "id": "Ukuran Kinerja Linearitas Komponen Frekuensi Radio." },
  { "en": "Apa Itu Pemblokiran (Blocking) Dalam Penerima?", "id": "Sinyal Kuat Mengurangi Sensitivitas Penerima." },
  { "en": "Apa Itu Persamaan Dioda Shockley?", "id": "Model Matematis Hubungan Arus-Tegangan Dioda." },
  { "en": "Apa Itu Arus Saturasi Balik (Is)?", "id": "Arus Bocor Dioda Ideal Saat Bias Mundur." },
  { "en": "Apa Itu Faktor Idealitas Dioda (n)?", "id": "Ukuran Penyimpangan Dioda Dari Perilaku Ideal." },
  { "en": "Apa Itu Model Transistor Ebers-Moll?", "id": "Model Fisik Sirkuit Ekuivalen Transistor BJT." },
  { "en": "Apa Itu Arus Bocor Kolektor-Emitor (ICEO)?", "id": "Arus Kecil Saat Basis Dibiarkan Terbuka." },
  { "en": "Apa Itu Tegangan Early (VA)?", "id": "Parameter Yang Memodelkan Efek Early Transistor." },
  { "en": "Apa Itu Teorema Fluktuasi-Disipasi?", "id": "Menghubungkan Respon Sistem Dengan Fluktuasi Termal." },
  { "en": "Apa Sumber Derau Johnson-Nyquist?", "id": "Agitasi Termal Acak Dari Pembawa Muatan." },
  { "en": "Apa Sumber Derau Tembak (Shot Noise)?", "id": "Sifat Diskret Dari Aliran Pembawa Muatan." },
  { "en": "Apa Sumber Derau Kedip (1/f Noise)?", "id": "Perangkap Dan Pelepasan Muatan Di Semikonduktor." },
  { "en": "Apa Itu Akurasi Pengukuran?", "id": "Seberapa Dekat Hasil Ukur Dengan Nilai Benar." },
  { "en": "Apa Itu Presisi Pengukuran?", "id": "Seberapa Dekat Pengukuran Berulang Satu Sama Lain." },
  { "en": "Apa Itu Resolusi Pengukuran?", "id": "Perubahan Terkecil Yang Dapat Diukur." },
  { "en": "Apa Itu Sensitivitas Pengukuran?", "id": "Rasio Perubahan Output Terhadap Input." },
  { "en": "Apa Itu Kesalahan Linearitas?", "id": "Penyimpangan Dari Respon Garis Lurus." },
  { "en": "Apa Itu Kesalahan Histeresis?", "id": "Perbedaan Output Saat Input Naik Dan Turun." },
  { "en": "Apa Fungsi Jembatan Kelvin?", "id": "Mengukur Nilai Resistansi Sangat Rendah Akurat." },
  { "en": "Apa Fungsi Jembatan Maxwell?", "id": "Mengukur Induktansi Dengan Membandingkan Kapasitor." },
  { "en": "Apa Fungsi Jembatan Hay?", "id": "Mengukur Induktor Dengan Faktor Q Tinggi." },
  { "en": "Apa Fungsi Jembatan Schering?", "id": "Mengukur Kapasitansi Dan Faktor Disipasi Isolator." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Nuklir (PLTN)?", "id": "Menggunakan Fisi Nuklir Menghasilkan Panas, Listrik." },
  { "en": "Apa Itu Reaktor Air Mendidih (BWR)?", "id": "Reaktor Nuklir Mendidihkan Air Di Dalam Inti." },
  { "en": "Apa Itu Reaktor Air Bertekanan (PWR)?", "id": "Reaktor Nuklir Menggunakan Air Bertekanan Tinggi." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Gas (PLTG)?", "id": "Menggunakan Turbin Gas Untuk Memutar Generator." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Uap (PLTU)?", "id": "Menggunakan Uap Untuk Memutar Turbin Uap." },
  { "en": "Apa Itu Pembangkit Siklus Gabungan (PLTGU)?", "id": "Menggabungkan Turbin Gas Dan Turbin Uap." },
  { "en": "Apa Itu Ko-generasi (CHP - Combined Heat and Power)?", "id": "Menghasilkan Listrik Dan Panas Sekaligus." },
  { "en": "Apa Itu Teknologi Batubara Bersih?", "id": "Mengurangi Emisi Dari Pembakaran Batubara." },
  { "en": "Apa Itu Penangkapan Karbon (Carbon Capture)?", "id": "Menangkap Emisi CO2 Dari Sumber Industri." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air (PLTA)?", "id": "Menggunakan Energi Air Untuk Menghasilkan Listrik." },
  { "en": "Apa Itu Apertur Efektif Antena?", "id": "Area Efektif Penangkapan Daya Gelombang Radio." },
  { "en": "Apa Itu Suhu Derau Antena?", "id": "Ukuran Derau Eksternal Diterima Antena." },
  { "en": "Apa Itu Persamaan Transmisi Friis?", "id": "Menghitung Daya Diterima Dalam Tautan Nirkabel." },
  { "en": "Apa Itu Sistem Kontrol Waktu-Diskret?", "id": "Sistem Kontrol Yang Beroperasi Pada Sampel Diskret." },
  { "en": "Apa Efek Kuantisasi Dalam Kontrol Digital?", "id": "Menimbulkan Kesalahan Akibat Representasi Nilai Terbatas." },
  { "en": "Apa Itu Periode Sampling (Sampling Period)?", "id": "Waktu Antara Dua Sampel Berurutan." },
  { "en": "Bagaimana Stabilitas Sistem Waktu-Diskret Ditentukan?", "id": "Posisi Kutub Terhadap Lingkaran Satuan." },
  { "en": "Apa Itu Model Ruang Keadaan Waktu-Diskret?", "id": "Representasi Matematis Sistem Menggunakan Matriks." },
  { "en": "Apa Itu Elektromagnetika Komputasi (CEM)?", "id": "Menyelesaikan Masalah Elektromagnetik Menggunakan Komputer." },
  { "en": "Apa Itu Metode Momen (MoM)?", "id": "Teknik Numerik Untuk Analisis Antena, Hamburan." },
  { "en": "Apa Itu Metode Elemen Hingga (FEM)?", "id": "Teknik Numerik Memecah Domain Menjadi Elemen." },
  { "en": "Apa Itu Metode FDTD (Finite-Difference Time-Domain)?", "id": "Menyelesaikan Persamaan Maxwell Langsung Domain Waktu." },
  { "en": "Apa Itu Sel Surya Film Tipis?", "id": "Sel Surya Dibuat Dari Lapisan Tipis Material." },
  { "en": "Apa Itu Sel Surya Perovskit?", "id": "Jenis Sel Surya Baru Berpotensi Efisiensi Tinggi." },
  { "en": "Apa Itu Sel Surya Multi-Simpang (Multi-Junction)?", "id": "Sel Surya Dengan Beberapa Lapisan Bahan Berbeda." },
  { "en": "Apa Itu Efisiensi Konversi Sel Surya?", "id": "Rasio Daya Listrik Terhadap Daya Surya Masuk." },
  { "en": "Apa Itu Sistem Proteksi Petir (LPS)?", "id": "Sistem Pelindung Bangunan Dari Sambaran Petir." },
  { "en": "Apa Itu Ikatan Ekuipotensial?", "id": "Menghubungkan Semua Bagian Konduktif Menyamakan Potensial." },
  { "en": "Apa Itu Tegangan Tahan Impuls?", "id": "Tegangan Impuls Maksimum Tanpa Terjadi Kegagalan." },
  { "en": "Apa Itu Koordinasi Isolasi?", "id": "Mendesain Kekuatan Isolasi Sesuai Tegangan Sistem." },
  { "en": "Apa Itu Protokol DNP3?", "id": "Protokol Komunikasi Untuk Sistem Otomasi Tenaga." },
  { "en": "Apa Itu Standar IEC 61850?", "id": "Standar Internasional Komunikasi Gardu Induk." },
  { "en": "Apa Itu Pesan GOOSE?", "id": "Pesan Cepat Untuk Sinyal Trip, Interlock." },
  { "en": "Apa Itu Pematangan (Annealing) Dalam Fabrikasi?", "id": "Proses Pemanasan Memperbaiki Struktur Kristal." },
  { "en": "Apa Itu Oksidasi Termal?", "id": "Proses Menumbuhkan Lapisan Silikon Dioksida (SiO₂)." },
  { "en": "Apa Itu Chemical Vapor Deposition (CVD)?", "id": "Proses Deposisi Film Tipis Dari Gas." },
  { "en": "Apa Itu Sputtering?", "id": "Teknik Deposisi Fisik Menggunakan Plasma Argon." },
  { "en": "Apa Itu Resistor Beban Aktif?", "id": "Menggunakan Transistor Sebagai Resistor Beban." },
  { "en": "Apa Itu Amplifier Umpan Arus (CFA)?", "id": "Jenis Amplifier Operasional Berkecepatan Sangat Tinggi." },
  { "en": "Apa Itu Topologi Filter Sallen-Key?", "id": "Topologi Populer Untuk Filter Aktif Orde Dua." },
  { "en": "Apa Itu Konverter Tegangan Ke Frekuensi (VFC)?", "id": "Rangkaian Yang Menghasilkan Frekuensi Proporsional Tegangan." },
  { "en": "Apa Itu Kode Siklik?", "id": "Jenis Kode Koreksi Error Blok Linear." },
  { "en": "Apa Itu Polinomial Generator?", "id": "Polinomial Kunci Dalam Pengkodean Siklik." },
  { "en": "Apa Itu Register Geser Umpan Balik Linear (LFSR)?", "id": "Rangkaian Digital Penghasil Urutan Semu-Acak." },
  { "en": "Di Mana LFSR (Linear-Feedback Shift Register) Digunakan?", "id": "CRC, Pengacakan Data, Spektrum Sebar." },
  { "en": "Apa Itu Teorema Wiener-Khinchin?", "id": "Menghubungkan Autokorelasi, Kepadatan Spektral Daya." },
  { "en": "Apa Itu Proses Gaussian?", "id": "Proses Stokastik Variabel Acaknya Berdistribusi Normal." },
  { "en": "Apa Itu Proses Stasioner Sense Luas (WSS)?", "id": "Rata-rata Konstan, Autokorelasi Tergantung Selisih Waktu." },
  { "en": "Apa Itu Derau Putih (White Noise)?", "id": "Sinyal Acak Dengan Spektrum Daya Datar." },
  { "en": "Apa Itu Proses Ergodik?", "id": "Rata-rata Waktu Sama Dengan Rata-rata Ensemble." },
  { "en": "Apa Itu Teorema Ekipartisi Energi?", "id": "Energi Terdistribusi Merata Di Antar Derajat Kebebasan." },
  { "en": "Apa Itu Radiasi Benda Hitam?", "id": "Radiasi Elektromagnetik Dari Objek Hitam Sempurna." },
  { "en": "Apa Itu Hukum Planck?", "id": "Mendeskripsikan Spektrum Radiasi Benda Hitam." },
  { "en": "Apa Itu Hukum Pergeseran Wien?", "id": "Panjang Gelombang Puncak Berbanding Terbalik Suhu." },
  { "en": "Apa Itu Hukum Stefan-Boltzmann?", "id": "Total Daya Radiasi Proporsional T Pangkat Empat." },
  { "en": "Apa Itu Efek Compton?", "id": "Hamburan Foton Oleh Partikel Bermuatan." },
  { "en": "Apa Itu Produksi Pasangan (Pair Production)?", "id": "Foton Berenergi Tinggi Menciptakan Pasangan Partikel-Antipartikel." },
  { "en": "Apa Itu Momentum Foton?", "id": "Foton Memiliki Momentum Meskipun Tanpa Massa." },
  { "en": "Apa Itu Tekanan Radiasi?", "id": "Tekanan Diberikan Oleh Radiasi Elektromagnetik." },
  { "en": "Apa Itu Pemanasan Induksi?", "id": "Memanaskan Objek Konduktif Menggunakan Arus Eddy." },
  { "en": "Apa Itu Pemanasan Dielektrik?", "id": "Memanaskan Material Isolator Menggunakan Medan Listrik." },
  { "en": "Di Mana Pemanasan Dielektrik Digunakan?", "id": "Oven Microwave." },
  { "en": "Apa Itu Pengelasan Busur Listrik (Arc Welding)?", "id": "Menggunakan Busur Listrik Mencairkan, Menyambung Logam." },
  { "en": "Apa Itu Pengelasan Tahanan (Resistance Welding)?", "id": "Menggunakan Tekanan, Arus Listrik Menyambung Logam." },
  { "en": "Apa Itu Electrostatic Precipitator (ESP)?", "id": "Menghilangkan Partikel Debu Menggunakan Medan Listrik." },
  { "en": "Apa Itu Penggerak Elektrostatik?", "id": "Aktuator Berbasis Gaya Tarik Antar Muatan Listrik." },
  { "en": "Apa Itu Motor Elektrostatik?", "id": "Motor Berbasis Prinsip Elektrostatik." },
  { "en": "Apa Itu Magnetohidrodinamika (MHD)?", "id": "Studi Dinamika Fluida Konduktif Listrik." },
  { "en": "Apa Itu Generator MHD?", "id": "Menghasilkan Listrik Dari Aliran Plasma." },
  { "en": "Apa Itu Sistem Tenaga Hibrida?", "id": "Menggabungkan Beberapa Sumber Energi (Matahari, Angin)." },
  { "en": "Apa Itu Penyimpanan Energi Baterai (BESS)?", "id": "Menyimpan Energi Listrik Dalam Skala Besar." },
  { "en": "Apa Itu Penyimpanan Energi Udara Terkompresi (CAES)?", "id": "Menyimpan Energi Dengan Mengkompresi Udara." },
  { "en": "Apa Itu Penyimpanan Energi Pompa Air (PHES)?", "id": "Memompa Air Ke Atas, Melepaskannya." },
  { "en": "Apa Itu Penyimpanan Energi Roda Gila (Flywheel)?", "id": "Menyimpan Energi Kinetik Dalam Rotor Berputar." },
  { "en": "Apa Itu Penyimpanan Energi Magnetik Superkonduktor (SMES)?", "id": "Menyimpan Energi Dalam Medan Magnet." },
  { "en": "Apa Itu Jaringan DC (Direct Current)?", "id": "Sistem Distribusi Listrik Menggunakan Arus Searah." },
  { "en": "Apa Keuntungan Jaringan DC Mikro?", "id": "Efisiensi Tinggi Untuk Beban Elektronik." },
  { "en": "Apa Itu Penggerak Langsung (Direct Drive)?", "id": "Motor Terhubung Langsung Ke Beban." },
  { "en": "Apa Itu Motor Torsi?", "id": "Motor Dirancang Untuk Torsi Tinggi, Kecepatan Rendah." },
  { "en": "Apa Itu Motor Linear?", "id": "Motor Menghasilkan Gerak Lurus." },
  { "en": "Apa Itu Levitation Magnetik (Maglev)?", "id": "Mengangkat Objek Menggunakan Medan Magnet." },
  { "en": "Apa Itu Bantalan Magnetik (Magnetic Bearing)?", "id": "Bantalan Tanpa Kontak Menggunakan Leviasi Magnetik." },
  { "en": "Apa Itu Kopling Magnetik?", "id": "Mentransfer Torsi Antar Poros Tanpa Kontak." },
  { "en": "Apa Itu Pengujian Non-Destruktif (NDT)?", "id": "Metode Evaluasi Tanpa Merusak Material." },
  { "en": "Apa Itu Pengujian Arus Eddy (NDT)?", "id": "Metode NDT Mendeteksi Cacat Permukaan." },
  { "en": "Apa Itu Pengujian Partikel Magnetik?", "id": "Metode NDT Lain Untuk Material Feromagnetik." },
  { "en": "Apa Itu Pengujian Ultrasonik?", "id": "Menggunakan Gelombang Suara Frekuensi Tinggi." },
  { "en": "Apa Itu Radiografi Industri?", "id": "Menggunakan Sinar-X Atau Gamma Melihat Internal." },
  { "en": "Apa Itu Elektronika Cetak (Printed Electronics)?", "id": "Mencetak Sirkuit Elektronik Pada Substrat." },
  { "en": "Apa Itu Tinta Konduktif?", "id": "Tinta Mengandung Partikel Konduktif." },
  { "en": "Apa Itu Elektronika Fleksibel?", "id": "Sirkuit Dibangun Di Atas Substrat Fleksibel." },
  { "en": "Apa Itu Elektronika Dapat Diregangkan (Stretchable)?", "id": "Elektronik Dapat Diregangkan Tanpa Rusak." },
  { "en": "Apa Itu Elektronika Organik?", "id": "Menggunakan Material Organik Berbasis Karbon." },
  { "en": "Apa Itu Polimer Konduktif?", "id": "Polimer Organik Yang Menghantarkan Listrik." },
  { "en": "Apa Kepanjangan OLED (Organic Light-Emitting Diode)?", "id": "Dioda Pemancar Cahaya Organik." },
  { "en": "Apa Kepanjangan OTFT (Organic Thin-Film Transistor)?", "id": "Transistor Film Tipis Organik." },
  { "en": "Apa Itu Sel Surya Organik (OPV)?", "id": "Sel Surya Berbasis Material Organik." },
  { "en": "Apa Itu Bioelektronik?", "id": "Integrasi Elektronika Dengan Sistem Biologis." },
  { "en": "Apa Itu Antarmuka Otak-Komputer (BCI)?", "id": "Jalur Komunikasi Langsung Otak-Perangkat." },
  { "en": "Apa Itu Elektrokardiogram (ECG/EKG)?", "id": "Merekam Aktivitas Listrik Jantung." },
  { "en": "Apa Itu Elektroensefalogram (EEG)?", "id": "Merekam Aktivitas Listrik Otak." },
  { "en": "Apa Itu Elektromiogram (EMG)?", "id": "Merekam Aktivitas Listrik Otot." },
  { "en": "Apa Itu Implan Koklea?", "id": "Perangkat Elektronik Membantu Pendengaran." },
  { "en": "Apa Itu Alat Pacu Jantung?", "id": "Perangkat Elektronik Mengatur Detak Jantung." },
  { "en": "Apa Itu Defibrilator?", "id": "Memberi Sengatan Listrik Memulihkan Irama Jantung." },
  { "en": "Apa Itu Pencitraan Resonansi Magnetik (MRI)?", "id": "Teknik Pencitraan Medis Menggunakan Medan Magnet." },
  { "en": "Apa Itu Tomografi Terkomputasi (CT Scan)?", "id": "Menggunakan Sinar-X Menghasilkan Gambar Penampang." },
  { "en": "Apa Itu Pencitraan Ultrasonik?", "id": "Menggunakan Gelombang Suara Frekuensi Tinggi." },
  { "en": "Apa Itu Elektroforesis?", "id": "Gerakan Partikel Bermuatan Dalam Medan Listrik." },
  { "en": "Apa Itu Biosensor?", "id": "Perangkat Analitis Menggabungkan Komponen Biologis." },
  { "en": "Apa Itu Sistem Otonom?", "id": "Sistem Dapat Beroperasi Tanpa Intervensi Manusia." },
  { "en": "Apa Itu Kendaraan Otonom?", "id": "Mobil Yang Dapat Mengemudi Sendiri." },
  { "en": "Apa Itu LiDAR (Light Detection and Ranging)?", "id": "Metode Penginderaan Jauh Menggunakan Laser." },
  { "en": "Apa Itu Radar (Radio Detection and Ranging)?", "id": "Menggunakan Gelombang Radio Mendeteksi Objek." },
  { "en": "Apa Itu Sonar (Sound Navigation and Ranging)?", "id": "Menggunakan Gelombang Suara Untuk Navigasi." },
  { "en": "Apa Itu Inertial Measurement Unit (IMU)?", "id": "Perangkat Mengukur Orientasi, Kecepatan Sudut." },
  { "en": "Apa Komponen IMU (Inertial Measurement Unit)?", "id": "Akselerometer Dan Giroskop." },
  { "en": "Apa Itu Attitude and Heading Reference System (AHRS)?", "id": "Memberi Informasi Orientasi Tiga Dimensi." },
  { "en": "Apa Itu Global Positioning System (GPS)?", "id": "Sistem Navigasi Satelit Global." },
  { "en": "Apa Itu Navigasi Inersia?", "id": "Menggunakan IMU Melacak Posisi, Orientasi." },
  { "en": "Apa Itu Fusi Sensor (Sensor Fusion)?", "id": "Menggabungkan Data Dari Beberapa Sensor." },
  { "en": "Apa Tujuan Fusi Sensor?", "id": "Mendapatkan Estimasi Lebih Akurat, Andal." },
  { "en": "Apa Itu Filter Kalman?", "id": "Algoritma Optimal Untuk Fusi Sensor." },
  { "en": "Apa Itu Robotika Kolaboratif (Cobot)?", "id": "Robot Dirancang Bekerja Bersama Manusia." },
  { "en": "Apa Itu Etika AI?", "id": "Pertimbangan Moral Dalam Pengembangan, Penggunaan AI." },
  { "en": "Apa Itu AI Yang Dapat Dijelaskan (XAI)?", "id": "AI Yang Keputusannya Dapat Dipahami Manusia." },
  { "en": "Apa Itu Pembelajaran Federasi (Federated Learning)?", "id": "Melatih Model AI Tanpa Memindahkan Data." },
  { "en": "Apa Itu Privasi Diferensial?", "id": "Teknik Menjaga Privasi Individu Dalam Dataset." },
  { "en": "Apa Itu Enkripsi Homomorfik?", "id": "Melakukan Komputasi Pada Data Terenkripsi." },
  { "en": "Apa Itu Secure Multi-Party Computation (SMPC)?", "id": "Menghitung Fungsi Tanpa Mengungkap Input." },
  { "en": "Apa Itu Bukti Tanpa Pengetahuan (Zero-Knowledge Proof)?", "id": "Membuktikan Pernyataan Tanpa Mengungkap Informasi." },
  { "en": "Apa Itu Jaringan Saraf Konvolusional Graf (GCN)?", "id": "Menerapkan Konvolusi Pada Data Terstruktur Graf." },
  { "en": "Apa Itu Attention Mechanism Dalam AI?", "id": "Mekanisme Jaringan Fokus Bagian Input Relevan." },
  { "en": "Apa Itu Model Transformer?", "id": "Arsitektur Jaringan Saraf Berbasis Perhatian." },
  { "en": "Apa Itu BERT (Bidirectional Encoder Representations from Transformers)?", "id": "Model Bahasa Berbasis Transformer." },
  { "en": "Apa Itu GPT (Generative Pre-trained Transformer)?", "id": "Keluarga Model Bahasa Generatif." },
  { "en": "Apa Itu Pembelajaran Penguatan (Reinforcement Learning)?", "id": "Belajar Melalui Interaksi Coba-coba." },
  { "en": "Apa Itu Agent Dalam Reinforcement Learning?", "id": "Entitas Yang Belajar Dan Membuat Keputusan." },
  { "en": "Apa Itu Lingkungan (Environment)?", "id": "Dunia Tempat Agent Berinteraksi." },
  { "en": "Apa Itu Keadaan (State)?", "id": "Deskripsi Lingkungan Pada Waktu Tertentu." },
  { "en": "Apa Itu Aksi (Action)?", "id": "Pilihan Yang Dapat Diambil Agent." },
  { "en": "Apa Itu Hadiah (Reward)?", "id": "Sinyal Umpan Balik Positif/Negatif." },
  { "en": "Apa Itu Kebijakan (Policy)?", "id": "Strategi Agent Memilih Aksi." },
  { "en": "Apa Itu Fungsi Nilai (Value Function)?", "id": "Memprediksi Hadiah Masa Depan Yang Diharapkan." },
  { "en": "Apa Itu Q-Learning?", "id": "Algoritma Pembelajaran Penguatan Tanpa Model." },
  { "en": "Apa Itu Deep Q-Network (DQN)?", "id": "Menggabungkan Q-Learning Dengan Jaringan Saraf Dalam." },
  { "en": "Apa Itu Metode Simetris Komponen?", "id": "Menganalisis Sistem Tiga Fasa Tidak Seimbang." },
  { "en": "Apa Itu Urutan Nol Dalam Komponen Simetris?", "id": "Tiga Vektor Sefasa Dan Sama Besar." },
  { "en": "Apa Itu Urutan Positif Dalam Komponen Simetris?", "id": "Tiga Vektor Seimbang Urutan Fasa Normal." },
  { "en": "Apa Itu Urutan Negatif Dalam Komponen Simetris?", "id": "Tiga Vektor Seimbang Urutan Fasa Terbalik." },
  { "en": "Apa Itu Impedansi Urutan Nol Generator?", "id": "Impedansi Terhadap Arus Urutan Nol." },
  { "en": "Apa Itu Gangguan Fasa-Ke-Tanah (SLG)?", "id": "Gangguan Paling Umum Dalam Sistem Tenaga." },
  { "en": "Apa Itu Gangguan Fasa-Ke-Fasa (LL)?", "id": "Hubung Singkat Antara Dua Fasa." },
  { "en": "Apa Itu Gangguan Tiga Fasa?", "id": "Hubung Singkat Melibatkan Ketiga Fasa." },
  { "en": "Apa Itu Arus Subtransien?", "id": "Arus Gangguan Paling Awal Dan Tertinggi." },
  { "en": "Apa Itu Arus Transien?", "id": "Arus Gangguan Setelah Periode Subtransien." },
  { "en": "Apa Itu Arus Keadaan Tunak (Steady-State)?", "id": "Arus Gangguan Setelah Efek Transien Hilang." },
  { "en": "Apa Itu Reaktansi Subtransien (X\"d)?", "id": "Reaktansi Mesin Selama Periode Subtransien." },
  { "en": "Apa Itu Reaktansi Transien (X'd)?", "id": "Reaktansi Mesin Selama Periode Transien." },
  { "en": "Apa Itu Reaktansi Sinkron (Xd)?", "id": "Reaktansi Mesin Pada Kondisi Keadaan Tunak." },
  { "en": "Apa Itu Flip-Flop Tipe D?", "id": "Menyimpan Nilai Input D Saat Clock Aktif." },
  { "en": "Apa Itu Flip-Flop Tipe T (Toggle)?", "id": "Mengubah Keadaan Outputnya Setiap Pulsa Clock." },
  { "en": "Apa Itu Flip-Flop Tipe JK?", "id": "Flip-Flop Universal Dengan Input J Dan K." },
  { "en": "Apa Itu Kondisi Balapan (Race-Around) JK Flip-Flop?", "id": "Osilasi Output Saat J=K=1." },
  { "en": "Bagaimana Mengatasi Kondisi Balapan JK Flip-Flop?", "id": "Menggunakan Desain Master-Slave." },
  { "en": "Apa Itu Flip-Flop Master-Slave?", "id": "Dua Flip-Flop Bekerja Pada Tepi Berlawanan." },
  { "en": "Apa Itu Input Preset Dan Clear?", "id": "Input Asinkron Mengatur Keadaan Flip-Flop." },
  { "en": "Apa Itu Register?", "id": "Kumpulan Flip-Flop Menyimpan Data Biner." },
  { "en": "Apa Itu Register SIPO (Serial-In, Parallel-Out)?", "id": "Data Masuk Serial, Keluar Paralel." },
  { "en": "Apa Itu Register PISO (Parallel-In, Serial-Out)?", "id": "Data Masuk Paralel, Keluar Serial." },
  { "en": "Apa Itu Penghitung (Counter) Sinkron?", "id": "Semua Flip-Flop Berubah Keadaan Bersamaan." },
  { "en": "Apa Itu Penghitung (Counter) Asinkron (Ripple)?", "id": "Output Flip-Flop Memicu Clock Berikutnya." },
  { "en": "Apa Kerugian Penghitung Riak (Ripple Counter)?", "id": "Adanya Penundaan Propagasi Kumulatif." },
  { "en": "Apa Itu Penghitung Cincin (Ring Counter)?", "id": "Register Geser Dengan Output Terhubung Input." },
  { "en": "Apa Itu Penghitung Johnson?", "id": "Penghitung Cincin Dengan Umpan Balik Terbalik." },
  { "en": "Apa Itu Modulus Penghitung?", "id": "Jumlah Keadaan Unik Yang Dimiliki Penghitung." },
  { "en": "Apa Itu Dekoder Biner?", "id": "Mengaktifkan Satu Output Berdasarkan Input Biner." },
  { "en": "Apa Itu Dekoder BCD (Binary-Coded Decimal) Ke 7-Segmen?", "id": "Mengubah Kode BCD Menjadi Tampilan 7-Segmen." },
  { "en": "Apa Itu Enkoder (Encoder)?", "id": "Menghasilkan Kode Biner Berdasarkan Input Aktif." },
  { "en": "Apa Itu Enkoder Prioritas (Priority Encoder)?", "id": "Mengenkode Input Prioritas Tertinggi Yang Aktif." },
  { "en": "Apa Itu Multiplexer (MUX)?", "id": "Memilih Satu Dari Banyak Input." },
  { "en": "Apa Itu Demultiplexer (DEMUX)?", "id": "Mengarahkan Satu Input Ke Banyak Output." },
  { "en": "Apa Itu Komparator Digital?", "id": "Membandingkan Dua Bilangan Biner." },
  { "en": "Apa Itu Read-Only Memory (ROM)?", "id": "Memori Non-Volatile Yang Hanya Dapat Dibaca." },
  { "en": "Apa Itu Programmable Read-Only Memory (PROM)?", "id": "ROM Yang Dapat Diprogram Sekali." },
  { "en": "Apa Itu Erasable Programmable Read-Only Memory (EPROM)?", "id": "PROM Yang Dapat Dihapus Dengan Sinar UV." },
  { "en": "Apa Itu Electrically Erasable Programmable ROM (EEPROM)?", "id": "EPROM Yang Dihapus Secara Elektrik." },
  { "en": "Apa Itu Memori Flash?", "id": "Jenis EEPROM Yang Dihapus Dalam Blok." },
  { "en": "Apa Itu Antarmuka Memori?", "id": "Sirkuit Pengontrol Akses Ke Memori." },
  { "en": "Apa Itu Waktu Akses Memori?", "id": "Waktu Membaca Data Dari Memori." },
  { "en": "Apa Itu Siklus Waktu Memori?", "id": "Waktu Minimum Antara Dua Akses Memori." },
  { "en": "Apa Itu Logika Diode-Transistor (DTL)?", "id": "Keluarga Logika Awal Menggunakan Dioda, Transistor." },
  { "en": "Apa Itu Logika Resistor-Transistor (RTL)?", "id": "Keluarga Logika Awal Menggunakan Resistor, Transistor." },
  { "en": "Apa Itu High-Threshold Logic (HTL)?", "id": "Keluarga Logika Tahan Derau Tinggi." },
  { "en": "Apa Itu Integrated Injection Logic (IIL atau I²L)?", "id": "Keluarga Logika Bipolar Kepadatan Tinggi." },
  { "en": "Apa Itu Sel Baterai Primer?", "id": "Baterai Sekali Pakai, Tidak Dapat Diisi Ulang." },
  { "en": "Apa Itu Sel Baterai Sekunder?", "id": "Baterai Dapat Diisi Ulang." },
  { "en": "Sebutkan Contoh Baterai Primer?", "id": "Alkaline, Lithium, Seng-Karbon." },
  { "en": "Sebutkan Contoh Baterai Sekunder?", "id": "Li-ion, NiMH, Asam Timbal." },
  { "en": "Apa Itu Kepadatan Energi Baterai?", "id": "Energi Yang Disimpan Per Satuan Volume/Berat." },
  { "en": "Apa Itu Kepadatan Daya Baterai?", "id": "Kemampuan Baterai Memberikan Daya Tinggi." },
  { "en": "Apa Itu Umur Siklus Baterai?", "id": "Jumlah Siklus Pengisian-Pengosongan." },
  { "en": "Apa Itu Efek Memori?", "id": "Penurunan Kapasitas Baterai Ni-Cd/Ni-MH." },
  { "en": "Apa Itu Tingkat Pengosongan Diri (Self-Discharge)?", "id": "Kehilangan Kapasitas Baterai Saat Tidak Digunakan." },
  { "en": "Apa Itu Metode Pengisian Constant Current (CC)?", "id": "Mengisi Baterai Dengan Arus Konstan." },
  { "en": "Apa Itu Metode Pengisian Constant Voltage (CV)?", "id": "Mengisi Baterai Dengan Tegangan Konstan." },
  { "en": "Apa Itu Pengisian Tetes (Trickle Charging)?", "id": "Mengisi Arus Sangat Rendah Menjaga Muatan." },
  { "en": "Apa Itu Sistem Manajemen Baterai (BMS)?", "id": "Sirkuit Elektronik Pelindung, Pengelola Baterai." },
  { "en": "Apa Fungsi Penyeimbangan Sel (Cell Balancing)?", "id": "Menyamakan Tegangan Setiap Sel Dalam Paket." },
  { "en": "Apa Itu Baterai Asam Timbal (Lead-Acid)?", "id": "Jenis Baterai Isi Ulang Tertua." },
  { "en": "Apa Itu Baterai AGM (Absorbent Glass Mat)?", "id": "Jenis Baterai Asam Timbal Tersegel." },
  { "en": "Apa Itu Baterai Gel?", "id": "Jenis Baterai Asam Timbal Elektrolit Gel." },
  { "en": "Apa Itu Pelarian Termal (Thermal Runaway)?", "id": "Kondisi Overheating Baterai Tak Terkendali." },
  { "en": "Apa Itu Impedansi Internal Baterai?", "id": "Resistansi Efektif Internal Sebuah Baterai." },
  { "en": "Apa Itu Baterai Logam Cair?", "id": "Baterai Suhu Tinggi Potensial Skala Grid." },
  { "en": "Apa Itu Baterai Alir (Flow Battery)?", "id": "Baterai Energi Disimpan Dalam Cairan Elektrolit." },
  { "en": "Apa Itu Baterai Lithium-Sulfur?", "id": "Teknologi Baterai Baru Kepadatan Energi Tinggi." },
  { "en": "Apa Itu Baterai Solid-State?", "id": "Baterai Menggunakan Elektrolit Padat." },
  { "en": "Apa Keuntungan Baterai Solid-State?", "id": "Keamanan Lebih Baik, Kepadatan Energi Tinggi." },
  { "en": "Apa Itu Baterai Lithium Iron Phosphate (LFP)?", "id": "Jenis Baterai Li-ion Umur Siklus Panjang." },
  { "en": "Apa Itu Anoda Grafit?", "id": "Material Anoda Umum Baterai Lithium-ion." },
  { "en": "Apa Itu Katoda Oksida Logam Lithium?", "id": "Material Katoda Umum Baterai Lithium-ion." },
  { "en": "Apa Itu Elektrolit Organik?", "id": "Pelarut Digunakan Dalam Baterai Lithium-ion." },
  { "en": "Apa Itu Separator Baterai?", "id": "Membran Pencegah Hubung Singkat Anoda, Katoda." },
  { "en": "Apa Itu Kapasitor Lapisan Ganda Listrik (EDLC)?", "id": "Nama Lain Untuk Superkapasitor." },
  { "en": "Apa Beda Superkapasitor Dan Baterai?", "id": "Superkapasitor Punya Kepadatan Daya Lebih Tinggi." },
  { "en": "Apa Itu Pseudokapasitor?", "id": "Jenis Superkapasitor Menggunakan Reaksi Redoks." },
  { "en": "Apa Itu Kapasitor Hibrida?", "id": "Menggabungkan Mekanisme EDLC Dan Pseudokapasitor." },
  { "en": "Apa Itu Elektronika Daya?", "id": "Konversi Dan Kontrol Daya Listrik." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah Arus Bolak-Balik Menjadi Arus Searah." },
  { "en": "Apa Itu Inverter?", "id": "Mengubah Arus Searah Menjadi Arus Bolak-Balik." },
  { "en": "Apa Itu Perajang DC (DC Chopper)?", "id": "Mengubah Tegangan DC Ke Level Lain." },
  { "en": "Apa Itu Siklokonverter?", "id": "Mengubah Frekuensi AC Langsung Tanpa Tautan DC." },
  { "en": "Apa Itu Sakelar Elektronik Ideal?", "id": "Tidak Ada Kerugian, Beralih Seketika." },
  { "en": "Apa Itu Kerugian Konduksi?", "id": "Disipasi Daya Saat Sakelar ON." },
  { "en": "Apa Itu Kerugian Switching?", "id": "Disipasi Daya Selama Transisi ON-OFF." },
  { "en": "Apa Itu Switching Keras (Hard Switching)?", "id": "Sakelar Beralih Saat Tegangan, Arus Tinggi." },
  { "en": "Apa Itu Switching Lunak (Soft Switching)?", "id": "Sakelar Beralih Saat Tegangan, Arus Nol." },
  { "en": "Apa Tujuan Switching Lunak?", "id": "Mengurangi Kerugian Switching Dan EMI." },
  { "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Beralih Saat Tegangan Melintasi Sakelar Nol." },
  { "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Beralih Saat Arus Melalui Sakelar Nol." },
  { "en": "Apa Itu Konverter Resonan?", "id": "Menggunakan Rangkaian LC Untuk Mencapai Switching Lunak." },
  { "en": "Apa Itu Konverter Maju (Forward Converter)?", "id": "Topologi Konverter DC-DC Terisolasi." },
  { "en": "Apa Itu Konverter Jembatan Penuh (Full-Bridge)?", "id": "Topologi Konverter DC-DC Empat Sakelar." },
  { "en": "Apa Itu Konverter Setengah Jembatan (Half-Bridge)?", "id": "Topologi Konverter DC-DC Dua Sakelar." },
  { "en": "Apa Itu Konverter Push-Pull?", "id": "Topologi Konverter Menggunakan Trafo Center-tapped." },
  { "en": "Apa Itu Induktor Gandeng (Coupled Inductor)?", "id": "Dua Atau Lebih Lilitan Pada Inti Sama." },
  { "en": "Apa Itu Transformator Flyback?", "id": "Induktor Gandeng Penyimpan, Transfer Energi." },
  { "en": "Apa Itu Reset Inti Transformator?", "id": "Mencegah Saturasi Inti Dalam Konverter Maju." },
  { "en": "Apa Itu Sirkuit Pengapit (Clamping Circuit)?", "id": "Menggeser Level DC Sinyal Tanpa Mengubah Bentuknya." },
  { "en": "Apa Itu Sirkuit Pemotong (Clipping Circuit)?", "id": "Memotong Bagian Sinyal Di Atas Atau Bawah Level." },
  { "en": "Apa Nama Lain Sirkuit Pemotong?", "id": "Limiter Atau Slicer." },
  { "en": "Apa Itu Amplifier Diferensial?", "id": "Menguatkan Selisih Antara Dua Sinyal Input." },
  { "en": "Apa Itu Common-Mode Rejection Ratio (CMRR)?", "id": "Kemampuan Amplifier Menolak Sinyal Common-Mode." },
  { "en": "Apa Itu Sinyal Common-Mode?", "id": "Sinyal Yang Sama Muncul Di Kedua Input." },
  { "en": "Apa Itu Power Supply Rejection Ratio (PSRR)?", "id": "Kemampuan Sirkuit Menolak Derau Dari Catu Daya." },
  { "en": "Apa Itu Tegangan Ofset Input?", "id": "Tegangan DC Kecil Diperlukan Membuat Output Nol." },
  { "en": "Apa Itu Arus Bias Input?", "id": "Arus DC Kecil Mengalir Ke Terminal Input." },
  { "en": "Apa Itu Arus Ofset Input?", "id": "Selisih Antara Dua Arus Bias Input." },
  { "en": "Apa Itu Produk Gain-Bandwidth (GBWP)?", "id": "Produk Penguatan Dan Bandwidth Konstan." },
  { "en": "Apa Itu Laju Perubahan (Slew Rate)?", "id": "Laju Perubahan Maksimum Tegangan Output." },
  { "en": "Apa Itu Waktu Tunak (Settling Time)?", "id": "Waktu Diperlukan Output Untuk Stabil." },
  { "en": "Apa Itu Kebisingan Tegangan Input Ekuivalen?", "id": "Sumber Kebisingan Internal Dimodelkan Di Input." },
  { "en": "Apa Itu Rentang Dinamis Bebas Spurious (SFDR)?", "id": "Rasio Sinyal Terhadap Komponen Spurious Terkuat." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran Distorsi Harmonik Dalam Sinyal." },
  { "en": "Apa Itu Distorsi Intermodulasi (IMD)?", "id": "Terciptanya Frekuensi Baru Akibat Non-Linearitas." },
  { "en": "Apa Itu Titik Kompresi 1-dB?", "id": "Titik Daya Di Mana Penguatan Turun 1 dB." },
  { "en": "Apa Itu Osilator Jembatan Wien?", "id": "Osilator Penghasil Gelombang Sinus Distorsi Rendah." },
  { "en": "Apa Itu Osilator Pergeseran Fasa?", "id": "Osilator Menggunakan Jaringan RC Menciptakan Pergeseran Fasa." },
  { "en": "Apa Itu Osilator Colpitts?", "id": "Osilator LC Menggunakan Pembagi Tegangan Kapasitif." },
  { "en": "Apa Itu Osilator Hartley?", "id": "Osilator LC Menggunakan Pembagi Tegangan Induktif." },
  { "en": "Apa Itu Osilator Kristal?", "id": "Osilator Sangat Stabil Menggunakan Resonator Kristal." },
  { "en": "Apa Itu Efek Piezoelektrik?", "id": "Sifat Kristal Menghasilkan Tegangan Saat Ditekan." },
  { "en": "Apa Itu Kriteria Osilasi Barkhausen?", "id": "Syarat Terjadinya Osilasi Berkelanjutan." },
  { "en": "Apa Saja Dua Syarat Barkhausen?", "id": "Penguatan Loop Satu, Pergeseran Fasa 360 Derajat." },
  { "en": "Apa Itu Automatic Gain Control (AGC)?", "id": "Rangkaian Penstabil Amplitudo Output Secara Otomatis." },
  { "en": "Apa Itu Multivibrator Astabil?", "id": "Osilator Penghasil Gelombang Kotak (Tanpa Keadaan Stabil)." },
  { "en": "Apa Itu Multivibrator Monostabil?", "id": "Rangkaian Penghasil Pulsa Tunggal (Satu Keadaan Stabil)." },
  { "en": "Apa Nama Lain Multivibrator Monostabil?", "id": "One-Shot." },
  { "en": "Apa Itu Multivibrator Bistabil?", "id": "Rangkaian Dengan Dua Keadaan Stabil (Flip-Flop)." },
  { "en": "Apa Itu Rangkaian Schmitt Trigger?", "id": "Komparator Dengan Histeresis." },
  { "en": "Apa Tujuan Histeresis Dalam Schmitt Trigger?", "id": "Mencegah Transisi Palsu Akibat Input Berderau." },
  { "en": "Apa Itu Timer 555?", "id": "Sirkuit Terpadu Populer Untuk Pewaktuan, Osilasi." },
  { "en": "Apa Itu Mode Astabil Timer 555?", "id": "Digunakan Sebagai Osilator Gelombang Kotak." },
  { "en": "Apa Itu Mode Monostabil Timer 555?", "id": "Digunakan Sebagai Penghasil Pulsa Tunggal." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Kontrol Umpan Balik Sinkronisasi Fasa." },
  { "en": "Apa Komponen Utama PLL (Phase-Locked Loop)?", "id": "Detektor Fasa, Filter Loop, Osilator Terkendali." },
  { "en": "Apa Itu Detektor Fasa (Phase Detector)?", "id": "Menghasilkan Sinyal Proporsional Perbedaan Fasa." },
  { "en": "Apa Itu Voltage-Controlled Oscillator (VCO)?", "id": "Osilator Yang Frekuensinya Dikontrol Oleh Tegangan." },
  { "en": "Apa Itu Filter Loop (Loop Filter)?", "id": "Filter Low-Pass Menstabilkan Loop PLL." },
  { "en": "Apa Itu Rentang Tangkap (Capture Range) PLL?", "id": "Rentang Frekuensi PLL Dapat Memperoleh Kunci." },
  { "en": "Apa Itu Rentang Kunci (Lock Range) PLL?", "id": "Rentang Frekuensi PLL Dapat Mempertahankan Kunci." },
  { "en": "Apa Itu Sintesis Frekuensi (Frequency Synthesis)?", "id": "Menghasilkan Berbagai Frekuensi Dari Satu Referensi." },
  { "en": "Apa Itu Prescaler?", "id": "Pembagi Frekuensi Digunakan Dalam Sintesis Frekuensi." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Itu Filter Pasif?", "id": "Filter Hanya Menggunakan Komponen Pasif." },
  { "en": "Apa Keuntungan Filter Aktif?", "id": "Penguatan Sinyal, Tanpa Membutuhkan Induktor." },
  { "en": "Apa Itu Topologi Sallen-Key?", "id": "Desain Filter Aktif Populer." },
  { "en": "Apa Itu Topologi Multiple Feedback (MFB)?", "id": "Desain Filter Aktif Lainnya." },
  { "en": "Apa Itu Orde Filter?", "id": "Menentukan Ketajaman Roll-off Filter." },
  { "en": "Apa Itu Roll-Off Filter?", "id": "Kemiringan Kurva Respon Di Stopband." },
  { "en": "Apa Itu Respon Filter Butterworth?", "id": "Respon Passband Paling Datar (Maximally Flat)." },
  { "en": "Apa Itu Respon Filter Chebyshev?", "id": "Roll-off Lebih Tajam Dengan Riak Passband." },
  { "en": "Apa Itu Respon Filter Bessel?", "id": "Respon Fasa Paling Linear (Penundaan Grup Konstan)." },
  { "en": "Kapan Filter Bessel Penting?", "id": "Saat Menjaga Bentuk Gelombang Sangat Penting." },
  { "en": "Apa Itu Filter All-Pass?", "id": "Filter Mengubah Fasa Tanpa Mengubah Amplitudo." },
  { "en": "Apa Itu Equalizer?", "id": "Filter Untuk Menyesuaikan Respon Frekuensi." },
  { "en": "Apa Itu Equalizer Grafis?", "id": "Equalizer Dengan Slider Untuk Pita Frekuensi." },
  { "en": "Apa Itu Equalizer Parametrik?", "id": "Equalizer Dengan Kontrol Frekuensi, Bandwidth, Gain." },
  { "en": "Apa Itu Jaringan Crossover Audio?", "id": "Filter Memisahkan Frekuensi Untuk Speaker Berbeda." },
  { "en": "Apa Itu Tweeter?", "id": "Speaker Untuk Frekuensi Tinggi." },
  { "en": "Apa Itu Woofer?", "id": "Speaker Untuk Frekuensi Rendah." },
  { "en": "Apa Itu Subwoofer?", "id": "Speaker Khusus Untuk Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Mid-range Speaker?", "id": "Speaker Untuk Frekuensi Di Antara Woofer, Tweeter." },
  { "en": "Apa Itu Rangkaian Resonansi?", "id": "Rangkaian LC (Induktor-Kapasitor) Dengan Respon Frekuensi." },
  { "en": "Apa Itu Frekuensi Resonansi?", "id": "Frekuensi Di Mana Reaktansi Saling Menghilangkan." },
  { "en": "Apa Itu Faktor Kualitas (Q)?", "id": "Ukuran Selektivitas Rangkaian Resonansi." },
  { "en": "Apa Arti Faktor Q Tinggi?", "id": "Respon Sangat Tajam, Bandwidth Sempit." },
  { "en": "Apa Arti Faktor Q Rendah?", "id": "Respon Lebar, Bandwidth Lebar." },
  { "en": "Apa Itu Rangkaian Tangki (Tank Circuit)?", "id": "Nama Lain Untuk Rangkaian Resonansi Paralel." },
  { "en": "Apa Itu Impedansi Rangkaian Resonansi Seri?", "id": "Minimum (Hanya Resistansi) Pada Frekuensi Resonansi." },
  { "en": "Apa Itu Impedansi Rangkaian Resonansi Paralel?", "id": "Maksimum Pada Frekuensi Resonansi." },
  { "en": "Apa Itu Bandwidth Setengah Daya?", "id": "Rentang Frekuensi Di Mana Daya Setengah." },
  { "en": "Apa Itu Frekuensi Cutoff?", "id": "Frekuensi Di Mana Penguatan Turun 3 dB." },
  { "en": "Apa Itu Dekade (Decade)?", "id": "Perubahan Frekuensi Faktor Sepuluh." },
  { "en": "Apa Itu Oktaf (Octave)?", "id": "Perubahan Frekuensi Faktor Dua." },
  { "en": "Apa Itu Konverter Analog-ke-Digital (ADC)?", "id": "Mengubah Sinyal Analog Menjadi Nilai Digital." },
  { "en": "Apa Itu Konverter Digital-ke-Analog (DAC)?", "id": "Mengubah Nilai Digital Menjadi Sinyal Analog." },
  { "en": "Apa Itu Resolusi ADC/DAC?", "id": "Jumlah Bit Yang Digunakan." },
  { "en": "Apa Itu Laju Sampling?", "id": "Seberapa Sering Sinyal Analog Diukur." },
  { "en": "Apa Itu Teorema Sampling Nyquist?", "id": "Laju Sampling Harus Dua Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Aliasing?", "id": "Distorsi Akibat Laju Sampling Terlalu Rendah." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter Low-pass Sebelum ADC Mencegah Aliasing." },
  { "en": "Apa Itu Kesalahan Kuantisasi?", "id": "Error Akibat Proses Aproksimasi Kuantisasi." },
  { "en": "Apa Itu Arsitektur Flash ADC?", "id": "Konverter Analog-Ke-Digital Paling Cepat." },
  { "en": "Apa Itu Arsitektur SAR ADC?", "id": "Successive Approximation Register, Keseimbangan Baik." },
  { "en": "Apa Itu Arsitektur Sigma-Delta ADC?", "id": "Konverter Resolusi Sangat Tinggi." },
  { "en": "Apa Itu Arsitektur R-2R Ladder DAC?", "id": "Konverter Digital-Ke-Analog Umum, Presisi." },
  { "en": "Apa Itu Differential Non-Linearity (DNL)?", "id": "Ukuran Error Antara Langkah-langkah ADC/DAC." },
  { "en": "Apa Itu Integral Non-Linearity (INL)?", "id": "Ukuran Penyimpangan Total Dari Fungsi Ideal." },
  { "en": "Apa Itu Glitch DAC?", "id": "Lonjakan Energi Tak Diinginkan Selama Transisi." },
  { "en": "Apa Itu Rekonstruksi Sinyal?", "id": "Proses Mengubah Sinyal Sampel Kembali Analog." },
  { "en": "Apa Itu Filter Rekonstruksi?", "id": "Filter Low-pass Setelah DAC Menghaluskan Output." },
  { "en": "Apa Itu Zero-Order Hold (ZOH)?", "id": "Metode Rekonstruksi Sederhana Menghasilkan Tangga." },
  { "en": "Apa Itu Oversampling?", "id": "Sampling Pada Laju Jauh Di Atas Nyquist." },
  { "en": "Apa Keuntungan Oversampling?", "id": "Menyederhanakan Desain Filter Analog." },
  { "en": "Apa Itu Decimation?", "id": "Mengurangi Laju Sampel Sinyal Oversampled." },
  { "en": "Apa Itu Interpolasi?", "id": "Meningkatkan Laju Sampel Sinyal Digital." },
  { "en": "Apa Itu Sinyal Waktu Diskret?", "id": "Sinyal Yang Didefinisikan Hanya Pada Waktu Tertentu." },
  { "en": "Apa Itu Sinyal Waktu Kontinu?", "id": "Sinyal Yang Didefinisikan Untuk Semua Waktu." },
  { "en": "Apa Itu Pemrosesan Sinyal Digital (DSP)?", "id": "Memanipulasi Sinyal Menggunakan Teknik Digital." },
  { "en": "Apa Itu Prosesor Sinyal Digital (DSP)?", "id": "Mikroprosesor Khusus Untuk Aplikasi DSP." },
  { "en": "Apa Itu Arsitektur Harvard?", "id": "Arsitektur Prosesor Memori Program, Data Terpisah." },
  { "en": "Apa Itu Unit MAC (Multiply-Accumulate)?", "id": "Perangkat Keras Mempercepat Operasi DSP Umum." },
  { "en": "Apa Itu Pemrosesan Sinyal Waktu Nyata?", "id": "Memproses Sinyal Tanpa Penundaan Yang Terlihat." },
  { "en": "Apa Itu Fast Fourier Transform (FFT)?", "id": "Algoritma Efisien Menghitung Discrete Fourier Transform." },
  { "en": "Apa Itu Decimation-In-Time (DIT) FFT?", "id": "Jenis Algoritma FFT Memecah Input Waktu." },
  { "en": "Apa Itu Decimation-In-Frequency (DIF) FFT?", "id": "Jenis Algoritma FFT Memecah Output Frekuensi." },
  { "en": "Apa Itu Kupu-Kupu (Butterfly) Dalam FFT?", "id": "Operasi Komputasi Dasar Dalam Algoritma FFT." },
  { "en": "Apa Itu Bit-Reversal?", "id": "Pengurutan Ulang Diperlukan Dalam Algoritma FFT." },
  { "en": "Apa Itu Twiddle Factor?", "id": "Koefisien Eksponensial Kompleks Dalam FFT." },
  { "en": "Apa Itu Circular Convolution?", "id": "Konvolusi Periodik Untuk Sinyal Waktu Diskret." },
  { "en": "Bagaimana Menghindari Circular Convolution?", "id": "Menggunakan Teknik Overlap-Add Atau Overlap-Save." },
  { "en": "Apa Itu Windowing Dalam Analisis Spektral?", "id": "Mengurangi Efek Kebocoran Spektral (Spectral Leakage)." },
  { "en": "Sebutkan Contoh Fungsi Jendela?", "id": "Hanning, Hamming, Blackman, Dan Kaiser." },
  { "en": "Apa Itu Efek Picket-Fence?", "id": "Kesalahan Amplitudo Akibat Resolusi Frekuensi Terbatas." },
  { "en": "Apa Itu Zero Padding?", "id": "Menambahkan Nol Ke Sinyal Peningkatan Resolusi FFT." },
  { "en": "Apa Itu Filter FIR (Finite Impulse Response)?", "id": "Filter Digital Tanpa Umpan Balik." },
  { "en": "Apa Keuntungan Filter FIR (Finite Impulse Response)?", "id": "Selalu Stabil, Dapat Memiliki Fasa Linear." },
  { "en": "Apa Itu Filter IIR (Infinite Impulse Response)?", "id": "Filter Digital Dengan Umpan Balik." },
  { "en": "Apa Keuntungan Filter IIR (Infinite Impulse Response)?", "id": "Implementasi Lebih Efisien (Orde Rendah)." },
  { "en": "Apa Kerugian Filter IIR (Infinite Impulse Response)?", "id": "Bisa Tidak Stabil, Fasa Tidak Linear." },
  { "en": "Apa Itu Fasa Linear?", "id": "Semua Komponen Frekuensi Tertunda Waktu Sama." },
  { "en": "Mengapa Fasa Linear Penting?", "id": "Menjaga Bentuk Gelombang Tanpa Distorsi Fasa." },
  { "en": "Apa Itu Metode Jendela Desain Filter FIR?", "id": "Metode Desain Filter FIR Paling Sederhana." },
  { "en": "Apa Itu Metode Sampling Frekuensi Desain Filter FIR?", "id": "Menentukan Koefisien Dari Sampel Respon Frekuensi." },
  { "en": "Apa Itu Algoritma Parks-McClellan?", "id": "Merancang Filter FIR Optimal (Equiripple)." },
  { "en": "Apa Itu Transformasi Bilinear?", "id": "Mengubah Filter Analog Menjadi Filter Digital." },
  { "en": "Apa Itu Warping Frekuensi?", "id": "Efek Non-Linear Dalam Transformasi Bilinear." },
  { "en": "Apa Itu Metode Invariansi Impuls?", "id": "Metode Lain Konversi Filter Analog Digital." },
  { "en": "Apa Itu Struktur Direct Form I?", "id": "Implementasi Langsung Persamaan Beda Filter." },
  { "en": "Apa Itu Struktur Direct Form II?", "id": "Implementasi Lebih Efisien Memori Dari Direct Form I." },
  { "en": "Apa Itu Struktur Transposed Direct Form II?", "id": "Struktur Lain Dengan Sifat Numerik Baik." },
  { "en": "Apa Itu Struktur Cascade?", "id": "Mengimplementasikan Filter Orde Tinggi Bertingkat." },
  { "en": "Apa Itu Struktur Lattice?", "id": "Struktur Implementasi Filter Yang Kuat." },
  { "en": "Apa Itu Kuantisasi Koefisien?", "id": "Efek Merepresentasikan Koefisien Filter Bit Terbatas." },
  { "en": "Apa Itu Osilasi Batas (Limit Cycle)?", "id": "Osilasi Kecil Tak Diinginkan Filter IIR." },
  { "en": "Apa Itu Scaling Dalam Implementasi Filter?", "id": "Menyesuaikan Level Sinyal Mencegah Overflow." },
  { "en": "Apa Itu Arsitektur SIMD (Single Instruction, Multiple Data)?", "id": "Satu Instruksi Beroperasi Pada Banyak Data." },
  { "en": "Apa Itu Arsitektur VLIW (Very Long Instruction Word)?", "id": "Mengeksekusi Beberapa Instruksi Paralel." },
  { "en": "Apa Itu Pengalamatan Sirkular (Circular Addressing)?", "id": "Mode Pengalamatan Berguna Untuk Buffer Sirkular." },
  { "en": "Apa Itu Bit-Reversed Addressing?", "id": "Mode Pengalamatan Khusus Untuk Algoritma FFT." },
  { "en": "Apa Itu Perangkat Keras Loop Nol Overhead?", "id": "Memungkinkan Loop Berjalan Tanpa Penalti Siklus." },
  { "en": "Apa Itu Kodek (Codec)?", "id": "Perangkat/Program Untuk Mengkodekan, Mendekodekan Data." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Jumlah Bit Mewakili Data." },
  { "en": "Apa Itu Kompresi Lossless?", "id": "Data Asli Dapat Dipulihkan Sempurna." },
  { "en": "Apa Itu Kompresi Lossy?", "id": "Beberapa Informasi Dibuang Secara Permanen." },
  { "en": "Apa Itu Pengkodean Huffman?", "id": "Teknik Kompresi Lossless Berbasis Frekuensi." },
  { "en": "Apa Itu Pengkodean Run-Length (RLE)?", "id": "Mengkodekan Urutan Data Berulang." },
  { "en": "Apa Itu Discrete Cosine Transform (DCT)?", "id": "Transformasi Digunakan Dalam Kompresi JPEG, MPEG." },
  { "en": "Apa Itu Kuantisasi Dalam Kompresi?", "id": "Langkah Utama Kompresi Lossy." },
  { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Demodulasi?", "id": "Mengekstrak Informasi Dari Sinyal Termodulasi." },
  { "en": "Apa Itu Amplitude Modulation (AM)?", "id": "Amplitudo Pembawa Bervariasi Sesuai Sinyal." },
  { "en": "Apa Itu Frequency Modulation (FM)?", "id": "Frekuensi Pembawa Bervariasi Sesuai Sinyal." },
  { "en": "Apa Itu Phase Modulation (PM)?", "id": "Fasa Pembawa Bervariasi Sesuai Sinyal." },
  { "en": "Apa Itu Sideband Atas (Upper Sideband)?", "id": "Pita Frekuensi Di Atas Frekuensi Pembawa." },
  { "en": "Apa Itu Sideband Bawah (Lower Sideband)?", "id": "Pita Frekuensi Di Bawah Frekuensi Pembawa." },
  { "en": "Apa Itu Double-Sideband Suppressed-Carrier (DSB-SC)?", "id": "Modulasi AM Dengan Pembawa Ditekan." },
  { "en": "Apa Itu Single-Sideband (SSB) Modulation?", "id": "Hanya Mengirimkan Satu Sideband." },
  { "en": "Apa Keuntungan SSB (Single-Sideband) Modulation?", "id": "Menghemat Bandwidth Dan Daya Pancar." },
  { "en": "Apa Itu Indeks Modulasi?", "id": "Ukuran Seberapa Banyak Pembawa Dimodulasi." },
  { "en": "Apa Itu Deviasi Frekuensi Dalam FM?", "id": "Pergeseran Frekuensi Maksimum Dari Pembawa." },
  { "en": "Apa Itu Aturan Carson Untuk Bandwidth FM?", "id": "Estimasi Bandwidth Sinyal Frekuensi Termodulasi." },
  { "en": "Apa Itu Pre-emphasis Dan De-emphasis?", "id": "Teknik Meningkatkan SNR Dalam Transmisi FM." },
  { "en": "Apa Itu Modulasi Digital?", "id": "Memodulasi Sinyal Pembawa Dengan Data Digital." },
  { "en": "Apa Itu Amplitude-Shift Keying (ASK)?", "id": "Data Direpresentasikan Oleh Amplitudo Berbeda." },
  { "en": "Apa Itu Frequency-Shift Keying (FSK)?", "id": "Data Direpresentasikan Oleh Frekuensi Berbeda." },
  { "en": "Apa Itu Phase-Shift Keying (PSK)?", "id": "Data Direpresentasikan Oleh Fasa Berbeda." },
  { "en": "Apa Itu Binary Phase-Shift Keying (BPSK)?", "id": "Menggunakan Dua Fasa (0 Dan 180 Derajat)." },
  { "en": "Apa Itu Quadrature Phase-Shift Keying (QPSK)?", "id": "Menggunakan Empat Fasa Berbeda." },
  { "en": "Apa Itu Quadrature Amplitude Modulation (QAM)?", "id": "Menggabungkan Modulasi Amplitudo Dan Fasa." },
  { "en": "Apa Itu Diagram Konstelasi?", "id": "Representasi Grafis Skema Modulasi Digital." },
  { "en": "Apa Itu Bit Error Rate (BER)?", "id": "Rasio Jumlah Bit Error Terhadap Total." },
  { "en": "Apa Itu Symbol Error Rate (SER)?", "id": "Rasio Jumlah Simbol Error Terhadap Total." },
  { "en": "Apa Itu Signal-to-Noise Ratio (SNR)?", "id": "Rasio Kekuatan Sinyal Terhadap Kekuatan Derau." },
  { "en": "Apa Itu Eb/N0?", "id": "Rasio Energi Per Bit Terhadap Kepadatan Derau." },
  { "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambahkan Redundansi Untuk Mendeteksi, Mengoreksi Error." },
  { "en": "Apa Itu Interleaving?", "id": "Mengubah Urutan Data Melawan Burst Error." },
  { "en": "Apa Itu Matched Filter?", "id": "Penerima Optimal Mendeteksi Sinyal Dikenal." },
  { "en": "Apa Itu Intersymbol Interference (ISI)?", "id": "Simbol Berinterferensi Dengan Simbol Berikutnya." },
  { "en": "Apa Itu Kriteria Nyquist Untuk Zero ISI?", "id": "Syarat Respon Kanal Agar Tidak Ada ISI." },
  { "en": "Apa Itu Filter Raised-Cosine?", "id": "Filter Pembentuk Pulsa Memenuhi Kriteria Nyquist." },
  { "en": "Apa Itu Equalizer?", "id": "Filter Mengkompensasi Distorsi Kanal." },
  { "en": "Apa Itu Equalizer Adaptif?", "id": "Equalizer Dapat Menyesuaikan Diri Dengan Perubahan." },
  { "en": "Apa Itu Skema Akses Ganda (Multiple Access)?", "id": "Beberapa Pengguna Berbagi Kanal Komunikasi." },
  { "en": "Apa Itu FDMA (Frequency Division Multiple Access)?", "id": "Berbagi Kanal Berdasarkan Pembagian Frekuensi." },
  { "en": "Apa Itu TDMA (Time Division Multiple Access)?", "id": "Berbagi Kanal Berdasarkan Pembagian Waktu." },
  { "en": "Apa Itu CDMA (Code Division Multiple Access)?", "id": "Setiap Pengguna Diberi Kode Unik." },
  { "en": "Apa Itu SDMA (Space Division Multiple Access)?", "id": "Berbagi Kanal Berdasarkan Pemisahan Spasial." },
  { "en": "Apa Itu Orthogonal Frequency-Division Multiplexing (OFDM)?", "id": "Mengirim Data Melalui Banyak Sub-carrier." },
  { "en": "Apa Keuntungan OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Tahan Terhadap Fading Selektif Frekuensi." },
  { "en": "Apa Itu Cyclic Prefix?", "id": "Menambahkan Salinan Akhir Simbol Ke Depan." },
  { "en": "Apa Fungsi Cyclic Prefix?", "id": "Menghilangkan ISI Akibat Multipath." },
  { "en": "Apa Itu Teori Informasi?", "id": "Studi Matematis Tentang Kuantifikasi Informasi." },
  { "en": "Apa Itu Entropi Informasi?", "id": "Ukuran Rata-rata Ketidakpastian Suatu Sumber." },
  { "en": "Apa Itu Kapasitas Kanal Shannon?", "id": "Batas Atas Laju Data Bebas Error." },
  { "en": "Apa Itu Teorema Pengkodean Sumber?", "id": "Menetapkan Batas Kompresi Data Lossless." },
  { "en": "Apa Itu Teorema Pengkodean Kanal?", "id": "Menyatakan Komunikasi Andal Mungkin Dilakukan." },
  { "en": "Apa Itu Informasi Timbal Balik (Mutual Information)?", "id": "Ukuran Pengurangan Ketidakpastian." },
  { "en": "Apa Itu Divergensi Kullback-Leibler?", "id": "Ukuran Perbedaan Antara Dua Distribusi Probabilitas." },
  { "en": "Apa Itu Pengkodean Sumber (Source Coding)?", "id": "Mengubah Informasi Menjadi Urutan Bit." },
  { "en": "Apa Itu Efisiensi Kode?", "id": "Seberapa Dekat Panjang Kode Dengan Batas Entropi." },
  { "en": "Apa Itu Kode Awalan (Prefix Code)?", "id": "Tidak Ada Kata Kode Merupakan Awalan Lainnya." },
  { "en": "Apa Itu Kode Fano?", "id": "Metode Konstruksi Kode Awalan." },
  { "en": "Apa Itu Algoritma Lempel-Ziv-Welch (LZW)?", "id": "Algoritma Kompresi Lossless Berbasis Kamus." },
  { "en": "Apa Itu Antena Yagi-Uda?", "id": "Antena Berarah Terdiri Dari Beberapa Elemen." },
  { "en": "Apa Saja Elemen Antena Yagi-Uda?", "id": "Elemen Aktif, Reflektor, Dan Beberapa Direktor." },
  { "en": "Apa Fungsi Reflektor Pada Antena Yagi?", "id": "Memantulkan Gelombang Radio Ke Arah Depan." },
  { "en": "Apa Fungsi Direktor Pada Antena Yagi?", "id": "Mengarahkan Gelombang Radio Ke Arah Depan." },
  { "en": "Apa Itu Antena Log-Periodik?", "id": "Antena Broadband Dengan Elemen Berskala Logaritmik." },
  { "en": "Apa Itu Antena Fraktal?", "id": "Antena Menggunakan Bentuk Geometri Fraktal." },
  { "en": "Apa Keuntungan Antena Fraktal?", "id": "Ukuran Kompak Dan Kinerja Multi-band." },
  { "en": "Apa Itu Radome?", "id": "Selungkup Pelindung Antena Yang Transparan Radio." },
  { "en": "Apa Itu Metode Momen (MoM)?", "id": "Metode Numerik Untuk Analisis Elektromagnetik." },
  { "en": "Apa Itu Ruang Anechoic?", "id": "Ruangan Dirancang Menyerap Pantulan Gelombang Elektromagnetik." },
  { "en": "Untuk Apa Ruang Anechoic Digunakan?", "id": "Pengujian Antena Dan Pengukuran Emisi EMI." },
  { "en": "Apa Itu Near-Field Antenna Measurement?", "id": "Mengukur Medan Dekat Untuk Menentukan Medan Jauh." },
  { "en": "Apa Itu Compact Antenna Test Range (CATR)?", "id": "Fasilitas Pengujian Antena Dalam Ruangan." },
  { "en": "Apa Itu Sistem Tenaga Listrik Pesawat?", "id": "Menghasilkan Dan Mendistribusikan Listrik Di Pesawat." },
  { "en": "Berapa Frekuensi Sistem Listrik Pesawat?", "id": "Umumnya 400 Hz Untuk Menghemat Berat." },
  { "en": "Apa Itu Avionik?", "id": "Sistem Elektronik Yang Digunakan Pada Pesawat." },
  { "en": "Apa Itu Bus Data ARINC 429?", "id": "Standar Bus Data Umum Dalam Avionik." },
  { "en": "Apa Itu Bus Data MIL-STD-1553?", "id": "Standar Bus Data Militer Untuk Avionik." },
  { "en": "Apa Itu Automatic Dependent Surveillance-Broadcast (ADS-B)?", "id": "Teknologi Pengawasan Posisi Pesawat Otomatis." },
  { "en": "Apa Itu Transponder?", "id": "Pemancar-Penerima Radio Merespon Sinyal Interogasi." },
  { "en": "Apa Itu Traffic Collision Avoidance System (TCAS)?", "id": "Sistem Peringatan Dan Pencegahan Tabrakan Pesawat." },
  { "en": "Apa Itu Inertial Navigation System (INS)?", "id": "Sistem Navigasi Menggunakan Sensor Inersia." },
  { "en": "Apa Itu Flight Management System (FMS)?", "id": "Sistem Komputer Terpusat Mengelola Navigasi." },
  { "en": "Apa Itu Electronic Flight Instrument System (EFIS)?", "id": "Tampilan Kokpit Digital (Glass Cockpit)." },
  { "en": "Apa Itu Fly-By-Wire (FBW)?", "id": "Sistem Kontrol Pesawat Menggunakan Sinyal Elektronik." },
  { "en": "Apa Itu Actuator Elektro-Hidrostatik?", "id": "Aktuator Menggabungkan Motor Listrik, Pompa Hidrolik." },
  { "en": "Apa Itu Sistem Tenaga Listrik Kapal?", "id": "Menghasilkan Dan Mendistribusikan Listrik Di Kapal." },
  { "en": "Apa Itu Propulsi Listrik Kapal?", "id": "Menggunakan Motor Listrik Untuk Menggerakkan Baling-baling." },
  { "en": "Apa Itu Variable Frequency Drive (VFD) Kapal?", "id": "Mengontrol Kecepatan Motor Propulsi Listrik." },
  { "en": "Apa Itu Sistem Sinkronisasi Generator Kapal?", "id": "Memungkinkan Beberapa Generator Beroperasi Secara Paralel." },
  { "en": "Apa Itu Shore Power?", "id": "Suplai Listrik Dari Darat Ke Kapal." },
  { "en": "Apa Itu Elektronika Medis?", "id": "Aplikasi Teknik Elektro Dalam Bidang Medis." },
  { "en": "Apa Itu Sinyal Biopotensial?", "id": "Sinyal Listrik Dihasilkan Oleh Proses Biologis." },
  { "en": "Apa Itu Amplifier Biopotensial?", "id": "Amplifier Khusus Sinyal Fisiologis Lemah." },
  { "en": "Apa Karakteristik Penting Amplifier Biopotensial?", "id": "CMRR Tinggi, Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu Sirkuit Driven Right Leg (DRL)?", "id": "Mengurangi Interferensi Common-Mode Dalam Pengukuran ECG." },
  { "en": "Apa Itu Keselamatan Listrik Medis?", "id": "Standar Mencegah Sengatan Listrik Pasien." },
  { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus Kecil Mengalir Dari Peralatan Medis." },
  { "en": "Apa Itu Mikroshock?", "id": "Sengatan Listrik Arus Sangat Kecil." },
  { "en": "Apa Itu Makroshock?", "id": "Sengatan Listrik Melintasi Tubuh." },
  { "en": "Apa Itu Isolasi Pasien?", "id": "Mengisolasi Pasien Dari Bagian Listrik Peralatan." },
  { "en": "Apa Itu Pulse Oximeter?", "id": "Mengukur Saturasi Oksigen Dalam Darah." },
  { "en": "Bagaimana Cara Kerja Pulse Oximeter?", "id": "Mengukur Penyerapan Cahaya Merah, Inframerah." },
  { "en": "Apa Itu Electrosurgical Unit (ESU)?", "id": "Menggunakan Arus Frekuensi Tinggi Memotong, Membekukan." },
  { "en": "Apa Itu Bantalan Pemulih (Return Pad) ESU?", "id": "Menyediakan Jalur Balik Arus Frekuensi Tinggi." },
  { "en": "Apa Itu Stimulator Saraf Listrik Transkutan (TENS)?", "id": "Menggunakan Listrik Untuk Meredakan Nyeri." },
  { "en": "Apa Itu Tomografi Impedansi Listrik (EIT)?", "id": "Teknik Pencitraan Berdasarkan Sifat Listrik Jaringan." },
  { "en": "Apa Itu Telemetri Medis?", "id": "Mengirimkan Data Fisiologis Pasien Secara Nirkabel." },
  { "en": "Apa Itu Body Area Network (BAN)?", "id": "Jaringan Nirkabel Sensor Di Atau Dalam Tubuh." },
  { "en": "Apa Itu Pengisian Nirkabel (Wireless Charging)?", "id": "Mentransfer Daya Listrik Tanpa Kabel." },
  { "en": "Apa Prinsip Dasar Pengisian Nirkabel?", "id": "Induksi Elektromagnetik Atau Resonansi Magnetik." },
  { "en": "Apa Itu Standar Qi?", "id": "Standar Populer Untuk Pengisian Nirkabel Induktif." },
  { "en": "Apa Itu Wireless Power Transfer (WPT)?", "id": "Transfer Daya Nirkabel." },
  { "en": "Apa Itu Rectenna?", "id": "Antena Penyearah, Mengubah Gelombang Mikro Jadi DC." },
  { "en": "Apa Itu Energy Harvesting?", "id": "Memanen Energi Dari Sumber Sekitar." },
  { "en": "Sebutkan Sumber Energy Harvesting?", "id": "Getaran, Panas, Cahaya, Gelombang Radio." },
  { "en": "Apa Itu Pemanen Piezoelektrik?", "id": "Mengubah Energi Getaran Mekanis Menjadi Listrik." },
  { "en": "Apa Itu Pemanen Termoelektrik?", "id": "Mengubah Perbedaan Suhu Menjadi Listrik." },
  { "en": "Apa Itu Pemanen Fotovoltaik?", "id": "Mengubah Energi Cahaya Menjadi Listrik (Sel Surya)." },
  { "en": "Apa Itu Pemanen Energi RF?", "id": "Memanen Energi Dari Gelombang Radio Sekitar." },
  { "en": "Apa Itu Elektronika Konsumen?", "id": "Perangkat Elektronik Untuk Penggunaan Sehari-hari." },
  { "en": "Apa Itu Desain Untuk Uji Coba (DFT)?", "id": "Teknik Desain Memudahkan Pengujian IC." },
  { "en": "Apa Itu Boundary Scan (JTAG)?", "id": "Metode Pengujian Papan Sirkuit Tercetak." },
  { "en": "Apa Itu Automatic Test Equipment (ATE)?", "id": "Peralatan Otomatis Untuk Menguji Perangkat Elektronik." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Tertanam Dalam Perangkat Keras." },
  { "en": "Apa Itu Bootloader?", "id": "Program Pertama Dijalankan Saat Perangkat Dinyalakan." },
  { "en": "Apa Itu Over-The-Air (OTA) Update?", "id": "Memperbarui Firmware Perangkat Secara Nirkabel." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Dengan Fungsi Khusus." },
  { "en": "Apa Karakteristik Sistem Tertanam?", "id": "Dibatasi Sumber Daya, Waktu Nyata." },
  { "en": "Apa Itu Real-Time Operating System (RTOS)?", "id": "Sistem Operasi Untuk Aplikasi Waktu Nyata." },
  { "en": "Apa Itu Kriptografi Kurva Eliptik (ECC)?", "id": "Kriptografi Kunci Publik Berbasis Kurva Eliptik." },
  { "en": "Apa Keuntungan ECC (Elliptic Curve Cryptography)?", "id": "Ukuran Kunci Lebih Kecil Keamanan Sama." },
  { "en": "Apa Itu Advanced Encryption Standard (AES)?", "id": "Standar Enkripsi Simetris Yang Digunakan Luas." },
  { "en": "Apa Itu Secure Hash Algorithm (SHA)?", "id": "Keluarga Fungsi Hash Kriptografis." },
  { "en": "Apa Itu Hardware Security Module (HSM)?", "id": "Perangkat Keras Pelindung, Pengelola Kunci Digital." },
  { "en": "Apa Itu Trusted Platform Module (TPM)?", "id": "Chip Kriptoprosesor Aman Di Papan Induk." },
  { "en": "Apa Itu Secure Boot?", "id": "Memastikan Hanya Perangkat Lunak Terpercaya Dijalankan." },
  { "en": "Apa Itu Serangan Saluran Samping (Side-Channel Attack)?", "id": "Serangan Berdasarkan Informasi Fisik (Daya, Waktu)." },
  { "en": "Apa Itu Analisis Daya Diferensial (DPA)?", "id": "Jenis Serangan Saluran Samping." },
  { "en": "Apa Itu Physically Unclonable Function (PUF)?", "id": "Fungsi Keamanan Berbasis Variasi Fisik." },
  { "en": "Apa Itu True Random Number Generator (TRNG)?", "id": "Menghasilkan Angka Acak Dari Proses Fisik." },
  { "en": "Apa Beda TRNG Dan PRNG?", "id": "TRNG Benar-benar Acak, PRNG Semu-Acak." },
  { "en": "Apa Itu Memori Resistif (ReRAM)?", "id": "Jenis Memori Non-Volatile Baru." },
  { "en": "Apa Itu Memori Perubahan Fasa (PCM)?", "id": "Memori Berbasis Perubahan Fasa Material." },
  { "en": "Apa Itu Magnetoresistive RAM (MRAM)?", "id": "Memori Non-Volatile Menggunakan Efek Magnetoresistif." },
  { "en": "Apa Itu Ferroelectric RAM (FeRAM)?", "id": "Memori Non-Volatile Berbasis Material Feroelektrik." },
  { "en": "Apa Itu Komputasi Neuromorfik?", "id": "Arsitektur Komputer Meniru Otak Manusia." },
  { "en": "Apa Itu Spiking Neural Network (SNN)?", "id": "Jaringan Saraf Generasi Ketiga." },
  { "en": "Apa Itu Quantum Dot Display (QLED)?", "id": "Layar LCD Menggunakan Titik Kuantum." },
  { "en": "Apa Itu MicroLED?", "id": "Teknologi Tampilan Emisif Generasi Berikutnya." },
  { "en": "Apa Itu Augmented Reality (AR)?", "id": "Melapisi Informasi Digital Ke Dunia Nyata." },
  { "en": "Apa Itu Virtual Reality (VR)?", "id": "Menciptakan Pengalaman Imersif Lingkungan Virtual." },
  { "en": "Apa Itu Haptic Feedback?", "id": "Umpan Balik Berupa Sentuhan, Getaran." },
  { "en": "Apa Itu Robotika Lunak (Soft Robotics)?", "id": "Robot Dibuat Dari Material Lunak, Fleksibel." },
  { "en": "Apa Itu Material Cerdas (Smart Materials)?", "id": "Material Sifatnya Dapat Diubah Terkontrol." },
  { "en": "Apa Itu Metamaterial?", "id": "Material Rekayasa Sifat Tak Ditemukan Alam." },
  { "en": "Apa Itu Cloaking Device?", "id": "Aplikasi Hipotetis Metamaterial." },
  { "en": "Apa Itu Superkonduktivitas?", "id": "Hilangnya Hambatan Listrik Pada Suhu Rendah." },
  { "en": "Apa Itu Suhu Kritis (Tc)?", "id": "Suhu Di Bawahnya Material Menjadi Superkonduktor." },
  { "en": "Apa Itu Efek Meissner?", "id": "Penolakan Medan Magnet Oleh Superkonduktor." },
  { "en": "Apa Itu SQUID (Superconducting Quantum Interference Device)?", "id": "Magnetometer Paling Sensitif." },
  { "en": "Apa Itu Josephson Junction?", "id": "Elemen Dasar Sirkuit Superkonduktor." },
  { "en": "Apa Itu Fusi Nuklir?", "id": "Menggabungkan Inti Atom Menghasilkan Energi." },
  { "en": "Apa Itu Tokamak?", "id": "Desain Reaktor Fusi Magnetik Berbentuk Torus." },
  { "en": "Apa Itu Stellarator?", "id": "Desain Reaktor Fusi Magnetik Lainnya." },
  { "en": "Apa Itu Efek Fotokonduktif?", "id": "Penurunan Resistansi Akibat Paparan Cahaya." },
  { "en": "Apa Itu Efek Fotovoltaik?", "id": "Timbulnya Tegangan Akibat Paparan Cahaya." },
  { "en": "Apa Itu Sel Beban (Load Cell)?", "id": "Sensor Mengubah Gaya Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor Mengukur Regangan Mekanis Suatu Objek." },
  { "en": "Apa Prinsip Kerja Strain Gauge?", "id": "Perubahan Resistansi Akibat Perubahan Bentuk." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Rangkaian Mengukur Perubahan Resistansi Presisi." },
  { "en": "Apa Itu Amplifier Instrumentasi?", "id": "Amplifier Diferensial Dengan Presisi Sangat Tinggi." },
  { "en": "Apa Keuntungan Amplifier Instrumentasi?", "id": "CMRR Tinggi Dan Impedansi Input Tinggi." },
  { "en": "Apa Itu Arus Gelap (Dark Current)?", "id": "Arus Kecil Fotodetektor Tanpa Cahaya." },
  { "en": "Apa Itu Responsivitas Spektral?", "id": "Sensitivitas Detektor Terhadap Panjang Gelombang." },
  { "en": "Apa Itu Titik Setengah Daya?", "id": "Frekuensi Di Mana Daya Turun Setengah." },
  { "en": "Berapa Penurunan Desibel Pada Titik Setengah Daya?", "id": "Turun Tiga Desibel (-3 dB)." },
  { "en": "Apa Itu Konstanta Atenuasi (α)?", "id": "Bagian Riil Dari Konstanta Propagasi." },
  { "en": "Apa Itu Konstanta Fasa (β)?", "id": "Bagian Imajiner Dari Konstanta Propagasi." },
  { "en": "Apa Itu Atenuasi Sinyal?", "id": "Pelemahan Kekuatan Sinyal Seiring Jarak." },
  { "en": "Apa Itu Amplifier Distribusi?", "id": "Menguatkan Satu Sinyal Untuk Beberapa Output." },
  { "en": "Apa Itu Amplifier Umpan Arus (CFA)?", "id": "Jenis Op-Amp Kecepatan Sangat Tinggi." },
  { "en": "Apa Keuntungan CFA (Current-Feedback Amplifier)?", "id": "Bandwidth Hampir Tidak Tergantung Penguatan." },
  { "en": "Apa Itu Amplifier Transimpedansi (TIA)?", "id": "Mengubah Sinyal Input Arus Ke Tegangan." },
  { "en": "Di Mana TIA (Transimpedance Amplifier) Digunakan?", "id": "Pada Penerima Optik Dengan Fotodioda." },
  { "en": "Apa Itu Rangkaian Bootstrapping?", "id": "Teknik Umpan Balik Meningkatkan Impedansi Input." },
  { "en": "Apa Itu Pengikut Sumber (Source Follower)?", "id": "Amplifier MOSFET Penguatan Tegangan Hampir Satu." },
  { "en": "Apa Impedansi Output Pengikut Sumber?", "id": "Impedansi Output Sangat Rendah." },
  { "en": "Apa Itu Efek Badan (Body Effect) MOSFET?", "id": "Perubahan Tegangan Ambang Akibat Bias Substrat." },
  { "en": "Apa Itu Modulasi Panjang Kanal?", "id": "Variasi Arus Drain Akibat Perubahan Tegangan." },
  { "en": "Apa Itu Pasangan Diferensial?", "id": "Tahap Input Dasar Amplifier Operasional." },
  { "en": "Apa Fungsi Beban Aktif?", "id": "Menggantikan Resistor Beban Dengan Sumber Arus." },
  { "en": "Apa Keuntungan Beban Aktif?", "id": "Menghasilkan Penguatan Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Sirkuit Menyalin Arus Referensi Ke Jalur Lain." },
  { "en": "Apa Itu Cermin Arus Widlar?", "id": "Cermin Arus Menghasilkan Arus Output Kecil." },
  { "en": "Apa Itu Referensi Celah Pita (Bandgap Reference)?", "id": "Menghasilkan Tegangan Referensi Sangat Stabil Suhu." },
  { "en": "Apa Itu Regulator Low-Dropout (LDO)?", "id": "Regulator Linear Beda Tegangan Input-Output Kecil." },
  { "en": "Apa Itu Tegangan Dropout?", "id": "Perbedaan Tegangan Minimum Untuk Regulasi." },
  { "en": "Apa Itu Arus Diam (Quiescent Current)?", "id": "Arus Dikonsumsi Sirkuit Saat Tidak Berbeban." },
  { "en": "Apa Itu Thermal Shutdown?", "id": "Fitur Proteksi Mematikan Sirkuit Terlalu Panas." },
  { "en": "Apa Itu Undervoltage Lockout (UVLO)?", "id": "Mematikan Sirkuit Saat Tegangan Catu Rendah." },
  { "en": "Apa Itu Overvoltage Protection (OVP)?", "id": "Proteksi Terhadap Tegangan Catu Daya Berlebih." },
  { "en": "Apa Itu Sinyal Power Good (PG)?", "id": "Sinyal Output Menandakan Tegangan Stabil." },
  { "en": "Apa Itu Soft Start?", "id": "Menaikkan Tegangan Output Perlahan Saat Dinyalakan." },
  { "en": "Apa Tujuan Rangkaian Soft Start?", "id": "Membatasi Arus Mula (Inrush Current)." },
  { "en": "Apa Itu Rangkaian Crowbar?", "id": "Rangkaian Proteksi Tegangan Lebih Kasar." },
  { "en": "Apa Itu Polifuse (Sekring Dapat Direset)?", "id": "Sekring Polimer Dapat Reset Setelah Dingin." },
  { "en": "Apa Itu Dioda TVS (Transient Voltage Suppressor)?", "id": "Dioda Pelindung Dari Lonjakan Tegangan Cepat." },
  { "en": "Apa Itu Metal Oxide Varistor (MOV)?", "id": "Komponen Pasif Pelindung Lonjakan Tegangan." },
  { "en": "Apa Itu Gas Discharge Tube (GDT)?", "id": "Pelindung Lonjakan Kapasitas Energi Sangat Tinggi." },
  { "en": "Apa Itu Isolasi Optik (Optical Isolation)?", "id": "Mengisolasi Sirkuit Menggunakan Komponen Optocoupler." },
  { "en": "Apa Itu Current Transfer Ratio (CTR)?", "id": "Rasio Arus Output Terhadap Input Optocoupler." },
  { "en": "Apa Itu Isolasi Kapasitif?", "id": "Mengisolasi Sinyal Frekuensi Tinggi Menggunakan Kapasitor." },
  { "en": "Apa Itu Isolasi Magnetik?", "id": "Mengisolasi Sirkuit Menggunakan Transformator." },
  { "en": "Apa Itu Kebisingan Tembak (Shot Noise)?", "id": "Kebisingan Akibat Sifat Diskret Muatan Listrik." },
  { "en": "Apa Itu Kebisingan Termal (Johnson-Nyquist Noise)?", "id": "Kebisingan Akibat Gerakan Acak Elektron." },
  { "en": "Apa Itu Kebisingan 1/f (Flicker Noise)?", "id": "Kebisingan Frekuensi Rendah Dalam Perangkat Semikonduktor." },
  { "en": "Apa Itu Angka Kebisingan (Noise Figure)?", "id": "Ukuran Degradasi SNR Oleh Suatu Perangkat." },
  { "en": "Apa Itu Suhu Kebisingan (Noise Temperature)?", "id": "Ukuran Alternatif Dari Kebisingan Perangkat." },
  { "en": "Apa Itu Persamaan Friis Untuk Kebisingan?", "id": "Menghitung Total Kebisingan Sistem Bertingkat." },
  { "en": "Apa Itu Arsitektur Delta-Sigma?", "id": "Arsitektur ADC Dan DAC Resolusi Tinggi." },
  { "en": "Apa Itu Pembentukan Derau (Noise Shaping)?", "id": "Memindahkan Derau Kuantisasi Ke Frekuensi Lain." },
  { "en": "Apa Itu Desimasi Dalam ADC Delta-Sigma?", "id": "Mengurangi Laju Sampel Aliran Bit Tinggi." },
  { "en": "Apa Itu Penyearah Presisi?", "id": "Penyearah Menggunakan Op-Amp Untuk Sinyal Kecil." },
  { "en": "Apa Itu Detektor Puncak (Peak Detector)?", "id": "Sirkuit Menyimpan Nilai Tegangan Puncak Sinyal." },
  { "en": "Apa Itu Sinyal Jam (Clock Signal)?", "id": "Sinyal Periodik Sinkronisasi Operasi Sistem Digital." },
  { "en": "Apa Itu Jitter Jam (Clock Jitter)?", "id": "Variasi Waktu Tepi Jam Dari Idealnya." },
  { "en": "Apa Itu Skew Jam (Clock Skew)?", "id": "Perbedaan Waktu Tiba Jam Di Titik Berbeda." },
  { "en": "Apa Itu Jaringan Distribusi Jam (Clock Tree)?", "id": "Jaringan Mendistribusikan Jam Ke Seluruh Chip." },
  { "en": "Apa Itu Gated Clocking?", "id": "Menonaktifkan Jam Ke Blok Logika Tidak Aktif." },
  { "en": "Apa Tujuan Teknik Gated Clocking?", "id": "Mengurangi Konsumsi Daya Dinamis Sirkuit." },
  { "en": "Apa Itu Metastabilitas Dalam Flip-Flop?", "id": "Keadaan Tidak Stabil Antara Logika 0 Dan 1." },
  { "en": "Kapan Metastabilitas Dapat Terjadi?", "id": "Pelanggaran Waktu Setup Atau Hold." },
  { "en": "Apa Itu Sinkronizer (Synchronizer)?", "id": "Rangkaian Pencegah Metastabilitas Lintas Domain Jam." },
  { "en": "Apa Itu Sinkronizer Dua Flip-Flop?", "id": "Desain Sinkronizer Paling Dasar Dan Umum." },
  { "en": "Apa Itu Mean Time Between Failures (MTBF)?", "id": "Waktu Rata-rata Antara Kegagalan Sistem." },
  { "en": "Apa Itu Kode Gray?", "id": "Urutan Biner Dua Nilai Berurutan Beda 1 Bit." },
  { "en": "Mengapa Kode Gray Sangat Berguna?", "id": "Mengurangi Error Pada Encoder Posisi Mekanis." },
  { "en": "Apa Itu Antarmuka JTAG (Joint Test Action Group)?", "id": "Antarmuka Standar Pengujian, Pemrograman PCB." },
  { "en": "Apa Itu Boundary Scan?", "id": "Metode Pengujian Koneksi Antar IC." },
  { "en": "Apa Saja Sinyal Utama Antarmuka JTAG?", "id": "TCK, TMS, TDI, TDO." },
  { "en": "Apa Itu Daisy Chain JTAG?", "id": "Menghubungkan Beberapa Perangkat JTAG Secara Seri." },
  { "en": "Apa Itu Programmable Logic Device (PLD)?", "id": "IC Digital Fungsinya Dikonfigurasi Pengguna." },
  { "en": "Apa Itu Complex PLD (CPLD)?", "id": "PLD Lebih Kompleks Dari PAL/GAL." },
  { "en": "Apa Itu Look-Up Table (LUT)?", "id": "Elemen Logika Dasar Dapat Diprogram FPGA." },
  { "en": "Apa Itu Configurable Logic Block (CLB)?", "id": "Blok Fungsional Utama Dalam Arsitektur FPGA." },
  { "en": "Apa Itu Bitstream?", "id": "Berkas Konfigurasi Yang Dimuat Ke FPGA." },
  { "en": "Apa Itu Soft-Core Processor?", "id": "CPU Diimplementasikan Dalam Logika FPGA." },
  { "en": "Apa Itu Hard-Core Processor?", "id": "CPU Fisik Tertanam Dalam Chip FPGA." },
  { "en": "Apa Itu Bahasa Deskripsi Perangkat Keras (HDL)?", "id": "Bahasa Pemrograman Mendeskripsikan Sirkuit Digital." },
  { "en": "Sebutkan Contoh HDL (Hardware Description Language)?", "id": "VHDL Dan Verilog." },
  { "en": "Apa Itu Sintesis (Synthesis) Dalam Desain HDL?", "id": "Menerjemahkan Kode HDL Menjadi Implementasi Gerbang." },
  { "en": "Apa Itu Simulasi (Simulation) Dalam Desain HDL?", "id": "Memverifikasi Fungsionalitas Desain HDL." },
  { "en": "Apa Itu Testbench?", "id": "Kode HDL Untuk Mensimulasikan Dan Menguji Desain." },
  { "en": "Apa Itu Static Timing Analysis (STA)?", "id": "Menganalisis Waktu Penundaan Sirkuit Tanpa Simulasi." },
  { "en": "Apa Itu Jalur Kritis (Critical Path)?", "id": "Jalur Dengan Penundaan Terpanjang Dalam Sirkuit." },
  { "en": "Apa Itu Kendala Waktu (Timing Constraint)?", "id": "Persyaratan Waktu Ditetapkan Untuk Suatu Desain." },
  { "en": "Apa Itu Place and Route?", "id": "Proses Fisik Menempatkan, Menghubungkan Gerbang." },
  { "en": "Apa Itu Application-Specific Integrated Circuit (ASIC)?", "id": "Sirkuit Terpadu Didesain Untuk Aplikasi Khusus." },
  { "en": "Apa Beda Utama ASIC Dan FPGA?", "id": "ASIC Tetap, FPGA Dapat Diprogram Ulang." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Transfer Pola Ke Wafer Silikon." },
  { "en": "Apa Itu Wafer Silikon?", "id": "Kepingan Dasar Untuk Fabrikasi Sirkuit Terpadu." },
  { "en": "Apa Itu Die?", "id": "Satu Chip Individual Di Atas Wafer." },
  { "en": "Apa Itu Wire Bonding?", "id": "Menghubungkan Die Sirkuit Ke Pin Paket." },
  { "en": "Apa Itu Flip-Chip Assembly?", "id": "Metode Pemasangan Die Terbalik Langsung Ke Papan." },
  { "en": "Apa Itu Elektromagnetisme?", "id": "Studi Medan Listrik Dan Medan Magnet." },
  { "en": "Apa Itu Persamaan Maxwell?", "id": "Empat Persamaan Dasar Yang Mengatur Elektromagnetisme." },
  { "en": "Apa Itu Gelombang Elektromagnetik?", "id": "Getaran Medan Listrik, Magnet Yang Merambat." },
  { "en": "Berapa Kecepatan Cahaya Di Ruang Hampa?", "id": "Sekitar 299.792 Kilometer Per Detik." },
  { "en": "Apa Itu Vektor Poynting?", "id": "Mewakili Arah Dan Besaran Aliran Energi." },
  { "en": "Apa Itu Impedansi Intrinsik Ruang Hampa?", "id": "Rasio Medan Listrik Terhadap Medan Magnet." },
  { "en": "Apa Itu Polarisasi Gelombang?", "id": "Orientasi Osilasi Medan Listrik." },
  { "en": "Apa Itu Polarisasi Linear?", "id": "Medan Listrik Berosilasi Dalam Satu Garis Lurus." },
  { "en": "Apa Itu Polarisasi Sirkular?", "id": "Ujung Vektor Medan Listrik Membentuk Lingkaran." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Struktur Logam Pemandu Gelombang Frekuensi Tinggi." },
  { "en": "Apa Itu Frekuensi Cutoff Pemandu Gelombang?", "id": "Frekuensi Minimum Untuk Propagasi Gelombang." },
  { "en": "Apa Itu Mode Dominan?", "id": "Mode Propagasi Dengan Frekuensi Cutoff Terendah." },
  { "en": "Apa Itu Rongga Resonansi (Cavity Resonator)?", "id": "Pemandu Gelombang Tertutup Menyimpan Energi Elektromagnetik." },
  { "en": "Apa Itu Teori Saluran Transmisi?", "id": "Model Tegangan, Arus Sepanjang Saluran Listrik." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Impedansi Saluran Transmisi Tak Terhingga." },
  { "en": "Apa Itu Koefisien Refleksi?", "id": "Rasio Gelombang Pantul Terhadap Gelombang Datang." },
  { "en": "Apa Itu Voltage Standing Wave Ratio (VSWR)?", "id": "Rasio Tegangan Maksimum Terhadap Minimum." },
  { "en": "Apa Arti VSWR (Voltage Standing Wave Ratio) 1:1?", "id": "Tidak Ada Pantulan, Transfer Daya Maksimal." },
  { "en": "Apa Itu Penyesuaian Impedansi (Impedance Matching)?", "id": "Membuat Impedansi Beban Sama Dengan Sumber." },
  { "en": "Apa Itu Stub Tuning?", "id": "Menggunakan Potongan Saluran Untuk Penyesuaian Impedansi." },
  { "en": "Apa Itu Diagram Smith?", "id": "Alat Grafis Analisis Saluran Transmisi." },
  { "en": "Apa Itu Antena?", "id": "Mengubah Sinyal Listrik Menjadi Gelombang Elektromagnetik." },
  { "en": "Apa Itu Pola Radiasi Antena?", "id": "Pola Pancaran Energi Dari Antena." },
  { "en": "Apa Itu Penguatan Antena (Antenna Gain)?", "id": "Ukuran Kemampuan Antena Memfokuskan Energi." },
  { "en": "Apa Itu Direktivitas Antena?", "id": "Rasio Intensitas Radiasi Maksimum Terhadap Rata-rata." },
  { "en": "Apa Itu Efisiensi Antena?", "id": "Rasio Daya Teradiasi Terhadap Daya Input." },
  { "en": "Apa Itu Lebar Sorotan (Beamwidth) Antena?", "id": "Lebar Sudut Sorotan Utama Antena." },
  { "en": "Apa Itu Antena Isotropik?", "id": "Antena Hipotetis Memancar Sama Ke Segala Arah." },
  { "en": "Apa Itu Antena Dipol?", "id": "Antena Terdiri Dari Dua Konduktor Lurus." },
  { "en": "Apa Itu Antena Monopol?", "id": "Setengah Antena Dipol Dengan Bidang Tanah." },
  { "en": "Apa Itu Array Antena?", "id": "Sekelompok Antena Bekerja Sama." },
  { "en": "Apa Itu Phased Array?", "id": "Array Antena Arah Sorotannya Dapat Diatur." },
  { "en": "Apa Itu S-Parameter (Scattering Parameters)?", "id": "Mendeskripsikan Perilaku Jaringan RF Linier." },
  { "en": "Apa Itu S11 Dalam S-Parameter?", "id": "Koefisien Refleksi Input (Return Loss)." },
  { "en": "Apa Itu S21 Dalam S-Parameter?", "id": "Penguatan Maju (Insertion Gain/Loss)." },
  { "en": "Apa Itu S12 Dalam S-Parameter?", "id": "Isolasi Mundur (Reverse Isolation)." },
  { "en": "Apa Itu S22 Dalam S-Parameter?", "id": "Koefisien Refleksi Output." },
  { "en": "Apa Itu Vector Network Analyzer (VNA)?", "id": "Alat Ukur S-Parameter Suatu Perangkat." },
  { "en": "Apa Itu Amplifier Kebisingan Rendah (LNA)?", "id": "Tahap Awal Penerima Radio, Kebisingan Rendah." },
  { "en": "Apa Itu Amplifier Daya (PA)?", "id": "Tahap Akhir Pemancar Radio, Daya Tinggi." },
  { "en": "Apa Itu Mixer Frekuensi?", "id": "Mengalikan Dua Sinyal, Menggeser Frekuensi." },
  { "en": "Apa Itu Osilator?", "id": "Sirkuit Menghasilkan Sinyal Periodik." },
  { "en": "Apa Itu Filter Frekuensi Radio (RF)?", "id": "Melewatkan Atau Menolak Pita Frekuensi Tertentu." },
  { "en": "Apa Itu Penerima Superheterodyne?", "id": "Arsitektur Penerima Radio Paling Umum." },
  { "en": "Apa Itu Frekuensi Antara (IF)?", "id": "Frekuensi Tetap Hasil Konversi Turun." },
  { "en": "Apa Keuntungan Arsitektur Superheterodyne?", "id": "Selektivitas Dan Sensitivitas Tinggi." },
  { "en": "Apa Itu Penerima Homodyne (Direct-Conversion)?", "id": "Mengubah Sinyal RF Langsung Ke Baseband." },
  { "en": "Apa Itu Masalah DC Offset Dalam Penerima Homodyne?", "id": "Komponen DC Tak Diinginkan Pada Output." },
  { "en": "Apa Itu Ketidakseimbangan I/Q?", "id": "Ketidakcocokan Amplitudo, Fasa Jalur I/Q." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Sinyal Informasi Sebelum Modulasi." },
  { "en": "Apa Itu Sinyal Passband?", "id": "Sinyal Termodulasi Di Sekitar Frekuensi Pembawa." },
  { "en": "Apa Itu Model Rambatan Gelombang?", "id": "Memprediksi Kekuatan Sinyal Di Lokasi Berbeda." },
  { "en": "Apa Itu Kerugian Jalur Ruang Bebas?", "id": "Pelemahan Sinyal Ideal Tanpa Hambatan." },
  { "en": "Apa Itu Fading Multipath?", "id": "Fluktuasi Sinyal Akibat Interferensi Pantulan." },
  { "en": "Apa Itu Fading Rayleigh?", "id": "Model Fading Tanpa Jalur Langsung (NLOS)." },
  { "en": "Apa Itu Fading Rician?", "id": "Model Fading Dengan Jalur Langsung (LOS)." },
  { "en": "Apa Itu Pergeseran Doppler?", "id": "Perubahan Frekuensi Akibat Gerakan Relatif." },
  { "en": "Apa Itu Teknik Keanekaragaman (Diversity)?", "id": "Melawan Efek Fading Dengan Banyak Jalur." },
  { "en": "Apa Itu Keanekaragaman Ruang?", "id": "Menggunakan Beberapa Antena Terpisah." },
  { "en": "Apa Itu Keanekaragaman Frekuensi?", "id": "Mengirim Sinyal Pada Frekuensi Berbeda." },
  { "en": "Apa Itu Keanekaragaman Waktu?", "id": "Mengirim Ulang Sinyal Pada Waktu Berbeda." },
  { "en": "Apa Itu Sistem MIMO (Multiple-Input Multiple-Output)?", "id": "Banyak Antena Di Pemancar Dan Penerima." },
  { "en": "Apa Itu Multiplexing Spasial?", "id": "Mengirim Aliran Data Berbeda Secara Paralel." },
  { "en": "Apa Itu Beamforming?", "id": "Mengarahkan Energi Antena Ke Arah Tertentu." },
  { "en": "Apa Itu Radar?", "id": "Sistem Mendeteksi Objek Menggunakan Gelombang Radio." },
  { "en": "Apa Itu Radar Pulsa?", "id": "Radar Memancarkan Pulsa Energi Singkat." },
  { "en": "Apa Itu Radar Gelombang Kontinu (CW)?", "id": "Radar Memancarkan Energi Secara Terus-menerus." },
  { "en": "Apa Itu Radar Doppler?", "id": "Radar Mengukur Kecepatan Objek." },
  { "en": "Apa Itu Penampang Radar (RCS)?", "id": "Ukuran Seberapa Terdeteksi Objek Oleh Radar." },
  { "en": "Apa Itu Clutter Dalam Radar?", "id": "Gema Tak Diinginkan Dari Objek (Tanah)." },
  { "en": "Apa Itu Chaff?", "id": "Tindakan Balasan Radar Menyebarkan Material Reflektif." },
  { "en": "Apa Itu Radar Apertur Sintetik (SAR)?", "id": "Menghasilkan Gambar Resolusi Tinggi." },
  { "en": "Apa Itu Radar Penembus Tanah (GPR)?", "id": "Mencitrakan Objek Di Bawah Permukaan Tanah." },
  { "en": "Apa Itu Telekomunikasi?", "id": "Transmisi Informasi Jarak Jauh." },
  { "en": "Apa Itu Sistem Komunikasi Analog?", "id": "Mengirimkan Sinyal Informasi Analog." },
  { "en": "Apa Itu Sistem Komunikasi Digital?", "id": "Mengirimkan Informasi Dalam Bentuk Digital." },
  { "en": "Apa Itu Kanal Komunikasi?", "id": "Medium Fisik Tempat Sinyal Merambat." },
  { "en": "Apa Itu Derau (Noise)?", "id": "Sinyal Acak Tak Diinginkan." },
  { "en": "Apa Itu Interferensi?", "id": "Gangguan Dari Sinyal Lain Yang Tidak Diinginkan." },
  { "en": "Apa Itu Distorsi?", "id": "Perubahan Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Jaringan Telepon?", "id": "Jaringan Untuk Komunikasi Suara." },
  { "en": "Apa Itu Public Switched Telephone Network (PSTN)?", "id": "Jaringan Telepon Tetap Global." },
  { "en": "Apa Itu Switching Sirkuit?", "id": "Membangun Jalur Khusus Selama Durasi Panggilan." },
  { "en": "Apa Itu Switching Paket?", "id": "Data Dibagi Menjadi Paket, Dikirim Individual." },
  { "en": "Apa Itu Internet?", "id": "Jaringan Komputer Global Menggunakan Protokol TCP/IP." },
  { "en": "Apa Itu Protokol?", "id": "Satu Set Aturan Mengatur Komunikasi." },
  { "en": "Apa Itu Jaringan Seluler?", "id": "Jaringan Komunikasi Nirkabel Terdistribusi." },
  { "en": "Apa Itu Generasi Pertama (1G)?", "id": "Jaringan Seluler Analog." },
  { "en": "Apa Itu Generasi Kedua (2G)?", "id": "Jaringan Seluler Digital Pertama (GSM)." },
  { "en": "Apa Itu Generasi Ketiga (3G)?", "id": "Mendukung Kecepatan Data Lebih Tinggi." },
  { "en": "Apa Itu Generasi Keempat (4G/LTE)?", "id": "Jaringan Seluler Berbasis IP Kecepatan Tinggi." },
  { "en": "Apa Itu Generasi Kelima (5G)?", "id": "Jaringan Seluler Generasi Terbaru." },
  { "en": "Apa Fitur Utama Jaringan 5G?", "id": "Kecepatan Sangat Tinggi, Latensi Rendah." },
  { "en": "Apa Itu Komunikasi Satelit?", "id": "Menggunakan Satelit Sebagai Stasiun Relay." },
  { "en": "Apa Itu Orbit Geostasioner (GEO)?", "id": "Orbit Di Mana Satelit Tampak Diam." },
  { "en": "Apa Itu Orbit Bumi Rendah (LEO)?", "id": "Orbit Dekat Permukaan Bumi." },
  { "en": "Apa Itu Orbit Bumi Menengah (MEO)?", "id": "Orbit Antara LEO Dan GEO." },
  { "en": "Apa Itu Transponder Satelit?", "id": "Penerima, Penguat, Pemancar Di Satelit." },
  { "en": "Apa Itu Stasiun Bumi (Ground Station)?", "id": "Terminal Di Bumi Berkomunikasi Dengan Satelit." },
  { "en": "Apa Itu Jaringan Komputer?", "id": "Kumpulan Komputer Terhubung." },
  { "en": "Apa Itu Local Area Network (LAN)?", "id": "Jaringan Area Lokal (Gedung, Kantor)." },
  { "en": "Apa Itu Wide Area Network (WAN)?", "id": "Jaringan Area Luas (Antar Kota, Negara)." },
  { "en": "Apa Itu Ethernet?", "id": "Teknologi Jaringan Kabel Paling Umum." },
  { "en": "Apa Itu Wi-Fi?", "id": "Teknologi Jaringan Nirkabel Lokal." },
  { "en": "Apa Itu Router?", "id": "Perangkat Menghubungkan Jaringan Berbeda." },
  { "en": "Apa Itu Switch?", "id": "Perangkat Menghubungkan Komputer Dalam Satu LAN." },
  { "en": "Apa Itu Hub?", "id": "Perangkat Jaringan Sederhana, Kurang Cerdas." },
  { "en": "Apa Itu Firewall?", "id": "Sistem Keamanan Jaringan Mengontrol Lalu Lintas." },
  { "en": "Apa Itu Model OSI?", "id": "Model Konseptual Tujuh Lapisan Jaringan Komputer." },
  { "en": "Apa Itu Lapisan Fisik (Physical Layer)?", "id": "Lapisan 1: Transmisi Bit Mentah." },
  { "en": "Apa Itu Lapisan Tautan Data (Data Link Layer)?", "id": "Lapisan 2: Transfer Data Antar Node." },
  { "en": "Apa Itu Lapisan Jaringan (Network Layer)?", "id": "Lapisan 3: Pengalamatan Logis, Perutean Paket." },
  { "en": "Apa Itu Lapisan Transport (Transport Layer)?", "id": "Lapisan 4: Koneksi Ujung-Ke-Ujung Andal." },
  { "en": "Apa Itu Lapisan Sesi (Session Layer)?", "id": "Lapisan 5: Mengelola Dialog Antar Komputer." },
  { "en": "Apa Itu Lapisan Presentasi (Presentation Layer)?", "id": "Lapisan 6: Translasi, Enkripsi, Kompresi Data." },
  { "en": "Apa Itu Lapisan Aplikasi (Application Layer)?", "id": "Lapisan 7: Antarmuka Jaringan Untuk Aplikasi." },
  { "en": "Apa Itu Protokol TCP/IP?", "id": "Kumpulan Protokol Inti Dari Internet." },
  { "en": "Apa Itu Alamat IP (Internet Protocol)?", "id": "Pengenal Numerik Unik Perangkat Di Jaringan." },
  { "en": "Apa Itu Alamat IPv4?", "id": "Alamat IP Versi 4 (32-bit)." },
  { "en": "Apa Itu Alamat IPv6?", "id": "Alamat IP Versi 6 (128-bit)." },
  { "en": "Apa Itu Subnetting?", "id": "Membagi Jaringan IP Menjadi Sub-jaringan." },
  { "en": "Apa Itu CIDR (Classless Inter-Domain Routing)?", "id": "Metode Alokasi Alamat IP Modern." },
  { "en": "Apa Itu Alamat IP Privat?", "id": "Alamat IP Digunakan Dalam Jaringan Lokal." },
  { "en": "Apa Itu NAT (Network Address Translation)?", "id": "Mengubah Alamat IP Privat Ke Publik." },
  { "en": "Apa Itu Protokol TCP?", "id": "Protokol Transport Andal, Berorientasi Koneksi." },
  { "en": "Apa Itu Protokol UDP?", "id": "Protokol Transport Cepat, Tanpa Koneksi." },
  { "en": "Apa Itu Port Jaringan?", "id": "Pengenal Aplikasi Dalam Komunikasi Jaringan." },
  { "en": "Apa Itu Socket?", "id": "Kombinasi Alamat IP Dan Nomor Port." },
  { "en": "Apa Itu DNS (Domain Name System)?", "id": "Menerjemahkan Nama Domain Ke Alamat IP." },
  { "en": "Apa Itu DHCP (Dynamic Host Configuration Protocol)?", "id": "Memberikan Konfigurasi IP Otomatis." },
  { "en": "Apa Itu Alamat MAC?", "id": "Alamat Fisik Unik Perangkat Keras Jaringan." },
  { "en": "Apa Itu ARP (Address Resolution Protocol)?", "id": "Memetakan Alamat IP Ke Alamat MAC." },
  { "en": "Apa Itu ICMP (Internet Control Message Protocol)?", "id": "Protokol Untuk Pesan Kesalahan, Kontrol Jaringan." },
  { "en": "Apa Kegunaan Perintah Ping?", "id": "Menguji Konektivitas Menggunakan Pesan ICMP." },
  { "en": "Apa Itu Perutean (Routing)?", "id": "Proses Memilih Jalur Paket Jaringan." },
  { "en": "Apa Itu Tabel Perutean?", "id": "Tabel Informasi Jalur Ke Tujuan Jaringan." },
  { "en": "Apa Itu Perutean Statis?", "id": "Jalur Dikonfigurasi Secara Manual." },
  { "en": "Apa Itu Perutean Dinamis?", "id": "Router Saling Bertukar Informasi Perutean." },
  { "en": "Apa Itu Protokol Perutean?", "id": "Aturan Yang Digunakan Router Berbagi Informasi." },
  { "en": "Apa Itu RIP (Routing Information Protocol)?", "id": "Protokol Perutean Distance-Vector Sederhana." },
  { "en": "Apa Itu OSPF (Open Shortest Path First)?", "id": "Protokol Perutean Link-State Internal." },
  { "en": "Apa Itu BGP (Border Gateway Protocol)?", "id": "Protokol Perutean Inti Internet." },
  { "en": "Apa Itu Autonomous System (AS)?", "id": "Sekelompok Jaringan Di Bawah Administrasi Tunggal." },
  { "en": "Apa Itu HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Untuk Mengakses World Wide Web." },
  { "en": "Apa Itu HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Versi Aman HTTP Menggunakan SSL/TLS." },
  { "en": "Apa Itu FTP (File Transfer Protocol)?", "id": "Protokol Untuk Transfer Berkas." },
  { "en": "Apa Itu SMTP (Simple Mail Transfer Protocol)?", "id": "Protokol Untuk Mengirim Email." },
  { "en": "Apa Itu POP3 (Post Office Protocol 3)?", "id": "Protokol Untuk Menerima Email." },
  { "en": "Apa Itu IMAP (Internet Message Access Protocol)?", "id": "Protokol Lain Untuk Menerima Email." },
  { "en": "Apa Itu Telnet?", "id": "Protokol Terminal Jarak Jauh (Tidak Aman)." },
  { "en": "Apa Itu SSH (Secure Shell)?", "id": "Protokol Aman Untuk Akses Jarak Jauh." },
  { "en": "Apa Itu SNMP (Simple Network Management Protocol)?", "id": "Protokol Untuk Manajemen Jaringan." },
  { "en": "Apa Itu Kriptografi?", "id": "Ilmu Mengamankan Komunikasi." },
  { "en": "Apa Itu Enkripsi Simetris?", "id": "Menggunakan Kunci Sama Untuk Enkripsi, Dekripsi." },
  { "en": "Apa Itu Enkripsi Asimetris?", "id": "Menggunakan Pasangan Kunci Publik, Privat." },
  { "en": "Apa Itu Fungsi Hash?", "id": "Membuat Sidik Jari Digital Ukuran Tetap." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Memastikan Keaslian Dan Integritas Pesan." },
  { "en": "Apa Itu Sertifikat Digital?", "id": "Mengikat Kunci Publik Dengan Suatu Identitas." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Membuat Terowongan Aman Melalui Jaringan Publik." },
  { "en": "Apa Itu Kontrol PID?", "id": "Metode Kontrol Umpan Balik (Proporsional-Integral-Derivatif)." },
  { "en": "Apa Itu Error Dalam Kontrol PID?", "id": "Perbedaan Antara Setpoint Dan Nilai Proses." },
  { "en": "Apa Itu Aksi Proporsional?", "id": "Output Proporsional Terhadap Error Saat Ini." },
  { "en": "Apa Itu Aksi Integral?", "id": "Mengakumulasi Error Masa Lalu." },
  { "en": "Apa Itu Aksi Derivatif?", "id": "Berdasarkan Laju Perubahan Error." },
  { "en": "Apa Fungsi Aksi Integral?", "id": "Menghilangkan Error Keadaan Tunak." },
  { "en": "Apa Fungsi Aksi Derivatif?", "id": "Mengurangi Lewatan Dan Osilasi." },
  { "en": "Apa Itu Penalaan PID (PID Tuning)?", "id": "Proses Menentukan Nilai Parameter PID." },
  { "en": "Apa Itu Metode Ziegler-Nichols?", "id": "Metode Heuristik Untuk Penalaan PID." },
  { "en": "Apa Itu Kontrol Kaskade?", "id": "Menggunakan Dua Kontroler PID Bertingkat." },
  { "en": "Apa Itu Kontrol Umpan Maju (Feedforward)?", "id": "Mengantisipasi Gangguan Sebelum Mempengaruhi Sistem." },
  { "en": "Apa Itu Dead Time (Waktu Mati)?", "id": "Penundaan Antara Aksi Kontrol Dan Respon." },
  { "en": "Apa Itu Otomasi?", "id": "Penggunaan Teknologi Mengontrol Proses." },
  { "en": "Apa Itu Sensor?", "id": "Mengubah Besaran Fisik Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Aktuator?", "id": "Mengubah Sinyal Kontrol Menjadi Aksi Fisik." },
  { "en": "Apa Itu Kontrol Loop Terbuka?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Kontrol Loop Tertutup?", "id": "Sistem Kontrol Menggunakan Umpan Balik." },
  { "en": "Apa Keuntungan Kontrol Loop Tertutup?", "id": "Lebih Akurat Dan Tahan Gangguan." },
  { "en": "Apa Itu Pemrosesan Sinyal?", "id": "Analisis Dan Manipulasi Sinyal." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Dalam Waktu Dan Amplitudo." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskret Dalam Waktu Dan Amplitudo." },
  { "en": "Apa Itu Konversi Analog-ke-Digital (ADC)?", "id": "Mengubah Sinyal Analog Menjadi Digital." },
  { "en": "Apa Itu Konversi Digital-ke-Analog (DAC)?", "id": "Mengubah Sinyal Digital Menjadi Analog." },
  { "en": "Apa Itu Laju Pencuplikan (Sampling Rate)?", "id": "Jumlah Sampel Diambil Per Detik." },
  { "en": "Apa Itu Frekuensi Nyquist?", "id": "Setengah Dari Laju Pencuplikan." },
  { "en": "Apa Itu Aliasing?", "id": "Distorsi Akibat Laju Pencuplikan Kurang." },
  { "en": "Apa Itu Kuantisasi?", "id": "Proses Memetakan Nilai Ke Level Diskret." },
  { "en": "Apa Itu Kesalahan Kuantisasi?", "id": "Error Akibat Proses Kuantisasi." },
  { "en": "Apa Itu Transformasi Fourier?", "id": "Menguraikan Sinyal Menjadi Komponen Frekuensinya." },
  { "en": "Apa Itu Spektrum Frekuensi?", "id": "Representasi Amplitudo Sinyal Terhadap Frekuensi." },
  { "en": "Apa Itu Filter Digital?", "id": "Memodifikasi Sinyal Digital Berdasarkan Frekuensi." },
  { "en": "Apa Itu Filter Low-Pass?", "id": "Melewatkan Frekuensi Rendah, Meredam Frekuensi Tinggi." },
  { "en": "Apa Itu Filter High-Pass?", "id": "Melewatkan Frekuensi Tinggi, Meredam Frekuensi Rendah." },
  { "en": "Apa Itu Filter Band-Pass?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Band-Stop?", "id": "Meredam Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Untuk Menerapkan Filter." },
  { "en": "Apa Itu Respon Impuls?", "id": "Output Filter Terhadap Input Impuls." },
  { "en": "Apa Itu Filter FIR?", "id": "Finite Impulse Response." },
  { "en": "Apa Itu Filter IIR?", "id": "Infinite Impulse Response." },
  { "en": "Apa Itu Sistem Tenaga Listrik?", "id": "Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Pembangkitan (Generation)?", "id": "Proses Mengubah Bentuk Energi Lain Listrik." },
  { "en": "Apa Itu Transmisi (Transmission)?", "id": "Penyaluran Listrik Jarak Jauh Tegangan Tinggi." },
  { "en": "Apa Itu Distribusi (Distribution)?", "id": "Penyaluran Listrik Ke Pelanggan Akhir." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Mengubah Level Tegangan Listrik." },
  { "en": "Apa Itu Transformator Daya?", "id": "Mengubah Level Tegangan Dalam Sistem AC." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Memutuskan Sirkuit Secara Otomatis Saat Gangguan." },
  { "en": "Apa Itu Jaringan Listrik (Grid)?", "id": "Jaringan Interkoneksi Untuk Penyaluran Listrik." },
  { "en": "Apa Itu Beban Listrik (Load)?", "id": "Perangkat Yang Mengkonsumsi Energi Listrik." },
  { "en": "Apa Itu Faktor Daya?", "id": "Rasio Daya Nyata Terhadap Daya Semu." },
  { "en": "Mengapa Faktor Daya Penting?", "id": "Faktor Daya Rendah Menyebabkan Kerugian Lebih Besar." },
  { "en": "Apa Itu Koreksi Faktor Daya?", "id": "Memasang Kapasitor Untuk Meningkatkan Faktor Daya." },
  { "en": "Apa Itu Energi Terbarukan?", "id": "Energi Dari Sumber Daya Alam Berkelanjutan." },
  { "en": "Sebutkan Contoh Energi Terbarukan?", "id": "Surya, Angin, Air, Panas Bumi." },
  { "en": "Apa Itu Sistem Tenaga Listrik?", "id": "Pembangkitan, Transmisi, Dan Distribusi Listrik." },
  { "en": "Apa Itu Jaringan Listrik Cerdas (Smart Grid)?", "id": "Jaringan Listrik Dengan Komunikasi Dua Arah." },
  { "en": "Apa Tujuan Utama Dari Smart Grid?", "id": "Meningkatkan Efisiensi, Keandalan, Dan Keamanan." },
  { "en": "Apa Itu Advanced Metering Infrastructure (AMI)?", "id": "Infrastruktur Pengukuran Canggih Untuk Smart Grid." },
  { "en": "Apa Fungsi Smart Meter?", "id": "Mencatat Konsumsi Energi Secara Real-Time." },
  { "en": "Apa Itu Demand Response (DR)?", "id": "Mekanisme Penyesuaian Penggunaan Listrik Konsumen." },
  { "en": "Apa Itu Distributed Generation (DG)?", "id": "Pembangkit Listrik Tersebar Skala Kecil." },
  { "en": "Sebutkan Contoh Distributed Generation (DG)?", "id": "Panel Surya Atap Dan Turbin Angin." },
  { "en": "Apa Itu Microgrid?", "id": "Jaringan Listrik Lokal Yang Dapat Beroperasi Mandiri." },
  { "en": "Apa Itu Mode Islanding?", "id": "Kondisi Microgrid Beroperasi Terpisah Dari Jaringan." },
  { "en": "Apa Itu Phasor Measurement Unit (PMU)?", "id": "Mengukur Fasor Tegangan, Arus Sinkron Waktu." },
  { "en": "Apa Fungsi PMU (Phasor Measurement Unit) Dalam Jaringan?", "id": "Pemantauan Jaringan Area Luas (WAMS)." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Listrik Menyuplai Daya Kembali Ke Jaringan." },
  { "en": "Apa Itu Home Area Network (HAN)?", "id": "Jaringan Komunikasi Perangkat Cerdas Di Rumah." },
  { "en": "Apa Itu Elektronika Daya?", "id": "Aplikasi Elektronika Solid-State Untuk Kontrol Daya." },
  { "en": "Apa Itu Penyearah Terkendali?", "id": "Penyearah Tegangan Outputnya Dapat Diatur." },
  { "en": "Apa Itu Inverter Multilevel?", "id": "Inverter Menghasilkan Bentuk Gelombang Bertingkat." },
  { "en": "Apa Keuntungan Inverter Multilevel?", "id": "Menghasilkan Output Dengan Distorsi Rendah." },
  { "en": "Apa Itu Space Vector Modulation (SVM)?", "id": "Teknik PWM (Pulse Width Modulation) Canggih Untuk Inverter." },
  { "en": "Apa Itu Kontrol Mode Arus?", "id": "Loop Umpan Balik Menggunakan Arus Induktor." },
  { "en": "Apa Itu Kontrol Mode Tegangan?", "id": "Loop Umpan Balik Menggunakan Tegangan Output." },
  { "en": "Apa Itu Sakelar Elektronik Lunak (Soft Switching)?", "id": "Mengurangi Kerugian Daya Selama Proses Switching." },
  { "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Switching Terjadi Saat Tegangan Sakelar Nol." },
  { "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Switching Terjadi Saat Arus Sakelar Nol." },
  { "en": "Apa Itu Konverter Resonan?", "id": "Menggunakan Rangkaian LC Untuk Mencapai Soft Switching." },
  { "en": "Apa Itu Power Factor Correction (PFC)?", "id": "Membuat Beban Elektronik Seperti Resistor." },
  { "en": "Apa Tujuan Power Factor Correction (PFC)?", "id": "Mengurangi Arus Harmonik Dan Beban Jaringan." },
  { "en": "Apa Itu Boost Converter Aktif PFC?", "id": "Topologi Paling Umum Untuk PFC Aktif." },
  { "en": "Apa Itu Material Semikonduktor Celah Pita Lebar?", "id": "Contoh: SiC (Silicon Carbide), GaN (Gallium Nitride)." },
  { "en": "Apa Keuntungan Semikonduktor Celah Pita Lebar?", "id": "Operasi Suhu, Tegangan, Frekuensi Tinggi." },
  { "en": "Apa Itu Bidang Elektrik?", "id": "Gaya Per Satuan Muatan." },
  { "en": "Apa Itu Bidang Magnetik?", "id": "Medan Dihasilkan Oleh Arus Listrik." },
  { "en": "Apa Itu Hukum Ampere?", "id": "Menghubungkan Arus Dengan Sirkulasi Medan Magnet." },
  { "en": "Apa Itu Hukum Induksi Faraday?", "id": "Perubahan Fluks Magnet Menginduksi GGL." },
  { "en": "Apa Itu Hukum Lenz?", "id": "Arus Induksi Melawan Perubahan Yang Menghasilkannya." },
  { "en": "Apa Itu Arus Perpindahan Maxwell?", "id": "Arus Akibat Perubahan Medan Listrik." },
  { "en": "Apa Itu Teorema Poynting?", "id": "Mendeskripsikan Aliran Energi Dalam Medan Elektromagnetik." },
  { "en": "Apa Itu Keadaan Tunak Sinusoidal?", "id": "Analisis Rangkaian Dengan Sumber Sinusoidal." },
  { "en": "Apa Itu Fasor (Phasor)?", "id": "Representasi Bilangan Kompleks Sinyal Sinusoidal." },
  { "en": "Apa Itu Impedansi Kompleks?", "id": "Hambatan Total Rangkaian AC (R + jX)." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Bagian Imajiner Dari Impedansi." },
  { "en": "Apa Itu Admitansi (Admittance)?", "id": "Kebalikan Dari Impedansi." },
  { "en": "Apa Itu Daya Kompleks?", "id": "Representasi Vektor Daya Nyata, Reaktif." },
  { "en": "Apa Itu Daya Nyata (P)?", "id": "Daya Rata-rata Yang Dikonsumsi Beban." },
  { "en": "Apa Itu Daya Reaktif (Q)?", "id": "Daya Bolak-balik Antara Sumber, Beban." },
  { "en": "Apa Itu Daya Semu (S)?", "id": "Besaran Vektor Dari Daya Kompleks." },
  { "en": "Apa Itu Segitiga Daya?", "id": "Hubungan Geometris Antara P, Q, S." },
  { "en": "Apa Itu Resonansi Dalam Rangkaian RLC?", "id": "Reaktansi Induktif Dan Kapasitif Saling Menghilangkan." },
  { "en": "Apa Itu Sistem Orde Pertama?", "id": "Sistem Dengan Satu Komponen Penyimpan Energi." },
  { "en": "Apa Itu Sistem Orde Kedua?", "id": "Sistem Dengan Dua Komponen Penyimpan Energi." },
  { "en": "Apa Itu Respon Alami (Natural Response)?", "id": "Perilaku Sistem Tanpa Input Eksternal." },
  { "en": "Apa Itu Respon Paksa (Forced Response)?", "id": "Perilaku Sistem Akibat Input Eksternal." },
  { "en": "Apa Itu Konstanta Waktu?", "id": "Ukuran Seberapa Cepat Sistem Merespon." },
  { "en": "Apa Itu Faktor Redaman (Damping Factor)?", "id": "Menentukan Sifat Osilasi Respon Sistem." },
  { "en": "Apa Itu Respon Kurang Teredam (Underdamped)?", "id": "Respon Yang Berosilasi Sebelum Stabil." },
  { "en": "Apa Itu Respon Teredam Kritis (Critically Damped)?", "id": "Respon Tercepat Menuju Kestabilan Tanpa Osilasi." },
  { "en": "Apa Itu Respon Lebih Teredam (Overdamped)?", "id": "Respon Lambat Tanpa Osilasi." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Mengubah Persamaan Diferensial Menjadi Aljabar." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Output Terhadap Input Domain Laplace." },
  { "en": "Apa Itu Kutub (Pole) Fungsi Transfer?", "id": "Akar Penyebut Yang Menentukan Respon Alami." },
  { "en": "Apa Itu Nol (Zero) Fungsi Transfer?", "id": "Akar Pembilang Yang Mempengaruhi Bentuk Respon." },
  { "en": "Apa Itu Plot Bode?", "id": "Grafik Penguatan Dan Fasa Vs Frekuensi." },
  { "en": "Apa Itu Margin Penguatan (Gain Margin)?", "id": "Ukuran Stabilitas Relatif Terkait Penguatan." },
  { "en": "Apa Itu Margin Fasa (Phase Margin)?", "id": "Ukuran Stabilitas Relatif Terkait Fasa." },
  { "en": "Apa Itu Sistem Umpan Balik?", "id": "Sistem Menggunakan Output Untuk Mengontrol Input." },
  { "en": "Apa Itu Umpan Balik Negatif?", "id": "Meningkatkan Stabilitas Dan Mengurangi Error." },
  { "en": "Apa Itu Umpan Balik Positif?", "id": "Dapat Menyebabkan Ketidakstabilan Dan Osilasi." },
  { "en": "Apa Itu Root Locus?", "id": "Plot Lintasan Kutub Loop Tertutup." },
  { "en": "Apa Itu State-Space Representation?", "id": "Representasi Sistem Menggunakan Variabel Keadaan." },
  { "en": "Apa Itu Motor DC Disikat (Brushed DC)?", "id": "Motor DC Menggunakan Sikat Untuk Komutasi." },
  { "en": "Apa Itu Jangkar (Armature)?", "id": "Bagian Berputar Motor DC." },
  { "en": "Apa Itu Medan Stator?", "id": "Medan Magnet Stasioner Motor." },
  { "en": "Apa Itu Komutator (Commutator)?", "id": "Sakelar Mekanis Pembalik Arah Arus Jangkar." },
  { "en": "Apa Itu Kontrol Kecepatan Motor DC?", "id": "Mengatur Tegangan Jangkar Atau Arus Medan." },
  { "en": "Apa Itu Motor Induksi (Asinkron)?", "id": "Motor AC Dengan Kecepatan Rotor Berbeda." },
  { "en": "Apa Itu Kecepatan Sinkron?", "id": "Kecepatan Putar Medan Magnet Stator." },
  { "en": "Apa Itu Slip Motor Induksi?", "id": "Perbedaan Persentase Antara Kecepatan Sinkron, Rotor." },
  { "en": "Apa Itu Rotor Sangkar Tupai?", "id": "Jenis Rotor Paling Umum Motor Induksi." },
  { "en": "Apa Itu Kontrol V/Hz Motor Induksi?", "id": "Menjaga Rasio Tegangan-Frekuensi Untuk Kontrol Kecepatan." },
  { "en": "Apa Itu Motor Sinkron?", "id": "Motor AC Rotor Berputar Pada Kecepatan Sinkron." },
  { "en": "Bagaimana Motor Sinkron Dijalankan?", "id": "Membutuhkan Mekanisme Start Hingga Kecepatan Sinkron." },
  { "en": "Apa Itu Sudut Torsi (Torque Angle)?", "id": "Sudut Antara Medan Magnet Rotor, Stator." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor DC Bergerak Dalam Langkah Sudut Diskret." },
  { "en": "Apa Itu Mode Full Step?", "id": "Mengaktifkan Dua Fasa Motor Stepper Sekaligus." },
  { "en": "Apa Itu Mode Half Step?", "id": "Mengaktifkan Satu Dan Dua Fasa Bergantian." },
  { "en": "Apa Itu Microstepping?", "id": "Membagi Langkah Menjadi Langkah Lebih Kecil." },
  { "en": "Apa Itu Mesin Listrik Fraksional?", "id": "Mesin Listrik Dengan Rating Daya Rendah." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle)?", "id": "Rasio Waktu ON Terhadap Total Periode." },
  { "en": "Apa Itu Efisiensi Mesin Listrik?", "id": "Rasio Daya Mekanik Output Terhadap Listrik Input." },
  { "en": "Apa Saja Kerugian Dalam Mesin Listrik?", "id": "Kerugian Tembaga, Inti, Mekanis, Dan Sampingan." },
  { "en": "Apa Itu Kerugian Tembaga (I²R)?", "id": "Kerugian Panas Akibat Resistansi Lilitan." },
  { "en": "Apa Itu Kerugian Inti Besi?", "id": "Kerugian Histeresis Dan Arus Eddy." },
  { "en": "Apa Itu Regulasi Tegangan Generator?", "id": "Perubahan Tegangan Terminal Dari Tanpa Beban, Beban Penuh." },
  { "en": "Apa Itu Reluktansi Magnetik?", "id": "Hambatan Terhadap Pembentukan Fluks Magnet." },
  { "en": "Apa Itu Rangkaian Magnetik?", "id": "Analogi Rangkaian Listrik Untuk Medan Magnet." },
  { "en": "Apa Itu Gaya Gerak Magnet (MMF)?", "id": "Penyebab Timbulnya Fluks Magnet Dalam Rangkaian." },
  { "en": "Apa Itu Kurva B-H (Kurva Magnetisasi)?", "id": "Hubungan Antara Kerapatan Fluks, Kuat Medan." },
  { "en": "Apa Itu Histeresis Magnetik?", "id": "Keterlambatan Magnetisasi Terhadap Perubahan Medan." },
  { "en": "Apa Itu Material Magnetik Lunak?", "id": "Mudah Dimagnetisasi Dan Didemagnetisasi." },
  { "en": "Apa Itu Material Magnetik Keras?", "id": "Sulit Didemagnetisasi, Untuk Magnet Permanen." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
