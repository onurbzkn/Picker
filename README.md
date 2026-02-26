<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Picko</title>
    <style>
        :root {
            --bg-color: #0b101b;
            --accent: #00f2ff;
            --text: #ffffff;
            --die-size: 85px;
        }

        * {
            -webkit-tap-highlight-color: transparent;
            -webkit-touch-callout: none;
            user-select: none;
            outline: none;
            box-sizing: border-box;
        }

        body, html {
            margin: 0; padding: 0;
            background: var(--bg-color);
            color: var(--text);
            font-family: 'Segoe UI', system-ui, sans-serif;
            overflow: hidden;
            height: 100dvh;
            width: 100vw;
        }

        .app-wrapper {
            display: flex;
            flex-direction: column;
            height: 100%;
        }

        .content-area {
            flex: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .tab-content { 
            display: none; 
            width: 100%; 
            height: 100%; 
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
        }
        .tab-content.active { display: flex; }

        #countdown { 
            position: absolute; 
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            font-size: 6rem; 
            font-weight: 900; 
            color: var(--accent); 
            text-shadow: 0 0 30px var(--accent); 
            z-index: 100;
            pointer-events: none;
        }

        /* Navigasyon */
        .bottom-nav {
            height: 80px;
            background: rgba(15, 20, 35, 0.98);
            backdrop-filter: blur(15px);
            display: flex; justify-content: space-around; align-items: center;
            border-top: 1px solid rgba(255,255,255,0.08);
            padding-bottom: env(safe-area-inset-bottom);
        }
        .nav-item { color: #555; cursor: pointer; font-weight: bold; font-size: 11px; text-transform: uppercase; transition: 0.2s; }
        .nav-item.active { color: var(--accent); text-shadow: 0 0 15px var(--accent); }

        /* Zar ve Ok */
        .dice-wrap { display: flex; gap: 30px; justify-content: center; align-items: center; margin-top: -20px; }
        .scene { width: var(--die-size); height: var(--die-size); perspective: 600px; }
        .cube {
            width: 100%; height: 100%; position: relative; transform-style: preserve-3d;
            transform: rotateX(-30deg) rotateY(45deg);
            transition: transform 1.2s cubic-bezier(0.15, 0.85, 0.35, 1.2);
        }
        .cube-face {
            position: absolute; width: var(--die-size); height: var(--die-size);
            background: white; border: 1px solid #ddd; display: flex;
            align-items: center; justify-content: center; font-size: 2.2rem; font-weight: 900; color: #111; border-radius: 15px;
        }
        .front { transform: rotateY(0deg) translateZ(42px); }
        .back { transform: rotateY(180deg) translateZ(42px); }
        .right { transform: rotateY(90deg) translateZ(42px); }
        .left { transform: rotateY(-90deg) translateZ(42px); }
        .top { transform: rotateX(90deg) translateZ(42px); }
        .bottom { transform: rotateX(-90deg) translateZ(42px); }

        /* 1) BEYAZ DAİRE KALDIRILDI */
        #arrow {
            width: 0; height: 0; 
            border-left: 35px solid transparent; 
            border-right: 35px solid transparent;
            border-bottom: 180px solid var(--accent); 
            filter: drop-shadow(0 0 25px var(--accent));
            transition: transform 4s cubic-bezier(0.1, 0, 0.1, 1); 
            transform-origin: 50% 100%;
            margin-bottom: 120px;
        }

        .hint { 
            position: absolute; bottom: 20px; width: 100%; text-align: center;
            font-size: 10px; color: rgba(255,255,255,0.3); text-transform: uppercase; letter-spacing: 2px; 
        }

        .dice-ctrl { position: absolute; top: 30px; display: flex; gap: 10px; }
        .dice-ctrl button { background: rgba(255,255,255,0.1); color: white; border: none; padding: 12px 20px; border-radius: 12px; font-weight: 900; }

        /* 2) SETTINGS - YENİDEN AYARLANDI */
        .settings-list { width: 90%; max-width: 400px; }
        .s-row {
            background: rgba(255,255,255,0.05); padding: 18px; border-radius: 18px;
            margin-bottom: 12px; display: flex; justify-content: space-between; align-items: center;
        }
        .c-dots { display: flex; gap: 10px; }
        .c-dot { width: 25px; height: 25px; border-radius: 50%; cursor: pointer; border: 2px solid transparent; transition: 0.2s; }
        .c-dot.active { border-color: white; transform: scale(1.1); }
        
        .toggle-btn { width: 45px; height: 24px; background: #333; border-radius: 20px; position: relative; cursor: pointer; transition: 0.3s; }
        .toggle-btn::after { content: ""; position: absolute; width: 18px; height: 18px; background: white; border-radius: 50%; top: 3px; left: 4px; transition: 0.3s; }
        .toggle-btn.on { background: var(--accent); }
        .toggle-btn.on::after { left: 23px; }

        select { background: #222; color: white; border: 1px solid #444; padding: 8px; border-radius: 8px; font-size: 12px; }
    </style>
</head>
<body>

    <div class="app-wrapper">
        <div class="content-area">
            <div id="countdown"></div>

            <div id="finger" class="tab-content active">
                <div id="finger-area" style="width:100%; height:100%; touch-action:none;"></div>
                <div class="hint" data-key="hint_finger">EN AZ 2 PARMAKLA DOKUN</div>
            </div>

            <div id="dice" class="tab-content" onclick="rollDice()">
                <div class="dice-ctrl">
                    <button onclick="event.stopPropagation(); setDice(1)" data-key="dice_1">1 ZAR</button>
                    <button onclick="event.stopPropagation(); setDice(2)" data-key="dice_2">2 ZAR</button>
                    <button onclick="event.stopPropagation(); setDice(3)" data-key="dice_3">3 ZAR</button>
                </div>
                <div id="dice-display" class="dice-wrap"></div>
                <div class="hint" data-key="hint_dice">ATMAK İÇİN DOKUNUN</div>
            </div>

            <div id="flow" class="tab-content" onclick="spinArrow()">
                <div id="arrow"></div>
                <div class="hint" data-key="hint_flow">DÖNDÜRMEK İÇİN DOKUNUN</div>
            </div>

            <div id="settings" class="tab-content">
                <div class="settings-list">
                    <div class="s-row">
                        <span data-key="set_color">Tema Rengi</span>
                        <div class="c-dots">
                            <div onclick="setT('#00f2ff')" class="c-dot active" style="background:#00f2ff;"></div>
                            <div onclick="setT('#00ff88')" class="c-dot" style="background:#00ff88;"></div>
                            <div onclick="setT('#ff00ea')" class="c-dot" style="background:#ff00ea;"></div>
                            <div onclick="setT('#ff9500')" class="c-dot" style="background:#ff9500;"></div>
                        </div>
                    </div>
                    <div class="s-row">
                        <span data-key="set_vibrate">Titreşim</span>
                        <div class="toggle-btn on" onclick="this.classList.toggle('on'); config.vibrate = !config.vibrate;"></div>
                    </div>
                    <div class="s-row">
                        <span data-key="set_sound">Ses</span>
                        <div class="toggle-btn on" onclick="this.classList.toggle('on'); config.sound = !config.sound;"></div>
                    </div>
                    <div class="s-row">
                        <span data-key="set_lang">Dil / Language</span>
                        <select onchange="changeLang(this.value)">
                            <option value="tr">Türkçe</option>
                            <option value="en">English</option>
                            <option value="es">Español</option>
                            <option value="de">Deutsch</option>
                            <option value="fr">Français</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>

        <nav class="bottom-nav">
            <div class="nav-item active" onclick="showTab('finger', this)" data-key="nav_finger">Finger</div>
            <div class="nav-item" onclick="showTab('dice', this)" data-key="nav_dice">Dice</div>
            <div class="nav-item" onclick="showTab('flow', this)" data-key="nav_flow">Flow</div>
            <div class="nav-item" onclick="showTab('settings', this)" data-key="nav_settings">Settings</div>
        </nav>
    </div>

    <script>
        const config = { vibrate: true, sound: true, lang: 'tr' };
        const langs = {
            tr: { hint_finger: "EN AZ 2 PARMAKLA DOKUN", hint_dice: "ATMAK İÇİN DOKUNUN", hint_flow: "DÖNDÜRMEK İÇİN DOKUNUN", dice_1: "1 ZAR", dice_2: "2 ZAR", dice_3: "3 ZAR", set_color: "Tema Rengi", set_vibrate: "Titreşim", set_sound: "Ses", set_lang: "Dil", nav_finger: "Finger", nav_dice: "Dice", nav_flow: "Flow", nav_settings: "Settings" },
            en: { hint_finger: "TOUCH WITH AT LEAST 2 FINGERS", hint_dice: "TOUCH TO ROLL", hint_flow: "TOUCH TO SPIN", dice_1: "1 DIE", dice_2: "2 DICE", dice_3: "3 DICE", set_color: "Theme Color", set_vibrate: "Vibration", set_sound: "Sound", set_lang: "Language", nav_finger: "Finger", nav_dice: "Dice", nav_flow: "Flow", nav_settings: "Settings" },
            es: { hint_finger: "TOCA CON AL MENOS 2 DEDOS", hint_dice: "TOCA PARA TIRAR", hint_flow: "TOCA PARA GIRAR", dice_1: "1 DADO", dice_2: "2 DADOS", dice_3: "3 DADOS", set_color: "Color del tema", set_vibrate: "Vibración", set_sound: "Sonido", set_lang: "Idioma", nav_finger: "Dedo", nav_dice: "Dados", nav_flow: "Giro", nav_settings: "Ajustes" },
            de: { hint_finger: "MIT MINDESTENS 2 FINGERN BERÜHREN", hint_dice: "ZUM WÜRFELN BERÜHREN", hint_flow: "ZUM DREHEN BERÜHREN", dice_1: "1 WÜRFEL", dice_2: "2 WÜRFEL", dice_3: "3 WÜRFEL", set_color: "Themenfarbe", set_vibrate: "Vibration", set_sound: "Ton", set_lang: "Sprache", nav_finger: "Finger", nav_dice: "Würfel", nav_flow: "Fluss", nav_settings: "Einstellungen" },
            fr: { hint_finger: "TOUCHER AVEC AU MOINS 2 DOIGTS", hint_dice: "TOUCHER POUR LANCER", hint_flow: "TOUCHER POUR TOURNER", dice_1: "1 DÉ", dice_2: "2 DÉS", dice_3: "3 DÉS", set_color: "Couleur du thème", set_vibrate: "Vibration", set_sound: "Son", set_lang: "Langue", nav_finger: "Doigt", nav_dice: "Dés", nav_flow: "Flux", nav_settings: "Réglages" }
        };

        function changeLang(l) {
            config.lang = l;
            document.querySelectorAll('[data-key]').forEach(el => {
                const key = el.getAttribute('data-key');
                if (langs[l][key]) el.innerText = langs[l][key];
            });
        }

        function showTab(id, el) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            el.classList.add('active');
            if(id === 'dice') setDice(1);
        }

        function setT(c) { 
            document.documentElement.style.setProperty('--accent', c);
            document.querySelectorAll('.c-dot').forEach(d => d.classList.remove('active'));
            event.target.classList.add('active');
        }

        function playS(t) {
            if(!config.sound) return;
            const ctx = new (window.AudioContext || window.webkitAudioContext)();
            const o = ctx.createOscillator(); const g = ctx.createGain();
            o.connect(g); g.connect(ctx.destination);
            if(t === 'win') { o.frequency.setValueAtTime(440, ctx.currentTime); o.frequency.exponentialRampToValueAtTime(880, ctx.currentTime+0.2); g.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime+0.3); o.start(); o.stop(ctx.currentTime+0.3); }
            else { o.type='square'; o.frequency.setValueAtTime(150, ctx.currentTime); g.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime+0.1); o.start(); o.stop(ctx.currentTime+0.1); }
        }

        /* Logic */
        const fA = document.getElementById('finger-area'); const cE = document.getElementById('countdown');
        let ts = {}, tm = null, fi = false;
        fA.addEventListener('touchstart', e => {
            if(fi) { fA.innerHTML = ""; ts = {}; fi = false; }
            Array.from(e.changedTouches).forEach(t => {
                const d = document.createElement('div'); d.className = 'touch-circle'; d.id = 't-'+t.identifier;
                d.style.cssText = `position:absolute; width:90px; height:90px; border:5px solid var(--accent); border-radius:50%; transform:translate(-50%,-50%); box-shadow:0 0 25px var(--accent); left:${t.pageX}px; top:${t.pageY}px;`;
                fA.appendChild(d); ts[t.identifier] = d;
            });
            if(Object.keys(ts).length > 1 && !tm) {
                let s = 3; cE.innerText = s;
                tm = setInterval(() => { s--; cE.innerText = s || ""; if(s<=0) {
                    clearInterval(tm); tm = null; fi = true; playS('win');
                    const ks = Object.keys(ts); const w = ks[Math.floor(Math.random()*ks.length)];
                    ks.forEach(k => { if(k===w){ts[k].style.borderColor='#00ff88'; ts[k].style.boxShadow='0 0 50px #00ff88'; ts[k].style.transform='translate(-50%,-50%) scale(1.3)';} else {ts[k].style.borderColor='#ff3366'; ts[k].style.opacity='0.2';}});
                    cE.innerText = "!"; if(config.vibrate && window.navigator.vibrate) window.navigator.vibrate([100,50,100]);
                }}, 1000);
            }
        });
        fA.addEventListener('touchmove', e => { e.preventDefault(); Array.from(e.changedTouches).forEach(t => { const d = document.getElementById('t-'+t.identifier); if(d){d.style.left=t.pageX+'px'; d.style.top=t.pageY+'px';}}); }, {passive:false});
        fA.addEventListener('touchend', e => { Array.from(e.changedTouches).forEach(t => { const d = document.getElementById('t-'+t.identifier); if(d) d.remove(); delete ts[t.identifier]; }); if(Object.keys(ts).length<2){clearInterval(tm); tm=null; cE.innerText="";}});

        function setDice(n) { const d = document.getElementById('dice-display'); d.innerHTML = ""; for(let i=0; i<n; i++) { const s = document.createElement('div'); s.className='scene'; s.innerHTML=`<div class="cube" id="cube-${i}"><div class="cube-face front">1</div><div class="cube-face back">6</div><div class="cube-face right">3</div><div class="cube-face left">4</div><div class="cube-face top">2</div><div class="cube-face bottom">5</div></div>`; d.appendChild(s); }}
        function rollDice() { playS('roll'); if(config.vibrate && window.navigator.vibrate) window.navigator.vibrate(50); document.querySelectorAll('.cube').forEach(c => { const r = Math.floor(Math.random()*6)+1; const ex = (Math.floor(Math.random()*4)+4)*360; const ey = (Math.floor(Math.random()*4)+4)*360; let x=0, y=0; switch(r){case 1:x=0;y=0;break;case 2:x=-90;y=0;break;case 3:x=0;y=-90;break;case 4:x=0;y=90;break;case 5:x=90;y=0;break;case 6:x=0;y=180;break;} c.style.transform = `rotateX(${x+ex}deg) rotateY(${y+ey}deg)`; }); }
        let rt = 0; function spinArrow() { playS('roll'); if(config.vibrate && window.navigator.vibrate) window.navigator.vibrate(20); rt += 2000 + Math.floor(Math.random()*2000); document.getElementById('arrow').style.transform = `rotate(${rt}deg)`; }
        setDice(1);
    </script>
</body>
</html>

