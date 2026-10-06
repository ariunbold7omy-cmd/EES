<!DOCTYPE html>
<html lang="mn" class="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ЭЕШ МАТЕМАТИК — 60 ДААЛГАВАР (БЭЛТГЭЛ ПЛАТФОРМ)</title>
    
    <!-- Tailwind CSS with Dark Mode -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#eff6ff',
                            100: '#dbeafe',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                        }
                    }
                }
            }
        }
    </script>
    
    <!-- FontAwesome icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- MathJax for rendering LaTeX formulas -->
    <script>
        window.MathJax = {
            tex: {
                inlineMath: [['$', '$'], ['\\(', '\\)']],
                displayMath: [['$$', '$$'], ['\\[', '\\]']]
            },
            svg: { fontCache: 'global' }
        };
    </script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            transition: background-color 0.3s ease, color 0.3s ease;
        }
        .option-card {
            transition: all 0.15s ease-in-out;
        }
        .option-card:hover {
            border-color: #3b82f6;
        }
        .option-card.selected {
            border-color: #2563eb;
            background-color: rgba(37, 99, 235, 0.1);
        }
        .option-card.correct-answer {
            border-color: #16a34a !important;
            background-color: rgba(22, 163, 74, 0.15) !important;
        }
        .option-card.wrong-answer {
            border-color: #dc2626 !important;
            background-color: rgba(220, 38, 38, 0.15) !important;
        }
        .variant-btn.active {
            background-color: #2563eb;
            color: #ffffff;
            border-color: #2563eb;
        }
        .digit-input {
            width: 2.8rem;
            height: 2.8rem;
            text-align: center;
            font-size: 1.2rem;
            font-weight: 700;
            border-radius: 0.5rem;
        }
    </style>
