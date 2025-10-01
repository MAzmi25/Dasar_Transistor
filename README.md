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


  { "en": "Apa Fungsi Utama Dari Sebuah Transistor?", "id": "Sebagai Saklar Dan Penguat Sinyal Elektronik." },
  { "en": "Sebutkan Tiga Terminal Pada Transistor BJT?", "id": "Emitor, Basis, Dan Juga Kolektor." },
  { "en": "Apa Kepanjangan Dari BJT Dalam Elektronika?", "id": "Bipolar Junction Transistor Adalah Kepanjangannya." },
  { "en": "Apa Kepanjangan Dari FET Dalam Elektronika?", "id": "Field-Effect Transistor Adalah Kepanjangannya." },
  { "en": "Siapa Penemu Pertama Kali Komponen Transistor?", "id": "John Bardeen, Walter Brattain, William Shockley." },
  { "en": "Bahan Semikonduktor Apa Yang Paling Umum?", "id": "Bahan Silikon Dan Germanium Adalah Jawabannya." },
  { "en": "Apa Itu Daerah Aktif Pada Transistor?", "id": "Daerah Kerja Normal Untuk Penguatan Sinyal." },
  { "en": "Apa Itu Daerah Saturasi Pada Transistor?", "id": "Kondisi Transistor Bekerja Sebagai Saklar Tertutup." },
  { "en": "Apa Itu Daerah Cut-Off Pada Transistor?", "id": "Kondisi Transistor Bekerja Sebagai Saklar Terbuka." },
  { "en": "Apa Jenis Transistor BJT Yang Ada?", "id": "Jenis NPN Dan Juga Jenis PNP." },
  { "en": "Apa Perbedaan Antara Transistor NPN Dan PNP?", "id": "Arah Aliran Arus Listrik Yang Berbeda." },
  { "en": "Apa Fungsi Terminal Basis Pada Transistor?", "id": "Mengontrol Aliran Arus Antara Emitor Kolektor." },
  { "en": "Apa Fungsi Terminal Emitor Pada Transistor?", "id": "Sumber Pembawa Muatan Listrik Pada Transistor." },
  { "en": "Apa Fungsi Terminal Kolektor Pada Transistor?", "id": "Mengumpulkan Pembawa Muatan Dari Terminal Emitor." },
  { "en": "Apa Itu Proses Doping Pada Semikonduktor?", "id": "Penambahan Atom Asing Untuk Mengubah Sifat." },
  { "en": "Apa Yang Dimaksud Dengan Penguatan Arus?", "id": "Rasio Arus Kolektor Terhadap Arus Basis." },
  { "en": "Apa Simbol Skematik Untuk Transistor NPN?", "id": "Panah Keluar Pada Terminal Kaki Emitor." },
  { "en": "Apa Simbol Skematik Untuk Transistor PNP?", "id": "Panah Masuk Pada Terminal Kaki Emitor." },
  { "en": "Apa Satuan Untuk Penguatan Arus (Beta)?", "id": "Tidak Memiliki Satuan Apapun (Besaran Rasio)." },
  { "en": "Bagaimana Cara Mengaktifkan Sebuah Transistor BJT?", "id": "Memberikan Tegangan Maju Pada Sambungan Basis-Emitor." },
  { "en": "Apa Itu Garis Beban Pada Transistor?", "id": "Grafik Hubungan Antara Arus Dan Tegangan." },
  { "en": "Apa Kepanjangan Dari MOSFET Dalam Elektronika?", "id": "Metal-Oxide-Semiconductor Field-Effect Transistor." },
  { "en": "Sebutkan Tiga Terminal Pada Transistor FET?", "id": "Source, Gate, Dan Juga Drain." },
  { "en": "Apa Fungsi Terminal Gate Pada FET?", "id": "Mengontrol Aliran Arus Antara Source Drain." },
  { "en": "Apa Keuntungan Utama Dari Transistor FET?", "id": "Memiliki Impedansi Input Yang Sangat Tinggi." },
  { "en": "Apa Jenis Transistor MOSFET Yang Ada?", "id": "Tipe N-Channel Dan Juga Tipe P-Channel." },
  { "en": "Bagaimana Cara Kerja Dari Transistor MOSFET?", "id": "Bekerja Berdasarkan Efek Medan Listrik Statis." },
  { "en": "Apa Itu Tegangan Threshold Pada MOSFET?", "id": "Tegangan Minimum Untuk Menghidupkan Transistor MOSFET." },
  { "en": "Transistor Digunakan Dalam Rangkaian Apa Saja?", "id": "Amplifier, Osilator, Dan Juga Rangkaian Logika." },
  { "en": "Apa Itu Arus Bocor Pada Transistor?", "id": "Arus Kecil Yang Mengalir Saat Transistor Mati." },
  { "en": "Apa Yang Menyebabkan Transistor Menjadi Panas?", "id": "Disipasi Daya Listrik Yang Terjadi Didalamnya." },
  { "en": "Apa Fungsi Heatsink Pada Sebuah Transistor?", "id": "Menyerap Dan Membuang Panas Yang Berlebih." },
  { "en": "Apa Itu Transistor Darlington Dalam Elektronika?", "id": "Gabungan Dua Transistor BJT Menjadi Satu." },
  { "en": "Apa Keuntungan Menggunakan Pasangan Darlington?", "id": "Mendapatkan Penguatan Arus Yang Sangat Tinggi." },
  { "en": "Apa Kepanjangan IGBT Dalam Dunia Elektronika?", "id": "Insulated Gate Bipolar Transistor Adalah Kepanjangannya." },
  { "en": "Transistor Apa Yang Menggabungkan BJT Dan MOSFET?", "id": "Transistor Jenis Insulated Gate Bipolar Transistor." },
  { "en": "Untuk Aplikasi Apa IGBT Sering Digunakan?", "id": "Aplikasi Daya Tinggi Seperti Inverter Motor." },
  { "en": "Apa Itu Fototransistor Dalam Dunia Elektronika?", "id": "Transistor Yang Dikontrol Menggunakan Intensitas Cahaya." },
  { "en": "Bagaimana Cara Kerja Komponen Fototransistor?", "id": "Mengubah Energi Cahaya Menjadi Sinyal Listrik." },
  { "en": "Di Mana Fototransistor Biasa Diterapkan?", "id": "Sensor Cahaya, Optocoupler, Dan Sistem Otomatis." },
  { "en": "Apa Itu Bias Transistor Yang Tepat?", "id": "Pemberian Tegangan DC Untuk Operasi Tepat." },
  { "en": "Mengapa Pembiasan Transistor Sangat Penting Dilakukan?", "id": "Agar Transistor Bekerja Pada Daerah Aktif." },
  { "en": "Sebutkan Salah Satu Metode Bias Transistor?", "id": "Metode Bias Pembagi Tegangan Yang Populer." },
  { "en": "Apa Yang Dimaksud Dengan Titik Q?", "id": "Titik Operasi Diam Pada Garis Beban." },
  { "en": "Apa Pengaruh Suhu Terhadap Kinerja Transistor?", "id": "Dapat Mengubah Karakteristik Dan Titik Kerjanya." },
  { "en": "Bagaimana Cara Menguji Kondisi Sebuah Transistor?", "id": "Menggunakan Multimeter Pada Mode Dioda Test." },
  { "en": "Apa Indikasi Transistor Yang Rusak Total?", "id": "Terjadi Hubungan Singkat Antara Semua Kaki." },
  { "en": "Apa Itu Konfigurasi Common Emitter (CE)?", "id": "Terminal Emitor Digunakan Bersama Input Output." },
  { "en": "Apa Karakteristik Utama Konfigurasi Common Emitter?", "id": "Penguatan Tegangan Dan Arus Yang Tinggi." },
  { "en": "Apa Itu Konfigurasi Common Collector (CC)?", "id": "Terminal Kolektor Digunakan Bersama Input Output." },
  { "en": "Apa Karakteristik Utama Konfigurasi Common Collector?", "id": "Penguatan Arus Tinggi, Penguatan Tegangan Rendah." },
  { "en": "Apa Nama Lain Konfigurasi Common Collector?", "id": "Nama Lainnya Adalah Rangkaian Emitter Follower." },
  { "en": "Apa Itu Konfigurasi Common Base (CB)?", "id": "Terminal Basis Digunakan Bersama Input Output." },
  { "en": "Apa Karakteristik Utama Konfigurasi Common Base?", "id": "Penguatan Tegangan Tinggi, Penguatan Arus Rendah." },
  { "en": "Konfigurasi Mana Yang Membalik Fasa Sinyal?", "id": "Konfigurasi Common Emitter Membalik Fasa Sinyal." },
  { "en": "Apa Itu Impedansi Input Pada Transistor?", "id": "Hambatan Internal Dilihat Dari Terminal Input." },
  { "en": "Apa Itu Impedansi Output Pada Transistor?", "id": "Hambatan Internal Dilihat Dari Terminal Output." },
  { "en": "Konfigurasi Mana Dengan Impedansi Input Tertinggi?", "id": "Konfigurasi Common Collector (Emitter Follower)." },
  { "en": "Konfigurasi Mana Dengan Impedansi Output Terendah?", "id": "Konfigurasi Common Collector (Emitter Follower)." },
  { "en": "Apa Itu Efek Miller Pada Transistor?", "id": "Peningkatan Kapasitansi Input Karena Penguatan Tegangan." },
  { "en": "Kapan Transistor Digunakan Sebagai Sebuah Saklar?", "id": "Saat Dioperasikan Pada Daerah Saturasi Cut-Off." },
  { "en": "Kapan Transistor Digunakan Sebagai Sebuah Penguat?", "id": "Saat Dioperasikan Pada Daerah Aktif Linear." },
  { "en": "Apa Bahan Dasar Dari Terminal Gate?", "id": "Bahan Logam Oksida (Metal Oxide)." },
  { "en": "Mengapa Transistor Merevolusi Dunia Elektronika Modern?", "id": "Ukuran Kecil, Efisiensi Tinggi, Tahan Lama." },
  { "en": "Apa Yang Digantikan Oleh Teknologi Transistor?", "id": "Menggantikan Teknologi Tabung Hampa Yang Besar." },
  { "en": "Di Mana Transistor Dapat Kita Temukan?", "id": "Hampir Di Semua Perangkat Elektronik Modern." },
  { "en": "Apa Itu Sirkuit Terpadu Atau IC?", "id": "Jutaan Transistor Yang Dikemas Menjadi Satu." },
  { "en": "Apa Kepanjangan Dari IC Dalam Elektronika?", "id": "Integrated Circuit Adalah Nama Kepanjangannya." },
  { "en": "Apa Hukum Moore Terkait Dengan Transistor?", "id": "Jumlah Transistor Pada IC Berlipat Ganda." },
  { "en": "Berapa Lama Hukum Moore Diperkirakan Berlaku?", "id": "Jumlahnya Berlipat Ganda Setiap Dua Tahun Sekali." },
  { "en": "Apa Itu Doping Tipe-N Pada Semikonduktor?", "id": "Menambahkan Atom Donor Untuk Kelebihan Elektron." },
  { "en": "Apa Itu Doping Tipe-P Pada Semikonduktor?", "id": "Menambahkan Atom Akseptor Untuk Kelebihan Lubang." },
  { "en": "Apa Pembawa Muatan Mayoritas Pada Tipe-N?", "id": "Elektron Bebas Adalah Pembawa Muatan Mayoritas." },
  { "en": "Apa Pembawa Muatan Mayoritas Pada Tipe-P?", "id": "Lubang (Hole) Adalah Pembawa Muatan Mayoritas." },
  { "en": "Bagaimana Sambungan PN Terbentuk Dalam Transistor?", "id": "Pertemuan Antara Semikonduktor Tipe P N." },
  { "en": "Apa Itu Daerah Deplesi Sambungan PN?", "id": "Daerah Tanpa Pembawa Muatan Dekat Sambungan." },
  { "en": "Apa Itu Tegangan Tembus Atau Breakdown?", "id": "Tegangan Yang Menyebabkan Arus Balik Besar." },
  { "en": "Apakah Transistor Merupakan Komponen Aktif Atau Pasif?", "id": "Transistor Merupakan Salah Satu Komponen Aktif." },
  { "en": "Mengapa Transistor Disebut Komponen Aktif Elektronik?", "id": "Karena Dapat Menguatkan Sinyal Daya Listrik." },
  { "en": "Sebutkan Contoh Komponen Pasif Dalam Elektronika?", "id": "Resistor, Kapasitor, Dan Juga Induktor." },
  { "en": "Apa Peran Transistor Dalam Gerbang Logika?", "id": "Sebagai Saklar Elektronik Untuk Operasi Digital." },
  { "en": "Apa Itu Mode Peningkatan Pada MOSFET?", "id": "Transistor Mati Saat Tegangan Gate Nol." },
  { "en": "Apa Itu Mode Deplesi Pada MOSFET?", "id": "Transistor Hidup Saat Tegangan Gate Nol." },
  { "en": "Bagaimana Polaritas Tegangan Bias Transistor PNP?", "id": "Basis Negatif Dan Emitor Positif Bekerja." },
  { "en": "Bagaimana Polaritas Tegangan Bias Transistor NPN?", "id": "Basis Positif Dan Emitor Negatif Bekerja." },
  { "en": "Apa Itu Parameter Hibrida (h-parameters)?", "id": "Model Matematika Untuk Menganalisis Sinyal Kecil." },
  { "en": "Apa Itu Arus Basis (Ib) Transistor?", "id": "Arus Kecil Yang Masuk Ke Terminal Basis." },
  { "en": "Apa Itu Arus Kolektor (Ic) Transistor?", "id": "Arus Besar Yang Dikontrol Oleh Arus Basis." },
  { "en": "Apa Itu Arus Emitor (Ie) Transistor?", "id": "Total Penjumlahan Dari Arus Basis Kolektor." },
  { "en": "Rumus Apa Yang Menghubungkan Ketiga Arus?", "id": "Arus Emitor Sama Dengan Arus Kolektor Basis." },
  { "en": "Apa Itu JFET Dalam Dunia Elektronika?", "id": "Junction Field-Effect Transistor Adalah Kepanjangannya." },
  { "en": "Apa Perbedaan Utama JFET Dengan MOSFET?", "id": "JFET Tidak Memiliki Lapisan Oksida Insulator." },
  { "en": "Material Apa Yang Digunakan Untuk Insulator Gate?", "id": "Material Silikon Dioksida (SiO2) Yang Umum." },
  { "en": "Apa Fungsi Transistor Dalam Rangkaian Osilator?", "id": "Menghasilkan Sinyal Periodik Secara Terus Menerus." },
  { "en": "Apa Itu Thermal Runaway Pada Transistor?", "id": "Peningkatan Suhu Yang Menyebabkan Kerusakan Permanen." },
  { "en": "Bagaimana Mencegah Terjadinya Thermal Runaway?", "id": "Menggunakan Rangkaian Umpan Balik Dan Heatsink." },
  { "en": "Apa Itu Transistor Unijunction Atau UJT?", "id": "Transistor Dengan Tiga Terminal Dan Satu Sambungan." },
  { "en": "Di Mana Transistor UJT Biasa Digunakan?", "id": "Dalam Rangkaian Pemicu Dan Rangkaian Osilator." },
  { "en": "Apa Itu Kelas Penguat (Amplifier) A?", "id": "Transistor Aktif Selama Satu Siklus Penuh." },
  { "en": "Apa Efisiensi Maksimal Dari Penguat Kelas A?", "id": "Efisiensi Teoritis Maksimal Adalah Dua Puluh Lima Persen." },
  { "en": "Apa Itu Kelas Penguat (Amplifier) B?", "id": "Transistor Aktif Selama Setengah Siklus Saja." },
  { "en": "Apa Masalah Utama Pada Penguat Kelas B?", "id": "Mengalami Cacat Silang Atau Crossover Distortion." },
  { "en": "Apa Itu Kelas Penguat (Amplifier) AB?", "id": "Gabungan Karakteristik Dari Kelas A Dan B." },
  { "en": "Mengapa Penguat Kelas AB Dibuat Dan Digunakan?", "id": "Untuk Mengatasi Masalah Cacat Silang (Crossover)." },
  { "en": "Apa Itu Kelas Penguat (Amplifier) C?", "id": "Transistor Aktif Kurang Dari Setengah Siklus." },
  { "en": "Di Mana Penguat Kelas C Biasa Digunakan?", "id": "Pada Rangkaian Frekuensi Radio (RF) Tinggi." },
  { "en": "Apa Itu Efek Awal Atau Early Effect?", "id": "Modulasi Lebar Basis Oleh Tegangan Kolektor-Basis." },
  { "en": "Siapa Yang Pertama Menemukan Efek Awal?", "id": "Ditemukan Pertama Kali Oleh James M. Early." },
  { "en": "Apa Akibat Dari Terjadinya Efek Awal?", "id": "Meningkatkan Arus Kolektor Dan Mengurangi Penguatan." },
  { "en": "Apa Itu Transkonduktansi Pada Komponen Transistor?", "id": "Perubahan Arus Output Terhadap Perubahan Tegangan Input." },
  { "en": "Apa Satuan Untuk Besaran Nilai Transkonduktansi?", "id": "Satuannya Adalah Siemens (S) Atau Mho." },
  { "en": "Transistor Jenis Apa Yang Mengandalkan Transkonduktansi?", "id": "Transistor Efek Medan (FET) Sangat Mengandalkannya." },
  { "en": "Apa Yang Dimaksud Dengan Analisis Sinyal Kecil?", "id": "Analisis Perilaku Transistor Untuk Sinyal AC Kecil." },
  { "en": "Apa Yang Dimaksud Dengan Analisis Sinyal Besar?", "id": "Analisis Perilaku Transistor Untuk Sinyal DC." },
  { "en": "Apa Itu Rise Time Pada Saklar Transistor?", "id": "Waktu Arus Naik Dari Sepuluh Persen." },
  { "en": "Apa Itu Fall Time Pada Saklar Transistor?", "id": "Waktu Arus Turun Dari Sembilan Puluh Persen." },
  { "en": "Apa Kepanjangan Dari RF Dalam Elektronika?", "id": "Radio Frequency Adalah Nama Kepanjangannya." },
  { "en": "Mengapa Transistor Punya Batas Frekuensi Kerja?", "id": "Karena Adanya Kapasitansi Parasitik Internal Didalamnya." },
  { "en": "Apa Itu Gain-Bandwidth Product (GBP)?", "id": "Hasil Kali Penguatan Dengan Lebar Pita." },
  { "en": "Bagaimana Transistor Bekerja Sebagai Resistor Variabel?", "id": "Dengan Mengoperasikannya Pada Daerah Triode Atau Ohmik." },
  { "en": "Apa Itu Daerah Ohmik Pada Transistor?", "id": "Daerah Operasi Dimana Bertindak Seperti Resistor." },
  { "en": "Apa Itu Kemasan Transistor Model TO-92?", "id": "Kemasan Plastik Kecil Dengan Tiga Kaki Lurus." },
  { "en": "Apa Itu Kemasan Transistor Model TO-220?", "id": "Kemasan Dengan Tab Logam Untuk Pendingin (Heatsink)." },
  { "en": "Apa Fungsi Tab Logam Pada Kemasan TO-220?", "id": "Untuk Memasang Heatsink Pembuang Panas Berlebih." },
  { "en": "Apa Itu Surface Mount Device (SMD)?", "id": "Komponen Elektronik Yang Dipasang Di Permukaan PCB." },
  { "en": "Apa Kepanjangan Dari PCB Dalam Dunia Elektronika?", "id": "Printed Circuit Board Adalah Kepanjangan Resminya." },
  { "en": "Apa Keuntungan Transistor Dengan Kemasan SMD?", "id": "Ukuran Jauh Lebih Kecil Dan Hemat Ruang." },
  { "en": "Bagaimana Cara Kerja Dari Sebuah Optocoupler?", "id": "Mengirim Sinyal Listrik Menggunakan Media Cahaya." },
  { "en": "Apa Komponen Utama Dalam Sebuah Optocoupler?", "id": "Light Emitting Diode Dan Sebuah Fototransistor." },
  { "en": "Apa Tujuan Utama Penggunaan Sebuah Optocoupler?", "id": "Untuk Isolasi Elektrik Antara Dua Rangkaian." },
  { "en": "Apa Itu Derau Atau Noise Transistor?", "id": "Sinyal Acak Yang Tidak Diinginkan Sama Sekali." },
  { "en": "Dari Mana Sumber Derau Pada Transistor?", "id": "Berasal Dari Proses Fisik Acak Internal." },
  { "en": "Sebutkan Jenis Derau Pada Komponen Transistor?", "id": "Shot Noise, Thermal Noise, Dan Flicker Noise." },
  { "en": "Apa Itu Faktor Derau Atau Noise Figure?", "id": "Ukuran Penurunan Rasio Sinyal Terhadap Derau." },
  { "en": "Apa Itu Tegangan Saturasi Kolektor-Emitor (VCEsat)?", "id": "Tegangan Rendah Saat Transistor Dalam Kondisi Saturasi." },
  { "en": "Berapa Nilai Ideal Dari Tegangan VCEsat?", "id": "Nilai Idealnya Adalah Nol Volt (0V)." },
  { "en": "Apakah Transistor Simetris Komponennya Secara Fisik?", "id": "Tidak, Emitor Dan Kolektor Tidak Simetris." },
  { "en": "Mengapa Daerah Kolektor Dibuat Lebih Besar?", "id": "Untuk Disipasi Panas Yang Lebih Baik." },
  { "en": "Mengapa Doping Emitor Dibuat Sangat Tinggi?", "id": "Untuk Menghasilkan Efisiensi Emitor Yang Tinggi." },
  { "en": "Apa Itu Efisiensi Emitor Pada Transistor?", "id": "Rasio Arus Emitor Terhadap Arus Total." },
  { "en": "Apa Itu Arus Balik Saturasi (Reverse Saturation)?", "id": "Arus Bocor Kecil Pada Sambungan Bias Balik." },
  { "en": "Apa Itu Efek Zener Pada Transistor?", "id": "Breakdown Pada Sambungan Basis-Emitor Yang Terbias Balik." },
  { "en": "Apa Itu Efek Avalanche Pada Transistor?", "id": "Breakdown Karena Percepatan Pembawa Muatan Listrik." },
  { "en": "Apa Perbedaan Antara Efek Zener Dan Avalanche?", "id": "Mekanisme Fisik Penyebab Breakdown Yang Berbeda." },
  { "en": "Apa Itu Kapasitansi Difusi Pada Sambungan?", "id": "Kapasitansi Akibat Injeksi Pembawa Muatan Minoritas." },
  { "en": "Apa Itu Kapasitansi Transisi Pada Sambungan?", "id": "Kapasitansi Akibat Lebar Daerah Deplesi Berubah." },
  { "en": "Bagaimana Cara Menentukan Kaki Transistor BJT?", "id": "Melihat Datasheet Atau Menggunakan Alat Ukur." },
  { "en": "Apa Informasi Penting Dalam Lembar Data?", "id": "Peringkat Maksimum Dan Karakteristik Kinerja Listrik." },
  { "en": "Apa Itu Peringkat Tegangan Maksimum Transistor?", "id": "Batas Tegangan Operasi Yang Paling Aman." },
  { "en": "Apa Itu Peringkat Arus Maksimum Transistor?", "id": "Batas Arus Operasi Yang Paling Aman." },
  { "en": "Apa Yang Terjadi Jika Peringkat Terlampaui?", "id": "Transistor Akan Mengalami Kerusakan Secara Permanen." },
  { "en": "Apa Itu Transistor Array Dalam Elektronika?", "id": "Beberapa Transistor Dalam Satu Kemasan Tunggal." },
  { "en": "Apa Keuntungan Menggunakan Sebuah Transistor Array?", "id": "Karakteristik Transistor Yang Sangat Mirip (Matched)." },
  { "en": "Apa Itu Current Mirror Atau Cermin Arus?", "id": "Rangkaian Untuk Menyalin Arus Secara Akurat." },
  { "en": "Mengapa Rangkaian Cermin Arus Sangat Berguna?", "id": "Untuk Memberikan Arus Bias Yang Stabil." },
  { "en": "Apa Itu Penguat Diferensial Dalam Elektronika?", "id": "Penguat Yang Menguatkan Selisih Dua Sinyal." },
  { "en": "Apa Komponen Utama Penguat Diferensial Modern?", "id": "Dua Transistor Dengan Karakteristik Yang Identik." },
  { "en": "Apa Itu Common-Mode Rejection Ratio (CMRR)?", "id": "Kemampuan Menolak Sinyal Yang Sama (Common)." },
  { "en": "Apa Kepanjangan CMRR Dalam Dunia Elektronika?", "id": "Common-Mode Rejection Ratio Adalah Kepanjangannya." },
  { "en": "Nilai CMRR Yang Baik Itu Tinggi Atau Rendah?", "id": "Nilai Yang Sangat Tinggi Dianggap Baik." },
  { "en": "Apa Itu Active Load Atau Beban Aktif?", "id": "Menggunakan Transistor Sebagai Pengganti Komponen Resistor." },
  { "en": "Apa Keuntungan Menggunakan Beban Aktif Modern?", "id": "Mendapatkan Penguatan Tegangan Yang Lebih Tinggi." },
  { "en": "Apa Itu Rangkaian Cascode Dalam Elektronika?", "id": "Gabungan Konfigurasi Common Emitter Common Base." },
  { "en": "Apa Tujuan Dari Penggunaan Rangkaian Cascode?", "id": "Meningkatkan Lebar Pita Dan Mengurangi Efek Miller." },
  { "en": "Apa Itu Thyristor Dalam Dunia Elektronika?", "id": "Keluarga Semikonduktor Dengan Empat Lapisan P-N-P-N." },
  { "en": "Sebutkan Contoh Komponen Keluarga Thyristor?", "id": "Silicon Controlled Rectifier (SCR) Dan TRIAC." },
  { "en": "Apa Fungsi Utama Dari Sebuah SCR?", "id": "Sebagai Saklar Daya Searah (DC) Terkendali." },
  { "en": "Apa Kepanjangan SCR Dalam Dunia Elektronika?", "id": "Silicon Controlled Rectifier Adalah Kepanjangannya." },
  { "en": "Apa Fungsi Utama Dari Sebuah TRIAC?", "id": "Sebagai Saklar Daya Bolak-Balik (AC) Terkendali." },
  { "en": "Apakah Transistor Dapat Menguatkan Sinyal Daya?", "id": "Ya, Transistor Adalah Penguat Sinyal Daya." },
  { "en": "Apa Beda Penguatan Tegangan Dan Penguatan Daya?", "id": "Penguatan Daya Melibatkan Arus Dan Juga Tegangan." },
  { "en": "Apa Itu Kurva Karakteristik Output Transistor?", "id": "Grafik Hubungan Arus Kolektor Dan Tegangan." },
  { "en": "Apa Itu Kurva Karakteristik Input Transistor?", "id": "Grafik Hubungan Arus Basis Dan Tegangan." },
  { "en": "Apa Itu Efek Kirk Pada Transistor?", "id": "Perluasan Daerah Basis Pada Arus Tinggi." },
  { "en": "Apa Itu Transistor Heterojunction Bipolar (HBT)?", "id": "Transistor BJT Menggunakan Material Semikonduktor Berbeda." },
  { "en": "Apa Keuntungan Utama Dari Transistor HBT?", "id": "Kinerja Frekuensi Tinggi Yang Jauh Unggul." },
  { "en": "Apa Itu Transistor Efek Medan Statik Induksi?", "id": "Perangkat Arus Mayoritas Dengan Kecepatan Tinggi." },
  { "en": "Bagaimana Cara Kerja Transistor Sebagai Dioda?", "id": "Menghubungkan Terminal Basis Dan Terminal Kolektor." },
  { "en": "Apa Itu Respon Frekuensi Sebuah Penguat?", "id": "Ukuran Kinerja Penguat Pada Frekuensi Berbeda." },
  { "en": "Apa Itu Frekuensi Cut-off Pada Penguat?", "id": "Frekuensi Dimana Penguatan Turun Tiga Desibel." },
  { "en": "Apa Itu Decibel (dB) Dalam Elektronika?", "id": "Satuan Logaritmik Untuk Menyatakan Rasio Daya." },
  { "en": "Apa Itu Umpan Balik (Feedback) Negatif?", "id": "Mengembalikan Sebagian Sinyal Output Ke Input." },
  { "en": "Apa Manfaat Umpan Balik Negatif Penguat?", "id": "Meningkatkan Stabilitas Dan Mengurangi Distorsi Sinyal." },
  { "en": "Apa Itu Distorsi Harmonik Pada Sinyal?", "id": "Munculnya Frekuensi Kelipatan Dari Sinyal Asli." },
  { "en": "Apa Penyebab Distorsi Pada Rangkaian Penguat?", "id": "Ketidaklinearan Dari Komponen Transistor Yang Dipakai." },
  { "en": "Apa Itu Model Ebers-Moll Transistor BJT?", "id": "Model Matematika Ideal Dari Transistor BJT." },
  { "en": "Apa Itu Model Hybrid-Pi Transistor BJT?", "id": "Model Sirkuit Sinyal Kecil Yang Populer." },
  { "en": "Apa Itu Stabilitas Termal Rangkaian Transistor?", "id": "Kemampuan Rangkaian Menjaga Titik Operasi Stabil." },
  { "en": "Bagaimana Resistor Emitor Meningkatkan Stabilitas Termal?", "id": "Memberikan Umpan Balik Negatif Arus DC." },
  { "en": "Apa Itu Penguat Operasional Atau Op-Amp?", "id": "Penguat Diferensial Dengan Penguatan Sangat Tinggi." },
  { "en": "Apa Kepanjangan Op-Amp Dalam Dunia Elektronika?", "id": "Operational Amplifier Adalah Nama Kepanjangannya." },
  { "en": "Apa Tahap Input Dari Sebuah Op-Amp?", "id": "Rangkaian Penguat Diferensial Berbasis Transistor Modern." },
  { "en": "Apa Fungsi Transistor Dalam Sebuah Catu Daya?", "id": "Sebagai Komponen Pengatur Tegangan (Voltage Regulator)." },
  { "en": "Apa Itu Transistor-Transistor Logic (TTL)?", "id": "Keluarga Sirkuit Logika Digital Berbasis BJT." },
  { "en": "Apa Kepanjangan Dari TTL Dalam Elektronika?", "id": "Transistor-Transistor Logic Adalah Kepanjangan Resminya." },
  { "en": "Berapa Tegangan Catu Daya Standar TTL?", "id": "Tegangan Standar Adalah Positif Lima Volt." },
  { "en": "Apa Itu Emitter-Coupled Logic (ECL)?", "id": "Keluarga Logika Berkecepatan Sangat Tinggi Sekali." },
  { "en": "Apa Kepanjangan Dari ECL Dalam Elektronika?", "id": "Emitter-Coupled Logic Adalah Kepanjangan Resminya." },
  { "en": "Apa Kelemahan Utama Dari Rangkaian ECL?", "id": "Membutuhkan Konsumsi Daya Yang Sangat Besar." },
  { "en": "Apa Itu Safe Operating Area (SOA)?", "id": "Area Kondisi Aman Operasi Sebuah Transistor." },
  { "en": "Apa Kepanjangan Dari SOA Dalam Datasheet?", "id": "Safe Operating Area Adalah Kepanjangan Resminya." },
  { "en": "Grafik Apa Yang Merepresentasikan Kurva SOA?", "id": "Grafik Antara Tegangan Kolektor Dan Arus." },
  { "en": "Apa Itu Kegagalan Second Breakdown Transistor?", "id": "Kerusakan Termal Lokal Yang Merusak Transistor." },
  { "en": "Apa Perbedaan Transistor Sinyal Dan Daya?", "id": "Kemampuan Menangani Arus Dan Tegangan Besar." },
  { "en": "Manakah Yang Punya Kemasan Fisik Lebih Besar?", "id": "Transistor Daya Punya Kemasan Lebih Besar." },
  { "en": "Apa Itu Pencocokan Transistor (Transistor Matching)?", "id": "Memilih Transistor Dengan Karakteristik Paling Mirip." },
  { "en": "Kapan Pencocokan Transistor Sangat Diperlukan Sekali?", "id": "Pada Rangkaian Cermin Arus Dan Diferensial." },
  { "en": "Apa Itu Teorema Miller Dalam Elektronika?", "id": "Metode Analisis Kapasitansi Umpan Balik Penguat." },
  { "en": "Apa Kode Populer Untuk Transistor NPN Universal?", "id": "Kode 2N2222 Dan BC547 Adalah Contohnya." },
  { "en": "Apa Kode Populer Untuk Transistor PNP Universal?", "id": "Kode 2N2907 Dan BC557 Adalah Contohnya." },
  { "en": "Bagaimana Transistor Mengendalikan Sebuah Kumparan Relay?", "id": "Bekerja Sebagai Saklar Untuk Mengalirkan Arus." },
  { "en": "Mengapa Perlu Dioda Paralel Dengan Relay?", "id": "Melindungi Transistor Dari Tegangan Induksi Balik." },
  { "en": "Dioda Tersebut Biasa Disebut Dioda Apa?", "id": "Biasa Disebut Dioda Flyback Atau Freewheeling." },
  { "en": "Apa Itu Slew Rate Pada Sebuah Penguat?", "id": "Laju Perubahan Tegangan Output Maksimum Penguat." },
  { "en": "Apa Satuan Untuk Besaran Nilai Slew Rate?", "id": "Volt Per Mikrodetik (V/µs) Adalah Satuannya." },
  { "en": "Apa Itu Transistor Fotodarlington Dalam Elektronika?", "id": "Gabungan Fototransistor Dengan Transistor Penguat Tambahan." },
  { "en": "Apa Keuntungan Utama Dari Fotodarlington?", "id": "Memiliki Sensitivitas Cahaya Yang Sangat Tinggi." },
  { "en": "Apa Itu Transistor Avalanche Dalam Elektronika?", "id": "Transistor Didesain Bekerja Di Daerah Breakdown." },
  { "en": "Apa Aplikasi Utama Dari Transistor Avalanche?", "id": "Pembangkit Pulsa Yang Sangat Cepat Sekali." },
  { "en": "Apa Itu Transistor Digital Atau Pre-Biased?", "id": "Transistor BJT Dengan Resistor Internal Terintegrasi." },
  { "en": "Apa Keuntungan Menggunakan Sebuah Transistor Digital?", "id": "Mengurangi Jumlah Komponen Eksternal Yang Diperlukan." },
  { "en": "Apa Tegangan Saturasi Pasangan Darlington?", "id": "Lebih Tinggi Dibandingkan Transistor BJT Tunggal." },
  { "en": "Apa Itu Efek Sziklai Pair Atau Pasangan?", "id": "Konfigurasi Darlington Menggunakan Transistor NPN PNP." },
  { "en": "Apa Nama Lain Untuk Pasangan Sziklai?", "id": "Nama Lainnya Adalah Complementary Feedback Pair." },
  { "en": "Apa Itu Power Bipolar Junction Transistor?", "id": "Transistor BJT Didesain Untuk Aplikasi Daya." },
  { "en": "Struktur Vertikal Atau Horisontal Untuk Power BJT?", "id": "Menggunakan Struktur Vertikal Untuk Arus Tinggi." },
  { "en": "Apa Itu Current Crowding Pada Emitor?", "id": "Distribusi Arus Yang Tidak Merata Sama Sekali." },
  { "en": "Bagaimana Cara Mengatasi Masalah Current Crowding?", "id": "Desain Emitor Berbentuk Jari-Jari (Interdigitated)." },
  { "en": "Apa Itu Transistor Single-Electron (SET)?", "id": "Transistor Yang Beroperasi Pada Level Elektron Tunggal." },
  { "en": "Apa Kepanjangan DARI SET Dalam Elektronika?", "id": "Single-Electron Transistor Adalah Kepanjangan Resminya." },
  { "en": "Apa Syarat Operasi Dari Transistor SET?", "id": "Harus Beroperasi Pada Suhu Sangat Rendah." },
  { "en": "Apa Itu FinFET Dalam Teknologi Semikonduktor?", "id": "Jenis MOSFET Non-Planar Dengan Struktur Sirip." },
  { "en": "Apa Kepanjangan Dari FinFET Dalam Elektronika?", "id": "Fin Field-Effect Transistor Adalah Kepanjangannya." },
  { "en": "Apa Keuntungan Utama Arsitektur FinFET Modern?", "id": "Mengurangi Arus Bocor Dan Ukuran Semakin Kecil." },
  { "en": "Apa Itu Tegangan Basis-Emitor (VBE) On?", "id": "Tegangan Untuk Menghidupkan Sambungan Basis Emitor." },
  { "en": "Berapa Nilai Umum Tegangan VBE On?", "id": "Sekitar Nol Koma Tujuh Volt (0.7V)." },
  { "en": "Apakah Nilai Tegangan VBE On Selalu Konstan?", "id": "Tidak, Nilainya Sedikit Berubah Terhadap Suhu." },
  { "en": "Apa Itu Baker Clamp Dalam Rangkaian Digital?", "id": "Rangkaian Untuk Mencegah Saturasi Dalam Transistor." },
  { "en": "Apa Tujuan Mencegah Saturasi Transistor Saklar?", "id": "Untuk Meningkatkan Kecepatan Mematikan (Turn-Off Speed)." },
  { "en": "Apa Itu Dioda Schottky Dalam Rangkaian Elektronika?", "id": "Dioda Dengan Tegangan Maju Yang Rendah." },
  { "en": "Bagaimana Dioda Schottky Mencegah Proses Saturasi?", "id": "Menjepit Tegangan Kolektor-Basis Tetap Rendah." },
  { "en": "Apa Itu Penguat Logaritmik Berbasis Transistor?", "id": "Penguat Dengan Output Sebanding Logaritma Input." },
  { "en": "Apa Hubungan Yang Dimanfaatkan Penguat Logaritmik?", "id": "Hubungan Eksponensial Antara Arus Dan Tegangan." },
  { "en": "Bagaimana Transistor Bisa Menjadi Sumber Arus?", "id": "Dengan Menjaga Arus Kolektor Tetap Konstan." },
  { "en": "Apa Itu Impedansi Output Sumber Arus Ideal?", "id": "Nilai Impedansi Outputnya Adalah Tak Terhingga." },
  { "en": "Apa Itu Sirkuit Wilson Current Mirror?", "id": "Penyempurnaan Rangkaian Cermin Arus Yang Dasar." },
  { "en": "Apa Keuntungan Wilson Current Mirror Modern?", "id": "Impedansi Output Yang Jauh Lebih Tinggi." },
  { "en": "Apa Itu Beban Aktif Cascode Atau Cascode Load?", "id": "Beban Aktif Dengan Impedansi Output Tinggi." },
  { "en": "Apa Itu Penguat Umpan Balik Arus?", "id": "Jenis Penguat Dengan Umpan Balik Arus." },
  { "en": "Apa Karakteristik Utama Penguat Umpan Balik Arus?", "id": "Memiliki Slew Rate Yang Sangat Tinggi." },
  { "en": "Apa Itu High Electron Mobility Transistor?", "id": "Jenis FET Untuk Aplikasi Frekuensi Tinggi." },
  { "en": "Apa Kepanjangan Dari HEMT Dalam Elektronika?", "id": "High Electron Mobility Transistor Adalah Kepanjangannya." },
  { "en": "Material Apa Yang Digunakan Dalam Transistor HEMT?", "id": "Menggunakan Heterojunction Dua Material Semikonduktor Berbeda." },
  { "en": "Apa Itu Efek Gunn Pada Semikonduktor?", "id": "Pembangkitan Gelombang Mikro Frekuensi Sangat Tinggi." },
  { "en": "Apakah Transistor Merupakan Perangkat Resiprokal?", "id": "Tidak, Transistor Adalah Perangkat Non-Resiprokal." },
  { "en": "Apa Artinya Perangkat Non-Resiprokal Secara Umum?", "id": "Perilaku Berbeda Tergantung Arah Aliran Sinyal." },
  { "en": "Dapatkah Emitor Dan Kolektor Ditukar Posisinya?", "id": "Bisa, Tapi Penguatan Akan Sangat Rendah." },
  { "en": "Mode Operasi Ini Disebut Mode Apa?", "id": "Ini Disebut Sebagai Mode Operasi Terbalik." },
  { "en": "Apa Itu Gummel Plot Dalam Karakteristik Transistor?", "id": "Grafik Logaritmik Arus Terhadap Tegangan Basis-Emitor." },
  { "en": "Apa Itu Beta Degradation Pada Transistor?", "id": "Penurunan Penguatan Arus Seiring Waktu Pakai." },
  { "en": "Apa Itu Efek Popcorn Noise Pada Transistor?", "id": "Derau Frekuensi Rendah Dengan Transisi Mendadak." },
  { "en": "Apa Itu Model Giacoletto Untuk Transistor?", "id": "Nama Lain Untuk Model Hybrid-Pi Transistor." },
  { "en": "Apa Itu Bootstrap Pada Rangkaian Elektronika?", "id": "Teknik Umpan Balik Positif Untuk Meningkatkan Impedansi." },
  { "en": "Apa Itu Current Hogging Dalam Transistor Paralel?", "id": "Satu Transistor Mengambil Arus Lebih Banyak." },
  { "en": "Bagaimana Mencegah Terjadinya Masalah Current Hogging?", "id": "Menambahkan Resistor Kecil Pada Setiap Emitor." },
  { "en": "Apa Itu Hot Carrier Injection (HCI)?", "id": "Mekanisme Degradasi Pada Transistor Jenis MOSFET." },
  { "en": "Apa Akibat Dari Terjadinya Hot Carrier Injection?", "id": "Menggeser Tegangan Threshold Dan Mengurangi Kinerja." },
  { "en": "Apa Itu Body Effect Pada MOSFET?", "id": "Perubahan Tegangan Threshold Karena Tegangan Substrat." },
  { "en": "Apa Itu Channel Length Modulation (CLM)?", "id": "Efek Mirip Efek Awal Pada Transistor BJT." },
  { "en": "Apa Akibat Dari Channel Length Modulation?", "id": "Mengurangi Impedansi Output Transistor Jenis MOSFET." },
  { "en": "Apa Itu Gate-Induced Drain Leakage (GIDL)?", "id": "Arus Bocor Pada Kondisi Off-State MOSFET." },
  { "en": "Apa Itu Latch-up Pada Rangkaian CMOS?", "id": "Kondisi Hubungan Singkat Karena Struktur Thyristor." },
  { "en": "Apa Kepanjangan Dari CMOS Dalam Elektronika?", "id": "Complementary Metal-Oxide-Semiconductor Adalah Kepanjangannya." },
  { "en": "Apa Itu Gilbert Cell Dalam Dunia Elektronika?", "id": "Sirkuit Pencampur (Mixer) Berbasis Inti Transistor." },
  { "en": "Apa Fungsi Utama Dari Rangkaian Gilbert Cell?", "id": "Untuk Mengalikan Dua Sinyal Secara Analog." },
  { "en": "Apa Itu Long-Tailed Pair Dalam Elektronika?", "id": "Nama Lain Untuk Rangkaian Penguat Diferensial." },
  { "en": "Siapa Yang Mempopulerkan Long-Tailed Pair?", "id": "Dipopulerkan Oleh Alan Blumlein Dari Inggris." },
  { "en": "Apa Itu Kapasitor Bypass Pada Kaki Emitor?", "id": "Meningkatkan Penguatan AC Tanpa Mengubah Titik DC." },
  { "en": "Bagaimana Kapasitor Bypass Bekerja Meningkatkan Penguatan?", "id": "Melewatkan Sinyal AC Langsung Ke Ground." },
  { "en": "Apa Itu Kapasitor Kopling Pada Rangkaian Penguat?", "id": "Meneruskan Sinyal AC Dan Memblokir Sinyal DC." },
  { "en": "Mengapa Perlu Memblokir Komponen Sinyal DC?", "id": "Agar Tidak Mengganggu Kondisi Bias Rangkaian Berikutnya." },
  { "en": "Apa Itu Kelas Penguat (Amplifier) D?", "id": "Penguat Switching Dengan Efisiensi Sangat Tinggi." },
  { "en": "Bagaimana Cara Kerja Penguat Kelas D?", "id": "Menggunakan Pulse Width Modulation (PWM) Sinyal." },
  { "en": "Apa Kepanjangan Dari PWM Dalam Elektronika?", "id": "Pulse Width Modulation Adalah Kepanjangan Resminya." },
  { "en": "Apa Komponen Utama Pada Penguat Kelas D?", "id": "Transistor MOSFET Sebagai Saklar Berkecepatan Tinggi." },
  { "en": "Apa Fungsi Filter Output Penguat Kelas D?", "id": "Menghilangkan Komponen Frekuensi Tinggi Dari Sinyal." },
  { "en": "Apa Itu Integrated Injection Logic (I²L)?", "id": "Keluarga Logika Bipolar Dengan Kepadatan Tinggi." },
  { "en": "Apa Kepanjangan Dari I²L Dalam Elektronika?", "id": "Integrated Injection Logic Adalah Kepanjangan Resminya." },
  { "en": "Apa Itu Teknologi BiCMOS Dalam Elektronika?", "id": "Gabungan Teknologi Bipolar Dan CMOS Bersamaan." },
  { "en": "Apa Kepanjangan Dari BiCMOS Dalam Semikonduktor?", "id": "Bipolar Complementary Metal-Oxide-Semiconductor Adalah Kepanjangannya." },
  { "en": "Apa Keuntungan Utama Dari Teknologi BiCMOS?", "id": "Kecepatan Bipolar Dan Konsumsi Daya Rendah." },
  { "en": "Apa Itu Hambatan Termal (Thermal Resistance)?", "id": "Ukuran Hambatan Aliran Panas Pada Transistor." },
  { "en": "Apa Itu Hambatan Termal Junction-to-Case?", "id": "Hambatan Panas Dari Chip Ke Kemasan." },
  { "en": "Apa Itu Hambatan Termal Junction-to-Ambient?", "id": "Hambatan Panas Dari Chip Ke Udara." },
  { "en": "Regulator Apa Yang Menggunakan Transistor Pass?", "id": "Regulator Tegangan Linear Seperti Jenis LDO." },
  { "en": "Apa Kepanjangan LDO Dalam Rangkaian Regulator?", "id": "Low-Dropout Regulator Adalah Kepanjangan Resminya." },
  { "en": "Berapa Total Penurunan VBE Pasangan Darlington?", "id": "Sekitar Satu Koma Empat Volt (1.4V)." },
  { "en": "Mengapa Penurunan VBE Darlington Lebih Besar?", "id": "Karena Merupakan Jumlah Dari Dua Sambungan." },
  { "en": "Apa Keuntungan Dari Pasangan Sziklai Pair?", "id": "Memiliki Penurunan Tegangan VBE Lebih Rendah." },
  { "en": "Apa Itu VDMOS Dalam Power MOSFET?", "id": "Struktur Vertikal Dengan Aliran Arus Vertikal." },
  { "en": "Apa Kepanjangan VDMOS Pada Power MOSFET?", "id": "Vertical Double-Diffused Metal-Oxide-Semiconductor." },
  { "en": "Apa Itu LDMOS Dalam Power MOSFET?", "id": "Struktur Lateral Dengan Aliran Arus Lateral." },
  { "en": "Apa Kepanjangan LDMOS Pada Power MOSFET?", "id": "Laterally Diffused Metal-Oxide-Semiconductor Adalah Jawabannya." },
  { "en": "Apa Itu Semikonduktor Celah Pita Lebar?", "id": "Material Dengan Energi Celah Pita Besar." },
  { "en": "Sebutkan Contoh Material Semikonduktor Celah Lebar?", "id": "Silikon Karbida (SiC) Dan Galium Nitrida (GaN)." },
  { "en": "Apa Keuntungan Transistor Berbasis Galium Nitrida?", "id": "Efisiensi Dan Frekuensi Switching Sangat Tinggi." },
  { "en": "Apa Itu DIAC Dalam Keluarga Thyristor?", "id": "Saklar AC Dua Arah Tanpa Gerbang." },
  { "en": "Apa Kepanjangan Dari DIAC Dalam Elektronika?", "id": "Diode for Alternating Current Adalah Kepanjangannya." },
  { "en": "Apa Fungsi Utama Komponen Elektronika DIAC?", "id": "Sebagai Pemicu Untuk Komponen Elektronika TRIAC." },
  { "en": "Apa Itu Programmable Unijunction Transistor (PUT)?", "id": "Thyristor Dengan Anoda Gate Yang Dapat Diprogram." },
  { "en": "Bagaimana Cara Menguji Transistor Di Sirkuit?", "id": "Menggunakan Pelacak Kurva Atau Penganalisis Khusus." },
  { "en": "Apa Itu Transistor Bipolar Parasitik Pada CMOS?", "id": "Struktur BJT Yang Tidak Diinginkan Pada CMOS." },
  { "en": "Apa Akibat Aktivasi Transistor Bipolar Parasitik?", "id": "Menyebabkan Kondisi Latch-up Yang Merusak Sirkuit." },
  { "en": "Apa Itu Arus Bocor Sub-Ambang (Subthreshold)?", "id": "Arus Bocor Dari Drain Ke Source." },
  { "en": "Kapan Arus Bocor Sub-Ambang Terjadi Biasanya?", "id": "Saat Tegangan Gate Dibawah Tegangan Threshold." },
  { "en": "Apa Itu Arus Bocor Gerbang (Gate Leakage)?", "id": "Arus Yang Bocor Melalui Lapisan Insulator." },
  { "en": "Apa Itu Tegangan Early (Early Voltage)?", "id": "Parameter Yang Berhubungan Dengan Efek Awal." },
  { "en": "Nilai Tegangan Early Ideal Itu Berapa?", "id": "Nilai Idealnya Adalah Tak Terhingga Besarnya." },
  { "en": "Apa Itu Frekuensi Transisi (fT) Transistor?", "id": "Frekuensi Dimana Penguatan Arus Menjadi Satu." },
  { "en": "Apa Nama Lain Untuk Frekuensi Transisi?", "id": "Nama Lainnya Gain-Bandwidth Product Yang Populer." },
  { "en": "Apa Itu Frekuensi Beta Cutoff (fβ)?", "id": "Frekuensi Dimana Penguatan Arus Turun 3dB." },
  { "en": "Bagaimana Kondisi Sambungan BJT Di Saturasi?", "id": "Basis-Emitor Dan Basis-Kolektor Berbias Maju." },
  { "en": "Bagaimana Kondisi Sambungan BJT Di Cut-off?", "id": "Basis-Emitor Dan Basis-Kolektor Berbias Mundur." },
  { "en": "Bagaimana Kondisi Sambungan BJT Di Aktif?", "id": "Basis-Emitor Maju Dan Basis-Kolektor Mundur." },
  { "en": "Apa Itu Waktu Tunda Penyimpanan (Storage Delay)?", "id": "Waktu Untuk Menghilangkan Muatan Dari Basis." },
  { "en": "Kapan Waktu Tunda Penyimpanan Terjadi Signifikan?", "id": "Saat Transistor Keluar Dari Daerah Saturasi." },
  { "en": "Apa Itu Sirkuit Schmitt Trigger Dengan Transistor?", "id": "Rangkaian Pembanding Dengan Umpan Balik Positif." },
  { "en": "Apa Fungsi Utama Rangkaian Schmitt Trigger?", "id": "Mengubah Sinyal Analog Menjadi Sinyal Digital." },
  { "en": "Apa Itu Histeresis Pada Schmitt Trigger?", "id": "Perbedaan Antara Tegangan Ambang Atas Bawah." },
  { "en": "Apa Itu Osilator Relaksasi Dengan Transistor?", "id": "Osilator Yang Menghasilkan Bentuk Gelombang Non-Sinusoidal." },
  { "en": "Apa Itu Multivibrator Astabil Berbasis Transistor?", "id": "Osilator Tanpa Status Stabil Sama Sekali." },
  { "en": "Apa Fungsi Multivibrator Astabil Pada Umumnya?", "id": "Menghasilkan Gelombang Kotak Secara Terus Menerus." },
  { "en": "Apa Itu Multivibrator Monostabil Berbasis Transistor?", "id": "Rangkaian Dengan Satu Status Stabil Saja." },
  { "en": "Apa Nama Lain Multivibrator Monostabil Populer?", "id": "Nama Lainnya Adalah Rangkaian One-Shot Pulse." },
  { "en": "Apa Itu Multivibrator Bistabil Berbasis Transistor?", "id": "Rangkaian Dengan Dua Status Stabil Sekaligus." },
  { "en": "Apa Nama Lain Multivibrator Bistabil Populer?", "id": "Nama Lainnya Adalah Rangkaian Flip-Flop." },
  { "en": "Apa Itu Dioda Varactor Atau Varicap?", "id": "Dioda Dengan Kapasitansi Yang Dapat Diubah." },
  { "en": "Bagaimana Transistor Digunakan Dalam Voltage-Controlled Oscillator?", "id": "Sebagai Elemen Penguat Dalam Rangkaian Osilator." },
  { "en": "Apa Kepanjangan VCO Dalam Dunia Elektronika?", "id": "Voltage-Controlled Oscillator Adalah Kepanjangan Resminya." },
  { "en": "Apa Itu Efek Tunneling Kuantum Transistor?", "id": "Efek Kuantum Yang Menyebabkan Arus Bocor." },
  { "en": "Apa Itu Ballistic Conduction Dalam Transistor?", "id": "Elektron Bergerak Tanpa Hamburan Sama Sekali." },
  { "en": "Apa Itu Silicon On Insulator (SOI)?", "id": "Teknologi Fabrikasi Untuk Mengurangi Kapasitansi Parasitik." },
  { "en": "Apa Kepanjangan SOI Dalam Dunia Semikonduktor?", "id": "Silicon On Insulator Adalah Kepanjangan Resminya." },
  { "en": "Apa Itu Penguat Kelas E Dalam Elektronika?", "id": "Penguat Switching Frekuensi Radio Efisiensi Tinggi." },
  { "en": "Apa Itu Penguat Kelas F Dalam Elektronika?", "id": "Penyempurnaan Dari Penguat Switching Kelas E." },
  { "en": "Apa Itu Efek Proksimitas Pada Semikonduktor?", "id": "Efek Interaksi Antara Komponen Yang Berdekatan." },
  { "en": "Apa Itu Hot-Electron Clocking Dalam Logika?", "id": "Teknik Untuk Mengurangi Disipasi Daya Statis." },
  { "en": "Apa Itu Velocity Saturation Pada Transistor?", "id": "Kondisi Kecepatan Pembawa Muatan Menjadi Konstan." },
  { "en": "Apa Pengaruh Dari Velocity Saturation Tersebut?", "id": "Membatasi Arus Dan Kinerja Frekuensi Tinggi." },
  { "en": "Apa Itu Sirkuit Totem-Pole Dalam Keluarga TTL?", "id": "Konfigurasi Output Untuk Menurunkan Impedansi Output." },
  { "en": "Apa Kelemahan Dari Sirkuit Totem-Pole TTL?", "id": "Menghasilkan Lonjakan Arus Saat Berganti Status." },
  { "en": "Apa Itu Open Collector Output Transistor?", "id": "Kolektor Tidak Terhubung Ke Komponen Apapun." },
  { "en": "Apa Keuntungan Konfigurasi Open Collector Output?", "id": "Dapat Dihubungkan Bersama Untuk Logika Wired-AND." },
  { "en": "Apa Itu Resistor Pull-Up Yang Diperlukan?", "id": "Resistor Eksternal Untuk Menentukan Level Tinggi." },
  { "en": "Apa Itu Power Transistor Co-Packaging?", "id": "Mengemas Transistor Dengan Dioda Freewheeling Bersama." },
  { "en": "Apa Tujuan Utama Dari Proses Co-Packaging?", "id": "Menghemat Ruang Dan Mengoptimalkan Kinerja Termal." },
  { "en": "Apa Itu Efek Hall Dalam Semikonduktor?", "id": "Timbulnya Tegangan Melintang Akibat Medan Magnet." },
  { "en": "Apa Itu Gerbang Logika Pass-Transistor?", "id": "Logika Yang Melewatkan Sinyal Menggunakan Transistor." },
  { "en": "Apa Kelemahan Utama Gerbang Logika Pass-Transistor?", "id": "Terjadi Penurunan Tegangan Pada Level Logika." },
  { "en": "Apa Itu Logika Transmisi Gerbang (Transmission Gate)?", "id": "Saklar Elektronik Menggunakan CMOS N-Channel P-Channel." },
  { "en": "Apa Keuntungan Utama Logika Transmisi Gerbang?", "id": "Tidak Ada Penurunan Tegangan Pada Logika." },
  { "en": "Apa Itu Scaling Pada Teknologi Transistor?", "id": "Proses Pengecilan Dimensi Ukuran Transistor Modern." },
  { "en": "Siapa Yang Mengusulkan Aturan Scaling Pertama?", "id": "Aturan Penskalaan Diusulkan Oleh Robert H. Dennard." },
  { "en": "Apa Itu Short-Channel Effects (SCEs)?", "id": "Efek Negatif Akibat Pengecilan Kanal MOSFET." },
  { "en": "Sebutkan Contoh Dari Efek Short-Channel?", "id": "Drain-Induced Barrier Lowering Atau Biasa Disingkat DIBL." },
  { "en": "Apa Itu Drain-Induced Barrier Lowering (DIBL)?", "id": "Penurunan Energi Pembatas Akibat Tegangan Drain." },
  { "en": "Bagaimana Transistor Mengalami Kegagalan Latch-up?", "id": "Terpicunya Struktur SCR Parasitik Dalam CMOS." },
  { "en": "Apa Itu Guard Ring Dalam Desain IC?", "id": "Struktur Untuk Mencegah Terjadinya Latch-up Merusak." },
  { "en": "Apa Itu Phase-Locked Loop (PLL) Sirkuit?", "id": "Sistem Umpan Balik Untuk Sinkronisasi Sinyal." },
  { "en": "Apa Kepanjangan PLL Dalam Dunia Elektronika?", "id": "Phase-Locked Loop Adalah Kepanjangan Resminya." },
  { "en": "Apa Peran Transistor Dalam Sirkuit PLL?", "id": "Sebagai Komponen Utama Dalam VCO Dan Detektor Fasa." },
  { "en": "Apa Itu Organic Light-Emitting Diode (OLED)?", "id": "LED Berbasis Senyawa Organik Yang Bercahaya." },
  { "en": "Apa Itu Organic Field-Effect Transistor (OFET)?", "id": "Transistor FET Menggunakan Material Semikonduktor Organik." },
  { "en": "Apa Keuntungan Potensial Dari Elektronika Organik?", "id": "Fleksibel, Transparan, Dan Biaya Produksi Murah." },
  { "en": "Apa Itu Penguat Chopper-Stabilized Atau Chopper?", "id": "Penguat Untuk Mengurangi Offset Dan Drift DC." },
  { "en": "Bagaimana Cara Kerja Penguat Chopper Sebenarnya?", "id": "Memodulasi Sinyal Input Ke Frekuensi Tinggi." },
  { "en": "Apa Itu Auto-Zeroing Dalam Rangkaian Penguat?", "id": "Teknik Untuk Mengkalibrasi Dan Menghilangkan Offset." },
  { "en": "Apa Itu Floating Gate MOSFET Dalam Elektronika?", "id": "MOSFET Dengan Gerbang Terisolasi Secara Elektrik." },
  { "en": "Di Mana Floating Gate MOSFET Digunakan?", "id": "Dalam Sel Memori Non-Volatil Seperti Flash." },
  { "en": "Bagaimana Data Disimpan Dalam Floating Gate?", "id": "Dengan Menjebak Atau Melepas Elektron Didalamnya." },
  { "en": "Apa Itu Sel Memori SRAM Enam Transistor?", "id": "Sel Memori Statis Menggunakan Enam Transistor." },
  { "en": "Apa Kepanjangan SRAM Dalam Dunia Memori?", "id": "Static Random-Access Memory Adalah Kepanjangannya." },
  { "en": "Apa Fungsi Transistor Dalam Sel Memori DRAM?", "id": "Sebagai Saklar Untuk Mengakses Muatan Kapasitor." },
  { "en": "Apa Kepanjangan DRAM Dalam Dunia Memori?", "id": "Dynamic Random-Access Memory Adalah Kepanjangannya." },
  { "en": "Apa Itu Thin-Film Transistor (TFT)?", "id": "Transistor Dibuat Dengan Menumpuk Lapisan Tipis." },
  { "en": "Di Mana Teknologi Thin-Film Transistor Digunakan?", "id": "Pada Layar Liquid Crystal Display (LCD)." },
  { "en": "Apa Itu Proses Fotolitografi Dalam Fabrikasi?", "id": "Proses Transfer Pola Menggunakan Paparan Cahaya." },
  { "en": "Apa Itu Proses Etching Dalam Fabrikasi?", "id": "Proses Pengikisan Material Yang Tidak Terlindungi." },
  { "en": "Apa Itu Proses Implantasi Ion Fabrikasi?", "id": "Proses Menembakkan Ion Untuk Doping Semikonduktor." },
  { "en": "Apa Itu Tunnel Field-Effect Transistor (TFET)?", "id": "Transistor Berbasis Mekanisme Terowongan Kuantum." },
  { "en": "Apa Keuntungan Potensial Dari Transistor TFET?", "id": "Konsumsi Daya Sangat Rendah Dan Cepat." },
  { "en": "Apa Itu Elektromigrasi Dalam Sirkuit Terpadu?", "id": "Perpindahan Atom Logam Akibat Aliran Elektron." },
  { "en": "Apa Akibat Buruk Dari Proses Elektromigrasi?", "id": "Menyebabkan Hubungan Terbuka Atau Hubungan Singkat." },
  { "en": "Apa Itu Time-Dependent Dielectric Breakdown (TDDB)?", "id": "Kegagalan Lapisan Insulator Seiring Waktu Pakai." },
  { "en": "Lapisan Mana Yang Paling Rentan TDDB?", "id": "Lapisan Oksida Gerbang (Gate Oxide) MOSFET." },
  { "en": "Apa Itu Parameter-S (Scattering Parameters)?", "id": "Parameter Untuk Mengkarakterisasi Jaringan Frekuensi Tinggi." },
  { "en": "Untuk Apa Parameter-S Digunakan Pada Transistor?", "id": "Menganalisis Kinerja Penguatan Dan Pencocokan Impedansi." },
  { "en": "Apa Itu Pengukuran Load-Pull Untuk Transistor?", "id": "Metode Karakterisasi Kinerja Transistor Daya RF." },
  { "en": "Apa Itu Kebisingan Substrat (Substrate Noise)?", "id": "Gangguan Listrik Yang Merambat Melalui Substrat." },
  { "en": "Bagaimana Kebisingan Substrat Dapat Mempengaruhi Sirkuit?", "id": "Dapat Mengganggu Kinerja Sirkuit Analog Sensitif." },
  { "en": "Apa Itu Frekuensi Osilasi Maksimum (fmax)?", "id": "Frekuensi Dimana Penguatan Daya Menjadi Satu." },
  { "en": "Apa Indikator Kinerja Yang Diberikan fmax?", "id": "Indikator Potensi Transistor Untuk Aplikasi Osilator." },
  { "en": "Apa Itu Titik Intersep Orde Ketiga (IP3)?", "id": "Ukuran Linearitas Sebuah Penguat Atau Mixer." },
  { "en": "Nilai IP3 Yang Baik Itu Tinggi Atau Rendah?", "id": "Nilai Yang Sangat Tinggi Dianggap Baik." },
  { "en": "Apa Itu Kompresi Penguatan (Gain Compression)?", "id": "Penurunan Penguatan Pada Daya Input Tinggi." },
  { "en": "Apa Itu Titik Kompresi 1-dB (P1dB)?", "id": "Daya Input Dimana Penguatan Turun 1dB." },
  { "en": "Apa Itu Penguat Terdistribusi (Distributed Amplifier)?", "id": "Penguat Untuk Mencapai Lebar Pita Sangat Lebar." },
  { "en": "Apa Itu Penguat Lock-In Berbasis Transistor?", "id": "Instrumen Untuk Mengukur Sinyal Sangat Kecil." },
  { "en": "Apa Itu Negative Differential Resistance (NDR)?", "id": "Kondisi Dimana Arus Menurun Saat Tegangan Naik." },
  { "en": "Sebutkan Perangkat Yang Menunjukkan Sifat NDR?", "id": "Tunnel Diode Dan Juga Gunn Diode." },
  { "en": "Apa Itu Efek Peltier Dalam Semikonduktor?", "id": "Pemanasan Atau Pendinginan Sambungan Akibat Arus." },
  { "en": "Apa Itu Efek Seebeck Dalam Semikonduktor?", "id": "Timbulnya Tegangan Akibat Perbedaan Suhu." },
  { "en": "Apa Itu Termokopel Berbasis Sambungan Semikonduktor?", "id": "Sensor Suhu Yang Memanfaatkan Efek Seebeck." },
  { "en": "Apa Itu Pass Transistor Dalam Regulator Tegangan?", "id": "Transistor Yang Mengatur Aliran Arus Beban." },
  { "en": "Apa Itu Crowbar Circuit Dengan Thyristor?", "id": "Rangkaian Proteksi Terhadap Tegangan Lebih (Overvoltage)." },
  { "en": "Bagaimana Cara Kerja Rangkaian Proteksi Crowbar?", "id": "Membuat Hubungan Singkat Untuk Memutuskan Sekring." },
  { "en": "Apa Itu Zero Crossing Detector Dengan Transistor?", "id": "Rangkaian Pendeteksi Saat Sinyal AC Melintasi Nol." },
  { "en": "Kapan Zero Crossing Detector Sangat Dibutuhkan?", "id": "Pada Rangkaian Pengontrol Fasa Seperti Dimmer." },
  { "en": "Apa Itu Gallium Arsenide (GaAs) FET?", "id": "FET Yang Dibuat Dari Material Gallium Arsenide." },
  { "en": "Apa Keuntungan Utama Dari Transistor GaAs?", "id": "Kecepatan Elektron Tinggi Dan Kebisingan Rendah." },
  { "en": "Apa Kerugian Utama Dari Transistor GaAs?", "id": "Biaya Produksi Mahal Dan Konduktivitas Termal Rendah." },
  { "en": "Apa Itu Indium Phosphide (InP) Transistor?", "id": "Transistor Untuk Kinerja Frekuensi Lebih Tinggi." },
  { "en": "Apa Itu Silicon Germanium (SiGe) Transistor?", "id": "Transistor BJT Dengan Penambahan Germanium Di Basis." },
  { "en": "Apa Tujuan Penambahan Germanium Pada Transistor SiGe?", "id": "Meningkatkan Kinerja Frekuensi Tinggi Secara Signifikan." },
  { "en": "Apa Itu Constant-Current Source Dengan Transistor?", "id": "Rangkaian Yang Memberikan Arus Beban Konstan." },
  { "en": "Apa Itu Constant-Voltage Source Dengan Transistor?", "id": "Rangkaian Yang Memberikan Tegangan Beban Konstan." },
  { "en": "Apa Itu Penguat Jembatan (Bridge Amplifier)?", "id": "Konfigurasi Empat Penguat Untuk Daya Maksimal." },
  { "en": "Di Mana Konfigurasi Penguat Jembatan Digunakan?", "id": "Pada Penguat Audio Mobil Daya Sangat Tinggi." },
  { "en": "Apa Itu Gyrator Circuit Dengan Transistor?", "id": "Rangkaian Yang Mensimulasikan Perilaku Sebuah Induktor." },
  { "en": "Mengapa Perlu Mensimulasikan Sebuah Komponen Induktor?", "id": "Karena Induktor Sulit Diintegrasikan Dalam IC." },
  { "en": "Apa Itu Negative Impedance Converter (NIC)?", "id": "Rangkaian Yang Menghasilkan Impedansi Bernilai Negatif." },
  { "en": "Apa Kepanjangan NIC Dalam Rangkaian Aktif?", "id": "Negative Impedance Converter Adalah Kepanjangannya." },
  { "en": "Apa Itu Bandgap Voltage Reference Sirkuit?", "id": "Sirkuit Yang Menghasilkan Tegangan Referensi Stabil." },
  { "en": "Apa Prinsip Kerja Bandgap Voltage Reference?", "id": "Menggabungkan Tegangan Yang Berlawanan Koefisien Suhu." },
  { "en": "Apa Itu Doping Retrograde Pada Profil MOSFET?", "id": "Konsentrasi Doping Tinggi Jauh Dari Permukaan." },
  { "en": "Apa Tujuan Dari Proses Doping Retrograde?", "id": "Untuk Menekan Efek Kanal Pendek (Short-Channel)." },
  { "en": "Apa Itu Efek Tembok Pendek (Narrow-Width Effect)?", "id": "Perubahan Tegangan Threshold Pada Kanal Sempit." },
  { "en": "Apa Itu Local Oxidation of Silicon (LOCOS)?", "id": "Metode Isolasi Antar Perangkat Dalam IC." },
  { "en": "Apa Kepanjangan LOCOS Dalam Fabrikasi Semikonduktor?", "id": "Local Oxidation of Silicon Adalah Kepanjangannya." },
  { "en": "Apa Itu Shallow Trench Isolation (STI)?", "id": "Metode Isolasi Yang Lebih Modern Canggih." },
  { "en": "Apa Kepanjangan STI Dalam Fabrikasi Semikonduktor?", "id": "Shallow Trench Isolation Adalah Kepanjangannya." },
  { "en": "Apa Itu Dioda Terkendali Silikon (SCS)?", "id": "Thyristor Dengan Empat Terminal Termasuk Gerbang Anoda." },
  { "en": "Apa Kepanjangan SCS Dalam Keluarga Thyristor?", "id": "Silicon Controlled Switch Adalah Kepanjangannya." },
  { "en": "Apa Itu Waktu Tunda Propagasi Gerbang Logika?", "id": "Waktu Antara Perubahan Input Dan Respon Output." },
  { "en": "Apa Itu Power-Delay Product (PDP)?", "id": "Ukuran Efisiensi Energi Sebuah Gerbang Logika." },
  { "en": "Nilai PDP Yang Baik Itu Tinggi Atau Rendah?", "id": "Nilai Yang Sangat Rendah Dianggap Baik." },
  { "en": "Apa Itu Fan-Out Dalam Gerbang Logika?", "id": "Jumlah Maksimum Input Yang Dapat Digerakkan." },
  { "en": "Apa Itu Fan-In Dalam Gerbang Logika?", "id": "Jumlah Input Pada Sebuah Gerbang Logika." },
  { "en": "Apa Itu Noise Margin Dalam Level Logika?", "id": "Toleransi Level Tegangan Terhadap Gangguan Derau." },
  { "en": "Apa Itu Level Logika High (VOH)?", "id": "Tegangan Output Minimum Untuk Logika Tinggi." },
  { "en": "Apa Itu Level Logika Low (VOL)?", "id": "Tegangan Output Maksimum Untuk Logika Rendah." },
  { "en": "Apa Itu Level Logika Input High (VIH)?", "id": "Tegangan Input Minimum Dianggap Logika Tinggi." },
  { "en": "Apa Itu Level Logika Input Low (VIL)?", "id": "Tegangan Input Maksimum Dianggap Logika Rendah." },
  { "en": "Apa Itu Level Shifter Circuit Dengan Transistor?", "id": "Rangkaian Untuk Mengubah Level Tegangan Logika." },
  { "en": "Kapan Rangkaian Level Shifter Sangat Diperlukan?", "id": "Saat Menghubungkan Keluarga Logika Yang Berbeda." },
  { "en": "Apa Itu Transistor Karbon Nanotube (CNTFET)?", "id": "FET Yang Menggunakan Karbon Nanotube Sebagai Kanal." },
  { "en": "Apa Potensi Keunggulan Transistor Karbon Nanotube?", "id": "Ukuran Sangat Kecil Dan Kinerja Tinggi." },
  { "en": "Apa Itu Spintronics Atau Spin Electronics?", "id": "Bidang Elektronik Yang Memanfaatkan Spin Elektron." },
  { "en": "Apa Itu Spin Transistor Dalam Dunia Spintronics?", "id": "Transistor Yang Mengontrol Arus Spin Elektron." },
  { "en": "Apa Itu Efek Piezoelektrik Pada Semikonduktor?", "id": "Timbulnya Tegangan Akibat Tekanan Mekanis." },
  { "en": "Apa Itu Surface Acoustic Wave (SAW) Filter?", "id": "Filter Frekuensi Berbasis Efek Piezoelektrik." },
  { "en": "Apa Peran Transistor Dalam Rangkaian SAW?", "id": "Sebagai Penguat Untuk Sinyal Yang Dilemahkan." },
  { "en": "Apa Itu Wafer Dalam Industri Semikonduktor?", "id": "Kepingan Tipis Material Semikonduktor Murni." },
  { "en": "Apa Itu Die Dalam Industri Semikonduktor?", "id": "Satu Potongan Kecil Sirkuit Terpadu Individual." },
  { "en": "Apa Itu Proses Dicing Dalam Fabrikasi IC?", "id": "Proses Pemotongan Wafer Menjadi Die Individual." },
  { "en": "Apa Itu Proses Wire Bonding Dalam Pengemasan?", "id": "Menghubungkan Die Ke Kaki Kemasan IC." },
  { "en": "Apa Itu Flip-Chip Bonding Dalam Pengemasan?", "id": "Metode Menghubungkan Die Langsung Ke Papan." },
  { "en": "Apa Itu Ball Grid Array (BGA) Package?", "id": "Jenis Kemasan Dengan Bola Solder Dibawahnya." },
  { "en": "Apa Kepanjangan BGA Dalam Kemasan Elektronik?", "id": "Ball Grid Array Adalah Kepanjangan Resminya." },
  { "en": "Apa Itu Class G Amplifier Atau Penguat?", "id": "Penguat Dengan Rel Catu Daya Ganda." },
  { "en": "Apa Tujuan Penggunaan Penguat Kelas G?", "id": "Meningkatkan Efisiensi Dibandingkan Dengan Kelas AB." },
  { "en": "Apa Itu Class H Amplifier Atau Penguat?", "id": "Penguat Yang Memodulasi Rel Catu Dayanya." },
  { "en": "Apa Itu Envelope Tracking Dalam Penguat RF?", "id": "Teknik Menyesuaikan Catu Daya Dengan Sinyal." },
  { "en": "Apa Tujuan Utama Dari Envelope Tracking?", "id": "Meningkatkan Efisiensi Penguat Daya Frekuensi Radio." },
  { "en": "Apa Itu Model Transistor Gummel-Poon BJT?", "id": "Model Fisik Lengkap Untuk Simulasi Komputer." },
  { "en": "Apa Itu Model Transistor BSIM MOSFET?", "id": "Model Standar Industri Untuk Simulasi MOSFET." },
  { "en": "Apa Kepanjangan BSIM Dalam Model Transistor?", "id": "Berkeley Short-channel IGFET Model Adalah Kepanjangannya." },
  { "en": "Apa Itu Simulasi SPICE Dalam Dunia Elektronika?", "id": "Program Simulasi Rangkaian Elektronik Analog Populer." },
  { "en": "Apa Kepanjangan SPICE Dalam Dunia Simulasi?", "id": "Simulation Program with Integrated Circuit Emphasis." },
  { "en": "Apa Itu Ekstraksi Parameter Transistor Modern?", "id": "Proses Mendapatkan Parameter Model Dari Data." },
  { "en": "Apa Itu Efek Kuantum Konfinemen (Quantum Confinement)?", "id": "Perubahan Sifat Elektronik Pada Skala Nanometer." },
  { "en": "Apa Itu Efek Velocity Overshoot Elektron?", "id": "Elektron Bergerak Lebih Cepat Dari Kecepatan Saturasi." },
  { "en": "Apa Itu Efek Polysilicon Depletion MOSFET?", "id": "Terbentuknya Daerah Deplesi Pada Gerbang Polisilikon." },
  { "en": "Apa Akibat Buruk Polysilicon Depletion Effect?", "id": "Mengurangi Arus Drive Dan Kinerja Transistor." },
  { "en": "Apa Itu Electrostatic Discharge (ESD)?", "id": "Pelepasan Listrik Statis Yang Merusak Sirkuit." },
  { "en": "Apa Kepanjangan ESD Dalam Dunia Elektronika?", "id": "Electrostatic Discharge Adalah Kepanjangan Resminya." },
  { "en": "Bagaimana Cara Melindungi Transistor Dari ESD?", "id": "Menggunakan Rangkaian Proteksi Khusus Pada Input." },
  { "en": "Apa Itu Antenna Effect Dalam Fabrikasi IC?", "id": "Penumpukan Muatan Yang Merusak Gerbang Transistor." },
  { "en": "Kapan Antenna Effect Dapat Terjadi Umumnya?", "id": "Selama Proses Etching Plasma Dalam Fabrikasi." },
  { "en": "Apa Itu Current Conveyor (CC) Sirkuit?", "id": "Blok Bangunan Sirkuit Analog Serbaguna Modern." },
  { "en": "Apa Itu Operational Transconductance Amplifier (OTA)?", "id": "Penguat Dengan Output Arus Terkontrol Tegangan." },
  { "en": "Apa Kepanjangan OTA Dalam Dunia Analog?", "id": "Operational Transconductance Amplifier Adalah Kepanjangannya." },
  { "en": "Apa Itu Ion-Sensitive FET (ISFET)?", "id": "Transistor FET Untuk Mengukur Konsentrasi Ion." },
  { "en": "Di Mana Transistor ISFET Biasa Digunakan?", "id": "Sebagai Sensor Kimia Atau Sensor pH." },
  { "en": "Bagaimana Transistor Digunakan Sebagai Sensor Tekanan?", "id": "Memanfaatkan Efek Piezoresistif Pada Semikonduktor Silikon." },
  { "en": "Apa Itu Elektronika Kriogenik (Cryogenic Electronics)?", "id": "Studi Perilaku Komponen Pada Suhu Rendah." },
  { "en": "Bagaimana Kinerja Transistor Pada Suhu Rendah?", "id": "Mobilitas Pembawa Muatan Umumnya Akan Meningkat." },
  { "en": "Apa Itu Freeze-Out Pada Suhu Kriogenik?", "id": "Atom Dopant Menjadi Tidak Aktif Lagi." },
  { "en": "Apa Itu Radiasi Pengion (Ionizing Radiation)?", "id": "Radiasi Yang Dapat Melepas Elektron Dari Atom." },
  { "en": "Apa Efek Radiasi Terhadap Kinerja Transistor?", "id": "Dapat Menyebabkan Kegagalan Permanen Atau Sementara." },
  { "en": "Apa Itu Single Event Upset (SEU)?", "id": "Perubahan Status Bit Akibat Partikel Tunggal." },
  { "en": "Apa Itu Radiation Hardening By Design (RHBD)?", "id": "Teknik Desain Untuk Membuat Sirkuit Tahan Radiasi." },
  { "en": "Apa Itu Logika Domino Dalam Desain CMOS?", "id": "Jenis Logika Dinamis Untuk Sirkuit Digital." },
  { "en": "Apa Keuntungan Utama Dari Penggunaan Logika Domino?", "id": "Ukuran Lebih Kecil Dan Kecepatan Tinggi." },
  { "en": "Apa Kelemahan Utama Dari Penggunaan Logika Domino?", "id": "Hanya Dapat Mengimplementasikan Logika Non-Inverting." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Kecenderungan Arus AC Mengalir Di Permukaan." },
  { "en": "Kapan Skin Effect Menjadi Sangat Signifikan?", "id": "Pada Frekuensi Yang Sangat Tinggi Sekali." },
  { "en": "Apa Itu Induktansi Parasitik Pada Kaki Komponen?", "id": "Induktansi Yang Tidak Diinginkan Pada Kaki IC." },
  { "en": "Apa Akibat Buruk Dari Induktansi Parasitik?", "id": "Menyebabkan Ground Bounce Dan Derau Catu Daya." },
  { "en": "Apa Itu Ground Bounce Dalam Sirkuit Digital?", "id": "Fluktuasi Tegangan Pada Jalur Ground Referensi." },
  { "en": "Apa Itu Kapasitor Dekopling Atau Decoupling?", "id": "Kapasitor Untuk Menstabilkan Tegangan Catu Daya." },
  { "en": "Di Mana Kapasitor Dekopling Seharusnya Diletakkan?", "id": "Sedekat Mungkin Dengan Kaki Catu Daya IC." },
  { "en": "Apa Itu Bistabilitas Pada Rangkaian Dengan Transistor?", "id": "Kemampuan Rangkaian Memiliki Dua Keadaan Stabil." },
  { "en": "Rangkaian Apa Yang Menunjukkan Sifat Bistabilitas?", "id": "Rangkaian Flip-Flop Atau Latch Adalah Contohnya." },
  { "en": "Apa Itu Keadaan Metastabil Dalam Flip-Flop?", "id": "Keadaan Tidak Stabil Antara Logika 0 Dan 1." },
  { "en": "Apa Itu Waktu Setup (Setup Time)?", "id": "Waktu Data Harus Stabil Sebelum Clock." },
  { "en": "Apa Itu Waktu Tahan (Hold Time)?", "id": "Waktu Data Harus Stabil Setelah Clock." },
  { "en": "Apa Itu Clock Skew Dalam Sirkuit Digital?", "id": "Perbedaan Waktu Kedatangan Sinyal Clock." },
  { "en": "Apa Itu Jitter Dalam Sinyal Clock?", "id": "Variasi Periodik Sinyal Clock Dari Ideal." },
  { "en": "Apa Itu Phase Noise Dalam Osilator?", "id": "Representasi Jitter Dalam Domain Frekuensi Osilator." },
  { "en": "Apa Itu Rangkaian Snubber Untuk Transistor?", "id": "Rangkaian Untuk Menekan Lonjakan Tegangan Berlebih." },
  { "en": "Apa Komponen Utama Dalam Rangkaian Snubber?", "id": "Umumnya Terdiri Dari Resistor Dan Kapasitor." },
  { "en": "Apa Itu Penguat Parametrik (Parametric Amplifier)?", "id": "Penguat Berbasis Variasi Parameter Reaktansi." },
  { "en": "Apa Keuntungan Utama Dari Penguat Parametrik?", "id": "Memiliki Angka Kebisingan (Noise) Sangat Rendah." },
  { "en": "Apa Itu Multiplier Frekuensi Dengan Transistor?", "id": "Rangkaian Untuk Menghasilkan Frekuensi Kelipatan Harmonik." },
  { "en": "Bagaimana Transistor Menciptakan Sinyal Harmonik?", "id": "Dengan Mengoperasikannya Dalam Mode Non-Linear." },
  { "en": "Apa Itu Power Combining Dalam Penguat RF?", "id": "Menggabungkan Output Beberapa Transistor Daya Rendah." },
  { "en": "Sebutkan Contoh Teknik Power Combining Populer?", "id": "Teknik Wilkinson Power Combiner Dan Divider." },
  { "en": "Apa Itu Doherty Amplifier Untuk Penguat RF?", "id": "Konfigurasi Penguat Untuk Meningkatkan Efisiensi Rata-rata." },
  { "en": "Bagaimana Cara Kerja Penguat Doherty Sebenarnya?", "id": "Menggunakan Penguat Utama Dan Penguat Puncak." },
  { "en": "Apa Itu Linearization Dalam Penguat Daya RF?", "id": "Teknik Untuk Mengurangi Distorsi Pada Penguat." },
  { "en": "Sebutkan Contoh Teknik Linearization Penguat Modern?", "id": "Teknik Predistorsi Digital (Digital Pre-Distortion)." },
  { "en": "Apa Itu Intermodulasi Distorsi (Intermodulation Distortion)?", "id": "Munculnya Sinyal Yang Tidak Diinginkan." },
  { "en": "Kapan Intermodulasi Distorsi Umumnya Terjadi?", "id": "Saat Dua Sinyal Melewati Perangkat Non-Linear." },
  { "en": "Apa Itu Dual-Gate MOSFET Dalam Elektronika?", "id": "MOSFET Yang Memiliki Dua Gerbang Kontrol." },
  { "en": "Apa Aplikasi Utama Dari Dual-Gate MOSFET?", "id": "Mixer, Automatic Gain Control (AGC), Dan Demodulator." },
  { "en": "Apa Itu Cascode Dengan Konfigurasi Folded?", "id": "Varian Rangkaian Cascode Untuk Rentang Tegangan." },
  { "en": "Apa Keuntungan Utama Rangkaian Folded Cascode?", "id": "Memungkinkan Operasi Pada Tegangan Catu Rendah." },
  { "en": "Apa Itu Self-Heating Effect Pada Transistor?", "id": "Peningkatan Suhu Chip Akibat Disipasi Daya." },
  { "en": "Apa Akibat Buruk Self-Heating Effect?", "id": "Dapat Mendegradasi Kinerja Dan Keandalan Transistor." },
  { "en": "Apa Itu Koefisien Temperatur Positif (PTC)?", "id": "Hambatan Meningkat Seiring Dengan Kenaikan Suhu." },
  { "en": "Apa Itu Koefisien Temperatur Negatif (NTC)?", "id": "Hambatan Menurun Seiring Dengan Kenaikan Suhu." },
  { "en": "Apakah Transistor BJT Memiliki Sifat PTC?", "id": "Tidak, BJT Memiliki Karakteristik Temperatur Negatif." },
  { "en": "Apa Itu Hot Spot Pada Die Transistor?", "id": "Area Lokal Dengan Suhu Jauh Lebih Tinggi." },
  { "en": "Apa Itu Epitaxy Dalam Proses Fabrikasi?", "id": "Proses Pertumbuhan Lapisan Kristal Tunggal Tipis." },
  { "en": "Apa Itu Chemical Vapor Deposition (CVD)?", "id": "Proses Untuk Menumbuhkan Lapisan Tipis Material." },
  { "en": "Apa Kepanjangan CVD Dalam Proses Fabrikasi?", "id": "Chemical Vapor Deposition Adalah Kepanjangannya." },
  { "en": "Apa Itu Sputtering Dalam Proses Fabrikasi?", "id": "Metode Deposisi Fisik Untuk Lapisan Tipis." },
  { "en": "Apa Itu Chemical-Mechanical Polishing (CMP)?", "id": "Proses Untuk Meratakan Permukaan Wafer Silikon." },
  { "en": "Apa Kepanjangan CMP Dalam Proses Fabrikasi?", "id": "Chemical-Mechanical Polishing Adalah Kepanjangannya." },
  { "en": "Apa Itu Salicide Dalam Proses Fabrikasi CMOS?", "id": "Teknologi Untuk Mengurangi Resistansi Source Drain." },
  { "en": "Apa Kepanjangan Dari Kata Salicide Itu?", "id": "Self-Aligned Silicide Adalah Kepanjangan Resminya." },
  { "en": "Apa Itu Strained Silicon Teknologi Transistor?", "id": "Teknik Meregangkan Kisi Silikon Secara Sengaja." },
  { "en": "Apa Tujuan Utama Dari Teknologi Strained Silicon?", "id": "Meningkatkan Mobilitas Elektron Dan Kinerja Transistor." },
  { "en": "Apa Itu High-k Dielectric Dalam MOSFET?", "id": "Material Insulator Dengan Konstanta Dielektrik Tinggi." },
  { "en": "Mengapa High-k Dielectric Sangat Diperlukan Sekali?", "id": "Untuk Mengurangi Arus Bocor Gerbang (Gate)." },
  { "en": "Apa Itu Metal Gate Dalam Teknologi MOSFET?", "id": "Mengganti Gerbang Polisilikon Dengan Gerbang Logam." },
  { "en": "Apa Tujuan Utama Penggunaan Metal Gate?", "id": "Menghilangkan Efek Deplesi Polisilikon Yang Merugikan." },
  { "en": "Apa Itu Teknologi HKMG Dalam Fabrikasi IC?", "id": "Kombinasi High-k Dielectric Dan Metal Gate." },
  { "en": "Apa Kepanjangan HKMG Dalam Dunia Semikonduktor?", "id": "High-k Metal Gate Adalah Kepanjangannya." },
  { "en": "Apa Itu Resonant-Tunneling Diode (RTD)?", "id": "Dioda Yang Bekerja Berdasarkan Terowongan Resonan." },
  { "en": "Apa Itu Resonant-Tunneling Transistor (RTT)?", "id": "Transistor Yang Menggabungkan Efek Terowongan Resonan." },
  { "en": "Apa Itu Zero-Temperature-Coefficient (ZTC) Point?", "id": "Titik Bias Dimana Efek Suhu Minimal." },
  { "en": "Apa Itu Subthreshold Swing (SS) MOSFET?", "id": "Ukuran Efektivitas Kontrol Gerbang Terhadap Arus." },
  { "en": "Nilai Subthreshold Swing Ideal Itu Berapa?", "id": "Nilai Idealnya Sekitar Enam Puluh mV/decade." },
  { "en": "Apa Itu Gate-All-Around (GAA) FET?", "id": "Arsitektur FET Dimana Gerbang Mengelilingi Kanal." },
  { "en": "Apa Kepanjangan GAA Dalam Arsitektur FET?", "id": "Gate-All-Around Adalah Kepanjangan Resminya." },
  { "en": "Apa Keuntungan Utama Arsitektur GAA FET?", "id": "Kontrol Gerbang Terbaik Untuk Skala Nanometer." },
  { "en": "Apa Itu Layout Common-Centroid Dalam IC?", "id": "Teknik Layout Untuk Pencocokan Perangkat Presisi." },
  { "en": "Mengapa Layout Common-Centroid Sangat Penting Sekali?", "id": "Untuk Mengurangi Efek Gradien Proses Fabrikasi." },
  { "en": "Apa Itu Teknik Layout Interdigitasi (Interdigitation)?", "id": "Menyusun Struktur Jari-Jari Secara Berselang-seling." },
  { "en": "Apa Itu Frekuensi Pojok Derau Flicker?", "id": "Frekuensi Dimana Derau Flicker Sama Derau Termal." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran Distorsi Harmonik Total Pada Sinyal." },
  { "en": "Apa Kepanjangan THD Dalam Pengukuran Audio?", "id": "Total Harmonic Distortion Adalah Kepanjangan Resminya." },
  { "en": "Apa Itu Spurious-Free Dynamic Range (SFDR)?", "id": "Rentang Sinyal Bebas Dari Komponen Spurious." },
  { "en": "Apa Kepanjangan SFDR Dalam Pengukuran Sinyal?", "id": "Spurious-Free Dynamic Range Adalah Kepanjangannya." },
  { "en": "Apa Itu Logika Adiabatik Dalam Desain Digital?", "id": "Logika Switching Untuk Mengurangi Disipasi Daya." },
  { "en": "Bagaimana Logika Adiabatik Menghemat Daya Listrik?", "id": "Dengan Mendaur Ulang Energi Muatan Kapasitor." },
  { "en": "Apa Itu Sirkuit Asinkron Atau Tanpa Clock?", "id": "Sirkuit Digital Yang Beroperasi Tanpa Sinyal Clock." },
  { "en": "Apa Itu Efek Penyempitan Celah Pita?", "id": "Pengurangan Energi Celah Pita Akibat Doping." },
  { "en": "Apa Itu Degradasi Mobilitas Dalam MOSFET?", "id": "Penurunan Mobilitas Pembawa Muatan Dekat Permukaan." },
  { "en": "Apa Penyebab Utama Degradasi Mobilitas Elektron?", "id": "Medan Listrik Vertikal Yang Sangat Kuat." },
  { "en": "Apa Itu Konverter Kuasi-Resonan Dalam Elektronika?", "id": "Konverter Switching Dengan Karakteristik Resonansi Parsial." },
  { "en": "Apa Itu Teknik Soft Switching Catu Daya?", "id": "Teknik Mengalihkan Transistor Pada Tegangan Nol." },
  { "en": "Apa Keuntungan Utama Dari Teknik Soft Switching?", "id": "Mengurangi Kerugian Switching Dan Interferensi Elektromagnetik." },
  { "en": "Apa Itu Current-Feedback Operational Amplifier (CFA)?", "id": "Jenis Op-Amp Dengan Karakteristik Umpan Balik Arus." },
  { "en": "Apa Keuntungan Utama Dari Penguat CFA?", "id": "Slew Rate Tinggi Dan Lebar Pita Konstan." },
  { "en": "Apa Itu Penguat Log-Antilog Dengan Transistor?", "id": "Rangkaian Untuk Operasi Perkalian Dan Pembagian." },
  { "en": "Apa Itu Wafer Probing Dalam Pengujian IC?", "id": "Pengujian Die Sirkuit Selagi Masih Di Wafer." },
  { "en": "Apa Itu Burn-In Testing Untuk Komponen?", "id": "Pengujian Komponen Pada Suhu Tinggi Terekstrim." },
  { "en": "Apa Tujuan Utama Dari Burn-In Testing?", "id": "Menyingkirkan Kegagalan Awal (Infant Mortality) Komponen." },
  { "en": "Apa Itu Semikonduktor Senyawa III-V?", "id": "Senyawa Dari Elemen Grup III Dan V." },
  { "en": "Sebutkan Contoh Utama Semikonduktor III-V?", "id": "Gallium Arsenide (GaAs) Dan Indium Phosphide (InP)." },
  { "en": "Apa Itu Semikonduktor Senyawa II-VI?", "id": "Senyawa Dari Elemen Grup II Dan VI." },
  { "en": "Sebutkan Contoh Utama Semikonduktor II-VI?", "id": "Cadmium Selenide (CdSe) Dan Zinc Sulfide (ZnS)." },
  { "en": "Apa Itu Transistor Kontak Titik (Point-Contact)?", "id": "Jenis Transistor Pertama Yang Berhasil Dibuat." },
  { "en": "Siapa Yang Mendemonstrasikan Transistor Kontak Titik?", "id": "John Bardeen Dan Walter Houser Brattain." },
  { "en": "Apa Itu Transistor Surface Barrier (SBT)?", "id": "Jenis Transistor Logam-Semikonduktor Kecepatan Tinggi." },
  { "en": "Apa Itu Efek Kirk Atau Perluasan Basis?", "id": "Perluasan Basis Efektif Pada Arus Tinggi." },
  { "en": "Apa Akibat Buruk Dari Terjadinya Efek Kirk?", "id": "Menurunkan Frekuensi Transisi (fT) Secara Drastis." },
  { "en": "Apa Itu Resistansi On (RDS(on)) MOSFET?", "id": "Resistansi Antara Drain Dan Source Saat On." },
  { "en": "Nilai RDS(on) Yang Baik Itu Tinggi Atau Rendah?", "id": "Nilai Yang Sangat Rendah Dianggap Baik." },
  { "en": "Apa Itu Muatan Gerbang Total (Total Gate Charge)?", "id": "Total Muatan Yang Diperlukan Mengaktifkan MOSFET." },
  { "en": "Apa Pengaruh Muatan Gerbang Terhadap Kinerja?", "id": "Mempengaruhi Kecepatan Switching Dan Kerugian Daya." },
  { "en": "Apa Itu Kapasitansi Miller Pada MOSFET?", "id": "Kapasitansi Antara Gerbang Dan Drain (Cgd)." },
  { "en": "Apa Itu Efek Plateau Miller Tegangan Gerbang?", "id": "Daerah Datar Pada Kurva Tegangan Gerbang." },
  { "en": "Apa Itu Dioda Body Intrinsik MOSFET?", "id": "Dioda Parasitik Antara Drain Dan Source." },
  { "en": "Kapan Dioda Body Intrinsik Menjadi Aktif?", "id": "Saat Tegangan Source-Drain Diberi Bias Balik." },
  { "en": "Apa Itu Pemulihan Terbalik (Reverse Recovery)?", "id": "Fenomena Pada Dioda Saat Beralih Ke Off." },
  { "en": "Apa Itu Waktu Pemulihan Terbalik (trr)?", "id": "Waktu Yang Dibutuhkan Dioda Untuk Pulih." },
  { "en": "Apa Itu Muatan Pemulihan Terbalik (Qrr)?", "id": "Muatan Yang Harus Dihilangkan Selama Pemulihan." },
  { "en": "Apa Itu Trench Gate MOSFET Dalam Elektronika?", "id": "Struktur Gerbang Berbentuk Parit Untuk Kepadatan." },
  { "en": "Apa Keuntungan Utama Dari Trench Gate MOSFET?", "id": "Mencapai Resistansi On (RDS(on)) Sangat Rendah." },
  { "en": "Apa Itu Superjunction MOSFET Dalam Elektronika?", "id": "MOSFET Dengan Pilar-Pilar Doping Yang Bergantian." },
  { "en": "Apa Keuntungan Utama Superjunction MOSFET Modern?", "id": "Resistansi Rendah Dengan Kemampuan Tegangan Tinggi." },
  { "en": "Apa Itu Penguat Kelas S Dalam Elektronika?", "id": "Penguat Non-Linear Yang Digunakan Dalam Transmiter." },
  { "en": "Apa Itu Efek Termoelektrik Dalam Dunia Semikonduktor?", "id": "Konversi Langsung Perbedaan Suhu Ke Listrik." },
  { "en": "Apa Itu Peltier Cooler Berbasis Semikonduktor?", "id": "Perangkat Pendingin Solid-State Tanpa Bagian Bergerak." },
  { "en": "Apa Itu Efek Piezoresistif Dalam Semikonduktor?", "id": "Perubahan Resistivitas Akibat Tegangan Mekanis." },
  { "en": "Bagaimana Efek Piezoresistif Digunakan Dalam Sensor?", "id": "Untuk Mengukur Tekanan, Regangan, Dan Akselerasi." },
  { "en": "Apa Itu Microelectromechanical Systems (MEMS)?", "id": "Perangkat Mikro Dengan Komponen Mekanik Elektronik." },
  { "en": "Apa Kepanjangan MEMS Dalam Dunia Teknologi?", "id": "Microelectromechanical Systems Adalah Kepanjangan Resminya." },
  { "en": "Apa Peran Transistor Dalam Teknologi MEMS?", "id": "Sebagai Bagian Dari Sirkuit Pengkondisian Sinyal." },
  { "en": "Apa Itu Junctionless Nanowire Transistor Modern?", "id": "Transistor Tanpa Sambungan PN Yang Jelas." },
  { "en": "Apa Itu Efek Stark Dalam Dunia Fisika?", "id": "Pergeseran Level Energi Atom Akibat Medan Listrik." },
  { "en": "Apa Itu Efek Zeeman Dalam Dunia Fisika?", "id": "Pemisahan Garis Spektral Akibat Medan Magnet." },
  { "en": "Apa Itu Avalanche Photodiode (APD)?", "id": "Dioda Foto Dengan Penguatan Arus Internal." },
  { "en": "Bagaimana APD Mencapai Penguatan Arus Internal?", "id": "Melalui Proses Breakdown Avalanche Yang Terkontrol." },
  { "en": "Apa Itu Single-Photon Avalanche Diode (SPAD)?", "id": "Dioda APD Yang Cukup Sensitif Mendeteksi Foton." },
  { "en": "Apa Itu Quenching Circuit Untuk Dioda SPAD?", "id": "Rangkaian Untuk Menghentikan Proses Avalanche Dengan Cepat." },
  { "en": "Apa Itu Afterpulsing Dalam Detektor SPAD?", "id": "Pulsa Palsu Yang Terjadi Setelah Deteksi." },
  { "en": "Apa Itu Dark Count Rate (DCR)?", "id": "Laju Pulsa Yang Terdeteksi Tanpa Adanya Cahaya." },
  { "en": "Apa Itu Photon Detection Efficiency (PDE)?", "id": "Probabilitas Foton Yang Datang Akan Terdeteksi." },
  { "en": "Apa Itu Charge-Coupled Device (CCD)?", "id": "Sensor Gambar Berbasis Pergeseran Paket Muatan." },
  { "en": "Apa Itu CMOS Image Sensor (CIS)?", "id": "Sensor Gambar Yang Menggunakan Teknologi CMOS." },
  { "en": "Mana Yang Lebih Dominan Saat Ini CCD Atau CIS?", "id": "CMOS Image Sensor (CIS) Jauh Lebih Dominan." },
  { "en": "Apa Itu Active Pixel Sensor (APS)?", "id": "Jenis Sensor CMOS Dengan Penguat Di Piksel." },
  { "en": "Apa Itu Rolling Shutter Pada Sensor CMOS?", "id": "Membaca Piksel Baris Demi Baris Berurutan." },
  { "en": "Apa Itu Global Shutter Pada Sensor CMOS?", "id": "Membaca Semua Piksel Secara Bersamaan Sekaligus." },
  { "en": "Apa Itu Rangkaian Correlated Double Sampling (CDS)?", "id": "Teknik Untuk Mengurangi Derau Pada Sensor Gambar." },
  { "en": "Apa Itu Quantum Dot Dalam Dunia Nanoteknologi?", "id": "Partikel Semikonduktor Berukuran Sangat Kecil Sekali." },
  { "en": "Apa Itu Quantum Dot Display (QLED)?", "id": "Layar Yang Menggunakan Quantum Dot Untuk Warna." },
  { "en": "Apa Itu Perovskite Solar Cell Atau Sel Surya?", "id": "Jenis Sel Surya Menggunakan Material Perovskite." },
  { "en": "Apa Itu Graphene Field-Effect Transistor (GFET)?", "id": "Transistor Yang Menggunakan Graphene Sebagai Kanal." },
  { "en": "Apa Keunggulan Potensial Dari Graphene GFET?", "id": "Mobilitas Elektron Sangat Tinggi Dan Transparan." },
  { "en": "Apa Itu Silicene Dalam Dunia Material?", "id": "Struktur Atom Silikon Mirip Dengan Graphene." },
  { "en": "Apa Itu Molybdenite (MoS2) Transistor Modern?", "id": "Transistor Berbasis Material Dua Dimensi MoS2." },
  { "en": "Apa Itu Logika Reversibel Dalam Komputasi?", "id": "Komputasi Dimana Prosesnya Dapat Dibalik Kembali." },
  { "en": "Apa Prinsip Landauer Dalam Dunia Komputasi?", "id": "Penghapusan Informasi Menghasilkan Disipasi Energi Panas." },
  { "en": "Apa Itu Komputasi Kuantum Atau Quantum Computing?", "id": "Komputasi Yang Memanfaatkan Fenomena Mekanika Kuantum." },
  { "en": "Apa Itu Qubit Atau Quantum Bit?", "id": "Unit Dasar Informasi Dalam Komputasi Kuantum." },
  { "en": "Apa Itu Superposisi Kuantum Dalam Dunia Fisika?", "id": "Kemampuan Sistem Berada Di Banyak Keadaan." },
  { "en": "Apa Itu Keterkaitan Kuantum Atau Entanglement?", "id": "Koneksi Antar Partikel Bahkan Saat Terpisah." },
  { "en": "Bagaimana Transistor Digunakan Dalam Komputasi Kuantum?", "id": "Untuk Mengontrol Dan Membaca Status Qubit." },
  { "en": "Apa Itu Transmon Dalam Komputasi Kuantum?", "id": "Jenis Qubit Superkonduktor Yang Sangat Populer." },
  { "en": "Apa Itu Neuromorphic Engineering Atau Rekayasa?", "id": "Desain Sirkuit Yang Meniru Otak Biologis." },
  { "en": "Apa Itu Sinapsis Elektronik Dengan Memristor?", "id": "Komponen Yang Meniru Fungsi Sinapsis Biologis." },
  { "en": "Apa Itu Memristor Sebagai Komponen Elektronik?", "id": "Elemen Sirkuit Pasif Keempat Setelah Resistor." },
  { "en": "Apa Itu Light-Emitting Transistor (LET)?", "id": "Transistor Yang Dapat Memancarkan Cahaya Sendiri." },
  { "en": "Apa Itu Tunnel Diode Dalam Dunia Elektronika?", "id": "Dioda Yang Menunjukkan Resistansi Diferensial Negatif." },
  { "en": "Apa Itu Arus Difusi Dalam Semikonduktor?", "id": "Aliran Muatan Akibat Perbedaan Tingkat Konsentrasi." },
  { "en": "Apa Itu Arus Drift Dalam Semikonduktor?", "id": "Aliran Muatan Akibat Pengaruh Medan Listrik." },
  { "en": "Apa Itu Hubungan Einstein Dalam Semikonduktor?", "id": "Hubungan Antara Konstanta Difusi Dan Mobilitas." },
  { "en": "Apa Itu Waktu Hidup Pembawa Muatan?", "id": "Waktu Rata-rata Sebelum Terjadinya Proses Rekombinasi." },
  { "en": "Apa Itu Rekombinasi Shockley-Read-Hall (SRH)?", "id": "Rekombinasi Melalui Perangkap (Trap) Cacat Kristal." },
  { "en": "Apa Itu Rekombinasi Auger Dalam Semikonduktor?", "id": "Rekombinasi Tiga Partikel Pada Doping Tinggi." },
  { "en": "Apa Itu Well Proximity Effect (WPE)?", "id": "Perubahan Karakteristik Transistor Dekat Tepi Well." },
  { "en": "Apa Fungsi Guard Ring Dalam Desain CMOS?", "id": "Struktur Untuk Mencegah Terjadinya Kondisi Latch-up." },
  { "en": "Apa Itu Waktu Mati (Dead Time)?", "id": "Waktu Jeda Singkat Antara Dua Saklar." },
  { "en": "Mengapa Waktu Mati Sangat Penting Diperlukan?", "id": "Mencegah Hubung Singkat (Shoot-Through) Pada Catu." },
  { "en": "Apa Itu Kerugian Konduksi Dioda Body?", "id": "Kerugian Daya Saat Dioda Body Menghantar." },
  { "en": "Apa Itu Faktor Stabilitas Rollett (K-Factor)?", "id": "Ukuran Stabilitas Tanpa Syarat Sebuah Penguat." },
  { "en": "Kapan Penguat Dikatakan Stabil Tanpa Syarat?", "id": "Ketika Nilai Faktor Stabilitas K > 1." },
  { "en": "Apa Itu Pencocokan Konjugat Simultan (Simultaneous Match)?", "id": "Mencocokkan Input Dan Output Secara Bersamaan." },
  { "en": "Apa Itu Crosstalk Atau Bincang Silang?", "id": "Gangguan Sinyal Antar Jalur Yang Berdekatan." },
  { "en": "Apa Itu Refleksi Sinyal Pada Jalur Transmisi?", "id": "Pantulan Sinyal Akibat Ketidakcocokan Nilai Impedansi." },
  { "en": "Apa Itu Terminasi Jalur Transmisi Sinyal?", "id": "Menambahkan Beban Untuk Mencegah Terjadinya Refleksi." },
  { "en": "Apa Itu Time-Domain Reflectometry (TDR)?", "id": "Teknik Mengukur Refleksi Untuk Karakterisasi Impedansi." },
  { "en": "Apa Itu Static Induction Transistor (SIT)?", "id": "Jenis JFET Daya Tinggi Dengan Kanal Vertikal." },
  { "en": "Apa Itu Metal-Semiconductor FET (MESFET)?", "id": "Transistor FET Menggunakan Sambungan Schottky Untuk Gerbang." },
  { "en": "Apa Kepanjangan MESFET Dalam Dunia Elektronika?", "id": "Metal-Semiconductor Field-Effect Transistor Adalah Kepanjangannya." },
  { "en": "Di Mana MESFET Umumnya Banyak Digunakan?", "id": "Pada Aplikasi Frekuensi Mikro Dan Frekuensi Radio." },
  { "en": "Apa Itu Hazard Statis Dalam Rangkaian Logika?", "id": "Potensi Glitch Saat Satu Input Berubah." },
  { "en": "Apa Itu Hazard Dinamis Dalam Rangkaian Logika?", "id": "Potensi Glitch Saat Output Seharusnya Berubah." },
  { "en": "Apa Itu Kondisi Balapan Atau Race Condition?", "id": "Output Sirkuit Bergantung Pada Urutan Sinyal." },
  { "en": "Apa Itu Transimpedance Amplifier (TIA)?", "id": "Penguat Yang Mengubah Sinyal Arus Ke Tegangan." },
  { "en": "Di Mana Penguat TIA Sering Digunakan?", "id": "Pada Penerima Optik Dengan Komponen Fotodioda." },
  { "en": "Apa Itu Konverter Tegangan-ke-Arus (V-I)?", "id": "Rangkaian Yang Menghasilkan Arus Sebanding Tegangan." },
  { "en": "Apa Nama Lain Konverter Tegangan-ke-Arus?", "id": "Nama Lainnya Adalah Penguat Transkonduktansi (Transconductance)." },
  { "en": "Apa Itu Figure of Merit (FOM)?", "id": "Parameter Kinerja Untuk Membandingkan Perangkat Elektronik." },
  { "en": "Apa Itu Backgating Effect Atau Efek Gerbang Belakang?", "id": "Pengaruh Tegangan Substrat Terhadap Arus Drain." },
  { "en": "Apa Itu Permukaan Fermi Dalam Fisika Zat Padat?", "id": "Permukaan Energi Konstan Yang Memisahkan Keadaan." },
  { "en": "Apa Itu Massa Efektif Elektron Atau Lubang?", "id": "Massa Partikel Saat Bergerak Dalam Kristal." },
  { "en": "Apa Itu Phonon Dalam Fisika Kristal?", "id": "Getaran Terkuantisasi Dari Kisi Atom Kristal." },
  { "en": "Apa Itu Hamburan Pembawa Muatan (Carrier Scattering)?", "id": "Interaksi Yang Menghambat Gerakan Pembawa Muatan." },
  { "en": "Apa Itu Doping Kompensasi Dalam Semikonduktor?", "id": "Menambahkan Donor Dan Akseptor Secara Bersamaan." },
  { "en": "Apa Itu Semikonduktor Intrinsik Murni (Intrinsic)?", "id": "Semikonduktor Murni Tanpa Proses Doping Tambahan." },
  { "en": "Apa Itu Semikonduktor Ekstrinsik Tidak Murni?", "id": "Semikonduktor Yang Sengaja Diberi Proses Doping." },
  { "en": "Apa Itu Level Fermi Intrinsik (Intrinsic Fermi)?", "id": "Level Fermi Pada Semikonduktor Tipe Intrinsik." },
  { "en": "Apa Itu Muatan Antarmuka (Interface Charge)?", "id": "Muatan Terperangkap Pada Antarmuka Silikon Oksida." },
  { "en": "Apa Itu Tegangan Flat-Band Pada Kapasitor MOS?", "id": "Tegangan Gerbang Untuk Membuat Pita Energi Datar." },
  { "en": "Apa Itu Inversi Kuat (Strong Inversion)?", "id": "Kondisi Lapisan Inversi Terbentuk Di Kanal." },
  { "en": "Apa Itu Inversi Lemah (Weak Inversion)?", "id": "Daerah Operasi Antara Threshold Dan Kondisi Off." },
  { "en": "Daerah Operasi Lainnya Disebut Daerah Apa?", "id": "Daerah Operasi Sub-Ambang (Subthreshold) Juga Populer." },
  { "en": "Apa Itu Lightly Doped Drain (LDD)?", "id": "Struktur Untuk Mengurangi Efek Hot Carrier." },
  { "en": "Apa Kepanjangan LDD Dalam Fabrikasi MOSFET?", "id": "Lightly Doped Drain Adalah Kepanjangan Resminya." },
  { "en": "Apa Itu Efek Halo Atau Halo Effect?", "id": "Peningkatan Doping Dekat Source Untuk Kontrol Kanal." },
  { "en": "Apa Itu Stress-Induced Leakage Current (SILC)?", "id": "Peningkatan Arus Bocor Akibat Tegangan Listrik." },
  { "en": "Apa Itu Negative-Bias Temperature Instability (NBTI)?", "id": "Degradasi Kinerja PMOS Akibat Bias Negatif." },
  { "en": "Apa Itu Positive-Bias Temperature Instability (PBTI)?", "id": "Degradasi Kinerja NMOS Akibat Bias Positif." },
  { "en": "Apa Itu Desain Full-Custom Dalam IC?", "id": "Desain Sirkuit Terpadu Dari Level Transistor." },
  { "en": "Apa Itu Desain Semi-Custom Dalam IC?", "id": "Desain Menggunakan Blok Standar Yang Telah Disediakan." },
  { "en": "Apa Itu Standard Cell Dalam Desain IC?", "id": "Kumpulan Gerbang Logika Dasar Yang Telah Didesain." },
  { "en": "Apa Itu Place and Route (P&R)?", "id": "Proses Penempatan Dan Penyambungan Sel Standar." },
  { "en": "Apa Itu Design Rule Check (DRC)?", "id": "Pemeriksaan Geometris Layout Terhadap Aturan Fabrikasi." },
  { "en": "Apa Itu Layout Versus Schematic (LVS)?", "id": "Pemeriksaan Kesesuaian Antara Layout Dan Skematik." },
  { "en": "Apa Itu System on a Chip (SoC)?", "id": "Integrasi Seluruh Komponen Sistem Dalam Satu Chip." },
  { "en": "Apa Kepanjangan SoC Dalam Dunia Mikroelektronika?", "id": "System on a Chip Adalah Kepanjangannya." },
  { "en": "Apa Itu Network on a Chip (NoC)?", "id": "Infrastruktur Komunikasi Antar Blok Dalam SoC." },
  { "en": "Apa Itu Intellectual Property (IP) Core?", "id": "Blok Desain Sirkuit Yang Dapat Digunakan Ulang." },
  { "en": "Apa Itu Soft Core Dalam Dunia IP?", "id": "IP Core Yang Disediakan Dalam Bahasa Deskripsi." },
  { "en": "Apa Itu Hard Core Dalam Dunia IP?", "id": "IP Core Yang Disediakan Dalam Bentuk Layout." },
  { "en": "Apa Itu Phase Frequency Detector (PFD)?", "id": "Komponen Dalam PLL Yang Membandingkan Fasa Frekuensi." },
  { "en": "Apa Itu Charge Pump Dalam Sirkuit PLL?", "id": "Menghasilkan Arus Proporsional Terhadap Perbedaan Fasa." },
  { "en": "Apa Itu Loop Filter Dalam Sirkuit PLL?", "id": "Filter Low-Pass Untuk Menstabilkan Loop PLL." },
  { "en": "Apa Itu Injection Locking Pada Osilator?", "id": "Sinkronisasi Osilator Oleh Sinyal Eksternal Dekat." },
  { "en": "Apa Itu Pulling Frekuensi Pada Osilator?", "id": "Pergeseran Frekuensi Akibat Perubahan Impedansi Beban." },
  { "en": "Apa Itu Varactor Dalam Dunia Elektronika?", "id": "Dioda Yang Digunakan Sebagai Kapasitor Variabel." },
  { "en": "Apa Itu Dioda PIN Dalam Dunia RF?", "id": "Dioda Dengan Lapisan Intrinsik Di Antara P-N." },
  { "en": "Apa Aplikasi Utama Dari Sebuah Dioda PIN?", "id": "Sebagai Saklar Atau Attenuator Frekuensi Tinggi." },
  { "en": "Apa Itu Dioda Terowongan Resonan (RTD)?", "id": "Perangkat Kuantum Dengan Resistansi Diferensial Negatif." },
  { "en": "Apa Itu Dioda Gunn Dalam Dunia Microwave?", "id": "Perangkat Untuk Pembangkit Sinyal Frekuensi Microwave." },
  { "en": "Apa Itu Dioda IMPATT Dalam Dunia Microwave?", "id": "Dioda Untuk Pembangkit Daya Microwave Tinggi." },
  { "en": "Apa Itu Dioda TRAPATT Dalam Dunia Microwave?", "id": "Varian Dioda IMPATT Untuk Efisiensi Tinggi." },
  { "en": "Apa Itu Sirkulator Dalam Rangkaian Frekuensi Radio?", "id": "Perangkat Tiga Port Non-Resiprokal Yang Pasif." },
  { "en": "Apa Itu Isolator Dalam Rangkaian Frekuensi Radio?", "id": "Perangkat Dua Port Yang Melewatkan Sinyal." },
  { "en": "Apa Itu Mixer Dalam Rangkaian Frekuensi Radio?", "id": "Rangkaian Untuk Mengubah Frekuensi Sinyal Listrik." },
  { "en": "Apa Itu Konversi Naik (Up-conversion) Frekuensi?", "id": "Mengubah Sinyal Frekuensi Rendah Ke Tinggi." },
  { "en": "Apa Itu Konversi Turun (Down-conversion) Frekuensi?", "id": "Mengubah Sinyal Frekuensi Tinggi Ke Rendah." },
  { "en": "Apa Itu Frekuensi Gambar (Image Frequency)?", "id": "Frekuensi Yang Tidak Diinginkan Pada Proses Konversi." },
  { "en": "Apa Itu Penolakan Frekuensi Gambar (Image Rejection)?", "id": "Kemampuan Penerima Menolak Sinyal Frekuensi Gambar." },
  { "en": "Apa Itu Penerima Superheterodyne Dalam Dunia Radio?", "id": "Arsitektur Penerima Menggunakan Pencampuran Frekuensi Radio." },
  { "en": "Apa Itu Frekuensi Antara (Intermediate Frequency)?", "id": "Frekuensi Tetap Hasil Dari Proses Pencampuran." },
  { "en": "Apa Itu Penerima Homodyne Atau Direct-Conversion?", "id": "Penerima Yang Langsung Mengubah Sinyal RF." },
  { "en": "Apa Itu DC Offset Pada Penerima Homodyne?", "id": "Masalah Tegangan DC Yang Tidak Diinginkan." },
  { "en": "Apa Itu I/Q Imbalance Pada Penerima?", "id": "Ketidakcocokan Antara Jalur In-phase Dan Quadrature." },
  { "en": "Apa Itu Modulasi Amplitudo (AM)?", "id": "Memodulasi Amplitudo Sinyal Pembawa Frekuensi Radio." },
  { "en": "Apa Itu Modulasi Frekuensi (FM)?", "id": "Memodulasi Frekuensi Sinyal Pembawa Frekuensi Radio." },
  { "en": "Apa Itu Modulasi Fasa (PM)?", "id": "Memodulasi Fasa Sinyal Pembawa Frekuensi Radio." },
  { "en": "Apa Itu Quadrature Amplitude Modulation (QAM)?", "id": "Skema Modulasi Digital Menggunakan Amplitudo Fasa." },
  { "en": "Apa Itu Diagram Konstelasi Dalam Modulasi Digital?", "id": "Representasi Grafis Dari Simbol Modulasi Digital." },
  { "en": "Apa Itu Dioda Laser (Laser Diode)?", "id": "Dioda Semikonduktor Yang Menghasilkan Cahaya Laser." },
  { "en": "Apa Itu Arus Ambang (Threshold Current)?", "id": "Arus Minimum Untuk Memulai Aksi Lasing." },
  { "en": "Apa Itu Sel Surya (Solar Cell)?", "id": "Perangkat Yang Mengubah Energi Cahaya Matahari." },
  { "en": "Apa Itu Kurva Arus-Tegangan Sel Surya?", "id": "Grafik Karakteristik Kinerja Dari Sebuah Sel Surya." },
  { "en": "Apa Itu Faktor Isi (Fill Factor)?", "id": "Ukuran Kualitas Dari Sebuah Sel Surya." },
  { "en": "Apa Itu Semikonduktor Degenerate Atau Cacat?", "id": "Semikonduktor Dengan Level Doping Sangat Tinggi." },
  { "en": "Apa Itu Terowongan Antar Pita (Band-to-Band Tunneling)?", "id": "Mekanisme Kuantum Elektron Melintasi Celah Pita." },
  { "en": "Apa Itu Ionisasi Tumbukan (Impact Ionization)?", "id": "Pembentukan Pasangan Elektron-Lubang Akibat Energi Tinggi." },
  { "en": "Apa Itu Metodologi Desain gm/ID Analog?", "id": "Metode Desain Berbasis Efisiensi Transkonduktansi Transistor." },
  { "en": "Apa Itu Peningkatan Impedansi Output (Output Enhancement)?", "id": "Teknik Untuk Meningkatkan Penguatan Tegangan Penguat." },
  { "en": "Apa Itu Verifikasi Fungsional Dalam Desain IC?", "id": "Memastikan Desain Bekerja Sesuai Dengan Spesifikasi." },
  { "en": "Apa Itu Verifikasi Formal Dalam Desain IC?", "id": "Menggunakan Pembuktian Matematis Untuk Verifikasi Desain." },
  { "en": "Apa Itu Automatic Test Pattern Generation (ATPG)?", "id": "Membuat Pola Uji Secara Otomatis." },
  { "en": "Apa Itu Rangkaian Kapasitor Bertukar (Switched-Capacitor)?", "id": "Rangkaian Yang Mensimulasikan Resistor Dengan Kapasitor." },
  { "en": "Apa Keuntungan Rangkaian Kapasitor Bertukar?", "id": "Implementasi Filter Presisi Tinggi Dalam Desain IC." },
  { "en": "Apa Itu Konverter Delta-Sigma Dalam Elektronika?", "id": "Jenis Konverter Analog-ke-Digital Resolusi Tinggi." },
  { "en": "Apa Prinsip Dasar Dari Konverter Delta-Sigma?", "id": "Menggunakan Over-sampling Dan Noise Shaping Akurat." },
  { "en": "Apa Itu Passivated Emitter and Rear Cell?", "id": "Struktur Sel Surya Efisiensi Sangat Tinggi." },
  { "en": "Apa Kepanjangan PERC Dalam Teknologi Sel Surya?", "id": "Passivated Emitter and Rear Cell." },
  { "en": "Apa Itu Nanosheet Field-Effect Transistor?", "id": "Evolusi Arsitektur FinFET Untuk Skala Lanjutan." },
  { "en": "Siapakah Yang Dijuluki Traitorous Eight Atau Delapan Pengkhianat?", "id": "Pendiri Perusahaan Semikonduktor Fairchild Yang Terkenal." },
  { "en": "Siapa Penemu Sirkuit Terpadu Atau IC?", "id": "Jack Kilby Dan Robert Noyce Adalah Penemunya." },
  { "en": "Apa Itu Konverter Buck Dalam Elektronika Daya?", "id": "Konverter DC-DC Untuk Menurunkan Level Tegangan." },
  { "en": "Apa Itu Konverter Boost Dalam Elektronika Daya?", "id": "Konverter DC-DC Untuk Meningkatkan Level Tegangan." },
  { "en": "Apa Itu Konverter Buck-Boost Elektronika Daya?", "id": "Konverter Yang Dapat Menaikkan Menurunkan Tegangan." },
  { "en": "Apa Peran Transistor Dalam Konverter Switching?", "id": "Berperan Sebagai Saklar Elektronik Berkecepatan Tinggi." },
  { "en": "Apa Itu Daur Kerja (Duty Cycle)?", "id": "Rasio Waktu Aktif Terhadap Total Periode." },
  { "en": "Apa Itu Mode Konduksi Kontinu (CCM)?", "id": "Arus Induktor Tidak Pernah Mencapai Titik Nol." },
  { "en": "Apa Itu Mode Konduksi Diskontinu (DCM)?", "id": "Arus Induktor Mencapai Nol Dalam Siklusnya." },
  { "en": "Apa Itu Topologi Cuk Dalam Konverter DC-DC?", "id": "Jenis Konverter Buck-Boost Dengan Polaritas Terbalik." },
  { "en": "Apa Itu Topologi SEPIC Dalam Konverter?", "id": "Konverter Buck-Boost Dengan Polaritas Tidak Terbalik." },
  { "en": "Apa Kepanjangan SEPIC Dalam Dunia Konverter?", "id": "Single-Ended Primary-Inductor Converter Adalah Kepanjangannya." },
  { "en": "Apa Itu Topologi Jembatan Penuh (Full-Bridge)?", "id": "Konverter Menggunakan Empat Transistor Saklar Utama." },
  { "en": "Apa Itu Topologi Setengah Jembatan (Half-Bridge)?", "id": "Konverter Menggunakan Dua Transistor Saklar Utama." },
  { "en": "Apa Itu Topologi Flyback Dalam Elektronika Daya?", "id": "Konverter DC-DC Terisolasi Berbasis Induktor Gabungan." },
  { "en": "Apa Itu Resonant Converter Dalam Elektronika Daya?", "id": "Konverter Yang Menggunakan Resonansi LC Untuk Switching." },
  { "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Mengalihkan Transistor Saat Tegangan Melintasinya Nol." },
  { "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Mengalihkan Transistor Saat Arus Melaluinya Nol." },
  { "en": "Apa Itu Ballast Elektronik Untuk Lampu Neon?", "id": "Rangkaian Catu Daya Frekuensi Tinggi Modern." },
  { "en": "Apa Peran Transistor Dalam Ballast Elektronik?", "id": "Sebagai Saklar Utama Dalam Inverter Resonan." },
  { "en": "Apa Itu Power Factor Correction (PFC)?", "id": "Teknik Membuat Beban Terlihat Seperti Resistor." },
  { "en": "Mengapa Power Factor Correction Sangat Diperlukan?", "id": "Meningkatkan Efisiensi Dan Kualitas Daya Listrik." },
  { "en": "Apa Itu Gallium Nitride (GaN) HEMT?", "id": "Transistor Berbasis GaN Untuk Elektronika Daya." },
  { "en": "Apa Keunggulan Transistor GaN Dibanding Silikon?", "id": "Kecepatan Switching Lebih Tinggi Dan Ukuran Kecil." },
  { "en": "Apa Itu Silicon Carbide (SiC) MOSFET?", "id": "MOSFET Berbasis SiC Untuk Tegangan Tinggi." },
  { "en": "Apa Keunggulan Transistor SiC Dibanding Silikon?", "id": "Tegangan Tembus Tinggi Dan Suhu Operasi." },
  { "en": "Apa Itu Miller Clamp Circuit Atau Rangkaian?", "id": "Rangkaian Untuk Mencegah Efek Plateau Miller." },
  { "en": "Apa Itu Desaturation Detection Pada IGBT?", "id": "Metode Proteksi Terhadap Kondisi Arus Berlebih." },
  { "en": "Apa Itu Kelpasan Sebagian (Partial Discharge)?", "id": "Pelepasan Listrik Lokal Pada Material Isolasi." },
  { "en": "Apa Itu Jarak Rambat (Creepage Distance)?", "id": "Jarak Terpendek Antara Konduktor Di Permukaan." },
  { "en": "Apa Itu Jarak Bebas (Clearance Distance)?", "id": "Jarak Terpendek Antara Konduktor Melalui Udara." },
  { "en": "Apa Itu Indeks Pelacakan Komparatif (CTI)?", "id": "Ukuran Ketahanan Material Isolasi Terhadap Pelacakan." },
  { "en": "Apa Itu Hot-Plug Controller Dengan Transistor?", "id": "Sirkuit Untuk Memasukkan Kartu Ke Sistem Aktif." },
  { "en": "Apa Itu OR-ing Controller Dengan MOSFET?", "id": "Sirkuit Untuk Menggabungkan Catu Daya Redundan." },
  { "en": "Apa Itu Load Switch Dengan Transistor MOSFET?", "id": "Saklar Elektronik Sederhana Untuk Menghidupkan Beban." },
  { "en": "Apa Itu Inrush Current Atau Arus Masuk?", "id": "Lonjakan Arus Awal Saat Perangkat Dinyalakan." },
  { "en": "Bagaimana Transistor Dapat Membatasi Inrush Current?", "id": "Dengan Mengontrol Laju Kenaikan Tegangan Perlahan." },
  { "en": "Apa Itu Proteksi Tegangan Balik (Reverse Voltage)?", "id": "Melindungi Sirkuit Dari Polaritas Catu Terbalik." },
  { "en": "Apa Itu Crowbar Circuit Yang Sederhana?", "id": "Rangkaian Proteksi Tegangan Lebih Menggunakan Thyristor." },
  { "en": "Apa Itu Thyristor Gate Turn-Off (GTO)?", "id": "Jenis Thyristor Yang Dapat Dimatikan Gerbangnya." },
  { "en": "Apa Itu Integrated Gate-Commutated Thyristor (IGCT)?", "id": "Thyristor Dengan Sirkuit Penggerak Gerbang Terintegrasi." },
  { "en": "Apa Itu Bidirectional Triode Thyristor (TRIAC)?", "id": "Saklar Semikonduktor Dua Arah Untuk Arus AC." },
  { "en": "Apa Itu Quadrant Operasi Pada Komponen TRIAC?", "id": "Empat Mode Pemicuan Berdasarkan Polaritas Tegangan." },
  { "en": "Apa Itu DIAC for Alternating Current?", "id": "Dioda Pemicu Dua Arah Untuk Thyristor." },
  { "en": "Apa Itu Snubberless TRIAC Dalam Elektronika?", "id": "TRIAC Didesain Untuk Beban Induktif Berat." },
  { "en": "Apa Itu Phototriac Atau Opto-TRIAC Modern?", "id": "TRIAC Yang Dipicu Oleh Cahaya Internal." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay Elektronik Tanpa Bagian Mekanis Bergerak." },
  { "en": "Apa Kepanjangan SSR Dalam Dunia Elektronika?", "id": "Solid State Relay Adalah Kepanjangannya." },
  { "en": "Apa Keuntungan SSR Dibanding Relay Elektromekanis?", "id": "Kecepatan Tinggi, Umur Panjang, Tidak Berisik." },
  { "en": "Apa Itu Peltier Device Atau Modul Termoelektrik?", "id": "Perangkat Yang Mentransfer Panas Menggunakan Arus." },
  { "en": "Apa Itu Proportional-Integral-Derivative (PID) Controller?", "id": "Mekanisme Kontrol Umpan Balik Yang Populer." },
  { "en": "Apa Peran Transistor Dalam Sistem Kontrol PID?", "id": "Sebagai Elemen Aktuator Akhir Mengatur Daya." },
  { "en": "Apa Itu H-Bridge Driver Motor DC?", "id": "Rangkaian Empat Saklar Untuk Mengontrol Arah." },
  { "en": "Bagaimana H-Bridge Mengontrol Arah Putaran Motor?", "id": "Dengan Mengubah Polaritas Tegangan Pada Motor." },
  { "en": "Apa Itu Pulse Width Modulation (PWM) Chopping?", "id": "Teknik Mengontrol Arus Dalam Driver Motor." },
  { "en": "Apa Itu Stepper Motor Atau Motor Langkah?", "id": "Motor DC Brushless Yang Bergerak Bertahap." },
  { "en": "Bagaimana Transistor Menggerakkan Motor Stepper Modern?", "id": "Dengan Memberi Energi Pada Lilitan Secara Berurutan." },
  { "en": "Apa Itu Brushless DC (BLDC) Motor?", "id": "Motor DC Tanpa Sikat Dengan Komutasi Elektronik." },
  { "en": "Apa Itu Komutasi Elektronik Pada Motor BLDC?", "id": "Menggunakan Transistor Untuk Mengalihkan Arus Lilitan." },
  { "en": "Apa Itu Hall Effect Sensor Pada Motor?", "id": "Sensor Untuk Mendeteksi Posisi Rotor Motor." },
  { "en": "Apa Itu Field-Oriented Control (FOC) Motor?", "id": "Metode Kontrol Canggih Untuk Motor Tiga Fasa." },
  { "en": "Apa Itu Space Vector Modulation (SVM)?", "id": "Algoritma PWM Untuk Menggerakkan Inverter Tiga Fasa." },
  { "en": "Apa Itu Inverter Dalam Elektronika Daya?", "id": "Rangkaian Yang Mengubah Daya DC Menjadi AC." },
  { "en": "Apa Itu Rectifier Dalam Elektronika Daya?", "id": "Rangkaian Yang Mengubah Daya AC Menjadi DC." },
  { "en": "Apa Itu Cycloconverter Dalam Elektronika Daya?", "id": "Konverter Yang Mengubah Frekuensi AC Langsung." },
  { "en": "Apa Itu Matrix Converter Elektronika Daya?", "id": "Konverter AC-AC Tanpa Penyimpanan Energi DC." },
  { "en": "Apa Itu Power Busing Dalam Desain PCB?", "id": "Teknik Merancang Jalur Catu Daya Lebar." },
  { "en": "Apa Itu Star Grounding Dalam Desain PCB?", "id": "Menghubungkan Semua Ground Ke Satu Titik Pusat." },
  { "en": "Apa Itu Shielding Untuk Interferensi Elektromagnetik?", "id": "Menggunakan Konduktor Untuk Memblokir Medan Elektromagnetik." },
  { "en": "Apa Itu Ferrite Bead Pada Kabel Elektronik?", "id": "Komponen Pasif Untuk Menekan Derau Frekuensi Tinggi." },
  { "en": "Apa Itu Common-Mode Choke Dalam Penyaringan?", "id": "Induktor Untuk Menekan Derau Mode Bersama." },
  { "en": "Apa Itu Differential-Mode Choke Dalam Penyaringan?", "id": "Induktor Untuk Menekan Derau Mode Diferensial." },
  { "en": "Apa Itu Fungsi Gelombang Bloch Fisika Kuantum?", "id": "Fungsi Gelombang Partikel Dalam Potensial Periodik." },
  { "en": "Apa Itu Zona Brillouin Dalam Fisika Kristal?", "id": "Sel Primitif Unik Dalam Ruang Kisi Resiprokal." },
  { "en": "Apa Itu Kerapatan Keadaan (Density of States)?", "id": "Jumlah Keadaan Elektronik Per Satuan Volume Energi." },
  { "en": "Apa Itu Eksiton (Exciton) Dalam Semikonduktor?", "id": "Keadaan Terikat Dari Pasangan Elektron Dan Lubang." },
  { "en": "Apa Itu Polaron Dalam Fisika Zat Padat?", "id": "Kuasi-Partikel Gabungan Elektron Dengan Distorsi Kisi." },
  { "en": "Apa Itu Yield Dalam Manufaktur Semikonduktor?", "id": "Persentase Chip Fungsional Per Wafer Silikon." },
  { "en": "Apa Itu Process Corners Dalam Desain IC?", "id": "Variasi Ekstrem Dari Parameter Proses Manufaktur." },
  { "en": "Sebutkan Contoh Process Corners Yang Umum?", "id": "Fast-Fast, Slow-Slow, Dan Juga Tipikal (Typical)." },
  { "en": "Apa Itu Design for Manufacturability (DFM)?", "id": "Metodologi Desain Untuk Memaksimalkan Hasil Yield." },
  { "en": "Apa Itu Lingkaran Stabilitas Dalam Desain RF?", "id": "Representasi Grafis Kondisi Stabilitas Pada Smith Chart." },
  { "en": "Apa Itu Angka Derau (Noise Figure, NF)?", "id": "Ukuran Degradasi Sinyal-ke-Derau Oleh Perangkat." },
  { "en": "Nilai Angka Derau Yang Baik Itu Apa?", "id": "Nilai Yang Paling Rendah Adalah Terbaik." },
  { "en": "Apa Itu Jaringan Pencocokan (Matching Network)?", "id": "Sirkuit Untuk Mencocokkan Impedansi Sumber Beban." },
  { "en": "Apa Itu Jaringan Pencocokan Tipe L-Match?", "id": "Jaringan Sederhana Menggunakan Dua Elemen Reaktansi." },
  { "en": "Apa Itu Jaringan Pencocokan Tipe Pi-Match?", "id": "Jaringan Menggunakan Tiga Elemen Dalam Konfigurasi Pi." },
  { "en": "Apa Itu Digital-to-Analog Converter (DAC)?", "id": "Rangkaian Yang Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Arsitektur DAC R-2R Ladder?", "id": "Arsitektur Populer Menggunakan Jaringan Resistor Presisi." },
  { "en": "Apa Itu Analog-to-Digital Converter (ADC)?", "id": "Rangkaian Yang Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu Arsitektur ADC Flash Yang Cepat?", "id": "Arsitektur ADC Paralel Dengan Kecepatan Sangat Tinggi." },
  { "en": "Apa Itu Successive Approximation Register (SAR) ADC?", "id": "Arsitektur ADC Populer Dengan Keseimbangan Kinerja." },
  { "en": "Apa Itu Migrasi Tegangan (Stress Migration)?", "id": "Pergerakan Atom Logam Akibat Tegangan Mekanis." },
  { "en": "Apa Itu Material Antarmuka Termal (TIM)?", "id": "Material Untuk Meningkatkan Transfer Panas Antar Permukaan." },
  { "en": "Apa Itu Pipa Panas (Heat Pipe)?", "id": "Perangkat Transfer Panas Pasif Efisiensi Tinggi." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Berfungsi Tanpa Menyebabkan Interferensi." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Elektromagnetik Yang Mempengaruhi Sirkuit Elektronik." },
  { "en": "Apa Itu Kerentanan Elektromagnetik (Susceptibility)?", "id": "Tingkat Kerentanan Perangkat Terhadap Gangguan EMI." },
  { "en": "Apa Itu Emisi Elektromagnetik (Emissions)?", "id": "Tingkat Energi EMI Yang Dipancarkan Perangkat." },
  { "en": "Apa Itu Emisi Terkonduksi (Conducted Emissions)?", "id": "Derau EMI Yang Merambat Melalui Kabel." },
  { "en": "Apa Itu Emisi Terpancar (Radiated Emissions)?", "id": "Derau EMI Yang Merambat Melalui Udara." },
  { "en": "Apa Itu Dioda Schottky Barrier (SBD)?", "id": "Dioda Berbasis Sambungan Logam-Semikonduktor Murni." },
  { "en": "Apa Keuntungan Utama Dari Dioda Schottky?", "id": "Tegangan Maju Rendah Dan Kecepatan Switching Cepat." },
  { "en": "Apa Aplikasi Regulator Tegangan Dioda Zener?", "id": "Sebagai Referensi Atau Regulator Tegangan Shunt." },
  { "en": "Apa Itu Osilator Dioda Terowongan (Tunnel Diode)?", "id": "Osilator Sederhana Berbasis Resistansi Diferensial Negatif." },
  { "en": "Apa Itu Efek Fotokonduktif Dalam Semikonduktor?", "id": "Peningkatan Konduktivitas Listrik Akibat Penyerapan Cahaya." },
  { "en": "Apa Itu Light Dependent Resistor (LDR)?", "id": "Resistor Yang Nilainya Berubah Tergantung Cahaya." },
  { "en": "Apa Itu Efek Fotovoltaik Dalam Semikonduktor?", "id": "Pembangkitan Tegangan Listrik Akibat Penyerapan Cahaya." },
  { "en": "Apa Itu Termistor Dalam Dunia Sensor Suhu?", "id": "Resistor Yang Nilainya Sensitif Terhadap Perubahan Suhu." },
  { "en": "Apa Itu Resistance Temperature Detector (RTD)?", "id": "Sensor Suhu Berbasis Perubahan Resistansi Logam." },
  { "en": "Apa Itu Kuasi-Statik Dalam Analisis Rangkaian?", "id": "Asumsi Sinyal Berubah Cukup Lambat Sekali." },
  { "en": "Apa Itu Efek Kedekatan (Proximity Effect)?", "id": "Distribusi Arus Tidak Merata Akibat Konduktor Berdekatan." },
  { "en": "Apa Itu Dioda Step Recovery (SRD)?", "id": "Dioda Dengan Transisi Pemulihan Terbalik Sangat Cepat." },
  { "en": "Aplikasi Apa Yang Menggunakan Dioda Step Recovery?", "id": "Pembangkit Pulsa Cepat Dan Pengganda Frekuensi." },
  { "en": "Apa Itu Dioda Backward Dalam Dunia Elektronika?", "id": "Jenis Dioda Terowongan Untuk Deteksi Sinyal Kecil." },
  { "en": "Apa Itu Doping Gradual (Graded Doping)?", "id": "Profil Konsentrasi Doping Yang Berubah Perlahan." },
  { "en": "Apa Itu Sambungan Abrupt (Abrupt Junction)?", "id": "Sambungan Dengan Perubahan Doping Yang Tiba-tiba." },
  { "en": "Apa Itu Epitaxial Growth Dalam Fabrikasi Wafer?", "id": "Proses Menumbuhkan Lapisan Kristal Diatas Substrat." },
  { "en": "Apa Itu Wafer Silikon Tipe-CZ (Czochralski)?", "id": "Metode Paling Umum Untuk Produksi Wafer Silikon." },
  { "en": "Apa Itu Wafer Silikon Tipe-FZ (Float-Zone)?", "id": "Metode Untuk Menghasilkan Silikon Dengan Kemurnian Tinggi." },
  { "en": "Apa Itu Gettering Dalam Proses Manufaktur IC?", "id": "Proses Menghilangkan Kontaminan Yang Tidak Diinginkan." },
  { "en": "Apa Itu Passivasi Permukaan (Surface Passivation)?", "id": "Proses Membuat Permukaan Semikonduktor Kurang Reaktif." },
  { "en": "Apa Peran Silikon Dioksida Dalam Passivasi?", "id": "Sebagai Lapisan Pelindung Dan Isolator Yang Stabil." },
  { "en": "Apa Itu Annihilation Dalam Fisika Partikel?", "id": "Proses Saling Memusnahkan Partikel Dan Antipartikel." },
  { "en": "Apa Itu Positron Sebagai Antipartikel Elektron?", "id": "Partikel Dengan Massa Sama Tapi Muatan Berlawanan." },
  { "en": "Apa Itu Keadaan Dasar (Ground State)?", "id": "Tingkat Energi Paling Rendah Dalam Sistem Kuantum." },
  { "en": "Apa Itu Keadaan Tereksitasi (Excited State)?", "id": "Tingkat Energi Lebih Tinggi Dari Keadaan Dasar." },
  { "en": "Apa Itu Emisi Spontan Dalam Fisika Atom?", "id": "Pelepasan Foton Tanpa Stimulasi Dari Luar." },
  { "en": "Apa Itu Emisi Terstimulasi (Stimulated Emission)?", "id": "Pelepasan Foton Akibat Stimulasi Foton Lain." },
  { "en": "Prinsip Apa Yang Mendasari Operasi Laser?", "id": "Prinsip Emisi Terstimulasi (Stimulated Emission) Adalah Jawabannya." },
  { "en": "Apa Itu Inversi Populasi Dalam Sistem Laser?", "id": "Kondisi Lebih Banyak Elektron Di Keadaan Tereksitasi." },
  { "en": "Apa Itu Rongga Optik (Optical Cavity)?", "id": "Susunan Cermin Untuk Menguatkan Cahaya Dalam Laser." },
  { "en": "Apa Itu Koherensi Cahaya Yang Dihasilkan Laser?", "id": "Semua Foton Memiliki Fasa Dan Frekuensi Sama." },
  { "en": "Apa Itu Monokromatisitas Cahaya Yang Dihasilkan Laser?", "id": "Cahaya Terdiri Dari Satu Panjang Gelombang Tunggal." },
  { "en": "Apa Itu Kolimasi Cahaya Yang Dihasilkan Laser?", "id": "Sinar Cahaya Berjalan Sangat Paralel Dan Lurus." },
  { "en": "Apa Itu Vertical-Cavity Surface-Emitting Laser (VCSEL)?", "id": "Laser Semikonduktor Yang Memancarkan Cahaya Vertikal." },
  { "en": "Apa Keuntungan Utama Dari Penggunaan VCSEL?", "id": "Dapat Dibuat Dan Diuji Dalam Kepadatan Tinggi." },
  { "en": "Apa Itu Distributed Feedback (DFB) Laser?", "id": "Laser Dengan Struktur Grating Untuk Seleksi Panjang Gelombang." },
  { "en": "Apa Itu Quantum Well Dalam Semikonduktor Heterostruktur?", "id": "Lapisan Tipis Yang Menjebak Elektron Dan Lubang." },
  { "en": "Apa Itu Quantum Wire Dalam Dunia Nanoteknologi?", "id": "Struktur Satu Dimensi Yang Menjebak Partikel." },
  { "en": "Apa Itu Quantum Dot Dalam Dunia Nanoteknologi?", "id": "Struktur Nol Dimensi Yang Menjebak Partikel." },
  { "en": "Apa Itu Sistem-dalam-Paket (System-in-Package, SiP)?", "id": "Mengintegrasikan Beberapa Chip Dalam Satu Paket Tunggal." },
  { "en": "Apa Perbedaan Utama Antara SiP Dan SoC?", "id": "SiP Menggabungkan Chip Berbeda, SoC Satu Chip." },
  { "en": "Apa Itu Teknologi Through-Silicon Via (TSV)?", "id": "Koneksi Vertikal Yang Menembus Wafer Silikon Langsung." },
  { "en": "Apa Tujuan Utama Teknologi TSV Modern?", "id": "Untuk Stacking Chip Tiga Dimensi (3D IC)." },
  { "en": "Apa Itu Interposer Dalam Pengemasan IC 3D?", "id": "Lapisan Penghubung Antara Chip Dan Papan Sirkuit." },
  { "en": "Apa Itu Efek Kedekatan Superkonduktor (Proximity Effect)?", "id": "Sifat Superkonduktor Terinduksi Pada Logam Normal." },
  { "en": "Apa Itu Josephson Junction Dalam Superkonduktor?", "id": "Sambungan Dua Superkonduktor Dipisahkan Lapisan Tipis." },
  { "en": "Apa Itu Superconducting Quantum Interference Device (SQUID)?", "id": "Magnetometer Paling Sensitif Berbasis Sambungan Josephson." },
  { "en": "Apa Itu Coulomb Blockade Dalam Titik Kuantum?", "id": "Penekanan Aliran Arus Akibat Energi Elektrostatik." },
  { "en": "Apa Itu Konduktansi Terkuantisasi (Quantized Conductance)?", "id": "Konduktansi Terjadi Dalam Kelipatan Kuantum Fundamental." },
  { "en": "Apa Itu Efek Hall Kuantum (Quantum Hall Effect)?", "id": "Manifestasi Kuantum Dari Efek Hall Klasik." },
  { "en": "Apa Itu Valleytronics Dalam Dunia Elektronika?", "id": "Pemanfaatan Derajat Kebebasan Lembah (Valley) Elektron." },
  { "en": "Apa Itu Topologi Baterai Switched-Capacitor?", "id": "Rangkaian Untuk Menyeimbangkan Tegangan Sel Baterai." },
  { "en": "Apa Itu Sel Bahan Bakar (Fuel Cell)?", "id": "Perangkat Yang Mengubah Energi Kimia Menjadi Listrik." },
  { "en": "Apa Peran Elektronika Daya Untuk Fuel Cell?", "id": "Mengatur Dan Mengkonversi Output Daya Listrik DC." },
  { "en": "Apa Itu Thermionic Emission Atau Emisi Termionik?", "id": "Pelepasan Elektron Dari Permukaan Logam Panas." },
  { "en": "Apa Itu Field Emission Atau Emisi Medan?", "id": "Pelepasan Elektron Akibat Medan Listrik Eksternal." },
  { "en": "Apa Itu Emisi Sekunder (Secondary Emission)?", "id": "Pelepasan Elektron Akibat Tumbukan Partikel Lain." },
  { "en": "Apa Itu Tabung Hampa (Vacuum Tube)?", "id": "Komponen Aktif Sebelum Era Penemuan Transistor." },
  { "en": "Apa Nama Lain Untuk Tabung Hampa Populer?", "id": "Nama Lainnya Adalah Katup Elektron (Electron Valve)." },
  { "en": "Apa Itu Katoda Dalam Sebuah Tabung Hampa?", "id": "Elemen Yang Memancarkan Elektron Saat Dipanaskan." },
  { "en": "Apa Itu Anoda Dalam Sebuah Tabung Hampa?", "id": "Elemen Yang Menerima Aliran Elektron (Plate)." },
  { "en": "Apa Itu Grid Kontrol Dalam Tabung Triode?", "id": "Elemen Untuk Mengontrol Aliran Arus Elektron." },
  { "en": "Apa Itu Cathode Ray Tube (CRT)?", "id": "Tabung Hampa Untuk Menampilkan Gambar Visual." },
  { "en": "Apa Itu Magnetron Dalam Dunia Gelombang Mikro?", "id": "Tabung Hampa Pembangkit Daya Microwave Tinggi." },
  { "en": "Apa Itu Klystron Dalam Dunia Gelombang Mikro?", "id": "Tabung Hampa Untuk Penguatan Sinyal Microwave." },
  { "en": "Apa Itu Tabung Tetroda Dalam Elektronika?", "id": "Tabung Hampa Dengan Empat Buah Elemen." },
  { "en": "Apa Itu Tabung Pentoda Dalam Elektronika?", "id": "Tabung Hampa Dengan Lima Buah Elemen." },
  { "en": "Siapa Yang Menemukan Efek Efek Medan?", "id": "Julius Edgar Lilienfeld Adalah Penemu Pertamanya." },
  { "en": "Apa Itu Klasifikasi Ruang Bersih (Cleanroom)?", "id": "Standar Kebersihan Udara Berdasarkan Ukuran Partikel." },
  { "en": "Apa Itu Wafer Chuck Dalam Peralatan Fabrikasi?", "id": "Meja Vakum Untuk Memegang Wafer Tetap." },
  { "en": "Apa Itu Reticle Atau Photomask Fabrikasi?", "id": "Pola Sirkuit Yang Digunakan Dalam Proses Litografi." },
  { "en": "Apa Itu Stepper Dalam Proses Fotolitografi?", "id": "Mesin Yang Memproyeksikan Pola Reticle Ke Wafer." },
  { "en": "Apa Itu Fungsi Wannier Dalam Fisika Kuantum?", "id": "Kumpulan Fungsi Lokal Yang Terkait Fungsi Bloch." },
  { "en": "Apa Itu Osilasi Bloch Dalam Fisika Kuantum?", "id": "Osilasi Elektron Dalam Kisi Kristal Periodik." },
  { "en": "Apa Itu Level Landau Dalam Fisika Kuantum?", "id": "Level Energi Terkuantisasi Akibat Medan Magnet." },
  { "en": "Apa Itu Filter Gyrator-Capacitor Dalam Elektronika?", "id": "Filter Aktif Yang Mensimulasikan Filter LC Pasif." },
  { "en": "Apa Itu Non-Resiprositas Dalam Teori Sirkuit?", "id": "Sifat Jaringan Yang Bergantung Arah Sinyal." },
  { "en": "Apa Itu Teorema Tellegen Dalam Teori Sirkuit?", "id": "Teorema Tentang Konservasi Daya Dalam Jaringan." },
  { "en": "Apa Itu Semikonduktor Chalcopyrite Dalam Material?", "id": "Material Untuk Aplikasi Sel Surya Dan LED." },
  { "en": "Apa Itu Insulator Topologis Dalam Dunia Material?", "id": "Material Yang Bersifat Isolator Di Dalam." },
  { "en": "Apa Itu Power Management Integrated Circuit (PMIC)?", "id": "IC Tunggal Untuk Mengelola Daya Sistem." },
  { "en": "Apa Itu Application-Specific Integrated Circuit (ASIC)?", "id": "IC Yang Didesain Untuk Tugas Sangat Spesifik." },
  { "en": "Apa Itu Field-Programmable Gate Array (FPGA)?", "id": "IC Digital Yang Dapat Dikonfigurasi Ulang." },
  { "en": "Apa Unit Dasar Dalam Sebuah FPGA?", "id": "Blok Logika Terkonfigurasi (Configurable Logic Block)." },
  { "en": "Apa Itu Diagram Mata (Eye Diagram)?", "id": "Tampilan Osiloskop Untuk Menganalisis Sinyal Digital." },
  { "en": "Apa Yang Diindikasikan Oleh Mata Yang Terbuka?", "id": "Kualitas Sinyal Yang Baik Dengan Derau Rendah." },
  { "en": "Apa Itu Jitter Acak (Random Jitter)?", "id": "Variasi Waktu Tak Terduga Tanpa Pola." },
  { "en": "Apa Itu Jitter Deterministik (Deterministic Jitter)?", "id": "Variasi Waktu Yang Memiliki Pola Dan Terikat." },
  { "en": "Apa Itu Focused Ion Beam (FIB)?", "id": "Teknik Untuk Analisis Dan Modifikasi Sirkuit." },
  { "en": "Apa Itu Scanning Electron Microscope (SEM)?", "id": "Mikroskop Untuk Melihat Gambar Permukaan Resolusi Tinggi." },
  { "en": "Apa Itu Dekoherensi Dalam Komputasi Kuantum?", "id": "Hilangnya Sifat Kuantum Akibat Interaksi Lingkungan." },
  { "en": "Apa Itu Koreksi Kesalahan Kuantum (QEC)?", "id": "Teknik Untuk Melindungi Informasi Kuantum Dari Dekoherensi." },
  { "en": "Apa Itu Rangkaian Startup Referensi Bandgap?", "id": "Rangkaian Untuk Memastikan Operasi Bias Yang Benar." },
  { "en": "Apa Itu Common-Mode Feedback (CMFB)?", "id": "Sirkuit Untuk Menstabilkan Titik DC Penguat Diferensial." },
  { "en": "Apa Itu Settling Time Dalam Penguat Analog?", "id": "Waktu Output Mencapai Dan Tetap Dalam Batas." },
  { "en": "Apa Itu Overshoot Dalam Respon Sinyal?", "id": "Saat Sinyal Melebihi Nilai Akhir Stabilnya." },
  { "en": "Apa Itu Ringing Dalam Respon Sinyal?", "id": "Osilasi Yang Tidak Diinginkan Pada Sinyal Output." },
  { "en": "Apa Itu Slew-Induced Distortion (SID)?", "id": "Distorsi Akibat Laju Perubahan Sinyal Terbatas." },
  { "en": "Apa Itu Charge Injection Dalam Saklar Analog?", "id": "Kesalahan Akibat Muatan Dari Gerbang Saklar." },
  { "en": "Apa Itu Clock Feedthrough Dalam Saklar Analog?", "id": "Bocornya Sinyal Clock Ke Jalur Sinyal." },
  { "en": "Apa Itu Bottom-Plate Sampling Dalam Rangkaian?", "id": "Teknik Sampling Untuk Mengurangi Distorsi Sinyal." },
  { "en": "Apa Itu Linearitas Integral (Integral Linearity)?", "id": "Ukuran Penyimpangan Maksimum Fungsi Transfer ADC." },
  { "en": "Apa Itu Linearitas Diferensial (Differential Linearity)?", "id": "Ukuran Keseragaman Ukuran Langkah Pada ADC." },
  { "en": "Apa Itu Missing Code Pada ADC?", "id": "Kode Output Digital Yang Tidak Pernah Muncul." },
  { "en": "Apa Itu Aperture Jitter Dalam Proses Sampling?", "id": "Variasi Waktu Pada Momen Pengambilan Sampel." },
  { "en": "Apa Itu Aliasing Dalam Proses Sampling Sinyal?", "id": "Frekuensi Tinggi Menyamar Sebagai Frekuensi Rendah." },
  { "en": "Apa Itu Teorema Sampling Nyquist-Shannon?", "id": "Frekuensi Sampling Harus Dua Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Over-sampling Dalam Konversi Data?", "id": "Sampling Pada Frekuensi Jauh Di Atas Nyquist." },
  { "en": "Apa Itu Noise Shaping Dalam Konverter Delta-Sigma?", "id": "Memindahkan Derau Kuantisasi Ke Frekuensi Tinggi." },
  { "en": "Apa Itu Decimation Filter Dalam Konverter Data?", "id": "Filter Untuk Mengurangi Laju Data Sampel." },
  { "en": "Apa Itu Glitching Pada Output DAC?", "id": "Lonjakan Energi Transien Saat Kode Input Berubah." },
  { "en": "Apa Itu Kode Termometer Dalam Desain DAC?", "id": "Representasi Unary Yang Mengurangi Efek Glitching." },
  { "en": "Apa Itu Dynamic Range Dalam Sistem Sinyal?", "id": "Rasio Antara Sinyal Terbesar Dan Terkecil." },
  { "en": "Apa Itu Signal-to-Noise Ratio (SNR)?", "id": "Rasio Daya Sinyal Terhadap Daya Derau." },
  { "en": "Apa Itu Signal-to-Noise and Distortion Ratio (SINAD)?", "id": "Rasio Sinyal Terhadap Derau Dan Distorsi." },
  { "en": "Apa Itu Effective Number of Bits (ENOB)?", "id": "Ukuran Kinerja Aktual Sebuah Konverter Data." },
  { "en": "Apa Itu Kalibrasi Latar Belakang (Background Calibration)?", "id": "Kalibrasi Yang Dilakukan Saat Sirkuit Beroperasi." },
  { "en": "Apa Itu Kalibrasi Latar Depan (Foreground Calibration)?", "id": "Kalibrasi Yang Dilakukan Sebelum Operasi Normal." },
  { "en": "Apa Itu Chopping Dalam Desain Penguat Analog?", "id": "Teknik Modulasi Untuk Menghilangkan Offset Dan Derau." },
  { "en": "Apa Itu Korelasi Dalam Sinyal Atau Derau?", "id": "Tingkat Hubungan Statistik Antara Dua Sinyal." },
  { "en": "Apa Itu Derau Putih (White Noise)?", "id": "Derau Dengan Kerapatan Spektral Daya Konstan." },
  { "en": "Apa Itu Derau Merah Jambu (Pink Noise)?", "id": "Derau Dengan Kerapatan Spektral Berbanding Terbalik." },
  { "en": "Apa Nama Lain Untuk Derau Merah Jambu?", "id": "Nama Lainnya Adalah Derau Flicker (1/f)." },
  { "en": "Apa Itu Derau Coklat (Brownian Noise)?", "id": "Derau Dengan Kerapatan Spektral Berbanding 1/f²." },
  { "en": "Apa Itu Preamplifier Dalam Rantai Sinyal?", "id": "Penguat Pertama Untuk Sinyal Tingkat Rendah." },
  { "en": "Apa Itu Automatic Gain Control (AGC)?", "id": "Rangkaian Untuk Menjaga Level Output Konstan." },
  { "en": "Apa Itu Variable-Gain Amplifier (VGA)?", "id": "Penguat Dengan Penguatan Yang Dapat Diatur." },
  { "en": "Apa Itu Logarithmic Amplifier Atau Penguat?", "id": "Penguat Dengan Output Sebanding Dengan Logaritma." },
  { "en": "Apa Itu Limiting Amplifier Dalam Rangkaian RF?", "id": "Penguat Yang Mencegah Amplitudo Melebihi Batas." },
  { "en": "Apa Itu Low-Noise Amplifier (LNA)?", "id": "Penguat RF Didesain Untuk Kebisingan Minimal." },
  { "en": "Apa Itu Power Amplifier (PA)?", "id": "Penguat Tahap Akhir Untuk Menggerakkan Beban." },
  { "en": "Apa Itu Efisiensi Drain Dari Power Amplifier?", "id": "Rasio Daya Output RF Terhadap Daya DC." },
  { "en": "Apa Itu Power Added Efficiency (PAE)?", "id": "Ukuran Efisiensi Yang Memperhitungkan Daya Input." },
  { "en": "Apa Itu Efek Memori Dalam Power Amplifier?", "id": "Perilaku Saat Ini Bergantung Pada Sinyal Sebelumnya." },
  { "en": "Apa Itu Tantalum Capacitor Atau Kapasitor?", "id": "Jenis Kapasitor Elektrolit Dengan Kinerja Tinggi." },
  { "en": "Apa Itu Kapasitor Keramik Multilayer (MLCC)?", "id": "Jenis Kapasitor Paling Umum Dalam Elektronika." },
  { "en": "Apa Itu Equivalent Series Resistance (ESR)?", "id": "Resistansi Parasitik Internal Dalam Sebuah Kapasitor." },
  { "en": "Apa Itu Equivalent Series Inductance (ESL)?", "id": "Induktansi Parasitik Internal Dalam Sebuah Kapasitor." },
  { "en": "Apa Itu Faktor Disipasi (Dissipation Factor)?", "id": "Ukuran Kerugian Energi Dalam Sebuah Kapasitor." },
  { "en": "Apa Itu Koefisien Temperatur Kapasitansi?", "id": "Ukuran Perubahan Kapasitansi Terhadap Perubahan Suhu." },
  { "en": "Apa Itu Penyerapan Dielektrik (Dielectric Absorption)?", "id": "Efek Memori Muatan Dalam Insulator Kapasitor." },
  { "en": "Apa Itu Piezoelectricity Dalam Bahan Keramik?", "id": "Timbulnya Tegangan Akibat Tekanan Pada Bahan." },
  { "en": "Apa Itu Induktor Inti Udara (Air-Core)?", "id": "Induktor Tanpa Material Inti Magnetik Didalamnya." },
  { "en": "Apa Itu Induktor Inti Ferit (Ferrite-Core)?", "id": "Induktor Dengan Inti Dari Material Ferit." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Kecenderungan Arus AC Mengalir Di Permukaan." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor)?", "id": "Ukuran Kualitas Sebuah Komponen Resonansi." },
  { "en": "Apa Itu Frekuensi Resonansi Diri (SRF)?", "id": "Frekuensi Dimana Induktor Mulai Berperilaku Kapasitif." },
  { "en": "Apa Itu Arus Saturasi Dalam Sebuah Induktor?", "id": "Arus Yang Menyebabkan Penurunan Induktansi Drastis." },
  { "en": "Apa Itu Histeresis Magnetik Dalam Material Inti?", "id": "Ketergantungan Keadaan Magnetik Pada Riwayat Sebelumnya." },
  { "en": "Apa Itu Kerugian Inti (Core Loss)?", "id": "Kerugian Energi Dalam Inti Akibat Medan Magnet." },
  { "en": "Apa Itu Resistansi DC (DCR) Induktor?", "id": "Resistansi Lilitan Kawat Pada Arus DC." },
  { "en": "Apa Itu Kopling Mutual Antara Dua Induktor?", "id": "Interaksi Medan Magnet Antara Induktor Berdekatan." },
  { "en": "Apa Itu Transformator Dalam Dunia Elektronika?", "id": "Perangkat Pasif Untuk Mentransfer Energi Listrik." },
  { "en": "Apa Prinsip Dasar Kerja Sebuah Transformator?", "id": "Prinsip Induksi Elektromagnetik Timbal Balik Faraday." },
  { "en": "Apa Itu Rasio Lilitan Dalam Sebuah Transformator?", "id": "Perbandingan Jumlah Lilitan Primer Dan Sekunder." },
  { "en": "Apa Itu Transformator Step-Up Menaikkan Tegangan?", "id": "Meningkatkan Tegangan Dengan Menurunkan Arus Listrik." },
  { "en": "Apa Itu Transformator Step-Down Menurunkan Tegangan?", "id": "Menurunkan Tegangan Dengan Meningkatkan Arus Listrik." },
  { "en": "Apa Itu Transformator Isolasi Dalam Elektronika?", "id": "Transformator Dengan Rasio Lilitan Satu Banding Satu." },
  { "en": "Apa Tujuan Utama Dari Transformator Isolasi?", "id": "Untuk Isolasi Galvani Antara Dua Rangkaian." }



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
