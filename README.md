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


    { "en": "Surabaya (Jawa Timur)?", "id": "Soekarno." },
    { "en": "Bukittinggi (Sumatera Barat)?", "id": "Mohammad Hatta." },
    { "en": "Purbalingga (Jawa Tengah)?", "id": "Jenderal Soedirman." },
    { "en": "Magelang (Jawa Tengah)?", "id": "Pangeran Diponegoro." },
    { "en": "Yogyakarta (DI Yogyakarta)?", "id": "Ki Hadjar Dewantara." },
    { "en": "Jepara (Jawa Tengah)?", "id": "R.A. Kartini." },
    { "en": "Aceh Besar (Aceh)?", "id": "Cut Nyak Dien." },
    { "en": "Pasaman (Sumatera Barat)?", "id": "Tuanku Imam Bonjol." },
    { "en": "Saparua (Maluku)?", "id": "Kapitan Pattimura." },
    { "en": "Gowa (Sulawesi Selatan)?", "id": "Sultan Hasanuddin." },
    { "en": "Tapanuli Utara (Sumatera Utara)?", "id": "Sisingamangaraja XII." },
    { "en": "Bandung (Jawa Barat)?", "id": "Dewi Sartika." },
    { "en": "Semarang (Jawa Tengah)?", "id": "Dr. Cipto Mangunkusumo." },
    { "en": "Banjarmasin (Kalimantan Selatan)?", "id": "Pangeran Antasari." },
    { "en": "Denpasar (Bali)?", "id": "I Gusti Ngurah Rai." },
    { "en": "Surakarta (Jawa Tengah)?", "id": "Slamet Rijadi." },
    { "en": "Blitar (Jawa Timur)?", "id": "Supriyadi." },
    { "en": "Lima Puluh Kota (Sumatera Barat)?", "id": "Tan Malaka." },
    { "en": "Ternate (Maluku Utara)?", "id": "Sultan Baabullah." },
    { "en": "Mojokerto (Jawa Timur)?", "id": "Raden Wijaya." },
    { "en": "Singaraja (Bali)?", "id": "Untung Suropati." },
    { "en": "Blora (Jawa Tengah)?", "id": "Tirto Adhi Soerjo." },
    { "en": "Manado (Sulawesi Utara)?", "id": "Sam Ratulangi." },
    { "en": "Jakarta Pusat (DKI Jakarta)?", "id": "Mohammad Hoesni Thamrin." },
    { "en": "Kudus (Jawa Tengah)?", "id": "K.H. R. Asnawi." },
    { "en": "Jombang (Jawa Timur)?", "id": "K.H. Hasyim Asy'ari." },
    { "en": "Pekalongan (Jawa Tengah)?", "id": "K.H. Ahmad Dahlan." },
    { "en": "Rembang (Jawa Tengah)?", "id": "Dr. Soetomo." },
    { "en": "Tasikmalaya (Jawa Barat)?", "id": "K.H. Zainal Mustafa." },
    { "en": "Bantul (DI Yogyakarta)?", "id": "Sultan Agung Hanyokrokusumo." },
    { "en": "Tidore (Maluku Utara)?", "id": "Sultan Nuku." },
    { "en": "Makassar (Sulawesi Selatan)?", "id": "Ranggong Daeng Romo." },
    { "en": "Padang (Sumatera Barat)?", "id": "Bagindo Azizchan." },
    { "en": "Kaur (Bengkulu)?", "id": "Pangeran Patah." },
    { "en": "Bengkulu (Bengkulu)?", "id": "Fatmawati." },
    { "en": "Lampung Selatan (Lampung)?", "id": "Radin Inten II." },
    { "en": "Palembang (Sumatera Selatan)?", "id": "Sultan Mahmud Badaruddin II." },
    { "en": "Pagar Alam (Sumatera Selatan)?", "id": "Demang Lebar Daun." },
    { "en": "Kerinci (Jambi)?", "id": "Depati Parbo." },
    { "en": "Pekanbaru (Riau)?", "id": "Sultan Syarif Kasim II." },
    { "en": "Rokan Hulu (Riau)?", "id": "Tuanku Tambusai." },
    { "en": "Sibolga (Sumatera Utara)?", "id": "Dr. Ferdinand Lumban Tobing." },
    { "en": "Karo (Sumatera Utara)?", "id": "Kirap Rempa." },
    { "en": "Pidie (Aceh)?", "id": "Teuku Muhammad Hasan." },
    { "en": "Lhokseumawe (Aceh)?", "id": "Cut Meutia." },
    { "en": "Cirebon (Jawa Barat)?", "id": "Sunan Gunung Jati." },
    { "en": "Sumedang (Jawa Barat)?", "id": "Pangeran Kornel." },
    { "en": "Purwakarta (Jawa Barat)?", "id": "K.H. Noer Ali." },
    { "en": "Serang (Banten)?", "id": "Sultan Ageng Tirtayasa." },
    { "en": "Pandeglang (Banten)?", "id": "Syekh Yusuf." },
    { "en": "Banyumas (Jawa Tengah)?", "id": "Gatot Soebroto." },
    { "en": "Kediri (Jawa Timur)?", "id": "Joyoboyo." },
    { "en": "Madiun (Jawa Timur)?", "id": "Sartono." },
    { "en": "Nganjuk (Jawa Timur)?", "id": "Supeno." },
    { "en": "Bangkalan (Jawa Timur)?", "id": "K.H. Mohammad Kholil." },
    { "en": "Sumenep (Jawa Timur)?", "id": "Halim Perdanakusuma." },
    { "en": "Badung (Bali)?", "id": "I Gusti Ketut Jelantik." },
    { "en": "Klungkung (Bali)?", "id": "Ida Dewa Agung Istri Kanya." },
    { "en": "Sumbawa (Nusa Tenggara Barat)?", "id": "Sultan Muhammad Kaharuddin III." },
    { "en": "Kupang (Nusa Tenggara Timur)?", "id": "Izaak Huru Doko." },
    { "en": "Pontianak (Kalimantan Barat)?", "id": "Sultan Hamid II." },
    { "en": "Sintang (Kalimantan Barat)?", "id": "Apang Semangai." },
    { "en": "Kotawaringin Barat (Kalimantan Tengah)?", "id": "Panglima Utar." },
    { "en": "Palangkaraya (Kalimantan Tengah)?", "id": "Tjilik Riwut." },
    { "en": "Samarinda (Kalimantan Timur)?", "id": "A.W. Sjahranie." },
    { "en": "Bone (Sulawesi Selatan)?", "id": "Arung Palakka." },
    { "en": "Wajo (Sulawesi Selatan)?", "id": "Lamaddukelleng." },
    { "en": "Mamuju (Sulawesi Barat)?", "id": "Andi Depu." },
    { "en": "Gorontalo (Gorontalo)?", "id": "Nani Wartabone." },
    { "en": "Poso (Sulawesi Tengah)?", "id": "Toma Ibi." },
    { "en": "Palu (Sulawesi Tengah)?", "id": "Pue Lasadindi." },
    { "en": "Kendari (Sulawesi Tenggara)?", "id": "Haluoleo." },
    { "en": "Ambon (Maluku)?", "id": "Martha Christina Tiahahu." },
    { "en": "Jayapura (Papua)?", "id": "Silas Papare." },
    { "en": "Biak Numfor (Papua)?", "id": "Frans Kaisiepo." },
    { "en": "Salatiga (Jawa Tengah)?", "id": "Yos Sudarso." },
    { "en": "Pematangsiantar (Sumatera Utara)?", "id": "Adam Malik." },
    { "en": "Tomohon (Sulawesi Utara)?", "id": "Lambertus Nicodemus Palar." },
    { "en": "Lombok Timur (Nusa Tenggara Barat)?", "id": "Muhammad Zainuddin Abdul Madjid." },
    { "en": "Cilacap (Jawa Tengah)?", "id": "Sukarjo Wiryopranoto." },
    { "en": "Kutai Kartanegara (Kalimantan Timur)?", "id": "Sultan Aji Muhammad Idris." },
    { "en": "Tanjungpinang (Kepulauan Riau)?", "id": "Raja Haji Fisabilillah." },
    { "en": "Pati (Jawa Tengah)?", "id": "K.H. Sahal Mahfudh." },
    { "en": "Garut (Jawa Barat)?", "id": "Ahmad Subardjo." },
    { "en": "Gresik (Jawa Timur)?", "id": "Sunan Giri." },
    { "en": "Sidoarjo (Jawa Timur)?", "id": "K.H. Ali Masykur Musa." },
    { "en": "Tulungagung (Jawa Timur)?", "id": "Mustopo." },
    { "en": "Cianjur (Jawa Barat)?", "id": "Utuy Tatang Sontani." },
    { "en": "Indramayu (Jawa Barat)?", "id": "K.H. Abdul Halim." },
    { "en": "Probolinggo (Jawa Timur)?", "id": "K.H. Hasan Genggong." },
    { "en": "Polewali Mandar (Sulawesi Barat)?", "id": "Andi Abdullah Bau Massepe." },
    { "en": "Luwu (Sulawesi Selatan)?", "id": "Opu Daeng Risadju." },
    { "en": "Minahasa (Sulawesi Utara)?", "id": "Maria Walanda Maramis." },
    { "en": "Tolitoli (Sulawesi Tengah)?", "id": "Mohammad Ali." },
    { "en": "Buton (Sulawesi Tenggara)?", "id": "Sultan Himayatuddin Muhammad Saidi." },
    { "en": "Sikka (Nusa Tenggara Timur)?", "id": "Frans Seda." },
    { "en": "Tabalong (Kalimantan Selatan)?", "id": "Panglima Batur." },
    { "en": "Paser (Kalimantan Timur)?", "id": "Aji Pangeran Sinum Panji Mendapa." },
    { "en": "Ketapang (Kalimantan Barat)?", "id": "Tanjungpura." },
    { "en": "Bungo (Jambi)?", "id": "Sultan Thaha Syaifuddin." },
    { "en": "Asahan (Sumatera Utara)?", "id": "Haji Adam Malik." },
    { "en": "Agam (Sumatera Barat)?", "id": "Rasuna Said." },
    { "en": "Tegal (Jawa Tengah)?", "id": "Ki Gede Sebayu." },
    { "en": "Klaten (Jawa Tengah)?", "id": "Ki Ageng Gribig." },
    { "en": "Boyolali (Jawa Tengah)?", "id": "Prof. Dr. Soeharso." },
    { "en": "Nusa Laut (Maluku)?", "id": "Martha Christina Tiahahu." },
    { "en": "Pangkalpinang (Kepulauan Bangka Belitung)?", "id": "Depati Amir." },
    { "en": "Situbondo (Jawa Timur)?", "id": "K.H.R. As'ad Syamsul Arifin." },
    { "en": "Bima (Nusa Tenggara Barat)?", "id": "Sultan Muhammad Salahuddin." },
    { "en": "Padangsidimpuan (Sumatera Utara)?", "id": "Lafran Pane." },
    { "en": "Kotabaru (Kalimantan Selatan)?", "id": "Ir. Pangeran Mohammad Noor." },
    { "en": "Barito Utara (Kalimantan Tengah)?", "id": "D.I. Panjaitan." },
    { "en": "Sawahlunto (Sumatera Barat)?", "id": "Mohammad Yamin." }


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
