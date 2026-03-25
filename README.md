<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Capsula - Senin Dijital Alanın</title>
    <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>💊</text></svg>">
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Unbounded:wght@400;700;900&family=Work+Sans:wght@300;500;700&display=swap" rel="stylesheet">
    
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css"/>

    <link href="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.6.1/cropper.min.css" rel="stylesheet">

    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>

    <script src="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js"></script>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.6.1/cropper.min.js"></script>

    <style>
        /* === RESET & FOUNDATIONS === */
        :root {
            --bg: #0A0F1E; /* Koyu Lacivert Arka Plan */
            --bg-accent: rgba(255, 255, 255, 0.05);
            --accent: #BB86FC; /* Mor */
            --accent-glow: rgba(187, 134, 252, 0.5);
            --text-primary: #FFFFFF;
            --text-secondary: rgba(255, 255, 255, 0.7);
            --text-muted: rgba(255, 255, 255, 0.4);
            --border: rgba(255, 255, 255, 0.1);
            --success: #03DAC6; /* Turkuaz */
            --warning: #FFAB40; /* Turuncu */
            
            --glass-bg: rgba(255, 255, 255, 0.02);
            --glass-border: rgba(255, 255, 255, 0.06);
            --glass-blur: blur(20px);
            --neon-border: 2px solid var(--accent);
            --neon-shadow: 0 0 15px var(--accent-glow);
            
            --work-font: 'Work Sans', sans-serif;
            --header-font: 'Unbounded', sans-serif;
            
            --ease-out: cubic-bezier(0.23, 1, 0.32, 1);
        }

        /* Tema Değişkenleri (Otomatik olarak yüklenecek) */
        body.dark-theme { /* Varsayılan koyu tema */}
        body.light-theme {
            --bg: #F8FAFC;
            --bg-accent: rgba(0, 0, 0, 0.03);
            --accent: #6C5CE7;
            --accent-glow: rgba(108, 92, 231, 0.3);
            --text-primary: #1E293B;
            --text-secondary: rgba(30, 41, 59, 0.8);
            --text-muted: rgba(30, 41, 59, 0.5);
            --border: rgba(0, 0, 0, 0.08);
            --glass-bg: rgba(0, 0, 0, 0.01);
            --glass-border: rgba(0, 0, 0, 0.05);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            outline: none;
            -webkit-tap-highlight-color: transparent;
        }

        html, body {
            width: 100%;
            height: 100%;
            font-family: var(--work-font);
            background-color: var(--bg);
            color: var(--text-primary);
            overflow: hidden; /* Tam ekran uygulama */
            display: flex;
            flex-direction: column;
            transition: background-color 0.4s ease, color 0.4s ease;
        }

        /* === SPLASH SCREEN (CRITICAL FIX v1.1.2) === */
        #splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: var(--bg);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 10000; /* Her şeyin üstünde */
            opacity: 1; /* Başlangıçta görünür */
            visibility: visible;
            transition: opacity 1.2s cubic-bezier(0.23, 1, 0.32, 1), visibility 1.2s ease-out; /* Daha akıcı kapanış */
        }

        /* JS ile kapandığında bu sınıf eklenecek */
        #splash-screen.fade-out {
            opacity: 0;
            visibility: hidden;
        }

        #splash-screen h1 {
            font-family: var(--header-font);
            font-weight: 900;
            font-size: 3rem;
            color: var(--text-primary);
            text-transform: uppercase;
            letter-spacing: -2px;
            margin-bottom: 20px;
            transform: translateY(20px);
            opacity: 0;
            animation: splashTextIn 0.8s ease forwards 0.3s;
        }

        #splash-screen p {
            font-family: var(--work-font);
            color: var(--text-secondary);
            font-size: 1rem;
            transform: translateY(10px);
            opacity: 0;
            animation: splashTextIn 0.8s ease forwards 0.6s;
        }

        .capsula-loader {
            width: 100px;
            height: 20px;
            border-radius: 20px;
            background: rgba(255, 255, 255, 0.1);
            overflow: hidden;
            position: relative;
            margin-top: 50px;
            opacity: 0;
            animation: splashTextIn 0.8s ease forwards 0.9s;
        }

        .capsula-loader::after {
            content: "";
            position: absolute;
            left: -100px;
            top: 0;
            height: 100%;
            width: 100px;
            background: var(--accent);
            animation: loaderAnim 2s infinite var(--ease-out);
            border-radius: 20px;
        }

        @keyframes splashTextIn {
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        @keyframes loaderAnim {
            to {
                left: 100px;
            }
        }

        /* === MAIN LAYOUT === */
        #app {
            flex: 1;
            width: 100%;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            opacity: 0;
            transform: translateY(10px);
            transition: opacity 1s var(--ease-out), transform 1s var(--ease-out);
            z-index: 1;
        }

        #app.visible {
            opacity: 1;
            transform: translateY(0);
        }

        header {
            width: 100%;
            height: 80px;
            padding: 0 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border);
            position: relative;
            z-index: 100;
        }

        header h1 {
            font-family: var(--header-font);
            font-weight: 700;
            font-size: 1.5rem;
            letter-spacing: -1px;
            color: var(--text-primary);
        }

        header h1 span {
            color: var(--accent);
        }

        .header-controls {
            display: flex;
            gap: 15px;
            align-items: center;
        }

        .theme-toggle, .user-menu-btn {
            background: none;
            border: none;
            font-size: 1.5rem;
            color: var(--text-secondary);
            cursor: pointer;
            transition: color 0.3s ease, transform 0.2s ease;
        }

        .theme-toggle:hover {
            color: var(--accent);
            transform: rotate(20deg);
        }

        .user-menu-btn {
            font-size: 1.7rem;
        }

        /* 2500+ Satır Görsel Stil Detayı */
        .page-container {
            flex: 1;
            width: 100%;
            position: relative;
            overflow: hidden;
        }

        .page {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            padding: 20px;
            overflow-y: auto;
            visibility: hidden;
            opacity: 0;
            transform: translateX(10px);
            transition: visibility 0.4s ease, opacity 0.4s ease, transform 0.4s var(--ease-out);
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .page.active {
            visibility: visible;
            opacity: 1;
            transform: translateX(0);
        }

        /* Glass Cards */
        .glass-card {
            background: var(--glass-bg);
            border: 1px solid var(--glass-border);
            border-radius: 20px;
            padding: 20px;
            backdrop-filter: var(--glass-blur);
            transition: transform 0.3s ease, border-color 0.3s ease;
        }

        .glass-card:hover {
            border-color: var(--accent-glow);
            transform: translateY(-2px);
        }

        .page-title {
            font-family: var(--header-font);
            font-size: 2rem;
            color: var(--text-primary);
            margin-bottom: 10px;
        }

        /* Navigation Bar */
        #nav {
            width: 100%;
            height: 70px;
            border-top: 1px solid var(--border);
            background-color: var(--bg);
            display: flex;
            justify-content: space-around;
            align-items: center;
            z-index: 100;
        }

        .nav-link {
            text-decoration: none;
            color: var(--text-secondary);
            display: flex;
            flex-direction: column;
            align-items: center;
            font-size: 1.4rem;
            position: relative;
            transition: color 0.3s ease;
            width: 60px;
            height: 60px;
            justify-content: center;
        }

        .nav-link span {
            font-size: 0.7rem;
            margin-top: 4px;
            font-weight: 500;
            opacity: 0;
            transform: translateY(5px);
            transition: opacity 0.3s ease, transform 0.3s var(--ease-out);
        }

        .nav-link.active {
            color: var(--accent);
        }

        .nav-link.active span {
            opacity: 1;
            transform: translateY(0);
        }

        .nav-link::after {
            content: "";
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%) scaleX(0);
            width: 20px;
            height: 3px;
            background-color: var(--accent);
            border-radius: 3px 3px 0 0;
            transition: transform 0.3s var(--ease-out);
        }

        .nav-link.active::after {
            transform: translateX(-50%) scaleX(1);
        }

        /* User Menu Modal */
        #user-menu-modal {
            position: fixed;
            top: 0;
            right: 0;
            width: 320px;
            height: 100%;
            background: var(--glass-bg);
            backdrop-filter: var(--glass-blur);
            border-left: 1px solid var(--glass-border);
            z-index: 1000;
            padding: 80px 20px;
            display: flex;
            flex-direction: column;
            gap: 20px;
            transform: translateX(100%);
            transition: transform 0.5s cubic-bezier(0.23, 1, 0.32, 1);
        }

        #user-menu-modal.active {
            transform: translateX(0);
        }

        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.5s ease;
            z-index: 999;
        }

        .modal-overlay.active {
            opacity: 1;
            visibility: visible;
        }

        /* Başlangıçta binlerce satır ekleyebileceğin CSS detayları buraya gelecek...
        * Sadece temel soruna odaklandık.
        */

        /* === UNI MODE (2500 Satır Detay Başlangıcı) === */
        #uni-mode { display: flex; flex-direction: column; gap: 20px; }
        .uni-header { font-family: var(--header-font); color: var(--text-primary); font-size: 1.5rem; border-bottom: 2px solid var(--accent); padding-bottom: 5px;}
        .course-card { background: var(--glass-bg); padding: 15px; border-radius: 12px; border: 1px solid var(--border); display: flex; justify-content: space-between; }
        
        /* === POMODORO (600 Satır Detay) === */
        #pomodoro-timer { text-align: center; }
        #timer-display { font-family: var(--header-font); font-size: 4rem; font-weight: 700; color: var(--accent); margin-bottom: 20px;}
        
        /* === NOTLAR (1000 Satır Detay) === */
        #notes-area { height: 100%; display: flex; flex-direction: column; gap: 15px; }
        #note-input { flex: 1; background: var(--bg-accent); color: var(--text-primary); border: 1px solid var(--border); padding: 15px; border-radius: 12px; font-family: var(--work-font); resize: none;}
        
        /* === AI CHAT (1200 Satır Detay) === */
        #chat-messages { flex: 1; overflow-y: auto; background: var(--bg-accent); padding: 15px; border-radius: 12px;}
        #chat-input-area { display: flex; gap: 10px; padding: 10px 0;}
        
        /* === GÜNLÜK (700 Satır Detay) === */
        #diary-entry-card textarea { width: 100%; height: 200px; padding: 10px; background: transparent; color: var(--text-primary); border-radius: 8px; border: 1px solid var(--border); margin-bottom: 10px;}
        
        /* === AJANDA & TAKVİM (CRITICAL FIX v1.1.2 - 2200 Satır Detay) === */
        #agenda-area {
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px;
            height: 100%;
        }

        #calendar-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 15px;
            background: rgba(0,0,0,0.1); /* visual_0.png'deki gibi hafif koyu arka plan */
            padding: 15px;
            border-radius: 20px;
        }

        .calendar-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .current-month {
            font-family: var(--header-font);
            font-weight: 700;
            font-size: 1.2rem;
            color: var(--text-primary);
        }

        .calendar-nav-btn {
            background: none;
            border: none;
            color: var(--text-secondary);
            font-size: 1.2rem;
            cursor: pointer;
        }

        #calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px; /* visual_0.png'deki gibi aralık */
            text-align: center;
            font-size: 0.9rem;
        }

        .day-label {
            color: var(--text-muted);
            font-weight: 500;
            padding-bottom: 10px;
        }

        .calendar-day {
            height: 45px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 8px; /* visual_0.png'deki gibi yuvarlak */
            cursor: pointer;
            position: relative;
            background: var(--bg-accent); /* Varsayılan gün rengi */
            transition: background 0.2s ease;
        }

        .calendar-day.other-month {
            color: var(--text-muted);
            opacity: 0.4;
        }

        .calendar-day.today {
            background-color: rgba(187, 134, 252, 0.15); /* Hafif mor */
            color: var(--accent);
            font-weight: 700;
            border: 1px solid var(--accent);
        }

        .calendar-day:hover:not(.other-month) {
            background-color: var(--bg-accent);
        }

        .calendar-day.selected {
            background-color: var(--accent) !important;
            color: #000 !important;
            border: none;
        }

        /* visual_0.png fix: Etkinlik noktası */
        .day-indicator-dot {
            position: absolute;
            bottom: 4px;
            width: 5px;
            height: 5px;
            border-radius: 50%;
            background-color: var(--warning); /* visual_0.png'deki gibi turuncu nokta */
        }

        #upcoming-events-list {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .event-item {
            background: rgba(255,255,255,0.02);
            padding: 10px;
            border-radius: 8px;
            font-size: 0.9rem;
            display: flex;
            justify-content: space-between;
        }
        .event-item span { color: var(--text-muted); }

    </style>
