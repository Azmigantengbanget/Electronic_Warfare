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


  { "en": "Kepanjangan EW (Electronic Warfare)?", "id": "Electronic Warfare." },
  { "en": "Definisi Perang Elektronika (EW)?", "id": "Pemanfaatan spektrum elektromagnetik." },
  { "en": "Tiga pilar utama EW (Electronic Warfare)?", "id": "EA, EP, dan ES." },
  { "en": "Kepanjangan EA (Electronic Attack)?", "id": "Electronic Attack." },
  { "en": "Kepanjangan EP (Electronic Protect)?", "id": "Electronic Protect." },
  { "en": "Kepanjangan ES (Electronic Support)?", "id": "Electronic Support." },
  { "en": "Tujuan utama EW (Electronic Warfare)?", "id": "Kontrol spektrum elektromagnetik." },
  { "en": "Domain operasi EW (Electronic Warfare)?", "id": "Spektrum elektromagnetik." },
  { "en": "Definisi Electronic Attack (EA)?", "id": "Penggunaan energi EM untuk menyerang." },
  { "en": "Aksi paling umum EA (Electronic Attack)?", "id": "Jamming (pengacakan sinyal)." },
  { "en": "Tujuan 'jamming'?", "id": "Mengganggu atau menipu penerima musuh." },
  { "en": "Tipe 'jamming' dasar?", "id": "Noise jamming dan deception jamming." },
  { "en": "Apa itu 'noise jamming'?", "id": "Memancarkan noise kuat menutupi sinyal." },
  { "en": "Apa itu 'deception jamming'?", "id": "Meniru sinyal untuk menipu musuh." },
  { "en": "Target utama EA (Electronic Attack)?", "id": "Radar dan sistem komunikasi." },
  { "en": "Apa itu 'radar jamming'?", "id": "Mengganggu operasi sistem radar." },
  { "en": "Apa itu 'communication jamming'?", "id": "Mengganggu sistem komunikasi radio." },
  { "en": "Definisi Electronic Protect (EP)?", "id": "Melindungi dari efek serangan EW." },
  { "en": "Aksi paling umum EP (Electronic Protect)?", "id": "Anti-jamming (AJ)." },
  { "en": "Apa itu 'spread spectrum'?", "id": "Teknik menyebar sinyal di pita lebar." },
  { "en": "Tujuan 'spread spectrum'?", "id": "Tahan terhadap jamming dan penyadapan." },
  { "en": "Kepanjangan FHSS (Frequency-Hopping Spread Spectrum)?", "id": "Frequency-Hopping Spread Spectrum." },
  { "en": "Prinsip FHSS (Frequency-Hopping Spread Spectrum)?", "id": "Frekuensi berubah-ubah secara acak." },
  { "en": "Kepanjangan DSSS (Direct-Sequence Spread Spectrum)?", "id": "Direct-Sequence Spread Spectrum." },
  { "en": "Prinsip DSSS (Direct-Sequence Spread Spectrum)?", "id": "Sinyal disebar dengan kode unik." },
  { "en": "Kepanjangan LPI/LPD?", "id": "Low Probability of Intercept/Detection." },
  { "en": "Tujuan komunikasi LPI/LPD?", "id": "Agar sulit dideteksi dan disadap." },
  { "en": "Definisi Electronic Support (ES)?", "id": "Mencari dan mengidentifikasi sumber energi EM." },
  { "en": "Nama lain untuk ES (Electronic Support)?", "id": "Electronic Support Measures (ESM)." },
  { "en": "Kepanjangan SIGINT (Signal Intelligence)?", "id": "Signal Intelligence." },
  { "en": "SIGINT (Signal Intelligence) adalah bagian dari?", "id": "Electronic Support (ES)." },
  { "en": "Dua sub-kategori utama SIGINT?", "id": "COMINT dan ELINT." },
  { "en": "Kepanjangan COMINT (Communications Intelligence)?", "id": "Communications Intelligence." },
  { "en": "Fokus COMINT (Communications Intelligence)?", "id": "Intelijen dari komunikasi manusia." },
  { "en": "Kepanjangan ELINT (Electronic Intelligence)?", "id": "Electronic Intelligence." },
  { "en": "Fokus ELINT (Electronic Intelligence)?", "id": "Intelijen dari sinyal non-komunikasi." },
  { "en": "Contoh target ELINT?", "id": "Sinyal radar, telemetri rudal." },
  { "en": "Apa itu 'Direction Finding' (DF)?", "id": "Menentukan arah datangnya sinyal." },
  { "en": "Kepanjangan DF (Direction Finding)?", "id": "Direction Finding." },
  { "en": "Apa itu 'emitter geolocation'?", "id": "Menentukan lokasi pemancar sinyal." },
  { "en": "Metode 'emitter geolocation'?", "id": "Triangulasi dari beberapa stasiun DF." },
  { "en": "Apa itu 'chaff'?", "id": "Material reflektif untuk menipu radar." },
  { "en": "Apa itu 'flare'?", "id": "Sumber panas untuk menipu rudal." },
  { "en": "'Chaff' dan 'flare' termasuk?", "id": "Expendable countermeasures." },
  { "en": "Apa itu 'radar warning receiver' (RWR)?", "id": "Pendeteksi sinyal radar musuh." },
  { "en": "Fungsi RWR (Radar Warning Receiver)?", "id": "Memberi peringatan jika terpapar radar." },
  { "en": "Apa itu 'stealth technology'?", "id": "Teknologi untuk mengurangi deteksi." },
  { "en": "Tujuan 'stealth technology'?", "id": "Mengurangi 'radar cross-section' (RCS)." },
  { "en": "Kepanjangan RCS (Radar Cross-Section)?", "id": "Radar Cross-Section." },
  { "en": "Apa itu RCS (Radar Cross-Section)?", "id": "Ukuran seberapa terdeteksi objek oleh radar." },
  { "en": "Apa itu 'radar absorbing material' (RAM)?", "id": "Material penyerap energi gelombang radar." },
  { "en": "Kepanjangan RAM (Radar Absorbing Material)?", "id": "Radar Absorbing Material." },
  { "en": "Apa itu 'barrage jamming'?", "id": "Jamming pada pita frekuensi lebar." },
  { "en": "Apa itu 'spot jamming'?", "id": "Jamming pada satu frekuensi spesifik." },
  { "en": "Apa itu 'sweep jamming'?", "id": "Jamming yang menyapu rentang frekuensi." },
  { "en": "Apa itu 'look-through'?", "id": "Jeda singkat saat jamming." },
  { "en": "Tujuan 'look-through'?", "id": "Memantau sinyal target saat menjamming." },
  { "en": "EA (Electronic Attack) non-destruktif disebut?", "id": "Soft kill." },
  { "en": "EA (Electronic Attack) destruktif disebut?", "id": "Hard kill." },
  { "en": "Contoh 'hard kill'?", "id": "Rudal anti-radiasi." },
  { "en": "Apa itu 'decoy'?", "id": "Umpan untuk mengalihkan perhatian musuh." },
  { "en": "Apa itu 'denial' dalam EA?", "id": "Mencegah musuh menggunakan spektrum EM." },
  { "en": "Apa itu 'deception' dalam EA?", "id": "Menipu musuh dengan informasi salah." },
  { "en": "Apa itu 'disruption' dalam EA?", "id": "Mengganggu operasi sistem musuh." },
  { "en": "Apa itu 'destruction' dalam EA?", "id": "Menghancurkan sistem elektronik musuh." },
  { "en": "Apa itu 'electromagnetic pulse' (EMP)?", "id": "Pulsa energi EM intensitas tinggi." },
  { "en": "Sumber EMP (Electromagnetic Pulse)?", "id": "Ledakan nuklir, senjata non-nuklir." },
  { "en": "Dampak EMP (Electromagnetic Pulse)?", "id": "Merusak perangkat elektronik tidak terlindungi." },
  { "en": "Perlindungan terhadap EMP?", "id": "Shielding, Faraday cage." },
  { "en": "Apa itu 'frequency agility'?", "id": "Kemampuan sistem mengubah frekuensi cepat." },
  { "en": "Tujuan 'frequency agility'?", "id": "Menghindari jamming." },
  { "en": "Apa itu 'electronic counter-countermeasures' (ECCM)?", "id": "Sinonim untuk Electronic Protect (EP)." },
  { "en": "Kepanjangan ECCM?", "id": "Electronic Counter-Countermeasures." },
  { "en": "Apa itu 'electronic warfare support' (EWS)?", "id": "Sinonim untuk Electronic Support (ES)." },
  { "en": "Apa itu 'order of battle' (OOB)?", "id": "Struktur dan disposisi kekuatan musuh." },
  { "en": "Peran ES (Electronic Support) dalam OOB?", "id": "Membangun 'electronic order of battle'." },
  { "en": "Apa itu 'threat library'?", "id": "Database karakteristik sinyal ancaman." },
  { "en": "Apa itu 'geolocation'?", "id": "Menentukan posisi geografis." },
  { "en": "Metode geolokasi pasif?", "id": "TDOA, FDOA." },
  { "en": "Kepanjangan TDOA (Time Difference of Arrival)?", "id": "Time Difference of Arrival." },
  { "en": "Prinsip TDOA (Time Difference of Arrival)?", "id": "Mengukur beda waktu tiba sinyal." },
  { "en": "Kepanjangan FDOA (Frequency Difference of Arrival)?", "id": "Frequency Difference of Arrival." },
  { "en": "Prinsip FDOA (Frequency Difference of Arrival)?", "id": "Mengukur beda frekuensi (efek Doppler)." },
  { "en": "Apa itu 'spoofing'?", "id": "Memalsukan sinyal untuk menipu." },
  { "en": "Contoh 'spoofing'?", "id": "Memalsukan sinyal GPS." },
  { "en": "Apa itu 'meaconing'?", "id": "Merekam dan memancarkan ulang sinyal navigasi." },
  { "en": "Apa itu 'jamming-to-signal ratio' (J/S)?", "id": "Rasio kekuatan sinyal jammer-target." },
  { "en": "Jamming efektif jika J/S?", "id": "Nilainya tinggi." },
  { "en": "Apa itu 'burn-through range'?", "id": "Jarak dimana radar atasi jamming." },
  { "en": "Apa itu 'home-on-jam' (HOJ)?", "id": "Kemampuan rudal mengunci sumber jammer." },
  { "en": "Apa itu 'infrared countermeasures' (IRCM)?", "id": "Tindakan melawan rudal pencari panas." },
  { "en": "Contoh IRCM (Infrared Countermeasures)?", "id": "Flare, DIRCM." },
  { "en": "Kepanjangan DIRCM?", "id": "Directed Infrared Countermeasure." },
  { "en": "Cara kerja DIRCM?", "id": "Menembakkan laser untuk membutakan rudal." },
  { "en": "Apa itu 'battle damage assessment' (BDA)?", "id": "Penilaian kerusakan setelah serangan." },
  { "en": "Peran ES (Electronic Support) dalam BDA?", "id": "Memantau apakah target berhenti memancar." },
  { "en": "Apa itu 'cyber warfare'?", "id": "Perang di domain siber." },
  { "en": "Hubungan EW dan 'cyber warfare'?", "id": "Saling tumpang tindih dan terintegrasi." },
  { "en": "Kepanjangan DRFM (Digital Radio Frequency Memory)?", "id": "Digital Radio Frequency Memory." },
  { "en": "Fungsi DRFM (Digital Radio Frequency Memory)?", "id": "Merekam dan memutar ulang sinyal RF." },
  { "en": "Aplikasi DRFM (Digital Radio Frequency Memory)?", "id": "Deception jamming yang sangat canggih." },
  { "en": "Apa itu 'Range Gate Pull-Off' (RGPO)?", "id": "Teknik menipu radar pelacak jarak." },
  { "en": "Apa itu 'Velocity Gate Pull-Off' (VGPO)?", "id": "Teknik menipu radar pelacak kecepatan." },
  { "en": "Serangan pada sinyal satelit navigasi?", "id": "GPS/GNSS jamming dan spoofing." },
  { "en": "Kepanjangan GNSS (Global Navigation Satellite System)?", "id": "Global Navigation Satellite System." },
  { "en": "Tujuan 'GPS (Global Positioning System) jamming'?", "id": "Mencegah penerima mendapatkan sinyal lokasi." },
  { "en": "Tujuan 'GPS (Global Positioning System) spoofing'?", "id": "Memberi informasi lokasi palsu." },
  { "en": "Apa itu 'Wi-Fi deauthentication attack'?", "id": "Serangan EA pada jaringan Wi-Fi." },
  { "en": "Kepanjangan DEW (Directed Energy Weapon)?", "id": "Directed Energy Weapon." },
  { "en": "Contoh DEW (Directed Energy Weapon)?", "id": "Laser berenergi tinggi, microwave." },
  { "en": "Kepanjangan CRPA (Controlled Reception Pattern Antenna)?", "id": "Controlled Reception Pattern Antenna." },
  { "en": "Fungsi CRPA (Controlled Reception Pattern Antenna)?", "id": "Antena anti-jamming untuk GPS." },
  { "en": "Cara kerja CRPA (Controlled Reception Pattern Antenna)?", "id": "Menciptakan 'null' ke arah jammer." },
  { "en": "Apa itu 'hop rate'?", "id": "Kecepatan perpindahan frekuensi FHSS." },
  { "en": "Apa itu 'dwell time'?", "id": "Waktu tinggal di satu frekuensi FHSS." },
  { "en": "Apa itu 'processing gain'?", "id": "Ukuran ketahanan DSSS terhadap jamming." },
  { "en": "Apa itu 'hardening' elektronika?", "id": "Melindungi perangkat dari efek EMP/HPM." },
  { "en": "Kepanjangan SEI (Specific Emitter Identification)?", "id": "Specific Emitter Identification." },
  { "en": "Tujuan SEI (Specific Emitter Identification)?", "id": "Mengidentifikasi pemancar spesifik, bukan tipe." },
  { "en": "Dasar SEI (Specific Emitter Identification)?", "id": "Ciri khas unik sinyal (fingerprint)." },
  { "en": "Kepanjangan PDW (Pulse Descriptor Word)?", "id": "Pulse Descriptor Word." },
  { "en": "Isi dari PDW (Pulse Descriptor Word)?", "id": "Parameter sinyal radar (frekuensi, PRI)." },
  { "en": "Kepanjangan PRI (Pulse Repetition Interval)?", "id": "Pulse Repetition Interval." },
  { "en": "Apa itu antena Adcock?", "id": "Tipe antena untuk Direction Finding (DF)." },
  { "en": "Apa itu 'Naval EW'?", "id": "Perang elektronika di domain maritim." },
  { "en": "Kepanjangan SEAD (Suppression of Enemy Air Defenses)?", "id": "Suppression of Enemy Air Defenses." },
  { "en": "Misi SEAD (Suppression of Enemy Air Defenses)?", "id": "Menekan pertahanan udara musuh." },
  { "en": "Senjata utama SEAD?", "id": "Rudal anti-radiasi (ARM)." },
  { "en": "Apa itu 'cognitive EW'?", "id": "EW yang menggunakan AI untuk beradaptasi." },
  { "en": "Apa itu 'cyber-EW convergence'?", "id": "Integrasi operasi siber dan EW." },
  { "en": "Apa itu 'distributed EW'?", "id": "Penggunaan banyak platform EW terkoordinasi." },
  { "en": "Apa itu 'infrared search and track' (IRST)?", "id": "Sistem deteksi pasif berbasis panas." },
  { "en": "Keuntungan IRST (Infrared Search and Track)?", "id": "Tidak memancarkan sinyal, sulit dideteksi." },
  { "en": "Apa itu 'laser warning receiver' (LWR)?", "id": "Mendeteksi pancaran sinar laser." },
  { "en": "Apa itu 'cross-eye jamming'?", "id": "Teknik menipu radar monopulse." },
  { "en": "Apa itu 'terrain bounce jamming'?", "id": "Memantulkan sinyal jamming dari tanah." },
  { "en": "Apa itu 'escort jamming'?", "id": "Jammer yang mengawal pesawat tempur." },
  { "en": "Apa itu 'stand-off jamming'?", "id": "Jammer beroperasi dari jarak aman." },
  { "en": "Apa itu 'self-protection jamming'?", "id": "Jammer untuk melindungi platform sendiri." },
  { "en": "Apa itu 'denial of service' (DoS) dalam EW?", "id": "Mencegah musuh menggunakan sistem elektroniknya." },
  { "en": "Apa itu 'parametric receiver'?", "id": "Penerima sensitif untuk deteksi sinyal." },
  { "en": "Apa itu 'intercept receiver'?", "id": "Penerima untuk deteksi dan analisa sinyal." },
  { "en": "Apa itu 'angle of arrival' (AOA)?", "id": "Sudut datangnya sinyal." },
  { "en": "DF (Direction Finding) mengukur apa?", "id": "Angle of Arrival (AOA)." },
  { "en": "Apa itu 'time of arrival' (TOA)?", "id": "Waktu tiba sinyal di penerima." },
  { "en": "Apa itu 'radio silence'?", "id": "Kondisi tidak memancarkan sinyal radio." },
  { "en": "Tujuan 'radio silence'?", "id": "Menghindari deteksi oleh ES musuh." },
  { "en": "Apa itu 'EMCON (Emission Control)'?", "id": "Kontrol terhadap semua emisi elektromagnetik." },
  { "en": "Apa itu 'passive detection'?", "id": "Deteksi target tanpa memancarkan sinyal." },
  { "en": "Contoh 'passive detection'?", "id": "Sistem IRST, RWR, dan ESM." },
  { "en": "Apa itu 'active detection'?", "id": "Deteksi dengan memancarkan sinyal." },
  { "en": "Contoh 'active detection'?", "id": "Radar, Sonar aktif." },
  { "en": "Apa itu 'bistatic radar'?", "id": "Radar dengan pemancar-penerima terpisah." },
  { "en": "Keuntungan 'bistatic radar'?", "id": "Sulit dideteksi, bisa deteksi stealth." },
  { "en": "Apa itu 'multistatic radar'?", "id": "Sistem dengan banyak pemancar/penerima." },
  { "en": "Apa itu 'over-the-horizon' (OTH) radar?", "id": "Radar yang bisa melihat melampaui cakrawala." },
  { "en": "Prinsip OTH (Over-the-Horizon) radar?", "id": "Memantulkan sinyal dari ionosfer." },
  { "en": "Apa itu 'low-frequency radar'?", "id": "Radar frekuensi rendah (VHF/UHF)." },
  { "en": "Keuntungan 'low-frequency radar'?", "id": "Potensi mendeteksi pesawat stealth." },
  { "en": "Apa itu 'signature management'?", "id": "Mengelola jejak sinyal agar sulit dideteksi." },
  { "en": "Apa itu 'spectrum management'?", "id": "Proses mengelola penggunaan spektrum frekuensi." },
  { "en": "Tujuan 'spectrum management'?", "id": "Menghindari interferensi, alokasi efisien." },
  { "en": "Apa itu 'electronic reconnaissance'?", "id": "Pengintaian untuk mendapatkan info ELINT." },
  { "en": "Apa itu 'electronic surveillance'?", "id": "Pengawasan sistematis spektrum EM." },
  { "en": "Kepanjangan IED (Improvised Explosive Device)?", "id": "Improvised Explosive Device." },
  { "en": "Peran EW (Electronic Warfare) melawan IED?", "id": "Menjamming sinyal pemicu radio." },
  { "en": "Apa itu 'drone detection system'?", "id": "Sistem untuk mendeteksi drone." },
  { "en": "Metode deteksi drone?", "id": "Radar, RF scanner, akustik, optik." },
  { "en": "Apa itu 'drone jamming'?", "id": "Mengganggu sinyal kontrol atau GPS drone." },
  { "en": "Apa itu 'drone spoofing'?", "id": "Mengambil alih kendali drone." },
  { "en": "Apa itu 'High Power Microwave' (HPM)?", "id": "Senjata gelombang mikro berdaya tinggi." },
  { "en": "Dampak HPM (High Power Microwave)?", "id": "Merusak sirkuit elektronik (soft kill)." },
  { "en": "Apa itu 'ultrawideband' (UWB)?", "id": "Teknologi sinyal dengan bandwidth sangat lebar." },
  { "en": "Kelebihan UWB (Ultrawideband)?", "id": "Sulit dijamming, resolusi tinggi." },
  { "en": "Apa itu 'network-centric warfare'?", "id": "Perang yang berpusat pada jaringan." },
  { "en": "Peran EW di dalamnya?", "id": "Melindungi jaringan, menyerang jaringan musuh." },
  { "en": "Apa itu 'adaptive jamming'?", "id": "Jamming yang beradaptasi dengan sinyal target." },
  { "en": "Apa itu 'communications deception'?", "id": "Menipu musuh melalui media komunikasi." },
  { "en": "Apa itu 'manipulative deception'?", "id": "Mengubah informasi yang dikirim musuh." },
  { "en": "Apa itu 'imitative deception'?", "id": "Meniru transmisi musuh." },
  { "en": "Apa itu 'false target generation'?", "id": "Menciptakan target palsu di radar." },
  { "en": "Apa itu 'infrared signature'?", "id": "Jejak panas yang dipancarkan objek." },
  { "en": "Cara mengurangi 'infrared signature'?", "id": "Pendinginan, material khusus." },
  { "en": "Apa itu 'acoustic signature'?", "id": "Jejak suara yang dihasilkan objek." },
  { "en": "Contoh 'acoustic signature'?", "id": "Suara mesin kapal selam." },
  { "en": "Apa itu 'magnetic anomaly detector' (MAD)?", "id": "Detektor anomali medan magnet bumi." },
  { "en": "Penggunaan MAD (Magnetic Anomaly Detector)?", "id": "Mendeteksi kapal selam." },
  { "en": "Apa itu 'passive sonar'?", "id": "Sonar yang hanya mendengarkan suara." },
  { "en": "Apa itu 'active sonar'?", "id": "Sonar yang memancarkan dan menerima 'ping'." },
  { "en": "Apa itu 'counter-targeting'?", "id": "Mengganggu proses penargetan senjata musuh." },
  { "en": "Apa itu 'jerk' dalam EW?", "id": "Tingkat perubahan percepatan (untuk menipu)." },
  { "en": "Apa itu 'waveform agility'?", "id": "Kemampuan mengubah bentuk gelombang." },
  { "en": "Tujuan 'waveform agility'?", "id": "Meningkatkan ketahanan terhadap deteksi/jamming." },
  { "en": "Apa itu 'cognitive radar'?", "id": "Radar yang bisa belajar dan beradaptasi." },
  { "en": "Apa itu 'EW (Electronic Warfare) pod'?", "id": "Pod eksternal di pesawat untuk EW." },
  { "en": "Tujuan 'EW (Electronic Warfare) pod'?", "id": "Memberi kemampuan EW tanpa modifikasi internal." },
  { "en": "Apa itu 'towed decoy'?", "id": "Umpan yang ditarik di belakang pesawat." },
  { "en": "Fungsi 'towed decoy'?", "id": "Menarik rudal radar menjauh dari pesawat." },
  { "en": "Apa itu 'cyclostationary feature detection'?", "id": "Teknik deteksi sinyal di dalam noise." },
  { "en": "Parameter dalam PDW (Pulse Descriptor Word)?", "id": "Frekuensi, lebar pulsa, PRI, AOA." },
  { "en": "Apa itu 'pulse agility'?", "id": "Kemampuan radar mengubah parameter pulsa." },
  { "en": "Apa itu 'staggered PRI (Pulse Repetition Interval)'?", "id": "Menggunakan interval pulsa yang bervariasi." },
  { "en": "Tujuan 'staggered PRI (Pulse Repetition Interval)'?", "id": "Mengatasi 'range ambiguity'." },
  { "en": "Apa itu 'phased array antenna'?", "id": "Antena yang beam-nya dikontrol elektronik." },
  { "en": "Keuntungan 'phased array'?", "id": "'Beam steering' sangat cepat, multi-target." },
  { "en": "Apa itu 'conformal antenna'?", "id": "Antena yang menyatu dengan bodi platform." },
  { "en": "Apa itu 'digital beamforming'?", "id": "Pembentukan beam secara digital." },
  { "en": "Kepanjangan NEW (Network Electronic Warfare)?", "id": "Network Electronic Warfare." },
  { "en": "Fokus NEW (Network Electronic Warfare)?", "id": "Menyerang dan melindungi jaringan data." },
  { "en": "Contoh target NEW (Network Electronic Warfare)?", "id": "Data links, C2 systems." },
  { "en": "Apa itu 'satellite uplink jamming'?", "id": "Mengganggu sinyal dari bumi ke satelit." },
  { "en": "Apa itu 'satellite downlink jamming'?", "id": "Mengganggu sinyal dari satelit ke bumi." },
  { "en": "Mana lebih mudah, uplink/downlink jamming?", "id": "Downlink jamming, butuh daya lebih kecil." },
  { "en": "Kepanjangan SSA (Space Situational Awareness)?", "id": "Space Situational Awareness." },
  { "en": "Peran EW (Electronic Warfare) dalam SSA?", "id": "Deteksi dan lacak satelit via sinyal RF." },
  { "en": "Kepanjangan C-UAS (Counter-Unmanned Aerial System)?", "id": "Counter-Unmanned Aerial System." },
  { "en": "Metode deteksi drone berbasis RF (Radio Frequency)?", "id": "Mendeteksi sinyal kontrol atau video." },
  { "en": "Metode mitigasi drone berbasis RF (Radio Frequency)?", "id": "Jamming atau spoofing." },
  { "en": "Apa itu 'information warfare' (IW)?", "id": "Perang di domain informasi." },
  { "en": "EW (Electronic Warfare) adalah bagian dari?", "id": "Information Warfare (IW)." },
  { "en": "Apa itu 'spectrum warfare'?", "id": "Konsep EW yang lebih luas dan terintegrasi." },
  { "en": "Apa itu 'range ambiguity' radar?", "id": "Ketidakmampuan radar menentukan jarak pasti." },
  { "en": "Apa itu 'Doppler ambiguity' radar?", "id": "Ketidakmampuan radar menentukan kecepatan pasti." },
  { "en": "Apa itu 'mainlobe' antena?", "id": "Arah pancaran utama antena." },
  { "en": "Apa itu 'backlobe' antena?", "id": "Pancaran minor ke arah belakang." },
  { "en": "Apa itu 'antenna reciprocity'?", "id": "Sifat antena sama saat pancar/terima." },
  { "en": "Apa itu 'polarization mismatch'?", "id": "Polarisasi antena pengirim-penerima beda." },
  { "en": "Dampak 'polarization mismatch'?", "id": "Pelemahan sinyal yang signifikan." },
  { "en": "Apa itu 'circular polarization'?", "id": "Polarisasi gelombang berputar (kanan/kiri)." },
  { "en": "Keuntungan 'circular polarization'?", "id": "Mengurangi efek pantulan (multipath)." },
  { "en": "Apa itu 'laser designator'?", "id": "Laser untuk menandai target." },
  { "en": "Countermeasure untuk 'laser designator'?", "id": "Deteksi laser, asap, material reflektif." },
  { "en": "Apa itu 'ELINT (Electronic Intelligence) parameters'?", "id": "Parameter spesifik sinyal non-komunikasi." },
  { "en": "Apa itu 'COMINT (Communications Intelligence) parameters'?", "id": "Parameter spesifik sinyal komunikasi." },
  { "en": "Apa itu 'flicker jamming'?", "id": "Jamming dengan menyalakan-mematikan jammer." },
  { "en": "Apa itu 'Digital-to-Analog Converter' (DAC)?", "id": "Mengubah sinyal digital ke analog." },
  { "en": "Peran DAC (Digital-to-Analog Converter) di DRFM?", "id": "Membangun kembali sinyal RF analog." },
  { "en": "Apa itu 'Analog-to-Digital Converter' (ADC)?", "id": "Mengubah sinyal analog ke digital." },
  { "en": "Peran ADC (Analog-to-Digital Converter) di DRFM?", "id": "Mengambil sampel sinyal RF masuk." },
  { "en": "Apa itu 'bandwidth' jammer?", "id": "Rentang frekuensi yang bisa di-jamming." },
  { "en": "Apa itu 'effective radiated power' (ERP)?", "id": "Daya efektif yang dipancarkan jammer." },
  { "en": "Kepanjangan ERP (Effective Radiated Power)?", "id": "Effective Radiated Power." },
  { "en": "Apa itu 'jamming effectiveness'?", "id": "Ukuran seberapa efektif suatu jamming." },
  { "en": "Apa itu 'terrain masking'?", "id": "Menggunakan medan (bukit) untuk bersembunyi." },
  { "en": "Tujuan 'terrain masking'?", "id": "Menghindari deteksi oleh radar." },
  { "en": "Apa itu 'doppler notch filter'?", "id": "Filter untuk menghilangkan sinyal 'clutter'." },
  { "en": "Apa itu 'clutter'?", "id": "Gema radar yang tidak diinginkan." },
  { "en": "Contoh 'clutter'?", "id": "Pantulan dari tanah, hujan, bangunan." },
  { "en": "Apa itu 'moving target indication' (MTI)?", "id": "Fitur radar untuk deteksi target bergerak." },
  { "en": "Kepanjangan MTI?", "id": "Moving Target Indication." },
  { "en": "Apa itu 'pulse doppler radar'?", "id": "Radar yang bisa ukur jarak-kecepatan." },
  { "en": "Apa itu 'chirp' sinyal?", "id": "Sinyal yang frekuensinya berubah." },
  { "en": "Apa itu 'pulse compression'?", "id": "Teknik untuk tingkatkan resolusi radar." },
  { "en": "Apa itu 'synthetic aperture radar' (SAR)?", "id": "Radar untuk pencitraan permukaan." },
  { "en": "Kepanjangan SAR (Synthetic Aperture Radar)?", "id": "Synthetic Aperture Radar." },
  { "en": "Apa itu 'inverse SAR' (ISAR)?", "id": "Pencitraan radar dari gerakan target." },
  { "en": "Apa itu 'passive coherent location' (PCL)?", "id": "Sistem deteksi pasif pakai sinyal lain." },
  { "en": "Contoh sinyal untuk PCL?", "id": "Siaran radio FM atau TV digital." },
  { "en": "Apa itu 'frequency-agile radar'?", "id": "Radar yang bisa mengubah frekuensi." },
  { "en": "Apa itu 'low probability of intercept' (LPI) radar?", "id": "Radar yang dirancang sulit dideteksi." },
  { "en": "Teknik LPI (Low Probability of Intercept) radar?", "id": "Daya rendah, bandwidth lebar." },
  { "en": "Apa itu 'automatic gain control' (AGC)?", "id": "Kontrol penguatan otomatis pada penerima." },
  { "en": "Tujuan AGC (Automatic Gain Control)?", "id": "Menjaga level sinyal output konstan." },
  { "en": "Apa itu 'signal demodulation'?", "id": "Proses mengekstrak informasi dari sinyal." },
  { "en": "Apa itu 'baud rate'?", "id": "Jumlah perubahan simbol per detik." },
  { "en": "Apa itu 'bit rate'?", "id": "Jumlah bit data per detik." },
  { "en": "Apa itu 'protocol analysis'?", "id": "Analisis protokol komunikasi yang digunakan." },
  { "en": "Apa itu 'traffic analysis'?", "id": "Analisis pola komunikasi, bukan isi." },
  { "en": "Apa itu 'cryptanalysis'?", "id": "Ilmu memecahkan kode enkripsi." },
  { "en": "Apa itu 'denial of imagery'?", "id": "Mencegah musuh dapatkan citra pengintaian." },
  { "en": "Apa itu 'directed energy'?", "id": "Energi yang dipancarkan ke arah tertentu." },
  { "en": "Apa itu 'kill chain'?", "id": "Tahapan dari penemuan hingga penghancuran target." },
  { "en": "Peran EW (Electronic Warfare) di 'kill chain'?", "id": "Di setiap tahap (deteksi, penargetan)." },
  { "en": "Apa itu 'jamming control loop'?", "id": "Sistem yang optimalkan parameter jamming." },
  { "en": "Apa itu 'threat reactive'?", "id": "Sistem EW yang merespon ancaman terdeteksi." },
  { "en": "Apa itu 'pre-emptive' EW?", "id": "Tindakan EW sebelum ancaman muncul." },
  { "en": "Apa itu 'non-cooperative target recognition'?", "id": "Pengenalan target tanpa bantuan target." },
  { "en": "Apa itu 'specific absorption rate' (SAR)?", "id": "Ukuran penyerapan energi RF oleh tubuh." },
  { "en": "Kepanjangan SAR (Specific Absorption Rate)?", "id": "Specific Absorption Rate." },
  { "en": "Apa itu 'electromagnetic compatibility' (EMC)?", "id": "Kemampuan perangkat berfungsi tanpa interferensi." },
  { "en": "Apa itu 'electromagnetic interference' (EMI)?", "id": "Gangguan elektromagnetik." },
  { "en": "Apa itu 'spectrum denial'?", "id": "Mencegah akses ke spektrum frekuensi." },
  { "en": "Apa itu 'deception repeater'?", "id": "Repeater yang memancarkan sinyal palsu." },
  { "en": "Apa itu 'coherent' sinyal?", "id": "Sinyal dengan hubungan fasa stabil." },
  { "en": "Apa itu 'non-coherent' sinyal?", "id": "Sinyal dengan fasa acak." },
  { "en": "Apa itu 'doppler shift'?", "id": "Pergeseran frekuensi akibat efek Doppler." },
  { "en": "Apa itu 'phase-locked loop' (PLL)?", "id": "Sirkuit untuk sinkronisasi fasa sinyal." },
  { "en": "Apa itu 'gallium arsenide' (GaAs)?", "id": "Bahan semikonduktor untuk sirkuit RF." },
  { "en": "Keunggulan GaAs (Gallium Arsenide)?", "id": "Kecepatan tinggi, noise rendah." },
  { "en": "Apa itu 'information entropy'?", "id": "Ukuran ketidakpastian suatu informasi." },
  { "en": "Bagaimana 'jamming' mempengaruhi 'channel capacity'?", "id": "Menurunkan kapasitas kanal secara drastis." },
  { "en": "Apa itu 'source coding' dalam EW?", "id": "Kompresi data sebelum transmisi." },
  { "en": "Apa itu 'channel coding' dalam EW?", "id": "Menambah redundansi untuk atasi error." },
  { "en": "Ancaman EW (Electronic Warfare) pada fiber optik?", "id": "Penyadapan sinyal cahaya (tapping)." },
  { "en": "Apa itu 'optical jamming'?", "id": "Mengganggu komunikasi optik dengan cahaya kuat." },
  { "en": "Ketahanan 64-QAM (Quadrature Amplitude Modulation) terhadap noise?", "id": "Kurang tahan dibanding QPSK." },
  { "en": "Apa itu 'trellis coded modulation' (TCM)?", "id": "Menggabungkan coding dan modulasi." },
  { "en": "Tujuan TCM (Trellis Coded Modulation)?", "id": "Meningkatkan ketahanan terhadap error." },
  { "en": "Apa itu 'adaptive nulling'?", "id": "Antena cerdas membuat 'null' ke jammer." },
  { "en": "Apa itu 'beam hopping'?", "id": "Mengalihkan 'beam' antena dengan cepat." },
  { "en": "Apa itu 'acoustic stealth' kapal selam?", "id": "Mengurangi jejak suara agar tidak terdeteksi." },
  { "en": "Ancaman EW (Electronic Warfare) pada komunikasi Li-Fi?", "id": "Jamming dengan sumber cahaya kuat." },
  { "en": "Apa itu 'intermodulation distortion' (IMD)?", "id": "Distorsi akibat sinyal non-linear." },
  { "en": "Dampak IMD (Intermodulation Distortion) pada penerima?", "id": "Menciptakan sinyal palsu yang mengganggu." },
  { "en": "Dampak 'phase noise' osilator?", "id": "Mengurangi kualitas sinyal modulasi." },
  { "en": "Apa itu 'noise figure' (NF)?", "id": "Ukuran penambahan noise oleh perangkat." },
  { "en": "NF (Noise Figure) yang baik untuk LNA?", "id": "Serendah mungkin." },
  { "en": "Apa itu 'dynamic range' penerima?", "id": "Rentang sinyal yang bisa diolah." },
  { "en": "Apa itu 'spurious-free dynamic range' (SFDR)?", "id": "Rentang dinamis bebas dari sinyal palsu." },
  { "en": "Apa itu 'desensitization'?", "id": "Penurunan sensitivitas penerima." },
  { "en": "Penyebab 'desensitization'?", "id": "Sinyal kuat di dekatnya (blocking)." },
  { "en": "Apa itu 'blocking'?", "id": "Sinyal kuat yang mengganggu penerima." },
  { "en": "Apa itu 'passive intermodulation' (PIM)?", "id": "Intermodulasi dari komponen pasif." },
  { "en": "Penyebab PIM (Passive Intermodulation)?", "id": "Konektor longgar, korosi." },
  { "en": "Apa itu 'baud rate scrambling'?", "id": "Mengacak data untuk spektrum merata." },
  { "en": "Apa itu 'forward error correction' (FEC)?", "id": "Menambahkan bit untuk koreksi error." },
  { "en": "Apa itu 'convolutional code'?", "id": "Jenis FEC yang umum digunakan." },
  { "en": "Apa itu 'turbo code'?", "id": "Jenis FEC dengan performa tinggi." },
  { "en": "Apa itu 'low-density parity-check' (LDPC) code?", "id": "Jenis FEC mendekati Shannon limit." },
  { "en": "Apa itu 'interleaving'?", "id": "Teknik menyebar data atasi 'error burst'." },
  { "en": "Apa itu 'error burst'?", "id": "Kesalahan data yang terjadi berurutan." },
  { "en": "Apa itu 'automatic link establishment' (ALE)?", "id": "Sistem otomatis memilih frekuensi terbaik." },
  { "en": "Penggunaan ALE (Automatic Link Establishment)?", "id": "Komunikasi radio HF jarak jauh." },
  { "en": "Apa itu 'meteor burst communication'?", "id": "Komunikasi dengan pantulan jejak meteor." },
  { "en": "Apa itu 'tropospheric scatter'?", "id": "Komunikasi dengan hamburan sinyal troposfer." },
  { "en": "Apa itu 'ionospheric sounding'?", "id": "Mengukur kondisi ionosfer." },
  { "en": "Apa itu 'maximum usable frequency' (MUF)?", "id": "Frekuensi maksimal untuk pantulan ionosfer." },
  { "en": "Apa itu 'lowest usable frequency' (LUF)?", "id": "Frekuensi terendah untuk komunikasi HF." },
  { "en": "Apa itu 'skip zone'?", "id": "Area mati antara 'ground wave' dan 'skywave'." },
  { "en": "Apa itu 'fading margin'?", "id": "Daya ekstra untuk kompensasi fading." },
  { "en": "Apa itu 'link budget'?", "id": "Perhitungan semua gain dan loss sinyal." },
  { "en": "Apa itu 'noise floor'?", "id": "Tingkat kebisingan dasar suatu sistem." },
  { "en": "Apa itu 'gain control'?", "id": "Pengaturan penguatan sinyal." },
  { "en": "Apa itu 'time-variant channel'?", "id": "Kanal yang karakteristiknya berubah waktu." },
  { "en": "Apa itu 'frequency-selective channel'?", "id": "Kanal yang penguatannya beda-beda." },
  { "en": "Apa itu 'flat fading channel'?", "id": "Kanal yang penguatannya sama." },
  { "en": "Apa itu 'doppler spread'?", "id": "Pelebaran spektrum akibat efek Doppler." },
  { "en": "Apa itu 'delay spread'?", "id": "Pelebaran sinyal akibat multipath." },
  { "en": "Apa itu 'coherence time'?", "id": "Durasi kanal dianggap tidak berubah." },
  { "en": "Apa itu 'coherence bandwidth'?", "id": "Rentang frekuensi kanal dianggap 'flat'." },
  { "en": "Apa itu 'orthogonal codes'?", "id": "Kode yang tidak saling berinterferensi." },
  { "en": "Contoh 'orthogonal codes'?", "id": "Kode Walsh-Hadamard." },
  { "en": "Apa itu 'pseudo-noise' (PN) sequence?", "id": "Sekuens acak semu." },
  { "en": "Penggunaan PN (Pseudo-Noise) sequence?", "id": "Pada sistem DSSS dan CDMA." },
  { "en": "Apa itu 'matched filter'?", "id": "Filter untuk memaksimalkan SNR." },
  { "en": "Apa itu 'correlation receiver'?", "id": "Penerima yang menggunakan korelasi." },
  { "en": "Apa itu 'raked receiver'?", "id": "Penerima untuk mengatasi efek multipath." },
  { "en": "Apa itu 'soft decision decoding'?", "id": "Decoding yang gunakan informasi probabilitas." },
  { "en": "Apa itu 'hard decision decoding'?", "id": "Decoding yang hanya gunakan nilai bit." },
  { "en": "Apa itu 'Viterbi algorithm'?", "id": "Algoritma decoding untuk convolutional code." },
  { "en": "Apa itu 'channel estimation'?", "id": "Proses mengestimasi karakteristik kanal." },
  { "en": "Apa itu 'training sequence'?", "id": "Sekuens yang diketahui untuk estimasi." },
  { "en": "Sinonim 'training sequence'?", "id": "Preamble atau pilot signal." },
  { "en": "Apa itu 'blind equalization'?", "id": "Equalization tanpa training sequence." },
  { "en": "Apa itu 'constant modulus algorithm' (CMA)?", "id": "Algoritma untuk blind equalization." },
  { "en": "Apa itu 'bit synchronization'?", "id": "Sinkronisasi waktu di level bit." },
  { "en": "Apa itu 'frame synchronization'?", "id": "Sinkronisasi waktu di level frame." },
  { "en": "Apa itu 'carrier recovery'?", "id": "Proses pemulihan sinyal carrier." },
  { "en": "Rangkaian untuk 'carrier recovery'?", "id": "Phase-Locked Loop (PLL)." },
  { "en": "Apa itu 'single sideband' (SSB) modulation?", "id": "Modulasi AM yang lebih efisien." },
  { "en": "Keuntungan SSB (Single Sideband)?", "id": "Menghemat bandwidth dan daya." },
  { "en": "Apa itu 'vestigial sideband' (VSB)?", "id": "Modulasi untuk siaran TV analog." },
  { "en": "Apa itu 'quadrature component' (Q)?", "id": "Komponen sinyal yang beda fasa 90°." },
  { "en": "Apa itu 'in-phase component' (I)?", "id": "Komponen sinyal referensi (0°)." },
  { "en": "Apa itu 'I/Q data'?", "id": "Representasi sinyal dalam komponen I/Q." },
  { "en": "Apa itu 'software-defined networking' (SDN)?", "id": "Arsitektur jaringan yang terprogram." },
  { "en": "Hubungan SDN (Software-Defined Networking) dan EW?", "id": "Memungkinkan EW yang lebih dinamis." },
  { "en": "Apa itu 'network function virtualization' (NFV)?", "id": "Virtualisasi fungsi jaringan." },
  { "en": "Apa itu 'man-portable' EW system?", "id": "Sistem EW yang bisa dibawa orang." },
  { "en": "Apa itu 'unattended ground sensor' (UGS)?", "id": "Sensor darat yang beroperasi mandiri." },
  { "en": "Komunikasi UGS (Unattended Ground Sensor)?", "id": "Biasanya nirkabel berdaya sangat rendah." },
  { "en": "Apa itu 'electronic warfare officer' (EWO)?", "id": "Perwira spesialis perang elektronika." },
  { "en": "Tugas EWO (Electronic Warfare Officer)?", "id": "Mengoperasikan dan mengelola sistem EW." },
  { "en": "Apa itu 'radar cross-section' (RCS) reduction?", "id": "Teknik mengurangi deteksi oleh radar." },
  { "en": "Contoh RCS (Radar Cross-Section) reduction?", "id": "Bentuk stealth, material penyerap." },
  { "en": "Apa itu 'spoofing' IED (Improvised Explosive Device)?", "id": "Memicu IED di waktu aman." },
  { "en": "Apa itu 'neutralization' IED (Improvised Explosive Device)?", "id": "Membuat IED tidak berfungsi." },
  { "en": "Apa itu 'geolocation of interference'?", "id": "Menentukan lokasi sumber interferensi." },
  { "en": "Pentingnya 'geolocation of interference'?", "id": "Untuk menindak atau menghancurkan jammer." },
  { "en": "Apa itu 'radio direction finding' (RDF)?", "id": "Sinonim untuk Direction Finding (DF)." },
  { "en": "Apa itu 'COMINT monitoring'?", "id": "Pemantauan komunikasi untuk intelijen." },
  { "en": "Apa itu 'ELINT analysis'?", "id": "Analisis sinyal radar untuk intelijen." },
  { "en": "Apa itu 'information operations' (IO)?", "id": "Sinonim luas untuk Information Warfare." },
  { "en": "Apa itu 'information operations' (IO)?", "id": "Operasi yang mempengaruhi informasi." },
  { "en": "Hubungan IO (Information Operations) dan EW (Electronic Warfare)?", "id": "EW adalah salah satu pilar IO." },
  { "en": "Apa itu 'deception' dalam IO (Information Operations)?", "id": "Menipu musuh melalui informasi." },
  { "en": "Apa itu 'operational security' (OPSEC)?", "id": "Proses melindungi informasi kritis." },
  { "en": "Hubungan OPSEC (Operational Security) dan EW?", "id": "EP adalah bagian dari OPSEC." },
  { "en": "Apa itu 'psychological operations' (PSYOP)?", "id": "Operasi untuk mempengaruhi emosi-pikiran." },
  { "en": "Apa itu 'military deception' (MILDEC)?", "id": "Aksi untuk menipu pembuat keputusan musuh." },
  { "en": "Apa itu 'counter-deception'?", "id": "Upaya untuk mendeteksi dan melawan penipuan." },
  { "en": "Apa itu 'counter-propaganda'?", "id": "Upaya untuk melawan propaganda musuh." },
  { "en": "Apa itu 'electronic warfare battle management'?", "id": "Manajemen sumber daya EW di pertempuran." },
  { "en": "Tujuan 'EW battle management'?", "id": "Alokasi sumber daya EW secara efektif." },
  { "en": "Apa itu 'time-hopping spread spectrum' (THSS)?", "id": "Sinyal meloncat antar slot waktu." },
  { "en": "Apa itu 'chirp spread spectrum' (CSS)?", "id": "Menggunakan sinyal 'chirp' untuk sebar energi." },
  { "en": "Keuntungan CSS (Chirp Spread Spectrum)?", "id": "Tahan terhadap interferensi dan multipath." },
  { "en": "Apa itu 'ultra-wideband' (UWB)?", "id": "Teknologi sinyal dengan bandwidth sangat lebar." },
  { "en": "Karakteristik UWB (Ultra-Wideband)?", "id": "Daya rendah, sulit dideteksi." },
  { "en": "Apa itu 'cognitive jamming'?", "id": "Jamming cerdas yang belajar dari lingkungan." },
  { "en": "Apa itu 'reactive jamming'?", "id": "Jamming yang aktif setelah deteksi sinyal." },
  { "en": "Apa itu 'proactive jamming'?", "id": "Jamming sebelum target memancarkan sinyal." },
  { "en": "Apa itu 'follower jammer'?", "id": "Jammer yang mengikuti frekuensi target." },
  { "en": "Apa itu 'terrain masking'?", "id": "Menggunakan medan untuk hindari deteksi." },
  { "en": "Apa itu 'radar clutter'?", "id": "Gema radar yang tidak diinginkan." },
  { "en": "Jenis 'radar clutter'?", "id": "Sea clutter, ground clutter, rain clutter." },
  { "en": "Apa itu 'doppler filtering'?", "id": "Filter untuk memisahkan target bergerak." },
  { "en": "Apa itu 'adaptive pulse compression'?", "id": "Kompresi pulsa yang beradaptasi." },
  { "en": "Apa itu 'ambiguity function'?", "id": "Fungsi matematis untuk analisa resolusi." },
  { "en": "Apa itu 'radar resolution cell'?", "id": "Volume terkecil yang bisa dibedakan." },
  { "en": "Faktor penentu resolusi jarak?", "id": "Lebar pulsa (pulse width)." },
  { "en": "Faktor penentu resolusi sudut?", "id": "Lebar beam antena." },
  { "en": "Apa itu 'inverse synthetic aperture radar' (ISAR)?", "id": "Pencitraan radar dari gerakan target." },
  { "en": "Kegunaan ISAR (Inverse Synthetic Aperture Radar)?", "id": "Identifikasi kapal atau pesawat." },
  { "en": "Apa itu 'passive coherent location' (PCL)?", "id": "Sistem deteksi pasif pakai sinyal lain." },
  { "en": "Sumber sinyal untuk PCL (Passive Coherent Location)?", "id": "Siaran radio FM atau TV digital." },
  { "en": "Apa itu 'polarization filtering'?", "id": "Memfilter sinyal berdasarkan polarisasinya." },
  { "en": "Apa itu 'antenna nulling'?", "id": "Mengarahkan 'null' antena ke jammer." },
  { "en": "Apa itu 'sidelobe cancellation'?", "id": "Teknik untuk menekan sinyal dari sidelobe." },
  { "en": "Apa itu 'spatial filtering'?", "id": "Filtering sinyal berdasarkan arah datang." },
  { "en": "Apa itu 'adaptive beamforming'?", "id": "Beamforming yang beradaptasi dengan lingkungan." },
  { "en": "Apa itu 'mutual coupling'?", "id": "Interaksi antar elemen dalam antena array." },
  { "en": "Apa itu 'blind source separation'?", "id": "Memisahkan sinyal sumber tanpa info." },
  { "en": "Apa itu 'independent component analysis' (ICA)?", "id": "Metode untuk 'blind source separation'." },
  { "en": "Apa itu 'feature extraction' sinyal?", "id": "Mengekstrak ciri khas dari sinyal." },
  { "en": "Apa itu 'automatic modulation classification' (AMC)?", "id": "Klasifikasi modulasi sinyal otomatis." },
  { "en": "Pentingnya AMC (Automatic Modulation Classification)?", "id": "Untuk demodulasi dan analisa COMINT." },
  { "en": "Apa itu 'network warfare' (NW)?", "id": "Bagian dari IO (Information Operations) terkait jaringan." },
  { "en": "Apa itu 'computer network attack' (CNA)?", "id": "Serangan pada jaringan komputer." },
  { "en": "Apa itu 'computer network defense' (CND)?", "id": "Pertahanan jaringan komputer." },
  { "en": "Apa itu 'computer network exploitation' (CNE)?", "id": "Eksploitasi jaringan untuk intelijen." },
  { "en": "Apa itu 'cyber electro magnetic activities' (CEMA)?", "id": "Konsep integrasi siber dan EW." },
  { "en": "Kepanjangan CEMA?", "id": "Cyber Electro Magnetic Activities." },
  { "en": "Apa itu 'GPS-denied environment'?", "id": "Lingkungan dimana sinyal GPS tidak ada." },
  { "en": "Solusi navigasi di 'GPS-denied'?", "id": "Navigasi inersia (INS), visi, LORAN." },
  { "en": "Kepanjangan INS (Inertial Navigation System)?", "id": "Inertial Navigation System." },
  { "en": "Apa itu 'terrain-referenced navigation' (TRN)?", "id": "Navigasi dengan membandingkan medan." },
  { "en": "Apa itu 'anti-radiation missile' (ARM)?", "id": "Rudal yang mengunci emisi radar." },
  { "en": "Apa itu 'expendables'?", "id": "Countermeasure sekali pakai (chaff, flare)." },
  { "en": "Apa itu 'non-expendables'?", "id": "Countermeasure yang bisa dipakai ulang." },
  { "en": "Contoh 'non-expendables'?", "id": "Jammer, towed decoy." },
  { "en": "Apa itu 'laser countermeasures'?", "id": "Tindakan melawan sistem berbasis laser." },
  { "en": "Apa itu 'electro-optical' (EO) system?", "id": "Sistem yang gunakan spektrum optik." },
  { "en": "Apa itu 'infrared' (IR) system?", "id": "Sistem yang gunakan spektrum inframerah." },
  { "en": "Contoh sistem EO/IR?", "id": "Kamera thermal, IRST." },
  { "en": "Apa itu 'DIRCM (Directed Infrared Countermeasure)'?", "id": "Sistem proteksi dari rudal panas." },
  { "en": "Cara kerja DIRCM?", "id": "Menembakkan laser untuk membutakan rudal." },
  { "en": "Apa itu 'full spectrum dominance'?", "id": "Dominasi di semua spektrum elektromagnetik." },
  { "en": "Apa itu 'cognitive electronic warfare'?", "id": "EW yang gunakan AI untuk adaptasi." },
  { "en": "Peran 'machine learning' di EW?", "id": "Klasifikasi sinyal, optimasi jamming." },
  { "en": "Apa itu 'deep learning' di EW?", "id": "Pengenalan pola sinyal yang kompleks." },
  { "en": "Apa itu 'reinforcement learning' di EW?", "id": "Jammer belajar strategi optimal." },
  { "en": "Apa itu 'spectrum sensing'?", "id": "Mendeteksi bagian spektrum yang kosong." },
  { "en": "Aplikasi 'spectrum sensing'?", "id": "Cognitive radio, dynamic spectrum access." },
  { "en": "Apa itu 'jamming margin'?", "id": "Tingkat jamming yang bisa ditoleransi." },
  { "en": "Apa itu 'processing gain'?", "id": "Peningkatan SNR karena teknik spread spectrum." },
  { "en": "Apa itu 'despreading'?", "id": "Proses kebalikan dari DSSS di penerima." },
  { "en": "Apa itu 'raked fingers'?", "id": "Komponen di 'raked receiver'." },
  { "en": "Apa itu 'orthogonal variable spreading factor' (OVSF)?", "id": "Kode yang digunakan di WCDMA." },
  { "en": "Apa itu 'radio frequency fingerprinting'?", "id": "Identifikasi perangkat dari ciri khas RF-nya." },
  { "en": "Apa itu 'covert communication'?", "id": "Komunikasi tersembunyi." },
  { "en": "Teknik 'covert communication'?", "id": "Steganografi, LPI/LPD." },
  { "en": "Apa itu 'acoustic intelligence' (ACINT)?", "id": "Intelijen dari sinyal akustik." },
  { "en": "Apa itu 'measurement and signature intelligence' (MASINT)?", "id": "Intelijen dari tanda-tanda spesifik." },
  { "en": "Apa itu 'flicker noise'?", "id": "Noise frekuensi rendah (1/f noise)." },
  { "en": "Apa itu 'shot noise'?", "id": "Noise dari aliran diskrit elektron." },
  { "en": "Apa itu 'thermal noise'?", "id": "Noise akibat agitasi termal elektron." },
  { "en": "Apa itu 'effective noise temperature'?", "id": "Ukuran kebisingan total suatu sistem." },
  { "en": "Apa itu 'link availability'?", "id": "Persentase waktu link komunikasi aktif." },
  { "en": "Faktor perusak 'link availability'?", "id": "Hujan (rain fade), fading, interferensi." },
  { "en": "Apa itu 'rain fade'?", "id": "Pelemahan sinyal RF akibat hujan." },
  { "en": "Frekuensi yang rentan 'rain fade'?", "id": "Frekuensi Ku-band dan Ka-band." },
  { "en": "Apa itu 'scintillation'?", "id": "Fluktuasi cepat sinyal akibat atmosfer." },
  { "en": "Apa itu 'signal propagation delay'?", "id": "Waktu yang dibutuhkan sinyal merambat." },
  { "en": "Apa itu 'free-space optical' (FSO) communication?", "id": "Komunikasi nirkabel menggunakan laser." },
  { "en": "Kelebihan FSO (Free-Space Optical)?", "id": "Bandwidth sangat tinggi, aman." },
  { "en": "Kelemahan FSO (Free-Space Optical)?", "id": "Rentan terhadap kabut dan halangan." },
  { "en": "Kepanjangan E3?", "id": "Electromagnetic Environmental Effects." },
  { "en": "Tujuan E3 (Electromagnetic Environmental Effects)?", "id": "Memastikan operasi sistem di lingkungan EM." },
  { "en": "Kepanjangan HERO?", "id": "Hazards of Electromagnetic Radiation to Ordnance." },
  { "en": "Risiko HERO (Hazards of Electromagnetic Radiation to Ordnance)?", "id": "Ledakan amunisi akibat radiasi RF." },
  { "en": "Kepanjangan HERP?", "id": "Hazards of Electromagnetic Radiation to Personnel." },
  { "en": "Risiko HERP (Hazards of Electromagnetic Radiation to Personnel)?", "id": "Bahaya radiasi RF pada manusia." },
  { "en": "Kepanjangan HERF?", "id": "Hazards of Electromagnetic Radiation to Fuel." },
  { "en": "Risiko HERF (Hazards of Electromagnetic Radiation to Fuel)?", "id": "Ledakan uap bahan bakar." },
  { "en": "Apa itu 'electromagnetic hardening'?", "id": "Pengerasan sistem terhadap efek EM." },
  { "en": "Metode 'electromagnetic hardening'?", "id": "Shielding, filtering, bonding, grounding." },
  { "en": "Apa itu 'smart noise'?", "id": "Noise yang dioptimalkan lawan sinyal." },
  { "en": "Apa itu 'coordinated jamming'?", "id": "Beberapa jammer bekerja sama." },
  { "en": "Apa itu 'swarm EW'?", "id": "Gerombolan platform EW kecil terkoordinasi." },
  { "en": "Tujuan 'swarm EW'?", "id": "Serangan terdistribusi dan masif." },
  { "en": "Apa itu 'false base station attack'?", "id": "Serangan meniru BTS seluler." },
  { "en": "Nama lain 'false base station'?", "id": "IMSI Catcher atau Stingray." },
  { "en": "Apa itu 'generative adversarial network' (GAN) di EW?", "id": "AI untuk ciptakan sinyal tipuan." },
  { "en": "Apa itu 'signal injection'?", "id": "Menyuntikkan data palsu ke sistem." },
  { "en": "Target 'signal injection'?", "id": "Sensor, jaringan kontrol industri." },
  { "en": "Apa itu 'velocity gate stealer'?", "id": "Teknik menipu pelacakan kecepatan radar." },
  { "en": "Apa itu 'orbital EW'?", "id": "EW yang dilakukan di luar angkasa." },
  { "en": "Ancaman EW pada 'TT&C link'?", "id": "Gangguan pada link telemetri-komando." },
  { "en": "Kepanjangan TT&C?", "id": "Telemetry, Tracking, and Command." },
  { "en": "Apa itu 'dazzling' satelit?", "id": "Membutakan sensor optik satelit." },
  { "en": "Metode 'dazzling' satelit?", "id": "Menggunakan laser dari darat/udara." },
  { "en": "Apa itu 'ASAT (Anti-Satellite) weapon'?", "id": "Senjata untuk menghancurkan satelit." },
  { "en": "Jenis 'ASAT (Anti-Satellite) weapon'?", "id": "Kinetik dan non-kinetik." },
  { "en": "Apa itu 'co-orbital ASAT'?", "id": "ASAT yang mendekati orbit targetnya." },
  { "en": "Apa itu 'space surveillance network' (SSN)?", "id": "Jaringan untuk melacak objek angkasa." },
  { "en": "Peran EW di SSN (Space Surveillance Network)?", "id": "Melacak satelit dari sinyal RF-nya." },
  { "en": "Sinonim 'underwater EW'?", "id": "Acoustic Warfare (perang akustik)." },
  { "en": "Apa itu 'prairie-masker system'?", "id": "Sistem peredam suara kapal." },
  { "en": "Cara kerja 'prairie-masker'?", "id": "Mengeluarkan gelembung udara di lambung." },
  { "en": "Apa itu 'sonar decoy'?", "id": "Umpan untuk menipu sistem sonar." },
  { "en": "Apa itu 'acoustic intercept'?", "id": "Mendeteksi sinyal sonar musuh." },
  { "en": "Apa itu 'wake homing torpedo'?", "id": "Torpedo yang mengikuti jejak kapal." },
  { "en": "Countermeasure 'wake homing'?", "id": "Manuver, umpan akustik aktif." },
  { "en": "Apa itu 'non-acoustic ASW'?", "id": "Deteksi kapal selam non-akustik." },
  { "en": "Contoh 'non-acoustic ASW'?", "id": "MAD, deteksi anomali air." },
  { "en": "Apa itu 'metamaterials' dalam EW?", "id": "Material rekayasa manipulasi gelombang EM." },
  { "en": "Aplikasi 'metamaterials'?", "id": "Antena, cloaking (penyelubungan), lensa." },
  { "en": "Apa itu 'photonic radar'?", "id": "Radar yang gunakan komponen fotonik." },
  { "en": "Keuntungan 'photonic radar'?", "id": "Bandwidth lebar, kebal EMI." },
  { "en": "Apa itu 'quantum radar'?", "id": "Radar berbasis prinsip mekanika kuantum." },
  { "en": "Prinsip 'quantum radar'?", "id": "Menggunakan 'quantum entanglement'." },
  { "en": "Potensi 'quantum radar'?", "id": "Mendeteksi objek stealth dengan mudah." },
  { "en": "Apa itu 'quantum sensing'?", "id": "Sensor ultra-sensitif berbasis kuantum." },
  { "en": "Apa itu 'RF convergence'?", "id": "Integrasi radar, EW, dan komunikasi." },
  { "en": "Tujuan 'RF convergence'?", "id": "Efisiensi spektrum dan perangkat keras." },
  { "en": "Apa itu AN/ALQ-99?", "id": "Sistem 'airborne jamming' taktis." },
  { "en": "Platform pengguna AN/ALQ-99?", "id": "Pesawat EA-18G Growler." },
  { "en": "Apa itu EA-18G Growler?", "id": "Pesawat tempur khusus perang elektronika." },
  { "en": "Apa itu 'Wild Weasel'?", "id": "Misi SEAD Angkatan Udara AS." },
  { "en": "Pesawat 'Wild Weasel' modern?", "id": "F-16CM Block 50/52." },
  { "en": "Apa itu Khibiny?", "id": "Sistem EW pesawat tempur Rusia." },
  { "en": "Apa itu Krasukha?", "id": "Sistem 'ground-based jamming' Rusia." },
  { "en": "Apa itu AN/SLQ-32?", "id": "Sistem EW kapal perang AS." },
  { "en": "Apa itu Nulka?", "id": "Decoy aktif untuk melindungi kapal." },
  { "en": "Apa itu 'technical ELINT' (TECHELINT)?", "id": "Analisis mendalam parameter sinyal asing." },
  { "en": "Apa itu 'operational ELINT' (OPELINT)?", "id": "Analisis taktis sinyal di pertempuran." },
  { "en": "Apa itu 'Foreign Instrumentation Signals Intelligence' (FISINT)?", "id": "Intelijen dari sinyal instrumentasi asing." },
  { "en": "Apa itu 'burst transmission'?", "id": "Transmisi data singkat dan cepat." },
  { "en": "Tujuan 'burst transmission'?", "id": "Mengurangi kemungkinan deteksi/intersep." },
  { "en": "Apa itu 'packet analysis'?", "id": "Analisis isi paket data jaringan." },
  { "en": "Apa itu 'flow analysis'?", "id": "Analisis metadata lalu lintas jaringan." },
  { "en": "Apa itu 'superresolution DF'?", "id": "Teknik DF melampaui batas konvensional." },
  { "en": "Apa itu 'EW reprogramming'?", "id": "Memperbarui 'threat library' sistem EW." },
  { "en": "Pentingnya 'EW reprogramming'?", "id": "Adaptasi terhadap ancaman baru." },
  { "en": "Apa itu 'mission data file' (MDF)?", "id": "Database ancaman untuk platform spesifik." },
  { "en": "Apa itu 'Joint Restricted Frequency List' (JRFL)?", "id": "Daftar frekuensi yang dilindungi/dibatasi." },
  { "en": "Tujuan JRFL (Joint Restricted Frequency List)?", "id": "Mencegah interferensi pasukan kawan." },
  { "en": "Apa itu 'Electromagnetic Battle Management' (EMBM)?", "id": "Manajemen dinamis spektrum di pertempuran." },
  { "en": "Tujuan EMBM (Electromagnetic Battle Management)?", "id": "De-konflik spektrum, alokasi dinamis." },
  { "en": "Apa itu 'EW cell'?", "id": "Tim personel yang merencanakan EW." },
  { "en": "Apa itu 'effects-based operations' (EBO)?", "id": "Operasi yang fokus pada hasil." },
  { "en": "Apa itu 'electromagnetic susceptibility'?", "id": "Tingkat kerentanan sistem terhadap EMI." },
  { "en": "Apa itu 'inter-system interference'?", "id": "Interferensi antar sistem elektronik kawan." },
  { "en": "Apa itu 'side-channel attack'?", "id": "Serangan berbasis informasi non-primer." },
  { "en": "Contoh 'side-channel attack'?", "id": "Analisis konsumsi daya, emisi EM." },
  { "en": "Apa itu 'TEMPEST'?", "id": "Standar untuk membatasi emisi EM." },
  { "en": "Tujuan TEMPEST?", "id": "Mencegah penyadapan melalui emisi." },
  { "en": "Ancaman EW pada 'power grid'?", "id": "Serangan EMP/HPM pada gardu listrik." },
  { "en": "Ancaman EW pada 'SCADA system'?", "id": "Jamming/spoofing link kontrol industri." },
  { "en": "Kepanjangan SCADA?", "id": "Supervisory Control and Data Acquisition." },
  { "en": "Apa itu 'sidelobe blanking'?", "id": "Mematikan penerima saat sinyal di sidelobe." },
  { "en": "Apa itu 'guard band'?", "id": "Pita frekuensi kosong antar kanal." },
  { "en": "Tujuan 'guard band'?", "id": "Mengurangi interferensi kanal bersebelahan." },
  { "en": "Apa itu 'constant false alarm rate' (CFAR)?", "id": "Menjaga tingkat alarm palsu konstan." },
  { "en": "Kepanjangan CFAR?", "id": "Constant False Alarm Rate." },
  { "en": "Apa itu 'track-before-detect' (TBD)?", "id": "Melacak target sebelum deteksi formal." },
  { "en": "Keuntungan TBD (Track-Before-Detect)?", "id": "Mendeteksi target SNR sangat rendah." },
  { "en": "Apa itu 'data fusion' di EW?", "id": "Menggabungkan data dari berbagai sensor." },
  { "en": "Tujuan 'data fusion'?", "id": "Meningkatkan akurasi gambaran situasi." },
  { "en": "Apa itu 'M-ary signaling'?", "id": "Skema modulasi dengan 'M' simbol." },
  { "en": "Apa itu 'adversarial AI' di EW?", "id": "AI yang dirancang menipu AI musuh." },
  { "en": "Tantangan data latih AI EW?", "id": "Data musuh langka dan selalu berubah." },
  { "en": "Apa itu 'transfer learning' untuk EW?", "id": "Adaptasi model AI ke ancaman baru." },
  { "en": "Peran 'neural network' di EW?", "id": "Klasifikasi sinyal, identifikasi ancaman." },
  { "en": "Apa itu 'cognitive radio'?", "id": "Radio cerdas yang beradaptasi spektrum." },
  { "en": "Apa itu 'dynamic spectrum access' (DSA)?", "id": "Teknik radio kognitif akses spektrum." },
  { "en": "Siklus OODA dalam EW?", "id": "Observe, Orient, Decide, and Act." },
  { "en": "Tahap 'Observe' dalam siklus OODA?", "id": "Mendeteksi sinyal di lingkungan EM." },
  { "en": "Tahap 'Orient' dalam siklus OODA?", "id": "Analisa dan identifikasi sinyal." },
  { "en": "Tahap 'Decide' dalam siklus OODA?", "id": "Memilih tindakan balasan EW terbaik." },
  { "en": "Tahap 'Act' dalam siklus OODA?", "id": "Mengeksekusi tindakan (jamming, spoofing)." },
  { "en": "Jenis DEW (Directed Energy Weapon) utama?", "id": "Laser dan High-Power Microwave (HPM)." },
  { "en": "Efek 'laser dazzler'?", "id": "Membutakan sementara sensor optik musuh." },
  { "en": "Efek 'laser-induced damage'?", "id": "Kerusakan permanen pada sensor/struktur." },
  { "en": "Apa itu 'microwave weapon'?", "id": "Merusak elektronik dengan gelombang mikro." },
  { "en": "Countermeasure terhadap DEW (Directed Energy Weapon)?", "id": "Material reflektif, sensor diperkeras." },
  { "en": "Apa itu 'passive radar'?", "id": "Radar yang gunakan sinyal pihak ketiga." },
  { "en": "Sinonim 'passive radar'?", "id": "Passive Coherent Location (PCL)." },
  { "en": "Keuntungan 'multilateration' untuk deteksi?", "id": "Deteksi target dari banyak sudut." },
  { "en": "Mengapa radar VHF/UHF deteksi stealth?", "id": "Panjang gelombangnya lebih besar." },
  { "en": "Apa itu 'resonance effect' pada stealth?", "id": "Fitur pesawat beresonansi dengan radar." },
  { "en": "Apa itu 'anechoic chamber'?", "id": "Ruangan bebas gema untuk tes antena." },
  { "en": "Tujuan 'anechoic chamber'?", "id": "Mengisolasi perangkat dari interferensi luar." },
  { "en": "Apa itu 'hardware-in-the-loop' (HWIL) simulation?", "id": "Simulasi EW dengan perangkat keras asli." },
  { "en": "Tujuan simulasi HWIL (Hardware-in-the-Loop)?", "id": "Menguji sistem di lingkungan virtual." },
  { "en": "Apa itu 'EW test range'?", "id": "Area terbuka untuk uji coba EW." },
  { "en": "Apa itu 'threat emitter simulator'?", "id": "Perangkat yang meniru sinyal ancaman." },
  { "en": "Tujuan 'threat simulator'?", "id": "Menguji RWR dan sistem ESM/ES." },
  { "en": "Apa itu 'figure of merit' (FOM)?", "id": "Ukuran performa kuantitatif sistem EW." },
  { "en": "Apa itu 'field testing'?", "id": "Pengujian sistem di lingkungan operasional." },
  { "en": "Ancaman EW pada jaringan 5G?", "id": "Jamming, spoofing, dan intersepsi sinyal." },
  { "en": "Apa itu 'beamforming' di 5G?", "id": "Mengarahkan sinyal langsung ke pengguna." },
  { "en": "Implikasi 'beamforming' untuk ES?", "id": "Lebih sulit untuk mencegat sinyal." },
  { "en": "Apa itu 'network slicing' di 5G?", "id": "Membuat jaringan virtual terisolasi." },
  { "en": "Ancaman EW pada 'Internet of Things' (IoT)?", "id": "Mengganggu atau mengambil alih perangkat." },
  { "en": "Ancaman pada 'Automatic Dependent Surveillance-Broadcast' (ADS-B)?", "id": "Spoofing data lokasi dan identitas pesawat." },
  { "en": "Kepanjangan ADS-B?", "id": "Automatic Dependent Surveillance-Broadcast." },
  { "en": "Ancaman pada 'Automatic Identification System' (AIS)?", "id": "Spoofing lokasi dan identitas kapal." },
  { "en": "Kepanjangan AIS?", "id": "Automatic Identification System." },
  { "en": "Apa itu 'Fast Fourier Transform' (FFT)?", "id": "Algoritma untuk analisis spektrum sinyal." },
  { "en": "Apa itu 'spectrogram'?", "id": "Visualisasi spektrum sinyal terhadap waktu." },
  { "en": "Apa itu 'waterfall display'?", "id": "Tipe spectrogram umum untuk ES." },
  { "en": "Apa itu 'cyclostationary analysis'?", "id": "Mendeteksi sinyal tersembunyi dalam noise." },
  { "en": "Apa itu 'wavelet transform'?", "id": "Analisis sinyal untuk fitur sesaat." },
  { "en": "Apa itu 'constellation diagram'?", "id": "Visualisasi sinyal modulasi digital." },
  { "en": "Tujuan 'constellation diagram'?", "id": "Menganalisis kualitas dan tipe modulasi." },
  { "en": "Apa itu 'eye diagram'?", "id": "Visualisasi kualitas sinyal data digital." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi atau pergeseran waktu sinyal." },
  { "en": "Apa itu 'carrier-to-noise ratio' (C/N)?", "id": "Rasio daya sinyal carrier-noise." },
  { "en": "Pilar utama 'Information Warfare' (IW)?", "id": "EW, PSYOP, OPSEC, MILDEC, Cyber." },
  { "en": "Definisi PSYOP (Psychological Operations)?", "id": "Mempengaruhi emosi dan pikiran audiens." },
  { "en": "Media penyebaran PSYOP?", "id": "Radio, selebaran, internet, televisi." },
  { "en": "Definisi MILDEC (Military Deception)?", "id": "Menipu pembuat keputusan militer musuh." },
  { "en": "Contoh MILDEC (Military Deception)?", "id": "Membuat pasukan atau tank palsu." },
  { "en": "Definisi OPSEC (Operations Security)?", "id": "Melindungi informasi kritis dari musuh." },
  { "en": "Langkah utama OPSEC (Operations Security)?", "id": "Identifikasi, analisis, terapkan countermeasures." },
  { "en": "Apa itu 'counter-intelligence' (CI)?", "id": "Upaya melawan aktivitas spionase musuh." },
  { "en": "Hubungan CI (Counter-Intelligence) dan ES?", "id": "CI gunakan data ES untuk deteksi." },
  { "en": "Apa itu 'public affairs' (PA) dalam IW?", "id": "Menyampaikan informasi akurat ke publik." },
  { "en": "Apa itu 'jamming triangulation'?", "id": "Geolokasi jammer dari beberapa stasiun." },
  { "en": "Apa itu 'deception detection'?", "id": "Mendeteksi bahwa suatu sinyal palsu." },
  { "en": "Metode 'deception detection'?", "id": "Analisis anomali parameter sinyal." },
  { "en": "Apa itu 'distributed aperture system' (DAS)?", "id": "Jaringan sensor optik di platform." },
  { "en": "Fungsi DAS (Distributed Aperture System)?", "id": "Peringatan 360° terhadap ancaman rudal." },
  { "en": "Teknik EP melawan 'follower jammer'?", "id": "Mengubah frekuensi lebih cepat." },
  { "en": "Apa itu 'trojan' dalam EW?", "id": "Sinyal palsu dalam sinyal kawan." },
  { "en": "Apa itu 'multipath exploitation'?", "id": "Memanfaatkan pantulan sinyal untuk deteksi." },
  { "en": "Apa itu 'polarimetric radar'?", "id": "Radar yang gunakan polarisasi berbeda." },
  { "en": "Tujuan 'polarimetric radar'?", "id": "Identifikasi bentuk dan material target." },
  { "en": "Apa itu 'bistatic fence'?", "id": "Pagar deteksi dari pemancar-penerima terpisah." },
  { "en": "Tujuan 'bistatic fence'?", "id": "Mendeteksi target yang melintasinya." },
  { "en": "Apa itu 'millimeter wave' (MMW) imaging?", "id": "Pencitraan pasif pakai frekuensi MMW." },
  { "en": "Kegunaan MMW (Millimeter Wave) imaging?", "id": "Deteksi senjata, lihat tembus kabut." },
  { "en": "Apa itu 'terahertz' (THz) imaging?", "id": "Pencitraan pada spektrum terahertz." },
  { "en": "Apa itu 'signal conditioning'?", "id": "Memproses sinyal untuk optimalkan analisa." },
  { "en": "Ancaman EW pada alat pacu jantung?", "id": "Gangguan atau reprogram dari luar." },
  { "en": "Ancaman EW pada pompa insulin?", "id": "Mengubah dosis dari jarak jauh." },
  { "en": "Apa itu 'wireless body area network' (WBAN)?", "id": "Jaringan sensor nirkabel di tubuh." },
  { "en": "Kerentanan WBAN (Wireless Body Area Network)?", "id": "Intersepsi data kesehatan, jamming." },
  { "en": "Apa itu 'biometric spoofing'?", "id": "Memalsukan data biometrik (sidik jari)." },
  { "en": "Apa itu 'gait recognition'?", "id": "Pengenalan individu dari cara berjalan." },
  { "en": "Ancaman EW pada 'gait recognition'?", "id": "Menipu sensor dengan data palsu." },
  { "en": "Tantangan EW di perkotaan?", "id": "Multipath, clutter, kepadatan sinyal tinggi." },
  { "en": "Apa itu 'urban canyon' effect?", "id": "Sinyal GPS terhalang gedung tinggi." },
  { "en": "Solusi navigasi di 'urban canyon'?", "id": "Sensor inersia, SLAM, visi." },
  { "en": "Kepanjangan SLAM?", "id": "Simultaneous Localization and Mapping." },
  { "en": "Tantangan ES di perkotaan?", "id": "Membedakan sinyal target dari sipil." },
  { "en": "Teknik mitigasi multipath?", "id": "Antena diversitas, equalization, RAKE receiver." },
  { "en": "Apa itu 'cognitive spectrum management'?", "id": "Manajemen spektrum cerdas di lingkungan padat." },
  { "en": "Apa itu 'geofencing'?", "id": "Membuat batas virtual geografis." },
  { "en": "Aplikasi 'geofencing' di C-UAS?", "id": "Menonaktifkan drone di area terlarang." },
  { "en": "Apa itu 'leave-behind' sensor?", "id": "Sensor kecil yang ditinggalkan untuk memantau." },
  { "en": "Apa itu 'EW data fusion'?", "id": "Menggabungkan data EW dari banyak sensor." },
  { "en": "Tujuan 'EW data fusion'?", "id": "Menciptakan gambaran situasi yang akurat." },
  { "en": "Apa itu 'Common Operating Picture' (COP)?", "id": "Tampilan operasional tunggal yang terintegrasi." },
  { "en": "Peran EW dalam COP (Common Operating Picture)?", "id": "Menyediakan data ancaman dan spektrum." },
  { "en": "Apa itu 'sensor fusion engine'?", "id": "Perangkat lunak yang melakukan data fusion." },
  { "en": "Tantangan 'big data' di EW?", "id": "Volume, kecepatan, dan variasi data." },
  { "en": "Apa itu 'data dissemination'?", "id": "Proses menyebarkan data intelijen." },
  { "en": "Apa itu 'pedigree' data?", "id": "Informasi asal-usul dan keandalan data." },
  { "en": "Apa itu 'track correlation'?", "id": "Mencocokkan jejak dari sensor berbeda." },
  { "en": "Apa itu 'interoperability'?", "id": "Kemampuan sistem berbeda bekerja sama." },
  { "en": "Apa itu OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Modulasi dengan banyak sub-carrier." },
  { "en": "Kerentanan OFDM?", "id": "Sensitif terhadap pergeseran frekuensi." },
  { "en": "Serangan pada 'pilot tones' OFDM?", "id": "Mengganggu sinyal referensi sinkronisasi." },
  { "en": "Bagaimana 'jamming' mempengaruhi QAM (Quadrature Amplitude Modulation)?", "id": "Meningkatkan bit error rate (BER)." },
  { "en": "Apa itu 'protocol-aware jamming'?", "id": "Jamming yang mengeksploitasi kelemahan protokol." },
  { "en": "Contoh 'protocol-aware jamming'?", "id": "Menyerang paket kontrol atau preamble." },
  { "en": "Apa itu 'symbol timing recovery'?", "id": "Sinkronisasi di level simbol penerima." },
  { "en": "Serangan pada 'symbol timing recovery'?", "id": "Menyebabkan kesalahan sampling sinyal." },
  { "en": "Apa itu 'equalizer' pada modem?", "id": "Mengkompensasi distorsi kanal komunikasi." },
  { "en": "Serangan pada 'equalizer'?", "id": "Mengacaukan proses adaptasi equalizer." },
  { "en": "Apa itu 'Rules of Engagement' (ROE) untuk EW?", "id": "Aturan penggunaan kekuatan dalam EW." },
  { "en": "Pertimbangan LOAC dalam 'jamming'?", "id": "Potensi dampak pada sistem sipil." },
  { "en": "Kepanjangan LOAC?", "id": "Law of Armed Conflict." },
  { "en": "Apa itu 'collateral damage' dari EW?", "id": "Gangguan tak sengaja pada non-kombatan." },
  { "en": "Contoh 'collateral damage' EW?", "id": "Mengganggu GPS rumah sakit, penerbangan." },
  { "en": "Apa itu 'cognitive load' EWO?", "id": "Beban mental pada operator EW." },
  { "en": "Apa itu 'human-machine interface' (HMI) EW?", "id": "Tampilan dan kontrol sistem EW." },
  { "en": "Desain HMI (Human-Machine Interface) yang baik?", "id": "Intuitif dan mengurangi beban kerja." },
  { "en": "Apa itu 'decision aid' EW?", "id": "Perangkat lunak bantu keputusan operator." },
  { "en": "Apa itu 'dual-use' technology?", "id": "Teknologi sipil dengan aplikasi militer." },
  { "en": "Apa itu 'spectrum maneuver'?", "id": "Bergerak di spektrum untuk keuntungan." },
  { "en": "Tujuan 'spectrum maneuver'?", "id": "Menghindari ancaman, menemukan spektrum bersih." },
  { "en": "Apa itu 'electromagnetic ambush'?", "id": "Menunggu target masuk zona EW." },
  { "en": "Apa itu 'E-ECCM' (atau ECCCM)?", "id": "Tindakan melawan proteksi elektronik musuh." },
  { "en": "Contoh E-ECCM?", "id": "Mengatasi 'frequency hopping' musuh." },
  { "en": "Apa itu 'hardware trojan'?", "id": "Sirkuit berbahaya yang disisipkan." },
  { "en": "Ancaman 'hardware trojan'?", "id": "Menciptakan celah keamanan, kegagalan sistem." },
  { "en": "Apa itu 'supply chain interdiction'?", "id": "Serangan pada rantai pasok komponen." },
  { "en": "Apa itu 'graceful degradation'?", "id": "Sistem tetap berfungsi parsial saat diserang." },
  { "en": "Pentingnya 'graceful degradation'?", "id": "Sistem tidak gagal total saat diserang." },
  { "en": "Definisi 'Multi-Domain Operations' (MDO)?", "id": "Operasi terintegrasi di semua domain." },
  { "en": "Peran EW dalam MDO (Multi-Domain Operations)?", "id": "Memungkinkan dan melindungi operasi lain." },
  { "en": "Apa itu 'cross-domain fires'?", "id": "Serangan dari satu domain ke lainnya." },
  { "en": "Contoh 'cross-domain fires' EW?", "id": "Jammer udara dukung pasukan darat." },
  { "en": "Apa itu 'JADC2'?", "id": "Joint All-Domain Command and Control." },
  { "en": "Tujuan JADC2?", "id": "Menghubungkan semua sensor ke penembak." },
  { "en": "Hubungan JADC2 dan EW?", "id": "EW melindungi dan serang jaringan JADC2." },
  { "en": "Apa itu 'sensor-to-shooter' link?", "id": "Jaringan yang hubungkan sensor-penembak." },
  { "en": "Ancaman EW pada 'sensor-to-shooter' link?", "id": "Jamming atau spoofing data link." },
  { "en": "Apa itu 'interferometry' dalam DF?", "id": "Mengukur beda fasa antar antena." },
  { "en": "Keuntungan 'interferometry'?", "id": "Akurasi penentuan arah sangat tinggi." },
  { "en": "Apa itu 'signal parameterization'?", "id": "Proses ekstraksi parameter kunci sinyal." },
  { "en": "Apa itu 'library matching'?", "id": "Mencocokkan sinyal dengan database ancaman." },
  { "en": "Apa itu 'unintentional modulation'?", "id": "Modulasi tak sengaja pada sinyal." },
  { "en": "Kegunaan 'unintentional modulation'?", "id": "Untuk identifikasi pemancar spesifik (SEI)." },
  { "en": "Apa itu 'carrier frequency offset' (CFO)?", "id": "Beda frekuensi osilator pengirim-penerima." },
  { "en": "Apa itu 'sampling clock offset' (SCO)?", "id": "Beda laju sampling pengirim-penerima." },
  { "en": "Apa itu 'kurtosis' sinyal?", "id": "Ukuran 'kepuncakan' distribusi sinyal." },
  { "en": "Kegunaan 'kurtosis' di EW?", "id": "Membedakan tipe modulasi sinyal." },
  { "en": "Target EW pada sistem finansial?", "id": "Jaringan ATM, transfer data bank." },
  { "en": "Metode serangan EW finansial?", "id": "Jamming atau spoofing sinyal waktu." },
  { "en": "Apa itu 'timing reference' untuk bank?", "id": "Sinyal GPS untuk sinkronisasi transaksi." },
  { "en": "Apa itu 'counter-insurgency' (COIN) EW?", "id": "EW untuk melawan operasi pemberontak." },
  { "en": "Fokus COIN (Counter-Insurgency) EW?", "id": "Mengganggu komunikasi komersial musuh." },
  { "en": "Apa itu 'digital forensics' RF?", "id": "Investigasi perangkat dari emisi RF-nya." },
  { "en": "EW pada 'smart grid'?", "id": "Mengganggu sistem distribusi listrik cerdas." },
  { "en": "EW pada 'autonomous vehicles'?", "id": "Jamming/spoofing GPS, Lidar, radar." },
  { "en": "Apa itu 'antenna gain'?", "id": "Kemampuan antena memfokuskan energi." },
  { "en": "Apa itu 'antenna efficiency'?", "id": "Rasio daya radiasi terhadap daya input." },
  { "en": "Apa itu 'beamwidth' antena?", "id": "Lebar sudut dari pancaran utama." },
  { "en": "Hubungan gain dan beamwidth?", "id": "Gain tinggi berarti beamwidth sempit." },
  { "en": "Apa itu 'near field' antena?", "id": "Area reaktif sangat dekat antena." },
  { "en": "Apa itu 'far field' antena?", "id": "Area radiasi jauh dari antena." },
  { "en": "Apa itu 'knife-edge diffraction'?", "id": "Pembelokan gelombang di sekitar objek." },
  { "en": "Apa itu 'tropospheric ducting'?", "id": "Perambatan sinyal jauh di troposfer." },
  { "en": "Dampak 'tropospheric ducting'?", "id": "Komunikasi/radar melampaui batas cakrawala." },
  { "en": "Apa itu 'Faraday rotation'?", "id": "Rotasi polarisasi sinyal di ionosfer." },
  { "en": "Apa itu 'scan-on-receive' jamming?", "id": "Jammer menipu radar saat menerima." },
  { "en": "Apa itu 'track-while-scan' (TWS) radar?", "id": "Lacak banyak target sambil memindai." },
  { "en": "Kerentanan TWS (Track-While-Scan) radar?", "id": "Dapat ditipu dengan target palsu." },
  { "en": "Faktor penentu 'burn-through range'?", "id": "Daya radar, gain, RCS target." },
  { "en": "Apa itu 'stand-in jamming'?", "id": "Jammer beroperasi sangat dekat target." },
  { "en": "Keuntungan 'stand-in jamming'?", "id": "Butuh daya kecil, sangat efektif." },
  { "en": "Apa itu 'quieting' dalam EP?", "id": "Mengurangi emisi sendiri hindari deteksi." },
  { "en": "Apa itu 'selective jamming'?", "id": "Memilih target jamming paling prioritas." },
  { "en": "Apa itu 'Electromagnetic Spectrum Manager' (EMSM)?", "id": "Personel yang mengelola penggunaan spektrum." },
  { "en": "Tugas EMSM (Electromagnetic Spectrum Manager)?", "id": "Alokasi frekuensi dan dekonfliksi." },
  { "en": "Apa itu 'EW Coordination Cell' (EWCC)?", "id": "Pusat koordinasi untuk operasi EW." },
  { "en": "Kepanjangan CONOPS?", "id": "Concept of Operations." },
  { "en": "Isi CONOPS EW?", "id": "Cara sistem EW digunakan taktis." },
  { "en": "Kepanjangan TTPs?", "id": "Tactics, Techniques, and Procedures." },
  { "en": "Isi TTPs EW?", "id": "Prosedur spesifik untuk misi EW." },
  { "en": "Apa itu 'EW modeling and simulation' (M&S)?", "id": "Menciptakan model virtual pertempuran EW." },
  { "en": "Tujuan M&S (Modeling and Simulation) di EW?", "id": "Analisis, pelatihan, dan pengembangan taktik." },
  { "en": "Apa itu 'engagement-level model'?", "id": "Model interaksi satu-lawan-satu." },
  { "en": "Apa itu 'mission-level model'?", "id": "Model simulasi seluruh misi pertempuran." },
  { "en": "Apa itu 'fidelity' dalam model?", "id": "Tingkat kemiripan model dengan realitas." },
  { "en": "Apa itu 'propagation model'?", "id": "Model matematika perambatan gelombang radio." },
  { "en": "Apa itu 'constructive simulation'?", "id": "Simulasi dengan entitas virtual (wargaming)." },
  { "en": "Apa itu 'virtual simulation'?", "id": "Manusia berinteraksi dengan sistem simulasi." },
  { "en": "Apa itu 'live simulation'?", "id": "Latihan dengan pasukan dan sistem nyata." },
  { "en": "Apa itu 'digital twin' dalam EW?", "id": "Replika digital dari sistem EW fisik." },
  { "en": "Apa itu NAVWAR (Navigation Warfare)?", "id": "Perang untuk kontrol informasi navigasi." },
  { "en": "Kepanjangan PNT?", "id": "Positioning, Navigation, and Timing." },
  { "en": "Apa itu 'Assured PNT'?", "id": "Upaya memastikan PNT di lingkungan sulit." },
  { "en": "Sumber 'timing' alternatif selain GPS?", "id": "Chip-Scale Atomic Clock (CSAC)." },
  { "en": "Apa itu 'celestial navigation'?", "id": "Navigasi menggunakan posisi benda langit." },
  { "en": "Apa itu 'vision-based navigation' (VBN)?", "id": "Navigasi pakai kamera dan peta visual." },
  { "en": "Kerentanan VBN (Vision-Based Navigation)?", "id": "Cuaca buruk, kegelapan, lingkungan berubah." },
  { "en": "Apa itu 'signals of opportunity' (SOP)?", "id": "Menggunakan sinyal sekitar untuk navigasi." },
  { "en": "Contoh SOP (Signals of Opportunity)?", "id": "Sinyal siaran TV, Wi-Fi, seluler." },
  { "en": "Apa itu 'anti-spoofing' GPS?", "id": "Teknik mendeteksi dan menolak sinyal palsu." },
  { "en": "Apa itu MIMO (Multiple-Input Multiple-Output)?", "id": "Penggunaan banyak antena pengirim-penerima." },
  { "en": "Aplikasi MIMO di EW?", "id": "Meningkatkan ketahanan dan deteksi sinyal." },
  { "en": "Apa itu 'smart antenna'?", "id": "Antena yang beradaptasi dengan lingkungan." },
  { "en": "Sinonim 'smart antenna'?", "id": "Adaptive array antenna." },
  { "en": "Apa itu 'reconfigurable antenna'?", "id": "Antena yang bisa ubah frekuensi/pola." },
  { "en": "Apa itu 'plasma antenna'?", "id": "Antena yang gunakan gas terionisasi." },
  { "en": "Keuntungan 'plasma antenna'?", "id": "Bisa diaktifkan dan dinonaktifkan cepat." },
  { "en": "Apa itu 'frequency selective surface' (FSS)?", "id": "Permukaan yang memfilter frekuensi." },
  { "en": "Aplikasi FSS (Frequency Selective Surface)?", "id": "Radome, stealth, dan shielding." },
  { "en": "Ancaman EW pada NFC (Near-Field Communication)?", "id": "Penyadapan dan korupsi data." },
  { "en": "Apa itu 'EW resource manager'?", "id": "Sistem yang alokasikan aset EW." },
  { "en": "Tugas 'EW resource manager'?", "id": "Prioritaskan ancaman, alokasikan daya jammer." },
  { "en": "Apa itu 'threat evaluation'?", "id": "Menilai tingkat bahaya setiap ancaman." },
  { "en": "Apa itu 'weapon assignment'?", "id": "Menugaskan 'senjata' (jammer) ke target." },
  { "en": "Apa itu 'time-sharing' jammer?", "id": "Jammer melayani banyak target bergantian." },
  { "en": "Apa itu 'power management' jammer?", "id": "Mengatur daya pancar sesuai kebutuhan." },
  { "en": "Tujuan 'power management'?", "id": "Efisiensi daya, mengurangi kemungkinan deteksi." },
  { "en": "Apa itu 'domain knowledge' dalam AI?", "id": "Pengetahuan ahli yang dimasukkan ke AI." },
  { "en": "Pentingnya 'domain knowledge' EW?", "id": "Membantu AI membuat keputusan taktis." },
  { "en": "Apa itu 'prioritization scheme'?", "id": "Skema atau aturan untuk prioritas." },
  { "en": "Peran 'chaff' di WWII?", "id": "Menipu radar pertahanan udara Jerman." },
  { "en": "Perang apa yang disebut 'first electronic war'?", "id": "Perang Vietnam oleh sebagian analis." },
  { "en": "Pelajaran EW dari Perang Yom Kippur 1973?", "id": "Pentingnya sistem EW yang modern." },
  { "en": "Apa itu 'Operation Mole Cricket 19'?", "id": "Operasi SEAD Israel di Lembah Bekaa." },
  { "en": "Hasil 'Operation Mole Cricket 19' (1982)?", "id": "Kehancuran masif pertahanan udara Suriah." },
  { "en": "Pelajaran dari Lembah Bekaa?", "id": "Pentingnya koordinasi EW, drone, pesawat." },
  { "en": "Peran EW di Perang Teluk Pertama?", "id": "Melumpuhkan komando dan radar Irak." },
  { "en": "Apa itu 'digital sneak attack'?", "id": "Serangan siber untuk melumpuhkan radar." },
  { "en": "Apa itu 'COMFY LEVIATHAN'?", "id": "Program 'digital sneak attack' Israel." },
  { "en": "Pelajaran EW dari konflik Georgia 2008?", "id": "Kelemahan pada komunikasi dan GPS." },
  { "en": "Hubungan kriptografi dan EW?", "id": "Melindungi informasi, mempersulit ES." },
  { "en": "Kepanjangan TRANSEC?", "id": "Transmission Security." },
  { "en": "Tujuan TRANSEC?", "id": "Melindungi transmisi dari deteksi/eksploitasi." },
  { "en": "Kepanjangan COMSEC?", "id": "Communications Security." },
  { "en": "Tujuan COMSEC?", "id": "Melindungi isi atau informasi komunikasi." },
  { "en": "Apa itu 'traffic flow security'?", "id": "Menyembunyikan pola lalu lintas komunikasi." },
  { "en": "Metode 'traffic flow security'?", "id": "Mengirimkan lalu lintas data palsu." },
  { "en": "Apa itu 'steganography'?", "id": "Menyembunyikan pesan dalam media lain." },
  { "en": "Beda steganografi dan kriptografi?", "id": "Steganografi sembunyikan keberadaan pesan." },
  { "en": "Apa itu 'signal watermarking'?", "id": "Menanamkan informasi tersembunyi pada sinyal." },
  { "en": "Apa itu 'EW payload'?", "id": "Muatan sistem EW pada platform." },
  { "en": "Keuntungan drone untuk 'stand-in jamming'?", "id": "Risiko rendah, bisa lebih dekat." },
  { "en": "Apa itu 'drone swarm EW'?", "id": "Serangan EW oleh banyak drone." },
  { "en": "Apa itu 'counter-swarm EW'?", "id": "Taktik EW untuk melawan gerombolan drone." },
  { "en": "Metode 'counter-swarm EW'?", "id": "Jamming terarah, HPM area luas." },
  { "en": "Kepanjangan USV?", "id": "Unmanned Surface Vehicle." },
  { "en": "Peran EW pada USV?", "id": "Pengintaian ES, umpan, dan jamming." },
  { "en": "Kepanjangan UUV?", "id": "Unmanned Underwater Vehicle." },
  { "en": "Peran EW pada UUV?", "id": "Pemetaan akustik, pengintaian sinyal." },
  { "en": "Apa itu 'mother ship' concept?", "id": "Platform induk yang meluncurkan drone." },
  { "en": "Apa itu 'non-lethal weapon' (NLW)?", "id": "Senjata yang dirancang tidak mematikan." },
  { "en": "Contoh NLW (Non-Lethal Weapon) berbasis RF?", "id": "Active Denial System (ADS)." },
  { "en": "Apa itu 'Active Denial System' (ADS)?", "id": "Senjata gelombang milimeter untuk massa." },
  { "en": "Efek ADS (Active Denial System)?", "id": "Sensasi panas tak tertahankan di kulit." },
  { "en": "Apa itu 'auditory effect' dari RF?", "id": "Mendengar suara dari pulsa RF." },
  { "en": "Nama lain 'auditory effect'?", "id": "Frey effect atau microwave auditory effect." },
  { "en": "Potensi penggunaan 'Frey effect'?", "id": "Mengirimkan suara langsung ke kepala." },
  { "en": "Apa itu 'incapacitant'?", "id": "Zat atau energi yang melumpuhkan." },
  { "en": "Apa itu 'psychotronic weapon'?", "id": "Senjata hipotetis yang pengaruhi pikiran." },
  { "en": "Status 'psychotronic weapon'?", "id": "Sebagian besar dianggap fiksi ilmiah." },
  { "en": "Apa itu 'solar flare'?", "id": "Ledakan radiasi dari permukaan matahari." },
  { "en": "Dampak 'solar flare' pada EW?", "id": "Mengganggu komunikasi HF dan SATCOM." },
  { "en": "Apa itu 'geomagnetic storm'?", "id": "Gangguan besar pada magnetosfer bumi." },
  { "en": "Dampak 'geomagnetic storm'?", "id": "Masalah pada GPS, satelit, listrik." },
  { "en": "Frekuensi paling terpengaruh 'scintillation'?", "id": "VHF, UHF, dan L-band (GPS)." },
  { "en": "Bagaimana hujan mempengaruhi sinyal Ku/Ka band?", "id": "Menyebabkan pelemahan signifikan ('rain fade')." },
  { "en": "Apa itu 'atmospheric absorption'?", "id": "Pelemahan sinyal oleh gas atmosfer." },
  { "en": "Gas atmosfer penyerap RF utama?", "id": "Oksigen dan uap air." },
  { "en": "Apa itu 'thermal blooming'?", "id": "Efek laser berenergi tinggi di atmosfer." },
  { "en": "Dampak 'thermal blooming'?", "id": "Menyebarkan dan melemahkan sinar laser." },
  { "en": "Apa itu 'EW system calibration'?", "id": "Menyesuaikan sistem agar akurat kembali." },
  { "en": "Mengapa kalibrasi EW penting?", "id": "Memastikan akurasi pengukuran dan penargetan." },
  { "en": "Apa itu 'lifecycle management' EW?", "id": "Mengelola sistem dari akuisisi-pembuangan." },
  { "en": "Apa itu 'software-defined' EW system?", "id": "Sistem yang fungsinya ditentukan perangkat lunak." },
  { "en": "Keuntungan 'software-defined' EW?", "id": "Mudah diperbarui dan dikonfigurasi ulang." },
  { "en": "Apa itu 'Built-In Test' (BIT)?", "id": "Fungsi tes mandiri dalam sistem." },
  { "en": "Tujuan BIT (Built-In Test)?", "id": "Mendeteksi kegagalan sistem dengan cepat." },
  { "en": "Apa itu 'spares provisioning'?", "id": "Penyediaan suku cadang untuk perbaikan." },
  { "en": "Apa itu 'technology refresh'?", "id": "Pembaruan komponen teknologi dalam sistem." },
  { "en": "Apa itu 'mean time between failures' (MTBF)?", "id": "Rata-rata waktu antar kegagalan sistem." },
  { "en": "Komponen utama pemancar jammer?", "id": "Power Amplifier (PA)." },
  { "en": "Dua jenis utama 'Power Amplifier'?", "id": "Solid-State dan Vacuum Tube." },
  { "en": "Contoh 'vacuum tube' PA?", "id": "Traveling Wave Tube (TWT)." },
  { "en": "Kepanjangan TWT?", "id": "Traveling Wave Tube." },
  { "en": "Keuntungan TWT (Traveling Wave Tube)?", "id": "Daya sangat tinggi, bandwidth lebar." },
  { "en": "Kelemahan TWT?", "id": "Ukuran besar, butuh tegangan tinggi." },
  { "en": "Contoh 'solid-state' PA?", "id": "Gallium Nitride (GaN)." },
  { "en": "Kepanjangan GaN?", "id": "Gallium Nitride." },
  { "en": "Keuntungan GaN (Gallium Nitride)?", "id": "Ukuran kecil, efisien, lebih andal." },
  { "en": "Kelemahan GaN?", "id": "Daya puncaknya lebih rendah dari TWT." },
  { "en": "Apa itu 'national EW strategy'?", "id": "Strategi negara untuk dominasi spektrum." },
  { "en": "Badan pengatur spektrum internasional?", "id": "International Telecommunication Union (ITU)." },
  { "en": "Peran ITU (International Telecommunication Union)?", "id": "Mengalokasikan spektrum frekuensi secara global." },
  { "en": "Apakah ada traktat pelarangan EW?", "id": "Tidak ada, diatur dalam LOAC." },
  { "en": "Apa itu 'spectrum sharing'?", "id": "Beberapa pengguna berbagi pita frekuensi sama." },
  { "en": "Apa itu 'sovereignty' spektrum?", "id": "Hak negara untuk mengatur spektrumnya." },
  { "en": "Apa itu 'escalation ladder' EW?", "id": "Tingkatan eskalasi dalam konflik EW." },
  { "en": "Contoh 'escalation' EW?", "id": "Dari jamming sementara ke destruksi fisik." },
  { "en": "Apa itu 'deterrence' dalam EW?", "id": "Mencegah serangan dengan kemampuan balasan." },
  { "en": "Apa itu 'de-escalation'?", "id": "Upaya menurunkan tensi konflik." },
  { "en": "Tantangan deteksi sinyal LPI?", "id": "Daya rendah, tersembunyi dalam noise." },
  { "en": "Kepanjangan LPI (lagi)?", "id": "Low Probability of Intercept." },
  { "en": "Teknik deteksi LPI?", "id": "Integrasi waktu panjang, analisis fitur." },
  { "en": "Apa itu 'long integration time'?", "id": "Mengumpulkan energi sinyal waktu lama." },
  { "en": "Apa itu 'radiometer'?", "id": "Penerima pasif sangat sensitif." },
  { "en": "Fungsi 'radiometer'?", "id": "Mendeteksi keberadaan energi RF." },
  { "en": "Apa itu 'feature-based detection'?", "id": "Deteksi berdasarkan fitur unik sinyal." },
  { "en": "Apa itu 'energy detection'?", "id": "Metode deteksi berbasis tingkat energi." },
  { "en": "Kelemahan 'energy detection'?", "id": "Tidak bisa bedakan sinyal dan noise." },
  { "en": "Bagaimana 'wide bandwidth' bantu deteksi LPI?", "id": "Menangkap seluruh energi sinyal lebar." },
  { "en": "Kepanjangan MUSIC (algorithm)?", "id": "Multiple Signal Classification." },
  { "en": "Fungsi algoritma MUSIC?", "id": "Estimasi arah datang sinyal (DF)." },
  { "en": "Kepanjangan ESPRIT (algorithm)?", "id": "Estimation of Signal Parameters via Rotational Invariance." },
  { "en": "Fungsi algoritma ESPRIT?", "id": "Estimasi arah datang akurasi tinggi." },
  { "en": "Apa itu 'blind signal separation' (BSS)?", "id": "Memisahkan sinyal tanpa info sumber." },
  { "en": "Apa itu 'principal component analysis' (PCA)?", "id": "Teknik reduksi dimensi data sinyal." },
  { "en": "Apa itu 'support vector machine' (SVM)?", "id": "Algoritma ML untuk klasifikasi sinyal." },
  { "en": "Apa itu 'decision tree' classifier?", "id": "Model klasifikasi berbasis aturan 'jika-maka'." },
  { "en": "Apa itu 'Kalman filter'?", "id": "Algoritma untuk estimasi dan lacak target." },
  { "en": "Aplikasi 'Kalman filter' di EW?", "id": "Pelacakan frekuensi atau posisi target." },
  { "en": "Apa itu LIDAR (Light Detection and Ranging)?", "id": "Radar yang menggunakan sinar laser." },
  { "en": "Ancaman EW pada LIDAR?", "id": "Spoofing dengan pulsa laser palsu." },
  { "en": "Apa itu 'laser communications' (LaserCom)?", "id": "Komunikasi menggunakan modulasi sinar laser." },
  { "en": "Keuntungan LaserCom?", "id": "Bandwidth sangat tinggi, sulit disadap." },
  { "en": "Kelemahan LaserCom?", "id": "Butuh 'line-of-sight', rentan cuaca." },
  { "en": "Countermeasure untuk 'laser designator'?", "id": "Detektor laser, asap, dan aerosol." },
  { "en": "Apa itu 'electro-optic countermeasure'?", "id": "Countermeasure terhadap sistem optik/IR." },
  { "en": "Contoh 'electro-optic countermeasure'?", "id": "DIRCM, obscurants (penghalang pandangan)." },
  { "en": "Apa itu 'laser dazzler'?", "id": "Laser untuk membutakan sensor sementara." },
  { "en": "Apa itu 'laser-based remote sensing'?", "id": "Penginderaan jauh menggunakan teknologi laser." },
  { "en": "Apa itu IoBT (Internet of Battlefield Things)?", "id": "Jaringan sensor dan perangkat di pertempuran." },
  { "en": "Kerentanan utama IoBT?", "id": "Permukaan serangan siber dan EW masif." },
  { "en": "Ancaman EA pada IoBT?", "id": "Jamming area luas, spoofing massal." },
  { "en": "Peluang ES dari IoBT musuh?", "id": "Memetakan jaringan dan posisi pasukan." },
  { "en": "Apa itu 'mesh network'?", "id": "Jaringan dimana setiap node terhubung." },
  { "en": "Apa itu 'sensor poisoning'?", "id": "Memberikan data input palsu ke sensor." },
  { "en": "Apa itu 'edge computing' di IoBT?", "id": "Pemrosesan data dilakukan dekat sensor." },
  { "en": "Manfaat 'edge computing'?", "id": "Mengurangi latensi dan kebutuhan bandwidth." },
  { "en": "EW pada 'wearable' tentara?", "id": "Jamming atau intersepsi data biometrik." },
  { "en": "Kerentanan terbesar IoBT?", "id": "Manajemen identitas dan akses." },
  { "en": "Kepanjangan HEMP?", "id": "High-Altitude Electromagnetic Pulse." },
  { "en": "Sumber HEMP?", "id": "Ledakan nuklir di atmosfer tinggi." },
  { "en": "Dampak HEMP?", "id": "Kerusakan elektronik skala sangat luas." },
  { "en": "Apa itu 'E1 pulse'?", "id": "Komponen HEMP pertama, sangat cepat." },
  { "en": "Apa itu 'E2 pulse'?", "id": "Komponen HEMP kedua, mirip petir." },
  { "en": "Apa itu 'E3 pulse'?", "id": "Komponen HEMP ketiga, lambat." },
  { "en": "Apa itu 'shielding effectiveness'?", "id": "Ukuran seberapa baik perisai bekerja." },
  { "en": "Satuan 'shielding effectiveness'?", "id": "Decibels (dB)." },
  { "en": "Apa itu 'grounding' untuk proteksi EMP?", "id": "Menyalurkan arus berlebih ke tanah." },
  { "en": "Apa itu 'Faraday cage'?", "id": "Selungkup pelindung dari medan elektromagnetik." },
  { "en": "Apa itu 'live-virtual-constructive' (LVC) training?", "id": "Menggabungkan tiga tipe simulasi EW." },
  { "en": "Tujuan LVC (Live-Virtual-Constructive) training?", "id": "Pelatihan realistis dalam skala besar." },
  { "en": "Apa itu 'EW range'?", "id": "Area untuk latihan EW dengan emiter." },
  { "en": "Tantangan pelatihan EW?", "id": "Meniru lingkungan EM yang kompleks." },
  { "en": "Apa itu 'Cryptologic Technician' (CT)?", "id": "Spesialis EW di Angkatan Laut AS." },
  { "en": "Pentingnya 'continuous learning' di EW?", "id": "Teknologi dan ancaman cepat berubah." },
  { "en": "Apa itu 'Red Team' dalam latihan?", "id": "Tim yang berperan sebagai musuh." },
  { "en": "Apa itu 'Blue Team' dalam latihan?", "id": "Tim yang berperan sebagai kawan." },
  { "en": "Apa itu 'White Team' dalam latihan?", "id": "Tim wasit dan kontrol latihan." },
  { "en": "Apa itu 'After Action Review' (AAR)?", "id": "Evaluasi dan pembelajaran setelah latihan." },
  { "en": "Peran EW dalam 'economic warfare'?", "id": "Mengganggu infrastruktur finansial dan logistik." },
  { "en": "Apa itu 'graph theory' di EW?", "id": "Analisis jaringan pakai node dan edge." },
  { "en": "Aplikasi 'graph theory'?", "id": "Menemukan node kritis jaringan komunikasi." },
  { "en": "Apa itu 'centrality analysis'?", "id": "Mengukur pentingnya sebuah node." },
  { "en": "Apa itu 'community detection'?", "id": "Menemukan kelompok dalam suatu jaringan." },
  { "en": "Tujuan 'community detection' COMINT?", "id": "Mengidentifikasi sel atau unit musuh." },
  { "en": "Apa itu 'choke point' jaringan?", "id": "Titik kritis yang jika diganggu, lumpuh." },
  { "en": "EW pada 'supply chain management' (SCM)?", "id": "Mengganggu pelacakan logistik (RFID, GPS)." },
  { "en": "Apa itu 'social network analysis' (SNA)?", "id": "Analisis struktur hubungan sosial/komunikasi." },
  { "en": "EW pada 'stock market'?", "id": "Mengganggu 'high-frequency trading' (HFT)." },
  { "en": "Apa itu 'autonomous EW'?", "id": "Sistem EW yang beroperasi tanpa manusia." },
  { "en": "Apa itu 'machine-to-machine' EW?", "id": "Sistem EW otonom melawan sistem otonom." },
  { "en": "Pertimbangan etis EW otonom?", "id": "Siapa yang bertanggung jawab atas tindakan." },
  { "en": "Apa itu 'human-in-the-loop'?", "id": "Manusia yang membuat keputusan akhir." },
  { "en": "Apa itu 'human-on-the-loop'?", "id": "Manusia mengawasi dan bisa intervensi." },
  { "en": "Apa itu 'human-out-of-the-loop'?", "id": "Sistem beroperasi sepenuhnya otonom." },
  { "en": "Apa itu 'dynamic tasking' EW?", "id": "Alokasi tugas EW otomatis saat misi." },
  { "en": "Peran AI dalam 'dynamic tasking'?", "id": "Mengoptimalkan penggunaan sumber daya EW." },
  { "en": "Apa itu 'collaborative autonomy'?", "id": "Sistem otonom bekerja sama capai tujuan." },
  { "en": "Contoh 'collaborative autonomy' EW?", "id": "Sekumpulan drone jammer yang terkoordinasi." },
  { "en": "Apa itu 'EW effectiveness measure'?", "id": "Ukuran seberapa efektif sistem EW." },
  { "en": "Apa itu 'Probability of Intercept' (PoI)?", "id": "Probabilitas berhasil mencegat suatu sinyal." },
  { "en": "Apa itu 'Probability of Detection' (Pd)?", "id": "Probabilitas berhasil mendeteksi suatu sinyal." },
  { "en": "Apa itu 'sortie'?", "id": "Satu misi penerbangan oleh satu pesawat." },
  { "en": "Apa itu 'denial effectiveness'?", "id": "Ukuran efektifitas pencegahan akses spektrum." },
  { "en": "Apa itu 'deception effectiveness'?", "id": "Ukuran efektifitas penipuan sistem musuh." },
  { "en": "Apa itu 'mission degradation'?", "id": "Penurunan kapabilitas misi akibat EW." },
  { "en": "Apa itu 'kill removal' dalam BDA?", "id": "Konfirmasi target hancur dari hilangnya emisi." },
  { "en": "Apa itu 'Measure of Performance' (MOP)?", "id": "Mengukur kinerja internal sistem." },
  { "en": "Apa itu 'Measure of Effectiveness' (MOE)?", "id": "Mengukur dampak sistem pada misi." },
  { "en": "Apa itu MFRFS (Multi-Function RF System)?", "id": "Sistem RF yang punya banyak fungsi." },
  { "en": "Fungsi dalam MFRFS?", "id": "Radar, EW, dan komunikasi (comms)." },
  { "en": "Keuntungan utama MFRFS?", "id": "Mengurangi ukuran, berat, dan daya (SWaP)." },
  { "en": "Kepanjangan SWaP?", "id": "Size, Weight, and Power." },
  { "en": "Apa itu 'shared aperture'?", "id": "Satu antena digunakan untuk banyak fungsi." },
  { "en": "Tantangan 'shared aperture'?", "id": "Mengelola alokasi waktu dan sumber daya." },
  { "en": "Apa itu 'resource manager' MFRFS?", "id": "Perangkat lunak pengatur alokasi fungsi." },
  { "en": "Apa itu 'AESA' (Active Electronically Scanned Array)?", "id": "Teknologi kunci untuk sistem MFRFS." },
  { "en": "Manfaat AESA untuk MFRFS?", "id": "Kemampuan mengubah fungsi dengan sangat cepat." },
  { "en": "Apa itu 'cognitive resource management'?", "id": "Manajemen sumber daya MFRFS berbasis AI." },
  { "en": "Fokus 'terrestrial EW'?", "id": "Operasi peperangan elektronika di domain darat." },
  { "en": "Contoh sistem 'terrestrial EW'?", "id": "Jammer konvoi, stasiun intersepsi darat." },
  { "en": "Tantangan mobilitas EW darat?", "id": "Medan, kebutuhan daya, ukuran antena." },
  { "en": "Apa itu 'manpack' EW system?", "id": "Sistem EW yang bisa dibawa ransel." },
  { "en": "Apa itu 'dismounted EW'?", "id": "EW yang dioperasikan tentara berjalan kaki." },
  { "en": "Misi EW perlindungan konvoi?", "id": "Menjamming pemicu radio IED." },
  { "en": "Misi EW untuk artileri?", "id": "Mengganggu komunikasi dan radar artileri musuh." },
  { "en": "Apa itu 'electronic screen'?", "id": "'Tabir' EW untuk melindungi pasukan." },
  { "en": "Sistem EW darat statis?", "id": "Stasiun pemantauan perbatasan jangka panjang." },
  { "en": "Integrasi EW darat dan udara?", "id": "Untuk gambaran situasi EM yang lengkap." },
  { "en": "Apa itu 'Information Exchange Requirement' (IER)?", "id": "Kebutuhan pertukaran informasi yang spesifik." },
  { "en": "Pentingnya IER dalam EW?", "id": "Memastikan interoperabilitas dan koordinasi." },
  { "en": "Apa itu 'USMTF'?", "id": "United States Message Text Format." },
  { "en": "Fungsi 'USMTF' dalam EW?", "id": "Format standar untuk pesan laporan EW." },
  { "en": "Apa itu 'threat library generation'?", "id": "Proses membuat dan memvalidasi database ancaman." },
  { "en": "Sumber data untuk 'threat library'?", "id": "Intelijen teknis (TECHELINT)." },
  { "en": "Apa itu 'validation and verification' (V&V)?", "id": "Proses memastikan data ancaman akurat." },
  { "en": "Tantangan distribusi 'threat library'?", "id": "Keamanan, kecepatan, dan kompatibilitas." },
  { "en": "Apa itu 'emitter parametric data'?", "id": "Data parameter spesifik dari pemancar." },
  { "en": "Apa itu 'tactical data link' (TDL)?", "id": "Jaringan untuk bertukar data taktis." },
  { "en": "Tantangan 'passive ranging'?", "id": "Menentukan jarak tanpa memancarkan sinyal." },
  { "en": "Metode 'single-platform passive ranging'?", "id": "Membutuhkan pergerakan platform (manuver)." },
  { "en": "Apa itu 'angle-only tracking'?", "id": "Pelacakan target hanya dengan data sudut." },
  { "en": "Apa itu 'emitter location by triangulation'?", "id": "Lokasi dari perpotongan garis arah." },
  { "en": "Syarat triangulasi?", "id": "Minimal dua stasiun ES terpisah." },
  { "en": "Metode 'multi-platform passive ranging'?", "id": "Menggunakan TDOA atau FDOA." },
  { "en": "Prinsip TDOA (Time Difference of Arrival)?", "id": "Mengukur beda waktu sinyal tiba." },
  { "en": "Apa itu 'geometric dilution of precision' (GDOP)?", "id": "Pengaruh geometri pada akurasi lokasi." },
  { "en": "GDOP (Geometric Dilution of Precision) yang baik?", "id": "Nilai rendah (stasiun tersebar luas)." },
  { "en": "Apa itu 'differential Doppler'?", "id": "Teknik penentuan lokasi pakai efek Doppler." },
  { "en": "Apa itu 'knowledge base' dalam CEW?", "id": "Database berisi pengetahuan dan aturan EW." },
  { "en": "Apa itu 'policy engine' dalam CEW?", "id": "Mesin yang terapkan aturan dan batasan." },
  { "en": "Contoh 'policy' dalam CEW?", "id": "Aturan ROE, batasan daya pancar." },
  { "en": "Apa itu 'reasoning engine' dalam CEW?", "id": "Membuat kesimpulan dari data yang ada." },
  { "en": "Apa itu 'online learning'?", "id": "Model AI yang belajar terus-menerus." },
  { "en": "Apa itu 'explainable AI' (XAI)?", "id": "AI yang bisa jelaskan keputusannya." },
  { "en": "Pentingnya XAI dalam EW?", "id": "Kepercayaan operator dan analisis pasca-misi." },
  { "en": "Apa itu 'reward function'?", "id": "Fungsi yang beri 'hadiah' pada AI." },
  { "en": "Apa itu 'state representation'?", "id": "Cara AI merepresentasikan lingkungan EW." },
  { "en": "Apa itu 'action space'?", "id": "Semua kemungkinan tindakan yang bisa diambil AI." },
  { "en": "Beda 'robust' dan 'resilient'?", "id": "Robust menahan, resilient pulih cepat." },
  { "en": "Apa itu 'anti-fragile system'?", "id": "Sistem yang lebih baik dari guncangan." },
  { "en": "Contoh 'anti-fragility' EW?", "id": "Sistem belajar dari upaya jamming musuh." },
  { "en": "Apa itu 'redundancy' dalam desain?", "id": "Memiliki komponen atau sistem cadangan." },
  { "en": "Apa itu 'diversity' dalam desain?", "id": "Menggunakan pendekatan atau komponen berbeda." },
  { "en": "Contoh 'diversity' EP?", "id": "Menggunakan beberapa teknik anti-jamming." },
  { "en": "Apa itu 'self-healing network'?", "id": "Jaringan yang bisa perbaiki dirinya sendiri." },
  { "en": "Bagaimana 'decentralization' meningkatkan resiliensi?", "id": "Tidak ada 'single point of failure'." },
  { "en": "Apa itu 'selective degradation'?", "id": "Memilih fungsi mana yang dikorbankan." },
  { "en": "Apa itu 'mission survivability'?", "id": "Kemampuan platform menyelesaikan misi dan selamat." },
  { "en": "Apa itu 'PSYOP broadcast'?", "id": "Siaran radio/TV untuk tujuan psikologis." },
  { "en": "Peran EA dalam 'PSYOP broadcast'?", "id": "Mengambil alih frekuensi siaran musuh." },
  { "en": "Peran EP dalam 'PSYOP broadcast'?", "id": "Melindungi siaran kawan dari jamming." },
  { "en": "Peran ES dalam 'PSYOP'?", "id": "Menemukan frekuensi dan audiens target." },
  { "en": "Apa itu 'voice morphing'?", "id": "Mengubah suara agar mirip orang lain." },
  { "en": "Aplikasi 'voice morphing' di PSYOP?", "id": "Membuat perintah palsu yang meyakinkan." },
  { "en": "Apa itu 'deepfake' video/audio?", "id": "Konten palsu yang dibuat oleh AI." },
  { "en": "Ancaman 'deepfake' dalam IW?", "id": "Disinformasi dan propaganda sangat kuat." },
  { "en": "Apa itu 'sentiment analysis'?", "id": "Analisis emosi dari teks atau suara." },
  { "en": "Aplikasi 'sentiment analysis' di PSYOP?", "id": "Mengukur efektivitas kampanye pesan." }


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
