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


  { "en": "Apa Itu Trafo?", "id": "Alat Untuk Mengubah Tegangan Listrik AC." },
  { "en": "Prinsip Kerja Trafo?", "id": "Induksi Elektromagnetik Faraday Dan Lenz." },
  { "en": "Komponen Utama Trafo?", "id": "Inti Besi Dan Dua Kumparan." },
  { "en": "Nama Dua Kumparan?", "id": "Kumparan Primer Dan Kumparan Sekunder." },
  { "en": "Fungsi Inti Besi?", "id": "Memusatkan Fluks Magnet Agar Tidak Bocor." },
  { "en": "Bahan Inti Besi?", "id": "Lempengan Besi Lunak Atau Baja Silikon." },
  { "en": "Kenapa Inti Berlapis?", "id": "Untuk Mengurangi Arus Eddy Atau Pusar." },
  { "en": "Apa Itu Kumparan Primer?", "id": "Kumparan Tempat Masuknya Tegangan Awal." },
  { "en": "Apa Itu Kumparan Sekunder?", "id": "Kumparan Tempat Keluarnya Tegangan Hasil." },
  { "en": "Jenis Trafo Berdasarkan Tegangan?", "id": "Step-Up Dan Step-Down." },
  { "en": "Fungsi Trafo Step-Up?", "id": "Menaikkan Tegangan Listrik Arus Bolak-Balik." },
  { "en": "Fungsi Trafo Step-Down?", "id": "Menurunkan Tegangan Listrik Arus Bolak-Balik." },
  { "en": "Ciri Trafo Step-Up?", "id": "Lilitan Sekunder Lebih Banyak Dari Primer." },
  { "en": "Ciri Trafo Step-Down?", "id": "Lilitan Primer Lebih Banyak Dari Sekunder." },
  { "en": "Apa Itu Induksi Elektromagnetik?", "id": "Gejala Timbulnya Arus Listrik Induksi." },
  { "en": "Siapa Penemu Prinsip Induksi?", "id": "Michael Faraday Pada Tahun 1831." },
  { "en": "Apa Itu Fluks Magnetik?", "id": "Ukuran Total Garis Gaya Magnetik." },
  { "en": "Satuan Fluks Magnetik?", "id": "Weber Disimbolkan Dengan Simbol Wb." },
  { "en": "Bagaimana Trafo Bekerja?", "id": "Arus AC Primer Hasilkan Fluks Magnet." },
  { "en": "Apa Yang Terjadi Di Sekunder?", "id": "Fluks Magnet Induksikan Tegangan Di Sekunder." },
  { "en": "Apakah Trafo Bekerja Pada DC?", "id": "Tidak Hanya Bekerja Pada Arus AC." },
  { "en": "Kenapa Tidak Bekerja Pada DC?", "id": "Arus DC Tidak Menghasilkan Fluks Berubah." },
  { "en": "Apa Itu Trafo Ideal?", "id": "Trafo Tanpa Kehilangan Daya Sama Sekali." },
  { "en": "Efisiensi Trafo Ideal?", "id": "Seratus Persen Efisien Tanpa Rugi." },
  { "en": "Rumus Tegangan Trafo?", "id": "Vp Per Vs Sama Dengan Np Per Ns." },
  { "en": "Rumus Arus Trafo?", "id": "Ip Per Is Sama Dengan Ns Per Np." },
  { "en": "Apa Itu Np?", "id": "Jumlah Lilitan Pada Kumparan Primer." },
  { "en": "Apa Itu Ns?", "id": "Jumlah Lilitan Pada Kumparan Sekunder." },
  { "en": "Apa Itu Vp?", "id": "Tegangan Pada Kumparan Bagian Primer." },
  { "en": "Apa Itu Vs?", "id": "Tegangan Pada Kumparan Bagian Sekunder." },
  { "en": "Apa Itu Ip?", "id": "Arus Listrik Pada Kumparan Primer." },
  { "en": "Apa Itu Is?", "id": "Arus Listrik Pada Kumparan Sekunder." },
  { "en": "Hubungan Tegangan Dan Lilitan?", "id": "Tegangan Sebanding Dengan Jumlah Lilitan." },
  { "en": "Hubungan Arus Dan Lilitan?", "id": "Arus Berbanding Terbalik Dengan Jumlah Lilitan." },
  { "en": "Daya Masukan Trafo Ideal?", "id": "Sama Dengan Daya Keluaran Trafo Ideal." },
  { "en": "Rumus Daya Trafo?", "id": "Daya Sama Dengan Tegangan Kali Arus." },
  { "en": "Apa Itu Efisiensi Trafo?", "id": "Perbandingan Daya Keluar Dengan Daya Masuk." },
  { "en": "Simbol Efisiensi Trafo?", "id": "Simbol Eta Atau Huruf Yunani η." },
  { "en": "Penyebab Rugi Pada Trafo?", "id": "Rugi Tembaga Rugi Besi Dan Histeresis." },
  { "en": "Apa Itu Rugi Tembaga?", "id": "Panas Akibat Resistansi Pada Kawat Kumparan." },
  { "en": "Bagaimana Mengurangi Rugi Tembaga?", "id": "Menggunakan Kawat Tembaga Diameter Besar." },
  { "en": "Apa Itu Rugi Arus Eddy?", "id": "Arus Pusar Yang Timbul Di Inti." },
  { "en": "Bagaimana Mengurangi Arus Eddy?", "id": "Memakai Inti Besi Dari Pelat Tipis." },
  { "en": "Apa Itu Rugi Histeresis?", "id": "Energi Saat Magnetisasi Inti Dibalikkan." },
  { "en": "Bagaimana Mengurangi Rugi Histeresis?", "id": "Menggunakan Bahan Inti Dengan Kurva Sempit." },
  { "en": "Apa Fungsi Minyak Trafo?", "id": "Sebagai Pendingin Dan Isolasi Listrik." },
  { "en": "Jenis Pendinginan Trafo?", "id": "Pendinginan Alami Dan Pendinginan Paksa." },
  { "en": "Apa Itu Autotransformator?", "id": "Trafo Dengan Satu Kumparan Saja." },
  { "en": "Fungsi Tap Changer?", "id": "Mengubah Rasio Lilitan Tanpa Mematikan." },
  { "en": "Trafo Tiga Fasa?", "id": "Digunakan Untuk Sistem Tenaga Tiga Fasa." },
  { "en": "Aplikasi Trafo Step-Down?", "id": "Adaptor Pengisi Daya Dan Bel Listrik." },
  { "en": "Aplikasi Trafo Step-Up?", "id": "Pembangkit Listrik Dan Televisi Tabung." },
  { "en": "Di Mana Letak Primer?", "id": "Terhubung Dengan Sumber Tegangan AC." },
  { "en": "Di Mana Letak Sekunder?", "id": "Terhubung Dengan Beban Atau Perangkat Listrik." },
  { "en": "Apa Satuan Daya?", "id": "Watt Atau Volt-Ampere Tergantung Jenisnya." },
  { "en": "Bagaimana Daya Ditransfer?", "id": "Melalui Medan Magnet Di Inti Besi." },
  { "en": "Apakah Frekuensi Berubah?", "id": "Tidak Frekuensi Listrik Tetap Sama." },
  { "en": "Bentuk Inti Trafo?", "id": "Bentuk Inti Kotak Dan Bentuk Cangkang." },
  { "en": "Apa Itu Trafo Isolasi?", "id": "Rasio Lilitan Primer Sekunder Satu Banding Satu." },
  { "en": "Fungsi Trafo Isolasi?", "id": "Memisahkan Rangkaian Untuk Keamanan Listrik." },
  { "en": "Apa Itu Belitan?", "id": "Istilah Lain Untuk Menyebut Kumparan Kawat." },
  { "en": "Apa Itu GGL Induksi?", "id": "Gaya Gerak Listrik Yang Diinduksikan." },
  { "en": "Hukum Apa Yang Terkait?", "id": "Hukum Faraday Tentang Induksi Elektromagnetik." },
  { "en": "Simbol Trafo Pada Diagram?", "id": "Dua Lingkaran Berhadapan Dengan Garis." },
  { "en": "Apa Itu Rasio Transformasi?", "id": "Perbandingan Antara Lilitan Primer Sekunder." },
  { "en": "Bagaimana Step-Up Mempengaruhi Arus?", "id": "Tegangan Naik Maka Arus Akan Turun." },
  { "en": "Bagaimana Step-Down Mempengaruhi Arus?", "id": "Tegangan Turun Maka Arus Akan Naik." },
  { "en": "Apa Itu Trafo Toroidal?", "id": "Trafo Dengan Inti Besi Berbentuk Donat." },
  { "en": "Keuntungan Trafo Toroidal?", "id": "Medan Magnet Bocor Sangat Kecil." },
  { "en": "Bahan Kawat Kumparan?", "id": "Umumnya Terbuat Dari Bahan Logam Tembaga." },
  { "en": "Kenapa Disebut Transformer?", "id": "Karena Dapat Mentransformasi Besaran Tegangan." },
  { "en": "Apa Yang Diukur Voltmeter?", "id": "Mengukur Besaran Tegangan Listrik AC." },
  { "en": "Apa Yang Diukur Amperemeter?", "id": "Mengukur Besaran Arus Listrik AC." },
  { "en": "Pentingnya Trafo Dalam Distribusi?", "id": "Mengurangi Kerugian Daya Jarak Jauh." },
  { "en": "Bagaimana Distribusi Dilakukan?", "id": "Tegangan Dinaikkan Untuk Jarak Jauh." },
  { "en": "Lalu Di Dekat Pengguna?", "id": "Tegangan Diturunkan Kembali Sesuai Kebutuhan." },
  { "en": "Apa Itu Kebocoran Fluks?", "id": "Fluks Magnet Tidak Melewati Inti Besi." },
  { "en": "Dampak Kebocoran Fluks?", "id": "Mengurangi Efisiensi Kinerja Transformator Listrik." },
  { "en": "Apa Itu Polaritas Trafo?", "id": "Arah Sesaat Tegangan Kumparan Primer." },
  { "en": "Jenis Polaritas Trafo?", "id": "Polaritas Aditif Dan Polaritas Subtraktif." },
  { "en": "Peralatan Elektronik Menggunakan Trafo?", "id": "Hampir Semua Peralatan Elektronik Menggunakannya." },
  { "en": "Contoh Di Rumah Tangga?", "id": "Charger Laptop Dan Adaptor Telepon Genggam." },
  { "en": "Apa Itu Trafo Arus?", "id": "Untuk Mengukur Arus Besar Secara Aman." },
  { "en": "Apa Itu Trafo Tegangan?", "id": "Untuk Mengukur Tegangan Tinggi Secara Aman." },
  { "en": "Prinsip Trafo Arus?", "id": "Bekerja Seperti Trafo Step-Up Tegangan." },
  { "en": "Apa Itu Inti Udara?", "id": "Trafo Tanpa Inti Besi Sama Sekali." },
  { "en": "Kapan Inti Udara Digunakan?", "id": "Pada Aplikasi Frekuensi Sangat Tinggi." },
  { "en": "Apakah Trafo Menghasilkan Energi?", "id": "Tidak Hanya Memindahkan Energi Listrik." },
  { "en": "Bunyi Dengung Pada Trafo?", "id": "Getaran Inti Besi Akibat Medan Magnet." },
  { "en": "Apa Itu Magnetostriksi?", "id": "Perubahan Dimensi Bahan Karena Medan Magnet." },
  { "en": "Apa Itu Bushing Trafo?", "id": "Isolator Untuk Menyalurkan Listrik Keluar." },
  { "en": "Bahan Bushing Trafo?", "id": "Terbuat Dari Bahan Porselen Atau Kaca." },
  { "en": "Apa Itu Konservator?", "id": "Tangki Cadangan Minyak Pada Transformator." },
  { "en": "Fungsi Konservator Trafo?", "id": "Menampung Pemuaian Minyak Akibat Panas." },
  { "en": "Penyebab Trafo Panas?", "id": "Adanya Rugi-Rugi Daya Menjadi Panas." },
  { "en": "Perawatan Utama Trafo?", "id": "Pengecekan Kualitas Minyak Secara Berkala." },
  { "en": "Apa Inti Laminasi?", "id": "Mencegah Arus Eddy Yang Berlebihan." },
  { "en": "Fungsi Kertas Isolasi?", "id": "Mencegah Hubung Singkat Antar Lilitan." },
  { "en": "Apa Itu Trafo Daya?", "id": "Trafo Untuk Transmisi Daya Listrik Besar." },
  { "en": "Apa Itu Trafo Distribusi?", "id": "Menurunkan Tegangan Untuk Konsumen Akhir." },
  { "en": "Perbedaan Trafo Daya Dan Distribusi?", "id": "Trafo Daya Untuk Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Tegangan Impuls?", "id": "Lonjakan Tegangan Sangat Cepat Dan Singkat." },
  { "en": "Penyebab Tegangan Impuls?", "id": "Sambaran Petir Atau Proses Switching." },
  { "en": "Apa Itu Impedansi Trafo?", "id": "Ukuran Oposisi Terhadap Arus Bolak-Balik." },
  { "en": "Satuan Impedansi Trafo?", "id": "Dinyatakan Dalam Persentase Dari Rating Dasarnya." },
  { "en": "Pentingnya Impedansi Trafo?", "id": "Menentukan Arus Hubung Singkat Maksimum." },
  { "en": "Apa Itu Regulasi Tegangan?", "id": "Perubahan Tegangan Sekunder Dari Tanpa Beban." },
  { "en": "Regulasi Tegangan Baik?", "id": "Nilai Persentase Kecil Menandakan Kinerja Baik." },
  { "en": "Apa Itu Arus Inrush?", "id": "Arus Magnetisasi Awal Yang Sangat Tinggi." },
  { "en": "Kapan Arus Inrush Terjadi?", "id": "Saat Trafo Pertama Kali Diberi Energi." },
  { "en": "Apa Itu Uji Hubung Singkat?", "id": "Untuk Menentukan Impedansi Dan Rugi Tembaga." },
  { "en": "Apa Itu Uji Tanpa Beban?", "id": "Untuk Menentukan Rugi Besi Dan Arus." },
  { "en": "Bagaimana Uji Tanpa Beban?", "id": "Sisi Sekunder Dibiarkan Terbuka Tanpa Beban." },
  { "en": "Bagaimana Uji Hubung Singkat?", "id": "Sisi Sekunder Dihubung Singkatkan Dengan Aman." },
  { "en": "Apa Itu Nameplate Trafo?", "id": "Pelat Informasi Spesifikasi Teknis Trafo." },
  { "en": "Informasi Di Nameplate?", "id": "Daya Tegangan Arus Impedansi Dan Lainnya." },
  { "en": "Satuan Daya Trafo?", "id": "Umumnya Dalam Kilo Volt Ampere (kVA)." },
  { "en": "Kenapa Satuan kVA?", "id": "Karena Beban Bisa Bersifat Induktif Kapasitif." },
  { "en": "Hubungan Kumparan Tiga Fasa?", "id": "Hubungan Bintang (Y) Atau Segitiga (Δ)." },
  { "en": "Apa Itu Hubungan Bintang?", "id": "Tiga Ujung Kumparan Terhubung Jadi Satu." },
  { "en": "Apa Itu Hubungan Segitiga?", "id": "Ujung Kumparan Terhubung Secara Seri Berantai." },
  { "en": "Apa Itu Buchholz Relay?", "id": "Alat Pengaman Trafo Berbasis Gas Terlarut." },
  { "en": "Fungsi Buchholz Relay?", "id": "Mendeteksi Gangguan Internal Pada Transformator." },
  { "en": "Di Mana Letak Buchholz Relay?", "id": "Antara Tangki Utama Dan Tangki Konservator." },
  { "en": "Apa Itu Silica Gel Breather?", "id": "Mengeringkan Udara Yang Masuk Ke Trafo." },
  { "en": "Kenapa Udara Harus Kering?", "id": "Kelembaban Dapat Merusak Kualitas Minyak Isolasi." },
  { "en": "Warna Silica Gel Baik?", "id": "Berwarna Biru Saat Kondisi Kering." },
  { "en": "Warna Silica Gel Jenuh?", "id": "Berubah Menjadi Merah Muda Atau Putih." },
  { "en": "Apa Itu Uji Rasio?", "id": "Memastikan Rasio Lilitan Sesuai Spesifikasi." },
  { "en": "Apa Itu Induktansi Bersama?", "id": "Prinsip Dasar Transfer Energi Antar Kumparan." },
  { "en": "Bagaimana Panas Dibuang?", "id": "Melalui Minyak Dan Sirip Pendingin Radiator." },
  { "en": "Apa Itu ONAN?", "id": "Oil Natural Air Natural Cooling System." },
  { "en": "Apa Itu ONAF?", "id": "Oil Natural Air Forced Cooling System." },
  { "en": "Apa Maksud Air Forced?", "id": "Pendinginan Udara Dibantu Dengan Kipas Angin." },
  { "en": "Apakah Inti Trafo Magnet Permanen?", "id": "Bukan Merupakan Sebuah Elektromagnet Sementara." },
  { "en": "Apa Itu Tegangan Tembus?", "id": "Batas Tegangan Maksimum Bahan Isolasi." },
  { "en": "Pentingnya Tegangan Tembus Minyak?", "id": "Menentukan Kemampuan Isolasi Minyak Trafo." },
  { "en": "Frekuensi Standar Di Indonesia?", "id": "Lima Puluh Hertz Untuk Sistem Kelistrikan." },
  { "en": "Pengaruh Frekuensi Pada Trafo?", "id": "Desain Trafo Disesuaikan Frekuensi Sistem." },
  { "en": "Apa Itu Faktor Daya?", "id": "Rasio Daya Aktif Dengan Daya Semu." },
  { "en": "Trafo Step Up Menaikkan Apa?", "id": "Hanya Menaikkan Tegangan Bukan Menaikkan Daya." },
  { "en": "Trafo Step Down Menurunkan Apa?", "id": "Hanya Menurunkan Tegangan Bukan Menurunkan Daya." },
  { "en": "Apa Itu Trafo Zig-Zag?", "id": "Trafo Khusus Untuk Pengetanahan Dan Harmonisa." },
  { "en": "Apa Itu Harmonisa?", "id": "Distorsi Gelombang Sinusoidal Akibat Beban Non-Linier." },
  { "en": "Dampak Harmonisa Pada Trafo?", "id": "Menyebabkan Pemanasan Berlebih Pada Transformator." },
  { "en": "Apa Itu Terminal Trafo?", "id": "Titik Koneksi Untuk Kabel Masuk Keluar." },
  { "en": "Bahan Terminal Trafo?", "id": "Biasanya Terbuat Dari Tembaga Atau Kuningan." },
  { "en": "Apa Itu Grounding Trafo?", "id": "Menghubungkan Titik Netral Ke Tanah." },
  { "en": "Tujuan Grounding Trafo?", "id": "Untuk Keamanan Dan Stabilitas Sistem Listrik." },
  { "en": "Konsep Kekekalan Energi?", "id": "Energi Masuk Sama Dengan Energi Keluar." },
  { "en": "Rugi-Rugi Diubah Menjadi Apa?", "id": "Diubah Menjadi Energi Panas Yang Terbuang." },
  { "en": "Apa Itu Medan Elektromagnetik?", "id": "Medan Yang Dihasilkan Oleh Arus Listrik." },
  { "en": "Apa Itu Arus Beban Penuh?", "id": "Arus Maksimum Saat Trafo Beroperasi Penuh." },
  { "en": "Apa Itu Overload Trafo?", "id": "Trafo Diberi Beban Melebihi Kapasitasnya." },
  { "en": "Akibat Overload Trafo?", "id": "Pemanasan Berlebih Dan Kerusakan Isolasi." },
  { "en": "Apa Itu Pelindung Tekanan Lebih?", "id": "Mengamankan Trafo Dari Tekanan Gas Berlebih." },
  { "en": "Apa Itu Indikator Level Minyak?", "id": "Menunjukkan Ketinggian Minyak Dalam Tangki." },
  { "en": "Kenapa Level Minyak Penting?", "id": "Memastikan Pendinginan Dan Isolasi Tetap Efektif." },
  { "en": "Apa Itu Uji Tahanan Isolasi?", "id": "Mengukur Kualitas Tahanan Bahan Isolasi." },
  { "en": "Alat Uji Tahanan Isolasi?", "id": "Megger Atau Insulation Resistance Tester." },
  { "en": "Satuan Tahanan Isolasi?", "id": "Megaohm Atau Gigaohm Bergantung Hasilnya." },
  { "en": "Apa Itu Gaya Gerak Magnet?", "id": "Penyebab Timbulnya Fluks Magnetik Di Inti." },
  { "en": "Rumus Gaya Gerak Magnet?", "id": "Jumlah Lilitan Dikalikan Dengan Arus Listrik." },
  { "en": "Apa Itu Rangkaian Magnetik?", "id": "Jalur Tertutup Untuk Fluks Magnetik." },
  { "en": "Inti Besi Sebagai Apa?", "id": "Sebagai Rangkaian Magnetik Dengan Reluktansi Rendah." },
  { "en": "Apa Itu Reluktansi?", "id": "Ukuran Hambatan Terhadap Fluks Magnetik." },
  { "en": "Bahan Reluktansi Rendah?", "id": "Bahan Feromagnetik Seperti Besi Lunak." },
  { "en": "Apakah Posisi Kumparan Penting?", "id": "Ya Untuk Memaksimalkan Kopling Fluks Magnet." },
  { "en": "Apa Itu Kopling Fluks?", "id": "Seberapa Baik Fluks Primer Mencapai Sekunder." },
  { "en": "Kopling Fluks Trafo Ideal?", "id": "Kopling Sempurna Tanpa Adanya Kebocoran Fluks." },
  { "en": "Penggunaan Trafo Pada Pengelasan?", "id": "Menurunkan Tegangan Dan Menaikkan Arus Besar." },
  { "en": "Bagaimana Menaikkan Efisiensi?", "id": "Meminimalkan Semua Jenis Rugi-Rugi Daya." },
  { "en": "Apa Itu Trafo Instrumen?", "id": "Trafo Arus Dan Trafo Tegangan Gabungan." },
  { "en": "Tujuan Trafo Instrumen?", "id": "Mengisolasi Dan Menurunkan Besaran Untuk Pengukuran." },
  { "en": "Kelas Akurasi Trafo Instrumen?", "id": "Menentukan Tingkat Ketelitian Hasil Pengukuran." },
  { "en": "Apa Itu Beban (Burden)?", "id": "Kapasitas Beban Terhubung Ke Trafo Instrumen." },
  { "en": "Satuan Burden Trafo?", "id": "Dinyatakan Dalam Satuan Volt-Ampere (VA)." },
  { "en": "Apa Itu Saturasi Inti?", "id": "Kondisi Inti Tak Mampu Menambah Fluks." },
  { "en": "Akibat Saturasi Inti?", "id": "Distorsi Bentuk Gelombang Dan Arus Tinggi." },
  { "en": "Penyebab Saturasi Inti?", "id": "Tegangan Berlebih Atau Komponen Arus DC." },
  { "en": "Bagaimana Mencegah Saturasi?", "id": "Mendesain Inti Dengan Ukuran Yang Cukup." },
  { "en": "Apa Itu Trafo Audio?", "id": "Untuk Menyesuaikan Impedansi Antar Perangkat Audio." },
  { "en": "Fungsi Lain Trafo Audio?", "id": "Mengisolasi Sinyal Dan Memblokir Arus DC." },
  { "en": "Apa Itu Permeabilitas Magnetik?", "id": "Kemampuan Bahan Untuk Dilewati Fluks Magnet." },
  { "en": "Inti Besi Punya Permeabilitas?", "id": "Ya Memiliki Permeabilitas Magnetik Sangat Tinggi." },
  { "en": "Udara Punya Permeabilitas?", "id": "Ya Tapi Jauh Lebih Rendah Dari Besi." },
  { "en": "Apa Itu Radiator Trafo?", "id": "Struktur Sirip Untuk Melepas Panas Minyak." },
  { "en": "Proses Pelepasan Panas?", "id": "Secara Konveksi Dan Radiasi Ke Udara." },
  { "en": "Apa Itu Dielektrik?", "id": "Bahan Isolator Listrik Yang Dapat dipolarisasi." },
  { "en": "Minyak Trafo Adalah?", "id": "Bahan Dielektrik Cair Sekaligus Sebagai Pendingin." },
  { "en": "Apa Itu Ketukan (Tap)?", "id": "Koneksi Tambahan Pada Kumparan Untuk Rasio." },
  { "en": "Apa Itu Off-Load Tap Changer?", "id": "Penggantian Tap Saat Trafo Tidak Bertegangan." },
  { "en": "Apa Itu On-Load Tap Changer?", "id": "Penggantian Tap Saat Trafo Masih Beroperasi." },
  { "en": "Aplikasi On-Load Tap Changer?", "id": "Pada Trafo Daya Besar Untuk Regulasi." },
  { "en": "Apakah Trafo Bisa Meledak?", "id": "Ya Jika Terjadi Gangguan Internal Parah." },
  { "en": "Penyebab Utama Ledakan?", "id": "Tekanan Gas Sangat Tinggi Akibat Hubung Singkat." },
  { "en": "Apa Itu Grup Vektor?", "id": "Menunjukkan Pergeseran Fasa Tegangan Trafo." },
  { "en": "Contoh Grup Vektor?", "id": "Dyn11 Yyn0 Dan Lain Sebagainya." },
  { "en": "Apa Arti Dyn11?", "id": "Delta Primer Bintang Sekunder Geser 30 Derajat." },
  { "en": "Pentingnya Grup Vektor?", "id": "Untuk Operasi Paralel Antar Transformator." },
  { "en": "Syarat Paralel Trafo?", "id": "Rasio Tegangan Dan Grup Vektor Sama." },
  { "en": "Apa Itu Belitan Tersier?", "id": "Belitan Ketiga Selain Primer Dan Sekunder." },
  { "en": "Fungsi Belitan Tersier?", "id": "Menekan Harmonisa Dan Menstabilkan Tegangan Netral." },
  { "en": "Apa Itu Trafo Kering?", "id": "Trafo Tanpa Menggunakan Pendingin Minyak Cair." },
  { "en": "Pendingin Trafo Kering?", "id": "Menggunakan Udara Atau Resin Sebagai Isolatornya." },
  { "en": "Kelebihan Trafo Kering?", "id": "Risiko Kebakaran Rendah Dan Bebas Perawatan." },
  { "en": "Kekurangan Trafo Kering?", "id": "Ukuran Lebih Besar Dan Biaya Awal Mahal." },
  { "en": "Apa Itu SFRA?", "id": "Sweep Frequency Response Analysis Test." },
  { "en": "Tujuan Uji SFRA?", "id": "Mendeteksi Deformasi Mekanis Pada Belitan." },
  { "en": "Apa Itu DGA?", "id": "Dissolved Gas Analysis Atau Analisa Gas." },
  { "en": "Sampel Uji DGA?", "id": "Mengambil Sampel Minyak Dari Dalam Trafo." },
  { "en": "Tujuan Uji DGA?", "id": "Mendeteksi Jenis Gangguan Internal Sejak Dini." },
  { "en": "Gas Indikasi Overheating?", "id": "Etana (C2H4) Dan Metana (CH4)." },
  { "en": "Gas Indikasi Percikan Api?", "id": "Asetilena (C2H2) Adalah Gas Utama." },
  { "en": "Apa Itu Uji Tan Delta?", "id": "Mengukur Faktor Disipasi Dari Bahan Isolasi." },
  { "en": "Tan Delta Mengindikasikan Apa?", "id": "Tingkat Kontaminasi Dan Penuaan Isolasi." },
  { "en": "Apa Itu Partial Discharge?", "id": "Pelepasan Sebagian Muatan Listrik Lokal." },
  { "en": "Akibat Partial Discharge?", "id": "Merusak Dan Mendegradasi Material Isolasi." },
  { "en": "Jenis Konstruksi Inti?", "id": "Tipe Inti (Core Type) Dan Cangkang." },
  { "en": "Ciri Tipe Inti?", "id": "Belitan Mengelilingi Kaki-Kaki Inti Besi." },
  { "en": "Ciri Tipe Cangkang?", "id": "Inti Besi Mengelilingi Belitan Kawat." },
  { "en": "Apa Itu Efisiensi Harian?", "id": "Efisiensi Trafo Selama Siklus 24 Jam." },
  { "en": "Pentingnya Efisiensi Harian?", "id": "Penting Untuk Trafo Distribusi Beban Bervariasi." },
  { "en": "Komponen Arus Tanpa Beban?", "id": "Komponen Magnetisasi Dan Komponen Rugi Besi." },
  { "en": "Apa Itu Sambungan Scott-T?", "id": "Menghubungkan Sistem Tiga Fasa Ke Dua Fasa." },
  { "en": "Apa Itu Sambungan Open Delta?", "id": "Menggunakan Dua Trafo Untuk Beban Tiga Fasa." },
  { "en": "Kapasitas Sambungan Open Delta?", "id": "Sekitar 57.7% Dari Kapasitas Tiga Trafo." },
  { "en": "Apa Itu Lightning Arrester?", "id": "Pelindung Peralatan Dari Tegangan Lebih Petir." },
  { "en": "Di Mana Arrester Dipasang?", "id": "Dipasang Di Sisi Tegangan Tinggi Trafo." },
  { "en": "Apa Itu Explosion Vent?", "id": "Saluran Pelepas Tekanan Gas Berlebih Mendadak." },
  { "en": "Fungsi Explosion Vent?", "id": "Mencegah Tangki Trafo Pecah Atau Meledak." },
  { "en": "Apa Itu Uji Kenaikan Suhu?", "id": "Memastikan Trafo Beroperasi Dalam Batas Suhu." },
  { "en": "Apa Itu Hot Spot?", "id": "Titik Paling Panas Di Dalam Belitan." },
  { "en": "Pentingnya Mengetahui Hot Spot?", "id": "Suhu Hot Spot Menentukan Umur Isolasi." },
  { "en": "Apa Itu Kertas Insulflex?", "id": "Bahan Isolasi Kertas Khusus Untuk Belitan." },
  { "en": "Apa Itu Pressboard?", "id": "Bahan Isolasi Padat Untuk Barier Dan Spacer." },
  { "en": "Apa Itu Reaktansi Bocor?", "id": "Reaktansi Akibat Fluks Yang Tidak Terkopling." },
  { "en": "Penyebab Reaktansi Bocor?", "id": "Adanya Fluks Magnet Yang Melewati Udara." },
  { "en": "Apa Itu Reaktansi Magnetisasi?", "id": "Reaktansi Akibat Pembentukan Fluks Di Inti." },
  { "en": "Apa Itu Rangkaian Ekuivalen?", "id": "Model Rangkaian Listrik Yang Mewakili Trafo." },
  { "en": "Tujuan Rangkaian Ekuivalen?", "id": "Memudahkan Analisis Performa Dan Karakteristik." },
  { "en": "Apa Itu Sistem Per Unit?", "id": "Menyatakan Besaran Listrik Sebagai Fraksi." },
  { "en": "Keuntungan Sistem Per Unit?", "id": "Menyederhanakan Perhitungan Pada Sistem Tenaga." },
  { "en": "Apa Itu Ferroresonance?", "id": "Osilasi Non-Linier Antara Kapasitor Dan Trafo." },
  { "en": "Kondisi Terjadi Ferroresonance?", "id": "Saat Trafo Tanpa Beban Terhubung Kapasitansi." },
  { "en": "Dampak Ferroresonance?", "id": "Menyebabkan Tegangan Lebih Dan Distorsi Parah." },
  { "en": "Apa Itu Trafo Pengetanahan?", "id": "Menyediakan Titik Netral Untuk Sistem Listrik." },
  { "en": "Jenis Trafo Pengetanahan?", "id": "Trafo Zig-Zag Adalah Salah Satu Contohnya." },
  { "en": "Apa Itu Harmonisa Orde Tiga?", "id": "Komponen Frekuensi Tiga Kali Frekuensi Dasar." },
  { "en": "Efek Harmonisa Pada Trafo?", "id": "Meningkatkan Rugi-Rugi Dan Pemanasan Berlebih." },
  { "en": "Apa Itu K-Factor Transformer?", "id": "Trafo Dirancang Khusus Untuk Beban Harmonisa." },
  { "en": "Penyebab Kebisingan Trafo?", "id": "Getaran Inti Akibat Efek Magnetostriksi." },
  { "en": "Satuan Kebisingan Trafo?", "id": "Diukur Dalam Satuan Desibel (dB)." },
  { "en": "Bagaimana Mengurangi Kebisingan?", "id": "Desain Inti Dan Peredam Getaran Eksternal." },
  { "en": "Apa Itu Uji Tahanan Belitan?", "id": "Mengukur Tahanan DC Dari Kawat Belitan." },
  { "en": "Tujuan Uji Tahanan Belitan?", "id": "Memeriksa Koneksi Dan Kualitas Belitan." },
  { "en": "Apa Itu Pelat Peringkat?", "id": "Sama Dengan Nameplate Berisi Info Trafo." },
  { "en": "Apa Itu BIL?", "id": "Basic Insulation Level Atau Level Isolasi." },
  { "en": "Arti Dari BIL?", "id": "Kemampuan Isolasi Menahan Tegangan Impuls Petir." },
  { "en": "Apa Itu Gaya Elektrodinamik?", "id": "Gaya Mekanis Pada Belitan Saat Hubung Singkat." },
  { "en": "Dampak Gaya Elektrodinamik?", "id": "Dapat Menyebabkan Deformasi Atau Kerusakan Belitan." },
  { "en": "Fungsi Penjepit Belitan?", "id": "Menahan Belitan Agar Tahan Gaya Elektrodinamik." },
  { "en": "Kenapa Inti Trafo Ditanahkan?", "id": "Mencegah Potensial Mengambang Pada Inti Besi." },
  { "en": "Di Mana Inti Ditanahkan?", "id": "Hanya Pada Satu Titik Untuk Hindari Loop." },
  { "en": "Apa Itu Pelindung Statis?", "id": "Lapisan Konduktif Untuk Mengontrol Medan Listrik." },
  { "en": "Apa Itu Arus Edaran?", "id": "Arus Yang Mengalir Antar Trafo Paralel." },
  { "en": "Penyebab Arus Edaran?", "id": "Perbedaan Rasio Tegangan Yang Tidak Sama." },
  { "en": "Apa Itu Proses VPI?", "id": "Vacuum Pressure Impregnation Untuk Trafo Kering." },
  { "en": "Tujuan Proses VPI?", "id": "Meningkatkan Kekuatan Mekanis Dan Sifat Dielektrik." },
  { "en": "Jenis Belitan Trafo?", "id": "Belitan Heliks Belitan Cakram Dan Silinder." },
  { "en": "Apa Itu Fenomena Pompa Minyak?", "id": "Sirkulasi Minyak Akibat Pemanasan Dan Pendinginan." },
  { "en": "Apa Itu Perangkap Udara?", "id": "Udara Yang Terjebak Dalam Sistem Isolasi." },
  { "en": "Bahaya Perangkap Udara?", "id": "Menurunkan Kekuatan Dielektrik Dan Picu PD." },
  { "en": "Apa Itu Tes Faktor Daya?", "id": "Sama Dengan Uji Tan Delta Untuk Isolasi." },
  { "en": "Apa Itu Kelas Isolasi?", "id": "Menunjukkan Batas Suhu Maksimum Material Isolasi." },
  { "en": "Contoh Kelas Isolasi?", "id": "Kelas A (105°C) Kelas F (155°C)." },
  { "en": "Minyak Trafo Berbasis Apa?", "id": "Umumnya Berbasis Mineral Atau Ester Sintetis." },
  { "en": "Apa Itu PCB dalam Minyak?", "id": "Polychlorinated Biphenyls Senyawa Beracun." },
  { "en": "Kenapa PCB Dilarang?", "id": "Berbahaya Bagi Lingkungan Dan Kesehatan Manusia." },
  { "en": "Apa Itu Relai Diferensial?", "id": "Proteksi Utama Trafo Dari Gangguan Internal." },
  { "en": "Prinsip Relai Diferensial?", "id": "Membandingkan Arus Masuk Dan Arus Keluar." },
  { "en": "Kapan Relai Diferensial Bekerja?", "id": "Jika Ada Selisih Arus Yang Signifikan." },
  { "en": "Apa Itu Pengeringan Trafo?", "id": "Proses Menghilangkan Kelembaban Dari Isolasi." },
  { "en": "Metode Pengeringan Trafo?", "id": "Menggunakan Oven Atau Pemanasan Dengan Sirkulasi." },
  { "en": "Apa Itu Umur Trafo?", "id": "Umur Trafo Ditentukan Oleh Umur Isolasinya." },
  { "en": "Faktor Utama Penuaan Isolasi?", "id": "Suhu Kelembaban Dan Oksigen Di Sekitarnya." },
  { "en": "Apa Itu Indikator Suhu Belitan?", "id": "Alat Untuk Memantau Suhu Hot Spot." },
  { "en": "Apa Itu Indikator Suhu Minyak?", "id": "Alat Untuk Memantau Suhu Minyak Atas." },
  { "en": "Apa Itu Koneksi Bintang?", "id": "Sama Dengan Hubungan Y (Wye Connection)." },
  { "en": "Apa Itu Koneksi Delta?", "id": "Sama Dengan Hubungan Segitiga Atau Mesh." },
  { "en": "Hubungan Bintang Punya Netral?", "id": "Ya Memiliki Titik Netral Yang Bisa Ditanahkan." },
  { "en": "Hubungan Delta Punya Netral?", "id": "Tidak Memiliki Titik Netral Yang Jelas." },
  { "en": "Apa Itu Over-Fluxing?", "id": "Kondisi Tegangan Per Frekuensi Melebihi Batas." },
  { "en": "Akibat Over-Fluxing?", "id": "Menyebabkan Saturasi Inti Dan Pemanasan Hebat." },
  { "en": "Apa Itu Uji IFT?", "id": "Interfacial Tension Test Untuk Minyak Trafo." },
  { "en": "Tujuan Uji IFT?", "id": "Mengukur Kontaminasi Polar Dalam Minyak Isolasi." },
  { "en": "Apa Itu Sludge?", "id": "Endapan Oksidasi Minyak Yang Merusak Pendinginan." },
  { "en": "Penyebab Terbentuknya Sludge?", "id": "Oksidasi Minyak Karena Panas Dan Oksigen." },
  { "en": "Apa Itu Inhibitor Minyak?", "id": "Zat Kimia Untuk Mencegah Oksidasi Minyak." },
  { "en": "Apa Itu Passivator Logam?", "id": "Zat Kimia Menekan Efek Katalitik Logam." },
  { "en": "Apa Itu Trafo Tipe Kering Cast Resin?", "id": "Belitan Dituang Dalam Cetakan Resin Epoksi." },
  { "en": "Keuntungan Cast Resin?", "id": "Tahan Lembab Dan Kekuatan Mekanis Tinggi." },
  { "en": "Apa Itu Restricted Earth Fault Protection?", "id": "Proteksi Sensitif Untuk Gangguan Tanah Belitan." },
  { "en": "Zona Proteksi REF?", "id": "Melindungi Belitan Trafo Dari Gangguan Internal." },
  { "en": "Apa Itu Proteksi Over-fluxing?", "id": "Melindungi Inti Dari Pemanasan Akibat Fluks Lebih." },
  { "en": "Simbol Proteksi Over-fluxing?", "id": "Dikenal Dengan Kode ANSI 24 (V/Hz)." },
  { "en": "Apa Itu Proses Tanking?", "id": "Memasukkan Bagian Aktif Trafo Ke Tangki." },
  { "en": "Apa Itu Proses Vapour Phase Drying?", "id": "Metode Pengeringan Isolasi Menggunakan Uap Kerosin." },
  { "en": "Apa Itu Trafo Penyearah (Rectifier)?", "id": "Trafo Untuk Sistem Konverter AC Ke DC." },
  { "en": "Ciri Trafo Penyearah?", "id": "Memiliki Banyak Belitan Sekunder Terpisah." },
  { "en": "Apa Itu Trafo Tungku (Furnace)?", "id": "Trafo Untuk Tungku Busur Api Listrik." },
  { "en": "Karakteristik Trafo Tungku?", "id": "Mampu Memberikan Arus Sekunder Sangat Tinggi." },
  { "en": "Apa Itu Sympathetic Inrush?", "id": "Arus Inrush Terinduksi Pada Trafo Paralel." },
  { "en": "Apa Itu Frequency Domain Spectroscopy?", "id": "Uji Lanjutan Untuk Kondisi Isolasi Trafo." },
  { "en": "Apa Yang Diukur FDS?", "id": "Kapasitansi Dan Faktor Disipasi Pada Frekuensi." },
  { "en": "Bahan Gasket Trafo?", "id": "Gabus Karet Nitril Atau Bahan Sintetis." },
  { "en": "Fungsi Gasket Trafo?", "id": "Mencegah Kebocoran Minyak Dari Sambungan." },
  { "en": "Apa Itu Faktor Percepatan Penuaan?", "id": "Kelipatan Laju Penuaan Isolasi Per Suhu." },
  { "en": "Standar Trafo Internasional?", "id": "IEC (International Electrotechnical Commission) Dan IEEE." },
  { "en": "Apa Itu Doble Test?", "id": "Nama Merek Populer Untuk Uji Faktor Daya." },
  { "en": "Apa Itu Angka Asam Minyak?", "id": "Ukuran Keasaman Minyak Akibat Oksidasi." },
  { "en": "Satuan Angka Asam?", "id": "Miligram KOH Per Gram Sampel Minyak." },
  { "en": "Bahaya Angka Asam Tinggi?", "id": "Bersifat Korosif Dan Mempercepat Degradasi Kertas." },
  { "en": "Apa Itu Pemurnian Minyak?", "id": "Proses Menghilangkan Kontaminan Dari Minyak Trafo." },
  { "en": "Metode Pemurnian Minyak?", "id": "Menggunakan Filter Pemanas Dan Ruang Vakum." },
  { "en": "Apa Itu Trafo Step-Voltage Regulator?", "id": "Autotrafo Dengan Pengubah Tap Otomatis." },
  { "en": "Fungsi Step-Voltage Regulator?", "id": "Menjaga Tegangan Jaringan Agar Tetap Konstan." },
  { "en": "Apa Itu Zero Sequence Impedance?", "id": "Impedansi Trafo Terhadap Arus Urutan Nol." },
  { "en": "Pentingnya Zero Sequence Impedance?", "id": "Menentukan Arus Gangguan Tanah Pada Sistem." },
  { "en": "Apa Itu Belitan Interleaved?", "id": "Teknik Lilitan Untuk Mengurangi Tegangan Impuls." },
  { "en": "Tujuan Belitan Interleaved?", "id": "Distribusi Kapasitansi Lebih Merata Di Belitan." },
  { "en": "Apa Itu Transposisi Konduktor?", "id": "Memutar Posisi Sub-Konduktor Dalam Belitan." },
  { "en": "Tujuan Transposisi Konduktor?", "id": "Mengurangi Rugi Tambahan Akibat Arus Eddy." },
  { "en": "Apa Itu Trafo Phase-Shifting?", "id": "Mengatur Aliran Daya Dengan Menggeser Fasa." },
  { "en": "Aplikasi Trafo Phase-Shifting?", "id": "Mengontrol Aliran Daya Pada Jaringan Interkoneksi." },
  { "en": "Apa Itu Fenomena Resonansi Harmonik?", "id": "Resonansi Jaringan Akibat Interaksi Harmonisa." },
  { "en": "Peran Trafo Dalam Resonansi Harmonik?", "id": "Impedansi Trafo Mempengaruhi Frekuensi Resonansi." },
  { "en": "Apa Itu Uji Kadar Air?", "id": "Mengukur Kandungan Air Dalam Minyak Isolasi." },
  { "en": "Metode Uji Kadar Air?", "id": "Metode Karl Fischer Adalah Standar Uji." },
  { "en": "Satuan Kadar Air?", "id": "Dinyatakan Dalam Parts Per Million (PPM)." },
  { "en": "Batas Maksimal Kadar Air?", "id": "Tergantung Pada Level Tegangan Transformator." },
  { "en": "Apa Itu Static Electrification?", "id": "Pembangkitan Muatan Statis Oleh Aliran Minyak." },
  { "en": "Bahaya Static Electrification?", "id": "Dapat Menyebabkan Percikan Api Dalam Trafo." },
  { "en": "Apa Itu Current Chopping?", "id": "Pemutusan Arus Tiba-Tiba Oleh Pemutus Sirkuit." },
  { "en": "Dampak Current Chopping?", "id": "Menghasilkan Tegangan Lebih Transien Sangat Tinggi." },
  { "en": "Apa Itu Termometer Saku (Pocket)?", "id": "Kantung Logam Untuk Memasang Sensor Suhu." },
  { "en": "Fungsi Termometer Saku?", "id": "Memudahkan Penggantian Sensor Tanpa Menguras Minyak." },
  { "en": "Standar Warna Cat Trafo?", "id": "Umumnya Abu-Abu Untuk Memantulkan Panas Matahari." },
  { "en": "Pentingnya Lapisan Cat?", "id": "Melindungi Tangki Dari Karat Dan Korosi." },
  { "en": "Apa Itu Derating Trafo?", "id": "Pengurangan Kapasitas Trafo Karena Kondisi Operasi." },
  { "en": "Penyebab Derating Trafo?", "id": "Ketinggian Tempat Suhu Dan Beban Harmonisa." },
  { "en": "Apa Itu Positive Sequence Impedance?", "id": "Impedansi Normal Trafo Saat Operasi Seimbang." },
  { "en": "Apa Itu Negative Sequence Impedance?", "id": "Impedansi Terhadap Arus Beban Tidak Seimbang." },
  { "en": "Hubungan Impedansi Urutan?", "id": "Pada Trafo Urutan Positif Negatif Sama." },
  { "en": "Apa Itu Uji Resistivitas Minyak?", "id": "Mengukur Kemampuan Minyak Menahan Aliran Arus." },
  { "en": "Resistivitas Minyak Baik?", "id": "Menunjukkan Nilai Tahanan Yang Sangat Tinggi." },
  { "en": "Apa Itu Trafo Pad-Mounted?", "id": "Trafo Distribusi Dalam Kabinet Logam Tertutup." },
  { "en": "Aplikasi Trafo Pad-Mounted?", "id": "Untuk Sistem Distribusi Bawah Tanah Perumahan." },
  { "en": "Apa Itu Trafo Pole-Mounted?", "id": "Trafo Distribusi Yang Dipasang Di Tiang." },
  { "en": "Ciri Trafo Pole-Mounted?", "id": "Ukuran Relatif Kecil Dengan Sirip Pendingin." },
  { "en": "Apa Itu Isolasi Kelas Termal?", "id": "Menentukan Kemampuan Tahan Panas Material Isolasi." },
  { "en": "Contoh Material Kelas A?", "id": "Kertas Katun Sutra Yang Diimpregnasi Minyak." },
  { "en": "Contoh Material Kelas H?", "id": "Silikon Mika Kaca Serat Dengan Resin." },
  { "en": "Apa Itu Umur Paruh Isolasi?", "id": "Waktu Hingga Kekuatan Mekanis Turun Setengah." },
  { "en": "Hukum Arrhenius Untuk Isolasi?", "id": "Laju Penuaan Naik Eksponensial Dengan Suhu." },
  { "en": "Apa Itu Sirkuit Terbuka Sekunder CT?", "id": "Kondisi Sangat Berbahaya Pada Trafo Arus." },
  { "en": "Bahaya Sirkuit Terbuka CT?", "id": "Menghasilkan Tegangan Sangat Tinggi Di Sekunder." },
  { "en": "Bagaimana Menangani Sekunder CT?", "id": "Selalu Dihubung Singkat Jika Tidak Terhubung Meter." },
  { "en": "Beban Pada Trafo Tegangan (PT)?", "id": "Seharusnya Sangat Ringan Atau Impedansi Tinggi." },
  { "en": "Apa Itu Sambungan T-T?", "id": "Sama Dengan Sambungan Scott-T Untuk Konversi." },
  { "en": "Apa Itu Rapat Fluks (Flux Density)?", "id": "Jumlah Fluks Magnetik Per Satuan Luas." },
  { "en": "Satuan Rapat Fluks?", "id": "Tesla (T) Atau Weber Per Meter Persegi." },
  { "en": "Apa Itu Titik Lutut (Knee Point)?", "id": "Titik Saturasi Pada Kurva Magnetisasi Inti." },
  { "en": "Apa Itu Trafo Variable Frequency?", "id": "Trafo Didesain Untuk Beroperasi Berbagai Frekuensi." },
  { "en": "Aplikasi Trafo VFT?", "id": "Pada Drive Motor Dengan Kecepatan Bervariasi." },
  { "en": "Apa Itu Sistem Pendingin OFWF?", "id": "Oil Forced Water Forced Cooling System." },
  { "en": "Pendingin OFWF Menggunakan Apa?", "id": "Minyak Dan Air Dipompa Melalui Heat Exchanger." },
  { "en": "Apa Itu Uji Ketahanan Impuls?", "id": "Menguji Kemampuan Trafo Menahan Sambaran Petir." },
  { "en": "Bentuk Gelombang Uji Impuls?", "id": "1.2 Per 50 Mikrodetik Bentuk Gelombang Standar." },
  { "en": "Apa Itu Creepage Distance?", "id": "Jarak Permukaan Terpendek Antara Dua Konduktor." },
  { "en": "Pentingnya Creepage Distance?", "id": "Mencegah Flashover Permukaan Pada Isolator Bushing." },
  { "en": "Apa Itu Clearance Distance?", "id": "Jarak Terpendek Di Udara Antar Konduktor." },
  { "en": "Apa Itu Trafo Konverter?", "id": "Istilah Umum Untuk Trafo Sistem Penyearah." },
  { "en": "Tantangan Desain Trafo Konverter?", "id": "Arus Harmonisa Tinggi Dan Komponen DC." },
  { "en": "Fungsi Reaktor Seri?", "id": "Membatasi Arus Gangguan Atau Arus Inrush." },
  { "en": "Apa Itu Winding Tapping Point?", "id": "Titik Sambungan Pada Belitan Untuk Tap." },
  { "en": "Bahan Konduktor Belitan?", "id": "Tembaga Atau Aluminium Bergantung Pada Desainnya." },
  { "en": "Kenapa Tembaga Sering Dipakai?", "id": "Konduktivitas Listrik Dan Kekuatan Mekanis Baik." },
  { "en": "Kelebihan Konduktor Aluminium?", "id": "Berat Lebih Ringan Dan Biaya Lebih Murah." },
  { "en": "Apa Itu Core Stacking Factor?", "id": "Rasio Volume Efektif Besi Dalam Inti." },
  { "en": "Fungsi Varnish Pada Laminasi?", "id": "Sebagai Lapisan Isolasi Antar Pelat Inti." },
  { "en": "Apa Itu Step-Lap Core?", "id": "Susunan Laminasi Inti Untuk Kurangi Rugi." },
  { "en": "Bagaimana Step-Lap Bekerja?", "id": "Mengurangi Rapat Fluks Di Sambungan Sudut." },
  { "en": "Apa Itu Uji Polaritas?", "id": "Memastikan Arah Belitan Primer Dan Sekunder." },
  { "en": "Apa Itu Derajat Polimerisasi (DP)?", "id": "Ukuran Kekuatan Mekanis Kertas Isolasi." },
  { "en": "Uji DP Menunjukkan Apa?", "id": "Tingkat Penuaan Dan Sisa Umur Isolasi." },
  { "en": "Nilai DP Kertas Baru?", "id": "Biasanya Berkisar Antara 1000 Hingga 1300." },
  { "en": "Nilai DP Akhir Umur?", "id": "Di Bawah 200 Menunjukkan Isolasi Rapuh." },
  { "en": "Apa Itu Bahan Inti Amorf?", "id": "Logam Non-Kristal Dengan Rugi Histeresis Rendah." },
  { "en": "Keuntungan Inti Amorf?", "id": "Mengurangi Rugi Tanpa Beban Secara Signifikan." },
  { "en": "Apa Itu Kertas Aramid (NOMEX)?", "id": "Kertas Isolasi Tahan Suhu Sangat Tinggi." },
  { "en": "Aplikasi Kertas Aramid?", "id": "Pada Trafo Tipe Kering Dan Traksi." },
  { "en": "Apa Itu Bushing Tipe Kondensor?", "id": "Bushing Dengan Lapisan Konduktif Untuk Kontrol Medan." },
  { "en": "Fungsi Lapisan Kondensor?", "id": "Memastikan Distribusi Tegangan Merata Pada Bushing." },
  { "en": "Jenis Bushing Kondensor?", "id": "Oil Impregnated Paper (OIP) Dan RIP." },
  { "en": "Apa Itu Bushing RIP?", "id": "Resin Impregnated Paper Bushing Technology." },
  { "en": "Apa Itu Relai Tekanan Mendadak?", "id": "Merespons Cepat Kenaikan Tekanan Internal Trafo." },
  { "en": "Penyebab Tekanan Mendadak?", "id": "Busur Api Internal Akibat Hubung Singkat." },
  { "en": "Apa Itu Trafo Traksi?", "id": "Trafo Khusus Untuk Kereta Listrik (Lokomotif)." },
  { "en": "Tantangan Trafo Traksi?", "id": "Getaran Konstan Dan Ukuran Sangat Kompak." },
  { "en": "Apa Itu Rugi Tambahan (Stray Loss)?", "id": "Rugi Akibat Fluks Bocor Di Komponen Logam." },
  { "en": "Di Mana Rugi Tambahan Terjadi?", "id": "Di Tangki Penjepit Dan Struktur Baja." },
  { "en": "Bagaimana Mengurangi Rugi Tambahan?", "id": "Menggunakan Pelindung Fluks Magnetik Non-Magnetik." },
  { "en": "Apa Itu Uji PDC?", "id": "Polarization Depolarization Current Analysis." },
  { "en": "Tujuan Uji PDC?", "id": "Menganalisis Konduktivitas Dan Kadar Air Isolasi." },
  { "en": "Apa Itu Geomagnetically Induced Current (GIC)?", "id": "Arus DC Semu Akibat Badai Matahari." },
  { "en": "Dampak GIC Pada Trafo?", "id": "Menyebabkan Setengah Siklus Saturasi Inti Besi." },
  { "en": "Apa Itu Proses Annealing Inti?", "id": "Pemanasan Inti Untuk Mengurangi Tegangan Mekanis." },
  { "en": "Tujuan Annealing Inti?", "id": "Memulihkan Sifat Magnetik Dan Kurangi Rugi." },
  { "en": "Apa Itu Pengaku Tangki (Stiffener)?", "id": "Struktur Baja Penguat Dinding Tangki Trafo." },
  { "en": "Fungsi Pengaku Tangki?", "id": "Menahan Tekanan Vakum Dan Tekanan Internal." },
  { "en": "Apa Itu Relai Thermal Replica?", "id": "Mensimulasikan Suhu Hot Spot Pada Belitan." },
  { "en": "Apa Itu Uji Titik Nyala Minyak?", "id": "Suhu Terendah Minyak Menghasilkan Uap Mudah Terbakar." },
  { "en": "Pentingnya Titik Nyala Minyak?", "id": "Indikator Keamanan Terhadap Risiko Kebakaran Internal." },
  { "en": "Apa Itu Uji Titik Tuang Minyak?", "id": "Suhu Terendah Dimana Minyak Masih Bisa Mengalir." },
  { "en": "Pentingnya Titik Tuang Minyak?", "id": "Penting Untuk Operasi Trafo Di Iklim Dingin." },
  { "en": "Apa Itu Magnetic Oil Gauge (MOG)?", "id": "Indikator Level Minyak Dengan Kopling Magnetik." },
  { "en": "Kelebihan MOG?", "id": "Tidak Ada Kebocoran Minyak Melalui Poros." },
  { "en": "Apa Itu Transformerboard?", "id": "Papan Isolasi Kaku Dari Selulosa Murni." },
  { "en": "Aplikasi Transformerboard?", "id": "Silinder Barrier Spacer Dan Cincin Penjepit." },
  { "en": "Apa Itu Uji Furan?", "id": "Analisis Senyawa Furan Dalam Sampel Minyak." },
  { "en": "Furan Berasal Dari Mana?", "id": "Dihasilkan Dari Degradasi Kertas Isolasi Selulosa." },
  { "en": "Hubungan Furan Dan DP?", "id": "Tingkat Furan Berhubungan Erat Dengan Nilai DP." },
  { "en": "Apa Itu Trafo Hermetik?", "id": "Trafo Tertutup Rapat Tanpa Konservator Minyak." },
  { "en": "Fitur Trafo Hermetik?", "id": "Ruang Gas Nitrogen Untuk Mengakomodasi Ekspansi." },
  { "en": "Apa Itu Sistem Proteksi Kebakaran?", "id": "Sistem Semprotan Air Atau Gas Nitrogen." },
  { "en": "Kapan Proteksi Kebakaran Aktif?", "id": "Saat Sensor Mendeteksi Suhu Sangat Tinggi." },
  { "en": "Apa Itu Rugi Dielektrik?", "id": "Energi Yang Hilang Sebagai Panas Di Isolasi." },
  { "en": "Faktor Yang Mempengaruhi Rugi Dielektrik?", "id": "Tegangan Frekuensi Suhu Dan Kontaminasi." },
  { "en": "Apa Itu Uji Tegangan Tahan AC?", "id": "Menguji Kemampuan Isolasi Menahan Tegangan AC." },
  { "en": "Apa Itu Uji Tegangan Tahan DC?", "id": "Menguji Tahanan Isolasi Terhadap Tegangan DC." },
  { "en": "Apa Itu Relai Jarak (Distance Relay)?", "id": "Proteksi Cadangan Untuk Gangguan Pada Trafo." },
  { "en": "Apa Itu Gangguan Simetris?", "id": "Gangguan Tiga Fasa Yang Seimbang Sempurna." },
  { "en": "Apa Itu Gangguan Asimetris?", "id": "Gangguan Satu Fasa Ke Tanah Atau Antar Fasa." },
  { "en": "Apa Itu Sistem Pendingin ODAF?", "id": "Oil Directed Air Forced Cooling System." },
  { "en": "Apa Itu Oil Directed?", "id": "Aliran Minyak Diarahkan Langsung Ke Belitan." },
  { "en": "Apa Itu Trafo Satu Fasa?", "id": "Trafo Dengan Satu Belitan Primer Dan Sekunder." },
  { "en": "Di Mana Trafo Satu Fasa Digunakan?", "id": "Pada Jaringan Distribusi Pedesaan Dan Beban Kecil." },
  { "en": "Apa Itu Circulating Current Protection?", "id": "Sama Dengan Proteksi Diferensial Pada Trafo." },
  { "en": "Apa Itu Uji Zero Sequence?", "id": "Mengukur Impedansi Urutan Nol Dari Trafo." },
  { "en": "Apa Itu Tegangan Pemulihan Transien?", "id": "Tegangan Yang Muncul Setelah Pemutusan Gangguan." },
  { "en": "Apa Itu Remanence Magnetik?", "id": "Sisa Magnetisme Di Inti Setelah Energi Dilepas." },
  { "en": "Dampak Remanence Magnetik?", "id": "Dapat Meningkatkan Arus Inrush Saat Diberi Energi." },
  { "en": "Bagaimana Menghilangkan Remanence?", "id": "Dengan Proses Demagnetisasi Secara Perlahan." },
  { "en": "Apa Itu Trafo Earthing?", "id": "Nama Lain Untuk Trafo Pengetanahan Sistem." },
  { "en": "Apa Itu Reluktansi Celah Udara?", "id": "Reluktansi Tinggi Pada Celah Sambungan Inti." },
  { "en": "Fungsi Celah Udara Pada Reaktor?", "id": "Mencegah Saturasi Inti Dan Melinierkan Induktansi." },
  { "en": "Apa Itu Skin Effect?", "id": "Kecenderungan Arus AC Mengalir Di Permukaan." },
  { "en": "Dampak Skin Effect?", "id": "Meningkatkan Resistansi Efektif Kawat Pada Frekuensi Tinggi." },
  { "en": "Apa Itu Proximity Effect?", "id": "Distribusi Arus Tidak Merata Akibat Konduktor Berdekatan." },
  { "en": "Dampak Proximity Effect?", "id": "Meningkatkan Rugi Tembaga Tambahan Pada Belitan." },
  { "en": "Apa Itu Koil Rogowski?", "id": "Sensor Arus Tanpa Inti Magnetik Besi." },
  { "en": "Apa Itu Sensor Serat Optik?", "id": "Sensor Suhu Berbasis Cahaya Untuk Titik Panas." },
  { "en": "Kelebihan Sensor Serat Optik?", "id": "Kebal Terhadap Gangguan Medan Elektromagnetik." },
  { "en": "Apa Itu Winding Surge Impedance?", "id": "Impedansi Karakteristik Belitan Terhadap Gelombang Impuls." },
  { "en": "Apa Itu Bak Penampung Minyak?", "id": "Struktur Beton Untuk Menampung Tumpahan Minyak." },
  { "en": "Tujuan Bak Penampung Minyak?", "id": "Mencegah Pencemaran Lingkungan Akibat Kebocoran." },
  { "en": "Apa Itu Beban Puncak (Peak Load)?", "id": "Beban Maksimum Yang Dialami Trafo Periodik." },
  { "en": "Apa Itu Beban Dasar (Base Load)?", "id": "Beban Minimum Kontinu Selama Periode Tertentu." },
  { "en": "Apa Itu Faktor Beban (Load Factor)?", "id": "Rasio Beban Rata-Rata Terhadap Beban Puncak." },
  { "en": "Apa Itu Arrester Oksida Logam (MOV)?", "id": "Jenis Arrester Modern Tanpa Celah Percik." },
  { "en": "Apa Itu Tegangan Referensi Arrester?", "id": "Tegangan Batas Di Mana Arrester Mulai Konduksi." },
  { "en": "Apa Itu Uji Frekuensi Rendah (LFFR)?", "id": "Metode Lain Untuk Menguji Integritas Mekanis." },
  { "en": "Apa Itu Kumparan Penstabil?", "id": "Sama Dengan Belitan Tersier Dalam Beberapa Aplikasi." },
  { "en": "Apa Itu Konstanta Waktu Termal?", "id": "Ukuran Seberapa Cepat Suhu Trafo Berubah." },
  { "en": "Apa Itu Perangkat Pelepas Tekanan?", "id": "Pressure Relief Device (PRD) Katup Pengaman." },
  { "en": "Fungsi Perangkat Pelepas Tekanan?", "id": "Melepaskan Tekanan Berlebih Secara Otomatis." },
  { "en": "Apa Itu Uji Kapasitansi Belitan?", "id": "Mengukur Kapasitansi Antar Belitan Dan Ke Tanah." },
  { "en": "Perubahan Kapasitansi Menandakan Apa?", "id": "Perubahan Geometri Atau Deformasi Pada Belitan." },
  { "en": "Apa Itu Kumparan Regulator?", "id": "Belitan Terpisah Untuk Fungsi Regulasi Tap Changer." },
  { "en": "Apa Itu Belitan Utama?", "id": "Belitan Utama Yang Menyalurkan Daya Penuh." },
  { "en": "Apa Itu Tegangan Fasa Ke Fasa?", "id": "Tegangan Antara Dua Jalur Fasa Berbeda." },
  { "en": "Apa Itu Tegangan Fasa Ke Netral?", "id": "Tegangan Antara Satu Fasa Dan Titik Netral." },
  { "en": "Tegangan Netral Ke Tanah?", "id": "Idealnya Nol Volt Pada Sistem Seimbang." },
  { "en": "Apa Itu Pergeseran Sudut Fasa?", "id": "Perbedaan Sudut Antara Tegangan Primer Sekunder." },
  { "en": "Penyebab Pergeseran Sudut Fasa?", "id": "Konfigurasi Hubungan Belitan (Bintang Atau Delta)." },
  { "en": "Apa Itu Diagram Fasor?", "id": "Representasi Grafis Besaran AC Seperti Vektor." },
  { "en": "Fungsi Diagram Fasor?", "id": "Menganalisis Hubungan Fasa Antara Tegangan Arus." },
  { "en": "Apa Itu Trafo Penyesuai Impedansi?", "id": "Trafo Untuk Transfer Daya Maksimal Antar Sirkuit." },
  { "en": "Aplikasi Trafo Penyesuai Impedansi?", "id": "Pada Rangkaian Audio Dan Frekuensi Radio." },
  { "en": "Apa Itu Baja Silikon CRGO?", "id": "Cold-Rolled Grain-Oriented Steel For Core." },
  { "en": "Kenapa Arah Grain Penting?", "id": "Sifat Magnetik Optimal Searah Dengan Arah Grain." },
  { "en": "Apa Itu Baja Hi-B?", "id": "Baja CRGO Dengan Permeabilitas Sangat Tinggi." },
  { "en": "Apa Itu Loss Capitalization?", "id": "Metode Evaluasi Biaya Total Kepemilikan Trafo." },
  { "en": "Biaya Apa Yang Dihitung?", "id": "Biaya Pembelian Ditambah Biaya Rugi-Rugi Daya." },
  { "en": "Formula Sederhana Loss Capitalization?", "id": "A (Biaya Tanpa Beban) + B (Biaya Beban)." },
  { "en": "Apa Itu Health Index (HI)?", "id": "Skor Kuantitatif Untuk Menilai Kondisi Trafo." },
  { "en": "Faktor Dalam Health Index?", "id": "Hasil Uji Minyak Umur Dan Beban." },
  { "en": "Skor Health Index Baik?", "id": "Nilai Rendah Menunjukkan Kondisi Sangat Baik." },
  { "en": "Apa Itu Arcing Horn?", "id": "Perangkat Pelindung Bushing Dari Lonjakan Tegangan." },
  { "en": "Bagaimana Arcing Horn Bekerja?", "id": "Membuat Celah Udara Untuk Tempat Flashover." },
  { "en": "Apa Itu Minyak Ester?", "id": "Minyak Trafo Biodegradable Dari Sumber Terbarukan." },
  { "en": "Keuntungan Minyak Ester?", "id": "Titik Nyala Tinggi Dan Ramah Lingkungan." },
  { "en": "Jenis Minyak Ester?", "id": "Ester Alami (Natural) Dan Ester Sintetis." },
  { "en": "Apa Itu Trafo Berinsulasi Gas?", "id": "Menggunakan Gas SF6 Sebagai Media Isolasi Pendingin." },
  { "en": "Keuntungan Trafo Gas SF6?", "id": "Tidak Mudah Terbakar Dan Ukuran Kompak." },
  { "en": "Apa Itu Kurva B-H?", "id": "Kurva Hubungan Antara Fluks Dan Arus Magnetisasi." },
  { "en": "Area Loop Kurva B-H?", "id": "Mewakili Rugi Histeresis Per Siklus Listrik." },
  { "en": "Apa Itu Retentivitas?", "id": "Sisa Magnetisme Setelah Gaya Magnet Dihilangkan." },
  { "en": "Apa Itu Koersivitas?", "id": "Gaya Balik Untuk Menghilangkan Sisa Magnetisme." },
  { "en": "Apa Itu Analisis Vibro-Akustik?", "id": "Mendeteksi Masalah Mekanis Melalui Getaran Suara." },
  { "en": "Apa Itu Pencitraan Termal?", "id": "Menggunakan Kamera Inframerah Untuk Deteksi Panas." },
  { "en": "Fungsi Pencitraan Termal?", "id": "Mendeteksi Koneksi Longgar Atau Pendinginan Buruk." },
  { "en": "Tipe Tap Changer OLTC?", "id": "Tipe Reaktor Dan Tipe Resistor (Resistive)." },
  { "en": "Prinsip Tap Changer Resistor?", "id": "Menggunakan Resistor Transisi Saat Pemindahan Tap." },
  { "en": "Apa Itu Bushing Komposit?", "id": "Bushing Dengan Isolator Luar Dari Polimer Silikon." },
  { "en": "Kelebihan Bushing Komposit?", "id": "Anti Pecah Ringan Dan Anti Polusi." },
  { "en": "Apa Itu Distribusi Tegangan Impuls?", "id": "Bagaimana Tegangan Petir Terdistribusi Di Belitan." },
  { "en": "Distribusi Awal Tegangan Impuls?", "id": "Tidak Linier Sangat Tinggi Di Lilitan Awal." },
  { "en": "Apa Itu Isolasi Bertingkat (Graded)?", "id": "Level Isolasi Berbeda Sepanjang Belitan Trafo." },
  { "en": "Tujuan Isolasi Bertingkat?", "id": "Mengurangi Biaya Dan Ukuran Dekat Titik Netral." },
  { "en": "Apa Itu Uji Faktor Amplifikasi?", "id": "Mengukur Transfer Tegangan Antar Belitan Frekuensi Tinggi." },
  { "en": "Apa Itu Fenomena Telescoping Belitan?", "id": "Pergeseran Aksial Belitan Akibat Gaya Elektrodinamik." },
  { "en": "Apa Itu Kegagalan Baut Inti?", "id": "Isolasi Baut Inti Rusak Menyebabkan Hubung Singkat." },
  { "en": "Apa Itu Restraint Harmonik?", "id": "Fitur Relai Diferensial Agar Tidak Trip." },
  { "en": "Fungsi Restraint Harmonik?", "id": "Membedakan Arus Inrush Dengan Arus Gangguan Internal." },
  { "en": "Harmonik Khas Arus Inrush?", "id": "Harmonik Orde Kedua (2nd Harmonic) Dominan." },
  { "en": "Apa Itu Koordinasi Isolasi?", "id": "Mengoordinasikan Kekuatan Isolasi Dengan Pelindung." },
  { "en": "Contoh Koordinasi Isolasi?", "id": "Level Isolasi Trafo Di Atas Arrester." },
  { "en": "Apa Itu SIL?", "id": "Switching Impulse Insulation Level Untuk Tegangan Switching." },
  { "en": "Perbedaan BIL dan SIL?", "id": "BIL Untuk Petir SIL Untuk Proses Switching." },
  { "en": "Apa Itu Detektor Gas Online?", "id": "Memantau Gas Terlarut Dalam Minyak Secara Real-Time." },
  { "en": "Apa Itu Monitoring Kelembaban Online?", "id": "Memantau Kadar Air Dalam Minyak Terus Menerus." },
  { "en": "Apa Itu Penjepit Inti (Core Clamp)?", "id": "Struktur Baja Untuk Menjepit Tumpukan Laminasi Inti." },
  { "en": "Fungsi Penjepit Inti?", "id": "Memberikan Kekuatan Mekanis Dan Kurangi Getaran." },
  { "en": "Apa Itu Pengaturan Lead Belitan?", "id": "Jalur Kabel Dari Belitan Ke Bushing." },
  { "en": "Pentingnya Pengaturan Lead?", "id": "Memastikan Jarak Aman Dan Tahan Gaya Mekanis." },
  { "en": "Apa Itu Uji Vakum Tangki?", "id": "Memastikan Tangki Trafo Tidak Mengalami Kebocoran Udara." },
  { "en": "Kapan Uji Vakum Dilakukan?", "id": "Sebelum Proses Pengisian Minyak Trafo Pertama." },
  { "en": "Apa Itu Angka Kualitas Minyak (OFQ)?", "id": "Skor Gabungan Dari Berbagai Parameter Uji Minyak." },
  { "en": "Apa Itu Trafo Penurun Fasa?", "id": "Nama Lain Untuk Trafo Phase-Shifting Transformer." },
  { "en": "Apa Itu Tegangan Pemulihan (Recovery Voltage)?", "id": "Tegangan Lintas Kontak Pemutus Setelah Membuka Gangguan." },
  { "en": "Apa Itu Uji Ketahanan Hubung Singkat?", "id": "Uji Tipe Untuk Memverifikasi Kekuatan Mekanis Trafo." },
  { "en": "Apa Itu De-energized Tap Changer (DETC)?", "id": "Nama Lain Untuk Off-Load Tap Changer." },
  { "en": "Apa Itu Ruang Lingkup Standar IEC 60076?", "id": "Standar Internasional Utama Untuk Trafo Daya." },
  { "en": "Apa Itu Tingkat Kebisingan NEMA?", "id": "Standar Untuk Tingkat Suara Trafo Di Amerika." },
  { "en": "Apa Itu CTC?", "id": "Continuously Transposed Cable Untuk Belitan Arus Tinggi." },
  { "en": "Keuntungan CTC?", "id": "Mengurangi Rugi Arus Eddy Di Dalam Konduktor." },
  { "en": "Apa Itu Diverter Switch?", "id": "Komponen Utama OLTC Yang Memindahkan Arus Beban." },
  { "en": "Apa Itu Selector Switch?", "id": "Komponen OLTC Yang Memilih Tap Selanjutnya." },
  { "en": "Apa Itu Jarak Rambat (Creepage)?", "id": "Sama Dengan Creepage Distance Pada Isolator Bushing." },
  { "en": "Faktor Keamanan Jarak Rambat?", "id": "Tergantung Pada Tingkat Polusi Lingkungan Sekitar." },
  { "en": "Apa Itu Belitan Foil?", "id": "Belitan Menggunakan Lembaran Tembaga Atau Aluminium Tipis." },
  { "en": "Kelebihan Belitan Foil?", "id": "Kekuatan Tahan Hubung Singkat Sangat Baik." },
  { "en": "Di Mana Belitan Foil Digunakan?", "id": "Umumnya Pada Belitan Tegangan Rendah Trafo Distribusi." },
  { "en": "Apa Itu Roda Trafo?", "id": "Roda Untuk Memudahkan Perpindahan Trafo Di Lokasi." },
  { "en": "Fitur Roda Trafo?", "id": "Dapat Diatur Untuk Bergerak Searah Atau Berbelok." },
  { "en": "Apa Itu Lubang Angkat (Lifting Lug)?", "id": "Titik Tumpu Untuk Mengangkat Trafo Dengan Derek." },
  { "en": "Apa Itu Lubang Derek (Hauling Lug)?", "id": "Titik Untuk Menarik Trafo Secara Horizontal." },
  { "en": "Apa Itu Sistem Termosifon?", "id": "Sirkulasi Minyak Alami Akibat Perbedaan Massa Jenis." },
  { "en": "Bagaimana Termosifon Bekerja?", "id": "Minyak Panas Naik Minyak Dingin Turun." },
  { "en": "Apa Itu Analisis Modal?", "id": "Studi Frekuensi Resonansi Mekanis Struktur Trafo." },
  { "en": "Apa Itu Uji Faktor Daya Terkoreksi Suhu?", "id": "Mengonversi Hasil Uji Ke Suhu Referensi Standar." },
  { "en": "Apa Itu Trafo Tipe Kolom?", "id": "Nama Lain Untuk Trafo Tipe Inti (Core Type)." },
  { "en": "Apa Itu Tegangan Tahanan Frekuensi Daya?", "id": "Kemampuan Isolasi Menahan Tegangan AC Selama Satu Menit." },
  { "en": "Apa Itu Endotermik Dan Eksotermik?", "id": "Reaksi Kimia Yang Menyerap Atau Melepas Panas." },
  { "en": "Degradasi Isolasi Adalah?", "id": "Proses Kimia Yang Dipengaruhi Oleh Banyak Faktor." },
  { "en": "Apa Itu Katup Penguras Bawah?", "id": "Katup Untuk Menguras Minyak Dan Mengambil Sampel." },
  { "en": "Apa Itu Katup Filter Atas?", "id": "Katup Untuk Sirkulasi Minyak Selama Proses Filtrasi." },
  { "en": "Apa Itu Belitan Disk?", "id": "Belitan Terdiri Dari Tumpukan Cakram Datar (Disk)." },
  { "en": "Di Mana Belitan Disk Digunakan?", "id": "Umumnya Untuk Belitan Tegangan Tinggi Trafo Daya." },
  { "en": "Apa Itu Belitan Heliks?", "id": "Belitan Dengan Konduktor Dililitkan Secara Spiral." },
  { "en": "Di Mana Belitan Heliks Digunakan?", "id": "Untuk Belitan Arus Tinggi Dan Tegangan Rendah." },
  { "en": "Apa Itu Spasi Radial?", "id": "Jarak Isolasi Antara Belitan Dan Inti." },
  { "en": "Apa Itu Spasi Aksial?", "id": "Jarak Isolasi Di Antara Ujung Belitan." },
  { "en": "Apa Itu Cincin Tekan (Press Ring)?", "id": "Cincin Untuk Memberi Tekanan Penjepitan Pada Belitan." },
  { "en": "Bahan Cincin Tekan?", "id": "Terbuat Dari Transformerboard Atau Baja Laminasi." },
  { "en": "Apa Itu Relai Arus Lebih?", "id": "Proteksi Cadangan Jika Relai Diferensial Gagal Bekerja." },
  { "en": "Apa Itu Kurva Kerusakan Trafo?", "id": "Batas Kemampuan Termal Trafo Terhadap Arus Lebih." },
  { "en": "Pentingnya Kurva Kerusakan?", "id": "Untuk Koordinasi Dengan Pengaturan Relai Arus Lebih." },
  { "en": "Apa Itu Minyak Parafinik?", "id": "Jenis Minyak Mineral Dengan Kandungan Parafin." },
  { "en": "Apa Itu Minyak Naftenik?", "id": "Jenis Minyak Mineral Dengan Kandungan Naftenik." },
  { "en": "Karakteristik Minyak Naftenik?", "id": "Memiliki Titik Tuang Rendah Sifat Pendinginan Baik." },
  { "en": "Apa Itu Umur Teknis?", "id": "Periode Waktu Hingga Trafo Tidak Lagi Aman." },
  { "en": "Apa Itu Umur Ekonomis?", "id": "Periode Waktu Hingga Biaya Operasi Terlalu Tinggi." },
  { "en": "Apa Itu Karbon Monoksida (CO)?", "id": "Gas Indikasi Dekomposisi Termal Kertas Isolasi." },
  { "en": "Apa Itu Karbon Dioksida (CO2)?", "id": "Produk Akhir Degradasi Selulosa Dan Minyak." },
  { "en": "Rasio CO2/CO Menunjukkan Apa?", "id": "Tingkat Keterlibatan Kertas Dalam Suatu Gangguan." },
  { "en": "Rasio Rendah CO2/CO?", "id": "Menandakan Gangguan Suhu Sangat Tinggi Berlangsung Cepat." },
  { "en": "Apa Itu Belitan Common?", "id": "Belitan Yang Digunakan Bersama Pada Autotransformer." },
  { "en": "Apa Itu Belitan Serial?", "id": "Belitan Penambah Tegangan Pada Sebuah Autotransformer." },
  { "en": "Daya Konduksi Autotrafo?", "id": "Daya Ditransfer Langsung Tanpa Proses Induksi." },
  { "en": "Daya Induksi Autotrafo?", "id": "Daya Ditransfer Melalui Kopling Medan Magnet." },
  { "en": "Apa Itu Marshalling Box?", "id": "Kotak Terminal Untuk Kabel Kontrol Dan Proteksi." },
  { "en": "Fungsi Marshalling Box?", "id": "Mengumpulkan Semua Kabel Sekunder Dan Sinyal." },
  { "en": "Apa Itu Acoustic Enclosure?", "id": "Struktur Dinding Untuk Meredam Kebisingan Trafo." },
  { "en": "Apa Itu Bund Wall?", "id": "Dinding Penahan Sekunder Di Sekitar Trafo." },
  { "en": "Fungsi Bund Wall?", "id": "Menampung Minyak Jika Terjadi Kebocoran Tangki." },
  { "en": "Apa Itu Winding Torsion?", "id": "Gerakan Memutar Belitan Saat Terjadi Gangguan." },
  { "en": "Apa Itu Uji Leakage Reactance?", "id": "Nama Lain Untuk Uji Impedansi Hubung Singkat." },
  { "en": "Apa Itu Kaki Inti (Core Limb)?", "id": "Bagian Vertikal Inti Yang Dikelilingi Belitan." },
  { "en": "Apa Itu Yoke Inti?", "id": "Bagian Horizontal Inti Penghubung Antar Kaki." },
  { "en": "Rapat Fluks Di Yoke?", "id": "Umumnya Sama Dengan Rapat Fluks Di Kaki." },
  { "en": "Apa Itu Pelindung Fluks (Flux Shield)?", "id": "Pelat Konduktif Untuk Mengalihkan Fluks Bocor." },
  { "en": "Tujuan Pelindung Fluks?", "id": "Mencegah Pemanasan Lokal Pada Dinding Tangki." },
  { "en": "Apa Itu Bushing Epoxy Resin?", "id": "Bushing Padat Dicetak Dari Bahan Resin Epoksi." },
  { "en": "Di Mana Bushing Epoxy Digunakan?", "id": "Pada Trafo Tipe Kering Dan Switchgear." },
  { "en": "Apa Itu Cincin Korona?", "id": "Cincin Logam Halus Di Ujung Bushing HV." },
  { "en": "Fungsi Cincin Korona?", "id": "Mengurangi Stres Medan Listrik Mencegah Korona." },
  { "en": "Apa Itu Penandaan Terminal?", "id": "Identifikasi Terminal Sesuai Standar (HV Dan LV)." },
  { "en": "Contoh Penandaan Terminal HV?", "id": "H1 H2 H3 Untuk Sisi Tegangan Tinggi." },
  { "en": "Contoh Penandaan Terminal LV?", "id": "X1 X2 X3 Untuk Sisi Tegangan Rendah." },
  { "en": "Apa Itu Proteksi Cross-Differential?", "id": "Proteksi Untuk Bank Trafo Satu Fasa." },
  { "en": "Apa Itu Blocking Harmonik Kedua?", "id": "Fitur Relai Diferensial Mencegah Trip Salah." },
  { "en": "Apa Itu Refurbishment Trafo?", "id": "Perbaikan Besar Untuk Memperpanjang Umur Trafo." },
  { "en": "Contoh Pekerjaan Refurbishment?", "id": "Penggantian Belitan Gasket Dan Pengecatan Ulang." },
  { "en": "Kapan Refurbishment Diperlukan?", "id": "Saat Kondisi Trafo Menurun Tapi Masih Layak." },
  { "en": "Apa Itu Penilaian Akhir Umur?", "id": "Evaluasi Komprehensif Untuk Menentukan Sisa Umur." },
  { "en": "Apa Itu Faktor Distribusi Kapasitif?", "id": "Mempengaruhi Distribusi Tegangan Impuls Awal." },
  { "en": "Apa Itu Osilasi Frekuensi Tinggi?", "id": "Getaran Tegangan Dan Arus Saat Transien." },
  { "en": "Apa Itu Uji Imunitas Medan Elektromagnetik?", "id": "Memastikan Perangkat Kontrol Tahan Gangguan Eksternal." },
  { "en": "Apa Itu Tegangan Langkah (Step Voltage)?", "id": "Perbedaan Tegangan Antara Dua Kaki Seseorang." },
  { "en": "Apa Itu Tegangan Sentuh (Touch Voltage)?", "id": "Tegangan Antara Tangan Dan Kaki Seseorang." },
  { "en": "Pentingnya Pengetanahan Untuk Keamanan?", "id": "Membatasi Tegangan Langkah Dan Tegangan Sentuh." },
  { "en": "Apa Itu Pemanas Anti Kondensasi?", "id": "Pemanas Di Marshalling Box Mencegah Pengembunan." },
  { "en": "Apa Itu Tegangan Sirkuit Terbuka?", "id": "Tegangan Di Terminal Sekunder Tanpa Beban." },
  { "en": "Apa Itu Arus Hubung Singkat Puncak?", "id": "Nilai Sesaat Maksimum Arus Hubung Singkat." },
  { "en": "Apa Itu Komponen Asimetris Arus?", "id": "Komponen DC Yang Meluruh Pada Arus Gangguan." },
  { "en": "Apa Itu Trafo Suhu Tinggi?", "id": "Trafo Dengan Sistem Isolasi Kelas Tinggi." },
  { "en": "Bahan Isolasi Suhu Tinggi?", "id": "Kertas Aramid Mika Dan Resin Silikon." },
  { "en": "Apa Itu Pembebanan Darurat (Emergency Loading)?", "id": "Membebani Trafo Di Atas Rating Sesaat." },
  { "en": "Apa Itu Kehilangan Kehidupan (Loss of Life)?", "id": "Penuaan Isolasi Dipercepat Akibat Suhu Tinggi." },
  { "en": "Apa Itu Dokumen Uji Pabrik?", "id": "Laporan Hasil Semua Pengujian Di Pabrik." },
  { "en": "Contoh Uji Rutin?", "id": "Pengukuran Tahanan Belitan Rasio Dan Polaritas." },
  { "en": "Contoh Uji Tipe?", "id": "Uji Kenaikan Suhu Dan Uji Impuls." },
  { "en": "Contoh Uji Khusus?", "id": "Pengukuran Impedansi Urutan Nol Dan Kebisingan." },
  { "en": "Apa Itu Sakelar Pembumian (Earthing Switch)?", "id": "Sakelar Untuk Menghubungkan Belitan Ke Tanah." },
  { "en": "Fungsi Sakelar Pembumian?", "id": "Untuk Keamanan Personel Selama Pekerjaan Pemeliharaan." },
  { "en": "Apa Itu Pengujian Online?", "id": "Pengujian Dilakukan Saat Trafo Masih Beroperasi." },
  { "en": "Apa Itu Pengujian Offline?", "id": "Pengujian Dilakukan Saat Trafo Dipadamkan." },
  { "en": "Apa Itu Sistem Scada?", "id": "Supervisory Control And Data Acquisition System." },
  { "en": "Peran Scada Untuk Trafo?", "id": "Memantau Dan Mengontrol Trafo Dari Jarak Jauh." },
  { "en": "Apa Itu Intelligent Electronic Device (IED)?", "id": "Perangkat Digital Seperti Relai Proteksi Modern." },
  { "en": "Standar Komunikasi Gardu Induk?", "id": "IEC 61850 Untuk Otomatisasi Gardu Induk." },
  { "en": "Apa Itu Winding Hot Spot Factor?", "id": "Faktor Pengali Untuk Memperkirakan Suhu Titik Panas." },
  { "en": "Apa Itu Minyak Mineral Baru?", "id": "Minyak Yang Belum Pernah Dipakai Sama Sekali." },
  { "en": "Apa Itu Minyak Rekondisi?", "id": "Minyak Bekas Yang Diproses Ulang Sederhana." },
  { "en": "Apa Itu Minyak Regenerasi?", "id": "Minyak Bekas Diproses Untuk Pulihkan Sifatnya." },
  { "en": "Proses Regenerasi Minyak?", "id": "Menghilangkan Asam Sludge Dan Produk Penuaan." },
  { "en": "Apa Itu Uji Stabilitas Oksidasi?", "id": "Mengukur Ketahanan Minyak Terhadap Proses Oksidasi." },
  { "en": "Apa Itu Belitan Kontinu?", "id": "Belitan Disk Dengan Koneksi Seri Berkelanjutan." },
  { "en": "Apa Itu Tekanan Penjepitan Belitan?", "id": "Tekanan Mekanis Untuk Menjaga Belitan Tetap Padat." },
  { "en": "Kenapa Penjepitan Penting?", "id": "Mencegah Gerakan Belitan Saat Terjadi Hubung Singkat." },
  { "en": "Apa Itu Pengeringan Belitan?", "id": "Proses Menghilangkan Kelembaban Dari Kertas Isolasi." },
  { "en": "Dampak Kelembaban Pada Isolasi?", "id": "Menurunkan Kekuatan Dielektrik Dan Mempercepat Penuaan." },
  { "en": "Apa Itu Indeks Polarisasi (PI)?", "id": "Rasio Tahanan Isolasi 10 Menit Dan 1 Menit." },
  { "en": "Nilai PI Yang Baik?", "id": "Nilai Di Atas Dua Menunjukkan Isolasi Kering." },
  { "en": "Apa Itu Dielectric Absorption Ratio (DAR)?", "id": "Rasio Tahanan Isolasi 60 Detik Dan 30 Detik." },
  { "en": "Apa Itu Arus Konduksi?", "id": "Arus Yang Mengalir Melalui Volume Bahan Isolasi." },
  { "en": "Apa Itu Arus Absorpsi?", "id": "Arus Untuk Polarisasi Molekul Bahan Dielektrik." },
  { "en": "Apa Itu Arus Kapasitif?", "id": "Arus Pengisian Kapasitansi Awal Saat Pengujian." },
  { "en": "Apa Itu Tangki Fleksibel?", "id": "Dinding Tangki Bergelombang Yang Bisa Mengembang." },
  { "en": "Fungsi Tangki Fleksibel?", "id": "Mengakomodasi Ekspansi Termal Minyak Tanpa Konservator." },
  { "en": "Apa Itu Katup Kupu-Kupu (Butterfly Valve)?", "id": "Katup Untuk Mengisolasi Radiator Dari Tangki Utama." },
  { "en": "Apa Itu Jendela Inspeksi?", "id": "Lubang Di Sisi Tangki Untuk Inspeksi Internal." },
  { "en": "Apa Itu Pelat Rating Tambahan?", "id": "Pelat Informasi Untuk Komponen Seperti Tap Changer." },
  { "en": "Apa Itu Pengujian Tipe Desain?", "id": "Pengujian Yang Dilakukan Pada Satu Unit Prototipe." },
  { "en": "Apa Itu Uji Penerimaan Pabrik (FAT)?", "id": "Factory Acceptance Test Disaksikan Oleh Pembeli." },
  { "en": "Apa Itu Uji Lapangan (Site Test)?", "id": "Pengujian Dilakukan Setelah Trafo Tiba Di Lokasi." },
  { "en": "Tujuan Uji Lapangan?", "id": "Memastikan Tidak Ada Kerusakan Selama Transportasi." },
  { "en": "Apa Itu Komisioning Trafo?", "id": "Proses Akhir Sebelum Trafo Resmi Dioperasikan." },
  { "en": "Apa Itu Daftar Periksa Komisioning?", "id": "Daftar Semua Item Yang Harus Diperiksa." },
  { "en": "Apa Itu Diagram Satu Garis?", "id": "Representasi Sederhana Sistem Tenaga Listrik." },
  { "en": "Apa Itu Diagram Pengkabelan?", "id": "Diagram Rinci Semua Koneksi Kabel Listrik." },
  { "en": "Apa Itu Kurva Saturasi CT?", "id": "Grafik Kinerja Trafo Arus Sebelum Saturasi." },
  { "en": "Pentingnya Kurva Saturasi CT?", "id": "Memastikan CT Tidak Saturasi Saat Gangguan." },
  { "en": "Apa Itu Beban CT?", "id": "Total Impedansi Yang Terhubung Ke Sekunder CT." },
  { "en": "Apa Itu Pemeliharaan Berbasis Kondisi?", "id": "Pemeliharaan Dilakukan Berdasarkan Kondisi Aktual Peralatan." },
  { "en": "Apa Itu Pemeliharaan Preventif?", "id": "Pemeliharaan terjadwal Untuk Mencegah Kerusakan." },
  { "en": "Apa Itu Pemeliharaan Korektif?", "id": "Perbaikan Dilakukan Setelah Terjadi Sebuah Kerusakan." },
  { "en": "Manajemen Aset Trafo?", "id": "Strategi Untuk Mengoptimalkan Siklus Hidup Trafo." },
  { "en": "Apa Itu Gaya Aksial?", "id": "Gaya Paralel Dengan Sumbu Belitan Trafo." },
  { "en": "Apa Itu Gaya Radial?", "id": "Gaya Tegak Lurus Sumbu Belitan Trafo." },
  { "en": "Gaya Radial Menyebabkan Apa?", "id": "Belitan Luar Meregang Belitan Dalam Tertekan." },
  { "en": "Gaya Aksial Menyebabkan Apa?", "id": "Menekan Belitan Atau Menyebabkan Gerakan Teleskopik." },
  { "en": "Apa Itu Metode RVM?", "id": "Recovery Voltage Method Untuk Analisis Isolasi." },
  { "en": "RVM Menganalisis Apa?", "id": "Spektrum Polarisasi Lambat Sistem Isolasi." },
  { "en": "Apa Itu Jacking Pad?", "id": "Titik Tumpu Kuat Untuk Mendongkrak Trafo." },
  { "en": "Apa Itu Pulling Eye?", "id": "Lubang Untuk Menarik Trafo Secara Horizontal." },
  { "en": "Apa Itu Analisis Kegagalan Akar Penyebab?", "id": "Investigasi Sistematis Untuk Menemukan Penyebab Kegagalan." },
  { "en": "Apa Itu Proteksi Standby Earth Fault?", "id": "Proteksi Cadangan Untuk Gangguan Tanah Sistem." },
  { "en": "Apa Itu Varnish Isolasi?", "id": "Lapisan Pelindung Untuk Belitan Dan Inti." },
  { "en": "Fungsi Varnish Isolasi?", "id": "Meningkatkan Kekuatan Dielektrik Dan Lindungi Dari Lembab." },
  { "en": "Apa Itu AMDT?", "id": "Amorphous Metal Distribution Transformer Technology." },
  { "en": "Apa Itu Blok Jarak (Spacer Block)?", "id": "Isolasi Padat Pemisah Antar Disk Belitan." },
  { "en": "Fungsi Blok Jarak?", "id": "Membentuk Saluran Pendingin Minyak Antar Belitan." },
  { "en": "Apa Itu Tirisan (Weep Hole)?", "id": "Lubang Kecil Untuk Mendeteksi Kebocoran Internal." },
  { "en": "Apa Itu Air Bag Konservator?", "id": "Kantong Fleksibel Pemisah Minyak Dari Udara." },
  { "en": "Fungsi Air Bag?", "id": "Mencegah Kontak Langsung Minyak Dengan Oksigen." },
  { "en": "Apa Itu Uji Impuls Gelombang Terpotong?", "id": "Uji Impuls Yang Dipotong Tiba-Tiba." },
  { "en": "Tujuan Uji Gelombang Terpotong?", "id": "Mensimulasikan Flashover Isolator Eksternal Dekat Trafo." },
  { "en": "Apa Itu Kontak Alarm Buchholz?", "id": "Kontak Untuk Akumulasi Gas Yang Lambat." },
  { "en": "Apa Itu Kontak Trip Buchholz?", "id": "Kontak Untuk Aliran Minyak Mendadak Cepat." },
  { "en": "Apa Itu Katup Pengambil Sampel?", "id": "Katup Khusus Untuk Mengambil Sampel Minyak." },
  { "en": "Apa Itu Moisture in Oil vs Moisture in Paper?", "id": "Kelembaban Dalam Minyak Dan Dalam Kertas." },
  { "en": "Di Mana Mayoritas Air Tersimpan?", "id": "Lebih Dari Sembilan Puluh Sembilan Persen." },
  { "en": "Apa Itu Kesetimbangan Kelembaban?", "id": "Distribusi Air Antara Kertas Dan Minyak." },
  { "en": "Pengaruh Suhu Pada Kesetimbangan?", "id": "Suhu Naik Air Pindah Ke Minyak." },
  { "en": "Apa Itu Persen Air Saturasi?", "id": "Jumlah Maksimum Air Yang Larut Dalam Minyak." },
  { "en": "Apa Itu Pasivator Tembaga?", "id": "Aditif Kimia Untuk Mencegah Korosi Tembaga." },
  { "en": "Apa Itu Belerang Korosif?", "id": "Senyawa Belerang Dalam Minyak Yang Korosif." },
  { "en": "Dampak Belerang Korosif?", "id": "Membentuk Tembaga Sulfida Pada Konduktor." },
  { "en": "Apa Itu Uji DBDS?", "id": "Uji Untuk Mendeteksi Senyawa Belerang Korosif." },
  { "en": "Apa Itu Penuaan Kinetik?", "id": "Model Matematika Untuk Memprediksi Penuaan Isolasi." },
  { "en": "Apa Itu Analisis Pasca-Mortem?", "id": "Pemeriksaan Mendetail Trafo Setelah Mengalami Kegagalan." },
  { "en": "Tujuan Analisis Pasca-Mortem?", "id": "Menentukan Akar Penyebab Kegagalan Secara Pasti." },
  { "en": "Apa Itu Kekuatan Tahan Dinamis?", "id": "Kemampuan Belitan Menahan Gaya Mekanis Hubung Singkat." },
  { "en": "Apa Itu Kekuatan Tahan Termal?", "id": "Kemampuan Belitan Menahan Panas Hubung Singkat." },
  { "en": "Apa Itu Batang Pengikat (Tie Rod)?", "id": "Batang Baja Untuk Menjepit Inti Besi." },
  { "en": "Isolasi Batang Pengikat?", "id": "Sangat Penting Untuk Mencegah Sirkulasi Arus." },
  { "en": "Apa Itu Trafo Cast Coil?", "id": "Jenis Trafo Kering Mirip Tipe Cast Resin." },
  { "en": "Apa Itu Kemiringan Relai Diferensial?", "id": "Karakteristik Untuk Mencegah Trip Akibat Beban Lebih." },
  { "en": "Apa Itu Over-Excitasi?", "id": "Sama Dengan Kondisi Over-Fluxing Pada Transformator." },
  { "en": "Apa Itu Pemanasan Berlebih Sirkuit Magnetik?", "id": "Pemanasan Lokal Pada Inti Akibat Hubung Singkat." },
  { "en": "Apa Itu Desain Tahan Gempa?", "id": "Desain Trafo Yang Tahan Terhadap Guncangan Gempa." },
  { "en": "Apa Itu Analisis Spektrum Respon?", "id": "Metode Untuk Menganalisis Respon Struktur Gempa." },
  { "en": "Apa Itu Bantalan Peredam Getaran?", "id": "Bantalan Elastis Di Bawah Trafo." },
  { "en": "Fungsi Bantalan Peredam?", "id": "Mengisolasi Getaran Trafo Dari Pondasi Sekitarnya." },
  { "en": "Apa Itu Indeks Kesehatan Aset?", "id": "Istilah Lain Untuk Health Index (HI) Trafo." },
  { "en": "Apa Itu Penandaan Polaritas?", "id": "Tanda Titik Untuk Menunjukkan Arah Tegangan Sesaat." },
  { "en": "Apa Itu Trafo Quad-Booster?", "id": "Tipe Khusus Trafo Phase-Shifting Sangat Kompleks." },
  { "en": "Apa Itu Tegangan Tahanan Isolasi?", "id": "Istilah Lain Untuk Pengujian Tahanan Isolasi." },
  { "en": "Apa Itu Uji Tumpahan Cat?", "id": "Memastikan Kualitas Dan Ketebalan Lapisan Cat." },
  { "en": "Apa Itu Sertifikat Kalibrasi?", "id": "Dokumen Yang Menyatakan Alat Uji Terkalibrasi." },
  { "en": "Apa Itu Diagram Lingkaran Trafo?", "id": "Representasi Grafis Performa Trafo Secara Lengkap." },
  { "en": "Apa Itu Uji Frekuensi Sapu?", "id": "Istilah Lain Untuk Uji SFRA (Sweep Frequency)." },
  { "en": "Interpretasi Hasil SFRA?", "id": "Membandingkan Hasil Dengan Referensi Awal (Fingerprint)." },
  { "en": "Penyimpangan SFRA Menunjukkan Apa?", "id": "Potensi Deformasi Belitan Atau Masalah Inti." },
  { "en": "Apa Itu Resistansi Dinamis Belitan?", "id": "Pengukuran Selama Operasi Tap Changer (OLTC)." },
  { "en": "Tujuan Uji Resistansi Dinamis?", "id": "Mendeteksi Masalah Pada Kontak Diverter Switch." },
  { "en": "Apa Itu Pendinginan Tiga Tahap?", "id": "Sistem Pendingin Dengan Beberapa Level Kipas." },
  { "en": "Apa Itu Indeks Viskositas Minyak?", "id": "Ukuran Perubahan Viskositas Minyak Terhadap Suhu." },
  { "en": "Apa Itu Tegangan Interfacial?", "id": "Sama Dengan IFT (Interfacial Tension) Minyak." },
  { "en": "Apa Itu Tekanan Nitrogen?", "id": "Tekanan Gas Di Atas Minyak Trafo Hermetik." },
  { "en": "Fungsi Tekanan Nitrogen?", "id": "Memberi Bantalan Dan Mencegah Oksidasi Minyak." },
  { "en": "Apa Itu Dokumen Manual O&M?", "id": "Buku Panduan Operasi Dan Pemeliharaan Trafo." },
  { "en": "Apa Itu Daftar Suku Cadang?", "id": "Daftar Komponen Pengganti Yang Direkomendasikan." },
  { "en": "Apa Itu Konservasi Energi?", "id": "Upaya Mengurangi Rugi-Rugi Daya Pada Trafo." },
  { "en": "Peran Trafo Efisiensi Tinggi?", "id": "Mengurangi Emisi Karbon Dari Sektor Ketenagalistrikan." },
  { "en": "Apa Itu Kawat Litz?", "id": "Kawat Terdiri Dari Banyak Untaian Terisolasi." },
  { "en": "Fungsi Kawat Litz?", "id": "Mengurangi Skin Effect Pada Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Sifat Hidrofobik?", "id": "Kemampuan Permukaan Untuk Menolak Air (Efek Daun Talas)." },
  { "en": "Pentingnya Sifat Hidrofobik?", "id": "Penting Untuk Isolator Bushing Komposit Silikon." },
  { "en": "Apa Itu Siklus Beban Harian?", "id": "Pola Variasi Beban Trafo Selama 24 Jam." },
  { "en": "Apa Itu Faktor Puncak (Crest Factor)?", "id": "Rasio Nilai Puncak Terhadap Nilai RMS." },
  { "en": "Dampak Beban Non-Linier?", "id": "Meningkatkan Faktor Puncak Dan Distorsi Harmonisa." },
  { "en": "Apa Itu Titik Curie?", "id": "Suhu Di Mana Bahan Feromagnetik Kehilangan Magnetnya." },
  { "en": "Apa Itu Domain Magnetik?", "id": "Wilayah Kecil Dalam Bahan Dengan Arah Magnet." },
  { "en": "Proses Magnetisasi Dalam Inti?", "id": "Menyelaraskan Domain Magnetik Dengan Medan Eksternal." },
  { "en": "Apa Itu Sirkuit Penahan (Snubber)?", "id": "Rangkaian Untuk Meredam Lonjakan Tegangan Transien." },
  { "en": "Apa Itu Gelombang Berjalan (Traveling Wave)?", "id": "Propagasi Gelombang Impuls Sepanjang Belitan Trafo." },
  { "en": "Apa Itu Analisis Elemen Hingga (FEA)?", "id": "Simulasi Komputer Untuk Analisis Mekanis Termal." },
  { "en": "Aplikasi FEA Dalam Desain Trafo?", "id": "Menganalisis Stres Mekanis Dan Distribusi Suhu." },
  { "en": "Apa Itu Papan Nama Tambahan Iklim?", "id": "Informasi Tambahan Untuk Operasi Di Kondisi Ekstrem." },
  { "en": "Apa Itu Korosi Galvanik?", "id": "Korosi Akibat Dua Logam Berbeda Bersentuhan." },
  { "en": "Pencegahan Korosi Galvanik?", "id": "Menggunakan Gasket Atau Pelapis Isolasi Antar Logam." },
  { "en": "Apa Itu Transportasi Trafo?", "id": "Proses Memindahkan Trafo Dari Pabrik Ke Lokasi." },
  { "en": "Tantangan Transportasi Trafo?", "id": "Ukuran Berat Dan Keterbatasan Infrastruktur Jalan." },
  { "en": "Apa Itu Indikator Guncangan?", "id": "Alat Untuk Merekam Guncangan Selama Transportasi." },
  { "en": "Tujuan Indikator Guncangan?", "id": "Sebagai Bukti Jika Terjadi Penanganan Yang Kasar." },
  { "en": "Apa Itu Pemasangan Di Atas Pondasi?", "id": "Proses Menempatkan Trafo Di Atas Pondasi Sipil." },
  { "en": "Pentingnya Leveling Trafo?", "id": "Memastikan Distribusi Berat Merata Dan Fungsi Optimal." },
  { "en": "Apa Itu Tegangan Langkah OLTC?", "id": "Besar Perubahan Tegangan Per Satu Langkah Tap." },
  { "en": "Apa Itu Rentang Tap OLTC?", "id": "Total Rentang Regulasi Tegangan Dari Minimum Maksimum." },
  { "en": "Apa Itu Umur Kontak OLTC?", "id": "Jumlah Operasi Sebelum Kontak Perlu Diganti." },
  { "en": "Pemeliharaan OLTC Melibatkan Apa?", "id": "Pemeriksaan Kualitas Minyak Dan Penggantian Kontak." },
  { "en": "Apa Itu Trafo Terendam (Submersible)?", "id": "Trafo Didesain Untuk Beroperasi Di Bawah Air." },
  { "en": "Aplikasi Trafo Terendam?", "id": "Pada Jaringan Listrik Bawah Tanah Perkotaan Padat." },
  { "en": "Apa Itu Standar Kinerja Efisiensi?", "id": "Peraturan Pemerintah Yang Menetapkan Efisiensi Minimum Trafo." },
  { "en": "Apa Itu Transformator Cerdas (Smart Transformer)?", "id": "Transformator Dengan Kontrol Elektronik Daya Canggih." }



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