</head>
<body class="bg-slate-50 dark:bg-slate-900 text-slate-800 dark:text-slate-100 min-h-screen flex flex-col justify-between">

    <!-- Top Navigation Header -->
    <header class="sticky top-0 z-40 bg-white/90 dark:bg-slate-800/90 backdrop-blur-md border-b border-slate-200 dark:border-slate-700 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 py-3 flex flex-wrap items-center justify-between gap-3">
            
            <!-- Logo & Title -->
            <div class="flex items-center gap-3">
                <div class="bg-blue-600 text-white p-2.5 rounded-xl shadow">
                    <i class="fa-solid fa-graduation-cap text-xl"></i>
                </div>
                <div>
                    <h1 class="font-bold text-slate-900 dark:text-white text-base md:text-lg leading-tight">ЭЕШ МАТЕМАТИК ПЛАТФОРМ</h1>
                    <p class="text-xs text-slate-500 dark:text-slate-400 font-medium">60 ДААЛГАВАР (50 СОНГОХ + 10 ЗАДГАЙ) — 100 ОНОО</p>
                </div>
            </div>

            <!-- Variant Selector -->
            <div class="flex items-center gap-1 bg-slate-100 dark:bg-slate-700/60 p-1 rounded-xl border border-slate-200 dark:border-slate-600">
                <span class="text-xs font-bold text-slate-500 dark:text-slate-300 px-2 hidden sm:inline">Хувилбар:</span>
                <button onclick="switchVariant('A')" id="varBtn_A" class="variant-btn px-3 py-1 text-xs font-bold rounded-lg border border-transparent bg-white dark:bg-slate-800 text-slate-700 dark:text-slate-200 shadow-sm">А</button>
                <button onclick="switchVariant('B')" id="varBtn_B" class="variant-btn px-3 py-1 text-xs font-bold rounded-lg border border-transparent bg-white dark:bg-slate-800 text-slate-700 dark:text-slate-200 shadow-sm">Б</button>
                <button onclick="switchVariant('C')" id="varBtn_C" class="variant-btn px-3 py-1 text-xs font-bold rounded-lg border border-transparent bg-white dark:bg-slate-800 text-slate-700 dark:text-slate-200 shadow-sm">В</button>
                <button onclick="switchVariant('D')" id="varBtn_D" class="variant-btn px-3 py-1 text-xs font-bold rounded-lg border border-transparent bg-white dark:bg-slate-800 text-slate-700 dark:text-slate-200 shadow-sm">Г</button>
            </div>

            <!-- Controls: Theme Toggle, Timer & Submit -->
            <div class="flex items-center gap-2">
                <!-- Theme Toggle Button -->
                <button onclick="toggleTheme()" class="p-2 rounded-xl border border-slate-200 dark:border-slate-700 bg-slate-100 dark:bg-slate-700 text-slate-700 dark:text-amber-400 hover:bg-slate-200 dark:hover:bg-slate-600 transition" title="Гэрэлтэй / Харанхуй горим">
                    <i class="fa-solid fa-moon dark:hidden text-slate-700"></i>
                    <i class="fa-solid fa-sun hidden dark:inline text-amber-400"></i>
                </button>

                <!-- Timer -->
                <div id="timerContainer" class="flex items-center gap-2 bg-slate-100 dark:bg-slate-700 border border-slate-200 dark:border-slate-600 px-3 py-1.5 rounded-xl font-mono font-bold text-slate-700 dark:text-slate-200 text-sm">
                    <i class="fa-regular fa-clock text-blue-600 dark:text-blue-400"></i>
                    <span id="timerDisplay">100:00</span>
                </div>

                <button onclick="confirmSubmit()" class="bg-emerald-600 hover:bg-emerald-700 text-white px-3.5 py-2 rounded-xl font-semibold text-xs md:text-sm transition shadow flex items-center gap-2">
                    <i class="fa-solid fa-paper-plane"></i>
                    <span class="hidden sm:inline">Хураалгах</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content Container -->
    <main class="max-w-7xl mx-auto px-4 py-6 flex-grow w-full grid grid-cols-1 lg:grid-cols-4 gap-6">

        <!-- Questions List (3 Columns) -->
        <div class="lg:col-span-3 space-y-6">

            <!-- Banner Info Card -->
            <div class="bg-white dark:bg-slate-800 p-5 rounded-2xl border border-slate-200 dark:border-slate-700 shadow-sm flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                <div class="space-y-1">
                    <div class="flex items-center gap-2">
                        <h2 class="text-base md:text-lg font-bold text-slate-800 dark:text-slate-100 flex items-center gap-2">
                            <i class="fa-solid fa-list-check text-blue-600"></i> ЭЕШ-ын жишиг даалгавар
                        </h2>
                        <span id="activeVariantBadge" class="bg-blue-100 dark:bg-blue-900/50 text-blue-700 dark:text-blue-300 font-extrabold text-xs px-2.5 py-0.5 rounded-full border border-blue-200 dark:border-blue-800">ХУВИЛБАР А</span>
                    </div>
                    <p class="text-xs text-slate-500 dark:text-slate-400">
                        • <strong>1 - 50:</strong> Сонгох даалгавар (Нэг зөв хариулт сонгоно)<br>
                        • <strong>51 - 60:</strong> Задгай даалгавар (Харьцаа ба хариуны цифрүүдийг нүдэнд оруулна)
                    </p>
                </div>

                <!-- Progress Bar -->
                <div class="bg-slate-50 dark:bg-slate-700/50 p-3 rounded-xl border border-slate-200 dark:border-slate-600 text-right min-w-[150px] w-full md:w-auto">
                    <span class="text-xs text-slate-500 dark:text-slate-400 font-bold uppercase block">Гүйцэтгэл</span>
                    <span id="progressText" class="text-lg font-black text-blue-600 dark:text-blue-400">0 / 60</span>
                    <div class="w-full bg-slate-200 dark:bg-slate-600 h-2 rounded-full mt-1 overflow-hidden">
                        <div id="progressBar" class="bg-blue-600 dark:bg-blue-400 h-full w-0 transition-all duration-300"></div>
                    </div>
                </div>
            </div>

            <!-- Result Summary Card (Hidden initially) -->
            <div id="resultSummary" class="hidden bg-white dark:bg-slate-800 p-6 rounded-2xl border border-slate-200 dark:border-slate-700 shadow-xl border-t-8 border-t-blue-600">
                <div class="flex flex-col md:flex-row justify-between items-start md:items-center border-b border-slate-200 dark:border-slate-700 pb-4 gap-4">
                    <div>
                        <span class="text-xs font-bold uppercase tracking-wider text-blue-600 dark:text-blue-400 bg-blue-50 dark:bg-blue-900/40 px-2.5 py-1 rounded-full">Шалгалтын үр дүн</span>
                        <h2 class="text-2xl font-extrabold text-slate-900 dark:text-white mt-1">Нийт дүн (<span id="resultVariantLabel">Хувилбар А</span>)</h2>
                    </div>
                    <button onclick="resetExam()" class="px-4 py-2 border border-slate-300 dark:border-slate-600 text-slate-700 dark:text-slate-200 rounded-xl hover:bg-slate-50 dark:hover:bg-slate-700 font-medium text-xs md:text-sm transition flex items-center gap-2">
                        <i class="fa-solid fa-rotate-right"></i> Дахин эхлүүлэх
                    </button>
                </div>

                <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mt-6">
                    <div class="bg-slate-50 dark:bg-slate-700/40 p-4 rounded-xl border border-slate-200 dark:border-slate-600 text-center">
                        <p class="text-xs font-semibold text-slate-500 dark:text-slate-400 uppercase">Зөв даалгавар</p>
                        <p class="text-3xl font-black text-slate-800 dark:text-white mt-1"><span id="rawScoreDisplay">0</span> <span class="text-xs font-normal text-slate-500">/ 60</span></p>
                    </div>
                    <div class="bg-blue-50 dark:bg-blue-950/40 p-4 rounded-xl border border-blue-200 dark:border-blue-800 text-center">
                        <p class="text-xs font-semibold text-blue-600 dark:text-blue-400 uppercase">Скейл оноо</p>
                        <p class="text-3xl font-black text-blue-700 dark:text-blue-300 mt-1" id="scaledScoreDisplay">200</p>
                    </div>
                    <div class="bg-indigo-50 dark:bg-indigo-950/40 p-4 rounded-xl border border-indigo-200 dark:border-indigo-800 text-center">
                        <p class="text-xs font-semibold text-indigo-600 dark:text-indigo-400 uppercase">Процентиль</p>
                        <p class="text-3xl font-black text-indigo-700 dark:text-indigo-300 mt-1"><span id="percentileDisplay">0</span>%</p>
                    </div>
                    <div class="bg-emerald-50 dark:bg-emerald-950/40 p-4 rounded-xl border border-emerald-200 dark:border-emerald-800 text-center">
                        <p class="text-xs font-semibold text-emerald-600 dark:text-emerald-400 uppercase">Гүйцэтгэл</p>
                        <p class="text-2xl font-black text-emerald-700 dark:text-emerald-300 mt-1" id="percentageDisplay">0%</p>
                    </div>
                </div>
            </div>

            <!-- Dynamic Questions Container -->
            <div id="questionsContainer" class="space-y-6"></div>

            <!-- Bottom Submit Button -->
            <div class="pt-4 flex justify-center">
                <button onclick="confirmSubmit()" class="w-full md:w-auto px-10 py-4 bg-emerald-600 hover:bg-emerald-700 text-white font-bold text-base md:text-lg rounded-2xl shadow-lg transition flex items-center justify-center gap-3">
                    <i class="fa-solid fa-circle-check text-xl"></i>
                    Шалгалтыг хураалгаж дүн харах
                </button>
            </div>

        </div>

        <!-- Sidebar Question Palette Grid (1 Column) -->
        <div class="lg:col-span-1">
            <div class="sticky top-20 bg-white dark:bg-slate-800 p-5 rounded-2xl border border-slate-200 dark:border-slate-700 shadow-sm space-y-4">
                <div class="flex items-center justify-between">
                    <h3 class="font-bold text-slate-800 dark:text-slate-100 text-sm flex items-center gap-2">
                        <i class="fa-solid fa-border-all text-blue-600"></i> Асуултын сүлжээ
                    </h3>
                    <span class="text-xs text-slate-500 dark:text-slate-400 font-semibold">60 Асуулт</span>
                </div>

                <!-- Palette Filters -->
                <div class="flex gap-1 bg-slate-100 dark:bg-slate-700 p-1 rounded-xl text-xs font-semibold">
                    <button onclick="filterPalette('all')" id="filter_all" class="flex-1 py-1 rounded-lg bg-white dark:bg-slate-800 text-slate-800 dark:text-slate-100 shadow-sm text-center">Бүгд</button>
                    <button onclick="filterPalette('mcq')" id="filter_mcq" class="flex-1 py-1 rounded-lg text-slate-500 hover:text-slate-900 dark:hover:text-white text-center">1-50</button>
                    <button onclick="filterPalette('open')" id="filter_open" class="flex-1 py-1 rounded-lg text-slate-500 hover:text-slate-900 dark:hover:text-white text-center">51-60</button>
                </div>

                <!-- 60 Buttons Grid -->
                <div id="paletteGrid" class="grid grid-cols-6 gap-1.5 max-h-[420px] overflow-y-auto pr-1"></div>

                <!-- Legend -->
                <div class="pt-3 border-t border-slate-100 dark:border-slate-700 grid grid-cols-2 gap-2 text-[11px] text-slate-500 dark:text-slate-400">
                    <div class="flex items-center gap-1.5">
                        <span class="w-3 h-3 rounded bg-blue-600 inline-block"></span> Хариулсан
                    </div>
                    <div class="flex items-center gap-1.5">
                        <span class="w-3 h-3 rounded bg-slate-200 dark:bg-slate-600 inline-block"></span> Хоосон
                    </div>
                    <div class="flex items-center gap-1.5">
                        <span class="w-3 h-3 rounded bg-emerald-500 inline-block"></span> Зөв
                    </div>
                    <div class="flex items-center gap-1.5">
                        <span class="w-3 h-3 rounded bg-red-500 inline-block"></span> Буруу
                    </div>
                </div>
            </div>
        </div>

    </main>

    <!-- Footer -->
    <footer class="bg-white dark:bg-slate-950 border-t border-slate-200 dark:border-slate-800 py-6 mt-12 text-center text-xs text-slate-500 dark:text-slate-400">
        <div class="max-w-7xl mx-auto px-4">
            <p>© 2024–2026 ЭЛСЭЛТИЙН ЕРӨНХИЙ ШАЛГАЛТАД БЭЛТГЭХ ИНТЕРАКТИВ ВЕБ ПЛАТФОРМ — МАТЕМАТИК (60 ДААЛГАВАР)</p>
            <p class="mt-1 text-slate-400 dark:text-slate-500">Боловсролын Үнэлгээний Төвийн (БҮТ) стандартын дагуу бүтээв.</p>
        </div>
    </footer>

    <!-- JavaScript Application Logic -->
    <script>
        const variantSeeds = { 'A': 0, 'B': 1, 'C': 2, 'D': 3 };

        // Helper to guarantee unique non-repeating option choices and shuffle them
        function createUniqueOptions(correctVal, distractors) {
            const set = new Set();
            set.add(String(correctVal));

            for (let d of distractors) {
                const s = String(d);
                if (!set.has(s) && set.size < 5) {
                    set.add(s);
                }
            }

            // If we still need options, generate fallback numbers
            let extra = 1;
            while (set.size < 5) {
                const num = parseFloat(correctVal);
                let fallback = !isNaN(num) ? String(num + extra) : `Утга ${extra}`;
                if (!set.has(fallback)) {
                    set.add(fallback);
                }
                extra++;
            }

            const optionsArr = Array.from(set);
            // Deterministic/Random shuffle
            const shuffled = [...optionsArr].sort(() => Math.random() - 0.5);
            const correctIndex = shuffled.indexOf(String(correctVal));

            return { options: shuffled, correct: correctIndex };
        }

        // 50 MCQ Template Generators
        const mcqTemplates = [
            { id: 1, title: "Илэрхийллийн утга бодох", gen: (k) => {
                const correct = "2";
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${k+1}`, `${2*k+3}`, "4", "1"]);
                return { text: `Илэрхийллийн утгыг олоорой: $$\\frac{${2*k+4}^2 - ${2*k+2}^2}{${4*k+6}}$$`, opts: options, correct: cIdx, exp: `$(a^2-b^2) = (a-b)(a+b)$ томьёогоор: $\\frac{2 \\cdot (${4*k+6})}{${4*k+6}} = 2$` };
            }},
            { id: 2, title: "Арифметик язгуур", gen: (k) => {
                const correct = `${3*(k+1)}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${k+1}`, `${9*(k+1)}`, `${3*k+9}`, `${k+5}`]);
                return { text: `Илэрхийллийг хялбарчлаарай: $$\\sqrt[3]{${27*(k+1)^3}}$$`, opts: options, correct: cIdx, exp: `$\\sqrt[3]{27(k+1)^3} = 3(k+1) = ${3*(k+1)}$` };
            }},
            { id: 3, title: "Илтгэгч тэгшитгэл", gen: (k) => {
                const correct = "0";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["1", `${k+1}`, "2", "-1"]);
                return { text: `Тэгшитгэлийн шийдийг олоорой: $$2^{x+${k+1}} - 2^x = ${2**(k+1)-1}$$`, opts: options, correct: cIdx, exp: `$2^x(2^{${k+1}} - 1) = ${2**(k+1)-1} \\implies 2^x = 1 \\implies x = 0$` };
            }},
            { id: 4, title: "Логарифм илэрхийлэл", gen: (k) => {
                const correct = "4";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["3", `${k+3}`, "1", `${k+5}`]);
                return { text: `Утгыг олоорой: $$\\log_${k+3} ${(k+3)**4}$$`, opts: options, correct: cIdx, exp: `$\\log_a(a^4) = 4$` };
            }},
            { id: 5, title: "Логарифм тэгшитгэл", gen: (k) => {
                const correct = `${9 - (k+2)}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${10-k}`, `${8-k}`, "9", "7"]);
                return { text: `Тэгшитгэлийг бодоорой: $$\\log_3(x + ${k+2}) = 2$$`, opts: options, correct: cIdx, exp: `$x + ${k+2} = 3^2 = 9 \\implies x = ${9 - (k+2)}$` };
            }},
            { id: 6, title: "Квадрат тэгшитгэл", gen: (k) => {
                const correct = `${k+2}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${k+3}`, `${2*k+5}`, "0", `${k+7}`]);
                return { text: `Тэгшитгэлийн бага шийдийг олоорой: $$x^2 - ${(2*k+5)}x + ${(k+2)*(k+3)} = 0$$`, opts: options, correct: cIdx, exp: `Шийдүүд $x_1 = ${k+2}, x_2 = ${k+3}$. Бага шийд нь ${k+2}$.` };
            }},
            { id: 7, title: "Виетийн теорем", gen: (k) => {
                const correct = `${(k+1)*(2*k+3)}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${2*k+3}`, `${k+1}`, `${3*k+4}`, `${k+10}`]);
                return { text: `$$x^2 - ${3*k+4}x + q = 0$$ тэгшитгэлийн нэг шийд $x_1 = ${k+1}$ бол $q$-г олоорой.`, opts: options, correct: cIdx, exp: `$x_2 = ${3*k+4} - (${k+1}) = ${2*k+3} \\implies q = x_1 x_2 = ${(k+1)*(2*k+3)}$` };
            }},
            { id: 8, title: "Рационал тэнцэтгэл биш", gen: (k) => {
                const correct = `$(-${k+1}; ${k+4}]$`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`[-${k+1}; ${k+4}]`, `(-${k+1}; ${k+4})`, `$(-\\infty; -${k+1})$`, `$[${k+4}; +\\infty)$`]);
                return { text: `Тэнцэтгэл бишийг бодоорой: $$\\frac{x - ${k+4}}{x + ${k+1}} \\le 0$$`, opts: options, correct: cIdx, exp: `Интервалын аргаар: $x \\in (-${k+1}; ${k+4}]$` };
            }},
            { id: 9, title: "Модультай тэгшитгэл", gen: (k) => {
                const correct = `${2*(k+3)}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${k+3}`, "14", "0", `${k+12}`]);
                return { text: `Тэгшитгэлийн шийдүүдийн нийлбэрийг олоорой: $$|x - ${k+3}| = 7$$`, opts: options, correct: cIdx, exp: `$x_1 = ${k+10}, x_2 = ${k-4} \\implies \\text{Нийлбэр} = ${2*(k+3)}$` };
            }},
            { id: 10, title: "Арифметик прогресс", gen: (k) => {
                const correct = `${k+38}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${k+40}`, "36", "40", `${k+20}`]);
                return { text: `$a_1 = ${k+2}$, $d = 4$ бол $a_{10}$-ийг олоорой.`, opts: options, correct: cIdx, exp: `$a_{10} = a_1 + 9d = ${k+2} + 36 = ${k+38}$` };
            }},
            { id: 11, title: "Геометрийн прогресс", gen: (k) => {
                const correct = `${13*(k+1)}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${9*(k+1)}`, `${27*(k+1)}`, "13", `${k+15}`]);
                return { text: `$b_1 = ${k+1}$, $q = 3$ бол $S_3$-ийг олоорой.`, opts: options, correct: cIdx, exp: `$S_3 = b_1 (1 + 3 + 9) = 13(${k+1}) = ${13*(k+1)}$` };
            }},
            { id: 12, title: "Төгсгөлгүй буурах прогресс", gen: (k) => {
                const correct = `${8*(k+1)}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${4*(k+1)}`, `${16*(k+1)}`, "8", `${k+12}`]);
                return { text: `$b_1 = ${4*(k+1)}$, $q = 1/2$ бол нийлбэрийг олоорой.`, opts: options, correct: cIdx, exp: `$S = \\frac{b_1}{1-q} = \\frac{${4*(k+1)}}{1/2} = ${8*(k+1)}$` };
            }},
            { id: 13, title: "Квадрат функцийн орой", gen: (k) => {
                const correct = "3";
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${k+2}`, "0", "9", "-3"]);
                return { text: `$$y = x^2 - ${2*(k+2)}x + ${(k+2)**2 + 3}$$ параболын оройн цэгийн ординатыг олоорой.`, opts: options, correct: cIdx, exp: `$y = (x - (${k+2}))^2 + 3 \\implies y_{орой} = 3$` };
            }},
            { id: 14, title: "Функцийн хамгийн бага утга", gen: (k) => {
                const correct = `${k+5}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${k+1}`, "0", "5", `${k+10}`]);
                return { text: `$$f(x) = (x - ${k+1})^2 + ${k+5}$$ функцийн хамгийн бага утгыг олоорой.`, opts: options, correct: cIdx, exp: `Квадрат илэрхийлэл 0-ээс багагүй тул хамгийн бага утга нь ${k+5}.` };
            }},
            { id: 15, title: "Тригонометр утга", gen: (k) => {
                const correct = "-1/2";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["1/2", "$\\sqrt{3}/2$", "$-\\sqrt{3}/2$", "0"]);
                return { text: `Утгыг олоорой: $$\\cos\\left(${2*k+1}\\pi + \\frac{\\pi}{3}\\right)$$`, opts: options, correct: cIdx, exp: "$\\cos((2k+1)\\pi + \\pi/3) = -\\cos(\\pi/3) = -1/2$" };
            }},
            { id: 16, title: "Тригонометр тэгшитгэл", gen: (k) => {
                const correct = "$\\pi/3$";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["$\\pi/6$", "$\\pi/4$", "$2\\pi/3$", "$\\pi/2$"]);
                return { text: `$[0, \\pi]$ завсарт тэгшитгэлийг бодоорой: $$2\\sin x - \\sqrt{3} = 0$$`, opts: options, correct: cIdx, exp: "$\\sin x = \\sqrt{3}/2 \\implies x = \\pi/3$" };
            }},
            { id: 17, title: "Тригонометр адилтгал", gen: (k) => {
                const correct = "1";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["0", "$\\tan^2 x$", "$\\cot^2 x$", "-1"]);
                return { text: `Илэрхийллийг хялбарчлаарай: $$\\frac{1 - \\cos^2 x}{\\sin^2 x}$$`, opts: options, correct: cIdx, exp: "$\\sin^2 x / \\sin^2 x = 1$" };
            }},
            { id: 18, title: "Уламжлал (Зэрэгт функц)", gen: (k) => {
                const correct = `${k+4}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, ["1", `${k+3}`, "0", `${k+8}`]);
                return { text: `$$f(x) = x^{${k+4}}$$ функцийн $x = 1$ цэг дэх уламжлалыг олоорой.`, opts: options, correct: cIdx, exp: `$f'(x) = (${k+4})x^{${k+3}} \\implies f'(1) = ${k+4}$` };
            }},
            { id: 19, title: "Уламжлал (Үржвэр)", gen: (k) => {
                const correct = "$\\cos x - x\\sin x$";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["$\\cos x + x\\sin x$", "$-\\sin x$", "$x\\cos x$", "$1 - \\sin x$"]);
                return { text: `$$f(x) = x \\cdot \\cos x$$ функцийн уламжлалыг олоорой.`, opts: options, correct: cIdx, exp: "$(uv)' = u'v + uv' \\implies \\cos x - x\\sin x$" };
            }},
            { id: 20, title: "Шүргэгчийн өнцөг", gen: (k) => {
                const correct = `${k+4}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${k+2}`, "2", `${2*k+4}`, `${k+9}`]);
                return { text: `$$y = x^2 + ${k+2}x$$ параболын $x_0 = 1$ цэгт татсан шүргэгчийн өнцгийн коэффициентийг олоорой.`, opts: options, correct: cIdx, exp: `$y'(x) = 2x + ${k+2} \\implies y'(1) = ${k+4}$` };
            }},
            { id: 21, title: "Өсөх завсар", gen: (k) => {
                const correct = `$[${k+1}; +\\infty)$`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`$(-\\infty; ${k+1}]$`, `$[0; +\\infty)$`, `$(-\\infty; 0)$`, `$[-1; 1]$`]);
                return { text: `$$f(x) = x^2 - ${2*(k+1)}x$$ функцийн өсөх завсрыг олоорой.`, opts: options, correct: cIdx, exp: `$f'(x) = 2x - ${2*(k+1)} \\ge 0 \\implies x \\ge ${k+1}$` };
            }},
            { id: 22, title: "Критик цэг", gen: (k) => {
                const correct = `${k+2}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${(k+2)**2}`, "1", "0", `${2*k+4}`]);
                return { text: `$$f(x) = \\frac{x^3}{3} - ${(k+2)**2}x$$ функцийн эерэг критик цэгийг олоорой.`, opts: options, correct: cIdx, exp: `$f'(x) = x^2 - ${(k+2)**2} = 0 \\implies x = ${k+2}$` };
            }},
            { id: 23, title: "Тодорхойгүй интеграл", gen: (k) => {
                const correct = `$x^3 + ${k+1}x^2 + C$`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`$6x + ${2*(k+1)} + C$`, `$x^3 + C$`, `$3x^3 + C$`, `$x^2 + C$`]);
                return { text: `$$\\int (3x^2 + ${2*(k+1)}x) \\, dx$$ интегралыг бодоорой.`, opts: options, correct: cIdx, exp: "$\\int (3x^2 + 2(k+1)x)dx = x^3 + (k+1)x^2 + C$" };
            }},
            { id: 24, title: "Тодорхой интеграл", gen: (k) => {
                const correct = `${4.5*(k+1)}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${9*(k+1)}`, `${3*(k+1)}`, "4.5", `${k+10}`]);
                return { text: `$$\\int_{0}^{3} ${k+1}x \\, dx$$ интегралыг бодоорой.`, opts: options, correct: cIdx, exp: `$\\left[\\frac{${k+1}x^2}{2}\\right]_0^3 = \\frac{9(${k+1})}{2} = ${4.5*(k+1)}$` };
            }},
            { id: 25, title: "Дүрсийн талбай", gen: (k) => {
                const correct = "32/3";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["16/3", "8/3", "4", "8"]);
                return { text: `$$y = x^2$$ ба $y = 4$ шулуунаар хязгаарлагдсан дүрсийн талбайг олоорой.`, opts: options, correct: cIdx, exp: "$\\int_{-2}^{2} (4 - x^2)dx = [4x - x^3/3]_{-2}^2 = 32/3$" };
            }},
            { id: 26, title: "Комплекс тооны модуль", gen: (k) => {
                const correct = `${10*(k+1)}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${14*(k+1)}`, "10", "14", `${10*k+20}`]);
                return { text: `$$z = ${6*(k+1)} + ${8*(k+1)}i$$ комплекс тооны модулийг олоорой.`, opts: options, correct: cIdx, exp: `$|z| = \\sqrt{(${6*(k+1)})^2 + (${8*(k+1)})^2} = ${10*(k+1)}$` };
            }},
            { id: 27, title: "Комплекс тоо", gen: (k) => {
                const correct = `${k+6}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, ["5", "2", `${k+1}`, `${k+12}`]);
                return { text: `$$(5 + 2i) + (${k+1} - 2i)$$ тооны бодит хэсгийг олоорой.`, opts: options, correct: cIdx, exp: "Бодит хэсэг $5 + " + (k+1) + " = " + (k+6) + "$" };
            }},
            { id: 28, title: "Векторын урт", gen: (k) => {
                const correct = `${5*(k+1)}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${7*(k+1)}`, "5", "7", `${5*k+12}`]);
                return { text: `$$\\vec{a} = (${3*(k+1)}, ${4*(k+1)})$$ векторын уртыг олоорой.`, opts: options, correct: cIdx, exp: `$|a| = \\sqrt{(${3*(k+1)})^2 + (${4*(k+1)})^2} = ${5*(k+1)}$` };
            }},
            { id: 29, title: "Скаляр үржвэр", gen: (k) => {
                const correct = "3";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["0", `${k+5}`, "1", "-3"]);
                return { text: `$$\\vec{a} = (3, ${k+1}), \\quad \\vec{b} = (${k+2}, -3)$$ векторуудын скаляр үржвэрийг олоорой.`, opts: options, correct: cIdx, exp: "$\\vec{a} \\cdot \\vec{b} = 3(k+2) - 3(k+1) = 3k + 6 - 3k - 3 = 3$" };
            }},
            { id: 30, title: "Перпендикуляр вектор", gen: (k) => {
                const correct = "2";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["3", "-2", "6", "0"]);
                return { text: `$$\\vec{a} = (x, 6)$$ ба $$\\vec{b} = (3, -1)$$ векторууд перпендикуляр бол $x$-ийг олоорой.`, opts: options, correct: cIdx, exp: "$3x - 6 = 0 \\implies x = 2$" };
            }},
            { id: 31, title: "Матрицын детерминант", gen: (k) => {
                const correct = `${k-3}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${k+3}`, "3", "1", `${k+7}`]);
                return { text: `$$A = \\begin{pmatrix} ${k+3} & 2 \\\\ 3 & 1 \\end{pmatrix}$$ детерминантыг бодоорой.`, opts: options, correct: cIdx, exp: "$\\det(A) = (k+3)(1) - (2)(3) = k - 3$" };
            }},
            { id: 32, title: "Хязгаар бодох", gen: (k) => {
                const correct = "6";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["3", "0", "9", "-6"]);
                return { text: `$$\\lim_{x \\to 3} \\frac{x^2 - 9}{x - 3}$$ хязгаарыг бодоорой.`, opts: options, correct: cIdx, exp: "$\\lim_{x \\to 3} (x + 3) = 6$" };
            }},
            { id: 33, title: "Гайхамшигт хязгаар", gen: (k) => {
                const correct = `${k+3}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, ["1", "0", `${k+7}`, "3"]);
                return { text: `$$\\lim_{x \\to 0} \\frac{\\sin(${k+3}x)}{x}$$ хязгаарыг бодоорой.`, opts: options, correct: cIdx, exp: "$\\lim_{x \\to 0} \\frac{\\sin(ax)}{x} = a \\implies " + (k+3) + "$" };
            }},
            { id: 34, title: "Пифагорын теорем", gen: (k) => {
                const correct = `${13*(k+1)} см`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${17*(k+1)} см`, "13 см", "17 см", `${13*k+20} см`]);
                return { text: `Тэгш өнцөгт гурвалжны катетууд ${5*(k+1)} см ба ${12*(k+1)} см бол гипотенузыг олоорой.`, opts: options, correct: cIdx, exp: `$c = \\sqrt{(${5*(k+1)})^2 + (${12*(k+1)})^2} = ${13*(k+1)}$ см` };
            }},
            { id: 35, title: "Гурвалжны талбай", gen: (k) => {
                const correct = "30 см²";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["60 см²", "120 см²", "15 см²", "25 см²"]);
                return { text: `Талууд 10 см, 12 см ба тэдгээрийн хоорондох өнцөг $30^\\circ$ бол талбайг олоорой.`, opts: options, correct: cIdx, exp: "$S = \\frac{1}{2} \\cdot 10 \\cdot 12 \\cdot \\sin 30^\\circ = 30$ см²" };
            }},
            { id: 36, title: "Косинусын теорем", gen: (k) => {
                const correct = "$\\sqrt{28}$ см";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["28 см", "10 см", "$\\sqrt{52}$ см", "2 см"]);
                return { text: `Гурвалжны хоёр тал 4 см, 6 см ба өнцөг нь $60^\\circ$ бол гурав дахь талыг олоорой.`, opts: options, correct: cIdx, exp: "$c^2 = 16 + 36 - 2(4)(6)(0.5) = 28 \\implies c = \\sqrt{28}$" };
            }},
            { id: 37, title: "Синусын теорем", gen: (k) => {
                const correct = "12 см";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["6 см", "24 см", "18 см", "3 см"]);
                return { text: `Талын урт 12 см, эсрэг өнцөг нь $30^\\circ$ бол багтсан тойргийн радиусыг олоорой.`, opts: options, correct: cIdx, exp: "$\\frac{a}{\\sin A} = 2R \\implies \\frac{12}{0.5} = 24 = 2R \\implies R = 12$ см" };
            }},
            { id: 38, title: "Тойргийн өнцөг", gen: (k) => {
                const correct = "$50^\\circ$";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["$100^\\circ$", "$200^\\circ$", "$25^\\circ$", "$90^\\circ$"]);
                return { text: `Тойргийн төв өнцөг $100^\\circ$ бол тухайн нугаламд тулсан багтсан өнцгийг олоорой.`, opts: options, correct: cIdx, exp: "Багтсан өнцөг $= 100^\\circ / 2 = 50^\\circ$" };
            }},
            { id: 39, title: "Ромбын талбай", gen: (k) => {
                const correct = `${(k+4)**2} см²`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${2*(k+4)**2} см²`, "8 см²", "16 см²", `${(k+4)**2 + 10} см²`]);
                return { text: `Ромбын диагоналиуд нь ${k+4} см ба ${2*k+8} см бол талбайг олоорой.`, opts: options, correct: cIdx, exp: `$S = \\frac{d_1 d_2}{2} = \\frac{(${k+4})(${2*k+8})}{2} = ${(k+4)**2}$ см²` };
            }},
            { id: 40, title: "Трапецийн дундаж шугам", gen: (k) => {
                const correct = `${k+7} см`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${2*k+14} см`, "7 см", "14 см", `${k+12} см`]);
                return { text: `Трапецийн сууриуд ${k+4} см ба ${k+10} см бол дундаж шугамыг олоорой.`, opts: options, correct: cIdx, exp: `$m = \\frac{(${k+4}) + (${k+10})}{2} = ${k+7}$ см` };
            }},
            { id: 41, title: "Хоёр цэгийн зай", gen: (k) => {
                const correct = "5";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["3", "4", "25", "7"]);
                return { text: `$A(${k+1}, 1)$ ба $B(${k+4}, 5)$ цэгүүдийн хоорондох зайг олоорой.`, opts: options, correct: cIdx, exp: "$d = \\sqrt{(k+4 - (k+1))^2 + (5 - 1)^2} = \\sqrt{9 + 16} = 5$" };
            }},
            { id: 42, title: "Шулууны тэгшитгэл", gen: (k) => {
                const correct = `$y = 3x + ${k+2}$`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`$y = x + ${k+2}$`, `$y = 3x - ${k+2}$`, `$y = ${k+2}x + 3$`, "$y = 3x$"]);
                return { text: `$(0, ${k+2})$ цэгийг дайрсан, $k=3$ өнцгийн коэффициенттэй шулууны тэгшитгэлийг сонгоорой.`, opts: options, correct: cIdx, exp: `$y = 3x + ${k+2}$` };
            }},
            { id: 43, title: "Тойргийн тэгшитгэл", gen: (k) => {
                const correct = `$x^2 + y^2 = ${(k+3)**2}$`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`$x^2 + y^2 = ${k+3}$`, `$x + y = ${k+3}$`, "$x^2 - y^2 = 9$", "$x^2 + y^2 = 1$"]);
                return { text: `Төв нь $(0, 0)$, радиус $R = ${k+3}$ байх тойргийн тэгшитгэлийг сонгоорой.`, opts: options, correct: cIdx, exp: `$x^2 + y^2 = R^2 = ${(k+3)**2}$` };
            }},
            { id: 44, title: "Кубын эзлэхүүн", gen: (k) => {
                const correct = `${(k+3)**3} см³`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${6*(k+3)**2} см³`, "27 см³", "64 см³", `${(k+3)**3 + 20} см³`]);
                return { text: `Кубын ирмэг $a = ${k+3}$ см бол эзлэхүүнийг олоорой.`, opts: options, correct: cIdx, exp: `$V = a^3 = (${k+3})^3 = ${(k+3)**3}$ см³` };
            }},
            { id: 45, title: "Кубын гадаргуу", gen: (k) => {
                const correct = `${6*(k+2)**2} см²`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${(k+2)**3} см²`, "24 см²", "54 см²", `${6*(k+2)**2 + 12} см²`]);
                return { text: `Кубын ирмэг $a = ${k+2}$ см бол бүтэн гадаргуугийн талбайг олоорой.`, opts: options, correct: cIdx, exp: `$S = 6a^2 = 6(${k+2})^2 = ${6*(k+2)**2}$ см²` };
            }},
            { id: 46, title: "Пирамидын эзлэхүүн", gen: (k) => {
                const correct = `${8*(k+2)} см³`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${24*(k+2)} см³`, "16 см³", "24 см³", `${8*k+30} см³`]);
                return { text: `Суурийн талбай $S = 24$ см², өндөр $h = ${k+2}$ см пирамидын эзлэхүүнийг олоорой.`, opts: options, correct: cIdx, exp: `$V = \\frac{1}{3} S h = 8(${k+2}) = ${8*(k+2)}$ см³` };
            }},
            { id: 47, title: "Цилиндрийн эзлэхүүн", gen: (k) => {
                const correct = `${4*(k+3)}\\pi$`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${2*(k+3)}\\pi$`, "12\\pi", "16\\pi", `${4*k+20}\\pi$`]);
                return { text: `Суурийн радиус $r = 2$, өндөр $h = ${k+3}$ цилиндрийн эзлэхүүнийг олоорой.`, opts: options, correct: cIdx, exp: `$V = \\pi r^2 h = 4(${k+3})\\pi = ${4*(k+3)}\\pi$` };
            }},
            { id: 48, title: "Бөмбөрцөгийн эзлэхүүн", gen: (k) => {
                const correct = "$288\\pi$ см³";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["$144\\pi$ см³", "$36\\pi$ см³", "$72\\pi$ см³", "$576\\pi$ см³"]);
                return { text: `Радиус $R = 6$ см бөмбөрцөгийн эзлэхүүнийг олоорой.`, opts: options, correct: cIdx, exp: "$V = \\frac{4}{3}\\pi R^3 = \\frac{4}{3}\\pi (216) = 288\\pi$ см³" };
            }},
            { id: 49, title: "Сэлгэмэл (Permutation)", gen: (k) => {
                const correct = `${[1,1,2,6,24,120,720][k+2]}`;
                const { options, correct: cIdx } = createUniqueOptions(correct, [`${(k+2)**2}`, "6", "24", "120"]);
                return { text: `${k+2} ширхэг өөр картнаас дараалуулан өрөх боломжийн тоог олоорой.`, opts: options, correct: cIdx, exp: `$P_{${k+2}} = (${k+2})! = ${[1,1,2,6,24,120,720][k+2]}$` };
            }},
            { id: 50, title: "Классик магадлал", gen: (k) => {
                const correct = "1/2";
                const { options, correct: cIdx } = createUniqueOptions(correct, ["1/3", "2/3", "1/6", "5/6"]);
                return { text: `Шоог нэг удаа хаяхад 3-аас дээш тоо буух магадлалыг олоорой.`, opts: options, correct: cIdx, exp: "Боломжууд {4, 5, 6} (3 ширхэг). $P = 3/6 = 1/2$" };
            }}
        ];

        // 10 Open-Ended Tasks Generators (Part II: Tasks 51-60)
        const openTemplates = [
            { id: 51, title: "Задгай: Квадрат функц", gen: (k) => ({ text: `$$f(x) = x^2 - ${(2*k+6)}x + ${(k+3)**2 + 4}$$ квадрат функцийн:<br>а) Оройн цэгийн абсцисс $x_0 = [a]$<br>б) Оройн цэгийн ординат $y_0 = [b]$`, fields: [{ label: "a", correct: `${k+3}` }, { label: "b", correct: "4" }], exp: `$f(x) = (x - (${k+3}))^2 + 4 \\implies x_0 = ${k+3}, y_0 = 4$` }) },
            { id: 52, title: "Задгай: Илтгэгч ба Логарифм", gen: (k) => ({ text: `а) $2^{x+1} = ${2**(k+2)}$ бол $x = [a]$<br>б) $\\log_2 y = ${k+3}$ бол $y = [b.c]$`, fields: [{ label: "a", correct: `${k+1}` }, { label: "bc", correct: `${2**(k+3)}` }], exp: `$a) x+1 = ${k+2} \\implies x = ${k+1}$, $b) y = 2^{${k+3}} = ${2**(k+3)}$` }) },
            { id: 53, title: "Задгай: Арифметик прогресс", gen: (k) => ({ text: `$a_1 = ${k+2}$, $d = 3$ арифметик прогрессийн:<br>а) 5-р гишүүн $a_5 = [a.b]$<br>б) Эхний 5 гишүүний нийлбэр $S_5 = [c.d]$`, fields: [{ label: "ab", correct: `${k+14}` }, { label: "cd", correct: `${5*(k+8)}` }], exp: `$a_5 = ${k+2} + 12 = ${k+14}$, $S_5 = \\frac{${k+2} + ${k+14}}{2} \\cdot 5 = ${5*(k+8)}$` }) },
            { id: 54, title: "Задгай: Тригонометр", gen: (k) => ({ text: `$\\sin x = 3/5$ ба $x \\in (0; \\pi/2)$ бол:<br>а) $\\cos x = [a]/5$<br>б) $\\tan x = 3/[b]$`, fields: [{ label: "a", correct: "4" }, { label: "b", correct: "4" }], exp: "$\\cos x = 4/5$, $\\tan x = 3/4$" }) },
            { id: 55, title: "Задгай: Уламжлал ба Шүргэгч", gen: (k) => ({ text: `$$f(x) = x^2 + ${2*k+2}x + 1$$ функцийн $x = 1$ цэгт:<br>а) Функцийн утга $f(1) = [a.b]$<br>б) Уламжлалын утга $f'(1) = [c]$`, fields: [{ label: "ab", correct: `${2*k+4}` }, { label: "c", correct: `${2*k+4}` }], exp: `$f(1) = 1 + ${2*k+2} + 1 = ${2*k+4}$, $f'(x) = 2x + ${2*k+2} \\implies f'(1) = ${2*k+4}$` }) },
            { id: 56, title: "Задгай: Интеграл", gen: (k) => ({ text: `$$\\int_0^2 (3x^2 + ${k+1}) dx$$ интегралын утга нь $[a.b]$ болно.`, fields: [{ label: "ab", correct: `${8 + 2*(k+1)}` }], exp: `$[x^3 + (${k+1})x]_0^2 = 8 + 2(${k+1}) = ${8 + 2*(k+1)}$` }) },
            { id: 57, title: "Задгай: Вектор ба Зай", gen: (k) => ({ text: `$\\vec{a} = (3, 4)$ ба $\\vec{b} = (${k+1}, 0)$ векторуудын:<br>а) $\\vec{a}$ векторын урт $[a]$<br>б) Скаляр үржвэр $\\vec{a} \\cdot \\vec{b} = [b.c]$`, fields: [{ label: "a", correct: "5" }, { label: "bc", correct: `${3*(k+1)}` }], exp: `$|\\vec{a}| = 5$, $\\vec{a} \\cdot \\vec{b} = 3(${k+1}) = ${3*(k+1)}$` }) },
            { id: 58, title: "Задгай: Геометр (Тэгш өнцөгт)", gen: (k) => ({ text: `Тэгш өнцөгт гурвалжны катетууд $a = 6$, $b = 8$ бол:<br>а) Гипотенуз $c = [a.b]$<br>б) Талбай $S = [c.d]$`, fields: [{ label: "ab", correct: "10" }, { label: "cd", correct: "24" }], exp: "$c = \\sqrt{36+64} = 10$, $S = (6 \\cdot 8)/2 = 24$" }) },
            { id: 59, title: "Задгай: Комбинаторик", gen: (k) => ({ text: `а) $C_5^3 = [a.b]$<br>б) $A_4^2 = [c.d]$`, fields: [{ label: "ab", correct: "10" }, { label: "cd", correct: "12" }], exp: "$C_5^3 = 10$, $A_4^2 = 12$" }) },
            { id: 60, title: "Задгай: Магадлал ба Статистик", gen: (k) => ({ text: `$2, 4, 6, 8, ${2*k+10}$ тоонуудын:<br>а) Медиан нь $[a]$<br>б) Арифметик дундаж нь $[b]$`, fields: [{ label: "a", correct: "6" }, { label: "b", correct: `${(20 + 2*k+10)/5}` }], exp: `Медиан $= 6$, Дундаж $= (20 + 2k+10)/5 = ${(20 + 2*k+10)/5}$` }) }
        ];

        // State Variables
        let currentVariant = 'A';
        let currentQuestions = [];
        let userAnswers = {}; 
        let isSubmitted = false;
        let paletteFilter = 'all';

        // Timer Variables
        let totalSeconds = 100 * 60;
        let timerInterval = null;

        // Theme Initialization
        if (localStorage.getItem('theme') === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.documentElement.classList.add('dark');
        } else {
            document.documentElement.classList.remove('dark');
        }

        function toggleTheme() {
            if (document.documentElement.classList.contains('dark')) {
                document.documentElement.classList.remove('dark');
                localStorage.setItem('theme', 'light');
            } else {
                document.documentElement.classList.add('dark');
                localStorage.setItem('theme', 'dark');
            }
        }

        // Initialize App
        window.addEventListener('DOMContentLoaded', () => {
            switchVariant('A');
            startTimer();
        });

        function generateVariantQuestions(varKey) {
            const seed = variantSeeds[varKey];
            
            const mcqs = mcqTemplates.map(t => {
                const data = t.gen(seed);
                return {
                    id: t.id,
                    type: 'mcq',
                    title: t.title,
                    text: data.text,
                    options: data.opts,
                    correct: data.correct,
                    exp: data.exp
                };
            });

            const opens = openTemplates.map(t => {
                const data = t.gen(seed);
                return {
                    id: t.id,
                    type: 'open',
                    title: t.title,
                    text: data.text,
                    fields: data.fields,
                    exp: data.exp
                };
            });

            return [...mcqs, ...opens];
        }

        function switchVariant(varKey) {
            currentVariant = varKey;
            currentQuestions = generateVariantQuestions(varKey);
            userAnswers = {};
            isSubmitted = false;

            ['A', 'B', 'C', 'D'].forEach(v => {
                const btn = document.getElementById(`varBtn_${v}`);
                if (v === varKey) {
                    btn.classList.add('active');
                } else {
                    btn.classList.remove('active');
                }
            });

            document.getElementById('activeVariantBadge').innerText = `ХУВИЛБАР ${varKey === 'A' ? 'А' : varKey === 'B' ? 'Б' : varKey === 'C' ? 'В' : 'Г'}`;
            document.getElementById('resultVariantLabel').innerText = `Хувилбар ${varKey === 'A' ? 'А' : varKey === 'B' ? 'Б' : varKey === 'C' ? 'В' : 'Г'}`;
            document.getElementById('resultSummary').classList.add('hidden');

            renderQuestions();
            renderPaletteGrid();
            updateProgress();
        }

        function renderQuestions() {
            const container = document.getElementById('questionsContainer');
            container.innerHTML = '';

            currentQuestions.forEach(q => {
                const qCard = document.createElement('div');
                qCard.id = `question_card_${q.id}`;
                qCard.className = "bg-white dark:bg-slate-800 p-6 rounded-2xl border border-slate-200 dark:border-slate-700 shadow-sm transition-all";

                if (q.type === 'mcq') {
                    renderMcqCard(q, qCard);
                } else {
                    renderOpenCard(q, qCard);
                }

                container.appendChild(qCard);
            });

            if (window.MathJax) {
                MathJax.typesetPromise();
            }
        }

        function renderMcqCard(q, qCard) {
            const selectedOpt = userAnswers[q.id];
            const labels = ['А', 'Б', 'В', 'Г', 'Д'];

            let optionsHTML = '';
            q.options.forEach((optText, optIdx) => {
                let cardClass = "border-slate-200 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-700/50";

                if (isSubmitted) {
                    if (optIdx === q.correct) {
                        cardClass = "correct-answer font-bold";
                    } else if (selectedOpt === optIdx && optIdx !== q.correct) {
                        cardClass = "wrong-answer";
                    }
                } else if (selectedOpt === optIdx) {
                    cardClass = "selected font-semibold";
                }

                optionsHTML += `
                    <div onclick="selectMcqOption(${q.id}, ${optIdx})" 
                         class="option-card border rounded-xl p-3.5 cursor-pointer flex items-center justify-between text-sm md:text-base ${cardClass}">
                        <div class="flex items-center gap-3">
                            <span class="w-7 h-7 rounded-lg border border-slate-300 dark:border-slate-600 bg-white dark:bg-slate-800 flex items-center justify-center text-xs font-bold text-slate-700 dark:text-slate-300 shadow-sm">
                                ${labels[optIdx]}
                            </span>
                            <span class="text-slate-800 dark:text-slate-100">${optText}</span>
                        </div>
                        ${isSubmitted && optIdx === q.correct ? '<i class="fa-solid fa-circle-check text-emerald-600 dark:text-emerald-400 text-lg"></i>' : ''}
                        ${isSubmitted && selectedOpt === optIdx && optIdx !== q.correct ? '<i class="fa-solid fa-circle-xmark text-red-600 dark:text-red-400 text-lg"></i>' : ''}
                    </div>
                `;
            });

            let expHTML = isSubmitted ? `
                <div class="mt-4 pt-4 border-t border-slate-100 dark:border-slate-700 bg-slate-50 dark:bg-slate-700/40 p-4 rounded-xl text-xs md:text-sm text-slate-700 dark:text-slate-300">
                    <span class="font-bold text-blue-600 dark:text-blue-400 flex items-center gap-1.5 mb-1">
                        <i class="fa-solid fa-lightbulb"></i> Бодолт:
                    </span>
                    <div>${q.exp}</div>
                </div>` : '';

            qCard.innerHTML = `
                <div class="flex justify-between items-start gap-3 mb-3">
                    <span class="bg-blue-50 dark:bg-blue-900/40 text-blue-700 dark:text-blue-300 font-extrabold text-xs px-3 py-1 rounded-full border border-blue-200 dark:border-blue-800">
                        Даалгавар ${q.id} (Сонгох)
                    </span>
                    <span class="text-xs font-semibold text-slate-400 dark:text-slate-500 uppercase tracking-wide">
                        ${q.title}
                    </span>
                </div>
                <div class="text-base md:text-lg font-medium text-slate-900 dark:text-slate-100 mb-4 leading-relaxed">
                    ${q.text}
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                    ${optionsHTML}
                </div>
                ${expHTML}
            `;
        }

        function renderOpenCard(q, qCard) {
            const currentValObj = userAnswers[q.id] || {};

            let fieldsHTML = '<div class="flex flex-wrap items-center gap-4 my-3">';
            q.fields.forEach(f => {
                const userVal = currentValObj[f.label] || '';
                let inputBorder = "border-slate-300 dark:border-slate-600 bg-white dark:bg-slate-700 text-slate-900 dark:text-white";

                if (isSubmitted) {
                    if (userVal.trim() === f.correct) {
                        inputBorder = "border-emerald-500 bg-emerald-50 dark:bg-emerald-950/40 text-emerald-700 dark:text-emerald-300 font-bold";
                    } else {
                        inputBorder = "border-red-500 bg-red-50 dark:bg-red-950/40 text-red-700 dark:text-red-300 font-bold";
                    }
                }

                fieldsHTML += `
                    <div class="flex items-center gap-2 bg-slate-50 dark:bg-slate-700/50 p-2 rounded-xl border border-slate-200 dark:border-slate-600">
                        <span class="font-mono font-bold text-slate-700 dark:text-slate-200 text-sm">[${f.label}] =</span>
                        <input type="text" 
                               value="${userVal}" 
                               ${isSubmitted ? 'disabled' : ''}
                               oninput="handleOpenInput(${q.id}, '${f.label}', this.value)"
                               class="digit-input border focus:outline-none focus:ring-2 focus:ring-blue-500 ${inputBorder}"
                               placeholder="?">
                        ${isSubmitted ? `<span class="text-xs font-bold text-slate-500 dark:text-slate-400 ml-1">(Зөв: ${f.correct})</span>` : ''}
                    </div>
                `;
            });
            fieldsHTML += '</div>';

            let expHTML = isSubmitted ? `
                <div class="mt-4 pt-4 border-t border-slate-100 dark:border-slate-700 bg-slate-50 dark:bg-slate-700/40 p-4 rounded-xl text-xs md:text-sm text-slate-700 dark:text-slate-300">
                    <span class="font-bold text-blue-600 dark:text-blue-400 flex items-center gap-1.5 mb-1">
                        <i class="fa-solid fa-lightbulb"></i> Бодолт:
                    </span>
                    <div>${q.exp}</div>
                </div>` : '';

            qCard.innerHTML = `
                <div class="flex justify-between items-start gap-3 mb-3">
                    <span class="bg-amber-50 dark:bg-amber-950/40 text-amber-700 dark:text-amber-300 font-extrabold text-xs px-3 py-1 rounded-full border border-amber-200 dark:border-amber-800">
                        Даалгавар ${q.id} (Задгай)
                    </span>
                    <span class="text-xs font-semibold text-slate-400 dark:text-slate-500 uppercase tracking-wide">
                        ${q.title}
                    </span>
                </div>
                <div class="text-base md:text-lg font-medium text-slate-900 dark:text-slate-100 mb-2 leading-relaxed">
                    ${q.text}
                </div>
                ${fieldsHTML}
                ${expHTML}
            `;
        }

        function selectMcqOption(qId, optIdx) {
            if (isSubmitted) return;
            userAnswers[qId] = optIdx;
            renderQuestions();
            renderPaletteGrid();
            updateProgress();
        }

        function handleOpenInput(qId, label, val) {
            if (isSubmitted) return;
            if (!userAnswers[qId]) userAnswers[qId] = {};
            userAnswers[qId][label] = val;
            renderPaletteGrid();
            updateProgress();
        }

        function renderPaletteGrid() {
            const grid = document.getElementById('paletteGrid');
            grid.innerHTML = '';

            currentQuestions.forEach(q => {
                let isAnswered = false;
                if (q.type === 'mcq') {
                    isAnswered = userAnswers[q.id] !== undefined;
                } else {
                    const ansObj = userAnswers[q.id] || {};
                    isAnswered = q.fields.every(f => (ansObj[f.label] || '').trim().length > 0);
                }

                if (paletteFilter === 'mcq' && q.id > 50) return;
                if (paletteFilter === 'open' && q.id <= 50) return;

                let btnClass = "bg-slate-100 dark:bg-slate-700 text-slate-700 dark:text-slate-200 border border-slate-200 dark:border-slate-600 hover:bg-slate-200 dark:hover:bg-slate-600";

                if (isSubmitted) {
                    let isCorrect = false;
                    if (q.type === 'mcq') {
                        isCorrect = userAnswers[q.id] === q.correct;
                    } else {
                        const ansObj = userAnswers[q.id] || {};
                        isCorrect = q.fields.every(f => (ansObj[f.label] || '').trim() === f.correct);
                    }

                    if (isCorrect) {
                        btnClass = "bg-emerald-500 text-white font-bold shadow-sm";
                    } else if (isAnswered) {
                        btnClass = "bg-red-500 text-white font-bold shadow-sm";
                    } else {
                        btnClass = "bg-slate-200 dark:bg-slate-700 text-slate-400 dark:text-slate-500";
                    }
                } else if (isAnswered) {
                    btnClass = "bg-blue-600 dark:bg-blue-500 text-white font-bold shadow-sm";
                }

                const btn = document.createElement('button');
                btn.className = `w-8 h-8 text-xs font-bold rounded-lg flex items-center justify-center transition ${btnClass}`;
                btn.innerText = q.id;
                btn.onclick = () => scrollToQuestion(q.id);

                grid.appendChild(btn);
            });
        }

        function filterPalette(type) {
            paletteFilter = type;
            ['all', 'mcq', 'open'].forEach(f => {
                const btn = document.getElementById(`filter_${f}`);
                if (f === type) {
                    btn.className = "flex-1 py-1 rounded-lg bg-white dark:bg-slate-800 text-slate-800 dark:text-slate-100 shadow-sm text-center font-bold";
                } else {
                    btn.className = "flex-1 py-1 rounded-lg text-slate-500 hover:text-slate-900 dark:hover:text-white text-center font-medium";
                }
            });
            renderPaletteGrid();
        }

        function scrollToQuestion(qId) {
            const el = document.getElementById(`question_card_${qId}`);
            if (el) {
                const offset = 80;
                const bodyRect = document.body.getBoundingClientRect().top;
                const elementRect = el.getBoundingClientRect().top;
                const offsetPosition = elementRect - bodyRect - offset;

                window.scrollTo({
                    top: offsetPosition,
                    behavior: 'smooth'
                });
            }
        }

        function updateProgress() {
            let answeredCount = 0;
            currentQuestions.forEach(q => {
                if (q.type === 'mcq') {
                    if (userAnswers[q.id] !== undefined) answeredCount++;
                } else {
                    const ansObj = userAnswers[q.id] || {};
                    if (q.fields.every(f => (ansObj[f.label] || '').trim().length > 0)) {
                        answeredCount++;
                    }
                }
            });

            document.getElementById('progressText').innerText = `${answeredCount} / 60`;
            const pct = Math.round((answeredCount / 60) * 100);
            document.getElementById('progressBar').style.width = `${pct}%`;
        }

        function confirmSubmit() {
            if (isSubmitted) return;
            let answeredCount = 0;
            currentQuestions.forEach(q => {
                if (q.type === 'mcq') {
                    if (userAnswers[q.id] !== undefined) answeredCount++;
                } else {
                    const ansObj = userAnswers[q.id] || {};
                    if (q.fields.every(f => (ansObj[f.label] || '').trim().length > 0)) answeredCount++;
                }
            });

            if (answeredCount < 60) {
                if (!confirm(`Та 60 даалгавраас ${answeredCount}-д нь хариулсан байна. Шалгалтыг хураалгахдаа итгэлтэй байна уу?`)) {
                    return;
                }
            }
            submitExam();
        }

        function submitExam() {
            isSubmitted = true;
            clearInterval(timerInterval);

            let correctCount = 0;
            currentQuestions.forEach(q => {
                if (q.type === 'mcq') {
                    if (userAnswers[q.id] === q.correct) correctCount++;
                } else {
                    const ansObj = userAnswers[q.id] || {};
                    const isAllFieldsCorrect = q.fields.every(f => (ansObj[f.label] || '').trim() === f.correct);
                    if (isAllFieldsCorrect) correctCount++;
                }
            });

            const percentage = Math.round((correctCount / 60) * 100);
            const scaledScore = Math.round(200 + (correctCount / 60) * 600);
            const percentile = Math.min(99, Math.round((correctCount / 60) * 98 + 1));

            document.getElementById('rawScoreDisplay').innerText = correctCount;
            document.getElementById('scaledScoreDisplay').innerText = scaledScore;
            document.getElementById('percentileDisplay').innerText = percentile;
            document.getElementById('percentageDisplay').innerText = `${percentage}%`;

            document.getElementById('resultSummary').classList.remove('hidden');
            window.scrollTo({ top: 0, behavior: 'smooth' });

            renderQuestions();
            renderPaletteGrid();
        }

        function resetExam() {
            totalSeconds = 100 * 60;
            switchVariant(currentVariant);
            startTimer();
        }

        function startTimer() {
            if (timerInterval) clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                if (totalSeconds <= 0) {
                    clearInterval(timerInterval);
                    alert("Шалгалтын хугацаа дууслаа!");
                    submitExam();
                    return;
                }
                totalSeconds--;
                const mins = Math.floor(totalSeconds / 60);
                const secs = totalSeconds % 60;
                document.getElementById('timerDisplay').innerText = `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
            }, 1000);
        }
    </script>
</body>
</html>
