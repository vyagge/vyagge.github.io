```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UTBK 180-Day Mastery Tracker</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts Inter & Outfit -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Outfit:wght@500;600;700;800&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f7ff',
                            100: '#e0effe',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                            900: '#1e3a8a',
                        },
                        subtest: {
                            pu: '#8b5cf6',    // Penalaran Umum (Purple)
                            ppu: '#ec4899',   // PPU (Pink)
                            pbm: '#10b981',   // PBM (Emerald)
                            pk: '#f59e0b',    // PK (Amber)
                            indo: '#06b6d4',  // Lit Indo (Cyan)
                            ing: '#3b82f6',   // Lit Eng (Blue)
                            pm: '#ef4444',    // Penmat (Red)
                            tryout: '#6366f1' // Tryout (Indigo)
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        heading: ['Outfit', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        /* Custom styles for animations and smooth UI */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0f172a;
            color: #f8fafc;
        }
        .heading-font {
            font-family: 'Outfit', sans-serif;
        }
        /* Glassmorphic elements */
        .glass-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }
        .glass-card-hover:hover {
            border-color: rgba(99, 102, 241, 0.4);
            transform: translateY(-2px);
            transition: all 0.2s ease;
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #475569;
        }
    </style>
</head>
<body class="min-h-screen bg-slate-950 text-slate-100 flex flex-col antialiased selection:bg-brand-500 selection:text-white">

    <!-- Top Navigation Header -->
    <header class="sticky top-0 z-40 bg-slate-900/80 backdrop-blur-md border-b border-slate-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-brand-600 to-indigo-500 flex items-center justify-center shadow-lg shadow-brand-500/20">
                    <i class="fa-solid fa-graduation-cap text-xl text-white"></i>
                </div>
                <div>
                    <h1 class="heading-font font-bold text-lg sm:text-xl bg-clip-text text-transparent bg-gradient-to-r from-white via-slate-200 to-slate-400">UTBK 180-Day Tracker</h1>
                    <p class="text-xs text-slate-400 hidden sm:block">Interleaving & Spaced Repetition Method</p>
                </div>
            </div>

            <div class="flex items-center gap-2 sm:gap-3">
                <!-- Day Jumper -->
                <div class="relative hidden sm:block">
                    <input type="number" id="jumpDayInput" min="1" max="180" placeholder="Ke Hari..." class="w-24 bg-slate-800/80 border border-slate-700 rounded-lg px-3 py-1.5 text-xs focus:outline-none focus:border-brand-500 transition text-slate-200">
                    <button onclick="jumpToDay()" class="absolute right-1 top-1 top-1/2 -translate-y-1/2 text-slate-400 hover:text-white p-1 text-xs">
                        <i class="fa-solid fa-arrow-right"></i>
                    </button>
                </div>

                <!-- Backup & Restore Buttons -->
                <button onclick="exportData()" title="Ekspor Data (JSON)" class="p-2 bg-slate-800 hover:bg-slate-700 text-slate-300 hover:text-white rounded-lg border border-slate-700 transition text-xs flex items-center gap-1">
                    <i class="fa-solid fa-download"></i> <span class="hidden md:inline">Ekspor</span>
                </button>
                <label title="Impor Data (JSON)" class="p-2 bg-slate-800 hover:bg-slate-700 text-slate-300 hover:text-white rounded-lg border border-slate-700 transition text-xs flex items-center gap-1 cursor-pointer">
                    <i class="fa-solid fa-upload"></i> <span class="hidden md:inline">Impor</span>
                    <input type="file" id="importFileInput" onchange="importData(event)" class="hidden" accept=".json">
                </label>
                <button onclick="confirmReset()" title="Reset Semua Progress" class="p-2 bg-rose-950/40 hover:bg-rose-900/60 text-rose-300 rounded-lg border border-rose-800/50 transition text-xs">
                    <i class="fa-solid fa-rotate-right"></i>
                </button>
            </div>
        </div>
    </header>

    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">
        
        <!-- Overall Progress & Key Stats Dashboard -->
        <section class="grid grid-cols-1 lg:grid-cols-4 gap-4">
            
            <!-- Overall Progress Box -->
            <div class="lg:col-span-2 glass-card rounded-2xl p-5 border border-slate-800 relative overflow-hidden flex flex-col justify-between">
                <div class="absolute -right-10 -bottom-10 w-40 h-40 bg-brand-600/10 rounded-full blur-3xl pointer-events-none"></div>
                <div>
                    <div class="flex justify-between items-start mb-3">
                        <div>
                            <span class="text-xs uppercase tracking-wider font-semibold text-brand-400">Target Belajar 180 Hari</span>
                            <h2 class="heading-font text-2xl sm:text-3xl font-bold text-white mt-1">Progress Utama UTBK</h2>
                        </div>
                        <span id="overallPercentage" class="heading-font text-3xl font-extrabold text-brand-400">0%</span>
                    </div>
                    
                    <!-- Progress Bar Overall -->
                    <div class="w-full bg-slate-800 h-3.5 rounded-full overflow-hidden p-0.5 border border-slate-700/50 my-3">
                        <div id="overallProgressBar" class="bg-gradient-to-r from-brand-600 via-indigo-500 to-emerald-400 h-full rounded-full transition-all duration-500" style="width: 0%"></div>
                    </div>
                </div>

                <div class="grid grid-cols-3 gap-2 pt-2 border-t border-slate-800/80 text-center">
                    <div class="bg-slate-900/50 rounded-xl p-2">
                        <span class="text-[10px] text-slate-400 uppercase font-medium block">Hari Selesai</span>
                        <span id="statCompletedDays" class="heading-font text-lg font-bold text-emerald-400">0/180</span>
                    </div>
                    <div class="bg-slate-900/50 rounded-xl p-2">
                        <span class="text-[10px] text-slate-400 uppercase font-medium block">Task Selesai</span>
                        <span id="statCompletedTasks" class="heading-font text-lg font-bold text-brand-400">0/0</span>
                    </div>
                    <div class="bg-slate-900/50 rounded-xl p-2">
                        <span class="text-[10px] text-slate-400 uppercase font-medium block">Tryout Lulus</span>
                        <span id="statCompletedTryouts" class="heading-font text-lg font-bold text-indigo-400">0/12</span>
                    </div>
                </div>
            </div>

            <!-- Learning Method Strategy Box -->
            <div class="glass-card rounded-2xl p-5 border border-slate-800 flex flex-col justify-between">
                <div>
                    <div class="flex items-center gap-2 mb-2">
                        <i class="fa-solid fa-brain text-purple-400 text-sm"></i>
                        <h3 class="heading-font font-bold text-slate-200">Strategi Aktif</h3>
                    </div>
                    <ul class="text-xs space-y-2 text-slate-300">
                        <li class="flex items-start gap-2">
                            <i class="fa-solid fa-check text-emerald-400 mt-0.5 text-[10px]"></i>
                            <span><b>Interleaving:</b> Subtes dirotasi tiap hari untuk melatih fleksibilitas kognitif.</span>
                        </li>
                        <li class="flex items-start gap-2">
                            <i class="fa-solid fa-check text-emerald-400 mt-0.5 text-[10px]"></i>
                            <span><b>Spaced Repetition:</b> Review otomatis bertahap pada interval H+3, 7, 14, 30.</span>
                        </li>
                        <li class="flex items-start gap-2">
                            <i class="fa-solid fa-check text-emerald-400 mt-0.5 text-[10px]"></i>
                            <span><b>Tryout Intensif:</b> TO berkala (B1-3: 1x/bln, B4-5: 2mgg, B6: 1mgg).</span>
                        </li>
                    </ul>
                </div>
                <div class="mt-3 pt-2 border-t border-slate-800 text-[11px] text-slate-400 italic">
                    💡 Tip: Tandai materi <span class="text-amber-400 font-semibold">Mudah</span> jika ingin gabungkan review agar waktu fokus ke materi <span class="text-rose-400 font-semibold">Sulit</span>.
                </div>
            </div>

            <!-- Quick Day Filter & Search Box -->
            <div class="glass-card rounded-2xl p-5 border border-slate-800 flex flex-col justify-between">
                <div>
                    <h3 class="heading-font font-bold text-slate-200 mb-3 flex items-center gap-2">
                        <i class="fa-solid fa-filter text-brand-400 text-sm"></i> Filter & Pencarian
                    </h3>
                    
                    <div class="space-y-2.5">
                        <!-- Search Box -->
                        <div class="relative">
                            <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 text-slate-500 text-xs"></i>
                            <input type="text" id="searchInput" oninput="renderSchedule()" placeholder="Cari materi, subtes, TO..." class="w-full bg-slate-900 border border-slate-700/70 rounded-xl pl-9 pr-3 py-2 text-xs focus:outline-none focus:border-brand-500 transition text-slate-200">
                        </div>

                        <!-- Phase Select Filter -->
                        <div>
                            <select id="phaseFilter" onchange="renderSchedule()" class="w-full bg-slate-900 border border-slate-700/70 rounded-xl px-3 py-2 text-xs focus:outline-none focus:border-brand-500 transition text-slate-200">
                                <option value="all">Semua Fase (Hari 1 - 180)</option>
                                <option value="f1">Fase 1: Konsep Dasar (Hari 1-60)</option>
                                <option value="f2">Fase 2: Pemantapan & Drilling (Hari 61-120)</option>
                                <option value="f3">Fase 3: Mastery & Intensif TO (Hari 121-180)</option>
                            </select>
                        </div>

                        <!-- Status Filter -->
                        <div>
                            <select id="statusFilter" onchange="renderSchedule()" class="w-full bg-slate-900 border border-slate-700/70 rounded-xl px-3 py-2 text-xs focus:outline-none focus:border-brand-500 transition text-slate-200">
                                <option value="all">Semua Status Hari</option>
                                <option value="completed">Hanya Selesai</option>
                                <option value="incomplete">Belum Selesai</option>
                                <option value="tryout">Hanya Hari Tryout</option>
                            </select>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Progress Breakdown 7 Subtes UTBK -->
        <section class="glass-card rounded-2xl p-5 border border-slate-800">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-2 mb-4">
                <div>
                    <h2 class="heading-font text-lg font-bold text-white flex items-center gap-2">
                        <i class="fa-solid fa-chart-pie text-brand-400"></i>
                        Progress per 7 Subtes UTBK
                    </h2>
                    <p class="text-xs text-slate-400">Kombinasi persentase Belajar Konsep, Latsol, dan Review (Spaced Repetition)</p>
                </div>
                <!-- Filter Subtest Buttons -->
                <div class="flex flex-wrap gap-1.5" id="subtestFilterContainer">
                    <!-- Dynamic Subtest Filter Badges Generated JS -->
                </div>
            </div>

            <!-- Grid Subtes Progress Cards -->
            <div id="subtestProgressGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-7 gap-3">
                <!-- Dynamically populated via JS -->
            </div>
        </section>

        <!-- Daily Schedule Section -->
        <section class="space-y-4">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3">
                <div class="flex items-center gap-3">
                    <h2 class="heading-font text-xl font-bold text-white">Jadwal Belajar Harian</h2>
                    <span id="activeFilterBadge" class="bg-brand-500/10 text-brand-400 border border-brand-500/20 text-xs px-2.5 py-0.5 rounded-full font-medium">Tampil: Semua Hari</span>
                </div>
                
                <div class="flex items-center gap-2 text-xs">
                    <button onclick="setQuickFilter('today')" class="px-3 py-1.5 bg-slate-800 hover:bg-slate-700 text-slate-200 rounded-lg border border-slate-700 transition">Hari Ini (Target)</button>
                    <button onclick="setQuickFilter('tryout')" class="px-3 py-1.5 bg-indigo-950/60 hover:bg-indigo-900/60 text-indigo-300 rounded-lg border border-indigo-800/50 transition flex items-center gap-1">
                        <i class="fa-solid fa-trophy text-amber-400"></i> Jadwal Tryout
                    </button>
                </div>
            </div>

            <!-- List Card Schedule Days -->
            <div id="scheduleGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                <!-- Cards render via JS -->
            </div>

            <!-- Empty State -->
            <div id="emptyState" class="hidden glass-card rounded-2xl p-12 text-center border border-slate-800">
                <div class="w-16 h-16 mx-auto bg-slate-800 rounded-full flex items-center justify-center text-slate-500 text-2xl mb-3">
                    <i class="fa-solid fa-filter-circle-xmark"></i>
                </div>
                <h3 class="heading-font font-bold text-slate-300 text-lg">Tidak Ada Jadwal Ditemukan</h3>
                <p class="text-xs text-slate-500 mt-1 max-w-md mx-auto">Coba ubah kata kunci pencarian atau reset filter subtes dan fase untuk melihat jadwal harian.</p>
                <button onclick="resetFilters()" class="mt-4 px-4 py-2 bg-brand-600 hover:bg-brand-500 text-white rounded-xl text-xs font-semibold transition">
                    Reset Semua Filter
                </button>
            </div>
        </section>

    </main>

    <footer class="mt-12 border-t border-slate-800/80 bg-slate-900/40 py-6 text-center text-xs text-slate-500">
        <div class="max-w-7xl mx-auto px-4">
            <p>© UTBK 180-Day Mastery System — Interleaving, Spaced Repetition & Adaptive Scheduling.</p>
            <p class="text-[11px] text-slate-600 mt-1">Konsistensi + Evaluasi Berkala = PTN Impian 🎓</p>
        </div>
    </footer>

    <script>
        /* ==========================================================================
           1. DATA CONFIGURATION & DEFINITIONS
           ========================================================================== */

        // 7 Subtes UTBK Definitions with Color Tokens and Icons
        const SUBTESTS = {
            PU: { name: 'Penalaran Umum', code: 'PU', color: 'subtest-pu', bgHex: '#8b5cf6', icon: 'fa-brain' },
            PPU: { name: 'Pengetahuan & Pemahaman Umum', code: 'PPU', color: 'subtest-ppu', bgHex: '#ec4899', icon: 'fa-book-open' },
            PBM: { name: 'Pemahaman Bacaan & Menulis', code: 'PBM', color: 'subtest-pbm', bgHex: '#10b981', icon: 'fa-pen-nib' },
            PK: { name: 'Pengetahuan Kuantitatif', code: 'PK', color: 'subtest-pk', bgHex: '#f59e0b', icon: 'fa-calculator' },
            INDO: { name: 'Literasi Bahasa Indonesia', code: 'INDO', color: 'subtest-indo', bgHex: '#06b6d4', icon: 'fa-language' },
            ING: { name: 'Literasi Bahasa Inggris', code: 'ING', color: 'subtest-ing', bgHex: '#3b82f6', icon: 'fa-globe' },
            PM: { name: 'Penalaran Matematika', code: 'PM', color: 'subtest-pm', bgHex: '#ef4444', icon: 'fa-square-root-variable' }
        };

        // Standardized Topics for 180-Day Curriculum
        const SYLLABUS = {
            PU: [
                'Penalaran Induktif & Deduktif', 'Penalaran Kuantitatif Simbolik', 'Kesesuaian Pernyataan', 
                'Logika Proposisi & Silogisme', 'Analisis Pola Deret & Gambar', 'Pengambilan Kesimpulan Teks'
            ],
            PPU: [
                'Ide Pokok & Makna Kata (Semanitk)', 'Bentuk Kata & Afiksasi', 'Kepaduan Paragraf & Hubungan Antar Kalimat', 
                'Pengelompokan Kata & Idiom', 'Pernyataan Benar/Salah Teks Panjang', 'Teks Bahasa Inggris Dasar'
            ],
            PBM: [
                'Ejaan & Tanda Baca (PUEBI/EYD)', 'Kalimat Efektif & Struktur Subjek-Predikat', 'Penggabungan Kalimat & Konjungsi', 
                'Kepaduan Paragraf & Diksi', 'Penulisan Kata Depan/Serapan', 'Suntingan Paragraf Rumpang'
            ],
            PK: [
                'Aljabar Dasar & Persamaan Linear', 'Pertidaksamaan & Nilai Mutlak', 'Aritmetika Sosial & Persentase', 
                'Geometri Bangun Datar & Ruang', 'Statistika & Peluang Dasar', 'Fungsi Kuadrat & Komposisi', 'Teori Bilangan & Operasi Hitung'
            ],
            INDO: [
                'Menemukan Informasi Tersurat/Tersirat', 'Tujuan Penulis & Sikap Penulis', 'Evaluasi Argumen & Bukti Teks', 
                'Inferensi & Asumsi Teks Opini/Ilmiah', 'Analisis Teks Sastra & Non-Sastra', 'Rangkuman Teks & Diagram'
            ],
            ING: [
                'Main Idea & Topic Sentence', 'Author\'s Tone, Purpose & Attitude', 'Inference & Restatement', 
                'Vocabulary in Context & Reference', 'Text Structure & Cohesion', 'Fact vs Opinion & Argument Ana
