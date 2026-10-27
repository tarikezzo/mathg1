<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>مغامرة الرياضيات للأطفال | Math Magic Adventure</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&family=Fredoka:wght@400;600;700&display=swap" rel="stylesheet">
    <!-- Canvas Confetti Library -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            purple: '#7C3AED',
                            pink: '#EC4899',
                            yellow: '#F59E0B',
                            green: '#10B981',
                            blue: '#3B82F6',
                            orange: '#F97316'
                        }
                    },
                    fontFamily: {
                        ar: ['Cairo', 'sans-serif'],
                        en: ['Fredoka', 'sans-serif']
                    }
                }
            }
        }
    </script>

    <style>
        body {
            font-family: 'Cairo', 'Fredoka', sans-serif;
            touch-action: manipulation;
            -webkit-tap-highlight-color: transparent;
            user-select: none;
        }
        .bouncy {
            animation: bounce 0.6s infinite alternate;
        }
        @keyframes bounce {
            from { transform: translateY(0); }
            to { transform: translateY(-8px); }
        }
        .pop-in {
            animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }
        @keyframes popIn {
            0% { transform: scale(0.8); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        .glass-card {
            background: rgba(255, 255, 255, 0.94);
            backdrop-filter: blur(10px);
            border: 4px solid rgba(255, 255, 255, 0.8);
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #F3F4F6;
            border-radius: 9999px;
        }
        ::-webkit-scrollbar-thumb {
            background: #A7F3D0;
            border-radius: 9999px;
        }
        /* Number line drawing styling */
        .number-line-container {
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: space-between;
            position: relative;
            padding: 25px 10px;
        }
        .number-line-bar {
            position: absolute;
            top: 50%;
            left: 4%;
            right: 4%;
            height: 6px;
            background-color: #7C3AED;
            border-radius: 4px;
            z-index: 1;
        }
        .number-line-point {
            position: relative;
            z-index: 2;
            width: 42px;
            height: 42px;
            border-radius: 50%;
            background: #ffffff;
            border: 3px solid #7C3AED;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 800;
            font-size: 1rem;
            color: #4C1D95;
            box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
        }
        .number-line-point.missing {
            background: #FDE047;
            border-color: #D97706;
            color: #92400E;
            font-size: 1.3rem;
            animation: pulse 1.5s infinite;
        }
    </style>
</head>
<body class="bg-gradient-to-br from-indigo-500 via-purple-500 to-pink-500 min-h-screen text-slate-800 flex flex-col justify-between overflow-x-hidden">

    <!-- Header / Navbar -->
    <header class="p-3 md:p-4 max-w-5xl mx-auto w-full flex justify-between items-center z-10">
        <div class="flex items-center space-x-2 space-x-reverse bg-white/90 px-3 py-1.5 rounded-full shadow-lg border-2 border-white">
            <div class="w-10 h-10 rounded-full bg-yellow-400 border-2 border-white flex items-center justify-center text-xl font-bold text-white shadow-inner" id="user-avatar">
                👦
            </div>
            <div>
                <span class="block text-[10px] font-bold text-gray-500" id="lbl-profile">الملف الشخصي</span>
                <span class="font-extrabold text-purple-700 text-xs md:text-sm leading-none" id="user-display-name">بطل الرياضيات</span>
            </div>
        </div>

        <!-- Language Switcher & Controls -->
        <div class="flex items-center gap-2">
            <button onclick="toggleAudio()" id="btn-audio" class="w-10 h-10 bg-white/90 rounded-full flex items-center justify-center text-purple-700 shadow-md border-2 border-white text-lg active:scale-95 transition">
                <i class="fa-solid fa-volume-high"></i>
            </button>
            <button onclick="toggleLanguage()" class="bg-yellow-400 hover:bg-yellow-300 text-purple-900 font-extrabold px-3 py-2 rounded-2xl shadow-md border-2 border-white text-xs md:text-sm active:scale-95 transition flex items-center gap-1">
                <i class="fa-solid fa-globe"></i>
                <span id="lang-btn-text">English</span>
            </button>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-1 max-w-5xl w-full mx-auto p-3 md:p-4 flex flex-col justify-center items-center">

        <!-- SCREEN 1: Profile Selection / Login -->
        <section id="screen-profile" class="w-full max-w-md glass-card rounded-3xl p-6 shadow-2xl text-center pop-in">
            <div class="text-5xl mb-2">🚀</div>
            <h1 class="text-2xl md:text-3xl font-black text-purple-700 mb-2" id="title-welcome">مرحباً يا بطل الرياضيات!</h1>
            <p class="text-gray-600 mb-6 text-sm font-bold" id="subtitle-welcome">أدخل اسمك وابدأ رحلة المغامرة والتعلم</p>

            <form onsubmit="handleProfileSubmit(event)" class="space-y-4">
                <div>
                    <input type="text" id="input-username" required maxlength="15" 
                           placeholder="اكتب اسمك هنا..." 
                           class="w-full text-center px-4 py-3 text-lg rounded-2xl border-4 border-purple-300 focus:border-purple-600 outline-none font-bold text-purple-900 bg-purple-50 shadow-inner">
                </div>
                <!-- Avatar selection -->
                <div class="flex justify-center gap-3 py-2">
                    <button type="button" onclick="selectAvatar('👦')" class="avatar-opt text-3xl p-2 rounded-2xl border-2 border-transparent bg-slate-100 hover:bg-purple-100 active:scale-95">👦</button>
                    <button type="button" onclick="selectAvatar('👧')" class="avatar-opt text-3xl p-2 rounded-2xl border-2 border-transparent bg-slate-100 hover:bg-purple-100 active:scale-95">👧</button>
                    <button type="button" onclick="selectAvatar('🦁')" class="avatar-opt text-3xl p-2 rounded-2xl border-2 border-transparent bg-slate-100 hover:bg-purple-100 active:scale-95">🦁</button>
                    <button type="button" onclick="selectAvatar('🤖')" class="avatar-opt text-3xl p-2 rounded-2xl border-2 border-transparent bg-slate-100 hover:bg-purple-100 active:scale-95">🤖</button>
                    <button type="button" onclick="selectAvatar('🦄')" class="avatar-opt text-3xl p-2 rounded-2xl border-2 border-transparent bg-slate-100 hover:bg-purple-100 active:scale-95">🦄</button>
                </div>
                <button type="submit" class="w-full py-4 bg-gradient-to-r from-green-400 to-emerald-500 hover:from-green-500 hover:to-emerald-600 text-white font-black text-xl rounded-2xl shadow-lg border-b-4 border-emerald-700 active:translate-y-1 transition" id="btn-start">
                    ابدأ اللعب الآن! 🎮
                </button>
            </form>
        </section>

        <!-- SCREEN 2: Levels Map (6 Main Operations) -->
        <section id="screen-levels" class="w-full hidden space-y-6 pop-in">
            <!-- Progress Overview Bar -->
            <div class="glass-card rounded-3xl p-4 shadow-xl flex flex-wrap justify-between items-center gap-3">
                <div class="flex items-center gap-3">
                    <div class="text-3xl text-yellow-500"><i class="fa-solid fa-trophy"></i></div>
                    <div>
                        <div class="text-xs text-gray-500 font-bold" id="lbl-total-stars">إجمالي النجوم</div>
                        <div class="text-xl font-black text-purple-700" id="stat-total-stars">0 ⭐</div>
                    </div>
                </div>
                <div class="flex items-center gap-3">
                    <div class="text-3xl text-pink-500"><i class="fa-solid fa-medal"></i></div>
                    <div>
                        <div class="text-xs text-gray-500 font-bold" id="lbl-badges-earned">الأوسمة المكتسبة</div>
                        <div class="text-xl font-black text-purple-700" id="stat-badges-earned">0 / 6 🏆</div>
                    </div>
                </div>
                <button onclick="showProfileModal()" class="px-3 py-1.5 bg-purple-100 text-purple-700 font-bold text-xs rounded-xl hover:bg-purple-200 transition">
                    <i class="fa-solid fa-user-gear"></i> <span id="btn-edit-profile">تغيير الحساب</span>
                </button>
            </div>

            <!-- Level Cards Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="levels-grid">
                <!-- Levels injected via JS -->
            </div>
        </section>

        <!-- SCREEN 3: Number Ranges Grid (1-20, 20-40, ..., 480-500) -->
        <section id="screen-ranges" class="w-full hidden space-y-4 pop-in">
            <div class="glass-card rounded-3xl p-4 flex justify-between items-center shadow-lg">
                <button onclick="navigateTo('levels')" class="w-10 h-10 bg-white rounded-full flex items-center justify-center text-purple-700 shadow border-2 border-purple-200 font-bold text-lg hover:bg-purple-50">
                    <i class="fa-solid fa-arrow-right dir-reflect"></i>
                </button>
                <h2 class="text-lg md:text-xl font-black text-purple-800" id="ranges-title">اختر مجموعة الأرقام</h2>
                <div class="w-10"></div>
            </div>

            <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-3 max-h-[65vh] overflow-y-auto p-1" id="ranges-grid">
                <!-- Number ranges injected via JS -->
            </div>
        </section>

        <!-- SCREEN 4: Exercises List (50 Exercises per Range) -->
        <section id="screen-exercises" class="w-full hidden space-y-4 pop-in">
            <div class="glass-card rounded-3xl p-4 flex justify-between items-center shadow-lg">
                <button onclick="navigateTo('ranges')" class="w-10 h-10 bg-white rounded-full flex items-center justify-center text-purple-700 shadow border-2 border-purple-200 font-bold text-lg hover:bg-purple-50">
                    <i class="fa-solid fa-arrow-right dir-reflect"></i>
                </button>
                <div class="text-center">
                    <h2 class="text-base md:text-lg font-black text-purple-800" id="exercises-title">التمارين</h2>
                    <span class="text-xs font-bold text-purple-600" id="exercises-range-subtitle">المجموعة 1 - 20</span>
                </div>
                <div class="w-10"></div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-5 gap-3 max-h-[65vh] overflow-y-auto p-1" id="exercises-grid">
                <!-- 50 Exercises injected via JS -->
            </div>
        </section>

        <!-- SCREEN 5: Game Play Room (10 Questions per Exercise + TTS Reader) -->
        <section id="screen-game" class="w-full hidden max-w-xl glass-card rounded-3xl p-4 md:p-6 shadow-2xl relative pop-in">
            <!-- Game Top Bar -->
            <div class="flex justify-between items-center mb-3 pb-2 border-b-2 border-purple-100">
                <button onclick="confirmExitGame()" class="text-gray-400 hover:text-red-500 text-2xl">
                    <i class="fa-solid fa-circle-xmark"></i>
                </button>
                <div class="flex items-center gap-2">
                    <span class="text-xs font-bold text-gray-500" id="lbl-question-count">السؤال:</span>
                    <span class="text-lg font-black text-purple-700" id="q-counter">1/10</span>
                </div>
                <div class="flex items-center gap-1 bg-yellow-100 px-3 py-1 rounded-full text-yellow-700 font-bold text-sm">
                    <i class="fa-solid fa-star text-yellow-500"></i>
                    <span id="game-score">0</span>
                </div>
            </div>

            <!-- Progress Bar -->
            <div class="w-full bg-slate-200 rounded-full h-3 mb-4 overflow-hidden">
                <div id="game-progress-bar" class="bg-gradient-to-r from-purple-500 to-pink-500 h-3 rounded-full transition-all duration-300" style="width: 10%"></div>
            </div>

            <!-- Visual Helper / Question Display Area -->
            <div class="bg-purple-50 border-2 border-purple-200 rounded-2xl p-4 mb-4 min-h-[140px] flex flex-col justify-center items-center shadow-inner relative" id="game-visual-area">
                <!-- Dynamic Visual Aids / Number Lines -->
            </div>

            <!-- Question Text Header with TTS Audio Reader Button -->
            <div class="flex items-center justify-center gap-3 mb-6 bg-purple-100/70 p-3 rounded-2xl border border-purple-200">
                <h3 class="text-lg md:text-2xl font-black text-purple-950 dir-ltr-auto" id="q-text">
                    ...
                </h3>
                <button onclick="speakQuestionText()" id="btn-speak-q" class="w-11 h-11 bg-yellow-400 hover:bg-yellow-300 text-purple-900 rounded-full flex items-center justify-center text-xl shadow-md border-2 border-white active:scale-95 transition flex-shrink-0" title="اقرأ السؤال">
                    <i class="fa-solid fa-volume-high"></i>
                </button>
            </div>

            <!-- Options Grid -->
            <div class="grid grid-cols-2 gap-3" id="game-options-grid">
                <!-- Options injected via JS -->
            </div>
        </section>

        <!-- SCREEN 6: Result / Summary Screen -->
        <section id="screen-result" class="w-full max-w-md hidden glass-card rounded-3xl p-6 shadow-2xl text-center pop-in">
            <div id="result-icon" class="text-6xl mb-3 bouncy">🏆</div>
            <h2 class="text-2xl md:text-3xl font-black text-purple-800 mb-1" id="result-title">عمل رائع!</h2>
            <p class="text-gray-600 text-sm mb-4 font-bold" id="result-subtitle">لقد أكملت التمرين بنجاح</p>

            <!-- Stars Awarded -->
            <div class="flex justify-center gap-2 text-4xl mb-6" id="result-stars">
                <i class="fa-solid fa-star text-slate-300"></i>
                <i class="fa-solid fa-star text-slate-300"></i>
                <i class="fa-solid fa-star text-slate-300"></i>
            </div>

            <!-- Score Box -->
            <div class="bg-purple-50 border-2 border-purple-200 rounded-2xl p-4 mb-6">
                <div class="text-xs text-gray-500 font-bold mb-1" id="lbl-your-score">النتيجة النهائية</div>
                <div class="text-3xl font-black text-purple-700" id="result-score-percentage">90%</div>
                <div class="text-xs text-purple-600 font-bold mt-1" id="result-correct-count">9 من 10 إجابات صحيحة</div>
            </div>

            <!-- Badge Unlocked Modal -->
            <div id="badge-earned-box" class="hidden bg-yellow-50 border-2 border-yellow-300 rounded-2xl p-3 mb-6 animate-pulse flex items-center gap-3">
                <div class="text-3xl" id="badge-earned-icon">🎖️</div>
                <div class="text-right">
                    <div class="text-xs font-bold text-yellow-800" id="lbl-badge-unlocked">وسام جديد!</div>
                    <div class="text-sm font-black text-yellow-900" id="badge-earned-name">بطل الرياضيات</div>
                </div>
            </div>

            <!-- Action Buttons -->
            <div class="flex gap-3">
                <button onclick="retryCurrentExercise()" class="flex-1 py-3 bg-slate-200 hover:bg-slate-300 text-slate-700 font-extrabold rounded-2xl text-sm transition">
                    <i class="fa-solid fa-rotate-right"></i> <span id="btn-retry">إعادة</span>
                </button>
                <button onclick="navigateTo('exercises')" class="flex-1 py-3 bg-purple-600 hover:bg-purple-700 text-white font-extrabold rounded-2xl text-sm shadow-md transition">
                    <i class="fa-solid fa-list"></i> <span id="btn-continue">التالي</span>
                </button>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="p-3 text-center text-xs font-bold text-white/80">
        <span id="footer-text">تطبيق رياضيات الصف الأول الإبتدائي - تفاعلي وصوتي 🌟</span>
    </footer>

    <!-- JS Logic -->
    <script>
        // --- TRANSLATIONS DICTIONARY ---
        const translations = {
            ar: {
                profile: "الملف الشخصي",
                defaultUser: "بطل الرياضيات",
                welcomeTitle: "مرحباً يا بطل الرياضيات! 🚀",
                welcomeSub: "أدخل اسمك وابدأ رحلة المغامرة والتعلم",
                inputPlaceholder: "اكتب اسمك هنا...",
                startBtn: "ابدأ اللعب الآن! 🎮",
                totalStars: "إجمالي النجوم",
                badgesEarned: "الأوسمة المكتسبة",
                editProfile: "تغيير الاسم",
                rangesTitle: "اختر مجموعة المدى الأرقامي",
                exercisesTitle: "تمارين المجموعة",
                questionCount: "السؤال:",
                greatJob: "عمل ممتاز يا بطل! 🎉",
                tryAgain: "حاول مرة أخرى لتكسب النجوم! 💪",
                finalScore: "النتيجة النهائية",
                correctAnswers: "من 10 إجابات صحيحة",
                badgeUnlocked: "وسام جديد متاح!",
                retry: "إعادة التمرين",
                continue: "التالي",
                footer: "تطبيق رياضيات الصف الأول الإبتدائي - تفاعلي وصوتي 🌟",
                confirmExit: "هل أنت تأكد من الخروج؟ سيفقد تقدمك في هذا التمرين الحالي.",
                levels: {
                    1: { name: "عالم الأرقام", desc: "قبل وبعد وبين على خط الأعداد" },
                    2: { name: "مملكة الجمع", desc: "جمع الأرقام تدريجياً حتى 500" },
                    3: { name: "غابة الطرح", desc: "طرح الأرقام وحساب الباقي" },
                    4: { name: "قلعة الضرب", desc: "مفهوم الضرب والمجموعات" },
                    5: { name: "جزيرة القسمة", desc: "مشاركة العناصر والتوزيع بالتساوي" },
                    6: { name: "مقارنة الأعداد", desc: "علامات أكبر (>), أصغر (<), يساوي (=)" }
                },
                badges: {
                    1: "عبقري الأرقام 🔢",
                    2: "ملك الجمع ➕",
                    3: "فارس الطرح ➖",
                    4: "سيد الضرب ✖️",
                    5: "أمير القسمة ➗",
                    6: "بطل المقارنة ⚖️"
                }
            },
            en: {
                profile: "Profile",
                defaultUser: "Math Hero",
                welcomeTitle: "Welcome Math Hero! 🚀",
                welcomeSub: "Enter your name to start learning adventure",
                inputPlaceholder: "Enter your name...",
                startBtn: "Start Playing Now! 🎮",
                totalStars: "Total Stars",
                badgesEarned: "Badges Earned",
                editProfile: "Change Profile",
                rangesTitle: "Select Number Range",
                exercisesTitle: "Group Exercises",
                questionCount: "Question:",
                greatJob: "Awesome Job! 🎉",
                tryAgain: "Try again to earn stars! 💪",
                finalScore: "Final Score",
                correctAnswers: "out of 10 correct answers",
                badgeUnlocked: "New Badge Unlocked!",
                retry: "Retry",
                continue: "Next",
                footer: "Grade 1 Math Interactive & Speech App 🌟",
                confirmExit: "Are you sure you want to exit? Your current progress will be lost.",
                levels: {
                    1: { name: "Numbers World", desc: "Before, after, and in-between on line" },
                    2: { name: "Addition Kingdom", desc: "Step-by-step addition up to 500" },
                    3: { name: "Subtraction Forest", desc: "Subtraction & finding remainder" },
                    4: { name: "Multiplication Castle", desc: "Multiplication concepts & groups" },
                    5: { name: "Division Island", desc: "Fair sharing and division" },
                    6: { name: "Number Comparison", desc: "Greater (>), Less (<), Equal (=)" }
                },
                badges: {
                    1: "Number Genius 🔢",
                    2: "Addition King ➕",
                    3: "Subtraction Knight ➖",
                    4: "Multiplication Master ✖️",
                    5: "Division Prince ➗",
                    6: "Comparison Hero ⚖️"
                }
            }
        };

        // --- APP STATE ---
        let currentLang = 'ar';
        let isAudioMuted = false;
        let userData = {
            name: "بطل الرياضيات",
            avatar: "👦",
            scores: {}, // Key: L{lvl}_R{rangeIdx}_E{exId} -> score
            stars: {}   // Key: L{lvl}_R{rangeIdx}_E{exId} -> stars count
        };

        let currentLevel = 1;
        let currentRangeIndex = 0; // 0 for 1-20, 1 for 20-40, etc.
        let currentExercise = 1;
        let currentQuestions = [];
        let currentQuestionIdx = 0;
        let userCorrectCount = 0;

        // Generate Ranges 1-20, 20-40, 40-60, ... 480-500 (25 ranges)
        const NUMBER_RANGES = [];
        for (let r = 0; r < 25; r++) {
            const min = r === 0 ? 1 : r * 20;
            const max = (r + 1) * 20;
            NUMBER_RANGES.push({ min, max, label: `${min} - ${max}` });
        }

        // --- SPEECH SYNTHESIS ENGINE (TEXT-TO-SPEECH) ---
        function speakText(text) {
            if (isAudioMuted || !('speechSynthesis' in window)) return;

            window.speechSynthesis.cancel(); // Stop ongoing speech

            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = currentLang === 'ar' ? 'ar-SA' : 'en-US';
            utterance.rate = 0.8; // Slower speed suitable for young children
            utterance.pitch = 1.1; // Friendly pitch

            window.speechSynthesis.speak(utterance);
        }

        function speakQuestionText() {
            const currentQ = currentQuestions[currentQuestionIdx];
            if (currentQ) {
                speakText(currentQ.text);
            }
        }

        // --- SOUND EFFECTS SYNTHESIZER ---
        const AudioFX = {
            ctx: null,
            init() {
                if (!this.ctx) {
                    this.ctx = new (window.AudioContext || window.webkitAudioContext)();
                }
            },
            playCorrect() {
                if (isAudioMuted) return;
                this.init();
                const now = this.ctx.currentTime;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(523.25, now);
                osc.frequency.exponentialRampToValueAtTime(659.25, now + 0.1);
                osc.frequency.exponentialRampToValueAtTime(783.99, now + 0.2);
                gain.gain.setValueAtTime(0.3, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.3);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start(now);
                osc.stop(now + 0.3);
            },
            playWrong() {
                if (isAudioMuted) return;
                this.init();
                const now = this.ctx.currentTime;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(220, now);
                osc.frequency.setValueAtTime(180, now + 0.1);
                gain.gain.setValueAtTime(0.3, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.3);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start(now);
                osc.stop(now + 0.3);
            },
            playWin() {
                if (isAudioMuted) return;
                this.init();
                const notes = [523.25, 659.25, 783.99, 1046.50];
                notes.forEach((freq, i) => {
                    const osc = this.ctx.createOscillator();
                    const gain = this.ctx.createGain();
                    osc.frequency.value = freq;
                    gain.gain.setValueAtTime(0.2, this.ctx.currentTime + i * 0.1);
                    gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + i * 0.1 + 0.2);
                    osc.connect(gain);
                    gain.connect(this.ctx.destination);
                    osc.start(this.ctx.currentTime + i * 0.1);
                    osc.stop(this.ctx.currentTime + i * 0.1 + 0.25);
                });
            }
        };

        // --- INIT & LOCAL STORAGE ---
        window.onload = function() {
            loadUserData();
            updateUIStrings();
            renderLevels();
            
            if (localStorage.getItem('math_app_user_created')) {
                navigateTo('levels');
            } else {
                navigateTo('profile');
            }
        };

        function saveUserData() {
            localStorage.setItem('math_app_user_data', JSON.stringify(userData));
            localStorage.setItem('math_app_user_created', 'true');
        }

        function loadUserData() {
            const stored = localStorage.getItem('math_app_user_data');
            if (stored) {
                try {
                    userData = JSON.parse(stored);
                } catch(e) {}
            }
            updateHeaderProfile();
        }

        function updateHeaderProfile() {
            document.getElementById('user-avatar').innerText = userData.avatar || '👦';
            document.getElementById('user-display-name').innerText = userData.name || translations[currentLang].defaultUser;
        }

        // --- NAVIGATION ---
        function navigateTo(screenName) {
            const screens = ['profile', 'levels', 'ranges', 'exercises', 'game', 'result'];
            screens.forEach(s => {
                document.getElementById(`screen-${s}`).classList.add('hidden');
            });
            document.getElementById(`screen-${screenName}`).classList.remove('hidden');

            if (screenName === 'levels') {
                updateLevelsProgressStats();
                renderLevels();
            }
        }

        function toggleLanguage() {
            currentLang = currentLang === 'ar' ? 'en' : 'ar';
            document.documentElement.lang = currentLang;
            document.documentElement.dir = currentLang === 'ar' ? 'rtl' : 'ltr';
            document.getElementById('lang-btn-text').innerText = currentLang === 'ar' ? 'English' : 'العربية';
            
            updateUIStrings();
            renderLevels();
        }

        function toggleAudio() {
            isAudioMuted = !isAudioMuted;
            const btn = document.getElementById('btn-audio');
            btn.innerHTML = isAudioMuted 
                ? '<i class="fa-solid fa-volume-xmark text-red-500"></i>' 
                : '<i class="fa-solid fa-volume-high"></i>';
        }

        function updateUIStrings() {
            const t = translations[currentLang];
            document.getElementById('lbl-profile').innerText = t.profile;
            document.getElementById('title-welcome').innerText = t.welcomeTitle;
            document.getElementById('subtitle-welcome').innerText = t.welcomeSub;
            document.getElementById('input-username').placeholder = t.inputPlaceholder;
            document.getElementById('btn-start').innerText = t.startBtn;
            document.getElementById('lbl-total-stars').innerText = t.totalStars;
            document.getElementById('lbl-badges-earned').innerText = t.badgesEarned;
            document.getElementById('btn-edit-profile').innerText = t.editProfile;
            document.getElementById('ranges-title').innerText = t.rangesTitle;
            document.getElementById('exercises-title').innerText = t.exercisesTitle;
            document.getElementById('lbl-question-count').innerText = t.questionCount;
            document.getElementById('lbl-your-score').innerText = t.finalScore;
            document.getElementById('lbl-badge-unlocked').innerText = t.badgeUnlocked;
            document.getElementById('btn-retry').innerText = t.retry;
            document.getElementById('btn-continue').innerText = t.continue;
            document.getElementById('footer-text').innerText = t.footer;

            updateHeaderProfile();
        }

        function selectAvatar(emoji) {
            userData.avatar = emoji;
            document.querySelectorAll('.avatar-opt').forEach(el => {
                el.classList.remove('border-purple-600', 'bg-purple-200');
            });
            event.currentTarget.classList.add('border-purple-600', 'bg-purple-200');
        }

        function handleProfileSubmit(e) {
            e.preventDefault();
            const nameInput = document.getElementById('input-username').value.trim();
            if (nameInput) {
                userData.name = nameInput;
            }
            saveUserData();
            updateHeaderProfile();
            navigateTo('levels');
        }

        function showProfileModal() {
            navigateTo('profile');
        }

        // --- UNLOCK & PROGRESS LOGIC ---
        function calculateTotalStars() {
            let stars = 0;
            Object.values(userData.stars || {}).forEach(s => stars += s);
            return stars;
        }

        function updateLevelsProgressStats() {
            document.getElementById('stat-total-stars').innerText = `${calculateTotalStars()} ⭐`;
            document.getElementById('stat-badges-earned').innerText = `${calculateBadgeCount()} / 6 🏆`;
        }

        function calculateBadgeCount() {
            let badges = 0;
            for (let l = 1; l <= 6; l++) {
                // Check if user played exercises in this level
                let total = 0;
                let count = 0;
                for (let r = 0; r < 5; r++) {
                    for (let e = 1; e <= 5; e++) {
                        const s = userData.scores[`L${l}_R${r}_E${e}`] || 0;
                        if (s > 0) { total += s; count++; }
                    }
                }
                if (count > 0 && (total / count) >= 90) badges++;
            }
            return badges;
        }

        // --- RENDER LEVELS (6 CATEGORIES) ---
        function renderLevels() {
            const grid = document.getElementById('levels-grid');
            grid.innerHTML = '';

            const levelIcons = ['1️⃣', '➕', '➖', '✖️', '➗', '⚖️'];
            const levelColors = [
                'from-blue-400 to-indigo-500',
                'from-emerald-400 to-teal-600',
                'from-orange-400 to-amber-600',
                'from-purple-400 to-pink-600',
                'from-rose-400 to-red-600',
                'from-cyan-400 to-blue-600'
            ];

            for (let l = 1; l <= 6; l++) {
                const lInfo = translations[currentLang].levels[l];
                const badgeName = translations[currentLang].badges[l];

                const card = document.createElement('div');
                card.className = `glass-card rounded-3xl p-5 shadow-lg border-4 transition transform hover:scale-[1.02] cursor-pointer relative overflow-hidden flex flex-col justify-between`;

                card.onclick = () => {
                    openLevelRanges(l);
                };

                card.innerHTML = `
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <div class="w-14 h-14 rounded-2xl bg-gradient-to-tr ${levelColors[l-1]} text-white flex items-center justify-center text-3xl font-black shadow-md">
                                ${levelIcons[l-1]}
                            </div>
                            <span class="bg-purple-100 text-purple-700 font-extrabold text-xs px-3 py-1 rounded-full border border-purple-300">
                                1 - 500
                            </span>
                        </div>
                        <h3 class="text-xl font-black text-purple-900 mb-1">${lInfo.name}</h3>
                        <p class="text-xs font-bold text-gray-500 mb-4">${lInfo.desc}</p>
                    </div>
                    
                    <div class="flex items-center justify-between pt-2 border-t border-purple-100 text-xs">
                        <span class="font-bold text-gray-600">${badgeName}</span>
                        <span class="text-xl">🏆</span>
                    </div>
                `;

                grid.appendChild(card);
            }
        }

        // --- RENDER NUMBER RANGES (1-20, 20-40, ... 480-500) ---
        function openLevelRanges(levelId) {
            currentLevel = levelId;
            renderRanges();
            navigateTo('ranges');
        }

        function renderRanges() {
            const grid = document.getElementById('ranges-grid');
            grid.innerHTML = '';

            NUMBER_RANGES.forEach((range, idx) => {
                const btn = document.createElement('button');
                btn.className = `glass-card rounded-2xl p-4 shadow border-2 hover:border-purple-500 hover:scale-105 active:scale-95 transition flex flex-col items-center justify-center`;

                btn.onclick = () => {
                    openRangeExercises(idx);
                };

                btn.innerHTML = `
                    <div class="text-xs font-bold text-gray-400 mb-1">${currentLang === 'ar' ? 'المجموعة' : 'Group'} ${idx + 1}</div>
                    <div class="text-lg font-black text-purple-800">${range.label}</div>
                    <div class="text-[10px] text-emerald-600 font-bold mt-1">50 ${currentLang === 'ar' ? 'تمرين' : 'Exercises'}</div>
                `;

                grid.appendChild(btn);
            });
        }

        // --- RENDER 50 EXERCISES PER RANGE ---
        function openRangeExercises(rangeIdx) {
            currentRangeIndex = rangeIdx;
            renderExercises();
            navigateTo('exercises');
        }

        function renderExercises() {
            const grid = document.getElementById('exercises-grid');
            grid.innerHTML = '';

            const rangeObj = NUMBER_RANGES[currentRangeIndex];
            document.getElementById('exercises-range-subtitle').innerText = `${currentLang === 'ar' ? 'المجموعة' : 'Range'}: (${rangeObj.label})`;

            // Generate 50 Exercises
            for (let e = 1; e <= 50; e++) {
                const key = `L${currentLevel}_R${currentRangeIndex}_E${e}`;
                const score = userData.scores[key] || 0;
                const starsCount = userData.stars[key] || 0;

                const card = document.createElement('div');
                card.className = `glass-card rounded-2xl p-3 shadow border-2 transition hover:border-purple-500 hover:shadow-md cursor-pointer flex flex-col justify-between`;

                card.onclick = () => {
                    startExercise(currentLevel, currentRangeIndex, e);
                };

                let starsHTML = '';
                for (let s = 1; s <= 3; s++) {
                    if (s <= starsCount) {
                        starsHTML += '<i class="fa-solid fa-star text-yellow-400 text-xs"></i>';
                    } else {
                        starsHTML += '<i class="fa-solid fa-star text-slate-300 text-xs"></i>';
                    }
                }

                card.innerHTML = `
                    <div class="flex justify-between items-center mb-1">
                        <span class="font-black text-xs text-purple-700 bg-purple-100 px-2 py-0.5 rounded-full">
                            #${e}
                        </span>
                        <span class="font-bold text-[10px] text-gray-500">${score}%</span>
                    </div>
                    <div class="flex justify-between items-center mt-2 pt-1 border-t border-purple-100">
                        <div class="flex gap-0.5">${starsHTML}</div>
                        <i class="fa-solid fa-circle-play text-purple-600 text-base"></i>
                    </div>
                `;

                grid.appendChild(card);
            }
        }

        // --- QUESTION GENERATOR ENGINE (10 QUESTIONS PER EXERCISE, NO REPETITION IN ROUND) ---
        function generateQuestionsForExercise(levelId, rangeIdx, exId) {
            const questions = [];
            const usedKeys = new Set();
            const range = NUMBER_RANGES[rangeIdx];

            for (let q = 0; q < 10; q++) {
                let questionObj = {};
                let attempts = 0;

                while (attempts < 100) {
                    attempts++;
                    let key = "";

                    if (levelId === 1) { // NUMBERS & NUMBER LINE
                        const type = q % 3; // 0: Next (بعد), 1: Previous (قبل), 2: Between (بين)
                        const min = Math.max(range.min, 2);
                        const max = Math.min(range.max - 2, 498);
                        const num = Math.floor(Math.random() * (max - min + 1)) + min;

                        if (type === 0) { // AFTER
                            const ans = num + 1;
                            key = `L1_AFTER_${num}`;
                            if (!usedKeys.has(key)) {
                                usedKeys.add(key);
                                questionObj = {
                                    text: currentLang === 'ar' ? `ما هو العدد الذي يأتِي بَعْد العدد (${num})؟` : `What number comes AFTER (${num})?`,
                                    visual: { type: 'numberline', start: num - 1, missing: ans, end: num + 3 },
                                    correct: ans,
                                    options: generateOptions(ans, range.min, range.max)
                                };
                                break;
                            }
                        } else if (type === 1) { // BEFORE
                            const ans = num - 1;
                            key = `L1_BEFORE_${num}`;
                            if (!usedKeys.has(key)) {
                                usedKeys.add(key);
                                questionObj = {
                                    text: currentLang === 'ar' ? `ما هو العدد الذي يأتِي قَبْل العدد (${num})؟` : `What number comes BEFORE (${num})?`,
                                    visual: { type: 'numberline', start: ans - 2, missing: ans, end: ans + 2 },
                                    correct: ans,
                                    options: generateOptions(ans, range.min, range.max)
                                };
                                break;
                            }
                        } else { // BETWEEN
                            const ans = num + 1;
                            key = `L1_BETWEEN_${num}`;
                            if (!usedKeys.has(key)) {
                                usedKeys.add(key);
                                questionObj = {
                                    text: currentLang === 'ar' ? `ما هو العدد الذي يقع بَيْن (${num}) و (${num + 2})؟` : `What number is BETWEEN (${num}) and (${num + 2})?`,
                                    visual: { type: 'numberline', start: num - 1, missing: ans, end: num + 3 },
                                    correct: ans,
                                    options: generateOptions(ans, range.min, range.max)
                                };
                                break;
                            }
                        }
                    }
                    else if (levelId === 2) { // ADDITION
                        const a = Math.floor(Math.random() * (range.max / 2 - range.min / 2 + 1)) + Math.floor(range.min / 2);
                        const b = Math.floor(Math.random() * (range.max / 2 - range.min / 2 + 1)) + Math.floor(range.min / 2);
                        const ans = a + b;
                        key = `L2_${a}_${b}`;

                        if (!usedKeys.has(key)) {
                            usedKeys.add(key);
                            questionObj = {
                                text: `${a} + ${b} = ?`,
                                visual: null,
                                correct: ans,
                                options: generateOptions(ans, range.min, range.max)
                            };
                            break;
                        }
                    }
                    else if (levelId === 3) { // SUBTRACTION
                        const a = Math.floor(Math.random() * (range.max - range.min + 1)) + range.min;
                        const b = Math.floor(Math.random() * (a - Math.floor(a/2) + 1));
                        const ans = a - b;
                        key = `L3_${a}_${b}`;

                        if (!usedKeys.has(key)) {
                            usedKeys.add(key);
                            questionObj = {
                                text: `${a} - ${b} = ?`,
                                visual: null,
                                correct: ans,
                                options: generateOptions(ans, 0, range.max)
                            };
                            break;
                        }
                    }
                    else if (levelId === 4) { // MULTIPLICATION
                        const table = (q % 9) + 2; // 2 to 10
                        const mult = Math.floor(Math.random() * 10) + 1;
                        const ans = table * mult;
                        key = `L4_${table}_${mult}`;

                        if (!usedKeys.has(key)) {
                            usedKeys.add(key);
                            questionObj = {
                                text: `${mult} × ${table} = ?`,
                                visual: null,
                                correct: ans,
                                options: generateOptions(ans, 1, 100)
                            };
                            break;
                        }
                    }
                    else if (levelId === 5) { // DIVISION
                        const divisor = (q % 8) + 2; // 2 to 9
                        const quotient = Math.floor(Math.random() * 10) + 1;
                        const dividend = divisor * quotient;
                        key = `L5_${dividend}_${divisor}`;

                        if (!usedKeys.has(key)) {
                            usedKeys.add(key);
                            questionObj = {
                                text: `${dividend} ÷ ${divisor} = ?`,
                                visual: null,
                                correct: quotient,
                                options: generateOptions(quotient, 1, 20)
                            };
                            break;
                        }
                    }
                    else if (levelId === 6) { // GREATER, LESS, EQUAL ( > , < , = )
                        const a = Math.floor(Math.random() * (range.max - range.min + 1)) + range.min;
                        let b = a;
                        if (q % 3 !== 0) {
                            b = Math.floor(Math.random() * (range.max - range.min + 1)) + range.min;
                        }
                        
                        let ans = "=";
                        if (a > b) ans = ">";
                        if (a < b) ans = "<";

                        key = `L6_${a}_${b}`;

                        if (!usedKeys.has(key)) {
                            usedKeys.add(key);
                            questionObj = {
                                text: currentLang === 'ar' ? `اختر العلامة المناسبة: ${a} [ ? ] ${b}` : `Choose operator: ${a} [ ? ] ${b}`,
                                visual: { type: 'compare', valA: a, valB: b },
                                correct: ans,
                                options: ['>', '<', '=']
                            };
                            break;
                        }
                    }
                }

                questions.push(questionObj);
            }

            return questions;
        }

        function generateOptions(correct, min, max) {
            if (typeof correct === 'string') return ['>', '<', '=']; // For comparison level

            const opts = new Set([correct]);
            while (opts.size < 4) {
                let rand = correct + (Math.floor(Math.random() * 7) - 3);
                if (rand >= 0 && rand !== correct) {
                    opts.add(rand);
                } else {
                    opts.add(Math.floor(Math.random() * (max + 5)) + 1);
                }
            }
            return Array.from(opts).sort(() => Math.random() - 0.5);
        }

        // --- GAME PLAY ENGINE ---
        function startExercise(levelId, rangeIdx, exId) {
            currentLevel = levelId;
            currentRangeIndex = rangeIdx;
            currentExercise = exId;

            currentQuestions = generateQuestionsForExercise(levelId, rangeIdx, exId);
            currentQuestionIdx = 0;
            userCorrectCount = 0;

            navigateTo('game');
            renderQuestion();
        }

        function renderQuestion() {
            const q = currentQuestions[currentQuestionIdx];
            
            // Update Top Bar
            document.getElementById('q-counter').innerText = `${currentQuestionIdx + 1}/10`;
            document.getElementById('game-score').innerText = userCorrectCount;
            const progressPct = ((currentQuestionIdx + 1) / 10) * 100;
            document.getElementById('game-progress-bar').style.width = `${progressPct}%`;

            // Question Text
            document.getElementById('q-text').innerText = q.text;

            // Automatically read question text aloud
            speakQuestionText();

            // Render Visual Helper Area
            const visualArea = document.getElementById('game-visual-area');
            visualArea.innerHTML = '';

            if (q.visual) {
                if (q.visual.type === 'numberline') {
                    let lineHTML = '<div class="number-line-container">';
                    lineHTML += '<div class="number-line-bar"></div>';

                    for (let val = q.visual.start; val <= q.visual.end; val++) {
                        if (val === q.visual.missing) {
                            lineHTML += `<div class="number-line-point missing">?</div>`;
                        } else {
                            lineHTML += `<div class="number-line-point">${val}</div>`;
                        }
                    }

                    lineHTML += '</div>';
                    visualArea.innerHTML = lineHTML;
                }
                else if (q.visual.type === 'compare') {
                    visualArea.innerHTML = `
                        <div class="flex items-center gap-6 text-3xl font-black text-purple-900">
                            <span class="px-4 py-2 bg-purple-200 rounded-2xl shadow-inner">${q.visual.valA}</span>
                            <span class="text-yellow-600 font-extrabold text-4xl">?</span>
                            <span class="px-4 py-2 bg-purple-200 rounded-2xl shadow-inner">${q.visual.valB}</span>
                        </div>
                    `;
                }
            } else {
                visualArea.innerHTML = '<div class="text-5xl bouncy">🧠⚡</div>';
            }

            // Render Options
            const optsGrid = document.getElementById('game-options-grid');
            optsGrid.innerHTML = '';

            const isSymbolMode = currentLevel === 6;
            optsGrid.className = isSymbolMode ? "grid grid-cols-3 gap-3" : "grid grid-cols-2 gap-3";

            q.options.forEach(optVal => {
                const btn = document.createElement('button');
                btn.className = 'py-4 bg-white hover:bg-purple-50 text-purple-900 border-4 border-purple-200 hover:border-purple-500 font-black text-2xl md:text-3xl rounded-2xl shadow-md active:scale-95 transition flex items-center justify-center';
                btn.innerText = optVal;
                
                btn.onclick = () => {
                    // Speak option value upon click
                    if (isSymbolMode) {
                        const symbolSpeech = optVal === '>' ? (currentLang === 'ar' ? 'أكبر من' : 'Greater than') : (optVal === '<' ? (currentLang === 'ar' ? 'أصغر من' : 'Less than') : (currentLang === 'ar' ? 'يساوي' : 'Equal'));
                        speakText(symbolSpeech);
                    } else {
                        speakText(optVal.toString());
                    }
                    handleAnswerSelect(optVal, btn);
                };
                
                optsGrid.appendChild(btn);
            });
        }

        function handleAnswerSelect(selectedVal, btnElement) {
            const allBtns = document.querySelectorAll('#game-options-grid button');
            allBtns.forEach(b => b.disabled = true);

            const currentQ = currentQuestions[currentQuestionIdx];
            const isCorrect = selectedVal.toString() === currentQ.correct.toString();

            if (isCorrect) {
                btnElement.classList.remove('bg-white', 'border-purple-200');
                btnElement.classList.add('bg-green-500', 'text-white', 'border-green-600');
                userCorrectCount++;
                AudioFX.playCorrect();
            } else {
                btnElement.classList.remove('bg-white', 'border-purple-200');
                btnElement.classList.add('bg-red-500', 'text-white', 'border-red-600');
                AudioFX.playWrong();

                allBtns.forEach(b => {
                    if (b.innerText.trim() === currentQ.correct.toString()) {
                        b.classList.add('bg-green-100', 'border-green-400');
                    }
                });
            }

            setTimeout(() => {
                currentQuestionIdx++;
                if (currentQuestionIdx < 10) {
                    renderQuestion();
                } else {
                    finishExercise();
                }
            }, 1200);
        }

        function confirmExitGame() {
            if (confirm(translations[currentLang].confirmExit)) {
                navigateTo('exercises');
            }
        }

        // --- RESULTS & STAR LOGIC ---
        function finishExercise() {
            const scorePercentage = Math.round((userCorrectCount / 10) * 100);
            
            let starsEarned = 0;
            if (scorePercentage >= 100) {
                starsEarned = 3;
            } else if (scorePercentage >= 90) {
                starsEarned = 2;
            } else if (scorePercentage >= 80) {
                starsEarned = 1;
            }

            // Save Progress Key: L{lvl}_R{range}_E{ex}
            const key = `L${currentLevel}_R${currentRangeIndex}_E${currentExercise}`;
            const previousScore = userData.scores[key] || 0;
            if (scorePercentage > previousScore) {
                userData.scores[key] = scorePercentage;
            }
            const previousStars = userData.stars[key] || 0;
            if (starsEarned > previousStars) {
                userData.stars[key] = starsEarned;
            }
            saveUserData();

            // Populate Result Screen UI
            document.getElementById('result-score-percentage').innerText = `${scorePercentage}%`;
            document.getElementById('result-correct-count').innerText = `${userCorrectCount} ${translations[currentLang].correctAnswers}`;

            // Render Stars
            const starsContainer = document.getElementById('result-stars');
            let starsHTML = '';
            for (let s = 1; s <= 3; s++) {
                if (s <= starsEarned) {
                    starsHTML += '<i class="fa-solid fa-star text-yellow-400 bouncy" style="animation-delay: '+(s*0.15)+'s"></i>';
                } else {
                    starsHTML += '<i class="fa-solid fa-star text-slate-300"></i>';
                }
            }
            starsContainer.innerHTML = starsHTML;

            // Title message & sound
            if (scorePercentage >= 80) {
                document.getElementById('result-title').innerText = translations[currentLang].greatJob;
                AudioFX.playWin();
                confetti({ particleCount: 100, spread: 70, origin: { y: 0.6 } });
            } else {
                document.getElementById('result-title').innerText = translations[currentLang].tryAgain;
            }

            navigateTo('result');
        }

        function retryCurrentExercise() {
            startExercise(currentLevel, currentRangeIndex, currentExercise);
        }
    </script>
</body>
</html>