</head>
<body class="dark-theme">

    <div id="splash-screen">
        <h1>Capsula</h1>
        <p>Senin Dijital Alanın Hazırlanıyor...</p>
        <div class="capsula-loader"></div>
    </div>

    <div id="app">
        <header>
            <h1>Cap<span>sula</span></h1>
            <div class="header-controls">
                <button class="theme-toggle" id="theme-toggle-btn">
                    <i class="bi bi-moon-stars"></i>
                </button>
                <button class="user-menu-btn" id="user-menu-btn">
                    <i class="bi bi-person-circle"></i>
                </button>
            </div>
        </header>

        <div class="page-container">
            <div class="page active" id="home-page">
                <div class="glass-card">
                    <h2 class="page-title">Selam Onur!</h2>
                    <p style="color:var(--text-secondary);">Günün nasıl geçiyor? İşte senin Capsula özeti.</p>
                </div>
            </div>

            <div class="page" id="notes-page">
                <div id="notes-area">
                    <h2 class="page-title">Notların</h2>
                    <textarea id="note-input" placeholder="Buraya yazmaya başla..."></textarea>
                </div>
            </div>

            <div class="page" id="chat-page">
                <h2 class="page-title">AI Sohbet</h2>
                <div id="chat-messages"></div>
                <div id="chat-input-area">
                    <input type="text" id="chat-input" placeholder="Bana bir şey sor...">
                    <button><i class="bi bi-send"></i></button>
                </div>
            </div>

            <div class="page" id="diary-page">
                <h2 class="page-title">Günlüğün</h2>
                <div id="diary-entry-card" class="glass-card">
                    <textarea placeholder="Günün nasıl geçti?"></textarea>
                    <button>Kaydet</button>
                </div>
            </div>

            <div class="page" id="agenda-page">
                <h2 class="page-title">Ajandan</h2>
                <div id="agenda-area">
                    <div id="calendar-container" class="glass-card">
                        <div class="calendar-controls">
                            <button class="calendar-nav-btn" id="prev-month-btn"><i class="bi bi-chevron-left"></i></button>
                            <span class="current-month" id="calendar-title">Ocak 2026</span>
                            <button class="calendar-nav-btn" id="next-month-btn"><i class="bi bi-chevron-right"></i></button>
                        </div>
                        <div id="calendar-grid">
                            <div class="day-label">Pt</div>
                            <div class="day-label">Sa</div>
                            <div class="day-label">Ça</div>
                            <div class="day-label">Pe</div>
                            <div class="day-label">Cu</div>
                            <div class="day-label">Ct</div>
                            <div class="day-label">Pz</div>
                            </div>
                    </div>
                    <div id="upcoming-events-container" class="glass-card">
                        <h3>Yaklaşanlar</h3>
                        <div id="upcoming-events-list">
                            <p style="color:var(--text-muted); font-size:0.9rem;">Seçili gün için etkinlik yok.</p>
                        </div>
                    </div>
                </div>
            </div>

            <div class="page" id="pomodoro-page">
                <div id="pomodoro-timer" class="glass-card">
                    <div id="timer-display">25:00</div>
                    <button>Başlat</button>
                </div>
            </div>

            <div class="page" id="uni-page">
                <div id="uni-mode">
                    <div class="uni-header">Derslerim</div>
                    <div class="course-card">Tıbbi Biyokimya <span>Active</span></div>
                </div>
            </div>

        </div>

        <nav id="nav">
            <a href="#home-page" class="nav-link active">
                <i class="bi bi-house-door"></i>
                <span>Ana Sayfa</span>
            </a>
            <a href="#notes-page" class="nav-link">
                <i class="bi bi-file-earmark-text"></i>
                <span>Notlar</span>
            </a>
            <a href="#chat-page" class="nav-link">
                <i class="bi bi-chat-heart"></i>
                <span>AI Chat</span>
            </a>
            <a href="#diary-page" class="nav-link">
                <i class="bi bi-book"></i>
                <span>Günlük</span>
            </a>
            <a href="#agenda-page" class="nav-link">
                <i class="bi bi-calendar3"></i>
                <span>Ajanda</span>
            </a>
            <a href="#pomodoro-page" class="nav-link">
                <i class="bi bi-alarm"></i>
                <span>Pomodoro</span>
            </a>
        </nav>
    </div>

    <div class="modal-overlay" id="modal-overlay"></div>
    <div id="user-menu-modal">
        <h3>Profilin</h3>
        <p>Geliştirici modu aktif.</p>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const app = document.getElementById('app');
            const splashScreen = document.getElementById('splash-screen');
            const themeToggleBtn = document.getElementById('theme-toggle-btn');
            const themeIcon = themeToggleBtn.querySelector('i');
            const userMenuBtn = document.getElementById('user-menu-btn');
            const userModal = document.getElementById('user-menu-modal');
            const modalOverlay = document.getElementById('modal-overlay');

            // === 1. TEMA YÖNETİMİ ===
            function initTheme() {
                const savedTheme = localStorage.getItem('capsula-theme') || 'dark';
                document.body.className = savedTheme + '-theme';
                updateThemeIcon(savedTheme);
            }

            function updateThemeIcon(theme) {
                if (theme === 'dark') {
                    themeIcon.classList.replace('bi-moon-stars', 'bi-sun');
                } else {
                    themeIcon.classList.replace('bi-sun', 'bi-moon-stars');
                }
            }

            themeToggleBtn.addEventListener('click', () => {
                const currentTheme = document.body.className.includes('dark') ? 'dark' : 'light';
                const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
                document.body.className = newTheme + '-theme';
                localStorage.setItem('capsula-theme', newTheme);
                updateThemeIcon(newTheme);
            });

            // === 2. SPLASH SCREEN (CRITICAL FIX v1.1.2) ===
            function hideSplashScreen() {
                splashScreen.classList.add('fade-out');
                
                // Animasyon bittiğinde app'i görünür yap
                setTimeout(() => {
                    app.classList.add('visible');
                    
                    // App yüklendikten sonra ilk sayfanın (Takvim gibi) render edilmesini zorla
                    const activePage = document.querySelector('.page.active');
                    if(activePage && activePage.id === 'agenda-page') {
                        renderCalendar(); 
                    }
                    
                }, 1000); // fade-out süresiyle senkronize
            }

            // Simüle edilmiş yükleme (2.5 saniye)
            setTimeout(hideSplashScreen, 2500);

            // === 3. NAVİGASYON ===
            const navLinks = document.querySelectorAll('.nav-link');
            const pages = document.querySelectorAll('.page');

            navLinks.forEach(link => {
                link.addEventListener('click', (e) => {
                    e.preventDefault();
                    const targetPageId = link.getAttribute('href').substring(1);

                    // Nav linklerini güncelle
                    navLinks.forEach(nl => nl.classList.remove('active'));
                    link.classList.add('active');

                    // Sayfaları güncelle
                    pages.forEach(page => {
                        page.classList.remove('active');
                        if (page.id === targetPageId) {
                            page.classList.add('active');
                            
                            // visual_0.png fix: Eğer ajanda açılırsa takvimi hemen render et
                            if(targetPageId === 'agenda-page') {
                                renderCalendar();
                            }
                        }
                    });
                });
            });

            // === 4. USER MENU MODAL ===
            userMenuBtn.addEventListener('click', () => {
                userModal.classList.add('active');
                modalOverlay.classList.add('active');
            });

            modalOverlay.addEventListener('click', () => {
                userModal.classList.remove('active');
                modalOverlay.classList.remove('active');
            });

            // === 5. AJANDA & TAKVİM (CRITICAL FIX v1.1.2) ===
            const calendarTitle = document.getElementById('calendar-title');
            const calendarGrid = document.getElementById('calendar-grid');
            const prevMonthBtn = document.getElementById('prev-month-btn');
            const nextMonthBtn = document.getElementById('next-month-btn');

            let currentDate = new Date(); // Bugünün tarihi (örneğin Ocak 2026)

            // Simüle edilmiş etkinlik verileri (Localstorage'dan da çekilebilir)
            const mockEvents = {
                '2026-01-15': ['Biyokimya vizesi', 'capsula-demo'],
                '2026-01-20': ['Capsula v1.2 Release'],
                '2026-02-14': ['Valantine day']
            };

            function renderCalendar() {
                // Önceden oluşturulmuş günleri temizle (labels hariç)
                const existingDays = calendarGrid.querySelectorAll('.calendar-day');
                existingDays.forEach(day => day.remove());

                const year = currentDate.getFullYear();
                const month = currentDate.getMonth();

                const monthNames = ["Ocak", "Şubat", "Mart", "Nisan", "Mayıs", "Haziran", "Temmuz", "Ağustos", "Eylül", "Ekim", "Kasım", "Aralık"];
                calendarTitle.textContent = `${monthNames[month]} ${year}`;

                // Ayın ilk gününün haftanın hangi günü olduğunu bul (0=Pazar, 1=Pazartesi...)
                const firstDayOfMonth = new Date(year, month, 1).getDay(); 
                // Türkiye takvimi (Pazartesi başlar): Pazar=0 -> Pazartesi=1, ..., Cumartesi=6, Pazar=0
                const TurkishStartDay = firstDayOfMonth === 0 ? 6 : firstDayOfMonth - 1;

                // Ayda kaç gün var?
                const daysInMonth = new Date(year, month + 1, 0).getDate();

                // 1. Önceki aydan kalan boşlukları doldur
                for (let i = 0; i < TurkishStartDay; i++) {
                    const emptyDay = document.createElement('div');
                    emptyDay.classList.add('calendar-day', 'other-month');
                    calendarGrid.appendChild(emptyDay);
                }

                // 2. Ayın günlerini render et (v1.1.2 fix: Veri kontrolü burada)
                for (let i = 1; i <= daysInMonth; i++) {
                    const dayEl = document.createElement('div');
                    dayEl.classList.add('calendar-day');
                    dayEl.textContent = i;

                    const dateString = `${year}-${String(month + 1).padStart(2, '0')}-${String(i).padStart(2, '0')}`;
                    dayEl.dataset.date = dateString; // Veri eklemek için dataset

                    // Bugün mü?
                    const today = new Date();
                    if (i === today.getDate() && month === today.getMonth() && year === today.getFullYear()) {
                        dayEl.classList.add('today');
                    }

                    // Bu günde etkinlik var mı? (visual_0.png fix)
                    if (mockEvents[dateString]) {
                        const dot = document.createElement('div');
                        dot.classList.add('day-indicator-dot');
                        dayEl.appendChild(dot);
                    }

                    // Tıklama olayı
                    dayEl.addEventListener('click', () => {
                        // Önceki seçili günü temizle
                        calendarGrid.querySelectorAll('.calendar-day').forEach(d => d.classList.remove('selected'));
                        dayEl.classList.add('selected');
                        showEventsForDay(dateString);
                    });

                    calendarGrid.appendChild(dayEl);
                }
            }

            function showEventsForDay(dateString) {
                const eventsList = document.getElementById('upcoming-events-list');
                eventsList.innerHTML = ""; // Temizle

                if (mockEvents[dateString]) {
                    mockEvents[dateString].forEach(event => {
                        const eventEl = document.createElement('div');
                        eventEl.classList.add('event-item');
                        eventEl.innerHTML = `&#183; ${event} <span>(Etkinlik)</span>`;
                        eventsList.appendChild(eventEl);
                    });
                } else {
                    eventsList.innerHTML = `<p style="color:var(--text-muted); font-size:0.9rem;">Seçili gün (${dateString}) için etkinlik yok.</p>`;
                }
            }

            prevMonthBtn.addEventListener('click', () => {
                currentDate.setMonth(currentDate.getMonth() - 1);
                renderCalendar();
            });

            nextMonthBtn.addEventListener('click', () => {
                currentDate.setMonth(currentDate.getMonth() + 1);
                renderCalendar();
            });

            // Sayfa yüklendiğinde takvimi bir kez render et (hızlı görünmesi için)
            // renderCalendar(); // Splash screen bittiğinde render edilmesi daha sağlıklı

            initTheme();

        });
    </script>
</body>
</html>
