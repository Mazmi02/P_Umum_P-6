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


  { "en": "Laut Ionia Adalah Bagian Dari Laut Apa Yang Lebih Besar?", "id": "Laut Mediterania." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1930?", "id": "Nathan Söderblom." },
  { "en": "Apa Nama Zat Kimia Pembawa Pesan Antar Sel Saraf?", "id": "Neurotransmitter." },
  { "en": "Apa Nama Periode Pembaharuan Gereja Katolik Di Eropa?", "id": "Reformasi Protestan." },
  { "en": "Apa Arti Istilah Musik 'Ritardando' Secara Bertahap?", "id": "Melambat." },
  { "en": "Siapakah Matematikawan Skotlandia Yang Mengembangkan Logaritma?", "id": "John Napier." },
  { "en": "Kuil Parthenon Di Athena Dibangun Untuk Menghormati Dewi Siapa?", "id": "Dewi Athena." },
  { "en": "Seni Pertunjukan Ludruk Berasal Dari Provinsi Mana?", "id": "Jawa Timur." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Sangurdi Di Telinga?", "id": "Stapes." },
  { "en": "Rasi Bintang Apa Dalam Zodiak Yang Melambangkan Pemanah?", "id": "Sagitarius." },
  { "en": "Laut Aegea Terletak Di Antara Dua Negara Apa?", "id": "Yunani Dan Turki." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1931?", "id": "Erik Axel Karlfeldt." },
  { "en": "Neurotransmitter Apa Yang Berperan Dalam Suasana Hati?", "id": "Serotonin." },
  { "en": "Apa Nama Periode 'Kelahiran Kembali' Seni Dan Ilmu?", "id": "Renaisans." },
  { "en": "Apa Arti Istilah Musik 'Vibrato'?", "id": "Getaran Nada." },
  { "en": "Siapa Matematikawan Yunani Kuno Yang Menulis 'Elements'?", "id": "Euclid." },
  { "en": "Piramida Agung Giza Dibangun Sebagai Makam Untuk Siapa?", "id": "Firaun Khufu." },
  { "en": "Seni Pertunjukan Ketoprak Berasal Dari Daerah Mana?", "id": "Jawa Tengah." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Landasan Di Telinga?", "id": "Inkus." },
  { "en": "Rasi Bintang Apa Dalam Zodiak Yang Melambangkan Kambing?", "id": "Kaprikornus." },
  { "en": "Laut Adriatik Memisahkan Semenanjung Italia Dari Semenanjung Apa?", "id": "Semenanjung Balkan." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1930?", "id": "Hans Fischer." },
  { "en": "Neurotransmitter Apa Yang Berperan Dalam Gerakan Otot?", "id": "Asetilkolin." },
  { "en": "Apa Nama Periode Sejarah Setelah Abad Pertengahan?", "id": "Zaman Modern Awal." },
  { "en": "Apa Arti Istilah Musik 'Glissando'?", "id": "Meluncur Antar Nada." },
  { "en": "Siapa Matematikawan Perancis Yang Terkenal Dengan Teori Grup?", "id": "Évariste Galois." },
  { "en": "Colosseum Di Roma Digunakan Sebagai Arena Untuk Apa?", "id": "Pertarungan Gladiator." },
  { "en": "Seni Pertunjukan Randai Berasal Dari Suku Apa?", "id": "Suku Minangkabau." },
  { "en": "Apa Nama Ilmiah Untuk Tulang Martil Di Telinga?", "id": "Maleus." },
  { "en": "Rasi Bintang Apa Dalam Zodiak Yang Melambangkan Pembawa Air?", "id": "Akuarius." },
  { "en": "Laut Tyrrhenian Dikelilingi Oleh Wilayah Negara Mana?", "id": "Italia." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1930?", "id": "C. V. Raman." },
  { "en": "Neurotransmitter Apa Yang Terkait Dengan Rasa Senang?", "id": "Dopamin." },
  { "en": "Apa Nama Periode Sejarah Yunani Kuno Sebelum Era Klasik?", "id": "Zaman Arkais." },
  { "en": "Apa Arti Istilah Musik 'Fermata'?", "id": "Menahan Nada Lebih Lama." },
  { "en": "Siapa Matematikawan Yang Mengembangkan Sistem Koordinat Kartesius?", "id": "René Descartes." },
  { "en": "Tembok Besar Tiongkok Dibangun Untuk Tujuan Apa?", "id": "Pertahanan Militer." },
  { "en": "Seni Pertunjukan Arja Berasal Dari Pulau Mana?", "id": "Bali." },
  { "en": "Apa Bagian Mata Yang Merupakan Jaringan Saraf Peka Cahaya?", "id": "Retina." },
  { "en": "Rasi Bintang Apa Dalam Zodiak Yang Melambangkan Ikan?", "id": "Pisces." },
  { "en": "Laut Liguria Berada Di Lepas Pantai Negara Mana?", "id": "Italia Dan Perancis." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1931?", "id": "Otto Heinrich Warburg." },
  { "en": "Neurotransmitter Apa Yang Berfungsi Sebagai 'Hormon Stres'?", "id": "Norepinefrin." },
  { "en": "Apa Nama Periode Sejarah Romawi Yang Damai?", "id": "Pax Romana." },
  { "en": "Apa Arti Istilah 'A Tempo' Dalam Musik?", "id": "Kembali Ke Tempo Awal." },
  { "en": "Siapa Matematikawan Swiss Yang Terkenal Di Abad Ke-18?", "id": "Leonhard Euler." },
  { "en": "Machu Picchu Adalah Situs Peninggalan Peradaban Apa?", "id": "Peradaban Inka." },
  { "en": "Seni Pertunjukan Mamanda Berasal Dari Provinsi Mana?", "id": "Kalimantan Selatan." },
  { "en": "Apa Bagian Mata Yang Bening Di Paling Depan?", "id": "Kornea." },
  { "en": "Rasi Bintang Apa Dalam Zodiak Yang Melambangkan Domba?", "id": "Aries." },
  { "en": "Laut Alboran Adalah Bagian Paling Barat Dari Laut Apa?", "id": "Laut Mediterania." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1932?", "id": "John Galsworthy." },
  { "en": "Neurotransmitter Apa Yang Berfungsi Sebagai Inhibitor Utama?", "id": "GABA." },
  { "en": "Apa Nama Periode 'Pencerahan' Di Eropa?", "id": "Abad Pencerahan." },
  { "en": "Apa Istilah Untuk Komposisi Musik Tiga Bagian?", "id": "Bentuk Sonata." },
  { "en": "Siapa Matematikawan Jerman Yang Terkenal Dengan 'Lengkungan Lonceng'?", "id": "Carl Friedrich Gauss." },
  { "en": "Angkor Wat Adalah Kompleks Candi Hindu-Buddha Di Negara Mana?", "id": "Kamboja." },
  { "en": "Seni Pertunjukan Mendu Berasal Dari Kepulauan Mana?", "id": "Kepulauan Riau." },
  { "en": "Apa Bagian Mata Yang Memberikan Warna?", "id": "Iris." },
  { "en": "Rasi Bintang Apa Dalam Zodiak Yang Melambangkan Banteng?", "id": "Taurus." },
  { "en": "Laut Levantine Berada Di Bagian Timur Laut Apa?", "id": "Laut Mediterania." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1932?", "id": "Irving Langmuir." },
  { "en": "Neurotransmitter Apa Yang Terkait Dengan Gairah Dan Kewaspadaan?", "id": "Adrenalin (Epinefrin)." },
  { "en": "Apa Nama Periode Ekspansi Kolonialisme Eropa?", "id": "Zaman Penjelajahan." },
  { "en": "Apa Istilah Untuk Sebuah Bagian Tunggal Dalam Simfoni?", "id": "Gerakan (Movement)." },
  { "en": "Siapa Matematikawan Inggris Yang Merumuskan Teorema Terakhirnya?", "id": "Andrew Wiles (Membuktikan)." },
  { "en": "Hagia Sophia Adalah Bangunan Bersejarah Di Kota Mana?", "id": "Istanbul." },
  { "en": "Seni Pertunjukan Mak Yong Berasal Dari Daerah Mana?", "id": "Riau Dan Malaysia." },
  { "en": "Apa Bagian Mata Yang Merupakan Otot Pengatur Lensa?", "id": "Otot Siliaris." },
  { "en": "Rasi Bintang Apa Dalam Zodiak Yang Melambangkan Si Kembar?", "id": "Gemini." },
  { "en": "Laut Balearik Adalah Perairan Di Sekitar Kepulauan Mana?", "id": "Kepulauan Balearik, Spanyol." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1931?", "id": "Addams Dan Butler." },
  { "en": "Neurotransmitter Apa Yang Paling Umum Di Sistem Saraf?", "id": "Glutamat." },
  { "en": "Apa Nama Periode Perang Antara Roma Dan Kartago?", "id": "Perang Punisia." },
  { "en": "Apa Istilah Untuk Tema Musik Berulang Yang Mewakili Karakter?", "id": "Leitmotif." },
  { "en": "Siapa Matematikawan Yang Dikenal Sebagai 'Bapak Komputer'?", "id": "Charles Babbage." },
  { "en": "Taj Mahal Di India Dibangun Sebagai Monumen Apa?", "id": "Makam." },
  { "en": "Seni Pertunjukan Topeng Cirebon Berasal Dari Kota Mana?", "id": "Cirebon." },
  { "en": "Apa Bagian Mata Yang Menghubungkan Lensa Dengan Otot?", "id": "Ligamen Suspensor." },
  { "en": "Rasi Bintang Apa Dalam Zodiak Yang Melambangkan Kepiting?", "id": "Cancer." },
  { "en": "Laut Sargasso Terletak Di Dalam Samudra Mana?", "id": "Samudra Atlantik." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1933?", "id": "Schrödinger Dan Dirac." },
  { "en": "Glutamat Adalah Neurotransmitter Utama Untuk Fungsi Apa?", "id": "Eksitasi (Pemicu)." },
  { "en": "Apa Nama Periode Revolusi Pertanian?", "id": "Revolusi Neolitik." },
  { "en": "Apa Istilah Untuk Pertunjukan Musik Oleh Satu Orang?", "id": "Resital Solo." },
  { "en": "Siapa Matematikawan Wanita Pertama Yang Terkenal?", "id": "Hypatia dari Alexandria." },
  { "en": "Patung Liberty Adalah Hadiah Dari Negara Mana?", "id": "Perancis." },
  { "en": "Seni Pertunjukan Ubrug Berasal Dari Provinsi Mana?", "id": "Banten." },
  { "en": "Apa Bagian Mata Yang Membawa Sinyal Visual Ke Otak?", "id": "Saraf Optik." },
  { "en": "Rasi Bintang Apa Dalam Zodiak Yang Melambangkan Singa?", "id": "Leo." },
  { "en": "Gunung Rushmore Terletak Di Negara Bagian Amerika Serikat Mana?", "id": "South Dakota." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1950?", "id": "Ralph Bunche." },
  { "en": "Apa Nama Katup Antara Lambung Dan Usus Halus?", "id": "Katup Pilorus." },
  { "en": "Di Negara Manakah Peristiwa 'Minggu Berdarah' (1905) Terjadi?", "id": "Rusia." },
  { "en": "Galeri Nasional (National Gallery) Terletak Di Kota Mana?", "id": "London." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Eksperimen Konformitas Asch'?", "id": "Solomon Asch." },
  { "en": "Gurun Thar, Atau Gurun Besar India, Adalah Jenis Gurun Apa?", "id": "Gurun Subtropis." },
  { "en": "Apa Nama Festival Perahu Naga Yang Dirayakan Di Indonesia?", "id": "Peh Cun." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Lingkungan Kering?", "id": "Xerophile." },
  { "en": "Apa Nama Formasi Batuan Sedimen Yang Miring?", "id": "Lapisan Miring." },
  { "en": "Gunung Logan Adalah Gunung Tertinggi Di Negara Mana?", "id": "Kanada." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1916?", "id": "Verner von Heidenstam." },
  { "en": "Apa Nama Katup Antara Kerongkongan Dan Lambung?", "id": "Katup Kardia." },
  { "en": "Di Kota Manakah Proklamasi Kemerdekaan Irlandia (1916) Dibacakan?", "id": "Dublin." },
  { "en": "Museum Seni Modern San Francisco Terletak Di Kota Mana?", "id": "San Francisco." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Terapi Kognitif'?", "id": "Aaron Beck." },
  { "en": "Gurun Kalahari Di Afrika Adalah Jenis Gurun Apa?", "id": "Gurun Semi-Kering." },
  { "en": "Apa Nama Festival Laut Tahunan Di Jepara?", "id": "Lomban Kupatan." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Lingkungan Basa?", "id": "Alkalifil." },
  { "en": "Apa Nama Perubahan Orientasi Lapisan Batuan?", "id": "Kemiringan (Dip)." },
  { "en": "Gunung Vinson Adalah Puncak Tertinggi Di Benua Mana?", "id": "Antartika." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1915?", "id": "Bragg Dan Bragg." },
  { "en": "Apa Nama Cairan Pencernaan Yang Diproduksi Oleh Hati?", "id": "Empedu." },
  { "en": "Di Negara Manakah Revolusi Tenang (Quiet Revolution) Terjadi?", "id": "Kanada." },
  { "en": "Museum Istana Nasional (National Palace Museum) Di Negara Mana?", "id": "Taiwan." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Kebutuhan'?", "id": "Abraham Maslow." },
  { "en": "Gurun Atacama Di Chili Adalah Jenis Gurun Apa?", "id": "Gurun Pesisir." },
  { "en": "Apa Nama Festival Budaya Tahunan Di Lembah Baliem?", "id": "Festival Lembah Baliem." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Lingkungan Asam?", "id": "Asidofil." },
  { "en": "Apa Nama Retakan Pada Batuan Tanpa Pergeseran?", "id": "Kekar (Joint)." },
  { "en": "Gunung Elbrus Adalah Puncak Tertinggi Di Benua Mana?", "id": "Eropa." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1915?", "id": "Richard Willstätter." },
  { "en": "Apa Nama Enzim Yang Memecah Laktosa (Gula Susu)?", "id": "Laktase." },
  { "en": "Di Negara Manakah Revolusi Melati (Jasmine Revolution) Dimulai?", "id": "Tunisia." },
  { "en": "Museum Reina Sofía Terletak Di Kota Mana?", "id": "Madrid." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Pembelajaran Sosial'?", "id": "Albert Bandura." },
  { "en": "Gurun Mojave Di Amerika Serikat Adalah Jenis Gurun Apa?", "id": "Gurun Hujan Bayangan." },
  { "en": "Apa Nama Perayaan Untuk Menghormati Dewi Padi?", "id": "Seren Taun." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Suhu Sangat Dingin?", "id": "Psikrofil." },
  { "en": "Apa Nama Proses Pengendapan Sedimen Oleh Air?", "id": "Sedimentasi Akuatik." },
  { "en": "Gunung Aconcagua Adalah Puncak Tertinggi Di Benua Mana?", "id": "Amerika Selatan." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1914?", "id": "Robert Bárány." },
  { "en": "Apa Nama Enzim Yang Memecah Sukrosa (Gula Pasir)?", "id": "Sukrase." },
  { "en": "Di Negara Manakah 'Revolusi Oktober' Terjadi?", "id": "Rusia." },
  { "en": "Museum Seni Kimbell Terletak Di Kota Mana?", "id": "Fort Worth, Texas." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Disonansi Kognitif'?", "id": "Leon Festinger." },
  { "en": "Gurun Patagonia Adalah Jenis Gurun Apa?", "id": "Gurun Dingin." },
  { "en": "Apa Nama Festival Perang Tombak Khas Sumba?", "id": "Pasola." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Suhu Sangat Panas?", "id": "Termofil." },
  { "en": "Apa Nama Proses Pengendapan Sedimen Oleh Angin?", "id": "Sedimentasi Eolian." },
  { "en": "Gunung Denali Adalah Puncak Tertinggi Di Benua Mana?", "id": "Amerika Utara." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1915?", "id": "Romain Rolland." },
  { "en": "Apa Nama Enzim Yang Memecah Lemak Di Usus Halus?", "id": "Lipase." },
  { "en": "Di Negara Manakah 'Revolusi Kebudayaan' Diluncurkan?", "id": "Tiongkok." },
  { "en": "Museum Getty Center Terletak Di Kota Mana?", "id": "Los Angeles." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Eksperimen Penjara Stanford'?", "id": "Philip Zimbardo." },
  { "en": "Gurun Namib Adalah Gurun Tertua Di Benua Mana?", "id": "Afrika." },
  { "en": "Apa Nama Festival Perahu Sandeq Di Sulawesi Barat?", "id": "Sandeq Race." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Bawah Tekanan Tinggi?", "id": "Barofil." },
  { "en": "Apa Nama Proses Pemindahan Material Oleh Gravitasi?", "id": "Gerakan Massa." },
  { "en": "Gunung Kilimanjaro Adalah Gunung Berapi Tertinggi Di Benua Mana?", "id": "Afrika." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1913?", "id": "Henri La Fontaine." },
  { "en": "Apa Nama Enzim Yang Memecah Protein Di Usus Halus?", "id": "Tripsin." },
  { "en": "Di Negara Manakah 'Revolusi EDSA' Terjadi?", "id": "Filipina." },
  { "en": "Museum Seni Sao Paulo (MASP) Terletak Di Kota Mana?", "id": "São Paulo." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Atribusi'?", "id": "Fritz Heider." },
  { "en": "Gurun Simpson Terletak Di Benua Mana?", "id": "Australia." },
  { "en": "Apa Nama Festival Danau Toba Di Sumatera Utara?", "id": "Festival Danau Toba." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Dalam Batuan?", "id": "Endolit." },
  { "en": "Apa Nama Aliran Lava Yang Mendingin Dengan Cepat?", "id": "Lava Aa." },
  { "en": "Gunung Fuji Adalah Gunung Tertinggi Di Negara Mana?", "id": "Jepang." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1913?", "id": "Heike Kamerlingh Onnes." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Lambung?", "id": "Gastrin." },
  { "en": "Di Negara Manakah 'Revolusi Bunga Anyelir' Terjadi?", "id": "Portugal." },
  { "en": "Museum Soumaya Terletak Di Kota Mana?", "id": "Kota Meksiko." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Kondisioning Klasik'?", "id": "Ivan Pavlov." },
  { "en": "Gurun Karakum Terletak Di Benua Mana?", "id": "Asia." },
  { "en": "Apa Nama Festival Budaya Masyarakat Dieng?", "id": "Dieng Culture Festival." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Atas Es?", "id": "Kriofil." },
  { "en": "Apa Nama Aliran Lava Yang Mendingin Dengan Lambat?", "id": "Lava Pahoehoe." },
  { "en": "Gunung Matterhorn Terletak Di Perbatasan Negara Mana?", "id": "Swiss Dan Italia." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1913?", "id": "Alfred Werner." },
  { "en": "Apa Nama Zat Yang Diproduksi Pankreas Untuk Menetralkan Asam?", "id": "Bikarbonat." },
  { "en": "Di Negara Manakah 'Revolusi Beludru' Terjadi?", "id": "Cekoslowakia." },
  { "en": "Museum Nasional Antropologi Di Meksiko Terletak Di Mana?", "id": "Kota Meksiko." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Kondisioning Operan'?", "id": "B.F. Skinner." },
  { "en": "Gurun Kyzylkum Terletak Di Benua Mana?", "id": "Asia." },
  { "en": "Apa Nama Upacara Adat Untuk Memperingati Kematian?", "id": "Tahlilan." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Gua?", "id": "Troglobit." },
  { "en": "Apa Nama Depresi Besar Yang Disebabkan Oleh Runtuhnya Gunung Berapi?", "id": "Kaldera." },
  { "en": "Kota Mana Di Amerika Serikat Yang Dijuluki 'Kota Luar Angkasa'?", "id": "Houston." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1949?", "id": "Lord Boyd Orr." },
  { "en": "Apa Nama Selaput Tipis Yang Melapisi Rongga Perut?", "id": "Peritoneum." },
  { "en": "Di Kota Manakah Perjanjian Damai Perang Vietnam Ditandatangani?", "id": "Paris." },
  { "en": "Museum Guggenheim Terletak Di Kota Mana?", "id": "New York." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Pembelajaran Latent'?", "id": "Edward Tolman." },
  { "en": "Danau Eyre, Danau Terbesar Di Australia, Terletak Di Negara Bagian Mana?", "id": "Australia Selatan." },
  { "en": "Apa Nama Festival Layang-Layang Tahunan Di Bali?", "id": "Bali Kite Festival." },
  { "en": "Apa Istilah Untuk Proses 'Penuaan' Biologis Sel?", "id": "Senescence." },
  { "en": "Apa Nama Formasi Lipatan Batuan Yang Cembung Ke Atas?", "id": "Antiklin." },
  { "en": "Kota Mana Di Italia Yang Dijuluki 'Kota Di Atas Air'?", "id": "Venesia." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1917?", "id": "Gjellerup Dan Pontoppidan." },
  { "en": "Apa Nama Jaringan Ikat Yang Menggantung Usus Di Dinding Perut?", "id": "Mesenterium." },
  { "en": "Di Negara Mana Pertempuran Balaclava Terjadi?", "id": "Ukraina (Krimea)." },
  { "en": "Museum Seni Modern San Francisco Terletak Di Kota Mana?", "id": "San Francisco." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Disonansi Kognitif'?", "id": "Leon Festinger." },
  { "en": "Danau Winnipeg Terletak Di Provinsi Mana Di Kanada?", "id": "Manitoba." },
  { "en": "Apa Nama Festival Seni Dan Budaya Tahunan Di Yogyakarta?", "id": "Festival Kesenian Yogyakarta." },
  { "en": "Apa Istilah Untuk Kematian Sel Akibat Cedera?", "id": "Nekrosis." },
  { "en": "Apa Nama Formasi Lipatan Batuan Yang Cekung Ke Bawah?", "id": "Sinklin." },
  { "en": "Kota Mana Di Australia Yang Dijuluki 'Kota Pelabuhan'?", "id": "Sydney." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1914?", "id": "Max von Laue." },
  { "en": "Apa Nama Lipatan Jaringan Di Dalam Usus Halus?", "id": "Vili." },
  { "en": "Di Negara Mana Perang Candu Terjadi?", "id": "Tiongkok." },
  { "en": "Museum Istana Nasional (National Palace Museum) Di Negara Mana?", "id": "Taiwan." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Eksperimen Bobo Doll'?", "id": "Albert Bandura." },
  { "en": "Danau Great Slave Terletak Di Wilayah Mana Di Kanada?", "id": "Northwest Territories." },
  { "en": "Apa Nama Festival Budaya Bahari Di Sulawesi Selatan?", "id": "Festival Pinisi." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Dasar Perairan?", "id": "Bentos." },
  { "en": "Apa Nama Pergerakan Vertikal Kerak Bumi?", "id": "Gerak Epirogenetik." },
  { "en": "Kota Mana Di Selandia Baru Yang Dijuluki 'Kota Layar'?", "id": "Auckland." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1915?", "id": "Richard Willstätter." },
  { "en": "Apa Nama Katup Antara Usus Halus Dan Usus Besar?", "id": "Katup Ileosekal." },
  { "en": "Di Negara Mana Perang Saudara Spanyol Terjadi?", "id": "Spanyol." },
  { "en": "Museum Reina Sofía Terletak Di Kota Mana?", "id": "Madrid." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Little Albert Experiment'?", "id": "John B. Watson." },
  { "en": "Danau Chad Berada Di Perbatasan Empat Negara Apa Saja?", "id": "Chad, Kamerun, Niger, Nigeria." },
  { "en": "Apa Nama Festival Budaya Melayu Di Provinsi Riau?", "id": "Festival Siak Bermadah." },
  { "en": "Apa Istilah Untuk Organisme Yang Melayang Di Perairan?", "id": "Plankton." },
  { "en": "Apa Nama Proses Pengangkatan Daratan?", "id": "Uplift." },
  { "en": "Kota Mana Di Skotlandia Yang Dijuluki 'Athena Dari Utara'?", "id": "Edinburgh." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1913?", "id": "Charles Richet." },
  { "en": "Apa Nama Bagian Pertama Dari Usus Halus?", "id": "Duodenum." },
  { "en": "Di Negara Mana Perang Seratus Tahun Terjadi?", "id": "Perancis." },
  { "en": "Museum Seni Kimbell Terletak Di Kota Mana?", "id": "Fort Worth, Texas." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Terapi Perilaku Kognitif'?", "id": "Aaron Beck." },
  { "en": "Danau Van, Danau Terbesar Di Turki, Adalah Danau Jenis Apa?", "id": "Danau Soda/Alkali." },
  { "en": "Apa Nama Festival Sastra Dan Seni Di Ubud?", "id": "Ubud Writers & Readers Festival." },
  { "en": "Apa Istilah Untuk Organisme Yang Berenang Aktif Di Perairan?", "id": "Nekton." },
  { "en": "Apa Nama Proses Penurunan Daratan?", "id": "Subsidence." },
  { "en": "Kota Mana Di Brasil Yang Dijuluki 'Kota Luar Biasa'?", "id": "Rio de Janeiro." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1911?", "id": "Maurice Maeterlinck." },
  { "en": "Apa Nama Bagian Kedua Dari Usus Halus?", "id": "Jejunum." },
  { "en": "Di Negara Mana Perang Musim Dingin (Winter War) Terjadi?", "id": "Finlandia." },
  { "en": "Museum Getty Center Terletak Di Kota Mana?", "id": "Los Angeles." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Teori Medan'?", "id": "Kurt Lewin." },
  { "en": "Danau Aral Terletak Di Antara Dua Negara Asia Tengah Apa?", "id": "Kazakhstan Dan Uzbekistan." },
  { "en": "Apa Nama Festival Budaya Suku Tengger Di Bromo?", "id": "Yadnya Kasada." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Darat?", "id": "Terestrial." },
  { "en": "Apa Nama Proses Pengendapan Material Oleh Air?", "id": "Sedimentasi." },
  { "en": "Kota Mana Di India Yang Dijuluki 'Kota Impian'?", "id": "Mumbai." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1911?", "id": "Asser Dan Fried." },
  { "en": "Apa Nama Bagian Ketiga Dari Usus Halus?", "id": "Ileum." },
  { "en": "Di Negara Mana Perang Yom Kippur Terjadi?", "id": "Timur Tengah." },
  { "en": "Museum Seni Sao Paulo (MASP) Terletak Di Kota Mana?", "id": "São Paulo." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Uji Asosiasi Implisit'?", "id": "Anthony Greenwald." },
  { "en": "Danau Balkhash Terletak Di Negara Mana?", "id": "Kazakhstan." },
  { "en": "Apa Nama Festival Erau Di Kutai Kartanegara?", "id": "Erau Adat Kutai." },
  { "en": "Apa Istilah Untuk Organisme Yang Hidup Di Air?", "id": "Akuatik." },
  { "en": "Apa Nama Proses Penghancuran Batuan Secara Fisik?", "id": "Pelapukan Mekanis." },
  { "en": "Kota Mana Di Ceko Yang Dijuluki 'Jantung Eropa'?", "id": "Praha." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1912?", "id": "Gustaf Dalén." },
  { "en": "Apa Enzim Di Lambung Yang Menggumpalkan Susu?", "id": "Renin." },
  { "en": "Di Negara Mana Perang Mawar (Wars of the Roses) Terjadi?", "id": "Inggris." },
  { "en": "Museum Soumaya Terletak Di Kota Mana?", "id": "Kota Meksiko." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Efek Lucifer'?", "id": "Philip Zimbardo." },
  { "en": "Danau Issyk-Kul Terletak Di Pegunungan Mana?", "id": "Pegunungan Tian Shan." },
  { "en": "Apa Nama Festival Budaya Khas Masyarakat Betawi?", "id": "Lebaran Betawi." },
  { "en": "Apa Istilah Untuk Hewan Yang Tidur Di Musim Dingin?", "id": "Hibernasi." },
  { "en": "Apa Nama Proses Penghancuran Batuan Secara Kimia?", "id": "Pelapukan Kimiawi." },
  { "en": "Kota Mana Di Irlandia Utara Yang Dijuluki 'Kota Titanic'?", "id": "Belfast." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1912?", "id": "Grignard Dan Sabatier." },
  { "en": "Apa Fungsi Utama Dari Cairan Empedu?", "id": "Mengemulsi Lemak." },
  { "en": "Di Negara Mana Perang Saudara Inggris Terjadi?", "id": "Inggris." },
  { "en": "Museum Nasional Antropologi Di Meksiko Terletak Di Mana?", "id": "Kota Meksiko." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Kondisioning Klasik'?", "id": "Ivan Pavlov." },
  { "en": "Danau Vostok, Danau Subglasial, Terletak Di Benua Mana?", "id": "Antartika." },
  { "en": "Apa Nama Festival Budaya Tahunan Di Lembah Baliem?", "id": "Festival Lembah Baliem." },
  { "en": "Apa Istilah Untuk Hewan Yang Aktif Di Malam Hari?", "id": "Nokturnal." },
  { "en": "Apa Nama Proses Pengendapan Material Oleh Angin?", "id": "Deposisi Eolian." },
  { "en": "Selat Sunda Menghubungkan Laut Jawa Dengan Samudra Apa?", "id": "Samudra Hindia." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1948?", "id": "Tidak Diberikan." },
  { "en": "Apa Nama Sel Pendukung Di Dalam Sistem Saraf?", "id": "Sel Glial." },
  { "en": "Apa Nama Serangan Mendadak Jepang Terhadap AS Di PD II?", "id": "Serangan Pearl Harbor." },
  { "en": "Galeri Uffizi, Museum Seni Terkenal, Terletak Di Kota Mana?", "id": "Florence." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Kecerdasan Emosional'?", "id": "Daniel Goleman." },
  { "en": "Semenanjung Jutland Sebagian Besar Membentuk Negara Apa?", "id": "Denmark." },
  { "en": "Apa Nama Perayaan Tahun Baru Islam Di Masyarakat Jawa?", "id": "Grebeg Suro." },
  { "en": "Apa Istilah Ilmiah Untuk Proses 'Ganti Kulit' Pada Hewan?", "id": "Ecdysis (Moulting)." },
  { "en": "Hukum Ohm Adalah Prinsip Dasar Dalam Bidang Ilmu Apa?", "id": "Kelistrikan." },
  { "en": "Selat Karimata Menghubungkan Laut Tiongkok Selatan Dengan Laut Apa?", "id": "Laut Jawa." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1928?", "id": "Sigrid Undset." },
  { "en": "Neurotransmitter Apa Yang Berperan Dalam Respon 'Lawan Atau Lari'?", "id": "Adrenalin." },
  { "en": "Apa Nama Kampanye Pengeboman Sekutu Terhadap Jerman Di PD II?", "id": "The Blitz." },
  { "en": "Museum Seni Rupa Metropolitan Terletak Di Kota Mana?", "id": "New York." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Kelekatan'?", "id": "John Bowlby." },
  { "en": "Semenanjung Sinai Adalah Bagian Dari Negara Mana?", "id": "Mesir." },
  { "en": "Apa Nama Perayaan Untuk Menghormati Dewi Sri?", "id": "Wiwitan." },
  { "en": "Apa Istilah Untuk Hewan Yang Memakan Kotoran?", "id": "Koprofag." },
  { "en": "Hukum Faraday Berlaku Dalam Bidang Ilmu Apa?", "id": "Elektromagnetisme." },
  { "en": "Selat Makassar Terletak Di Antara Pulau Apa?", "id": "Kalimantan Dan Sulawesi." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1919?", "id": "Johannes Stark." },
  { "en": "Apa Nama Bagian Dari Neuron Yang Menghasilkan Mielin?", "id": "Sel Schwann." },
  { "en": "Apa Nama Operasi Pendaratan Sekutu Di Normandia?", "id": "Operasi Overlord (D-Day)." },
  { "en": "Museum Seni Modern (MoMA) Terletak Di Kota Mana?", "id": "New York." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Uji Marshmallow'?", "id": "Walter Mischel." },
  { "en": "Semenanjung Korea Memisahkan Laut Kuning Dari Laut Apa?", "id": "Laut Jepang." },
  { "en": "Apa Nama Upacara Adat Pemakaman Di Bali?", "id": "Ngaben." },
  { "en": "Apa Istilah Untuk Hewan Yang Hidup Di Lingkungan Kering?", "id": "Xerocole." },
  { "en": "Hukum Kepler Berlaku Dalam Bidang Ilmu Apa?", "id": "Astronomi." },
  { "en": "Selat Lombok Memisahkan Pulau Bali Dengan Pulau Apa?", "id": "Lombok." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1919?", "id": "Tidak Ada." },
  { "en": "Apa Nama Cairan Yang Mengelilingi Otak Dan Sumsum Tulang Belakang?", "id": "Cairan Serebrospinal." },
  { "en": "Apa Nama Pertempuran Laut Terbesar Dalam Sejarah?", "id": "Pertempuran Teluk Leyte." },
  { "en": "Museum Prado Adalah Museum Seni Utama Di Kota Mana?", "id": "Madrid." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Belajar Kognitif Sosial'?", "id": "Albert Bandura." },
  { "en": "Semenanjung Florida Adalah Bagian Dari Negara Mana?", "id": "Amerika Serikat." },
  { "en": "Apa Nama Festival Tari Internasional Di Solo?", "id": "Solo International Performing Arts." },
  { "en": "Apa Istilah Untuk Hewan Yang Memakan Semut Dan Rayap?", "id": "Myrmecophage." },
  { "en": "Hukum Coulomb Berlaku Dalam Bidang Ilmu Apa?", "id": "Elektrostatika." },
  { "en": "Selat Bali Memisahkan Pulau Jawa Dengan Pulau Apa?", "id": "Bali." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1920?", "id": "August Krogh." },
  { "en": "Apa Bagian Sistem Saraf Yang Mengontrol Gerakan Sukarela?", "id": "Sistem Saraf Somatik." },
  { "en": "Apa Nama Proyek Rahasia AS Untuk Mengembangkan Bom Atom?", "id": "Proyek Manhattan." },
  { "en": "Rijksmuseum Adalah Museum Nasional Di Kota Mana?", "id": "Amsterdam." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Motivasi Berprestasi'?", "id": "David McClelland." },
  { "en": "Semenanjung Kamchatka Terletak Di Negara Mana?", "id": "Rusia." },
  { "en": "Apa Nama Festival Danau Toba Di Sumatera Utara?", "id": "Festival Danau Toba." },
  { "en": "Apa Istilah Untuk Hewan Yang Hidup Di Dalam Tanah?", "id": "Fossorial." },
  { "en": "Hukum Archimedes Berlaku Dalam Bidang Ilmu Apa?", "id": "Mekanika Fluida." },
  { "en": "Selat Gibraltar Menghubungkan Samudra Atlantik Dengan Laut Apa?", "id": "Laut Mediterania." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1920?", "id": "Knut Hamsun." },
  { "en": "Apa Bagian Sistem Saraf Yang Mengontrol Fungsi Tak Sadar?", "id": "Sistem Saraf Otonom." },
  { "en": "Apa Nama Konferensi Yang Membagi Afrika Di Antara Kekuatan Eropa?", "id": "Konferensi Berlin." },
  { "en": "Museum Hermitage Terletak Di Kota Mana?", "id": "St. Petersburg." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Dua Faktor Emosi'?", "id": "Schachter Dan Singer." },
  { "en": "Semenanjung Balkan Terletak Di Benua Mana?", "id": "Eropa." },
  { "en": "Apa Nama Festival Budaya Masyarakat Dieng?", "id": "Dieng Culture Festival." },
  { "en": "Apa Istilah Untuk Hewan Yang Berdarah Panas?", "id": "Endoterm." },
  { "en": "Hukum Pascal Berlaku Dalam Bidang Ilmu Apa?", "id": "Hidrostatika." },
  { "en": "Selat Hormuz Menghubungkan Teluk Persia Dengan Teluk Apa?", "id": "Teluk Oman." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1920?", "id": "Léon Bourgeois." },
  { "en": "Sistem Saraf Otonom Dibagi Menjadi Dua Bagian Apa?", "id": "Simpatik Dan Parasimpatik." },
  { "en": "Apa Nama Pengadilan Militer Pasca-PD II Di Jerman?", "id": "Pengadilan Nuremberg." },
  { "en": "Museum Vatikan Terletak Di Negara Mana?", "id": "Kota Vatikan." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Teori Penentuan Nasib Sendiri'?", "id": "Deci Dan Ryan." },
  { "en": "Semenanjung Anatolia Sebagian Besar Membentuk Negara Apa?", "id": "Turki." },
  { "en": "Apa Nama Festival Seni Dan Budaya Di Jakarta?", "id": "Jakarta Fair (Pekan Raya Jakarta)." },
  { "en": "Apa Istilah Untuk Hewan Yang Berdarah Dingin?", "id": "Ektoterm." },
  { "en": "Hukum Boyle Berlaku Dalam Bidang Ilmu Apa?", "id": "Termodinamika." },
  { "en": "Selat Bering Memisahkan Benua Asia Dengan Benua Apa?", "id": "Amerika Utara." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1921?", "id": "Albert Einstein." },
  { "en": "Sistem Saraf Simpatik Bertanggung Jawab Atas Respon Apa?", "id": "Respon 'Lawan Atau Lari'." },
  { "en": "Apa Nama Rencana Bantuan Ekonomi AS Untuk Eropa Pasca-PD II?", "id": "Rencana Marshall." },
  { "en": "Museum Acropolis Terletak Di Kota Mana?", "id": "Athena." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Keadilan'?", "id": "John Stacey Adams." },
  { "en": "Semenanjung Skandinavia Terdiri Dari Negara Apa Saja?", "id": "Norwegia Dan Swedia." },
  { "en": "Apa Nama Festival Budaya Tahunan Di Lembah Baliem?", "id": "Festival Lembah Baliem." },
  { "en": "Apa Istilah Untuk Hewan Yang Aktif Saat Fajar Dan Senja?", "id": "Krepuskular." },
  { "en": "Hukum Charles Berlaku Dalam Bidang Ilmu Apa?", "id": "Termodinamika." },
  { "en": "Selat Bosporus Memisahkan Sisi Eropa Dan Asia Kota Mana?", "id": "Istanbul." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1921?", "id": "Frederick Soddy." },
  { "en": "Sistem Saraf Parasimpatik Bertanggung Jawab Atas Respon Apa?", "id": "Respon 'Istirahat Dan Cerna'." },
  { "en": "Apa Nama Blokade Uni Soviet Terhadap Berlin Barat?", "id": "Blokade Berlin." },
  { "en": "Museum Ghibli Terletak Di Kota Mana?", "id": "Tokyo." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Harapan'?", "id": "Victor Vroom." },
  { "en": "Semenanjung Iberia Terdiri Dari Negara Apa Saja?", "id": "Spanyol Dan Portugal." },
  { "en": "Apa Nama Festival Erau Di Kutai Kartanegara?", "id": "Erau Adat Kutai." },
  { "en": "Apa Istilah Untuk Kemampuan Regenerasi Bagian Tubuh?", "id": "Regenerasi." },
  { "en": "Hukum Gay-Lussac Berlaku Dalam Bidang Ilmu Apa?", "id": "Termodinamika." },
  { "en": "Jembatan Rialto Adalah Jembatan Terkenal Di Kota Mana?", "id": "Venesia." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1922?", "id": "Hill Dan Meyerhof." },
  { "en": "Apa Nama Sel Saraf Yang Mengirimkan Sinyal Ke Otot?", "id": "Neuron Motorik." },
  { "en": "Di Kota Manakah Gerakan Seni 'Bauhaus' Didirikan?", "id": "Weimar, Jerman." },
  { "en": "Apa Nama Tanda Musik Yang Mengindikasikan Keheningan?", "id": "Tanda Istirahat." },
  { "en": "Apa Nama Batu Permata Yang Merupakan Bentuk Dari Karbon?", "id": "Intan." },
  { "en": "Apa Nama Upacara Adat Untuk Menyambut Masa Panen?", "id": "Sedekah Bumi." },
  { "en": "Apa Istilah Untuk Bentuk Tubuh Simetris Radial?", "id": "Simetri Radial." },
  { "en": "Apa Istilah Untuk Studi Ilmiah Tentang Cuaca?", "id": "Meteorologi." },
  { "en": "Siapa Penemu Penisilin Secara Tidak Sengaja?", "id": "Alexander Fleming." },
  { "en": "Jembatan Charles Di Praha Melintasi Sungai Apa?", "id": "Sungai Vltava." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1923?", "id": "William Butler Yeats." },
  { "en": "Apa Nama Sel Saraf Yang Menerima Informasi Sensorik?", "id": "Neuron Sensorik." },
  { "en": "Di Negara Mana 'Revolusi Kebudayaan' Diluncurkan?", "id": "Tiongkok." },
  { "en": "Apa Nama Tanda Musik Untuk Menghubungkan Dua Nada?", "id": "Lengkung (Slur)." },
  { "en": "Apa Nama Batu Permata Hijau Dari Mineral Beril?", "id": "Zamrud." },
  { "en": "Apa Nama Upacara Adat Untuk Menolak Bencana?", "id": "Tolak Bala." },
  { "en": "Apa Istilah Untuk Bentuk Tubuh Simetris Bilateral?", "id": "Simetri Bilateral." },
  { "en": "Apa Istilah Untuk Studi Ilmiah Tentang Gempa Bumi?", "id": "Seismologi." },
  { "en": "Siapa Ilmuwan Yang Mengembangkan Teori Relativitas?", "id": "Albert Einstein." },
  { "en": "Jembatan Brooklyn Menghubungkan Manhattan Dengan Wilayah Mana?", "id": "Brooklyn." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1922?", "id": "Niels Bohr." },
  { "en": "Apa Nama Neuron Yang Menghubungkan Neuron Lain?", "id": "Interneuron." },
  { "en": "Di Kota Manakah Gerakan 'Summer of Love' Terjadi?", "id": "San Francisco." },
  { "en": "Apa Nama Tanda Musik Untuk Mengulang Bagian Tertentu?", "id": "Tanda Ulang (Repeat Sign)." },
  { "en": "Apa Nama Batu Permata Merah Dari Mineral Korundum?", "id": "Rubi." },
  { "en": "Apa Nama Festival Seni Budaya Tahunan Di Jakarta?", "id": "Festival Jakarta." },
  { "en": "Apa Istilah Untuk Hewan Yang Memiliki Rongga Tubuh Sejati?", "id": "Selomata." },
  { "en": "Apa Istilah Untuk Studi Ilmiah Tentang Gunung Berapi?", "id": "Vulkanologi." },
  { "en": "Siapa Ilmuwan Yang Menemukan Struktur DNA?", "id": "Watson Dan Crick." },
  { "en": "Ponte Vecchio Adalah Jembatan Terkenal Di Kota Mana?", "id": "Florence." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1922?", "id": "Francis Aston." },
  { "en": "Apa Nama Lapisan Lemak Pelindung Di Sekitar Akson?", "id": "Selubung Mielin." },
  { "en": "Di Negara Mana 'Revolusi Tenang' Terjadi?", "id": "Kanada (Quebec)." },
  { "en": "Apa Nama Lima Garis Paralel Dalam Notasi Musik?", "id": "Paranada." },
  { "en": "Apa Nama Batu Permata Biru Dari Mineral Korundum?", "id": "Safir." },
  { "en": "Apa Nama Tradisi Lisan Mendongeng Dari Aceh?", "id": "Hikayat." },
  { "en": "Apa Istilah Untuk Hewan Yang Tidak Memiliki Rongga Tubuh?", "id": "Aselomata." },
  { "en": "Apa Istilah Untuk Studi Ilmiah Tentang Batuan?", "id": "Petrologi." },
  { "en": "Siapa Ilmuwan Yang Dikenal Sebagai 'Bapak Genetika'?", "id": "Gregor Mendel." },
  { "en": "Jembatan Rantai Széchenyi Terletak Di Kota Mana?", "id": "Budapest." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1923?", "id": "Banting Dan Macleod." },
  { "en": "Apa Nama Celah Antara Dua Sel Saraf?", "id": "Sinapsis." },
  { "en": "Di Kota Manakah Peristiwa 'Pembantaian Hari St. Valentine'?", "id": "Chicago." },
  { "en": "Apa Nama Simbol Di Awal Paranada Yang Menentukan Kunci?", "id": "Tanda Kunci (Clef)." },
  { "en": "Apa Nama Batu Permata Ungu Dari Mineral Kuarsa?", "id": "Kecubung (Amethyst)." },
  { "en": "Apa Nama Upacara Adat Untuk Membersihkan Desa?", "id": "Bersih Desa." },
  { "en": "Apa Istilah Untuk Hewan Dengan Rongga Tubuh Semu?", "id": "Pseudoselomata." },
  { "en": "Apa Istilah Untuk Studi Ilmiah Tentang Fosil?", "id": "Paleontologi." },
  { "en": "Siapa Ilmuwan Yang Mengemukakan Hukum Gerak Planet?", "id": "Johannes Kepler." },
  { "en": "Jembatan Vasco da Gama Adalah Jembatan Terpanjang Di Eropa?", "id": "Benar." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1924?", "id": "Władysław Reymont." },
  { "en": "Apa Nama Potensial Listrik Saat Neuron Beristirahat?", "id": "Potensial Istirahat." },
  { "en": "Di Negara Mana 'Pemberontakan Boxer' Terjadi?", "id": "Tiongkok." },
  { "en": "Apa Nama Tanda Nada Yang Menunjukkan Durasi?", "id": "Nilai Nada." },
  { "en": "Apa Nama Batu Permata Kuning Dari Mineral Kuarsa?", "id": "Sitrin." },
  { "en": "Apa Nama Festival Budaya Bahari Di Banyuwangi?", "id": "Banyuwangi Ethno Carnival." },
  { "en": "Apa Istilah Untuk Hewan Yang Memiliki Tiga Lapisan Germinal?", "id": "Triploblastik." },
  { "en": "Apa Istilah Untuk Studi Ilmiah Tentang Air?", "id": "Hidrologi." },
  { "en": "Siapa Ilmuwan Yang Menemukan Sel Pertama Kali?", "id": "Robert Hooke." },
  { "en": "Jembatan Akashi Kaikyō Terletak Di Negara Mana?", "id": "Jepang." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1925?", "id": "Chamberlain Dan Dawes." },
  { "en": "Apa Nama Lonjakan Listrik Singkat Di Akson?", "id": "Potensial Aksi." },
  { "en": "Di Kota Manakah 'Pemberontakan Paskah' (1916) Terjadi?", "id": "Dublin, Irlandia." },
  { "en": "Apa Nama Tanda Birama Yang Menunjukkan Jumlah Ketukan?", "id": "Tanda Birama." },
  { "en": "Apa Nama Batu Permata Yang Dihasilkan Oleh Moluska?", "id": "Mutiara." },
  { "en": "Apa Nama Kesenian Drama Tari Dari Surakarta?", "id": "Wayang Wong." },
  { "en": "Apa Istilah Untuk Hewan Dengan Dua Lapisan Germinal?", "id": "Diploblastik." },
  { "en": "Apa Istilah Untuk Studi Ilmiah Tentang Gua?", "id": "Speleologi." },
  { "en": "Siapa Ilmuwan Yang Dikenal Sebagai 'Bapak Mikrobiologi'?", "id": "Antonie van Leeuwenhoek." },
  { "en": "Jembatan Tsing Ma Di Hong Kong Adalah Jenis Jembatan Apa?", "id": "Jembatan Gantung." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1924?", "id": "Manne Siegbahn." },
  { "en": "Apa Nama Proses Pemulihan Neuron Setelah Potensial Aksi?", "id": "Periode Refraktori." },
  { "en": "Di Negara Mana 'Revolusi Mawar' Terjadi?", "id": "Georgia." },
  { "en": "Apa Istilah Musik Untuk Kecepatan Sebuah Lagu?", "id": "Tempo." },
  { "en": "Apa Nama Resin Fosil Pohon Yang Menjadi Batu Permata?", "id": "Amber." },
  { "en": "Apa Nama Upacara Adat Pernikahan Khas Batak?", "id": "Martumpol." },
  { "en": "Apa Istilah Untuk Hewan Yang Bertelur?", "id": "Ovipar." },
  { "en": "Apa Istilah Untuk Studi Ilmiah Tentang Gletser?", "id": "Glasiologi." },
  { "en": "Siapa Ilmuwan Yang Dikenal Sebagai 'Bapak Taksonomi'?", "id": "Carolus Linnaeus." },
  { "en": "Jembatan Menara (Tower Bridge) Terletak Di Kota Mana?", "id": "London." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1925?", "id": "Richard Zsigmondy." },
  { "en": "Apa Nama Sel Saraf Yang Membawa Sinyal Ke Otak?", "id": "Neuron Aferen." },
  { "en": "Di Negara Mana 'Pemberontakan Mau Mau' Terjadi?", "id": "Kenya." },
  { "en": "Apa Istilah Musik Untuk Volume Atau Kekerasan Nada?", "id": "Dinamika." },
  { "en": "Apa Nama Batu Permata Hijau Kebiruan Dari Mineral Beril?", "id": "Akuamarin." },
  { "en": "Apa Nama Festival Seni Pertunjukan Di Bali?", "id": "Pesta Kesenian Bali." },
  { "en": "Apa Istilah Untuk Hewan Yang Melahirkan?", "id": "Vivipar." },
  { "en": "Apa Istilah Untuk Studi Ilmiah Tentang Lautan?", "id": "Oseanografi." },
  { "en": "Siapa Ilmuwan Yang Mengemukakan Teori Evolusi?", "id": "Charles Darwin." },
  { "en": "Kota Mana Di Amerika Serikat Yang Dijuluki 'Kota Kopi'?", "id": "Seattle." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1947?", "id": "The Quakers." },
  { "en": "Apa Nama Selaput Pelindung Yang Membungkus Jantung?", "id": "Perikardium." },
  { "en": "Di Negara Manakah 'Pemberontakan Sepoy' (1857) Terjadi?", "id": "India." },
  { "en": "The Art Institute of Chicago Terletak Di Kota Mana?", "id": "Chicago." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Sifat' Kepribadian?", "id": "Gordon Allport." },
  { "en": "Apa Kode IATA Untuk Bandara Internasional Toronto Pearson?", "id": "YYZ." },
  { "en": "Apa Nama Kain Tenun Tradisional Khas Dari Tana Toraja?", "id": "Tenun Toraja." },
  { "en": "Apa Istilah Untuk Proses Evolusi Cepat Beragam Spesies?", "id": "Radiasi Adaptif." },
  { "en": "Hukum Ampere Adalah Prinsip Dasar Dalam Bidang Ilmu Apa?", "id": "Elektromagnetisme." },
  { "en": "Kota Mana Di Tiongkok Yang Dijuluki 'Kota Musim Semi'?", "id": "Kunming." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1928?", "id": "Sigrid Undset." },
  { "en": "Apa Nama Lapisan Otot Di Dinding Jantung?", "id": "Miokardium." },
  { "en": "Di Negara Manakah 'Pemberontakan Taiping' Terjadi?", "id": "Tiongkok." },
  { "en": "Museum Seni Modern (MoMA) Terletak Di Kota Mana?", "id": "New York." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Kebutuhan'?", "id": "Abraham Maslow." },
  { "en": "Apa Kode IATA Untuk Bandara Internasional Sydney?", "id": "SYD." },
  { "en": "Apa Nama Ukiran Kayu Khas Suku Asmat?", "id": "Patung Asmat." },
  { "en": "Apa Istilah Untuk Dua Spesies Yang Berevolusi Bersama?", "id": "Koevolusi." },
  { "en": "Hukum Fick Adalah Prinsip Dasar Dalam Bidang Ilmu Apa?", "id": "Difusi." },
  { "en": "Kota Mana Di Australia Yang Dijuluki 'Kota Pelabuhan'?", "id": "Sydney." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1919?", "id": "Johannes Stark." },
  { "en": "Apa Nama Lapisan Dalam Dinding Jantung?", "id": "Endokardium." },
  { "en": "Di Negara Manakah 'Revolusi Kekuatan Rakyat' (EDSA) Terjadi?", "id": "Filipina." },
  { "en": "Museum Prado Adalah Museum Seni Utama Di Kota Mana?", "id": "Madrid." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Belajar Sosial'?", "id": "Albert Bandura." },
  { "en": "Apa Kode IATA Untuk Bandara Internasional Munich?", "id": "MUC." },
  { "en": "Apa Nama Batik Dengan Motif Awan Dari Cirebon?", "id": "Batik Mega Mendung." },
  { "en": "Apa Istilah Untuk Struktur Tubuh Yang Mirip Tetapi Beda Asal?", "id": "Struktur Analog." },
  { "en": "Hukum Snellius Berlaku Dalam Bidang Ilmu Apa?", "id": "Optika." },
  { "en": "Kota Mana Di Selandia Baru Yang Dijuluki 'Kota Layar'?", "id": "Auckland." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1919?", "id": "Tidak Ada." },
  { "en": "Apa Nama Pembuluh Arteri Terbesar Dalam Tubuh?", "id": "Aorta." },
  { "en": "Di Negara Mana 'Pemberontakan Mau Mau' Terjadi?", "id": "Kenya." },
  { "en": "Rijksmuseum Adalah Museum Nasional Di Kota Mana?", "id": "Amsterdam." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Eksperimen Bobo Doll'?", "id": "Albert Bandura." },
  { "en": "Apa Kode IATA Untuk Bandara Internasional Zurich?", "id": "ZRH." },
  { "en": "Apa Nama Seni Pertunjukan Dengan Kuda Lumping?", "id": "Jaran Kepang." },
  { "en": "Apa Istilah Untuk Struktur Tubuh Yang Sama Asal Beda Fungsi?", "id": "Struktur Homolog." },
  { "en": "Hukum Stefan-Boltzmann Berlaku Dalam Bidang Ilmu Apa?", "id": "Termodinamika." },
  { "en": "Kota Mana Di Skotlandia Yang Dijuluki 'Athena Dari Utara'?", "id": "Edinburgh." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1920?", "id": "August Krogh." },
  { "en": "Apa Nama Vena Terbesar Yang Masuk Ke Jantung?", "id": "Vena Cava." },
  { "en": "Di Negara Mana 'Pemberontakan Boxer' Terjadi?", "id": "Tiongkok." },
  { "en": "Museum Hermitage Terletak Di Kota Mana?", "id": "St. Petersburg." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Little Albert Experiment'?", "id": "John B. Watson." },
  { "en": "Apa Kode IATA Untuk Bandara Internasional Istanbul?", "id": "IST." },
  { "en": "Apa Nama Tradisi Karapan Sapi Berasal Dari Pulau Mana?", "id": "Madura." },
  { "en": "Apa Istilah Untuk Organ Vestigial Yang Tidak Lagi Berfungsi?", "id": "Struktur Vestigial." },
  { "en": "Hukum Planck Berlaku Dalam Bidang Ilmu Apa?", "id": "Mekanika Kuantum." },
  { "en": "Kota Mana Di Brasil Yang Dijuluki 'Kota Luar Biasa'?", "id": "Rio de Janeiro." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1920?", "id": "Knut Hamsun." },
  { "en": "Apa Nama Arteri Yang Membawa Darah Ke Paru-Paru?", "id": "Arteri Pulmonalis." },
  { "en": "Di Kota Manakah 'Pembantaian Hari St. Valentine'?", "id": "Chicago." },
  { "en": "Museum Vatikan Terletak Di Negara Mana?", "id": "Kota Vatikan." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Terapi Perilaku Kognitif'?", "id": "Aaron Beck." },
  { "en": "Apa Kode IATA Untuk Bandara Internasional Shanghai Pudong?", "id": "PVG." },
  { "en": "Apa Nama Upacara Adat Untuk Menyambut Kelahiran Bayi?", "id": "Akikah." },
  { "en": "Apa Istilah Untuk Kemampuan Regenerasi Bagian Tubuh?", "id": "Regenerasi." },
  { "en": "Hukum Bernoulli Berlaku Dalam Bidang Ilmu Apa?", "id": "Mekanika Fluida." },
  { "en": "Kota Mana Di India Yang Dijuluki 'Kota Impian'?", "id": "Mumbai." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1920?", "id": "Léon Bourgeois." },
  { "en": "Apa Nama Vena Yang Membawa Darah Dari Paru-Paru?", "id": "Vena Pulmonalis." },
  { "en": "Di Kota Manakah 'Pemberontakan Paskah' (1916) Terjadi?", "id": "Dublin, Irlandia." },
  { "en": "Museum Acropolis Terletak Di Kota Mana?", "id": "Athena." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Teori Medan'?", "id": "Kurt Lewin." },
  { "en": "Apa Kode IATA Untuk Bandara Internasional Amsterdam Schiphol?", "id": "AMS." },
  { "en": "Apa Nama Upacara Adat Pernikahan Khas Jawa?", "id": "Siraman, Midodareni." },
  { "en": "Apa Istilah Untuk Hewan Yang Bertelur?", "id": "Ovipar." },
  { "en": "Hukum Hooke Berlaku Dalam Bidang Ilmu Apa?", "id": "Mekanika (Elastisitas)." },
  { "en": "Kota Mana Di Ceko Yang Dijuluki 'Jantung Eropa'?", "id": "Praha." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1921?", "id": "Albert Einstein." },
  { "en": "Apa Nama Katup Antara Atrium Kiri Dan Ventrikel Kiri?", "id": "Katup Mitral." },
  { "en": "Di Negara Mana 'Revolusi Mawar' Terjadi?", "id": "Georgia." },
  { "en": "Museum Ghibli Terletak Di Kota Mana?", "id": "Tokyo." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Uji Asosiasi Implisit'?", "id": "Anthony Greenwald." },
  { "en": "Apa Kode IATA Untuk Bandara Internasional Madrid Barajas?", "id": "MAD." },
  { "en": "Apa Nama Ritual Tolak Bala Masyarakat Pesisir?", "id": "Sedekah Laut." },
  { "en": "Apa Istilah Untuk Hewan Yang Melahirkan?", "id": "Vivipar." },
  { "en": "Hukum Wien Berlaku Dalam Bidang Ilmu Apa?", "id": "Termodinamika." },
  { "en": "Kota Mana Di Irlandia Utara Yang Dijuluki 'Kota Titanic'?", "id": "B Belfast." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1921?", "id": "Frederick Soddy." },
  { "en": "Apa Nama Katup Antara Atrium Kanan Dan Ventrikel Kanan?", "id": "Katup Trikuspid." },
  { "en": "Di Negara Mana 'Pemberontakan Mau Mau' Terjadi?", "id": "Kenya." },
  { "en": "Museum Orsay Terletak Di Kota Mana?", "id": "Paris." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Efek Lucifer'?", "id": "Philip Zimbardo." },
  { "en": "Apa Kode IATA Untuk Bandara Internasional Melbourne?", "id": "MEL." },
  { "en": "Apa Nama Upacara Adat Kesyukuran Suku Baduy?", "id": "Seba Baduy." },
  { "en": "Apa Istilah Untuk Hewan Yang Bertelur Dan Melahirkan?", "id": "Ovovivipar." },
  { "en": "Hukum Hess Berlaku Dalam Bidang Ilmu Apa?", "id": "Kimia (Termokimia)." },
  { "en": "Kota Mana Di Italia Yang Dijuluki 'Kota Di Atas Air'?", "id": "Venesia." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1946?", "id": "Emily Greene Balch, John Mott." },
  { "en": "Apa Nama Selaput Pelindung Yang Membungkus Jantung?", "id": "Perikardium." },
  { "en": "Di Negara Mana 'Pemberontakan Sepoy' (1857) Terjadi?", "id": "India." },
  { "en": "The Art Institute of Chicago Terletak Di Kota Mana?", "id": "Chicago." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Sifat' Kepribadian?", "id": "Gordon Allport." },
  { "en": "Pegunungan Apennine Berada Di Negara Mana?", "id": "Italia." },
  { "en": "Taman Nasional Komodo Terkenal Dengan Hewan Apa?", "id": "Komodo." },
  { "en": "Bandara Mana Yang Tersibuk Di Dunia Berdasarkan Lalu Lintas Penumpang?", "id": "Hartsfield-Jackson Atlanta." },
  { "en": "Apa Nama Konstanta Fisika Yang Melambangkan Kecepatan Cahaya?", "id": "c." },
  { "en": "Kota Mana Di Tiongkok Yang Dijuluki 'Kota Musim Semi'?", "id": "Kunming." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1928?", "id": "Sigrid Undset." },
  { "en": "Apa Nama Lapisan Otot Di Dinding Jantung?", "id": "Miokardium." },
  { "en": "Di Negara Mana 'Pemberontakan Taiping' Terjadi?", "id": "Tiongkok." },
  { "en": "Museum Seni Modern (MoMA) Terletak Di Kota Mana?", "id": "New York." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Kelekatan'?", "id": "John Bowlby." },
  { "en": "Pegunungan Alpen Terletak Di Benua Mana?", "id": "Eropa." },
  { "en": "Taman Nasional Ujung Kulon Terkenal Dengan Hewan Apa?", "id": "Badak Jawa." },
  { "en": "Bandara Mana Yang Tersibuk Di Dunia Berdasarkan Lalu Lintas Kargo?", "id": "Bandara Internasional Hong Kong." },
  { "en": "Apa Nama Konstanta Fisika Yang Melambangkan Gravitasi?", "id": "G." },
  { "en": "Kota Mana Di Australia Yang Dijuluki 'Kota Pelabuhan'?", "id": "Sydney." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1919?", "id": "Johannes Stark." },
  { "en": "Apa Nama Lapisan Dalam Dinding Jantung?", "id": "Endokardium." },
  { "en": "Di Negara Mana 'Revolusi Kekuatan Rakyat' (EDSA) Terjadi?", "id": "Filipina." },
  { "en": "Museum Prado Adalah Museum Seni Utama Di Kota Mana?", "id": "Madrid." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Uji Marshmallow'?", "id": "Walter Mischel." },
  { "en": "Pegunungan Rocky Terletak Di Benua Mana?", "id": "Amerika Utara." },
  { "en": "Taman Nasional Tanjung Puting Terkenal Dengan Hewan Apa?", "id": "Orangutan." },
  { "en": "Bandara Mana Yang Memiliki Landasan Pacu Terpanjang Di Dunia?", "id": "Bandara Qamdo Bamda." },
  { "en": "Apa Nama Konstanta Fisika Yang Melambangkan Energi Kuantum?", "id": "h (Konstanta Planck)." },
  { "en": "Kota Mana Di Selandia Baru Yang Dijuluki 'Kota Layar'?", "id": "Auckland." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1919?", "id": "Tidak Ada." },
  { "en": "Apa Nama Pembuluh Arteri Terbesar Dalam Tubuh?", "id": "Aorta." },
  { "en": "Di Negara Mana 'Pemberontakan Mau Mau' Terjadi?", "id": "Kenya." },
  { "en": "Rijksmuseum Adalah Museum Nasional Di Kota Mana?", "id": "Amsterdam." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Teori Belajar Kognitif Sosial'?", "id": "Albert Bandura." },
  { "en": "Pegunungan Andes Terletak Di Benua Mana?", "id": "Amerika Selatan." },
  { "en": "Taman Nasional Way Kambas Terkenal Dengan Hewan Apa?", "id": "Gajah Sumatra." },
  { "en": "Bandara Mana Yang Terletak Di Ketinggian Tertinggi?", "id": "Bandara Daocheng Yading." },
  { "en": "Apa Nama Konstanta Fisika Untuk Gas Ideal?", "id": "R." },
  { "en": "Kota Mana Di Skotlandia Yang Dijuluki 'Athena Dari Utara'?", "id": "Edinburgh." },
  { "en": "Siapa Pemenang Nobel Fisiologi Pada Tahun 1920?", "id": "August Krogh." },
  { "en": "Apa Nama Vena Terbesar Yang Masuk Ke Jantung?", "id": "Vena Cava." },
  { "en": "Di Negara Mana 'Pemberontakan Boxer' Terjadi?", "id": "Tiongkok." },
  { "en": "Museum Hermitage Terletak Di Kota Mana?", "id": "St. Petersburg." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Little Albert Experiment'?", "id": "John B. Watson." },
  { "en": "Pegunungan Himalaya Terletak Di Benua Mana?", "id": "Asia." },
  { "en": "Taman Nasional Lorentz Terkenal Dengan Apa?", "id": "Keanekaragaman Hayati." },
  { "en": "Bandara Mana Yang Paling Utara Di Dunia?", "id": "Bandara Alert, Kanada." },
  { "en": "Apa Nama Konstanta Fisika Untuk Bilangan Avogadro?", "id": "Nₐ." },
  { "en": "Kota Mana Di Brasil Yang Dijuluki 'Kota Luar Biasa'?", "id": "Rio de Janeiro." },
  { "en": "Siapa Pemenang Nobel Sastra Pada Tahun 1920?", "id": "Knut Hamsun." },
  { "en": "Apa Nama Arteri Yang Membawa Darah Ke Paru-Paru?", "id": "Arteri Pulmonalis." },
  { "en": "Di Kota Manakah 'Pembantaian Hari St. Valentine'?", "id": "Chicago." },
  { "en": "Museum Vatikan Terletak Di Negara Mana?", "id": "Kota Vatikan." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Terapi Perilaku Kognitif'?", "id": "Aaron Beck." },
  { "en": "Pegunungan Ural Memisahkan Benua Apa?", "id": "Eropa Dan Asia." },
  { "en": "Taman Nasional Bromo Tengger Semeru Terkenal Dengan Apa?", "id": "Gunung Bromo." },
  { "en": "Bandara Mana Yang Paling Selatan Di Dunia?", "id": "Jack F. Paulus Skiway." },
  { "en": "Apa Nama Konstanta Fisika Untuk Muatan Elementer?", "id": "e." },
  { "en": "Kota Mana Di India Yang Dijuluki 'Kota Impian'?", "id": "Mumbai." },
  { "en": "Siapa Pemenang Nobel Perdamaian Pada Tahun 1920?", "id": "Léon Bourgeois." },
  { "en": "Apa Nama Vena Yang Membawa Darah Dari Paru-Paru?", "id": "Vena Pulmonalis." },
  { "en": "Di Kota Manakah 'Pemberontakan Paskah' (1916) Terjadi?", "id": "Dublin, Irlandia." },
  { "en": "Museum Acropolis Terletak Di Kota Mana?", "id": "Athena." },
  { "en": "Siapa Psikolog Yang Mengembangkan 'Teori Medan'?", "id": "Kurt Lewin." },
  { "en": "Pegunungan Atlas Terletak Di Benua Mana?", "id": "Afrika." },
  { "en": "Taman Nasional Gunung Leuser Terkenal Dengan Hewan Apa?", "id": "Orangutan Sumatra." },
  { "en": "Bandara Mana Yang Terletak Di Sebuah Pulau Buatan?", "id": "Bandara Internasional Kansai." },
  { "en": "Apa Nama Konstanta Matematika Yang Bernilai Sekitar 3.14?", "id": "Pi (π)." },
  { "en": "Kota Mana Di Ceko Yang Dijuluki 'Jantung Eropa'?", "id": "Praha." },
  { "en": "Siapa Pemenang Nobel Fisika Pada Tahun 1921?", "id": "Albert Einstein." },
  { "en": "Apa Nama Katup Antara Atrium Kiri Dan Ventrikel Kiri?", "id": "Katup Mitral." },
  { "en": "Di Negara Mana 'Revolusi Mawar' Terjadi?", "id": "Georgia." },
  { "en": "Museum Ghibli Terletak Di Kota Mana?", "id": "Tokyo." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Uji Asosiasi Implisit'?", "id": "Anthony Greenwald." },
  { "en": "Pegunungan Kaukasus Terletak Di Antara Laut Apa?", "id": "Laut Hitam Dan Laut Kaspia." },
  { "en": "Taman Nasional Kerinci Seblat Terkenal Dengan Hewan Apa?", "id": "Harimau Sumatra." },
  { "en": "Bandara Mana Yang Terkenal Dengan 'The Jewel'?", "id": "Bandara Changi Singapura." },
  { "en": "Apa Nama Konstanta Matematika Untuk Basis Logaritma Natural?", "id": "e (Bilangan Euler)." },
  { "en": "Kota Mana Di Irlandia Utara Yang Dijuluki 'Kota Titanic'?", "id": "B Belfast." },
  { "en": "Siapa Pemenang Nobel Kimia Pada Tahun 1921?", "id": "Frederick Soddy." },
  { "en": "Apa Nama Katup Antara Atrium Kanan Dan Ventrikel Kanan?", "id": "Katup Trikuspid." },
  { "en": "Di Negara Mana 'Pemberontakan Mau Mau' Terjadi?", "id": "Kenya." },
  { "en": "Museum Orsay Terletak Di Kota Mana?", "id": "Paris." },
  { "en": "Siapa Psikolog Yang Terkenal Dengan 'Efek Lucifer'?", "id": "Philip Zimbardo." },
  { "en": "Pegunungan Appalachia Terletak Di Benua Mana?", "id": "Amerika Utara." },
  { "en": "Taman Nasional Baluran Terkenal Dengan Sabana Apa?", "id": "Sabana Bekol." },
  { "en": "Bandara Mana Yang Terkenal Dengan Pemandangan Pendaratannya?", "id": "Bandara Princess Juliana." },
  { "en": "Apa Nama Konstanta Matematika Yang Dikenal Sebagai Rasio Emas?", "id": "Phi (φ)." },
  { "en": "Tahun Berapa Pidato 'I Have a Dream' Disampaikan?", "id": "Tahun 1963." },
  { "en": "Natrium Bikarbonat Dikenal Sebagai Bahan Apa Dalam Memasak?", "id": "Soda Kue." },
  { "en": "Otot Apa Yang Berfungsi Untuk Mengangkat Lengan Ke Samping?", "id": "Otot Deltoid." },
  { "en": "Siapa Penjelajah Arab Yang Terkenal Dengan Perjalanannya?", "id": "Ibnu Battuta." },
  { "en": "Apa Nama Bentuk Musik Yang Terdiri Dari Tema Dan Variasi?", "id": "Tema Dan Variasi." },
  { "en": "Apa Bias Kognitif Dimana Orang Cenderung Mencari Konfirmasi?", "id": "Bias Konfirmasi." },
  { "en": "Siapakah Penulis Novel 'Bumi Manusia'?", "id": "Pramoedya Ananta Toer." },
  { "en": "Hewan Pengerat Seperti Tikus Termasuk Dalam Ordo Apa?", "id": "Rodentia." },
  { "en": "Apa Nama Nebula Paling Terang Yang Terlihat Dari Bumi?", "id": "Nebula Orion." },
  { "en": "Apa Nama Proses Jatuhnya Material Di Lereng Akibat Gravitasi?", "id": "Gerakan Massa." },
  { "en": "Siapa Yang Menyampaikan Pidato 'Gettysburg Address'?", "id": "Abraham Lincoln." },
  { "en": "Kalsium Karbonat Adalah Senyawa Utama Dalam Batuan Apa?", "id": "Batu Kapur." },
  { "en": "Otot Apa Yang Berfungsi Untuk Meluruskan Lengan Bawah?", "id": "Otot Trisep." },
  { "en": "Siapa Penjelajah Tiongkok Yang Terkenal Dengan Armada Besarnya?", "id": "Zheng He (Cheng Ho)." },
  { "en": "Apa Nama Komposisi Musik Untuk Satu Instrumen Tunggal?", "id": "Solo." },
  { "en": "Apa Bias Kognitif Dimana Orang Mengandalkan Informasi Pertama?", "id": "Bias Penjangkaran (Anchoring)." },
  { "en": "Siapakah Penulis Novel 'Sitti Nurbaya'?", "id": "Marah Roesli." },
  { "en": "Kelinci Dan Terwelu Termasuk Dalam Ordo Hewan Apa?", "id": "Lagomorpha." },
  { "en": "Apa Nama Gugus Bintang Terbuka Terdekat Dengan Bumi?", "id": "Hyades." },
  { "en": "Apa Nama Proses Erosi Yang Disebabkan Oleh Air?", "id": "Erosi Fluvial." },
  { "en": "Tahun Berapa Pidato 'Tear Down This Wall' Disampaikan?", "id": "Tahun 1987." },
  { "en": "Asam Asetat Adalah Komponen Utama Dalam Cairan Apa?", "id": "Cuka." },
  { "en": "Otot Apa Yang Berfungsi Untuk Menekuk Lengan Bawah?", "id": "Otot Bisep." },
  { "en": "Siapa Penjelajah Portugis Yang Pertama Mengelilingi Afrika?", "id": "Vasco da Gama." },
  { "en": "Apa Nama Komposisi Musik Untuk Dua Instrumen?", "id": "Duet." },
  { "en": "Apa Bias Kognitif Dimana Orang Mengikuti Tindakan Kelompok?", "id": "Efek Ikut-Ikutan (Bandwagon)." },
  { "en": "Siapakah Penulis Puisi 'Aku' Yang Sangat Terkenal?", "id": "Chairil Anwar." },
  { "en": "Kuda Dan Badak Termasuk Dalam Ordo Hewan Apa?", "id": "Perissodactyla." },
  { "en": "Apa Nama Nebula Yang Menyerupai Cincin?", "id": "Nebula Cincin." },
  { "en": "Apa Nama Proses Pengangkutan Sedimen Oleh Angin?", "id": "Transportasi Eolian." },
  { "en": "Siapa Yang Menyampaikan Pidato 'The Ballot or the Bullet'?", "id": "Malcolm X." },
  { "en": "Silikon Dioksida Adalah Komponen Utama Dari Apa?", "id": "Pasir Dan Kaca." },
  { "en": "Otot Apa Yang Berfungsi Untuk Mengangkat Kaki Ke Depan?", "id": "Otot Psoas Mayor." },
  { "en": "Siapa Penjelajah Spanyol Yang Menaklukkan Kerajaan Aztec?", "id": "Hernán Cortés." },
  { "en": "Apa Nama Komposisi Musik Tiga Bagian, Cepat-Lambat-Cepat?", "id": "Concerto." },
  { "en": "Apa Bias Kognitif Dimana Orang Merasa Sudah Tahu Sesuatu?", "id": "Bias Pengetahuan Kemudian (Hindsight)." },
  { "en": "Siapakah Penulis Novel 'Tenggelamnya Kapal Van Der Wijck'?", "id": "Hamka." },
  { "en": "Babi Dan Jerapah Termasuk Dalam Ordo Hewan Apa?", "id": "Artiodactyla." },
  { "en": "Apa Nama Nebula Yang Dikenal Sebagai 'Mata Kucing'?", "id": "Nebula Mata Kucing." },
  { "en": "Apa Nama Proses Pelapukan Batuan Oleh Akar Tumbuhan?", "id": "Pelapukan Biologis." },
  { "en": "Tahun Berapa Pidato 'We Shall Fight on the Beaches'?", "id": "Tahun 1940." },
  { "en": "Amonia (NH₃) Umumnya Digunakan Sebagai Apa?", "id": "Pupuk Dan Pembersih." },
  { "en": "Otot Apa Yang Berfungsi Untuk Menggerakkan Bola Mata?", "id": "Otot Ekstraokular." },
  { "en": "Siapa Penjelajah Spanyol Yang Menaklukkan Kekaisaran Inka?", "id": "Francisco Pizarro." },
  { "en": "Apa Nama Komposisi Musik Empat Bagian Untuk Orkestra?", "id": "Simfoni." },
  { "en": "Apa Bias Kognitif Dimana Orang Melebih-lebihkan Kemampuannya?", "id": "Efek Dunning-Kruger." },
  { "en": "Siapakah Penulis Novel 'Ronggeng Dukuh Paruk'?", "id": "Ahmad Tohari." },
  { "en": "Kanguru Dan Koala Termasuk Dalam Kelompok Hewan Apa?", "id": "Marsupialia." },
  { "en": "Apa Nama Nebula Yang Menyerupai Kepiting?", "id": "Nebula Kepiting." },
  { "en": "Apa Nama Proses Terbentuknya Gua Kapur?", "id": "Pelarutan (Karstifikasi)." },
  { "en": "Siapa Yang Menyampaikan Pidato Kemerdekaan Indonesia?", "id": "Soekarno." },
  { "en": "Natrium Hipoklorit Adalah Bahan Aktif Dalam Produk Apa?", "id": "Pemutih Pakaian." },
  { "en": "Otot Apa Yang Berfungsi Menggerakkan Rahang Bawah?", "id": "Otot Masseter." },
  { "en": "Siapa Penjelajah Inggris Yang Memetakan Pantai Australia?", "id": "James Cook." },
  { "en": "Apa Nama Komposisi Musik Dengan Tema Berulang?", "id": "Rondo." },
  { "en": "Apa Bias Kognitif Dimana Ketersediaan Informasi Mempengaruhi Keputusan?", "id": "Heuristik Ketersediaan." },
  { "en": "Siapakah Penulis Novel 'Harimau! Harimau!'?", "id": "Mochtar Lubis." },
  { "en": "Platipus Termasuk Dalam Ordo Hewan Apa?", "id": "Monotremata." },
  { "en": "Apa Nama Nebula Terdekat Dengan Tata Surya?", "id": "Nebula Helix." },
  { "en": "Apa Nama Proses Pembentukan Lembah Oleh Gletser?", "id": "Glasiasi." },
  { "en": "Tahun Berapa Pidato 'A Date Which Will Live in Infamy'?", "id": "Tahun 1941." },
  { "en": "Aseton Umumnya Digunakan Sebagai Apa?", "id": "Penghapus Cat Kuku." },
  { "en": "Otot Apa Yang Membentuk Dinding Jantung?", "id": "Otot Jantung (Miokardium)." },
  { "en": "Siapa Penjelajah Yang Mencapai Kutub Utara Pertama?", "id": "Robert Peary." },
  { "en": "Apa Nama Tarian Istana Yang Lambat Dan Anggun?", "id": "Minuet." },
  { "en": "Apa Bias Kognitif Dimana Orang Cenderung Malas Berpikir?", "id": "Kemalasan Kognitif." },
  { "en": "Siapakah Penulis Novel 'Layar Terkembang'?", "id": "Sutan Takdir Alisjahbana." },
  { "en": "Primata Seperti Monyet Dan Manusia Termasuk Ordo Apa?", "id": "Primates." },
  { "en": "Apa Nama Sisa Bintang Mati Di Pusat Nebula Planet?", "id": "Katai Putih." },
  { "en": "Apa Nama Proses Terkikisnya Tanah Oleh Air?", "id": "Erosi Air." },
  { "en": "Siapa Yang Menyampaikan Pidato 'I Am Prepared to Die'?", "id": "Nelson Mandela." },
  { "en": "Hidrogen Peroksida Digunakan Sebagai Apa Dalam Medis?", "id": "Antiseptik." },
  { "en": "Otot Apa Yang Membentuk Dinding Perut?", "id": "Otot Abdominal." },
  { "en": "Siapa Penjelajah Yang Mencapai Kutub Selatan Pertama?", "id": "Roald Amundsen." },
  { "en": "Apa Nama Komposisi Musik Yang Menceritakan Sebuah Kisah?", "id": "Puisi Simfonik." },
  { "en": "Apa Bias Kognitif Dimana Orang Meremehkan Waktu Tugas?", "id": "Kekeliruan Perencanaan." },
  { "en": "Siapakah Penulis Novel 'Belenggu'?", "id": "Armijn Pane." },
  { "en": "Burung Elang Dan Rajawali Termasuk Dalam Ordo Apa?", "id": "Accipitriformes." },
  { "en": "Apa Nama Nebula Yang Menjadi Tempat Pembentukan Bintang?", "id": "Nebula Emisi." },
  { "en": "Apa Nama Proses Terkikisnya Batuan Oleh Angin?", "id": "Erosi Angin (Deflasi)." },
  { "en": "Tahun Berapa Pidato 'The Iron Curtain' Disampaikan?", "id": "Tahun 1946." },
  { "en": "Etilena Glikol Adalah Komponen Utama Dalam Apa?", "id": "Antibeku (Antifreeze)." },
  { "en": "Otot Terbesar Di Tubuh Manusia Adalah Otot Apa?", "id": "Gluteus Maximus." },
  { "en": "Siapa Penjelajah Yang Membuktikan Bumi Itu Bulat?", "id": "Ekspedisi Magellan." },
  { "en": "Apa Nama Komposisi Musik Yang Ditulis Untuk Paduan Suara?", "id": "Koral." },
  { "en": "Apa Bias Kognitif Dimana Orang Menghindari Kerugian?", "id": "Aversi Kerugian." },
  { "en": "Siapakah Penulis Novel 'Pada Sebuah Kapal'?", "id": "Nh. Dini." },
  { "en": "Burung Hantu Termasuk Dalam Ordo Apa?", "id": "Strigiformes." },
  { "en": "Apa Nama Nebula Yang Terbentuk Dari Bintang Mati?", "id": "Nebula Planet." },
  { "en": "Apa Nama Proses Longsoran Salju Di Pegunungan?", "id": "Avalanche." },
  { "en": "Apa Nama Upacara Adat Pemakaman Di Tana Toraja?", "id": "Rambu Solo'." },
  { "en": "Siapakah Penulis Novel Fiksi 'The War of the Worlds'?", "id": "H.G. Wells." },
  { "en": "Apa Nama Proses Perubahan Cahaya Menjadi Energi Kimia?", "id": "Fotosintesis." },
  { "en": "Gunung Tertinggi Di Tata Surya Berada Di Planet Mana?", "id": "Mars." },
  { "en": "Periode Sejarah Apa Yang Dikenal Sebagai Zaman Pencerahan?", "id": "Abad Ke-18." },
  { "en": "Tari Kecak Adalah Seni Drama Tari Dari Pulau Mana?", "id": "Bali." },
  { "en": "Siapa Filsuf Yunani Kuno Yang Menjadi Guru Alexander Agung?", "id": "Aristoteles." },
  { "en": "Apa Nama Ibukota Kuno Kekaisaran Persia?", "id": "Persepolis." },
  { "en": "Lagu 'Bohemian Rhapsody' Dipopulerkan Oleh Band Apa?", "id": "Queen." },
  { "en": "Apa Nama Samudra Terkecil Di Dunia?", "id": "Samudra Arktik." },
  { "en": "Apa Nama Upacara Adat Untuk Menyambut Masa Panen?", "id": "Sedekah Bumi." },
  { "en": "Siapakah Penulis Novel 'Brave New World'?", "id": "Aldous Huxley." },
  { "en": "Apa Nama Proses Pemecahan Air Menggunakan Listrik?", "id": "Elektrolisis." },
  { "en": "Bulan Terbesar Di Tata Surya Adalah Satelit Planet Mana?", "id": "Jupiter." },
  { "en": "Zaman Apa Dalam Sejarah Yang Disebut Zaman Logam?", "id": "Zaman Perunggu Dan Besi." },
  { "en": "Tari Saman Adalah Kesenian Tradisional Dari Provinsi Mana?", "id": "Aceh." },
  { "en": "Siapa Filsuf Perancis Yang Terkenal Dengan 'Cogito, Ergo Sum'?", "id": "René Descartes." },
  { "en": "Apa Nama Ibukota Kekaisaran Romawi Timur?", "id": "Konstantinopel." },
  { "en": "Siapa Penyanyi Yang Dijuluki 'Raja Pop'?", "id": "Michael Jackson." },
  { "en": "Apa Nama Benua Yang Paling Dingin Dan Kering?", "id": "Antartika." },
  { "en": "Apa Nama Upacara Adat Bakar Batu Di Papua?", "id": "Barapen." },
  { "en": "Siapakah Penulis Novel '1984'?", "id": "George Orwell." },
  { "en": "Apa Nama Proses Perubahan Zat Padat Menjadi Gas?", "id": "Sublimasi." },
  { "en": "Planet Apa Yang Dikenal Memiliki Cincin Paling Spektakuler?", "id": "Saturnus." },
  { "en": "Periode Sejarah Apa Yang Ditandai Runtuhnya Kekaisaran Romawi?", "id": "Abad Pertengahan." },
  { "en": "Tari Piring Adalah Kesenian Tradisional Dari Suku Apa?", "id": "Minangkabau." },
  { "en": "Siapa Filsuf Jerman Yang Menulis 'Thus Spoke Zarathustra'?", "id": "Friedrich Nietzsche." },
  { "en": "Apa Nama Ibukota Kuno Kerajaan Mesir?", "id": "Memphis." },
  { "en": "Siapa Pelukis Yang Menciptakan Karya 'Mona Lisa'?", "id": "Leonardo da Vinci." },
  { "en": "Apa Nama Gurun Pasir Terbesar Di Dunia?", "id": "Gurun Sahara." },
  { "en": "Apa Nama Upacara Adat Lompat Batu Di Nias?", "id": "Fahombo." },
  { "en": "Siapakah Penulis Novel 'The Lord of the Rings'?", "id": "J.R.R. Tolkien." },
  { "en": "Apa Nama Proses Pembusukan Bahan Organik Tanpa Oksigen?", "id": "Fermentasi Anaerobik." },
  { "en": "Planet Apa Yang Memiliki Rotasi Paling Cepat?", "id": "Jupiter." },
  { "en": "Zaman Apa Dalam Prasejarah Yang Ditandai Penggunaan Batu?", "id": "Zaman Batu." },
  { "en": "Tari Jaipong Adalah Tarian Modern Dari Provinsi Mana?", "id": "Jawa Barat." },
  { "en": "Siapa Filsuf Tiongkok Kuno Yang Mengajarkan Taoisme?", "id": "Lao Tzu." },
  { "en": "Apa Nama Ibukota Kekaisaran Aztec?", "id": "Tenochtitlan." },
  { "en": "Siapa Komposer Yang Menciptakan 'Symphony No. 5'?", "id": "Ludwig van Beethoven." },
  { "en": "Apa Nama Selat Yang Memisahkan Asia Dan Amerika?", "id": "Selat Bering." },
  { "en": "Apa Nama Upacara Adat Pernikahan Khas Jawa?", "id": "Panggih." },
  { "en": "Siapakah Penulis Novel 'Frankenstein'?", "id": "Mary Shelley." },
  { "en": "Apa Nama Proses Perubahan Gas Menjadi Cair?", "id": "Kondensasi." },
  { "en": "Planet Apa Yang Memiliki Hari Terpanjang?", "id": "Venus." },
  { "en": "Periode Sejarah Apa Yang Dikenal Sebagai Kelahiran Kembali?", "id": "Renaisans." },
  { "en": "Tari Tor-Tor Adalah Tarian Tradisional Dari Suku Mana?", "id": "Suku Batak." },
  { "en": "Siapa Filsuf Yang Dikenal Sebagai Bapak Liberalisme?", "id": "John Locke." },
  { "en": "Apa Nama Ibukota Kekaisaran Inka?", "id": "Cusco." },
  { "en": "Siapa Pelukis Yang Menciptakan 'The Starry Night'?", "id": "Vincent van Gogh." },
  { "en": "Apa Nama Palung Terdalam Di Dunia?", "id": "Palung Mariana." },
  { "en": "Apa Nama Upacara Adat Pemakaman Di Bali?", "id": "Ngaben." },
  { "en": "Siapakah Penulis Novel 'Moby-Dick'?", "id": "Herman Melville." },
  { "en": "Apa Nama Proses Pelepasan Energi Dari Makanan?", "id": "Respirasi." },
  { "en": "Planet Apa Yang Memiliki Suhu Permukaan Paling Panas?", "id": "Venus." },
  { "en": "Zaman Apa Yang Ditandai Dengan Revolusi Industri?", "id": "Abad Ke-18 Dan Ke-19." },
  { "en": "Tari Serimpi Adalah Tarian Sakral Dari Istana Mana?", "id": "Keraton Jawa." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Konsep 'Superman'?", "id": "Friedrich Nietzsche." },
  { "en": "Apa Nama Ibukota Kekaisaran Ottoman?", "id": "Istanbul (Konstantinopel)." },
  { "en": "Siapa Komposer Yang Menciptakan 'The Four Seasons'?", "id": "Antonio Vivaldi." },
  { "en": "Apa Nama Sungai Terpanjang Di Dunia?", "id": "Sungai Nil." },
  { "en": "Apa Nama Upacara Adat Untuk Meminta Hujan?", "id": "Ritual Minta Hujan." },
  { "en": "Siapakah Penulis Novel 'Don Quixote'?", "id": "Miguel de Cervantes." },
  { "en": "Apa Nama Proses Perubahan Cair Menjadi Padat?", "id": "Pembekuan." },
  { "en": "Planet Apa Yang Memiliki Atmosfer Paling Tipis?", "id": "Merkurius." },
  { "en": "Periode Sejarah Apa Yang Ditandai Perang Dingin?", "id": "Pasca-Perang Dunia II." },
  { "en": "Tari Golek Adalah Tarian Klasik Dari Daerah Mana?", "id": "Yogyakarta." },
  { "en": "Siapa Filsuf Yang Menjadi Murid Plato?", "id": "Aristoteles." },
  { "en": "Apa Nama Ibukota Kekaisaran Mongol?", "id": "Karakorum." },
  { "en": "Siapa Pelukis Yang Menciptakan 'The Persistence of Memory'?", "id": "Salvador Dalí." },
  { "en": "Apa Nama Gunung Berapi Tertinggi Di Dunia?", "id": "Ojos del Salado." },
  { "en": "Apa Nama Upacara Adat Turun Tanah Bayi Di Jawa?", "id": "Tedak Siten." },
  { "en": "Siapakah Penulis 'The Adventures of Huckleberry Finn'?", "id": "Mark Twain." },
  { "en": "Apa Nama Proses Pergerakan Air Melalui Tanaman?", "id": "Transpirasi." },
  { "en": "Planet Apa Yang Paling Ringan Di Tata Surya?", "id": "Saturnus." },
  { "en": "Zaman Apa Yang Dikenal Sebagai Zaman Penjelajahan?", "id": "Abad Ke-15 Hingga Ke-17." },
  { "en": "Tari Topeng Cirebon Berasal Dari Kota Mana?", "id": "Cirebon." },
  { "en": "Siapa Filsuf Yang Terkenal Dengan Metode Sokrates?", "id": "Socrates." },
  { "en": "Apa Nama Ibukota Kuno Kerajaan Yunani?", "id": "Athena." },
  { "en": "Siapa Komposer Yang Menciptakan 'The Magic Flute'?", "id": "Wolfgang Amadeus Mozart." },
  { "en": "Apa Nama Air Terjun Tertinggi Di Dunia?", "id": "Air Terjun Angel." },
  { "en": "Apa Nama Festival Seni Dan Budaya Tahunan Di Bali?", "id": "Pesta Kesenian Bali." },
  { "en": "Siapakah Penulis Novel 'Pride and Prejudice'?", "id": "Jane Austen." },
  { "en": "Apa Nama Proses Pembentukan Gula Pada Tanaman?", "id": "Fotosintesis." },
  { "en": "Planet Apa Yang Memiliki Kecepatan Angin Tertinggi?", "id": "Neptunus." },
  { "en": "Periode Sejarah Apa Yang Disebut 'Belle Époque'?", "id": "Akhir Abad Ke-19." },
  { "en": "Tari Bedhaya Adalah Tarian Sakral Dari Keraton Mana?", "id": "Keraton Surakarta Dan Yogyakarta." },
  { "en": "Siapa Filsuf Yang Dikenal Dengan Konsep 'Tabula Rasa'?", "id": "John Locke." },
  { "en": "Apa Nama Ibukota Kekaisaran Romawi?", "id": "Roma." },
  { "en": "Siapa Pelukis Yang Menciptakan 'The Scream'?", "id": "Edvard Munch." },
  { "en": "Apa Nama Benua Terbesar Di Dunia?", "id": "Asia." },



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
