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


  { "en": "Apa komponen dasar pembuat transistor?", "id": "Transistor terbuat dari bahan semikonduktor, paling umum adalah silikon." },
  { "en": "Apa dua fungsi utama transistor?", "id": "Dua fungsi utamanya adalah sebagai saklar elektronik dan sebagai penguat sinyal." },
  { "en": "Sebutkan tiga terminal pada transistor bipolar (BJT).", "id": "Tiga terminal pada BJT adalah Emitor, Basis, dan Kolektor." },
  { "en": "Sebutkan dua jenis dasar transistor bipolar (BJT).", "id": "Dua jenis dasar BJT adalah NPN dan PNP." },
  { "en": "Apa kepanjangan dari BJT?", "id": "BJT adalah singkatan dari Bipolar Junction Transistor." },
  { "en": "Apa yang mengontrol aliran arus utama pada BJT?", "id": "Arus kecil yang mengalir pada terminal Basis mengontrol aliran arus yang lebih besar antara Kolektor dan Emitor." },
  { "en": "Apa analogi sederhana untuk cara kerja transistor?", "id": "Mirip seperti keran air, di mana putaran kecil pada gagang (Basis) mengontrol aliran air yang deras (arus Kolektor-Emitor)." },
  { "en": "Apa perbedaan utama antara transistor NPN dan PNP?", "id": "Perbedaan utamanya terletak pada arah aliran arusnya dan polaritas tegangan yang dibutuhkan untuk mengaktifkannya." },
  { "en": "Apa yang dimaksud dengan 'doping' pada semikonduktor?", "id": "Doping adalah proses menambahkan material lain (pengotor) ke semikonduktor murni untuk mengubah sifat kelistrikannya." },
  { "en": "Apa itu semikonduktor tipe-N?", "id": "Semikonduktor tipe-N adalah semikonduktor yang memiliki kelebihan elektron sebagai pembawa muatan." },
  { "en": "Apa itu semikonduktor tipe-P?", "id": "Semikonduktor tipe-P adalah semikonduktor yang memiliki kelebihan 'hole' (kekurangan elektron) sebagai pembawa muatan." },
  { "en": "Apa kepanjangan dari FET?", "id": "FET adalah singkatan dari Field-Effect Transistor." },
  { "en": "Sebutkan tiga terminal pada Field-Effect Transistor (FET).", "id": "Tiga terminal pada FET adalah Source, Gate, dan Drain." },
  { "en": "Apa yang mengontrol aliran arus pada FET?", "id": "Tegangan yang diberikan pada terminal Gate mengontrol aliran arus antara Drain dan Source." },
  { "en": "Apa perbedaan mendasar antara BJT dan FET?", "id": "BJT dikontrol oleh arus (di Basis), sedangkan FET dikontrol oleh tegangan (di Gate)." },
  { "en": "Mengapa transistor disebut komponen aktif?", "id": "Karena transistor dapat memberikan penguatan sinyal (amplifikasi), artinya daya sinyal output bisa lebih besar dari daya sinyal input." },
  { "en": "Apa itu daerah 'cut-off' pada transistor?", "id": "Daerah cut-off adalah kondisi di mana transistor dalam keadaan 'OFF' atau tidak menghantarkan arus." },
  { "en": "Apa itu daerah 'saturasi' pada transistor?", "id": "Daerah saturasi adalah kondisi di mana transistor dalam keadaan 'ON' penuh, menghantarkan arus secara maksimal." },
  { "en": "Untuk fungsi saklar, di daerah mana saja transistor beroperasi?", "id": "Untuk fungsi saklar, transistor beroperasi di antara daerah cut-off (OFF) dan saturasi (ON)." },
  { "en": "Untuk fungsi penguat, di daerah mana transistor beroperasi?", "id": "Untuk fungsi penguat, transistor beroperasi di daerah aktif, yaitu di antara cut-off dan saturasi." },
  { "en": "Apa bahan semikonduktor yang paling umum digunakan saat ini?", "id": "Silikon adalah bahan semikonduktor yang paling dominan digunakan dalam pembuatan transistor dan sirkuit terpadu (IC)." },
  { "en": "Apa fungsi resistor yang terhubung ke Basis transistor?", "id": "Resistor Basis berfungsi untuk membatasi atau mengontrol jumlah arus yang masuk ke Basis agar transistor tidak rusak." },
  { "en": "Apa itu 'gain' atau penguatan arus pada transistor BJT?", "id": "Gain adalah kemampuan transistor untuk menghasilkan arus Kolektor yang jauh lebih besar dari arus Basis pemicunya." },
  { "en": "Apa yang terjadi jika transistor terlalu panas?", "id": "Jika terlalu panas (overheating), transistor bisa mengalami kerusakan permanen dan tidak dapat berfungsi lagi." },
  { "en": "Apa fungsi 'heatsink' pada transistor daya?", "id": "Heatsink adalah komponen pendingin yang membantu membuang panas dari transistor ke udara sekitar." },
  { "en": "Siapa saja penemu transistor?", "id": "Transistor ditemukan oleh John Bardeen, Walter Brattain, dan William Shockley." },
  { "en": "Apa itu 'datasheet' komponen?", "id": "Datasheet adalah dokumen dari pabrikan yang berisi semua spesifikasi teknis dan karakteristik sebuah komponen." },
  { "en": "Bagaimana cara kerja transistor sebagai penguat sinyal?", "id": "Perubahan kecil pada sinyal input (di Basis atau Gate) menghasilkan perubahan besar yang sebanding pada sinyal output (di Kolektor atau Drain)." },
  { "en": "Apa itu 'biasing' transistor?", "id": "Biasing adalah proses pengaturan tegangan DC pada terminal-terminal transistor agar ia siap bekerja pada titik operasi yang diinginkan (misalnya, di daerah aktif)." },
  { "en": "Mengapa 'biasing' itu penting?", "id": "Tanpa biasing yang tepat, transistor tidak akan dapat menguatkan sinyal dengan benar dan dapat menyebabkan distorsi atau tidak berfungsi sama sekali." },
  { "en": "Apa itu 'arus bocor' (leakage current)?", "id": "Arus bocor adalah arus yang sangat kecil yang tetap mengalir meskipun transistor dalam kondisi 'OFF' atau cut-off." },
  { "en": "Apa fungsi transistor di dalam sebuah prosesor komputer (CPU)?", "id": "Di dalam CPU, miliaran transistor berfungsi sebagai saklar super cepat yang membentuk gerbang logika untuk melakukan pemrosesan data." },
  { "en": "Apa itu gerbang logika (logic gate)?", "id": "Gerbang logika adalah sirkuit elektronik dasar (terbuat dari transistor) yang melakukan operasi logika sederhana seperti AND, OR, dan NOT." },
  { "en": "Apa itu 'tegangan breakdown'?", "id": "Tegangan breakdown adalah tegangan maksimum yang bisa ditahan oleh transistor sebelum rusak." },
  { "en": "Apa itu fototransistor?", "id": "Fototransistor adalah jenis transistor khusus yang diaktifkan oleh cahaya, bukan oleh arus Basis." },
  { "en": "Apa kegunaan fototransistor?", "id": "Fototransistor digunakan dalam berbagai sensor cahaya, seperti pada remote control TV atau pendeteksi garis pada robot." },
  { "en": "Apa itu pasangan Darlington?", "id": "Pasangan Darlington adalah konfigurasi dua transistor BJT yang digabungkan untuk mendapatkan penguatan arus yang sangat tinggi." },
  { "en": "Apa keuntungan utama pasangan Darlington?", "id": "Keuntungannya adalah sensitivitas yang sangat tinggi, di mana arus input yang sangat kecil dapat mengontrol arus output yang sangat besar." },
  { "en": "Mengapa transistor bipolar disebut 'bipolar'?", "id": "Karena konduksi arusnya melibatkan dua jenis pembawa muatan, yaitu elektron dan 'hole'." },
  { "en": "Mengapa FET sering disebut 'unipolar'?", "id": "Karena konduksi arusnya hanya melibatkan satu jenis pembawa muatan mayoritas (elektron saja atau 'hole' saja)." },
  { "en": "Mana yang memiliki impedansi input lebih tinggi, BJT atau FET?", "id": "FET memiliki impedansi input yang jauh lebih tinggi dibandingkan BJT." },
  { "en": "Apa keuntungan dari impedansi input yang tinggi?", "id": "Impedansi input yang tinggi berarti penguat tidak banyak 'membebani' sumber sinyal, sehingga sinyal tidak melemah saat masuk ke penguat." },
  { "en": "Apa kepanjangan dari MOSFET?", "id": "MOSFET adalah singkatan dari Metal-Oxide-Semiconductor Field-Effect Transistor." },
  { "en": "Apa fungsi lapisan 'oksida' pada MOSFET?", "id": "Lapisan oksida berfungsi sebagai isolator antara terminal Gate dan badan transistor, yang menyebabkannya memiliki impedansi input sangat tinggi." },
  { "en": "Sebutkan dua mode utama MOSFET.", "id": "Dua mode utama MOSFET adalah mode deplesi (depletion) dan mode peningkatan (enhancement)." },
  { "en": "Apa itu 'optocoupler' atau 'opto-isolator'?", "id": "Optocoupler adalah komponen yang menggunakan cahaya untuk mengirim sinyal antara dua sirkuit yang terisolasi secara elektrik." },
  { "en": "Apa isi dari sebuah optocoupler?", "id": "Sebuah optocoupler berisi sumber cahaya (seperti LED) dan sebuah sensor cahaya (seperti fototransistor) dalam satu kemasan." },
  { "en": "Mengapa isolasi elektrik menggunakan optocoupler itu penting?", "id": "Untuk melindungi sirkuit yang sensitif dari tegangan tinggi di sirkuit lain dan untuk mengurangi gangguan (noise)." },
  { "en": "Apa itu komponen 'SMD'?", "id": "SMD (Surface-Mount Device) adalah jenis komponen elektronik yang sangat kecil dan dipasang langsung di permukaan papan sirkuit (PCB)." },
  { "en": "Apa kelebihan komponen SMD dibandingkan komponen biasa (through-hole)?", "id": "Komponen SMD memungkinkan pembuatan perangkat elektronik yang jauh lebih kecil dan ringkas." },
  { "en": "Apa itu 'distorsi' pada sinyal audio yang dikuatkan?", "id": "Distorsi adalah perubahan bentuk sinyal asli yang tidak diinginkan setelah dikuatkan, seringkali terdengar sebagai suara 'pecah' atau 'cacat'." },
  { "en": "Apa yang menyebabkan distorsi 'clipping'?", "id": "Clipping terjadi ketika sinyal input terlalu kuat, sehingga transistor dipaksa beroperasi melewati batas daerah aktifnya (masuk ke saturasi atau cut-off)." },
  { "en": "Apa itu 'titik-Q' (Quiescent Point)?", "id": "Titik-Q adalah titik operasi 'diam' dari transistor (arus dan tegangan DC) ketika tidak ada sinyal input yang diberikan." },
  { "en": "Sebutkan tiga konfigurasi dasar penguat BJT.", "id": "Tiga konfigurasi dasar adalah Common Emitter, Common Collector, dan Common Base." },
  { "en": "Konfigurasi mana yang paling umum digunakan untuk penguatan tegangan?", "id": "Konfigurasi Common Emitter adalah yang paling umum digunakan karena memberikan penguatan tegangan dan arus yang baik." },
  { "en": "Apa nama lain untuk konfigurasi Common Collector?", "id": "Konfigurasi Common Collector juga sering disebut sebagai 'Emitter Follower'." },
  { "en": "Apa fungsi utama konfigurasi Emitter Follower?", "id": "Fungsi utamanya adalah sebagai penyangga (buffer) sinyal, dengan penguatan arus tinggi tetapi penguatan tegangan mendekati satu." },
  { "en": "Apa yang dimaksud dengan 'semikonduktor intrinsik'?", "id": "Semikonduktor intrinsik adalah semikonduktor dalam bentuknya yang paling murni, tanpa ada atom pengotor (doping)." },
  { "en": "Apa itu 'pembawa muatan mayoritas'?", "id": "Dalam semikonduktor yang sudah di-doping, ini adalah jenis pembawa muatan (elektron atau hole) yang jumlahnya paling dominan." },
  { "en": "Apa itu 'pembawa muatan minoritas'?", "id": "Dalam semikonduktor yang sudah di-doping, ini adalah jenis pembawa muatan yang jumlahnya jauh lebih sedikit." },
  { "en": "Apa peran daerah Basis yang dibuat sangat tipis pada BJT?", "id": "Agar sebagian besar pembawa muatan dari Emitor dapat melewatinya dan mencapai Kolektor tanpa banyak yang 'hilang' di Basis." },
  { "en": "Apa itu arus Emitor?", "id": "Arus Emitor adalah total arus yang keluar dari (atau masuk ke) terminal Emitor, yang merupakan penjumlahan arus Basis dan Kolektor." },
  { "en": "Apa perbedaan transistor sinyal kecil dan transistor daya?", "id": "Transistor sinyal kecil untuk menangani sinyal level rendah, sedangkan transistor daya untuk mengontrol arus dan tegangan yang besar." },
  { "en": "Bagaimana transistor menyimpan data '1' atau '0' dalam memori?", "id": "Dengan berada dalam kondisi ON (saturasi) untuk mewakili '1' atau kondisi OFF (cut-off) untuk mewakili '0'." },
  { "en": "Apakah nilai 'gain' (penguatan) setiap transistor itu sama persis?", "id": "Tidak, bahkan untuk transistor dengan nomor seri yang sama, akan ada sedikit variasi nilai gain karena proses manufaktur." },
  { "en": "Apa itu 'Hukum Moore'?", "id": "Hukum Moore adalah pengamatan bahwa jumlah transistor dalam sebuah sirkuit terpadu (IC) bertambah dua kali lipat kira-kira setiap dua tahun." },
  { "en": "Apa fungsi kapasitor kopling dalam rangkaian penguat?", "id": "Kapasitor kopling berfungsi untuk melewatkan sinyal AC yang ingin dikuatkan, tetapi memblokir tegangan DC dari tahap sebelumnya." },
  { "en": "Apa itu 'tegangan ambang' (threshold voltage) pada MOSFET?", "id": "Ini adalah tegangan minimum pada Gate yang diperlukan untuk 'menyalakan' transistor dan memungkinkan arus mengalir." },
  { "en": "Apa yang terjadi pada transistor saat sambungan Basis-Emitor diberi bias maju?", "id": "Memberi bias maju pada sambungan Basis-Emitor adalah langkah pertama untuk 'menyalakan' transistor BJT." },
  { "en": "Apa yang terjadi pada transistor saat sambungan Basis-Emitor diberi bias mundur?", "id": "Jika diberi bias mundur, transistor BJT akan tetap dalam kondisi 'OFF' atau cut-off." },
  { "en": "Pada transistor NPN, terminal Basis harus lebih positif atau negatif dari Emitor?", "id": "Agar aktif, terminal Basis pada NPN harus lebih positif daripada terminal Emitor." },
  { "en": "Pada transistor PNP, terminal Basis harus lebih positif atau negatif dari Emitor?", "id": "Agar aktif, terminal Basis pada PNP harus lebih negatif daripada terminal Emitor." },
  { "en": "Bisakah transistor menguatkan sinyal DC?", "id": "Tidak. Transistor hanya menguatkan perubahan atau variasi sinyal (AC), bukan level DC yang statis." },
  { "en": "Bagaimana transistor menggantikan tabung vakum?", "id": "Transistor menggantikan tabung vakum karena ukurannya jauh lebih kecil, lebih andal, lebih efisien, dan tidak memerlukan pemanasan." },
  { "en": "Apa itu sirkuit terpadu atau IC (Integrated Circuit)?", "id": "IC adalah sebuah chip silikon kecil yang berisi ribuan hingga miliaran transistor dan komponen lain yang saling terhubung." },
  { "en": "Mengapa terminal Kolektor seringkali terhubung dengan bagian logam pada transistor daya?", "id": "Untuk mempermudah transfer panas dari dalam transistor ke heatsink (pendingin)." },
  { "en": "Apa itu 'stabilitas termal' pada rangkaian transistor?", "id": "Stabilitas termal adalah kemampuan rangkaian untuk mempertahankan titik operasinya tetap stabil meskipun suhu transistor berubah." },
  { "en": "Apa fungsi resistor Emitor pada rangkaian Common Emitter?", "id": "Resistor Emitor membantu meningkatkan stabilitas rangkaian terhadap perubahan suhu dan variasi gain transistor." },
  { "en": "Apakah transistor dapat beroperasi seketika?", "id": "Tidak, ada penundaan waktu yang sangat singkat (waktu switching) antara saat sinyal diberikan hingga transistor merespons." },
  { "en": "Mengapa kecepatan switching transistor itu penting?", "id": "Kecepatan switching sangat penting dalam aplikasi digital dan frekuensi tinggi, seperti pada prosesor komputer dan sistem komunikasi." },
  { "en": "Apa itu JFET?", "id": "JFET (Junction Field-Effect Transistor) adalah jenis FET yang menggunakan sambungan PN dengan bias mundur untuk mengontrol aliran arus." },
  { "en": "Apa perbedaan utama antara JFET dan MOSFET?", "id": "Perbedaan utama terletak pada terminal Gate-nya; Gate pada MOSFET terisolasi secara elektrik, sedangkan pada JFET tidak." },
  { "en": "Apakah transistor dapat digunakan untuk mengatur tegangan?", "id": "Ya, dengan konfigurasi sirkuit tertentu, transistor dapat digunakan sebagai bagian dari regulator tegangan untuk menghasilkan output tegangan yang stabil." },
  { "en": "Apa yang dimaksud 'channel' pada FET?", "id": "Channel adalah jalur konduktif di antara terminal Source dan Drain yang aliran arusnya dikontrol oleh Gate." },
  { "en": "Apa itu 'frekuensi transisi' (fT) pada transistor?", "id": "fT adalah frekuensi teoretis di mana penguatan arus transistor turun menjadi satu, menunjukkan batas kecepatan operasinya." },
  { "en": "Apa yang dimaksud 'noise' dalam konteks penguat?", "id": "Noise adalah sinyal listrik acak yang tidak diinginkan yang bisa ditambahkan oleh transistor itu sendiri ke sinyal yang sedang dikuatkan." },
  { "en": "Apakah semua transistor memiliki tingkat noise yang sama?", "id": "Tidak, ada transistor 'low-noise' yang dirancang khusus untuk aplikasi audio atau frekuensi radio yang sensitif." },
  { "en": "Apa peran transistor dalam catu daya (power supply)?", "id": "Dalam catu daya mode-saklar (SMPS), transistor bertindak sebagai saklar berkecepatan tinggi untuk meregulasi daya secara efisien." },
  { "en": "Bagaimana cara memeriksa apakah sebuah BJT masih baik menggunakan multimeter?", "id": "Dengan mengujinya seperti dua dioda yang terhubung (Basis-Emitor dan Basis-Kolektor) menggunakan mode tes dioda pada multimeter." },
  { "en": "Apa itu 'elektrostatik' dan mengapa berbahaya bagi beberapa transistor?", "id": "Elektrostatik (ESD) adalah listrik statis yang dapat merusak komponen sensitif seperti MOSFET jika tidak ditangani dengan benar." },
  { "en": "Mengapa Gate pada MOSFET sangat rentan terhadap ESD?", "id": "Karena lapisan oksidanya yang sangat tipis dapat dengan mudah tertembus dan rusak oleh lonjakan tegangan statis yang tinggi." },
  { "en": "Apa itu 'osiloskop' dan bagaimana hubungannya dengan transistor?", "id": "Osiloskop adalah alat ukur untuk melihat bentuk gelombang sinyal. Alat ini sangat berguna untuk menganalisis kinerja rangkaian penguat transistor." },
  { "en": "Dapatkah transistor dibuat dari bahan selain silikon?", "id": "Ya, bahan lain seperti Germanium (dulu) atau Gallium Arsenide (untuk aplikasi frekuensi sangat tinggi) juga dapat digunakan." },
  { "en": "Apa itu 'wafer' silikon?", "id": "Wafer adalah lempengan tipis dari kristal silikon murni yang digunakan sebagai dasar untuk membuat ratusan atau ribuan chip sirkuit terpadu (IC)." },
  { "en": "Secara fisik, bagian mana dari BJT yang dopingnya paling pekat?", "id": "Terminal Emitor memiliki tingkat doping yang paling tinggi untuk dapat menyuntikkan banyak pembawa muatan ke Basis." },
  { "en": "Apakah transistor memiliki polaritas?", "id": "Ya, terminal-terminal transistor (seperti Emitor, Basis, Kolektor) memiliki fungsi spesifik dan tidak dapat dipasang terbalik dalam sirkuit." },
  { "en": "Dalam sirkuit apa transistor sering ditemukan?", "id": "Hampir di semua perangkat elektronik modern, mulai dari ponsel, komputer, TV, radio, hingga charger dan lampu LED." },
  { "en": "Apa simbol skematik untuk transistor NPN?", "id": "Simbol transistor NPN memiliki panah pada terminal Emitor yang menunjuk ke arah luar." },
  { "en": "Apa simbol skematik untuk transistor PNP?", "id": "Simbol transistor PNP memiliki panah pada terminal Emitor yang menunjuk ke arah dalam." },
  { "en": "Apa fungsi transistor dalam rangkaian osilator?", "id": "Dalam osilator, transistor berfungsi sebagai penguat untuk mempertahankan ayunan sinyal secara terus-menerus, sehingga menghasilkan gelombang periodik." },
  { "en": "Apa yang dimaksud dengan 'umpan balik' (feedback) dalam rangkaian penguat?", "id": "Umpan balik adalah proses mengambil sebagian kecil sinyal output dan mengembalikannya ke input untuk menstabilkan atau mengubah karakteristik penguat." },
  { "en": "Jenis umpan balik apa yang digunakan untuk menstabilkan penguat?", "id": "Umpan balik negatif (negative feedback) digunakan untuk meningkatkan stabilitas, mengurangi distorsi, dan mengontrol gain penguat." },
  { "en": "Apa itu IGBT?", "id": "IGBT (Insulated Gate Bipolar Transistor) adalah transistor hibrida yang menggabungkan keunggulan input MOSFET dan kemampuan arus tinggi BJT." },
  { "en": "Di mana IGBT sering digunakan?", "id": "IGBT digunakan dalam aplikasi daya tinggi seperti penggerak motor listrik, kompor induksi, dan inverter las." },
  { "en": "Apa yang terjadi jika Basis pada BJT dibiarkan mengambang (tidak terhubung)?", "id": "Basis yang mengambang membuat kondisi transistor tidak dapat diprediksi dan rentan terhadap gangguan (noise), bisa menyala atau mati secara acak." },
  { "en": "Bagaimana transistor digunakan untuk menyalakan sebuah relay?", "id": "Transistor berfungsi sebagai saklar berarus rendah untuk mengaktifkan koil relay yang membutuhkan arus lebih besar." },
  { "en": "Apa fungsi dioda yang dipasang paralel dengan koil relay dalam rangkaian transistor?", "id": "Dioda itu (disebut flyback diode) melindungi transistor dari lonjakan tegangan induktif yang berbahaya saat relay dimatikan." },
  { "en": "Apa perbedaan utama antara MOSFET mode 'enhancement' dan 'depletion'?", "id": "MOSFET 'enhancement' secara normal dalam keadaan OFF dan perlu tegangan Gate untuk menyala, sedangkan 'depletion' secara normal ON dan perlu tegangan untuk mati." },
  { "en": "Apa jenis kemasan (package) transistor yang umum?", "id": "Jenis kemasan umum antara lain TO-92 (plastik kecil), TO-220 (untuk daya menengah dengan lubang pendingin), dan SOT-23 (tipe SMD kecil)." },
  { "en": "Bagaimana cara membaca kode pada badan transistor?", "id": "Kode pada badan transistor biasanya menunjukkan nomor part atau tipe komponen, yang dapat digunakan untuk mencari datasheet-nya." },
  { "en": "Apakah aman menyentuh kaki-kaki transistor saat sirkuit menyala?", "id": "Tidak, menyentuh kaki-kaki transistor saat sirkuit aktif bisa berbahaya karena tegangan listrik dan juga dapat merusak komponen sensitif seperti MOSFET." },
  { "en": "Mengapa transistor mengubah dunia?", "id": "Karena transistor memungkinkan miniaturisasi (pembuatan perangkat kecil), efisiensi daya, dan produksi massal sirkuit kompleks yang menjadi dasar semua elektronik modern." },
  { "en": "Apa itu 'arus saturasi'?", "id": "Arus saturasi adalah arus maksimum yang dapat mengalir melalui Kolektor-Emitor ketika transistor sepenuhnya diaktifkan (dalam mode saklar ON)." },
  { "en": "Apa peran transistor dalam rangkaian timer (pewaktu)?", "id": "Dalam rangkaian timer, transistor sering digunakan sebagai saklar yang dikontrol oleh pengisian atau pengosongan sebuah kapasitor." },
  { "en": "Apa yang terjadi jika transistor dipasang terbalik dalam sirkuit?", "id": "Jika dipasang terbalik, transistor tidak akan berfungsi sebagaimana mestinya dan berpotensi rusak secara permanen." },
  { "en": "Apa itu 'matching' transistor?", "id": "Matching adalah proses memilih dua atau lebih transistor yang memiliki karakteristik (seperti gain) yang sangat mirip, seringkali penting untuk rangkaian audio presisi." },
  { "en": "Mengapa matching transistor penting dalam penguat audio?", "id": "Untuk memastikan kedua sisi sinyal (positif dan negatif) dikuatkan secara seimbang dan menghindari distorsi, terutama pada penguat 'push-pull'." },
  { "en": "Apa itu 'kurva karakteristik' transistor?", "id": "Kurva karakteristik adalah grafik yang menunjukkan hubungan antara arus dan tegangan pada terminal-terminal transistor, membantu memahami perilakunya." },
  { "en": "Dapatkah transistor digunakan sebagai resistor variabel?", "id": "Ya, dalam daerah aktifnya, transistor (terutama FET) dapat bertindak sebagai resistor yang nilainya dikontrol oleh tegangan atau arus input." },
  { "en": "Apa yang dimaksud dengan 'sirkuit diferensial'?", "id": "Sirkuit diferensial adalah rangkaian penguat (biasanya menggunakan dua transistor) yang menguatkan perbedaan sinyal antara dua input." },
  { "en": "Di mana penguat diferensial digunakan?", "id": "Penguat diferensial adalah blok bangunan dasar dari Operational Amplifier (Op-Amp) dan penting untuk menolak noise yang sama pada kedua input." },
  { "en": "Apa itu 'common-mode noise'?", "id": "Common-mode noise adalah sinyal gangguan yang muncul secara identik pada kedua jalur input sebuah penguat diferensial." },
  { "en": "Apa keunggulan Gallium Nitride (GaN) sebagai bahan transistor?", "id": "Transistor GaN dapat beroperasi pada frekuensi dan suhu yang lebih tinggi serta lebih efisien daripada transistor silikon, ideal untuk charger cepat dan radar." },
  { "en": "Apakah transistor menghasilkan panas saat bekerja?", "id": "Ya, setiap transistor menghasilkan panas karena adanya daya yang hilang (disipasi daya) selama beroperasi, terutama pada aplikasi daya tinggi." },
  { "en": "Apa itu 'disipasi daya' (power dissipation)?", "id": "Disipasi daya adalah jumlah energi listrik yang diubah menjadi panas oleh transistor saat bekerja." },
  { "en": "Apa batasan utama dari transistor?", "id": "Setiap transistor memiliki batasan maksimum untuk tegangan, arus, dan daya yang dapat ditanganinya, yang tertera dalam datasheet." },
  { "en": "Apa itu transistor 'array'?", "id": "Transistor array adalah sebuah sirkuit terpadu (IC) yang berisi beberapa transistor individual dalam satu kemasan." },
  { "en": "Apa keuntungan menggunakan transistor array?", "id": "Menghemat ruang pada papan sirkuit (PCB) dan transistor di dalamnya biasanya memiliki karakteristik yang sangat mirip (well-matched)." },
  { "en": "Apa itu 'Miller effect'?", "id": "Miller effect adalah fenomena pada penguat transistor di mana kapasitansi antara input dan output tampak membesar, yang dapat membatasi kinerja frekuensi tinggi." },
  { "en": "Bagaimana transistor membentuk gerbang logika NAND?", "id": "Gerbang NAND dapat dibuat dengan menghubungkan dua transistor secara seri; output akan menjadi rendah hanya jika kedua input transistor tinggi." },
  { "en": "Bagaimana transistor membentuk gerbang logika NOR?", "id": "Gerbang NOR dapat dibuat dengan menghubungkan dua transistor secara paralel; output akan menjadi rendah jika salah satu atau kedua input transistor tinggi." },
  { "en": "Mengapa gerbang NAND dan NOR disebut 'gerbang universal'?", "id": "Karena kombinasi gerbang NAND atau kombinasi gerbang NOR dapat digunakan untuk membuat jenis gerbang logika lainnya (AND, OR, NOT)." },
  { "en": "Apa itu 'rangkaian push-pull'?", "id": "Rangkaian push-pull menggunakan sepasang transistor (misalnya NPN dan PNP) untuk menguatkan paruh positif dan negatif dari sinyal secara terpisah." },
  { "en": "Di mana rangkaian push-pull umum digunakan?", "id": "Rangkaian ini sangat umum digunakan pada tahap akhir (output stage) dari penguat audio untuk menggerakkan speaker secara efisien." },
  { "en": "Apa itu 'crossover distortion'?", "id": "Crossover distortion adalah cacat sinyal yang terjadi pada penguat push-pull tepat saat sinyal melintasi titik nol, jika transistor tidak di-bias dengan benar." },
  { "en": "Bagaimana cara mengatasi crossover distortion?", "id": "Dengan memberikan sedikit bias 'diam' (quiescent bias) agar kedua transistor sedikit aktif bahkan saat tidak ada sinyal, memastikan transisi yang mulus." },
  { "en": "Apa itu 'penguat kelas A'?", "id": "Penguat kelas A adalah jenis penguat di mana transistor selalu aktif (menghantarkan arus) sepanjang siklus sinyal, menghasilkan fidelitas tinggi tetapi efisiensi rendah." },
  { "en": "Apa itu 'penguat kelas B'?", "id": "Penguat kelas B menggunakan dua transistor (push-pull) di mana masing-masing hanya aktif selama setengah siklus sinyal, lebih efisien dari kelas A tetapi rentan crossover distortion." },
  { "en": "Apa itu 'penguat kelas AB'?", "id": "Penguat kelas AB adalah kompromi antara kelas A dan B, di mana transistor diberi sedikit bias untuk menghilangkan crossover distortion sambil tetap mempertahankan efisiensi yang baik." },
  { "en": "Apa itu 'penguat kelas D'?", "id": "Penguat kelas D menggunakan transistor sebagai saklar berkecepatan tinggi (teknik PWM) untuk mencapai efisiensi daya yang sangat tinggi, sering disebut penguat digital." },
  { "en": "Apa itu 'sumber arus konstan' (constant current source)?", "id": "Ini adalah rangkaian (seringkali menggunakan transistor) yang dirancang untuk menyediakan arus listrik yang stabil dan konstan terlepas dari perubahan beban atau tegangan." },
  { "en": "Di mana sumber arus konstan digunakan?", "id": "Digunakan untuk mengisi daya baterai dengan aman, menggerakkan LED dengan kecerahan seragam, dan dalam biasing rangkaian terpadu." },
  { "en": "Apa itu 'cermin arus' (current mirror)?", "id": "Cermin arus adalah sirkuit (menggunakan dua transistor) di mana arus yang mengalir melalui satu transistor secara akurat 'dicerminkan' atau disalin ke transistor kedua." },
  { "en": "Apa fungsi cermin arus dalam IC?", "id": "Sangat penting dalam sirkuit terpadu (IC) untuk menyediakan arus bias yang stabil dan cocok untuk berbagai bagian dari sirkuit." },
  { "en": "Apakah transistor bisa aus seperti komponen mekanis?", "id": "Tidak seperti saklar mekanis, transistor tidak aus karena gesekan, tetapi kinerjanya bisa menurun seiring waktu atau gagal akibat panas, tegangan berlebih, atau radiasi." },
  { "en": "Apa itu 'latching' pada rangkaian transistor?", "id": "Latching adalah kondisi di mana sebuah saklar transistor (seperti pada SCR atau Thyristor) tetap menyala bahkan setelah sinyal pemicunya dihilangkan." },
  { "en": "Apa itu SCR (Silicon Controlled Rectifier)?", "id": "SCR adalah jenis transistor empat lapis yang berfungsi seperti saklar terkunci (latching switch), sering digunakan untuk mengontrol daya AC." },
  { "en": "Bagaimana cara mematikan SCR setelah aktif?", "id": "Untuk mematikannya, arus utama yang mengalir melaluinya harus diputus atau dikurangi di bawah level 'holding current'." },
  { "en": "Apa itu TRIAC?", "id": "TRIAC mirip dengan dua SCR yang terhubung antiparalel, memungkinkannya untuk mengontrol arus bolak-balik (AC) pada kedua arah siklus." },
  { "en": "Di mana TRIAC sering digunakan?", "id": "TRIAC umum digunakan dalam rangkaian peredup lampu (dimmer) dan pengontrol kecepatan motor AC." },
  { "en": "Apa itu 'tegangan saturasi' pada BJT?", "id": "Tegangan saturasi (VCE(sat)) adalah sisa tegangan kecil antara Kolektor dan Emitor saat transistor dalam kondisi ON penuh." },
  { "en": "Mengapa tegangan saturasi yang rendah itu diinginkan?", "id": "Tegangan saturasi yang rendah berarti lebih sedikit daya yang hilang sebagai panas saat transistor bekerja sebagai saklar, sehingga lebih efisien." },
  { "en": "Apa itu 'impedansi output'?", "id": "Impedansi output adalah resistansi internal yang terlihat pada terminal output sebuah penguat, memengaruhi seberapa baik ia dapat menggerakkan beban." },
  { "en": "Mana yang memiliki impedansi output lebih rendah, Common Emitter atau Common Collector?", "id": "Konfigurasi Common Collector (Emitter Follower) memiliki impedansi output yang jauh lebih rendah, membuatnya ideal untuk menggerakkan beban." },
  { "en": "Apa itu 'efek Early'?", "id": "Efek Early adalah variasi lebar efektif lapisan basis pada BJT akibat perubahan tegangan Kolektor-Basis, yang sedikit memengaruhi karakteristik output." },
  { "en": "Dapatkah transistor digunakan untuk mendeteksi suhu?", "id": "Ya, tegangan pada sambungan PN transistor (seperti Basis-Emitor) berubah secara dapat diprediksi seiring dengan perubahan suhu, sehingga dapat digunakan sebagai sensor suhu." },
  { "en": "Apa bahan pendingin (thermal paste) yang dioleskan pada heatsink?", "id": "Thermal paste adalah bahan konduktif termal yang mengisi celah udara mikroskopis antara transistor dan heatsink untuk transfer panas yang maksimal." },
  { "en": "Mengapa perlu ada kapasitor 'bypass' atau 'decoupling' dekat transistor?", "id": "Kapasitor bypass menyediakan jalur singkat ke ground untuk noise frekuensi tinggi pada jalur catu daya, menjaga operasi transistor tetap stabil." },
  { "en": "Apa yang dimaksud dengan 'rangkaian aktif' vs 'rangkaian pasif'?", "id": "Rangkaian aktif mengandung komponen aktif seperti transistor yang dapat menguatkan sinyal, sedangkan rangkaian pasif hanya terdiri dari komponen pasif (resistor, kapasitor, induktor)." },
  { "en": "Apa itu 'frekuensi unity-gain' pada transistor?", "id": "Ini adalah frekuensi di mana gain atau penguatan sinyal dari transistor turun menjadi 1. Ini menunjukkan batas atas kemampuan penguatan frekuensi." },
  { "en": "Apa itu 'HBT' (Heterojunction Bipolar Transistor)?", "id": "HBT adalah jenis BJT berkecepatan tinggi yang menggunakan bahan semikonduktor yang berbeda untuk Emitor dan Basis, umum dalam ponsel dan sistem RF." },
  { "en": "Apa itu 'MOSFET gerbang ganda' (dual-gate MOSFET)?", "id": "Ini adalah MOSFET dengan dua terminal Gate terpisah, memungkinkan satu Gate mengontrol sinyal sementara Gate kedua mengontrol gain, berguna dalam mixer dan penguat RF." },
  { "en": "Apa tanda-tanda visual transistor yang rusak?", "id": "Tanda-tanda visual bisa berupa retakan pada kemasan, bekas terbakar atau hangus, atau tonjolan fisik pada badan komponen." },
  { "en": "Apakah transistor yang terlihat baik pasti berfungsi?", "id": "Tidak selalu. Transistor bisa saja gagal secara elektrik tanpa menunjukkan kerusakan fisik apa pun." },
  { "en": "Bagaimana cara kerja 'touch switch' sederhana menggunakan transistor?", "id": "Sentuhan jari dapat memberikan jalur resistif yang cukup bagi arus kecil untuk mengalir ke Basis transistor, yang kemudian menyalakan arus yang lebih besar untuk mengaktifkan LED atau buzzer." },
  { "en": "Apa itu 'slew rate' pada penguat?", "id": "Slew rate adalah kecepatan maksimum perubahan tegangan output penguat, yang membatasi kemampuannya untuk mereproduksi sinyal frekuensi tinggi dengan amplitudo besar." },
  { "en": "Apa itu 'kapasitansi parasit'?", "id": "Kapasitansi parasit adalah kapasitansi yang tidak diinginkan tetapi ada secara alami antara terminal-terminal transistor dan jejak sirkuit, yang dapat memengaruhi kinerja pada frekuensi tinggi." },
  { "en": "Apa beda transistor dan transformator?", "id": "Transistor adalah komponen semikonduktor untuk switching/amplifikasi, sedangkan transformator adalah komponen pasif dengan lilitan kawat untuk mengubah level tegangan/arus AC." },
  { "en": "Apa itu 'transistor digital'?", "id": "Transistor digital adalah BJT yang sudah memiliki resistor bias internal (satu atau dua) dalam satu kemasan, menyederhanakan penggunaannya sebagai saklar digital." },
  { "en": "Apa itu 'h-parameters' (parameter hibrida)?", "id": "Parameter hibrida adalah satu set parameter yang digunakan untuk memodelkan perilaku transistor sinyal kecil secara matematis untuk analisis rangkaian." },
  { "en": "Apakah transistor selalu memerlukan papan sirkuit (PCB)?", "id": "Tidak selalu. Untuk eksperimen sederhana, transistor dapat dirangkai di breadboard atau menggunakan teknik 'dead-bug' (disolder langsung antar komponen)." },
  { "en": "Mengapa kemasan transistor daya seringkali terbuat dari logam?", "id": "Bagian logam tersebut berfungsi sebagai tab Kolektor dan dirancang untuk dipasang langsung ke heatsink untuk pembuangan panas yang efisien." },
  { "en": "Apa itu 'safe operating area' (SOA)?", "id": "SOA adalah grafik dalam datasheet yang menunjukkan kombinasi arus dan tegangan di mana transistor dapat beroperasi dengan aman tanpa mengalami kerusakan." },
  { "en": "Apa itu 'doping gradien'?", "id": "Doping gradien adalah perubahan konsentrasi doping secara bertahap di dalam suatu daerah semikonduktor, yang dapat menciptakan medan listrik internal untuk mempercepat pembawa muatan." },
  { "en": "Apa itu 'drift' dan 'diffusion' pada semikonduktor?", "id": "Drift adalah gerakan pembawa muatan karena pengaruh medan listrik, sedangkan diffusion adalah gerakan pembawa muatan dari area konsentrasi tinggi ke rendah." },
  { "en": "Dapatkah transistor digunakan dalam kondisi vakum atau luar angkasa?", "id": "Ya, tetapi harus dipilih jenis 'radiation-hardened' yang dirancang khusus untuk tahan terhadap efek radiasi kosmik yang dapat merusaknya." },
  { "en": "Apa itu 'beta droop'?", "id": "Beta droop adalah fenomena penurunan nilai gain arus (β) pada BJT saat beroperasi pada arus Kolektor yang sangat tinggi." },
  { "en": "Mengapa kita tidak bisa membuat transistor PNP dan NPN raksasa dengan menyatukan tiga balok semikonduktor?", "id": "Karena fungsi transistor bergantung pada sambungan (junction) yang terbentuk pada tingkat atomik dalam satu kristal tunggal, bukan sekadar kontak fisik." },
  { "en": "Apa itu 'diode-connected transistor'?", "id": "Ini adalah konfigurasi di mana Basis dan Kolektor dari BJT dihubungkan bersama, membuatnya berfungsi seperti sebuah dioda." },
  { "en": "Apa fungsi konfigurasi diode-connected transistor?", "id": "Sering digunakan dalam sirkuit terpadu (IC) untuk membuat dioda yang karakteristiknya cocok dengan transistor lain di dalam chip yang sama." },
  { "en": "Apa itu 'avalanche breakdown'?", "id": "Avalanche breakdown adalah mekanisme kerusakan di mana medan listrik yang kuat mempercepat pembawa muatan sehingga menabrak atom lain dan melepaskan lebih banyak pembawa muatan, menciptakan 'longsoran' arus." },
  { "en": "Apa itu 'Zener breakdown'?", "id": "Zener breakdown adalah mekanisme kerusakan lain yang terjadi pada tegangan lebih rendah di semikonduktor dengan doping sangat tinggi, di mana medan listrik cukup kuat untuk menarik elektron langsung dari ikatannya." },
  { "en": "Apakah transistor sensitif terhadap cahaya meskipun bukan fototransistor?", "id": "Ya, jika chip semikonduktor di dalam kemasan terpapar cahaya yang cukup kuat, hal itu dapat menghasilkan arus bocor dan memengaruhi operasi normal." },
  { "en": "Apa itu 'finFET'?", "id": "FinFET adalah jenis desain transistor 3D non-planar di mana 'channel' dibentuk seperti sirip (fin) yang dikelilingi oleh Gate, digunakan dalam prosesor modern untuk mengurangi arus bocor dan meningkatkan kinerja." },
  { "en": "Mengapa transistor modern semakin kecil?", "id": "Semakin kecil transistor, semakin banyak yang bisa dimasukkan ke dalam sebuah chip, meningkatkan kecepatan, mengurangi konsumsi daya, dan menurunkan biaya per fungsi." },
  { "en": "Apa itu 'wafer-level testing'?", "id": "Ini adalah proses pengujian transistor dan sirkuit saat masih menjadi bagian dari wafer silikon utuh, sebelum dipotong menjadi chip individual." },
  { "en": "Apa itu 'shot noise' pada transistor?", "id": "Shot noise adalah noise elektronik yang timbul karena sifat diskrit dari muatan listrik (elektron), terjadi saat arus melewati sebuah sambungan (junction)." },
  { "en": "Apa itu 'thermal noise' pada transistor?", "id": "Thermal noise (Johnson-Nyquist noise) adalah noise yang dihasilkan oleh agitasi termal acak dari pembawa muatan di dalam material konduktif, seperti di dalam transistor." },
  { "en": "Apa itu 'saturasi lunak' (soft saturation)?", "id": "Ini adalah kondisi pada beberapa transistor di mana transisi dari daerah aktif ke daerah saturasi tidak tajam, melainkan terjadi secara bertahap." },
  { "en": "Mengapa terminal Emitor pada BJT memiliki doping yang sangat tinggi?", "id": "Agar Emitor sangat efisien dalam menyuntikkan atau 'memancarkan' pembawa muatan mayoritasnya ke dalam Basis." },
  { "en": "Apa itu 'base-width modulation'?", "id": "Ini adalah nama lain untuk 'Efek Early', di mana lebar efektif dari daerah Basis berubah seiring dengan perubahan tegangan Kolektor." },
  { "en": "Apa yang dimaksud dengan 'transkonduktansi' pada FET?", "id": "Transkonduktansi adalah parameter yang menunjukkan seberapa efektif tegangan Gate mengontrol arus Drain, mirip dengan konsep gain pada BJT." },
  { "en": "Bagaimana transistor membantu dalam konversi sinyal analog ke digital (ADC)?", "id": "Transistor membentuk sirkuit komparator, yang merupakan inti dari ADC, untuk membandingkan tegangan analog input dengan level tegangan referensi." },
  { "en": "Bagaimana transistor membantu dalam konversi sinyal digital ke analog (DAC)?", "id": "Dalam DAC, transistor digunakan sebagai saklar presisi untuk menggabungkan arus atau tegangan dari sumber referensi sesuai dengan bit-bit input digital." },
  { "en": "Mengapa terminal pada BJT dinamai Emitor, Basis, dan Kolektor?", "id": "Karena Emitor 'memancarkan' (emits) pembawa muatan, Basis menjadi 'dasar' (base) kontrol, dan Kolektor 'mengumpulkan' (collects) pembawa muatan tersebut." },
  { "en": "Mengapa terminal pada FET dinamai Source, Gate, dan Drain?", "id": "Karena Source adalah 'sumber' (source) pembawa muatan, Gate adalah 'gerbang' (gate) kontrol, dan Drain adalah tempat pembawa muatan 'dialirkan keluar' (drain)." },
  { "en": "Apa yang dimaksud dengan 'keluarga logika' (logic family)?", "id": "Keluarga logika adalah sekelompok gerbang logika digital yang dibuat menggunakan teknologi transistor yang sama, contohnya TTL (Transistor-Transistor Logic) dan CMOS (Complementary MOS)." },
  { "en": "Apa teknologi transistor yang digunakan pada prosesor modern?", "id": "Prosesor modern hampir secara eksklusif menggunakan teknologi CMOS (Complementary Metal-Oxide-Semiconductor) karena efisiensi dayanya yang sangat tinggi." },
  { "en": "Apa keunggulan utama teknologi CMOS?", "id": "Keunggulan utamanya adalah konsumsi daya statis yang sangat rendah, di mana ia hanya menggunakan daya signifikan saat beralih kondisi (switching)." },
  { "en": "Apa itu 'rangkaian totem-pole'?", "id": "Rangkaian output totem-pole adalah konfigurasi push-pull yang umum digunakan dalam gerbang logika TTL untuk memberikan output HIGH dan LOW yang kuat." },
  { "en": "Dapatkah transistor berfungsi sebagai dioda?", "id": "Ya, jika terminal Basis dan Kolektor pada BJT dihubungkan bersama, komponen tersebut akan berfungsi seperti sebuah dioda." },
  { "en": "Apa yang terjadi pada penguat jika kapasitor bypass Emitor dihilangkan?", "id": "Penguatan tegangan akan menurun drastis, tetapi stabilitas rangkaian terhadap perubahan suhu akan meningkat." },
  { "en": "Jika sinyal output penguat audio terdengar 'tipis' atau kurang bass, apa kemungkinan penyebabnya?", "id": "Kapasitor kopling (input atau output) kemungkinan memiliki nilai yang terlalu kecil, sehingga ia memblokir sinyal frekuensi rendah (bass)." },
  { "en": "Apa itu 'ringing' pada sinyal output transistor?", "id": "'Ringing' adalah osilasi yang tidak diinginkan yang muncul sesaat setelah transistor beralih status (ON atau OFF), mirip seperti getaran bel setelah dipukul." },
  { "en": "Apa fungsi dari 'snubber circuit'?", "id": "Snubber circuit, biasanya terdiri dari resistor dan kapasitor, berfungsi untuk meredam lonjakan tegangan dan 'ringing' saat transistor menyalakan atau mematikan beban induktif." },
  { "en": "Apa itu 'beban induktif'?", "id": "Beban induktif adalah beban yang memiliki lilitan kawat (kumparan) seperti motor, relay, atau solenoida, yang cenderung menahan perubahan arus." },
  { "en": "Mengapa mengalihkan beban induktif itu sulit bagi transistor?", "id": "Karena saat arus dihentikan tiba-tiba, lilitan kawat akan menghasilkan lonjakan tegangan balik yang sangat tinggi (inductive kickback) yang dapat merusak transistor." },
  { "en": "Apa itu 'charge storage effect' pada BJT?", "id": "Ini adalah fenomena di mana perlu waktu sesaat untuk membersihkan pembawa muatan dari daerah Basis saat mematikan transistor, yang membatasi kecepatan switching-nya." },
  { "en": "Mana yang umumnya lebih cepat dalam beralih (switching), BJT atau MOSFET?", "id": "MOSFET umumnya memiliki kecepatan switching yang lebih tinggi daripada BJT standar karena ia tidak mengalami 'charge storage effect'." },
  { "en": "Apa itu 'ground loop' dan bagaimana hubungannya dengan transistor?", "id": "Ground loop adalah jalur arus yang tidak diinginkan yang terbentuk melalui koneksi ground, dapat memasukkan noise ke input transistor yang sensitif dan mengganggu operasinya." },
  { "en": "Apa fungsi ferit (ferrite bead) yang kadang dipasang pada kaki transistor?", "id": "Ferit berfungsi sebagai filter untuk menekan noise frekuensi sangat tinggi yang mungkin masuk atau keluar melalui terminal transistor." },
  { "en": "Dapatkah transistor digunakan untuk menghasilkan cahaya?", "id": "Tidak secara langsung, tetapi transistor adalah komponen kunci untuk menyalakan dan mengontrol komponen penghasil cahaya seperti LED (Light Emitting Diode)." },
  { "en": "Apa itu 'active load'?", "id": "Active load adalah penggunaan transistor (atau cermin arus) sebagai pengganti resistor beban, umum di IC untuk mendapatkan gain tinggi dan menghemat ruang." },
  { "en": "Mengapa menggunakan 'active load' lebih baik daripada resistor di dalam IC?", "id": "Karena transistor dapat memberikan resistansi dinamis yang sangat tinggi dalam area chip yang jauh lebih kecil daripada resistor fisik dengan nilai yang sama." },
  { "en": "Apa itu 'slew-rate'?", "id": "Slew-rate adalah batasan kecepatan perubahan tegangan output dari sebuah penguat. Nilai yang rendah dapat menyebabkan distorsi pada sinyal frekuensi tinggi." },
  { "en": "Apa itu 'pemodelan sinyal kecil' (small-signal modeling)?", "id": "Ini adalah teknik analisis di mana perilaku non-linier transistor dianggap linier untuk sinyal AC yang kecil di sekitar titik bias DC-nya." },
  { "en": "Apa itu 'model hybrid-pi'?", "id": "Model hybrid-pi adalah model rangkaian sinyal kecil yang umum digunakan untuk menganalisis kinerja frekuensi tinggi dari transistor BJT dan FET." },
  { "en": "Bagaimana transistor dapat gagal selain karena panas atau tegangan berlebih?", "id": "Transistor dapat gagal karena 'electromigration', di mana aliran elektron secara fisik mengikis jalur logam tipis di dalam chip seiring waktu." },
  { "en": "Apa itu 'ESD' (Electrostatic Discharge)?", "id": "ESD adalah pelepasan muatan listrik statis secara tiba-tiba, seperti sengatan kecil saat menyentuh gagang pintu, yang dapat merusak komponen sensitif seperti MOSFET." },
  { "en": "Mengapa MOSFET sangat rentan terhadap ESD?", "id": "Karena lapisan isolator Gate-nya sangat tipis dan dapat dengan mudah ditembus atau dirusak oleh tegangan tinggi dari pelepasan muatan statis." },
  { "en": "Bagaimana cara melindungi transistor dari ESD?", "id": "Dengan menggunakan gelang antistatis, bekerja di atas alas antistatis, dan menyimpan komponen dalam kemasan pelindung khusus." },
  { "en": "Apa peran transistor dalam rangkaian 'sample and hold'?", "id": "Transistor (biasanya FET) digunakan sebagai saklar berkecepatan tinggi untuk 'mencuplik' (sample) tegangan sinyal sesaat dan menyimpannya di kapasitor." },
  { "en": "Apa itu 'efek longsoran' (avalanche effect)?", "id": "Ini adalah kondisi breakdown di mana tegangan yang sangat tinggi menyebabkan elektron menabrak atom lain dan melepaskan lebih banyak elektron, menciptakan arus besar yang merusak." },
  { "en": "Apa perbedaan antara breakdown 'avalanche' dan 'Zener'?", "id": "Avalanche terjadi karena tumbukan, sementara Zener terjadi karena medan listrik yang sangat kuat langsung menarik elektron keluar dari ikatannya pada tegangan yang lebih rendah." },
  { "en": "Apa itu 'transistor terkopel langsung' (direct-coupled)?", "id": "Ini adalah rangkaian di mana output dari satu tahap transistor terhubung langsung ke input tahap berikutnya tanpa menggunakan kapasitor kopling." },
  { "en": "Apa keuntungan dari rangkaian terkopel langsung?", "id": "Keuntungannya adalah dapat menguatkan sinyal frekuensi sangat rendah, bahkan perubahan DC, yang tidak bisa dilakukan jika menggunakan kapasitor." },
  { "en": "Apa kerugian dari rangkaian terkopel langsung?", "id": "Kerugiannya adalah setiap pergeseran titik DC pada tahap pertama akan ikut dikuatkan oleh tahap berikutnya, membuatnya kurang stabil." },
  { "en": "Apa itu 'transistor efek medan sambungan' (JFET)?", "id": "JFET (Junction Field-Effect Transistor) adalah jenis FET yang paling sederhana di mana 'gerbang' (gate) adalah sambungan PN dengan bias terbalik." },
  { "en": "Apa itu 'tegangan pinch-off' pada JFET?", "id": "Tegangan pinch-off adalah tegangan Gate yang menyebabkan 'channel' konduksi antara Source dan Drain tertutup sepenuhnya, menghentikan aliran arus." },
  { "en": "Bagaimana transistor digunakan untuk mengontrol kecerahan LED?", "id": "Dengan menggunakan sinyal PWM (Pulse Width Modulation) ke Basis atau Gate, transistor dapat dinyalakan dan dimatikan dengan sangat cepat untuk mengatur kecerahan rata-rata LED." },
  { "en": "Apa itu 'tegangan saturasi basis-emitor'?", "id": "Ini adalah tegangan pada sambungan Basis-Emitor ketika transistor berada dalam kondisi saturasi penuh." },
  { "en": "Apakah transistor bisa berosilasi sendiri secara tidak sengaja?", "id": "Ya, dalam kondisi tertentu (terutama pada frekuensi tinggi), kapasitansi dan induktansi parasit dalam rangkaian dapat membentuk osilator dan membuat penguat menjadi tidak stabil." },
  { "en": "Apa itu 'kompensasi frekuensi' pada penguat?", "id": "Kompensasi frekuensi adalah teknik menambahkan komponen (biasanya kapasitor) untuk sengaja mengurangi gain pada frekuensi tinggi guna mencegah osilasi yang tidak diinginkan." },
  { "en": "Apa itu 'kapasitansi sambungan' (junction capacitance)?", "id": "Setiap sambungan PN dalam transistor memiliki kapasitansi alami yang dapat memengaruhi seberapa cepat transistor dapat merespons perubahan sinyal." },
  { "en": "Apa itu 'waktu transit' (transit time) pada BJT?", "id": "Waktu transit adalah waktu rata-rata yang dibutuhkan pembawa muatan untuk bergerak dari Emitor ke Kolektor melintasi daerah Basis." },
  { "en": "Bagaimana waktu transit memengaruhi kinerja transistor?", "id": "Waktu transit yang lebih singkat berarti transistor dapat beroperasi pada frekuensi yang lebih tinggi." },
  { "en": "Apa itu 'resistansi saturasi' (RDS(on)) pada MOSFET?", "id": "RDS(on) adalah resistansi dari 'channel' antara Drain dan Source ketika MOSFET dalam keadaan ON penuh. Nilai yang rendah berarti efisiensi tinggi." },
  { "en": "Mengapa RDS(on) yang rendah sangat penting untuk aplikasi saklar?", "id": "Karena resistansi yang lebih rendah berarti lebih sedikit daya yang hilang sebagai panas saat arus besar mengalir melalui MOSFET." },
  { "en": "Apa itu 'body effect' pada MOSFET?", "id": "Body effect adalah perubahan tegangan ambang (threshold voltage) yang disebabkan oleh perubahan tegangan antara terminal Source dan badan (body/substrate) MOSFET." },
  { "en": "Bagaimana cara menguji transistor BJT dengan multimeter analog?", "id": "Dengan menggunakan mode ohmmeter, sambungan Basis-Emitor dan Basis-Kolektor harus menunjukkan perilaku seperti dioda (konduksi satu arah)." },
  { "en": "Apa yang dimaksud dengan 'hot-swappable'?", "id": "Hot-swappable berarti suatu komponen atau kartu dapat dipasang atau dilepas saat sirkuit masih menyala. Sirkuit transistor khusus diperlukan untuk memungkinkan hal ini dengan aman." },
  { "en": "Apa itu 'crowbar circuit'?", "id": "Crowbar circuit adalah rangkaian proteksi yang menggunakan komponen seperti SCR untuk membuat hubungan pendek (short circuit) pada catu daya jika terjadi tegangan berlebih, meledakkan sekring dan melindungi beban." },
  { "en": "Apa peran transistor dalam 'crowbar circuit'?", "id": "Transistor dapat digunakan untuk mendeteksi kondisi tegangan berlebih dan kemudian memicu SCR untuk 'beraksi'." },
  { "en": "Mengapa ada begitu banyak jenis transistor yang berbeda?", "id": "Karena setiap aplikasi memiliki kebutuhan yang berbeda, seperti kecepatan tinggi, daya tinggi, noise rendah, atau efisiensi tinggi, sehingga transistor dioptimalkan untuk tugas-tugas spesifik." },
  { "en": "Apa itu 'efek terowongan' (tunneling) pada transistor?", "id": "Pada transistor yang sangat kecil, elektron kadang-kadang dapat 'menembus' lapisan isolator yang tipis meskipun secara klasik tidak memiliki cukup energi, menyebabkan arus bocor." },
  { "en": "Apa itu 'single-electron transistor' (SET)?", "id": "SET adalah perangkat eksperimental yang sangat kecil yang beroperasi dengan mengontrol aliran elektron satu per satu, menjanjikan komputasi berdaya sangat rendah." },
  { "en": "Dapatkah transistor dibuat dari bahan organik?", "id": "Ya, OFET (Organic Field-Effect Transistor) menggunakan molekul organik sebagai semikonduktor, berpotensi untuk elektronik yang fleksibel dan dapat dicetak." },
  { "en": "Apa keunggulan potensial dari elektronik organik?", "id": "Biaya produksi yang rendah, transparansi, dan kemampuan untuk dibuat pada substrat yang fleksibel seperti plastik." },
  { "en": "Apa tantangan dalam membuat transistor yang lebih kecil?", "id": "Tantangannya meliputi mengontrol arus bocor, membuang panas dari area yang padat, dan efek kuantum yang menjadi lebih dominan." },
  { "en": "Apa itu 'gain-bandwidth product' (GBWP)?", "id": "GBWP adalah parameter yang menunjukkan trade-off pada penguat: jika Anda meningkatkan gain (penguatan), bandwidth (rentang frekuensi) yang dapat dioperasikan akan berkurang, dan sebaliknya." },
  { "en": "Apa itu 'rangkaian cascode'?", "id": "Rangkaian cascode menggunakan dua transistor (misalnya Common Emitter diikuti Common Base) untuk mendapatkan bandwidth yang lebih lebar dan isolasi input-output yang lebih baik daripada satu transistor tunggal." },
  { "en": "Apa keuntungan dari rangkaian cascode?", "id": "Sangat baik untuk penguat frekuensi tinggi karena mengurangi dampak dari 'Miller effect'." },
  { "en": "Apa itu 'penguat logaritmik'?", "id": "Ini adalah rangkaian penguat khusus di mana tegangan output sebanding dengan logaritma dari tegangan input, seringkali memanfaatkan karakteristik eksponensial dari sambungan PN transistor." },
  { "en": "Apa itu 'penguat transimpedansi'?", "id": "Penguat transimpedansi mengubah sinyal input berupa arus menjadi sinyal output berupa tegangan, sangat penting untuk menguatkan sinyal dari fotodioda." },
  { "en": "Mengapa transistor lebih disukai daripada relay untuk switching cepat?", "id": "Karena transistor tidak memiliki bagian yang bergerak, ia dapat beralih jutaan kali lebih cepat daripada relay mekanis dan tidak akan aus." },
  { "en": "Apa itu 'debouncing'?", "id": "Debouncing adalah proses menghilangkan sinyal palsu yang terjadi saat kontak saklar mekanis 'memantul' saat ditutup. Rangkaian transistor dapat digunakan untuk ini." },
  { "en": "Apa itu 'Schmitt trigger'?", "id": "Schmitt trigger adalah rangkaian komparator dengan histeresis, menggunakan transistor untuk mengubah sinyal analog yang berisik menjadi sinyal digital yang bersih dan bebas 'getaran'." },
  { "en": "Apa yang dimaksud dengan 'histeresis'?", "id": "Histeresis berarti titik ambang untuk beralih ke ON berbeda dari titik ambang untuk beralih ke OFF, ini membantu mencegah output 'bergetar' ketika input berada di dekat ambang batas." },
  { "en": "Apa itu 'bistable multivibrator'?", "id": "Ini adalah nama lain untuk 'flip-flop', sebuah rangkaian (menggunakan dua transistor) yang memiliki dua keadaan stabil dan merupakan dasar dari memori digital." },
  { "en": "Apa itu 'astable multivibrator'?", "id": "Ini adalah rangkaian osilator sederhana (menggunakan dua transistor dan kapasitor) yang tidak memiliki keadaan stabil, terus-menerus beralih antara ON dan OFF, sering digunakan untuk membuat lampu berkedip." },
  { "en": "Apa itu 'monostable multivibrator'?", "id": "Ini adalah rangkaian 'one-shot' (menggunakan transistor) yang memiliki satu keadaan stabil. Ketika dipicu, ia akan beralih ke keadaan tidak stabil untuk periode waktu tertentu sebelum kembali." },
  { "en": "Bagaimana transistor digunakan dalam catu daya linier?", "id": "Transistor (biasanya dalam konfigurasi Emitter Follower) digunakan sebagai elemen pengatur variabel untuk menjaga tegangan output tetap konstan meskipun beban berubah." },
  { "en": "Apa kerugian dari catu daya linier?", "id": "Kerugian utamanya adalah efisiensinya yang rendah, karena transistor pengatur membuang kelebihan daya sebagai panas." },
  { "en": "Apa itu 'catu daya mode-saklar' (SMPS)?", "id": "SMPS menggunakan transistor sebagai saklar berkecepatan tinggi, menyalakan dan mematikannya ribuan kali per detik, untuk mentransfer daya dengan sangat efisien." },
  { "en": "Mengapa SMPS lebih efisien daripada catu daya linier?", "id": "Karena transistornya hampir selalu dalam kondisi ON penuh atau OFF penuh, di mana disipasi dayanya minimal, tidak seperti pada mode aktif linier." },
  { "en": "Apa peran induktor dalam SMPS tipe 'buck converter'?", "id": "Induktor berfungsi untuk menyimpan energi saat transistor ON dan melepaskannya saat transistor OFF, secara efektif 'meratakan' pulsa menjadi tegangan DC yang lebih rendah." },
  { "en": "Apa peran dioda dalam SMPS tipe 'buck converter'?", "id": "Dioda menyediakan jalur bagi arus induktor untuk terus mengalir saat transistor dalam keadaan OFF, mencegah lonjakan tegangan." },
  { "en": "Apa itu 'radiasi elektromagnetik' (EMI) dari transistor?", "id": "Operasi switching berkecepatan tinggi pada transistor dapat menghasilkan noise listrik yang terpancar ke udara sebagai gelombang radio, yang dapat mengganggu perangkat lain." },
  { "en": "Bagaimana cara mengurangi EMI dari rangkaian transistor?", "id": "Dengan tata letak PCB yang baik, penggunaan filter, perisai logam (shielding), dan mengontrol kecepatan switching transistor." },
  { "en": "Apa itu 'model SPICE' untuk transistor?", "id": "SPICE (Simulation Program with Integrated Circuit Emphasis) adalah program simulasi sirkuit, dan model SPICE untuk transistor adalah serangkaian parameter matematis kompleks yang mendeskripsikan perilakunya secara akurat." },
  { "en": "Mengapa simulasi sirkuit penting?", "id": "Simulasi memungkinkan perancang untuk menguji dan memverifikasi fungsi sebuah rangkaian di komputer sebelum membuatnya secara fisik, menghemat waktu dan biaya." },
  { "en": "Apa itu 'doping retrograd'?", "id": "Doping retrograd adalah teknik di mana konsentrasi doping tertinggi berada di bawah permukaan semikonduktor, digunakan dalam MOSFET canggih untuk mengontrol efek 'short-channel'." },
  { "en": "Apa itu 'efek channel pendek' (short-channel effects)?", "id": "Ini adalah serangkaian masalah yang muncul saat panjang gate MOSFET menjadi sangat pendek, seperti penurunan tegangan ambang dan peningkatan arus bocor." },
  { "en": "Apa itu 'transistor film tipis' (TFT)?", "id": "TFT (Thin-Film Transistor) adalah jenis FET yang dibuat dengan menumpuk lapisan tipis pada substrat pendukung seperti kaca." },
  { "en": "Di mana TFT digunakan secara luas?", "id": "TFT digunakan sebagai elemen saklar untuk setiap piksel dalam layar modern seperti LCD, LED, dan OLED." },
  { "en": "Bagaimana TFT mengontrol piksel di layar LCD?", "id": "Setiap piksel memiliki TFT yang berfungsi sebagai saklar untuk menyalakan atau mematikan piksel tersebut, memungkinkan gambar terbentuk." },
  { "en": "Apakah transistor selalu berupa komponen terpisah (diskrit)?", "id": "Tidak. Sebagian besar transistor di dunia saat ini berada di dalam sirkuit terpadu (IC), bukan sebagai komponen diskrit." },
  { "en": "Apa itu 'parameter-S' (Scattering parameters)?", "id": "Parameter-S digunakan untuk mengkarakterisasi perilaku transistor pada frekuensi sangat tinggi (RF/microwave), menggambarkan bagaimana sinyal dipantulkan dan ditransmisikan." },
  { "en": "Apa itu 'angka kebisingan' (noise figure)?", "id": "Angka kebisingan adalah ukuran seberapa banyak noise (gangguan) yang ditambahkan oleh sebuah penguat (atau transistor) ke sinyal. Angka yang lebih rendah lebih baik." },
  { "en": "Mengapa angka kebisingan penting untuk penerima radio?", "id": "Karena tahap pertama dari penerima radio harus dapat menguatkan sinyal antena yang sangat lemah tanpa menambahkan banyak noise, agar sinyal tidak hilang dalam gangguan." },
  { "en": "Apa itu 'titik intersep orde ketiga' (IP3)?", "id": "IP3 adalah ukuran linearitas sebuah penguat. Nilai IP3 yang tinggi menunjukkan bahwa penguat dapat menangani beberapa sinyal kuat secara bersamaan tanpa menghasilkan distorsi antar-modulasi." },
  { "en": "Apa itu 'distorsi antar-modulasi' (intermodulation distortion)?", "id": "Ini adalah jenis distorsi yang menciptakan frekuensi hantu yang tidak diinginkan saat dua atau lebih sinyal dikuatkan secara bersamaan oleh penguat non-linier." },
  { "en": "Apa itu 'pembebanan aktif' (bootstrapping)?", "id": "Bootstrapping adalah teknik umpan balik positif yang digunakan, misalnya, untuk membuat impedansi input sebuah penguat tampak jauh lebih tinggi daripada yang sebenarnya." },
  { "en": "Dapatkah transistor menahan muatan listrik seperti kapasitor?", "id": "Ya, kapasitansi internal pada sambungan PN atau struktur Gate dapat menyimpan muatan untuk waktu yang sangat singkat." },
  { "en": "Apa itu 'floating gate transistor'?", "id": "Ini adalah jenis MOSFET dengan gerbang tambahan yang terisolasi sepenuhnya, mampu 'menjebak' elektron untuk waktu yang lama." },
  { "en": "Di mana 'floating gate transistor' digunakan?", "id": "Ini adalah blok bangunan dasar dari memori non-volatile seperti EPROM, EEPROM, dan memori Flash (yang digunakan di SSD dan USB drive)." },
  { "en": "Bagaimana memori flash menyimpan data tanpa daya?", "id": "Dengan menjebak atau melepaskan muatan (elektron) di dalam floating gate transistor, yang mewakili bit '0' atau '1' dan tetap ada bahkan setelah daya dimatikan." },
  { "en": "Apa analogi sederhana untuk 'biasing' transistor?", "id": "Biasing seperti menyetel ketinggian panggung untuk seorang aktor (sinyal), agar ia bisa terlihat jelas tanpa membentur lantai (cut-off) atau langit-langit (saturasi)." },
  { "en": "Jika penguat adalah sebuah megaphone, apa itu 'gain'?", "id": "Gain adalah seberapa besar megaphone tersebut memperkuat suara Anda. Gain tinggi berarti suara menjadi jauh lebih keras." },
  { "en": "Jika transistor adalah gerbang tol, apa itu 'tegangan Gate' pada MOSFET?", "id": "Tegangan Gate adalah sinyal dari petugas yang menaikkan atau menurunkan palang gerbang untuk mengizinkan mobil (arus) lewat atau tidak." },
  { "en": "Apa perbedaan visual utama antara simbol JFET dan MOSFET?", "id": "Pada simbol MOSFET, terminal Gate digambar terpisah (terisolasi) dari 'channel', sedangkan pada JFET, panah Gate menyentuh 'channel'." },
  { "en": "Kapan kita lebih memilih menggunakan Darlington Pair daripada satu transistor?", "id": "Ketika kita membutuhkan penguatan arus yang ekstrem, di mana sinyal input sangat lemah sehingga tidak cukup untuk mengaktifkan satu transistor biasa." },
  { "en": "Sebuah penguat audio menghasilkan suara 'dengung' (humming). Apa kemungkinan penyebab terkait catu daya transistor?", "id": "Dengung sering disebabkan oleh 'ripple' atau sisa tegangan AC dari catu daya yang tidak terfilter dengan baik dan ikut terkuatkan oleh transistor." },
  { "en": "Sebuah LED yang dikontrol transistor tidak menyala. Apa langkah pertama pengecekan terkait transistor?", "id": "Periksa apakah terminal Basis (atau Gate) menerima tegangan sinyal yang benar untuk menyalakan transistor tersebut." },
  { "en": "Mengapa transistor daya bisa terasa sangat panas bahkan saat tidak ada beban (misal, speaker mati)?", "id": "Ini bisa disebabkan oleh arus 'diam' (quiescent current) yang terlalu tinggi pada rangkaian biasnya, membuatnya terus-menerus membuang banyak daya sebagai panas." },
  { "en": "Apa hubungan antara Hukum Ohm dan pemilihan resistor Basis?", "id": "Hukum Ohm membantu menentukan nilai resistor yang tepat untuk mengubah tegangan sinyal input menjadi arus Basis yang aman dan sesuai." },
  { "en": "Apa itu 'resistansi termal' (thermal resistance) pada transistor?", "id": "Ini adalah ukuran seberapa sulit panas mengalir dari inti semikonduktor ke lingkungan luar. Nilai yang lebih rendah berarti pendinginan lebih baik." },
  { "en": "Selain untuk amplifikasi, apa kegunaan lain dari daerah aktif?", "id": "Daerah aktif dapat dimanfaatkan untuk membuat komponen lain seperti sumber arus konstan atau resistor yang nilainya dapat diatur tegangan." },
  { "en": "Apa peran transistor dalam sebuah radio AM?", "id": "Transistor berfungsi untuk menguatkan sinyal radio frekuensi tinggi yang lemah dari antena, dan juga dalam proses 'demodulasi' untuk mengekstrak sinyal audio." },
  { "en": "Bagaimana transistor membantu menghemat baterai pada ponsel?", "id": "Dengan memungkinkan bagian-bagian sirkuit yang tidak digunakan untuk masuk ke mode 'tidur' (sleep mode) berdaya sangat rendah, yang dikontrol oleh transistor sebagai saklar daya." },
  { "en": "Apa itu 'resistansi input dinamis'?", "id": "Ini adalah resistansi efektif yang 'dilihat' oleh sinyal AC pada input transistor, yang bisa berbeda dari resistansi yang 'dilihat' oleh sumber tegangan DC." },
  { "en": "Mengapa penguat Kelas A memiliki fidelitas terbaik?", "id": "Karena transistornya selalu aktif dan beroperasi di bagian paling linier dari kurva karakteristiknya, sehingga mereproduksi sinyal input dengan sangat akurat." },
  { "en": "Apa kelemahan utama penguat Kelas A selain boros energi?", "id": "Karena terus menerus panas, komponennya (terutama transistor itu sendiri) bisa memiliki umur yang lebih pendek jika pendinginannya tidak memadai." },
  { "en": "Apa itu 'self-biasing' pada rangkaian transistor?", "id": "Self-biasing adalah teknik biasing (seperti menggunakan resistor emitor) yang secara otomatis menstabilkan titik kerja transistor meskipun ada perubahan suhu atau gain." },
  { "en": "Apa itu 'early voltage' dan apa pengaruhnya?", "id": "Early voltage adalah parameter yang menunjukkan ketidakidealan transistor, di mana resistansi outputnya tidak tak terhingga. Ini sedikit membatasi gain maksimum yang bisa dicapai." },
  { "en": "Bisakah transistor digunakan untuk melindungi sirkuit lain?", "id": "Ya, transistor dapat digunakan dalam sirkuit proteksi untuk mendeteksi kondisi arus berlebih atau tegangan berlebih dan kemudian memutus daya ke sirkuit yang dilindungi." },
  { "en": "Apa itu 'load line' atau garis beban?", "id": "Garis beban adalah sebuah garis pada grafik karakteristik transistor yang merepresentasikan semua kemungkinan titik operasi (kombinasi arus dan tegangan) untuk beban tertentu." },
  { "en": "Apa yang terjadi jika titik-Q (titik diam) terlalu dekat ke daerah saturasi?", "id": "Bagian atas dari sinyal output akan terpotong (clipped) karena transistor mencapai batas konduksi maksimumnya terlalu cepat." },
  { "en": "Apa yang terjadi jika titik-Q terlalu dekat ke daerah cut-off?", "id": "Bagian bawah dari sinyal output akan terpotong (clipped) karena transistor mati sebelum seluruh siklus sinyal selesai." },
  { "en": "Apa itu 'efisiensi kolektor'?", "id": "Ini adalah ukuran seberapa efisien sebuah penguat mengubah daya DC dari catu daya menjadi daya sinyal AC pada output. Penguat Kelas D memiliki efisiensi tertinggi." },
  { "en": "Mengapa silikon lebih banyak digunakan daripada germanium untuk transistor modern?", "id": "Silikon dapat beroperasi pada suhu yang lebih tinggi dan memiliki arus bocor yang jauh lebih rendah daripada germanium, membuatnya lebih stabil dan andal." },
  { "en": "Apa itu 'emitor degenerasi'?", "id": "Ini adalah istilah untuk menempatkan resistor di jalur Emitor. 'Degenerasi' di sini berarti mengurangi gain tetapi sebagai gantinya akan meningkatkan stabilitas dan linearitas." },
  { "en": "Apa itu 'penguat dua tahap' (two-stage amplifier)?", "id": "Ini adalah penguat yang terdiri dari dua tingkat transistor yang dihubungkan secara seri untuk mencapai penguatan total yang jauh lebih tinggi daripada satu tingkat saja." },
  { "en": "Bagaimana cara kerja transistor sebagai 'saklar analog'?", "id": "Sebagai saklar analog, transistor (terutama FET) dapat melewatkan sinyal analog variabel tanpa distorsi saat ON, dan memblokirnya sepenuhnya saat OFF. Ini digunakan dalam multiplexer." },
  { "en": "Apa itu 'multiplexer analog'?", "id": "Multiplexer adalah sirkuit yang menggunakan beberapa saklar transistor untuk memilih salah satu dari banyak sinyal input analog untuk diteruskan ke satu output." },
  { "en": "Dapatkah transistor menahan tegangan negatif pada Kolektor-Emitor?", "id": "Umumnya tidak. Memberikan polaritas tegangan yang salah pada terminal transistor (misalnya VCE negatif pada NPN) dapat merusaknya dengan cepat." },
  { "en": "Apa itu 'efek Kirk'?", "id": "Efek Kirk adalah fenomena pada BJT di arus tinggi yang menyebabkan perluasan efektif daerah basis, yang dapat menurunkan gain dan frekuensi transisi." },
  { "en": "Apa itu 'doping seragam' (uniform doping)?", "id": "Ini berarti konsentrasi atom pengotor didistribusikan secara merata di seluruh daerah semikonduktor." },
  { "en": "Apa itu 'figure of merit' (FOM) untuk sebuah transistor?", "id": "FOM adalah parameter atau kombinasi parameter yang digunakan untuk membandingkan kinerja transistor untuk aplikasi tertentu, contohnya adalah produk gain-bandwidth." },
  { "en": "Apa itu 'tegangan tembus balik' (reverse breakdown voltage)?", "id": "Ini adalah tegangan maksimum yang dapat ditahan oleh sambungan Basis-Emitor saat diberi bias mundur sebelum rusak." },
  { "en": "Mengapa tegangan tembus balik Basis-Emitor biasanya sangat rendah?", "id": "Karena daerah Emitor memiliki doping yang sangat tinggi, yang menciptakan sambungan PN yang tidak dapat menahan tegangan balik yang besar (biasanya hanya 5-7 Volt)." },
  { "en": "Bagaimana cara kerja 'voltage-controlled oscillator' (VCO)?", "id": "VCO menggunakan transistor dalam rangkaian osilator di mana frekuensi outputnya dapat diubah dengan mengatur tegangan DC input, seringkali menggunakan dioda varaktor." },
  { "en": "Apa itu 'phase-locked loop' (PLL)?", "id": "PLL adalah sirkuit umpan balik (menggunakan VCO, detektor fasa, dan filter) yang mengunci frekuensi osilatornya agar sama dengan frekuensi sinyal referensi. Transistor adalah inti dari komponen-komponen ini." },
  { "en": "Apa peran transistor dalam sistem injeksi bahan bakar mobil?", "id": "Transistor daya (seringkali IGBT) bertindak sebagai saklar presisi untuk membuka dan menutup injektor bahan bakar (solenoida) dengan waktu yang sangat akurat." },
  { "en": "Apa itu 'resistansi penyebaran basis' (base spreading resistance)?", "id": "Ini adalah resistansi internal yang ada di dalam material daerah Basis itu sendiri, yang dapat membatasi kinerja frekuensi tinggi." },
  { "en": "Apa perbedaan antara 'kegagalan katastropik' dan 'degradasi'?", "id": "Kegagalan katastropik berarti transistor berhenti bekerja sama sekali (mis. korsleting). Degradasi berarti kinerjanya menurun secara bertahap seiring waktu (mis. gain berkurang)." },
  { "en": "Apa itu 'hot carrier injection'?", "id": "Ini adalah mekanisme degradasi pada MOSFET di mana elektron berenergi tinggi dapat merusak lapisan isolator di dekat Drain, mengubah karakteristik transistor seiring waktu." },
  { "en": "Apakah bentuk fisik transistor menentukan fungsinya?", "id": "Tidak sepenuhnya, tetapi bentuk fisik (kemasan) seringkali memberikan petunjuk tentang kemampuannya, misalnya kemasan logam besar biasanya untuk transistor daya tinggi." },
  { "en": "Apa itu 'penguat instrumentasi'?", "id": "Ini adalah jenis penguat diferensial presisi tinggi (dibuat dari beberapa transistor/op-amp) yang dirancang untuk menguatkan sinyal kecil dari sensor dengan akurasi tinggi." },
  { "en": "Apa itu 'common mode rejection ratio' (CMRR)?", "id": "CMRR adalah ukuran kemampuan penguat diferensial untuk menolak sinyal gangguan (noise) yang muncul secara bersamaan dan sama di kedua inputnya. Nilai yang tinggi lebih baik." },
  { "en": "Bagaimana transistor digunakan dalam logika 'wired-AND'?", "id": "Dengan menghubungkan beberapa output transistor tipe 'open-collector' bersama, hasilnya akan menjadi HIGH hanya jika SEMUA output transistor dalam keadaan HIGH (atau lebih tepatnya, tidak menarik ke LOW)." },
  { "en": "Apa itu output 'open-collector' atau 'open-drain'?", "id": "Ini adalah jenis output transistor di mana terminal Kolektor (atau Drain) tidak terhubung ke resistor internal, memungkinkannya untuk dihubungkan bersama dengan output lain." },
  { "en": "Apa itu 'resistor pull-up'?", "id": "Resistor pull-up adalah resistor yang terhubung antara output open-collector/drain dan sumber tegangan positif (Vcc) untuk memastikan level logika HIGH yang pasti saat transistor OFF." },
  { "en": "Dapatkah transistor memblokir tegangan AC?", "id": "Satu transistor saja tidak bisa memblokir tegangan AC sepenuhnya. Untuk itu, biasanya digunakan dua transistor (seperti dalam 'transmission gate' CMOS) atau TRIAC." },
  { "en": "Apa itu 'CMOS transmission gate'?", "id": "Ini adalah saklar analog yang sangat efisien yang dibuat dengan menghubungkan MOSFET P-channel dan N-channel secara paralel, mampu melewatkan sinyal dari ground hingga Vcc." },
  { "en": "Apa itu 'current hogging' pada transistor paralel?", "id": "Ketika beberapa transistor dihubungkan paralel, salah satu transistor mungkin 'mencuri' lebih banyak arus karena perbedaan karakteristik, menjadi lebih panas, dan mencuri lebih banyak arus lagi hingga rusak." },
  { "en": "Bagaimana cara mencegah 'current hogging'?", "id": "Dengan menambahkan resistor kecil (resistor balast) secara seri dengan emitor setiap transistor untuk membantu menyeimbangkan pembagian arus." },
  { "en": "Apa itu 'efek saturasi kuasi'?", "id": "Ini adalah kondisi operasi antara daerah aktif dan saturasi keras, di mana transistor mulai menunjukkan beberapa karakteristik saturasi tetapi belum sepenuhnya jenuh." },
  { "en": "Mengapa transistor kadang-kadang disebut 'perangkat transfer-resistor'?", "id": "Nama 'transistor' berasal dari 'transfer-resistor' karena ia mentransfer sinyal dari sirkuit beresistansi rendah (input) ke sirkuit beresistansi tinggi (output), yang menghasilkan penguatan." },
  { "en": "Apa itu 'waktu tunda propagasi' (propagation delay) pada gerbang logika?", "id": "Ini adalah waktu singkat yang dibutuhkan output gerbang logika untuk merespons perubahan pada inputnya, ditentukan oleh kecepatan switching transistor di dalamnya." },
  { "en": "Apa itu 'fan-out' pada gerbang logika?", "id": "Fan-out adalah jumlah maksimum input gerbang logika lain yang dapat digerakkan oleh satu output gerbang logika tanpa menurunkan kinerjanya." },
  { "en": "Apa yang membatasi 'fan-out'?", "id": "Kemampuan transistor output untuk menyediakan (source) atau menyerap (sink) arus yang cukup untuk semua input yang terhubung." },
  { "en": "Apa itu 'level logika' (logic level)?", "id": "Level logika adalah rentang tegangan yang didefinisikan untuk merepresentasikan nilai biner '0' (LOW) dan '1' (HIGH) dalam sistem digital." },
  { "en": "Apa itu 'noise margin' (batas kebisingan)?", "id": "Noise margin adalah 'zona aman' tegangan di mana sinyal gangguan (noise) tidak akan secara keliru mengubah logika '0' menjadi '1' atau sebaliknya." },
  { "en": "Bagaimana transistor berkontribusi pada 'noise margin'?", "id": "Desain transistor dan ambang batas switchingnya pada gerbang logika menentukan level logika dan seberapa besar noise margin yang dimiliki sirkuit." },
  { "en": "Apa itu 'dioda Schottky' dan bagaimana hubungannya dengan transistor?", "id": "Dioda Schottky adalah dioda berkecepatan tinggi. Dalam logika TTL, dioda ini ditempatkan di antara Basis dan Kolektor untuk mencegah transistor masuk ke saturasi dalam, mempercepat waktu switching." },
  { "en": "Apa itu 'Schottky Transistor Logic' (STL)?", "id": "Ini adalah varian dari keluarga logika TTL yang menggunakan transistor Schottky untuk mencapai kecepatan yang jauh lebih tinggi." },
  { "en": "Apa itu 'bandgap reference'?", "id": "Ini adalah sirkuit presisi tinggi (menggunakan transistor) yang menghasilkan tegangan referensi yang sangat stabil dan tidak terpengaruh oleh perubahan suhu atau tegangan catu daya." },
  { "en": "Di mana 'bandgap reference' digunakan?", "id": "Digunakan di hampir semua IC kompleks, seperti konverter analog-ke-digital dan regulator tegangan, sebagai titik referensi internal yang andal." },
  { "en": "Apa itu 'recombination' (rekombinasi) dalam semikonduktor?", "id": "Rekombinasi adalah proses di mana sebuah elektron bebas bertemu dengan sebuah 'hole' dan keduanya saling meniadakan, melepaskan energi (seringkali sebagai panas)." },
  { "en": "Di mana rekombinasi terjadi di dalam BJT?", "id": "Sebagian kecil rekombinasi terjadi di daerah Basis, yang menjadi penyebab adanya arus Basis (IB). Sebagian besar pembawa muatan berhasil melintas ke Kolektor." },
  { "en": "Apa itu 'generasi pembawa muatan' (carrier generation)?", "id": "Ini adalah proses terciptanya pasangan elektron-hole, biasanya karena energi termal atau paparan cahaya." },
  { "en": "Bagaimana 'generasi pembawa muatan' berhubungan dengan arus bocor?", "id": "Generasi termal adalah penyebab utama arus bocor (leakage current) pada transistor yang dalam keadaan OFF." },
  { "en": "Apa itu 'epitaxy' dalam pembuatan transistor?", "id": "Epitaxy adalah proses menumbuhkan lapisan kristal tunggal di atas substrat kristal lain, digunakan untuk membuat lapisan semikonduktor dengan doping dan ketebalan yang sangat terkontrol." },
  { "en": "Apa itu 'ion implantation' (implantasi ion)?", "id": "Ini adalah teknik doping di mana atom pengotor dipercepat dan 'ditembakkan' ke dalam wafer silikon untuk menempatkannya pada kedalaman dan konsentrasi yang tepat." },
  { "en": "Apa itu 'annealing' (anil)?", "id": "Setelah implantasi ion, annealing adalah proses pemanasan wafer untuk memperbaiki kerusakan kristal yang disebabkan oleh penembakan ion dan untuk 'mengaktifkan' atom dopan." },
  { "en": "Apa itu 'polysilicon'?", "id": "Polysilicon (silikon polikristalin) adalah bentuk silikon yang banyak digunakan untuk membuat terminal Gate pada MOSFET modern." },
  { "en": "Apa itu 'silicon on insulator' (SOI)?", "id": "SOI adalah teknologi manufaktur di mana lapisan tipis silikon ditempatkan di atas lapisan isolator (seperti oksida silikon) untuk mengurangi kapasitansi parasit dan meningkatkan kinerja." },
  { "en": "Apa keuntungan teknologi SOI?", "id": "Keuntungannya termasuk kecepatan yang lebih tinggi, konsumsi daya yang lebih rendah, dan ketahanan yang lebih baik terhadap radiasi." },
  { "en": "Apa itu 'gate-source voltage' (VGS)?", "id": "VGS adalah tegangan yang diterapkan antara terminal Gate dan Source pada sebuah FET, yang berfungsi untuk mengontrol aliran arus." },
  { "en": "Apa itu 'drain-source voltage' (VDS)?", "id": "VDS adalah tegangan antara terminal Drain dan Source pada sebuah FET." },
  { "en": "Apa itu 'triode region' atau 'linear region' pada MOSFET?", "id": "Ini adalah daerah operasi di mana MOSFET bertindak seperti resistor yang nilainya dikontrol oleh tegangan Gate (VGS)." },
  { "en": "Kapan MOSFET digunakan di 'triode region'?", "id": "Daerah ini digunakan saat MOSFET berfungsi sebagai saklar analog atau sebagai resistor variabel yang dikontrol tegangan." },
  { "en": "Apa itu 'channel length modulation'?", "id": "Ini adalah fenomena pada MOSFET yang mirip dengan efek Early pada BJT, di mana panjang efektif channel sedikit berkurang dengan meningkatnya VDS, menyebabkan resistansi output yang tidak tak terhingga." },
  { "en": "Apa itu 'subthreshold conduction'?", "id": "Ini adalah arus bocor kecil yang mengalir melalui MOSFET bahkan ketika tegangan Gate berada di bawah tegangan ambang (threshold), menjadi masalah signifikan pada IC berdaya rendah." },
  { "en": "Apa itu 'transistor sebagai kapasitor'?", "id": "Sebuah MOSFET dapat dikonfigurasi untuk berfungsi sebagai kapasitor (MOSCAP), yang berguna dalam sirkuit terpadu karena dapat dibuat dengan presisi." },
  { "en": "Apa itu 'gummel plot'?", "id": "Gummel plot adalah grafik standar yang digunakan untuk mengkarakterisasi BJT, dengan memplot arus Kolektor dan Basis terhadap tegangan Basis-Emitor pada skala logaritmik." },
  { "en": "Apa informasi yang bisa didapat dari Gummel plot?", "id": "Ini memberikan informasi tentang gain arus, idealitas sambungan, dan arus bocor dari sebuah BJT." },
  { "en": "Apa itu 'body diode' pada MOSFET?", "id": "Struktur internal MOSFET secara inheren mengandung sebuah dioda parasit antara terminal Drain dan Source. Dioda ini penting dalam aplikasi switching." },
  { "en": "Kapan 'body diode' ini menjadi penting?", "id": "Dalam rangkaian H-bridge atau buck converter, dioda ini sering berfungsi sebagai dioda 'freewheeling' untuk mengalirkan arus induktif saat transistor OFF." },
  { "en": "Apa itu 'avalanche rating' pada MOSFET?", "id": "Ini adalah spesifikasi yang menunjukkan seberapa banyak energi breakdown (avalanche) yang dapat diserap oleh MOSFET tanpa mengalami kerusakan." },
  { "en": "Apa itu 'gate charge' (QG)?", "id": "Gate charge adalah jumlah total muatan listrik yang dibutuhkan untuk menyalakan (atau mematikan) sebuah MOSFET. Ini menentukan seberapa banyak arus yang dibutuhkan untuk menggerakkan Gate." },
  { "en": "Mengapa 'gate charge' penting?", "id": "Nilai gate charge yang lebih rendah berarti MOSFET dapat dinyalakan dan dimatikan lebih cepat dan dengan lebih sedikit daya, yang krusial untuk aplikasi switching frekuensi tinggi." },
  { "en": "Apa itu 'gate driver IC'?", "id": "Ini adalah sirkuit terpadu khusus yang dirancang untuk menyediakan pulsa arus tinggi yang dibutuhkan untuk mengisi dan mengosongkan gate charge MOSFET dengan cepat dan efisien." },
  { "en": "Kapan kita memerlukan 'gate driver IC'?", "id": "Saat mengendalikan MOSFET daya besar atau pada frekuensi switching yang sangat tinggi, di mana output pin mikrokontroler biasa tidak cukup kuat untuk melakukannya dengan cepat." },
  { "en": "Apa itu 'kelvin connection' pada terminal transistor?", "id": "Ini adalah penggunaan koneksi empat-terminal (dua untuk arus daya, dua untuk sinyal sensor) untuk mengukur atau mengontrol tegangan secara akurat langsung di terminal, menghilangkan efek penurunan tegangan pada jalur." },
  { "en": "Apa itu 'breakdown sekunder' (secondary breakdown)?", "id": "Ini adalah kegagalan termal yang merusak pada BJT daya, di mana 'current hogging' di dalam chip itu sendiri menciptakan titik panas (hotspot) yang menyebabkan kerusakan permanen." },
  { "en": "Mengapa bagian dasar dari kemasan TO-220 terbuat dari logam?", "id": "Bagian logam itu biasanya terhubung langsung ke terminal Kolektor (atau Drain) dan berfungsi sebagai jalur utama untuk membuang panas ke heatsink." },
  { "en": "Apa perbedaan antara 'sinyal' dan 'daya' dalam konteks transistor?", "id": "Transistor 'sinyal' dirancang untuk memanipulasi informasi dengan akurat (gain tinggi, noise rendah). Transistor 'daya' dirancang untuk menangani energi besar (mengontrol motor, lampu) dengan efisiensi tinggi." },
  { "en": "Bagaimana suhu memengaruhi RDS(on) pada MOSFET?", "id": "Berbeda dengan tegangan ambang, RDS(on) atau resistansi saat ON pada MOSFET akan meningkat seiring dengan kenaikan suhu." },
  { "en": "Apa itu 'negative temperature coefficient' (NTC)?", "id": "NTC berarti suatu parameter menurun nilainya ketika suhu meningkat, contohnya adalah tegangan Basis-Emitor pada BJT." },
  { "en": "Apa itu 'positive temperature coefficient' (PTC)?", "id": "PTC berarti suatu parameter meningkat nilainya ketika suhu meningkat, contohnya adalah resistansi RDS(on) pada MOSFET." },
  { "en": "Mengapa karakteristik suhu MOSFET (PTC pada RDS(on)) membantu saat diparalel?", "id": "Karena jika satu MOSFET mulai 'mencuri' arus dan memanas, resistansinya akan naik, yang secara alami akan 'mendorong' arus ke MOSFET lain yang lebih dingin, membantu penyeimbangan secara otomatis." },
  { "en": "Apakah transistor menciptakan energi saat menguatkan sinyal?", "id": "Tidak, transistor tidak menciptakan energi. Ia bertindak seperti katup yang membentuk dan mengontrol energi dari sumber daya (catu daya) untuk meniru sinyal input dalam bentuk yang lebih kuat." },
  { "en": "Apakah arus Basis 'berubah menjadi' arus Kolektor?", "id": "Tidak. Arus Basis adalah sinyal kontrol kecil yang membuka 'gerbang', memungkinkan aliran arus yang jauh lebih besar dan terpisah untuk mengalir dari Kolektor ke Emitor." },
  { "en": "Saat menyolder transistor, mengapa sebaiknya tidak terlalu lama memanaskan kakinya?", "id": "Panas yang berlebihan dari solder dapat merambat naik melalui kaki (terminal) dan merusak chip semikonduktor yang sensitif di dalam kemasan." },
  { "en": "Mengapa penting untuk membaca datasheet sebelum menggunakan transistor tipe baru?", "id": "Datasheet berisi informasi krusial seperti batas maksimum tegangan, arus, dan daya, serta konfigurasi pin (kaki), yang jika diabaikan dapat merusak transistor." },
  { "en": "Dalam rangkaian filter aktif, apa peran utama transistor?", "id": "Transistor berfungsi sebagai penguat (amplifier) dan penyangga (buffer) untuk jaringan filter pasif (resistor-kapasitor), memungkinkan filter bekerja tanpa membebani sumber sinyal." },
  { "en": "Apa fungsi kapasitor yang sering dipasang paralel dengan resistor emitor?", "id": "Kapasitor tersebut (bypass capacitor) berfungsi sebagai jalan pintas untuk sinyal AC, yang secara efektif meningkatkan penguatan (gain) rangkaian untuk sinyal AC tanpa mengubah bias DC-nya." },
  { "en": "Bagaimana transistor dan dioda Zener bekerja sama dalam regulator tegangan sederhana?", "id": "Dioda Zener menyediakan tegangan referensi yang stabil ke Basis transistor, dan transistor kemudian menguatkan arus untuk menyediakan tegangan output yang stabil ke beban." },
  { "en": "Bagaimana pilihan transistor bisa memengaruhi daya tahan baterai sebuah perangkat?", "id": "Transistor dengan efisiensi tinggi (misalnya RDS(on) rendah pada MOSFET) dan arus bocor yang sangat kecil akan membuang lebih sedikit daya, sehingga memperpanjang masa pakai baterai." },
  { "en": "Mengapa transistor dalam CPU modern beroperasi pada tegangan sangat rendah (misalnya sekitar 1V)?", "id": "Untuk mengurangi konsumsi daya dan produksi panas secara drastis. Ini memungkinkan miliaran transistor untuk ditempatkan dalam satu chip tanpa meleleh." },
  { "en": "Jelaskan 'linearitas' penguat dengan analogi mesin fotokopi.", "id": "Penguat yang sangat linier seperti mesin fotokopi sempurna: salinannya (output) identik dengan aslinya (input), hanya lebih besar. Penguat non-linier akan menghasilkan salinan yang terdistorsi." },
  { "en": "Apa itu 'headroom' dalam sebuah penguat?", "id": "Headroom adalah ruang aman atau margin antara level puncak sinyal normal dan level maksimum absolut yang dapat ditangani penguat sebelum terjadi 'clipping' atau distorsi." },
  { "en": "Apa perbedaan antara kegagalan 'korsleting' (short) dan 'terbuka' (open) pada transistor?", "id": "Kegagalan 'short' berarti terminalnya terhubung secara internal (mis. Kolektor ke Emitor), sedangkan 'open' berarti koneksi internalnya terputus." },
  { "en": "Apa itu 'hambatan input' (input impedance) dan mengapa ini penting?", "id": "Hambatan input adalah resistansi yang 'dilihat' oleh sumber sinyal. Hambatan input yang tinggi lebih disukai karena tidak banyak 'menyedot' arus dari sumber sinyal, sehingga tidak melemahkannya." },
  { "en": "Apa itu 'hambatan output' (output impedance) dan mengapa ini penting?", "id": "Hambatan output adalah resistansi internal pada keluaran penguat. Hambatan output yang rendah lebih disukai karena memungkinkan penguat untuk menggerakkan beban (seperti speaker) secara efisien." },
  { "en": "Apa peran transistor dalam rangkaian 'pembangkit gelombang kotak' (square wave generator)?", "id": "Dalam multivibrator astabil, dua transistor digunakan sebagai saklar yang saling memicu secara bergantian, menghasilkan sinyal output yang terus beralih antara tinggi dan rendah." },
  { "en": "Mengapa kita tidak bisa langsung menghubungkan output mikrokontroler ke motor?", "id": "Karena pin output mikrokontroler hanya dapat menyediakan arus yang sangat kecil, tidak cukup untuk memutar motor. Diperlukan transistor sebagai saklar untuk menangani arus motor yang besar." },
  { "en": "Apa itu 'slew rate induced distortion'?", "id": "Ini adalah jenis distorsi yang terjadi ketika sinyal input berubah lebih cepat daripada yang bisa diikuti oleh output penguat (dibatasi oleh slew rate), menyebabkan sinyal output menjadi 'tumpul' atau berbentuk segitiga." },
  { "en": "Apa itu 'termistor' dan bagaimana bedanya dengan menggunakan transistor sebagai sensor suhu?", "id": "Termistor adalah resistor yang nilainya sangat sensitif terhadap suhu. Keduanya dapat digunakan, tetapi transistor memberikan respons yang lebih linier sementara termistor seringkali lebih sensitif." },
  { "en": "Apa itu 'efek saturasi kecepatan' (velocity saturation) pada FET?", "id": "Pada medan listrik yang sangat tinggi, kecepatan pembawa muatan dalam channel mencapai batas maksimum dan tidak bisa bertambah lagi, yang membatasi peningkatan arus lebih lanjut." },
  { "en": "Apa itu 'kelas efisiensi' pada catu daya?", "id": "Ini adalah standar (misalnya 80 Plus Bronze, Gold) yang menunjukkan seberapa efisien catu daya mengubah listrik AC dari stopkontak menjadi listrik DC untuk komponen, di mana efisiensi tinggi dicapai berkat transistor switching modern." },
  { "en": "Apa yang dimaksud dengan 'soft switching' dalam SMPS?", "id": "Ini adalah teknik canggih di mana transistor dinyalakan atau dimatikan hanya ketika tegangan atau arusnya nol, yang secara dramatis mengurangi kerugian switching dan EMI." },
  { "en": "Apa itu 'zero-crossing detector'?", "id": "Ini adalah sirkuit (seringkali menggunakan transistor atau op-amp) yang menghasilkan pulsa setiap kali sinyal AC melewati nol volt, berguna untuk men-sinkronisasi switching seperti pada dimmer TRIAC." },
  { "en": "Apa itu 'penguat diferensial pasangan ekor panjang' (long-tailed pair)?", "id": "Ini adalah nama lain untuk penguat diferensial BJT, di mana resistor Emitor bersama (disebut 'ekor') memiliki resistansi tinggi untuk meningkatkan CMRR." },
  { "en": "Apa itu 'arus diam' atau 'quiescent current' (IQ)?", "id": "IQ adalah arus yang dikonsumsi oleh sebuah sirkuit (atau transistor) saat dalam keadaan siaga atau 'diam' (tidak ada sinyal input). Nilai IQ yang rendah penting untuk perangkat bertenaga baterai." },
  { "en": "Bagaimana cara kerja rangkaian 'peak detector' sederhana dengan transistor?", "id": "Sebuah dioda mengisi kapasitor hingga tegangan puncak sinyal, dan transistor (dalam konfigurasi emitter follower) berfungsi sebagai buffer untuk memungkinkan kita mengukur tegangan pada kapasitor tanpa mengosongkannya." },
  { "en": "Apa itu 'die' semikonduktor?", "id": "'Die' adalah potongan kecil silikon dari wafer yang berisi sirkuit lengkap dari sebuah komponen, baik itu satu transistor atau miliaran transistor dalam sebuah CPU." },
  { "en": "Apa itu 'wire bonding' di dalam kemasan IC?", "id": "Wire bonding adalah proses menghubungkan 'die' semikonduktor ke pin logam dari kemasan komponen menggunakan kabel emas atau aluminium yang sangat tipis." },
  { "en": "Mengapa kemasan keramik lebih disukai daripada plastik untuk aplikasi militer atau luar angkasa?", "id": "Kemasan keramik memberikan perlindungan yang jauh lebih baik terhadap kelembaban, guncangan, dan radiasi, serta memiliki konduktivitas termal yang lebih baik." },
  { "en": "Apa itu 'hermetic seal'?", "id": "Ini adalah segel kedap udara yang digunakan pada kemasan keramik untuk melindungi die semikonduktor dari kontaminasi lingkungan, memastikan keandalan jangka panjang." },
  { "en": "Apa itu 'efek popcorn' pada transistor?", "id": "Ini adalah jenis noise frekuensi rendah yang terdengar seperti letupan popcorn pada output audio, disebabkan oleh cacat pada chip semikonduktor." },
  { "en": "Apa itu 'flicker noise' atau '1/f noise'?", "id": "Ini adalah jenis noise elektronik yang dominan pada frekuensi rendah, di mana intensitasnya berbanding terbalik dengan frekuensi (semakin rendah frekuensi, semakin kuat noise-nya)." },
  { "en": "Dapatkah tata letak PCB memengaruhi kinerja transistor?", "id": "Sangat bisa. Jalur yang panjang atau loop ground yang buruk dapat menambah induktansi dan resistansi parasit, menyebabkan ketidakstabilan, noise, dan penurunan kinerja frekuensi tinggi." },
  { "en": "Apa aturan praktis untuk menempatkan kapasitor decoupling?", "id": "Tempatkan kapasitor decoupling sedekat mungkin secara fisik dengan pin catu daya transistor untuk meminimalkan panjang jalur dan memaksimalkan efektivitasnya." },
  { "en": "Apa itu 'ground plane' pada PCB?", "id": "Ground plane adalah lapisan tembaga besar yang didedikasikan untuk koneksi ground. Ini menyediakan jalur kembali berimpedansi rendah untuk arus dan membantu melindungi dari noise." },
  { "en": "Apa itu 'isolasi galvanik'?", "id": "Isolasi galvanik berarti tidak ada jalur konduksi listrik langsung antara dua bagian sirkuit. Ini dicapai menggunakan komponen seperti transformator atau optocoupler (yang menggunakan transistor)." },
  { "en": "Apa itu 'penguat isolasi'?", "id": "Ini adalah penguat khusus yang menyediakan isolasi galvanik antara input dan output, penting untuk pengukuran medis yang aman atau di lingkungan industri yang bising." },
  { "en": "Apa itu 'efek piezoelektrik' dan bagaimana hubungannya dengan transistor?", "id": "Beberapa material menghasilkan tegangan saat ditekan. Dalam kasus ekstrem, getaran mekanis pada papan sirkuit dapat menghasilkan tegangan 'noise' kecil yang dapat memengaruhi sirkuit transistor yang sangat sensitif." },
  { "en": "Apa itu 'kapasitor variabel' (varactor diode)?", "id": "Ini adalah dioda khusus yang kapasitansinya berubah tergantung pada tegangan bias mundur yang diberikan. Ini sering digunakan bersama transistor dalam sirkuit VCO." },
  { "en": "Bagaimana cara menguji MOSFET dengan multimeter digital?", "id": "Gunakan mode tes dioda untuk memeriksa 'body diode' internal. Anda juga bisa mencoba mengisi Gate dengan sentuhan probe untuk melihat apakah Drain-Source melakukan konduksi sesaat." },
  { "en": "Apa itu 'floating body effect' pada teknologi SOI?", "id": "Karena badan (body) transistor terisolasi, muatan dapat menumpuk di sana, yang dapat menyebabkan perubahan tak terduga pada tegangan ambang transistor." },
  { "en": "Apa itu 'latch-up' pada sirkuit CMOS?", "id": "Latch-up adalah kondisi korsleting yang merusak di mana struktur parasit NPN dan PNP di dalam chip CMOS membentuk SCR yang 'terkunci', menyebabkan aliran arus besar dari catu daya ke ground." },
  { "en": "Apa yang dapat memicu 'latch-up'?", "id": "Latch-up dapat dipicu oleh lonjakan tegangan di atas Vcc atau di bawah ground pada pin input/output, atau oleh ESD." },
  { "en": "Apa perbedaan antara JFET N-channel dan P-channel?", "id": "Selain material dan polaritas tegangan yang berlawanan, JFET P-channel umumnya memiliki kinerja sedikit lebih rendah karena mobilitas 'hole' lebih rendah daripada elektron." },
  { "en": "Apa itu 'resistansi Emitor dinamis' (re)?", "id": "Ini adalah model resistansi internal yang sangat disederhanakan dari sambungan Basis-Emitor untuk analisis sinyal AC, yang nilainya bergantung pada arus bias DC." },
  { "en": "Apa itu 'penguat operasional' atau Op-Amp?", "id": "Op-Amp adalah sirkuit terpadu penguat diferensial gain sangat tinggi yang merupakan blok bangunan fundamental dalam elektronika analog. Di dalamnya terdapat banyak transistor." },
  { "en": "Apa itu 'Op-Amp ideal'?", "id": "Op-Amp ideal adalah model teoretis dengan gain tak terhingga, impedansi input tak terhingga, dan impedansi output nol, yang menyederhanakan analisis rangkaian." },
  { "en": "Apa peran tahap input pada Op-Amp?", "id": "Tahap input adalah penguat diferensial (menggunakan BJT atau JFET) yang memberikan gain tinggi, CMRR tinggi, dan impedansi input tinggi." },
  { "en": "Apa peran tahap output pada Op-Amp?", "id": "Tahap output adalah penguat push-pull (biasanya Kelas AB) yang dirancang untuk memberikan arus yang cukup untuk menggerakkan beban dengan impedansi output yang rendah." },
  { "en": "Bagaimana umpan balik negatif membuat Op-Amp berguna?", "id": "Dengan menerapkan umpan balik negatif, gain 'tak terhingga' dari Op-Amp dapat dikontrol secara presisi untuk menciptakan penguat dengan gain yang stabil dan dapat diprediksi." },
  { "en": "Apa itu 'komparator'?", "id": "Komparator (yang dapat dibuat dari transistor atau Op-Amp tanpa umpan balik) adalah sirkuit yang membandingkan dua tegangan dan mengeluarkan sinyal digital HIGH atau LOW yang menunjukkan mana yang lebih besar." },
  { "en": "Apa itu 'tegangan offset input' pada Op-Amp?", "id": "Ini adalah tegangan DC kecil yang harus diterapkan antara input untuk memaksa output menjadi nol, yang disebabkan oleh ketidakcocokan kecil antara transistor di tahap input." },
  { "en": "Apa itu 'arus bias input' pada Op-Amp?", "id": "Ini adalah arus DC kecil yang harus mengalir ke (atau dari) terminal input untuk mem-bias transistor tahap input. Ini bisa menjadi masalah saat bekerja dengan sumber sinyal berimpedansi sangat tinggi." },
  { "en": "Apa itu 'pengikut tegangan' (voltage follower)?", "id": "Ini adalah konfigurasi Op-Amp (atau transistor Common Collector) dengan gain tegangan 1. Fungsinya bukan untuk menguatkan, tetapi sebagai 'buffer' untuk mengisolasi sumber dari beban." },
  { "en": "Mengapa transistor kadang disebut komponen 'solid-state'?", "id": "Karena operasinya terjadi di dalam material padat (solid) semikonduktor, berbeda dengan tabung vakum yang memiliki ruang hampa di dalamnya." },
  { "en": "Apa itu 'wafer fabrication plant' atau 'fab'?", "id": "Ini adalah pabrik berteknologi sangat tinggi dan sangat bersih tempat transistor dan sirkuit terpadu dibuat di atas wafer silikon." },
  { "en": "Mengapa kebersihan ekstrem ('cleanroom') sangat penting dalam pembuatan IC?", "id": "Karena bahkan satu partikel debu kecil pun jauh lebih besar dari fitur pada transistor modern dan dapat dengan mudah menyebabkan cacat yang merusak seluruh chip." },
  { "en": "Apa itu 'fotolitografi'?", "id": "Fotolitografi adalah proses inti dalam pembuatan IC di mana pola sirkuit dari sebuah 'mask' ditransfer ke wafer silikon menggunakan cahaya (biasanya ultraviolet)." },
  { "en": "Apa itu 'mask' dalam fotolitografi?", "id": "Mask adalah lempengan kuarsa dengan pola sirkuit buram (biasanya krom) yang bertindak seperti stensil atau negatif foto untuk proses litografi." },
  { "en": "Apa itu 'photoresist'?", "id": "Photoresist adalah bahan kimia peka cahaya yang dioleskan pada wafer. Saat terkena cahaya melalui mask, sifatnya berubah, memungkinkannya untuk dicuci bersih dan meninggalkan pola sirkuit." },
  { "en": "Apa itu 'etching' (pengetsaan)?", "id": "Setelah pola dibuat dengan photoresist, etsa adalah proses menggunakan bahan kimia atau plasma untuk menghilangkan material (seperti oksida atau logam) dari area yang tidak terlindungi." },
  { "en": "Apa itu 'doping difusi'?", "id": "Ini adalah metode doping yang lebih tua di mana wafer dipanaskan dalam tungku dengan gas yang mengandung atom dopan, yang kemudian 'berdifusi' atau meresap ke dalam permukaan silikon." },
  { "en": "Mengapa implantasi ion lebih disukai daripada difusi?", "id": "Implantasi ion memungkinkan kontrol yang jauh lebih presisi atas jumlah (dosis) dan kedalaman atom dopan, yang sangat penting untuk transistor modern yang sangat kecil." },
  { "en": "Apa itu 'mobilitas pembawa muatan' (carrier mobility)?", "id": "Mobilitas adalah ukuran seberapa mudah pembawa muatan (elektron atau hole) dapat bergerak melalui kristal semikonduktor di bawah pengaruh medan listrik. Mobilitas yang lebih tinggi berarti kinerja yang lebih baik." },
  { "en": "Mengapa mobilitas elektron lebih tinggi dari mobilitas hole?", "id": "Secara sederhana, elektron bebas bergerak lebih mudah melalui kisi kristal daripada 'hole', yang pergerakannya melibatkan perpindahan elektron ikatan dari satu atom ke atom lainnya." },
  { "en": "Apa implikasi dari perbedaan mobilitas ini?", "id": "Ini berarti transistor berbasis elektron (NPN, N-channel) umumnya lebih cepat dan lebih efisien daripada padanannya yang berbasis hole (PNP, P-channel) dengan ukuran yang sama." },
  { "en": "Apa itu 'semikonduktor majemuk' (compound semiconductor)?", "id": "Ini adalah semikonduktor yang terbuat dari dua atau lebih elemen, seperti Gallium Arsenide (GaAs). Mereka seringkali memiliki mobilitas elektron yang jauh lebih tinggi daripada silikon." },
  { "en": "Kapan semikonduktor majemuk digunakan?", "id": "Mereka digunakan dalam aplikasi frekuensi sangat tinggi (RF) seperti pada ponsel dan radar, dan dalam komponen optoelektronik seperti LED dan dioda laser." },
  { "en": "Apa itu 'strained silicon'?", "id": "Ini adalah teknik di mana kisi kristal silikon sengaja diregangkan. Peregangan ini memungkinkan elektron bergerak lebih mudah, meningkatkan mobilitas dan kinerja transistor." },
  { "en": "Apa itu 'high-k dielectric'?", "id": "'High-k' mengacu pada material isolator gerbang dengan konstanta dielektrik tinggi yang digunakan sebagai pengganti silikon dioksida pada transistor canggih." },
  { "en": "Mengapa 'high-k dielectric' diperlukan?", "id": "Saat gerbang semakin kecil, isolator silikon dioksida menjadi sangat tipis sehingga arus bocor karena efek terowongan menjadi masalah besar. Material high-k memungkinkan isolator yang lebih tebal secara fisik namun setara secara elektrik, mengurangi kebocoran." },
  { "en": "Apa itu 'gerbang logam' (metal gate)?", "id": "Ini adalah kembalinya penggunaan gerbang yang terbuat dari logam (bukan polisilikon) pada transistor modern, seringkali digunakan bersama dengan dielektrik high-k untuk meningkatkan kinerja." },
  { "en": "Bagaimana transistor digunakan dalam sel memori SRAM (Static RAM)?", "id": "Satu sel memori SRAM biasanya menggunakan enam transistor untuk membentuk rangkaian flip-flop yang dapat menyimpan satu bit data (0 atau 1) selama daya menyala." },
  { "en": "Mengapa SRAM disebut 'statis'?", "id": "Karena ia akan menyimpan datanya secara statis selama daya listrik tersedia, tidak seperti DRAM yang harus 'di-refresh' secara berkala." },
  { "en": "Bagaimana transistor digunakan dalam sel memori DRAM (Dynamic RAM)?", "id": "Satu sel memori DRAM jauh lebih sederhana, biasanya hanya menggunakan satu transistor dan satu kapasitor kecil. Transistor bertindak sebagai saklar untuk mengakses kapasitor." },
  { "en": "Mengapa DRAM disebut 'dinamis'?", "id": "Karena muatan pada kapasitornya yang kecil akan bocor seiring waktu, sehingga harus dibaca dan ditulis ulang (di-refresh) secara dinamis ribuan kali per detik agar tidak kehilangan data." },
  { "en": "Mana yang lebih padat, SRAM atau DRAM?", "id": "DRAM jauh lebih padat karena selnya (1T/1C) jauh lebih kecil daripada sel SRAM (6T), memungkinkan lebih banyak memori untuk dimasukkan ke dalam area chip yang sama." },
  { "en": "Mana yang lebih cepat, SRAM atau DRAM?", "id": "SRAM secara signifikan lebih cepat daripada DRAM karena tidak memerlukan siklus refresh dan desain flip-flopnya memungkinkan akses yang lebih langsung." },
  { "en": "Di mana SRAM digunakan?", "id": "SRAM digunakan untuk memori cache yang sangat cepat di dalam CPU, di mana kecepatan adalah yang terpenting." },
  { "en": "Di mana DRAM digunakan?", "id": "DRAM digunakan untuk memori utama (RAM) sistem komputer, di mana kepadatan tinggi dan biaya per bit yang rendah lebih penting daripada kecepatan absolut." },
  { "en": "Apa itu 'yield' dalam manufaktur semikonduktor?", "id": "Yield adalah persentase chip yang berfungsi dengan benar dari sebuah wafer. Yield yang tinggi sangat penting untuk menjaga biaya produksi tetap rendah." },
  { "en": "Apa itu 'binning'?", "id": "Binning adalah proses menguji dan mengelompokkan chip (seperti CPU atau GPU) berdasarkan kinerjanya. Chip yang berkinerja terbaik dijual dengan harga lebih tinggi daripada yang kinerjanya lebih rendah, meskipun berasal dari wafer yang sama." },
  { "en": "Apa itu 'ESD clamp'?", "id": "Ini adalah sirkuit proteksi berbasis transistor yang terintegrasi di dalam IC, dirancang untuk menyerap dan mengalihkan lonjakan ESD yang berbahaya menjauh dari sirkuit internal yang sensitif." },
  { "en": "Apa itu 'power-on reset' (POR) circuit?", "id": "POR adalah sirkuit (seringkali menggunakan transistor dan kapasitor) yang memastikan mikrokontroler atau IC digital lainnya memulai dalam keadaan yang diketahui dan stabil saat daya pertama kali diberikan." },
  { "en": "Apa itu 'brown-out detector' (BOD)?", "id": "BOD adalah sirkuit yang memonitor tegangan catu daya. Jika tegangan turun di bawah level aman (brown-out), ia akan menempatkan prosesor dalam keadaan reset untuk mencegah perilaku yang tidak menentu atau kerusakan data." },
  { "en": "Apa itu 'watchdog timer'?", "id": "Watchdog timer adalah penghitung waktu yang harus di-reset secara berkala oleh perangkat lunak. Jika perangkat lunak macet dan gagal me-resetnya, timer akan meluap dan secara otomatis me-reboot prosesor, di mana transistor menjadi dasar dari semua sirkuit ini." },
  { "en": "Apa itu 'transistor array'?", "id": "Transistor array adalah sebuah IC tunggal yang berisi beberapa transistor terpisah (biasanya 4 atau 7) yang tidak terhubung secara internal, berguna untuk menghemat ruang papan sirkuit." },
  { "en": "Apa contoh penggunaan transistor array?", "id": "Contoh populer adalah IC ULN2003, yang berisi tujuh pasang transistor Darlington dengan dioda flyback, ideal untuk menggerakkan beberapa relay atau lampu dari mikrokontroler." },
  { "en": "Apa itu 'analog-to-digital converter' (ADC)?", "id": "ADC adalah sirkuit yang mengubah sinyal analog (seperti dari sensor) menjadi nilai digital. Transistor adalah komponen inti dari komparator dan saklar di dalam ADC." },
  { "en": "Apa itu 'digital-to-analog converter' (DAC)?", "id": "DAC adalah sirkuit yang mengubah data digital kembali menjadi sinyal tegangan atau arus analog. Ini sering menggunakan jaringan resistor presisi dan saklar transistor." },
  { "en": "Apa itu 'R-2R ladder'?", "id": "R-2R ladder adalah arsitektur DAC sederhana dan elegan yang menggunakan jaringan resistor dengan hanya dua nilai (R dan 2R) dan saklar berbasis transistor untuk menghasilkan tegangan analog." },
  { "en": "Apa itu 'charge pump'?", "id": "Charge pump adalah jenis konverter DC-DC yang menggunakan kapasitor dan saklar transistor untuk menghasilkan tegangan yang lebih tinggi atau lebih rendah dari tegangan input, sering digunakan untuk aplikasi daya rendah." },
  { "en": "Apa itu 'H-bridge'?", "id": "H-bridge adalah sirkuit yang menggunakan empat transistor sebagai saklar untuk memungkinkan motor DC berputar ke dua arah (maju dan mundur)." },
  { "en": "Mengapa disebut 'H-bridge'?", "id": "Karena diagram skematiknya terlihat seperti huruf 'H', dengan motor ditempatkan di palang tengah dan empat transistor di keempat 'kaki'-nya." },
  { "en": "Apa itu 'shoot-through' pada H-bridge?", "id": "Shoot-through adalah kondisi berbahaya di mana kedua transistor di sisi yang sama (atas dan bawah) menyala secara bersamaan, menciptakan korsleting langsung dari catu daya ke ground." },
  { "en": "Bagaimana cara mencegah 'shoot-through'?", "id": "Dengan menggunakan 'dead time' (waktu mati) dalam sinyal kontrol, yaitu jeda singkat untuk memastikan satu transistor sudah benar-benar mati sebelum yang lain di sisi yang sama mulai menyala." },
  { "en": "Secara ringkas, jelaskan peran transistor di dunia digital dan dunia analog.", "id": "Di dunia digital, transistor adalah saklar biner (0 atau 1) super cepat. Di dunia analog, ia adalah katup variabel untuk membentuk dan memperkuat sinyal." },
  { "en": "Apa perbedaan paling mendasar antara cara kerja BJT dan MOSFET?", "id": "BJT adalah perangkat yang dikontrol oleh arus (arus kecil di Basis mengontrol arus besar di Kolektor). MOSFET adalah perangkat yang dikontrol oleh tegangan (tegangan di Gate mengontrol aliran arus di Drain)." },
  { "en": "Sebutkan kembali tiga daerah operasi transistor dan fungsi utamanya.", "id": "Cut-off (transistor OFF, untuk saklar), Saturasi (transistor ON penuh, untuk saklar), dan Daerah Aktif (di antaranya, untuk penguatan/amplifikasi)." },
  { "en": "Jika Anda menemukan transistor tak dikenal, apa tes pertama untuk mengidentifikasinya?", "id": "Gunakan multimeter pada mode tes dioda untuk menemukan terminal Basis (yang terhubung seperti dioda ke dua terminal lainnya) dan menentukan apakah itu tipe NPN atau PNP." },
  { "en": "Mengapa dalam desain sirkuit, gain seringkali sengaja dibuat lebih rendah dari gain maksimum transistor?", "id": "Untuk mendapatkan stabilitas yang lebih baik, mengurangi distorsi (meningkatkan linearitas), dan memperlebar rentang frekuensi (bandwidth) yang dapat dioperasikan." },
  { "en": "Apa keuntungan utama merancang sirkuit dengan umpan balik negatif (negative feedback)?", "id": "Ini membuat kinerja sirkuit lebih dapat diprediksi dan stabil, lebih bergantung pada nilai komponen pasif (resistor/kapasitor) daripada karakteristik transistor yang dapat bervariasi." },
  { "en": "Mengapa penting untuk memeriksa 'Safe Operating Area' (SOA) dan bukan hanya Vmax dan Imax?", "id": "Karena kemampuan transistor menangani daya bergantung pada kombinasi tegangan dan arus secara bersamaan. Beroperasi pada Vmax dan Imax sekaligus hampir pasti akan merusaknya." },
  { "en": "Hubungkan konsep 'biasing', 'titik-Q', dan 'headroom'.", "id": "Biasing yang tepat digunakan untuk menempatkan titik-Q (titik operasi diam) di tengah-tengah daerah aktif, yang akan memaksimalkan 'headroom' (ruang gerak) untuk sinyal agar tidak terpotong." },
  { "en": "Bagaimana 'arus bocor' dan 'disipasi daya' berhubungan dalam sebuah CPU?", "id": "Meskipun arus bocor per transistor sangat kecil, CPU modern memiliki miliaran transistor. Jumlah total dari semua arus bocor ini menciptakan 'disipasi daya statis' yang signifikan, yang menghasilkan panas bahkan saat CPU tidak aktif." },
  { "en": "Apa yang akan terjadi pada dunia elektronik jika Hukum Moore berhenti total hari ini?", "id": "Inovasi akan bergeser dari sekadar membuat transistor lebih kecil, ke arah arsitektur sirkuit yang lebih cerdas, material baru, komputasi 3D (menumpuk chip), dan efisiensi perangkat lunak." },
  { "en": "Apa analogi paling sederhana untuk sebuah IC (Integrated Circuit)?", "id": "Sebuah kota yang sangat padat di atas sepotong kecil pasir, di mana transistor adalah bangunan, dan lapisan kabel logam adalah jalan dan jalan layangnya." },
  { "en": "Jika Anda harus menjelaskan transistor kepada seorang anak, apa yang akan Anda katakan?", "id": "Ini adalah saklar listrik super kecil dan super cepat yang bisa kamu nyalakan dan matikan hanya dengan sentuhan sinyal listrik kecil lainnya." },
  { "en": "Apa itu 'transistor sebagai dioda Zener'?", "id": "Sambungan Basis-Emitor dari BJT dapat digunakan sebagai dioda Zener darurat karena memiliki tegangan tembus balik yang relatif stabil dan dapat diprediksi (biasanya 5-9V)." },
  { "en": "Apa itu 'model Ebers-Moll'?", "id": "Ini adalah salah satu model matematika pertama dan fundamental untuk BJT, yang merepresentasikan transistor sebagai dua dioda yang terhubung dan sumber arus yang saling bergantung." },
  { "en": "Apa itu 'current crowding'?", "id": "Pada arus tinggi, aliran arus di bawah emitor cenderung 'berkerumun' atau terkonsentrasi di tepi emitor, yang dapat meningkatkan resistansi internal dan menghasilkan panas tidak merata." },
  { "en": "Apa itu 'resistansi on' (Ron) pada saklar analog?", "id": "Ini adalah resistansi dari saklar berbasis transistor saat dalam keadaan ON. Nilai yang rendah lebih disukai karena menyebabkan lebih sedikit pelemahan sinyal." },
  { "en": "Apa itu 'isolasi off' (off isolation) pada saklar analog?", "id": "Ini adalah ukuran seberapa baik saklar memblokir sinyal saat dalam keadaan OFF. Nilai isolasi yang tinggi (dalam dB) lebih baik." },
  { "en": "Bagaimana cara kerja 'pengatur volume digital' menggunakan transistor?", "id": "Ini menggunakan jaringan resistor presisi dan saklar analog berbasis transistor (multiplexer) untuk memilih pelemahan sinyal yang berbeda, menghasilkan langkah-langkah volume yang diskrit." },
  { "en": "Apa itu 'bipolar-CMOS' (BiCMOS)?", "id": "BiCMOS adalah teknologi manufaktur yang menggabungkan transistor BJT dan CMOS pada chip yang sama, memanfaatkan kecepatan BJT dan kepadatan serta daya rendah CMOS." },
  { "en": "Di mana teknologi BiCMOS sering digunakan?", "id": "Sering digunakan dalam sirkuit sinyal campuran (mixed-signal) berkinerja tinggi, seperti pada sistem komunikasi dan konverter data." },
  { "en": "Apa itu 'gain-cell' memori?", "id": "Ini adalah jenis sel memori DRAM eksperimental yang menggunakan gain dari transistor di dalamnya untuk meningkatkan sinyal yang tersimpan, berpotensi menggantikan kapasitor." },
  { "en": "Apa itu 'tegangan saturasi kolektor-basis'?", "id": "Ini adalah tegangan pada sambungan Kolektor-Basis saat transistor berada dalam saturasi. Dalam saturasi, sambungan ini sebenarnya diberi bias maju (forward-biased)." },
  { "en": "Apa itu 'saturasi terbalik' (inverted mode)?", "id": "Ini adalah saat BJT dioperasikan dengan menukar fungsi Kolektor dan Emitor. Transistor masih bekerja, tetapi dengan gain yang jauh lebih rendah." },
  { "en": "Kapan mode terbalik (inverted mode) digunakan?", "id": "Kadang-kadang digunakan dalam saklar analog bipolar dan chopper karena menawarkan tegangan offset yang lebih rendah daripada mode normal." },
  { "en": "Apa itu 'alpha' (α) pada BJT?", "id": "Alpha adalah perbandingan antara arus Kolektor dan arus Emitor (α = IC/IE). Nilainya selalu sedikit di bawah 1." },
  { "en": "Jika alpha sangat dekat dengan 1, apa artinya?", "id": "Artinya hampir semua arus dari Emitor berhasil mencapai Kolektor, dan hanya sebagian kecil yang 'hilang' menjadi arus Basis. Ini menandakan transistor yang baik." },
  { "en": "Apa itu 'penguat cascode lipat' (folded cascode)?", "id": "Ini adalah varian dari penguat cascode yang menggunakan transistor tipe komplementer (misalnya PNP untuk melipat NPN) agar dapat beroperasi pada tegangan catu daya yang lebih rendah." },
  { "en": "Apa itu 'penguat kelas C'?", "id": "Penguat kelas C di-bias di bawah titik cut-off, sehingga hanya menghantarkan arus untuk kurang dari setengah siklus sinyal. Ini sangat efisien tetapi juga sangat non-linier." },
  { "en": "Di mana penguat kelas C digunakan?", "id": "Hampir secara eksklusif digunakan dalam pemancar radio frekuensi tinggi (RF), di mana sirkuit tangki (tank circuit) LC digunakan untuk 'memulihkan' bentuk gelombang sinus penuh." },
  { "en": "Apa itu 'sirkuit tangki' (tank circuit)?", "id": "Sirkuit tangki adalah kombinasi paralel dari induktor (L) dan kapasitor (C) yang beresonansi pada frekuensi tertentu, berfungsi seperti ayunan elektronik." },
  { "en": "Apa itu 'penguat kelas E'?", "id": "Penguat kelas E adalah jenis penguat switching RF ber efisiensi sangat tinggi yang menggunakan penjadwalan waktu yang cermat dan resonansi beban untuk meminimalkan kerugian switching." },
  { "en": "Apa itu 'silicon carbide' (SiC) MOSFET?", "id": "Ini adalah jenis MOSFET daya yang terbuat dari silikon karbida, sebuah bahan semikonduktor 'celah-pita-lebar' (wide-bandgap)." },
  { "en": "Apa keunggulan MOSFET SiC dibandingkan silikon?", "id": "Mereka dapat beroperasi pada tegangan, suhu, dan frekuensi yang jauh lebih tinggi, serta memiliki kerugian switching yang lebih rendah, ideal untuk kendaraan listrik dan energi terbarukan." },
  { "en": "Apa itu 'gallium nitride' (GaN) HEMT?", "id": "HEMT (High-Electron-Mobility Transistor) GaN adalah jenis transistor lain yang berbasis bahan celah-pita-lebar, terkenal karena kecepatan switchingnya yang ekstrem." },
  { "en": "Di mana transistor GaN unggul?", "id": "Dalam aplikasi switching frekuensi sangat tinggi, seperti pada charger laptop modern yang sangat kecil, sistem LiDAR, dan infrastruktur 5G." },
  { "en": "Apa itu 'celah pita' (bandgap) semikonduktor?", "id": "Celah pita adalah energi minimum yang dibutuhkan untuk melepaskan sebuah elektron dari ikatannya agar dapat menghantarkan listrik. Celah pita yang lebih lebar berarti kemampuan menangani tegangan dan suhu yang lebih tinggi." },
  { "en": "Apa itu 'semikonduktor intrinsik'?", "id": "Semikonduktor intrinsik adalah semikonduktor dalam keadaan murni absolut, tanpa atom pengotor (doping). Konduktivitasnya sangat rendah." },
  { "en": "Apa itu 'tingkat Fermi' (Fermi level)?", "id": "Tingkat Fermi adalah tingkat energi referensi dalam sebuah material yang menggambarkan probabilitas sebuah elektron untuk menempati suatu keadaan energi." },
  { "en": "Bagaimana doping memengaruhi tingkat Fermi?", "id": "Doping dengan donor (tipe-N) menaikkan tingkat Fermi. Doping dengan akseptor (tipe-P) menurunkannya." },
  { "en": "Apa itu 'daerah deplesi' (depletion region)?", "id": "Ini adalah daerah di sekitar sambungan P-N di mana pembawa muatan bebas (elektron dan hole) telah 'dikosongkan', meninggalkan ion-ion yang diam dan menciptakan medan listrik internal." },
  { "en": "Bagaimana bias mundur memengaruhi daerah deplesi?", "id": "Bias mundur akan menarik lebih banyak pembawa muatan menjauh dari sambungan, sehingga memperlebar daerah deplesi." },
  { "en": "Bagaimana bias maju memengaruhi daerah deplesi?", "id": "Bias maju akan mendorong pembawa muatan menuju sambungan, menyempitkan daerah delesi dan memungkinkan arus mengalir." },
  { "en": "Apa itu 'potensial bawaan' (built-in potential)?", "id": "Ini adalah beda potensial alami yang ada di seluruh daerah deplesi pada sambungan P-N yang tidak diberi bias." },
  { "en": "Apa itu 'gradien konsentrasi'?", "id": "Ini adalah laju perubahan jumlah pembawa muatan per satuan jarak. Gradien inilah yang mendorong arus difusi." },
  { "en": "Apa itu 'arus difusi'?", "id": "Arus difusi adalah aliran pembawa muatan yang terjadi secara alami dari daerah berkonsentrasi tinggi ke daerah berkonsentrasi rendah, bahkan tanpa adanya medan listrik." },
  { "en": "Apa itu 'arus drift'?", "id": "Arus drift adalah aliran pembawa muatan yang disebabkan oleh gaya dari medan listrik eksternal." },
  { "en": "Dalam BJT, arus apa yang dominan melintasi Basis?", "id": "Arus difusi. Emitor menyuntikkan begitu banyak pembawa muatan ke Basis sehingga mereka berdifusi melintasi Basis yang sempit menuju Kolektor." },
  { "en": "Apa itu 'resistivitas' semikonduktor?", "id": "Resistivitas adalah ukuran seberapa kuat suatu material menentang aliran arus listrik. Nilainya bergantung pada tingkat doping." },
  { "en": "Bagaimana doping memengaruhi resistivitas?", "id": "Semakin tinggi tingkat doping, semakin banyak pembawa muatan yang tersedia, sehingga resistivitasnya semakin rendah (konduktivitasnya semakin tinggi)." },
  { "en": "Apa itu 'efek Gunn'?", "id": "Ini adalah fenomena pada beberapa semikonduktor majemuk (seperti GaAs) di mana pada medan listrik tinggi, mobilitas elektron justru menurun, yang dapat digunakan untuk membuat osilator gelombang mikro." },
  { "en": "Apa itu 'IMOS' (Impact-ionization MOS)?", "id": "Ini adalah jenis transistor eksperimental yang menggunakan longsoran ionisasi untuk mencapai kemiringan sub-ambang (subthreshold slope) yang sangat curam, memungkinkannya beroperasi pada tegangan sangat rendah." },
  { "en": "Apa itu 'TFET' (Tunnel FET)?", "id": "TFET adalah jenis transistor lain yang dirancang untuk operasi berdaya sangat rendah, yang mengandalkan efek terowongan kuantum untuk switching, bukan emisi termionik seperti MOSFET tradisional." },
  { "en": "Apa itu 'pengujian wafer' (wafer probing)?", "id": "Ini adalah langkah dalam manufaktur di mana setiap die pada wafer diuji menggunakan kartu probe dengan jarum-jarum kecil untuk memastikan fungsinya sebelum wafer dipotong." },
  { "en": "Apa itu 'pemotongan wafer' (wafer dicing)?", "id": "Ini adalah proses memotong wafer menjadi chip-chip individual (die) menggunakan gergaji berlian presisi tinggi atau laser." },
  { "en": "Apa itu 'perakitan die' (die attach)?", "id": "Ini adalah proses mengambil die individual dan menempelkannya ke substrat atau bingkai dari kemasan komponen, biasanya menggunakan perekat epoksi konduktif." },
  { "en": "Apa itu 'enkapsulasi'?", "id": "Enkapsulasi adalah proses akhir di mana die yang telah terhubung ditutup dengan senyawa cetak plastik atau keramik untuk melindunginya dari lingkungan." },
  { "en": "Apa itu 'reliability testing'?", "id": "Ini adalah serangkaian pengujian stres (seperti siklus suhu, kelembaban, tegangan tinggi) yang dilakukan pada sampel transistor untuk memastikan mereka dapat bertahan lama di kondisi nyata." },
  { "en": "Apa itu 'MTTF' (Mean Time To Failure)?", "id": "MTTF adalah metrik statistik yang memprediksi waktu rata-rata operasi sebelum sebuah komponen (seperti transistor) diperkirakan akan gagal." },
  { "en": "Apa itu 'fit' (Failures In Time)?", "id": "FIT adalah satuan tingkat kegagalan, yang didefinisikan sebagai satu kegagalan per miliar jam operasi perangkat. Nilai FIT yang lebih rendah berarti komponen lebih andal." },
  { "en": "Apa itu 'derating'?", "id": "Derating adalah praktik rekayasa yang baik di mana komponen sengaja dioperasikan jauh di bawah batas maksimumnya (misalnya, hanya 50% dari daya maksimumnya) untuk meningkatkan keandalan dan umur panjang." },
  { "en": "Apa itu 'transistor sebagai sumber noise'?", "id": "Dalam aplikasi presisi, transistor itu sendiri adalah sumber noise (thermal, shot, flicker). Memilih transistor 'low-noise' sangat penting untuk menguatkan sinyal yang sangat lemah." },
  { "en": "Apa itu 'titik 1dB kompresi' (P1dB)?", "id": "P1dB adalah ukuran linearitas penguat, yang menunjukkan titik daya output di mana gain telah turun sebesar 1 dB dari gain sinyal kecilnya. Ini menandai awal dari saturasi." },
  { "en": "Apa itu 'efisiensi drain' pada penguat RF?", "id": "Ini adalah ukuran seberapa efisien penguat mengubah daya DC dari catu daya menjadi daya output RF. Ini adalah parameter kunci untuk pemancar." },
  { "en": "Apa itu 'linearizer'?", "id": "Linearizer adalah sirkuit tambahan yang digunakan bersama penguat RF untuk memperbaiki non-linearitasnya, seringkali dengan menerapkan 'pre-distorsi' pada sinyal input." },
  { "en": "Apa itu 'penguat terdistribusi' (distributed amplifier)?", "id": "Ini adalah arsitektur penguat gelombang mikro yang menggunakan beberapa transistor yang dipisahkan oleh jalur transmisi untuk mencapai bandwidth yang sangat lebar." },
  { "en": "Apa itu 'efek termoelektrik'?", "id": "Ini adalah fenomena di mana perbedaan suhu di sepanjang konduktor dapat menghasilkan tegangan (efek Seebeck), yang dapat menjadi sumber kesalahan pada sirkuit DC presisi tinggi." },
  { "en": "Apa itu 'pemodelan perilaku' (behavioral modeling)?", "id": "Untuk sirkuit yang sangat kompleks, daripada memodelkan setiap transistor, para insinyur menggunakan model perilaku yang hanya mendeskripsikan fungsi input-output dari blok sirkuit." },
  { "en": "Apa itu 'verifikasi formal'?", "id": "Ini adalah metode menggunakan bukti matematis untuk memverifikasi bahwa desain sebuah sirkuit digital (yang terbuat dari transistor) memenuhi spesifikasinya, penting untuk CPU yang bebas bug." },
  { "en": "Bagaimana transistor memungkinkan 'Internet of Things' (IoT)?", "id": "IoT bergantung pada sensor dan radio berdaya sangat rendah, kecil, dan murah, yang semuanya dimungkinkan oleh kemajuan teknologi transistor CMOS." },
  { "en": "Apa itu 'energy harvesting'?", "id": "Energy harvesting adalah teknologi yang mengumpulkan energi dari lingkungan (seperti cahaya, getaran, atau panas) untuk memberi daya pada sirkuit berdaya sangat rendah, seringkali menggunakan transistor yang sangat efisien." },
  { "en": "Apa itu 'komputasi neuromorfik'?", "id": "Ini adalah bidang penelitian yang mencoba membangun chip komputer yang meniru arsitektur otak manusia, menggunakan transistor untuk membuat 'neuron' dan 'sinapsis' buatan." },
  { "en": "Apa itu 'transistor terowongan resonan' (resonant-tunneling transistor)?", "id": "Ini adalah perangkat kuantum yang menunjukkan resistansi diferensial negatif, memungkinkannya untuk digunakan dalam osilator frekuensi sangat tinggi dan sirkuit logika multi-nilai." },
  { "en": "Apa itu 'spintronics'?", "id": "Spintronics adalah teknologi masa depan yang mengeksploitasi 'spin' elektron (selain muatannya) untuk memproses dan menyimpan informasi, berpotensi mengarah pada perangkat yang lebih cepat dan lebih efisien." },
  { "en": "Apa itu 'graphene transistor'?", "id": "Graphene, lapisan karbon setebal satu atom, memiliki mobilitas elektron yang luar biasa tinggi, menjadikannya kandidat yang menjanjikan untuk transistor generasi berikutnya yang sangat cepat." },
  { "en": "Apa tantangan utama dari 'graphene transistor'?", "id": "Tantangan utamanya adalah graphene secara alami tidak memiliki 'celah pita', sehingga sulit untuk 'mematikannya' sepenuhnya, yang penting untuk aplikasi logika." },
  { "en": "Apa itu 'cryogenic electronics'?", "id": "Ini adalah studi tentang sirkuit elektronik pada suhu yang sangat rendah (mendekati nol absolut). Pada suhu ini, beberapa transistor menunjukkan perilaku yang lebih baik dan noise yang lebih rendah." },
  { "en": "Di mana elektronik kriogenik digunakan?", "id": "Digunakan untuk membaca sinyal yang sangat lemah dari komputer kuantum dan teleskop radio astronomi." },
  { "en": "Apa itu 'single-photon avalanche diode' (SPAD)?", "id": "SPAD adalah fotodetektor berbasis transistor yang dioperasikan dalam mode Geiger, sangat sensitif sehingga dapat mendeteksi satu foton cahaya." },
  { "en": "Di mana SPAD digunakan?", "id": "SPAD digunakan dalam aplikasi canggih seperti pencitraan medis (PET), LiDAR untuk mobil otonom, dan komunikasi kuantum." },
  { "en": "Apa itu 'transistor organik' (OFET)?", "id": "OFET (Organic Field-Effect Transistor) menggunakan bahan semikonduktor berbasis karbon (organik), yang memungkinkan pembuatan perangkat elektronik yang fleksibel, dapat diregangkan, dan transparan." },
  { "en": "Apa aplikasi potensial dari transistor organik?", "id": "Layar yang dapat digulung, sensor medis yang dapat dikenakan seperti kulit, dan label identifikasi frekuensi radio (RFID) yang murah." },
  { "en": "Apa itu 'CMOS Image Sensor' (CIS)?", "id": "Ini adalah teknologi sensor gambar yang digunakan di hampir semua kamera ponsel dan webcam. Setiap piksel berisi fotodioda dan sirkuit penguat berbasis transistor CMOS." },
  { "en": "Apa keunggulan sensor gambar CMOS dibandingkan CCD?", "id": "Sensor CMOS mengintegrasikan lebih banyak fungsi pada chip yang sama, mengkonsumsi lebih sedikit daya, dan lebih murah untuk diproduksi daripada sensor CCD (Charge-Coupled Device) yang lebih tua." },
  { "en": "Apa itu 'piksel aktif' (active pixel sensor)?", "id": "Ini adalah desain piksel pada sensor CMOS di mana setiap piksel memiliki penguat transistornya sendiri untuk membaca dan memperkuat sinyal sebelum dikirim keluar, yang mengurangi noise." },
  { "en": "Apa itu 'rolling shutter'?", "id": "Rolling shutter adalah artefak pada banyak sensor CMOS di mana gambar dibaca baris per baris, yang dapat menyebabkan distorsi pada objek yang bergerak cepat." },
  { "en": "Apa itu 'global shutter'?", "id": "Global shutter adalah fitur pada sensor gambar yang lebih canggih di mana seluruh piksel diekspos dan dibaca secara bersamaan, menghilangkan distorsi gerakan. Ini memerlukan transistor tambahan di setiap piksel." },
  { "en": "Apa itu 'back-illuminated sensor' (BSI)?", "id": "BSI adalah arsitektur sensor CMOS di mana kabel logam dipindahkan ke belakang fotodioda, memungkinkan lebih banyak cahaya untuk mencapai area sensitif, meningkatkan kinerja dalam cahaya rendah." },
  { "en": "Apa itu 'stacked sensor'?", "id": "Stacked sensor adalah teknologi canggih di mana chip sensor gambar dan chip pemrosesan logika (keduanya terbuat dari transistor) dibuat secara terpisah dan kemudian ditumpuk bersama, memungkinkan kinerja yang lebih tinggi." },
  { "en": "Apa peran transistor dalam 'memristor'?", "id": "Memristor adalah komponen pasif keempat yang teoretis. Transistor sering digunakan dalam sirkuit untuk meniru atau memodelkan perilakunya untuk penelitian dalam komputasi neuromorfik." },
  { "en": "Apa itu 'resistansi diferensial negatif'?", "id": "Ini adalah properti yang tidak biasa di mana peningkatan tegangan pada suatu komponen justru menyebabkan penurunan arus. Perangkat seperti dioda terowongan dan dioda Gunn menunjukkan ini dan dapat digunakan sebagai osilator." },
  { "en": "Secara keseluruhan, apa dampak terbesar transistor bagi masyarakat?", "id": "Transistor memungkinkan revolusi digital, yang mengarah ke komputer pribadi, internet, ponsel, dan hampir semua teknologi modern yang kita gunakan sehari-hari, mengubah cara kita bekerja, berkomunikasi, dan hidup." },
  { "en": "Jelaskan hubungan antara Hukum Moore, disipasi daya, dan kebutuhan akan tegangan operasi rendah.", "id": "Hukum Moore berarti lebih banyak transistor per chip. Lebih banyak transistor berarti lebih banyak panas (disipasi daya). Menurunkan tegangan operasi adalah cara paling efektif untuk mengurangi daya yang dikonsumsi setiap transistor, memungkinkan lebih banyak transistor tanpa kepanasan." },
  { "en": "Mengapa MOSFET seringkali lebih disukai daripada BJT untuk dikendalikan langsung oleh mikrokontroler?", "id": "Karena MOSFET dikontrol oleh tegangan, ia tidak 'menarik' banyak arus dari pin mikrokontroler. BJT yang dikontrol arus akan membutuhkan resistor basis dan menarik arus yang lebih signifikan." },
  { "en": "Kapan Anda akan memilih menggunakan optocoupler daripada hanya transistor untuk menyalakan relay?", "id": "Ketika ada kebutuhan mutlak untuk isolasi elektrik, misalnya untuk melindungi mikrokontroler yang sensitif dari 'noise' listrik yang dihasilkan oleh motor atau sirkuit bertegangan tinggi." },
  { "en": "Apa analogi sederhana untuk arsitektur 'FinFET'?", "id": "Bayangkan gerbang tol biasa di mana palang hanya ada di atas mobil. FinFET seperti gerbang tol di mana palangnya mengelilingi mobil dari tiga sisi (kiri, kanan, atas), memberikan kontrol yang jauh lebih baik untuk menghentikan 'kebocoran' mobil." },
  { "en": "Jika sebuah transistor adalah pipa air, apa itu 'celah pita' (bandgap)?", "id": "Celah pita adalah kekuatan material pipa itu sendiri. Pipa yang terbuat dari material yang sangat kuat (celah pita lebar seperti SiC atau GaN) dapat menahan tekanan air (tegangan) yang jauh lebih tinggi sebelum pecah (breakdown)." },
  { "en": "Anda merakit penguat, tetapi outputnya diam. Setelah memeriksa catu daya, apa langkah selanjutnya yang paling logis?", "id": "Ukur tegangan bias DC pada setiap terminal transistor. Tanpa tegangan bias yang benar, transistor tidak akan pernah bisa menguatkan sinyal, bahkan jika sinyal input ada." },
  { "en": "Mengapa negara-negara berinvestasi besar dalam membangun pabrik semikonduktor ('fab')?", "id": "Karena semikonduktor (dan transistor di dalamnya) adalah teknologi dasar yang menggerakkan hampir semua industri modern, mulai dari komputasi, komunikasi, otomotif, hingga pertahanan." },
  { "en": "Apa itu 'resistansi kontak' dan bagaimana hubungannya dengan transistor?", "id": "Resistansi kontak adalah hambatan kecil yang muncul di titik di mana logam terhubung ke semikonduktor. Dalam transistor modern yang sangat kecil, resistansi ini menjadi faktor pembatas kinerja yang signifikan." },
  { "en": "Apa perbedaan antara 'penguat sinyal kecil' dan 'penguat sinyal besar'?", "id": "'Sinyal kecil' berarti sinyal AC cukup kecil sehingga kita bisa menganggap transistor linier. 'Sinyal besar' berarti sinyalnya cukup besar sehingga kita harus mempertimbangkan non-linearitas transistor di seluruh rentang operasinya." },
  { "en": "Apa itu 'gettering' dalam pembuatan silikon?", "id": "Gettering adalah proses untuk menjebak dan menonaktifkan pengotor yang tidak diinginkan di dalam kristal silikon dengan memindahkannya ke lokasi di mana mereka tidak akan mengganggu area aktif transistor." },
  { "en": "Mengapa transistor bipolar kadang-kadang lebih disukai untuk penguat audio high-fidelity?", "id": "Beberapa perancang audio merasa bahwa karakteristik transfer BJT yang eksponensial menghasilkan harmonik yang terdengar lebih 'musikal' atau menyenangkan saat terjadi distorsi ringan, dibandingkan dengan karakteristik kuadratik FET." },
  { "en": "Apa itu 'efek terowongan pita-ke-pita' (Band-to-Band Tunneling - BTBT)?", "id": "Ini adalah mekanisme arus bocor pada MOSFET di mana elektron dapat 'menerowong' langsung dari pita valensi ke pita konduksi di bawah pengaruh medan listrik yang kuat di daerah drain." },
  { "en": "Apa itu 'antena gerbang' (gate antenna effect) dalam manufaktur IC?", "id": "Selama proses etsa plasma, jejak polisilikon yang panjang dapat bertindak seperti antena, mengumpulkan muatan yang dapat menyebabkan kerusakan tegangan tinggi pada isolator gerbang yang terhubung." },
  { "en": "Bagaimana para perancang IC mengatasi 'efek antena'?", "id": "Dengan menambahkan 'lompatan dioda' (diode jumper) di dekat gerbang atau dengan membatasi panjang jejak logam yang terhubung ke gerbang selama proses manufaktur." },
  { "en": "Apa peran transistor dalam 'sirkuit penyearah presisi' (precision rectifier)?", "id": "Dalam penyearah presisi (atau 'superdiode'), transistor atau op-amp digunakan untuk menghilangkan penurunan tegangan maju dioda, memungkinkan penyearahan sinyal yang sangat kecil dengan akurat." },
  { "en": "Jelaskan konsep 'stabilitas tak bersyarat' (unconditional stability) pada penguat RF.", "id": "Ini berarti penguat akan tetap stabil dan tidak akan berosilasi tidak peduli impedansi beban atau sumber apa pun yang terhubung padanya. Ini adalah tujuan desain yang sangat diinginkan." },
  { "en": "Apa itu 'lingkaran stabilitas' (stability circles) pada Smith Chart?", "id": "Ini adalah alat grafis yang digunakan oleh insinyur RF untuk menentukan rentang impedansi sumber atau beban yang akan menyebabkan penguat transistor menjadi tidak stabil (berosilasi)." },
  { "en": "Apa itu 'penguat dengan noise rendah' (Low-Noise Amplifier - LNA)?", "id": "LNA adalah penguat pertama dalam rantai penerima (misalnya, di antena GPS atau radio). Tugas utamanya adalah menguatkan sinyal yang sangat lemah sambil menambahkan noise sekecil mungkin." },
  { "en": "Apa karakteristik kunci dari transistor yang digunakan dalam LNA?", "id": "Transistor tersebut harus memiliki 'angka kebisingan' (noise figure) yang sangat rendah dan gain yang cukup tinggi pada frekuensi operasi." },
  { "en": "Apa itu 'penguat daya' (Power Amplifier - PA)?", "id": "PA adalah penguat terakhir dalam rantai pemancar. Tugasnya adalah mengambil sinyal yang sudah diproses dan meningkatkannya ke tingkat daya tinggi yang dibutuhkan untuk transmisi melalui antena." },
  { "en": "Apa karakteristik kunci dari transistor yang digunakan dalam PA?", "id": "Transistor tersebut harus mampu menangani daya tinggi, sangat efisien untuk meminimalkan panas, dan cukup linier untuk menghindari distorsi sinyal yang dipancarkan." },
  { "en": "Apa hubungan antara 'bandwidth' dan 'waktu naik' (rise time) sebuah sinyal?", "id": "Hubungannya berbanding terbalik. Sinyal dengan waktu naik yang lebih cepat (lebih tajam) memerlukan penguat dengan bandwidth yang lebih lebar untuk mereproduksinya dengan akurat." },
  { "en": "Apa itu 'transistor sebagai pelindung tegangan lebih' (overvoltage protection)?", "id": "Sebuah BJT atau MOSFET dapat digunakan untuk memonitor tegangan. Jika tegangan melebihi ambang batas, transistor akan aktif dan memicu mekanisme perlindungan, seperti mematikan daya atau mengaktifkan crowbar." },
  { "en": "Apa itu 'soft-start circuit'?", "id": "Ini adalah sirkuit, seringkali menggunakan transistor, yang memungkinkan daya naik secara bertahap saat perangkat dinyalakan, untuk mencegah lonjakan arus besar (inrush current) yang dapat merusak komponen." },
  { "en": "Mengapa ada lonjakan arus (inrush current) saat menyalakan perangkat?", "id": "Karena kapasitor dalam catu daya pada awalnya dalam keadaan kosong dan bertindak seperti korsleting sesaat saat pertama kali terhubung ke sumber daya." },
  { "en": "Apa itu 'isolator digital'?", "id": "Ini adalah IC modern yang menyediakan isolasi galvanik untuk sinyal digital berkecepatan tinggi, seringkali menggunakan teknologi kapasitif atau magnetik kecil, menggantikan optocoupler berbasis transistor dalam banyak aplikasi." },
  { "en": "Apa itu 'penguat diferensial sepenuhnya' (fully differential amplifier)?", "id": "Ini adalah penguat yang memiliki input diferensial dan output diferensial. Ini menawarkan penolakan noise yang superior dan sangat umum dalam sistem komunikasi modern." },
  { "en": "Apa itu 'tegangan mode-umum input' (input common-mode voltage)?", "id": "Ini adalah level tegangan rata-rata dari kedua input pada penguat diferensial. Setiap penguat memiliki rentang tegangan mode-umum yang dapat diterimanya." },
  { "en": "Apa itu 'rentang ayunan output' (output swing range)?", "id": "Ini adalah rentang tegangan di mana output penguat dapat berayun, yang selalu dibatasi oleh tegangan catu daya positif dan negatifnya." },
  { "en": "Apa itu 'rail-to-rail amplifier'?", "id": "Ini adalah jenis penguat khusus (biasanya op-amp) yang dirancang agar ayunan outputnya dapat mencapai sangat dekat dengan level tegangan catu daya positif dan negatif ('rel' daya)." },
  { "en": "Apa itu 'efek popcorn' pada kemasan plastik?", "id": "Selama proses penyolderan reflow, kelembaban yang terperangkap di dalam kemasan plastik komponen dapat menguap dengan cepat, menyebabkan kemasan retak atau 'meletup' seperti popcorn." },
  { "en": "Apa itu 'Tingkat Sensitivitas Kelembaban' (Moisture Sensitivity Level - MSL)?", "id": "MSL adalah standar yang mengklasifikasikan seberapa rentan sebuah komponen (seperti IC atau transistor) terhadap kerusakan akibat kelembaban selama proses penyolderan." },
  { "en": "Apa itu 'baking' komponen elektronik?", "id": "Ini adalah proses memanaskan komponen pada suhu sedang dalam oven khusus untuk menghilangkan kelembaban yang terserap sebelum proses penyolderan untuk mencegah efek popcorn." },
  { "en": "Apa itu 'interkoneksi' dalam sebuah IC?", "id": "Interkoneksi adalah jaringan kabel logam (biasanya tembaga atau aluminium) yang sangat kompleks yang menghubungkan jutaan atau miliaran transistor di dalam sebuah chip." },
  { "en": "Apa itu 'keterlambatan RC' (RC delay)?", "id": "Saat ukuran transistor menyusut, penundaan dari kabel interkoneksi (yang memiliki resistansi R dan kapasitansi C) menjadi faktor pembatas utama kecepatan chip, seringkali lebih signifikan daripada penundaan transistor itu sendiri." },
  { "en": "Mengapa tembaga menggantikan aluminium untuk interkoneksi di IC canggih?", "id": "Tembaga memiliki resistivitas yang lebih rendah daripada aluminium, yang membantu mengurangi keterlambatan RC dan memungkinkan arus yang lebih tinggi, sehingga meningkatkan kinerja chip." },
  { "en": "Apa itu 'dielektrik low-k'?", "id": "Ini adalah material isolator dengan konstanta dielektrik rendah yang digunakan di antara kabel interkoneksi untuk mengurangi kapasitansi parasit di antara mereka, yang selanjutnya mengurangi keterlambatan RC." },
  { "en": "Apa itu 'electromigration'?", "id": "Electromigration adalah pergerakan atom logam dalam sebuah konduktor akibat momentum dari elektron yang mengalir. Seiring waktu, ini dapat menyebabkan 'void' (kekosongan) atau 'hillock' (bukit), yang menyebabkan sirkuit gagal." },
  { "en": "Faktor apa yang mempercepat electromigration?", "id": "Kepadatan arus yang tinggi dan suhu yang tinggi adalah dua faktor utama yang mempercepat kegagalan akibat electromigration." },
  { "en": "Apa itu 'stress migration'?", "id": "Ini adalah mekanisme kegagalan lain di mana tegangan mekanis internal dalam lapisan logam (akibat perbedaan koefisien ekspansi termal) dapat menyebabkan pergerakan atom dan kegagalan sirkuit." },
  { "en": "Apa itu 'Time-Dependent Dielectric Breakdown' (TDDB)?", "id": "Ini adalah mekanisme kegagalan di mana isolator gerbang MOSFET secara bertahap 'aus' dan akhirnya rusak setelah beroperasi pada tegangan tinggi untuk waktu yang lama." },
  { "en": "Apa itu 'akselerasi pengujian' (accelerated testing)?", "id": "Ini adalah proses menguji komponen pada kondisi yang jauh lebih berat (tegangan, suhu, kelembaban yang lebih tinggi) daripada kondisi operasi normal untuk memprediksi umur panjangnya dalam waktu yang lebih singkat." },
  { "en": "Apa itu 'model Arrhenius'?", "id": "Ini adalah persamaan yang sering digunakan dalam pengujian keandalan untuk menghubungkan tingkat kegagalan suatu komponen dengan suhu operasi. Aturan umumnya adalah setiap kenaikan 10°C dapat mengurangi umur komponen hingga setengahnya." },
  { "en": "Apa itu 'sirkuit terintegrasi 3D' (3D IC)?", "id": "3D IC adalah teknologi di mana beberapa die semikonduktor ditumpuk secara vertikal dan dihubungkan bersama, memungkinkan kepadatan yang lebih tinggi dan jalur sinyal yang lebih pendek." },
  { "en": "Apa itu 'Through-Silicon Via' (TSV)?", "id": "TSV adalah koneksi listrik vertikal yang menembus seluruh wafer atau die silikon, yang merupakan teknologi kunci untuk memungkinkan integrasi 3D IC." },
  { "en": "Apa itu 'chiplet'?", "id": "Chiplet adalah pendekatan desain di mana sebuah prosesor kompleks dibuat dengan menggabungkan beberapa die yang lebih kecil dan terspesialisasi (chiplet) bersama dalam satu kemasan, yang bisa lebih hemat biaya daripada membuat satu die monolitik besar." },
  { "en": "Apa itu 'system-in-package' (SiP)?", "id": "SiP adalah satu kemasan komponen yang berisi beberapa chip IC dan komponen pasif yang membentuk seluruh sistem atau subsistem. Ini berbeda dari System-on-Chip (SoC) di mana semuanya terintegrasi pada satu die." },
  { "en": "Apa itu 'pengujian fungsional' vs 'pengujian struktural'?", "id": "Pengujian fungsional memeriksa apakah chip melakukan apa yang seharusnya dilakukan (misalnya, menjalankan program). Pengujian struktural memeriksa apakah setiap transistor dan gerbang di dalamnya berfungsi dengan benar, bahkan jika fungsinya tidak digunakan secara normal." },
  { "en": "Apa itu 'scan chain'?", "id": "Scan chain adalah teknik desain untuk pengujian (design-for-test) di mana flip-flop di dalam chip dapat dihubungkan menjadi rantai geser panjang, memungkinkan status internal chip dibaca dan dikontrol dengan mudah selama pengujian." },
  { "en": "Apa itu 'Built-In Self-Test' (BIST)?", "id": "BIST adalah sirkuit di dalam sebuah chip yang dirancang untuk menguji bagian lain dari chip itu sendiri, mengurangi ketergantungan pada peralatan pengujian eksternal yang mahal." },
  { "en": "Apa itu 'JTAG' (Joint Test Action Group)?", "id": "JTAG adalah standar industri untuk antarmuka pengujian on-chip (sering disebut 'boundary scan') yang memungkinkan pengujian, pemrograman, dan debugging sirkuit pada tingkat papan." },
  { "en": "Apa itu 'power gating'?", "id": "Power gating adalah teknik manajemen daya di mana transistor 'header' atau 'footer' digunakan sebagai saklar untuk mematikan sepenuhnya catu daya ke blok sirkuit yang tidak digunakan, mengurangi arus bocor secara signifikan." },
  { "en": "Apa itu 'dynamic voltage and frequency scaling' (DVFS)?", "id": "DVFS adalah teknik hemat daya di mana tegangan dan frekuensi clock prosesor disesuaikan secara dinamis berdasarkan beban kerja saat ini. Beban ringan menggunakan tegangan/frekuensi rendah, beban berat menggunakan tegangan/frekuensi tinggi." },
  { "en": "Apa itu 'clock gating'?", "id": "Ini adalah teknik hemat daya populer di mana sinyal clock ke bagian-bagian sirkuit yang tidak aktif untuk sementara dimatikan, mencegah transistor di sana untuk beralih secara tidak perlu." },
  { "en": "Apa itu 'resistansi diferensial'?", "id": "Ini adalah resistansi efektif antara dua titik dalam sirkuit yang berubah tergantung pada level sinyal atau arus. Sambungan dioda adalah contohnya." },
  { "en": "Apa itu 'transistor sebagai pengganda frekuensi' (frequency multiplier)?", "id": "Dengan mengoperasikan transistor dalam mode yang sangat non-linier, ia akan menghasilkan banyak sinyal harmonik. Sebuah filter kemudian dapat digunakan untuk memilih harmonik yang diinginkan (misalnya, dua atau tiga kali frekuensi input)." },
  { "en": "Apa itu 'mixer' dalam radio?", "id": "Mixer adalah sirkuit non-linier (menggunakan transistor atau dioda) yang menggabungkan dua frekuensi (sinyal RF yang masuk dan sinyal dari osilator lokal) untuk menghasilkan frekuensi baru (frekuensi antara atau IF)." },
  { "en": "Mengapa proses 'mixing' ke frekuensi antara (IF) penting?", "id": "Karena lebih mudah untuk merancang penguat dan filter yang stabil dan selektif pada satu frekuensi tetap (IF) daripada mencoba melakukannya pada frekuensi RF yang dapat berubah-ubah." },
  { "en": "Apa itu 'superheterodyne receiver'?", "id": "Ini adalah arsitektur penerima radio yang paling umum, yang didasarkan pada prinsip mengubah frekuensi RF yang masuk menjadi frekuensi IF yang lebih rendah dan tetap untuk diproses lebih lanjut." },
  { "en": "Apa itu 'direct conversion receiver' (zero-IF)?", "id": "Ini adalah arsitektur penerima modern di mana sinyal RF langsung diubah menjadi sinyal baseband (IF = 0 Hz), menghilangkan kebutuhan akan filter IF yang besar dan mahal, ideal untuk integrasi chip." },
  { "en": "Apa itu 'homojunction' vs 'heterojunction'?", "id": "Homojunction adalah sambungan antara dua semikonduktor dari jenis yang sama tetapi doping berbeda (misalnya, silikon N dan silikon P). Heterojunction adalah sambungan antara dua bahan semikonduktor yang berbeda (misalnya, Gallium Arsenide dan Aluminium Gallium Arsenide)." },
  { "en": "Apa keuntungan dari 'heterojunction'?", "id": "Heterojunction memungkinkan rekayasa 'celah pita', menciptakan penghalang atau sumur potensial untuk mengontrol pergerakan elektron dengan cara yang tidak mungkin dilakukan dengan homojunction, yang mengarah pada perangkat seperti HBT dan HEMT." },
  { "en": "Apa itu '2D electron gas' (2DEG)?", "id": "2DEG adalah lapisan elektron yang sangat tipis dan sangat mobile yang terbentuk pada antarmuka heterojunction (seperti pada HEMT). Ini memungkinkan konduksi arus yang sangat cepat dengan sedikit hambatan." },
  { "en": "Apa itu 'transistor efek medan kuantum' (quantum-effect transistor)?", "id": "Ini adalah istilah umum untuk transistor yang operasinya didasarkan pada prinsip-prinsip mekanika kuantum yang berbeda dari transistor klasik, seperti TFET atau SET." },
  { "en": "Apa itu 'material 2D' selain graphene?", "id": "Material lain seperti 'molybdenum disulfide' (MoS2) sedang diteliti untuk transistor masa depan karena mereka, tidak seperti graphene, memiliki celah pita alami." },
  { "en": "Apa itu 'pendinginan termoelektrik' (Peltier effect)?", "id": "Ini adalah penciptaan perbedaan suhu dengan menerapkan tegangan pada sambungan dua material yang berbeda. Meskipun tidak efisien, ini digunakan untuk pendinginan presisi pada beberapa komponen optik berbasis transistor." },
  { "en": "Apa itu 'transistor fotonik'?", "id": "Ini adalah perangkat eksperimental yang berfungsi sebagai saklar atau penguat untuk sinyal cahaya (foton), bukan sinyal listrik (elektron), berpotensi untuk komputasi optik masa depan." },
  { "en": "Apa itu 'kalibrasi self-healing' pada IC?", "id": "Ini adalah konsep di mana sirkuit di dalam chip dapat mendeteksi perubahan atau degradasi pada transistornya sendiri dari waktu ke waktu dan secara otomatis menyesuaikan bias atau waktu untuk mengkompensasi perubahan tersebut." },
  { "en": "Apa itu 'komputasi perkiraan' (approximate computing)?", "id": "Ini adalah paradigma di mana untuk aplikasi tertentu (seperti pemrosesan gambar atau AI), hasil yang sedikit tidak akurat dapat diterima jika itu berarti penghematan daya atau kecepatan yang signifikan, memungkinkan transistor untuk dijalankan dengan tegangan lebih rendah." },
  { "en": "Apa itu 'sirkuit asinkron'?", "id": "Ini adalah sirkuit digital yang tidak bergantung pada sinyal clock global untuk sinkronisasi. Mereka dapat lebih hemat daya tetapi jauh lebih sulit untuk dirancang daripada sirkuit sinkron standar." },
  { "en": "Bagaimana transistor digunakan dalam sel memori 'phase-change' (PCM)?", "id": "Dalam PCM, transistor digunakan untuk mengalirkan pulsa arus melalui material khusus untuk mengubahnya antara keadaan amorf (resistansi tinggi) dan kristal (resistansi rendah), untuk menyimpan bit 0 atau 1." },
  { "en": "Apa itu 'magnetoresistive RAM' (MRAM)?", "id": "MRAM menyimpan data menggunakan keadaan magnetik, bukan muatan listrik. Transistor digunakan sebagai saklar untuk membaca dan menulis keadaan magnetik dari 'magnetic tunnel junction' (MTJ)." },
  { "en": "Apa keuntungan MRAM?", "id": "MRAM bersifat non-volatile (seperti flash), secepat SRAM, dan memiliki daya tahan tulis yang hampir tak terbatas, menjadikannya kandidat kuat untuk memori universal masa depan." },
  { "en": "Apa itu 'resistive RAM' (ReRAM)?", "id": "ReRAM bekerja dengan menerapkan tegangan melalui material dielektrik untuk membuat atau memutuskan filamen konduktif kecil, mengubah resistansinya antara keadaan tinggi dan rendah untuk menyimpan data." },
  { "en": "Apa itu 'komputasi in-memory'?", "id": "Ini adalah arsitektur baru di mana beberapa komputasi sederhana dilakukan di dalam unit memori itu sendiri (menggunakan sel memori seperti ReRAM atau MRAM), mengurangi kebutuhan untuk memindahkan data bolak-balik ke CPU, yang sangat menghemat energi." },
  { "en": "Apa itu 'noise shaping'?", "id": "Dalam konverter data seperti delta-sigma ADC, noise shaping adalah teknik yang secara cerdas 'mendorong' noise kuantisasi keluar dari pita frekuensi yang diminati ke frekuensi yang lebih tinggi, di mana ia dapat dengan mudah disaring." },
  { "en": "Apa itu 'delta-sigma modulator'?", "id": "Ini adalah inti dari ADC resolusi tinggi modern, yang menggunakan oversampling, komparator 1-bit (berbasis transistor), dan umpan balik untuk mencapai presisi yang sangat tinggi." },
  { "en": "Apa itu 'gain-scheduling'?", "id": "Ini adalah teknik kontrol canggih di mana parameter dari sebuah pengontrol (yang diimplementasikan dengan transistor) diubah secara dinamis saat kondisi sistem berubah untuk mempertahankan kinerja yang optimal." },
  { "en": "Apa itu 'sistem mikro-elektromekanis' (MEMS)?", "id": "MEMS adalah perangkat miniatur yang menggabungkan elemen mekanis (seperti pegas, roda gigi, membran) dan elektronik pada chip silikon yang sama. Contohnya adalah akselerometer di ponsel Anda." },
  { "en": "Apa peran transistor dalam MEMS?", "id": "Transistor pada chip yang sama digunakan untuk mengontrol aktuator MEMS dan untuk membaca serta menguatkan sinyal dari sensor MEMS." },
  { "en": "Apa itu 'lab-on-a-chip'?", "id": "Ini adalah perangkat MEMS canggih yang mengintegrasikan beberapa fungsi laboratorium (seperti pencampuran cairan, pemanasan, deteksi) pada satu chip tunggal, seringkali untuk analisis medis atau biologis." },
  { "en": "Bagaimana transistor memungkinkan 'artificial intelligence' (AI) di perangkat tepi (edge AI)?", "id": "Perkembangan transistor yang sangat efisien memungkinkan pembuatan prosesor khusus (akselerator AI) yang dapat menjalankan model AI kompleks secara langsung di perangkat seperti ponsel atau kamera, tanpa perlu terhubung ke cloud." },
  { "en": "Apa itu 'tensor processing unit' (TPU)?", "id": "TPU adalah jenis akselerator AI yang dikembangkan oleh Google, yang arsitekturnya (terdiri dari jutaan transistor) dioptimalkan secara khusus untuk jenis perkalian matriks yang umum dalam jaringan saraf tiruan." },
  { "en": "Apa itu 'radiasi pengion' dan bagaimana pengaruhnya pada transistor?", "id": "Radiasi pengion (seperti sinar gamma atau partikel alfa) dapat menciptakan pasangan elektron-hole di dalam semikonduktor, menyebabkan 'single-event upset' (bit terbalik) atau bahkan kerusakan permanen, menjadi perhatian besar di luar angkasa." },
  { "en": "Apa itu 'radiation hardening'?", "id": "Ini adalah serangkaian teknik desain dan manufaktur (seperti menggunakan substrat isolator, mengubah tata letak, dan menambahkan redundansi) untuk membuat transistor dan IC lebih tahan terhadap efek radiasi." },
  { "en": "Apa itu 'transistor bipolar heterojunction' (HBT)?", "id": "HBT adalah BJT berkinerja sangat tinggi yang menggunakan bahan berbeda untuk emitor dan basis (heterojunction), memungkinkan gain dan kecepatan yang lebih tinggi daripada BJT silikon biasa." },
  { "en": "Di mana HBT digunakan?", "id": "HBT sangat umum digunakan dalam penguat daya untuk ponsel dan sirkuit frekuensi radio (RF) lainnya." },
  { "en": "Apa itu 'silicon-germanium' (SiGe)?", "id": "SiGe adalah paduan yang memungkinkan rekayasa celah pita pada platform berbasis silikon. Transistor SiGe HBT menawarkan banyak kinerja dari semikonduktor majemuk tetapi dengan biaya yang lebih rendah dari manufaktur silikon." },
  { "en": "Apa itu 'white noise'?", "id": "White noise adalah sinyal acak yang memiliki intensitas yang sama di semua frekuensi, seperti suara 'ssshhh'. Noise termal pada dasarnya adalah white noise." },
  { "en": "Apa itu 'pink noise'?", "id": "Pink noise adalah sinyal acak yang energinya menurun pada frekuensi yang lebih tinggi, sering dianggap lebih 'alami' bagi telinga manusia. Flicker noise (1/f) adalah jenis pink noise." },
  { "en": "Apa itu 'transimpedance amplifier' (TIA)?", "id": "TIA adalah penguat yang mengubah sinyal input berupa arus menjadi sinyal output berupa tegangan. Ini adalah tahap pertama yang penting dalam penerima serat optik untuk menguatkan sinyal lemah dari fotodioda." },
  { "en": "Mengapa transistor terakhir akan dibuat?", "id": "Secara teoretis, miniaturisasi akan berhenti ketika fitur transistor mencapai ukuran beberapa atom, di mana efek kuantum yang tidak dapat diprediksi akan mengambil alih sepenuhnya dan model transistor klasik tidak lagi berlaku." },
  { "en": "Secara keseluruhan, jika transistor adalah sebuah kata, apa sinonim terbaiknya?", "id": "Sinonim terbaik untuk transistor mungkin adalah 'kontrol' atau 'pengungkit', karena ia memungkinkan sinyal kecil untuk mengontrol sinyal yang jauh lebih besar, yang merupakan dasar dari semua elektronik modern." },
  { "en": "Apa tiga penemuan terbesar yang dimungkinkan secara langsung oleh transistor?", "id": "Komputer pribadi, Internet (melalui infrastruktur komunikasi dan komputasi), dan telepon seluler." },
  { "en": "Jika Anda bisa mengubah satu parameter dari transistor ideal, mana yang akan memberikan dampak terbesar?", "id": "Menghilangkan disipasi daya (menjadi 100% efisien). Ini akan menyelesaikan masalah panas, yang merupakan batasan utama dalam komputasi modern." },
  { "en": "Jelaskan evolusi elektronik dalam tiga tahap.", "id": "Tahap 1: Tabung Vakum (besar, boros daya, rapuh). Tahap 2: Transistor Diskrit (kecil, efisien, andal, memungkinkan radio portabel). Tahap 3: Sirkuit Terpadu (jutaan/miliaran transistor pada satu chip, memungkinkan kompleksitas modern)." },
  { "en": "Anda membangun saklar super cepat untuk beban daya rendah. Anda pilih BJT atau MOSFET? Mengapa?", "id": "MOSFET, karena umumnya memiliki kecepatan switching yang lebih tinggi dan dikontrol oleh tegangan, sehingga lebih mudah dihubungkan ke logika digital." },
  { "en": "Anda membangun tahap input penguat audio sensitif. Anda pilih BJT atau JFET? Mengapa?", "id": "JFET, karena seringkali memiliki karakteristik noise yang lebih baik pada frekuensi audio dan impedansi input yang sangat tinggi, sehingga tidak membebani sumber sinyal." },
  { "en": "Gejala: Penguat audio terdistorsi bahkan pada volume rendah. Apa kemungkinan masalah pada bias transistor?", "id": "Titik-Q (titik operasi diam) kemungkinan besar telah bergeser dari tengah daerah aktif, terlalu dekat ke daerah cut-off atau saturasi." },
  { "en": "Alat ukur apa yang paling penting bagi teknisi yang bekerja dengan rangkaian transistor?", "id": "Multimeter digital (untuk mengukur tegangan DC, arus, dan pengujian dasar) dan osiloskop (untuk melihat bentuk gelombang sinyal secara real-time)." },
  { "en": "Mengapa memeriksa catu daya selalu menjadi langkah pertama dalam troubleshooting?", "id": "Karena tidak ada komponen aktif, termasuk transistor, yang dapat berfungsi dengan benar tanpa daya yang bersih, stabil, dan pada level tegangan yang tepat." },
  { "en": "Bagaimana transistor di dalam sensor gambar kamera mengubah cahaya menjadi data?", "id": "Setiap piksel memiliki fotodioda yang mengubah foton (cahaya) menjadi muatan listrik (elektron). Sirkuit transistor di piksel kemudian mengukur muatan ini dan mengubahnya menjadi level tegangan, yang kemudian didigitalkan." },
  { "en": "Saat Anda menyimpan file ke USB drive, peran apa yang dimainkan transistor?", "id": "Miliaran transistor 'gerbang-mengambang' (floating-gate) di dalam chip memori flash digunakan untuk menjebak atau melepaskan elektron, secara fisik menyimpan setiap bit data (0 atau 1) bahkan saat daya dimatikan." },
  { "en": "Jelaskan perbedaan antara 'arus bocor' dan 'arus basis' dengan analogi keran air.", "id": "Arus basis adalah energi yang Anda gunakan untuk memutar gagang keran. Arus bocor adalah tetesan air kecil yang terus keluar dari ujung keran bahkan saat keran sudah ditutup serapat mungkin." },
  { "en": "Apa itu 'semikonduktor' dalam istilah yang paling sederhana?", "id": "Bahan yang dapat bertindak sebagai penghambat (isolator) atau penghantar (konduktor) listrik, tergantung pada kondisi yang diberikan, memungkinkannya untuk berfungsi sebagai saklar." },
  { "en": "Definisikan transistor dalam satu kalimat yang mencakup kedua fungsi utamanya.", "id": "Transistor adalah komponen semikonduktor yang dapat berfungsi sebagai saklar yang dikontrol secara elektronik atau sebagai penguat untuk meningkatkan kekuatan sinyal listrik." },
  { "en": "Mana yang lebih sulit untuk 'dimatikan', JFET N-channel mode deplesi atau MOSFET N-channel mode enhancement?", "id": "JFET mode deplesi, karena secara alami ia 'ON' dan memerlukan tegangan negatif pada gerbangnya untuk mematikannya. MOSFET enhancement secara alami 'OFF'." },
  { "en": "Apa itu 'pembebanan bootstrap' (bootstrapping)?", "id": "Ini adalah teknik umpan balik cerdas di mana output dari suatu tahap digunakan untuk 'mengangkat' level tegangan inputnya sendiri, sering digunakan untuk menciptakan impedansi input yang terlihat sangat tinggi." },
  { "en": "Mengapa transistor yang sama dapat berfungsi sebagai saklar atau penguat?", "id": "Karena fungsinya ditentukan oleh bagaimana sirkuit di sekitarnya (terutama resistor bias) mengaturnya. Sirkuit bias menentukan apakah ia beroperasi di daerah ekstrim (cut-off/saturasi) atau di daerah tengah (aktif)." },
  { "en": "Apa itu 'stabilitas' dalam konteks penguat?", "id": "Stabilitas adalah kemampuan penguat untuk tidak berosilasi (menghasilkan sinyalnya sendiri) secara tidak sengaja. Umpan balik yang tidak terkontrol pada frekuensi tinggi adalah penyebab umum ketidakstabilan." },
  { "en": "Apa itu 'penguat video'?", "id": "Ini adalah jenis penguat sinyal lebar-pita (wide-bandwidth) yang dirancang khusus untuk menangani sinyal video analog, yang membentang dari frekuensi DC hingga beberapa megahertz." },
  { "en": "Apa peran transistor dalam 'voltage-to-frequency converter'?", "id": "Transistor digunakan untuk membangun osilator di mana frekuensinya dikontrol secara linier oleh tegangan input, mengubah level tegangan analog menjadi sinyal frekuensi digital." },
  { "en": "Apa itu 'pengganda Gilbert' (Gilbert cell)?", "id": "Ini adalah sirkuit inti yang digunakan dalam hampir semua mixer radio modern. Ini menggunakan beberapa transistor dalam konfigurasi diferensial untuk melakukan perkalian sinyal dengan presisi dan linearitas tinggi." },
  { "en": "Mengapa terkadang ada beberapa transistor kecil di dalam satu kemasan besar?", "id": "Ini mungkin transistor array, atau lebih mungkin, beberapa transistor kecil dihubungkan secara paralel di dalam kemasan untuk menangani arus yang lebih besar, dengan desain yang cermat untuk menyeimbangkan beban." },
  { "en": "Apa itu 'efek piezo-resistif'?", "id": "Ini adalah perubahan resistivitas suatu bahan saat mengalami tekanan mekanis. Ini adalah prinsip di balik beberapa jenis sensor tekanan berbasis semikonduktor." },
  { "en": "Apa itu 'efek termionik'?", "id": "Ini adalah emisi elektron dari permukaan yang dipanaskan. Ini adalah prinsip kerja tabung vakum. Transistor modern tidak mengandalkan ini, tetapi beberapa fenomena internalnya (seperti subthreshold conduction) bersifat termionik." },
  { "en": "Apa itu 'pendinginan pasif' vs 'pendinginan aktif' untuk transistor?", "id": "Pendinginan pasif hanya menggunakan heatsink dan aliran udara alami. Pendinginan aktif menggunakan cara-cara bertenaga seperti kipas atau sistem pendingin cair untuk membuang panas." },
  { "en": "Apa itu 'konduksi termal' vs 'konveksi termal'?", "id": "Konduksi adalah transfer panas melalui benda padat (dari chip ke heatsink). Konveksi adalah transfer panas melalui pergerakan fluida (dari heatsink ke udara)." },
  { "en": "Mengapa sirip pada heatsink dibuat vertikal?", "id": "Untuk memaksimalkan aliran udara konveksi alami. Udara panas yang naik dari bawah akan menarik udara dingin masuk, menciptakan sirkulasi udara yang mendinginkan." },
  { "en": "Apa itu 'resistansi parasit'?", "id": "Ini adalah resistansi yang tidak diinginkan tetapi ada secara alami di kaki komponen, jejak PCB, dan di dalam chip itu sendiri, yang dapat memengaruhi kinerja pada arus tinggi atau frekuensi tinggi." },
  { "en": "Apa itu 'induktansi parasit'?", "id": "Setiap kabel atau jejak PCB memiliki sedikit induktansi. Pada frekuensi yang sangat tinggi, induktansi ini bisa menjadi signifikan, menyebabkan 'ringing' dan membatasi kecepatan switching." },
  { "en": "Apa itu 'power integrity' (PI)?", "id": "PI adalah bidang analisis yang memastikan semua transistor di dalam IC menerima daya yang bersih dan stabil. Ini melibatkan desain jaringan distribusi daya yang cermat di dalam chip dan di papan sirkuit." },
  { "en": "Apa itu 'signal integrity' (SI)?", "id": "SI adalah bidang analisis yang memastikan sinyal mempertahankan bentuk dan waktunya saat berjalan dari satu titik ke titik lain. Ini melibatkan pengelolaan impedansi, refleksi, dan crosstalk." },
  { "en": "Apa itu 'crosstalk'?", "id": "Crosstalk adalah transfer energi sinyal yang tidak diinginkan dari satu jejak sirkuit ke jejak tetangganya melalui kapasitansi atau induktansi parasit." },
  { "en": "Apa itu 'transistor sebagai varistor'?", "id": "Dalam beberapa konfigurasi, terutama di sirkuit perlindungan, transistor dapat dirancang untuk bertindak seperti varistor (resistor yang tergantung tegangan) untuk menjepit lonjakan tegangan." },
  { "en": "Apa itu 'efek Proximity' dalam konduktor?", "id": "Pada frekuensi tinggi, arus dalam konduktor yang berdekatan cenderung mengalir di sisi yang berhadapan satu sama lain, yang dapat meningkatkan resistansi efektif." },
  { "en": "Apa itu 'efek kulit' (skin effect)?", "id": "Pada frekuensi tinggi, arus AC cenderung mengalir hanya di permukaan luar ('kulit') sebuah konduktor, bukan di seluruh penampangnya, yang juga meningkatkan resistansi." },
  { "en": "Bagaimana efek-efek ini memengaruhi desain sirkuit transistor frekuensi tinggi?", "id": "Para desainer harus menggunakan jejak PCB yang lebar dan pendek, ground plane yang solid, dan teknik tata letak yang cermat untuk meminimalkan efek-efek ini." },
  { "en": "Apa itu 'pemodelan kotak hitam' (black box modeling)?", "id": "Ini adalah pendekatan di mana perilaku sirkuit yang kompleks dimodelkan hanya berdasarkan respons outputnya terhadap berbagai input, tanpa mempertimbangkan apa yang sebenarnya ada di dalamnya." },
  { "en": "Apa itu 'pemodelan kotak abu-abu' (grey box modeling)?", "id": "Ini adalah model kompromi yang menggunakan beberapa pengetahuan tentang struktur internal sirkuit tetapi tidak memodelkan setiap transistor secara detail." },
  { "en": "Apa itu 'penguat audio kelas G dan H'?", "id": "Ini adalah evolusi dari penguat kelas AB yang menggunakan beberapa tingkat tegangan catu daya. Mereka beralih ke rel tegangan yang lebih tinggi hanya saat dibutuhkan untuk sinyal puncak, sehingga meningkatkan efisiensi." },
  { "en": "Jelaskan konsep 'selektivitas' pada penerima radio.", "id": "Selektivitas adalah kemampuan penerima untuk memilih stasiun yang diinginkan dan menolak stasiun lain di frekuensi yang berdekatan. Ini terutama dicapai oleh filter." },
  { "en": "Jelaskan konsep 'sensitivitas' pada penerima radio.", "id": "Sensitivitas adalah kemampuan penerima untuk menangkap sinyal yang sangat lemah. Ini terutama ditentukan oleh gain dan angka kebisingan dari LNA tahap pertama." },
  { "en": "Apa itu 'analisis transient' dalam simulasi sirkuit?", "id": "Ini adalah jenis simulasi yang menghitung tegangan dan arus sirkuit sebagai fungsi waktu, berguna untuk melihat bagaimana sirkuit merespons pulsa atau sinyal saklar." },
  { "en": "Apa itu 'analisis AC' dalam simulasi sirkuit?", "id": "Ini adalah jenis simulasi yang menghitung respons sirkuit (gain dan fasa) terhadap berbagai frekuensi, berguna untuk merancang penguat dan filter." },
  { "en": "Apa itu 'analisis DC' dalam simulasi sirkuit?", "id": "Ini adalah jenis simulasi yang menghitung titik operasi DC (titik-Q) dari semua transistor dalam sirkuit saat tidak ada sinyal AC." },
  { "en": "Apa itu 'analisis Monte Carlo'?", "id": "Ini adalah jenis simulasi statistik di mana nilai komponen divariasikan secara acak dalam toleransinya untuk melihat bagaimana kinerja sirkuit akan bervariasi dalam produksi massal." },
  { "en": "Apa itu 'analisis kasus terburuk' (worst-case analysis)?", "id": "Ini adalah analisis di mana semua toleransi komponen diatur ke nilai ekstremnya untuk memastikan sirkuit masih akan berfungsi dalam kondisi yang paling tidak menguntungkan." },
  { "en": "Apa itu 'komputasi kuantum' dan bagaimana hubungannya dengan transistor?", "id": "Komputasi kuantum menggunakan 'qubit' yang dapat berada dalam superposisi 0 dan 1. Sementara itu berbeda secara fundamental, transistor klasik masih sangat penting untuk sirkuit kontrol yang membaca dan memanipulasi qubit ini." },
  { "en": "Apa itu 'dekoherensi kuantum'?", "id": "Ini adalah hilangnya keadaan kuantum yang rapuh dari sebuah qubit karena interaksi dengan lingkungannya. Melindungi qubit dari dekoherensi adalah tantangan utama dalam komputasi kuantum." },
  { "en": "Apa itu 'transmon'?", "id": "Transmon adalah jenis qubit superkonduktor yang merupakan salah satu kandidat terdepan untuk membangun komputer kuantum. Strukturnya mirip dengan sirkuit LC yang dikontrol oleh sambungan Josephson." },
  { "en": "Apa peran transistor dalam 'penguat parametrik'?", "id": "Dalam beberapa aplikasi frekuensi sangat tinggi atau kuantum, transistor (atau dioda varaktor) digunakan untuk 'memompa' sirkuit resonan, menciptakan amplifikasi dengan noise yang sangat rendah." },
  { "en": "Apa itu 'kriptografi' dan bagaimana transistor mendukungnya?", "id": "Kriptografi adalah ilmu pengamanan informasi. Algoritma enkripsi modern memerlukan perhitungan matematis yang sangat intensif, yang hanya mungkin dilakukan oleh prosesor yang berisi miliaran transistor cepat." },
  { "en": "Apa itu 'percepatan perangkat keras' (hardware acceleration)?", "id": "Ini adalah penggunaan sirkuit khusus (ASIC atau FPGA), yang terbuat dari transistor yang dioptimalkan untuk satu tugas, untuk melakukan fungsi tertentu jauh lebih cepat dan lebih efisien daripada CPU serba guna." },
  { "en": "Apa contoh dari percepatan perangkat keras?", "id": "GPU untuk grafis, TPU untuk AI, dan chip enkripsi khusus adalah contoh di mana tugas-tugas spesifik dialihkan dari CPU ke perangkat keras yang lebih efisien." },
  { "en": "Apa itu 'FPGA' (Field-Programmable Gate Array)?", "id": "FPGA adalah IC yang berisi lautan blok logika dan interkoneksi yang dapat diprogram oleh pengguna setelah pembuatan. Ini memungkinkan pembuatan prototipe sirkuit digital dengan cepat." },
  { "en": "Apa itu 'ASIC' (Application-Specific Integrated Circuit)?", "id": "ASIC adalah IC yang dirancang dari awal untuk satu tujuan spesifik. Ini menawarkan kinerja dan efisiensi tertinggi tetapi dengan biaya desain awal yang sangat tinggi dan tanpa fleksibilitas." },
  { "en": "Apa perbedaan mendasar antara CPU, GPU, FPGA, dan ASIC?", "id": "CPU sangat fleksibel. GPU dioptimalkan untuk komputasi paralel. FPGA fleksibel dan dapat diprogram di lapangan. ASIC adalah yang tercepat dan paling efisien tetapi tidak fleksibel sama sekali." },
  { "en": "Apa itu 'leakage power' vs 'dynamic power'?", "id": "Leakage power (daya bocor) adalah daya yang dikonsumsi bahkan saat transistor tidak beralih. Dynamic power (daya dinamis) adalah daya yang dikonsumsi selama proses switching." },
  { "en": "Pada chip modern, mana yang lebih menjadi masalah, leakage atau dynamic power?", "id": "Pada teknologi proses yang lebih tua, daya dinamis mendominasi. Pada chip modern dengan miliaran transistor yang sangat kecil, daya bocor menjadi kontributor yang sangat signifikan terhadap total konsumsi daya." },
  { "en": "Apa itu 'dark silicon'?", "id": "Ini adalah istilah yang menggambarkan fenomena pada chip modern di mana tidak semua transistor dapat dinyalakan secara bersamaan karena akan melebihi batas daya dan panas yang aman. Sebagian besar silikon harus tetap 'gelap' atau tidak aktif." },
  { "en": "Apa itu 'multivibrator' secara umum?", "id": "Ini adalah kelas sirkuit elektronik (berbasis transistor) yang digunakan untuk mengimplementasikan sistem dua keadaan sederhana seperti osilator, timer, dan flip-flop." },
  { "en": "Apa itu 'hysteresis' dalam konteks saklar?", "id": "Hysteresis berarti ada 'zona mati' antara ambang batas ON dan OFF. Anda harus melewati ambang batas atas untuk menyala, tetapi kemudian turun di bawah ambang batas yang lebih rendah untuk mati. Ini mencegah 'getaran' atau pergantian yang cepat." },
  { "en": "Sirkuit apa yang secara inheren menunjukkan hysteresis?", "id": "Schmitt trigger adalah contoh klasik dari sirkuit yang dirancang khusus untuk memiliki hysteresis, berguna untuk 'membersihkan' sinyal yang bising." },
  { "en": "Apa itu 'umpan balik positif'?", "id": "Umpan balik positif adalah ketika sebagian kecil dari output dikembalikan ke input dengan cara yang memperkuat perubahan. Ini adalah dasar dari osilasi dan perilaku 'latching'." },
  { "en": "Sirkuit apa yang menggunakan umpan balik positif?", "id": "Osilator, Schmitt trigger, dan semua jenis multivibrator (astabil, monostabil, bistabil) mengandalkan umpan balik positif untuk operasinya." },
  { "en": "Apa itu 'kondisi balapan' (race condition) dalam logika digital?", "id": "Ini adalah situasi yang tidak diinginkan di mana output sirkuit bergantung pada urutan atau waktu yang tidak dapat diprediksi dari peristiwa lain, seringkali karena keterlambatan propagasi yang berbeda melalui jalur transistor yang berbeda." },
  { "en": "Apa itu 'glitch' dalam sinyal digital?", "id": "Glitch adalah pulsa transien yang sangat singkat dan tidak diinginkan pada sinyal digital, seringkali disebabkan oleh kondisi balapan atau bahaya logika lainnya." },
  { "en": "Apa itu 'penyimpanan elektrostatik'?", "id": "Ini adalah prinsip di balik DRAM dan memori flash, di mana informasi disimpan sebagai ada atau tidak adanya muatan listrik pada kapasitor atau gerbang mengambang." },
  { "en": "Apa itu 'penyimpanan magnetik'?", "id": "Ini adalah prinsip di balik hard disk drive (HDD) dan MRAM, di mana informasi disimpan sebagai arah medan magnet pada suatu material." },
  { "en": "Apa itu 'penguat diferensial terkunci arus' (current-feedback amplifier)?", "id": "Ini adalah jenis op-amp yang berbeda di mana bandwidthnya sebagian besar tidak tergantung pada gainnya, membuatnya sangat baik untuk aplikasi video dan sinyal berkecepatan tinggi." },
  { "en": "Apa itu 'efek Gutzwiller'?", "id": "Ini adalah fenomena teoretis dalam fisika benda padat yang menggambarkan bagaimana interaksi elektron dapat menghambat pergerakan mereka, yang relevan untuk memahami perilaku material yang sangat berkorelasi." },
  { "en": "Apa itu 'elektronik molekuler'?", "id": "Ini adalah bidang penelitian yang mengeksplorasi penggunaan molekul tunggal atau rakitan molekul sebagai komponen elektronik, berpotensi sebagai langkah selanjutnya setelah transistor silikon." },
  { "en": "Apa itu 'tegangan saturasi lutut' (knee voltage)?", "id": "Ini adalah titik pada kurva karakteristik di mana transistor mulai beralih dari daerah aktif ke daerah saturasi." },
  { "en": "Apa itu 'transistor sebagai pelindung arus balik'?", "id": "Sebuah MOSFET P-channel sering digunakan pada input sirkuit bertenaga baterai sebagai saklar yang akan otomatis mati jika baterai dipasang terbalik, melindungi sirkuit." },
  { "en": "Mengapa transistor lebih baik daripada dioda untuk proteksi arus balik?", "id": "Karena penurunan tegangan pada MOSFET yang sepenuhnya ON (karena RDS(on)) jauh lebih rendah daripada penurunan tegangan maju dioda, sehingga jauh lebih efisien dan menghasilkan lebih sedikit panas." },
  { "en": "Apa itu 'load-switch' IC?", "id": "Load-switch adalah IC sederhana yang berisi MOSFET daya, sirkuit driver, dan fitur proteksi dalam satu kemasan, dirancang untuk menyalakan dan mematikan jalur daya dengan aman." },
  { "en": "Apa itu 'hot-swap controller'?", "id": "Ini adalah IC yang lebih canggih yang mengelola koneksi kartu sirkuit ke backplane yang sudah aktif, mengontrol lonjakan arus dan memastikan penyisipan yang aman menggunakan MOSFET eksternal." },
  { "en": "Apa itu 'ideal diode' controller?", "id": "Ini adalah IC yang mengontrol MOSFET eksternal untuk meniru perilaku dioda ideal, dengan penurunan tegangan maju yang sangat rendah dan tanpa arus bocor terbalik." },
  { "en": "Kapan 'ideal diode' digunakan?", "id": "Digunakan dalam aplikasi efisiensi tinggi di mana beberapa sumber daya perlu digabungkan (OR-ing) atau untuk proteksi arus balik di mana kehilangan daya harus diminimalkan." },
  { "en": "Apa itu 'solid-state relay' (SSR)?", "id": "SSR adalah versi elektronik dari relay mekanis. Ia menggunakan optocoupler untuk isolasi dan transistor daya (seperti MOSFET atau TRIAC) untuk menyalakan beban, tanpa ada bagian yang bergerak." },
  { "en": "Apa keuntungan SSR dibandingkan relay mekanis?", "id": "Kecepatan switching yang jauh lebih tinggi, tidak ada suara 'klik', umur yang lebih panjang (tidak ada keausan kontak), dan ketahanan terhadap guncangan dan getaran." },
  { "en": "Apa kerugian SSR?", "id": "Mereka menghasilkan lebih banyak panas (karena penurunan tegangan pada transistor), bisa lebih mahal, dan rentan terhadap kerusakan akibat lonjakan tegangan." },
  { "en": "Apa itu 'transistor efek medan karbon nanotube' (CNTFET)?", "id": "Ini adalah transistor eksperimental di mana channel-nya terbuat dari nanotube karbon, sebuah molekul silinder dari atom karbon, yang menjanjikan kinerja sangat tinggi." },
  { "en": "Apa itu 'termokopel'?", "id": "Termokopel adalah sensor suhu yang terbuat dari sambungan dua logam yang berbeda, yang menghasilkan tegangan kecil yang sebanding dengan suhu. Ini tidak menggunakan transistor secara langsung tetapi sinyalnya memerlukan penguat presisi tinggi." },
  { "en": "Apa itu 'RTD' (Resistance Temperature Detector)?", "id": "RTD adalah sensor suhu yang sangat akurat yang terbuat dari lilitan kawat (biasanya platina) yang resistansinya berubah secara dapat diprediksi dengan suhu. Seperti termokopel, sinyalnya memerlukan pengkondisian oleh sirkuit berbasis transistor." },
  { "en": "Apa itu 'sensor gambar kuantum-dot'?", "id": "Ini adalah teknologi sensor gambar baru di mana titik-titik kuantum (nanokristal semikonduktor) digunakan untuk menyerap cahaya, berpotensi menawarkan rentang dinamis dan akurasi warna yang lebih baik." },
  { "en": "Apa itu 'silicon photonics'?", "id": "Ini adalah bidang teknologi yang bertujuan untuk membuat komponen optik (seperti pemandu gelombang, modulator, dan detektor) langsung di atas chip silikon, berdampingan dengan sirkuit transistor elektronik." },
  { "en": "Apa tujuan akhir dari 'silicon photonics'?", "id": "Untuk menggantikan interkoneksi listrik yang lambat dan boros energi di antara chip (dan bahkan di dalam chip) dengan interkoneksi optik (cahaya) yang jauh lebih cepat dan lebih efisien." },
  { "en": "Mengapa transistor tetap menjadi 'jantung' dari hampir semua kemajuan teknologi?", "id": "Karena kemampuannya yang tak tertandingi untuk memanipulasi informasi (sebagai saklar) dan energi (sebagai penguat) dalam skala yang sangat kecil, cepat, dan efisien, menjadikannya blok bangunan paling fundamental di zaman modern." },

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
            }, 7500);
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
