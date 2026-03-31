<!DOCTYPE html>
<html lang="sv">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>Kojano – spel & lärande</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: radial-gradient(circle at 20% 30%, #0a0f1e, #020408);
            font-family: 'Poppins', 'Segoe UI', system-ui, monospace;
            color: #eef4ff;
            padding: 20px 16px 60px;
        }

        .container {
            max-width: 1300px;
            margin: 0 auto;
            background: rgba(12, 20, 35, 0.7);
            backdrop-filter: blur(12px);
            border-radius: 48px;
            border: 1px solid rgba(0, 255, 255, 0.4);
            box-shadow: 0 20px 40px rgba(0,0,0,0.6), 0 0 0 2px rgba(0,255,255,0.1) inset;
            overflow: hidden;
        }

        header {
            background: linear-gradient(135deg, #0a1122, #010101);
            padding: 24px 20px;
            text-align: center;
            border-bottom: 3px solid #0ff;
            box-shadow: 0 0 12px #0ff;
            position: relative;
        }
        header h1 {
            font-size: 2.5rem;
            background: linear-gradient(135deg, #fff, #0ff, #f0f);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 8px cyan;
            letter-spacing: 2px;
        }
        .admin-toggle {
            position: absolute;
            top: 18px;
            right: 20px;
            background: #0a1a2a;
            border: 1px solid #0ff;
            border-radius: 60px;
            padding: 6px 18px;
            font-weight: bold;
            font-size: 0.8rem;
            color: #0ff;
            cursor: pointer;
            transition: 0.2s;
        }
        .admin-toggle:active { transform: scale(0.96); background: #0ff; color: #000; }
        .admin-badge {
            background: #0ff;
            color: #000;
            padding: 4px 12px;
            border-radius: 60px;
            font-size: 0.7rem;
            font-weight: bold;
            display: inline-block;
            margin-top: 8px;
        }

        nav {
            background: rgba(0, 0, 0, 0.7);
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 10px;
            padding: 14px 12px;
            border-bottom: 1px solid #0ff3;
        }
        nav button {
            background: #0e1a2b;
            border: 1px solid #2a6f8f;
            padding: 8px 20px;
            font-weight: 600;
            border-radius: 60px;
            color: #c2e9ff;
            font-size: 0.85rem;
            cursor: pointer;
            transition: 0.2s;
        }
        nav button.active {
            background: #0ff;
            color: #010c1a;
            border-color: #fff;
            box-shadow: 0 0 12px #0ff;
        }

        .tab-content {
            display: none;
            padding: 28px 24px 40px;
            animation: fadeGlow 0.25s ease;
        }
        .tab-content.active { display: block; }
        @keyframes fadeGlow {
            from { opacity: 0; transform: translateY(6px); filter: blur(1px);}
            to { opacity: 1; transform: translateY(0); filter: blur(0);}
        }

        h2 {
            font-size: 1.9rem;
            border-left: 5px solid #0ff;
            padding-left: 20px;
            margin-bottom: 24px;
            text-shadow: 0 0 3px cyan;
        }
        h3 {
            font-size: 1.3rem;
            margin: 20px 0 10px;
            color: #9efff0;
        }
        .card {
            background: rgba(10, 20, 35, 0.7);
            backdrop-filter: blur(6px);
            border-radius: 32px;
            padding: 18px 22px;
            margin-bottom: 28px;
            border: 1px solid #0ff4;
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
        }

        .alphabet-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin: 20px 0;
        }
        .letter {
            background: #101f2c;
            padding: 8px 16px;
            border-radius: 60px;
            font-weight: bold;
            font-size: 1.1rem;
            border: 1px solid #0ff6;
            font-family: monospace;
        }

        .word-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
            gap: 10px;
            margin-bottom: 30px;
        }
        .word-card {
            background: #0b1422cc;
            border-radius: 28px;
            padding: 8px 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border: 1px solid #0ff5;
        }
        .word-sv { font-weight: 700; color: #fff5c4; }
        .word-ko {
            font-family: monospace;
            background: #001122;
            padding: 3px 12px;
            border-radius: 60px;
            font-size: 0.85rem;
            border: 1px solid #0ff;
        }

        .dict-table {
            width: 100%;
            border-collapse: collapse;
            background: #07111ecc;
            border-radius: 28px;
            overflow: hidden;
        }
        .dict-table th, .dict-table td {
            padding: 10px 12px;
            text-align: left;
            border-bottom: 1px solid #0ff4;
        }
        .dict-table th { background: #0a192b; color: #0ff; }

        .search-input {
            width: 100%;
            padding: 12px 20px;
            border-radius: 60px;
            background: #0c1622;
            border: 1px solid #0ff;
            color: white;
            font-size: 1rem;
            margin-bottom: 20px;
        }

        /* spel-styling */
        .game-area {
            background: linear-gradient(145deg, #0b1424, #03070f);
            border-radius: 48px;
            padding: 20px;
            text-align: center;
            border: 2px solid #0ff;
            box-shadow: 0 0 25px rgba(0,255,255,0.3);
        }
        .game-stats {
            display: flex;
            justify-content: space-between;
            background: #000000aa;
            padding: 12px 20px;
            border-radius: 60px;
            margin-bottom: 20px;
            font-weight: bold;
            font-size: 1.2rem;
        }
        .game-question {
            font-size: 2rem;
            font-weight: 800;
            background: #000000bb;
            padding: 20px;
            border-radius: 60px;
            margin: 15px 0;
            border: 1px solid #0ff;
        }
        .game-options {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 12px;
            margin: 20px 0;
        }
        .game-option {
            background: #1f3a4b;
            border: 1px solid #0ff;
            border-radius: 60px;
            padding: 12px 24px;
            font-size: 1rem;
            cursor: pointer;
            transition: 0.1s;
            min-width: 160px;
        }
        .game-option:active { transform: scale(0.96); background: #0ff; color: black; }
        .game-feedback {
            font-size: 1.2rem;
            margin: 15px 0;
            font-weight: bold;
        }
        .game-btn {
            background: #2a5f7a;
            border: none;
            border-radius: 60px;
            padding: 10px 28px;
            margin: 8px;
            font-weight: bold;
        }
        button {
            background: #1f3a4b;
            border: none;
            border-radius: 60px;
            padding: 10px 22px;
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: 0.1s;
            border-bottom: 2px solid #0ff;
        }
        button:active { transform: scale(0.96); background: #0ff; color: #000; }
        .result-box {
            background: #091520;
            border-radius: 60px;
            padding: 16px;
            text-align: center;
            border: 1px solid #0ff;
        }
        .flex-row { display: flex; flex-wrap: wrap; gap: 12px; align-items: center; }
        footer {
            text-align: center;
            font-size: 0.7rem;
            padding: 20px;
            background: #03070f;
            border-top: 1px solid #0ff3;
        }
        @media (max-width: 600px) {
            .game-question { font-size: 1.4rem; }
            .game-option { padding: 8px 16px; min-width: 120px; }
            nav button { padding: 6px 12px; font-size: 0.75rem; }
        }
    </style>
</head>
<body>
<div class="container">
    <header>
        <h1>🎮 KOJANO – LÄROSPEL</h1>
        <p>byggspråket | lär dig genom att spela</p>
        <button id="adminLoginBtn" class="admin-toggle">👑 ADMIN</button>
        <div id="adminStatus"></div>
    </header>
    <nav>
        <button data-tab="learn">📘 ALFABET</button>
        <button data-tab="categories">📂 KATEGORIER</button>
        <button data-tab="dictionary">📖 ORDBOK</button>
        <button data-tab="game">🎮 SPEL</button>
        <button data-tab="quiz">🎯 QUIZ</button>
        <button data-tab="translate">🔍 ÖVERSÄTT</button>
        <button data-tab="adminPanel" id="adminTabBtn" style="display:none;">⚙️ ADMIN</button>
    </nav>

    <!-- ALFABET + GRAMMATIK -->
    <div id="learn" class="tab-content">
        <h2>🔤 29 BOKSTÄVER</h2>
        <div class="alphabet-grid" id="alphabetGrid"></div>
        <div class="card">
            <strong>📐 9 GRAMMATIKREGLER</strong><br>
            1. VEM – GÖR – VAD: <em>Mi konstru koj</em><br>
            2. TID: -ar (nu), -de (då), -er (framtid)<br>
            3. FLERA: +s (<em>pin → pins</em>)<br>
            4. BESTÄMD: +en/-et (<em>kojen, taktet</em>)<br>
            5. ADJEKTIV FÖRE: <em>grand koj</em><br>
            6. FRÅGA: We först (<em>We tu bygg?</em>)<br>
            7. NEJ: no före verb (<em>Mi no bygg</em>)<br>
            8. UPPMANING: -a! (<em>Bygga!</em>)<br>
            9. ÄGANDE: min, din, lis, las, nosar, vosar, lesar
        </div>
    </div>

    <!-- KATEGORIER -->
    <div id="categories" class="tab-content">
        <h2>📂 ORDLISTOR</h2>
        <div id="categoryContainer"></div>
    </div>

    <!-- ORDBOK -->
    <div id="dictionary" class="tab-content">
        <h2>📖 ALLA ORD</h2>
        <input type="text" id="dictSearch" class="search-input" placeholder="🔍 Filtrera ord...">
        <div id="dictTableContainer" style="max-height: 550px; overflow-y: auto;"></div>
        <p>✨ Totalt <span id="wordCount">0</span> ord</p>
    </div>

    <!-- SPEL (NYTT) -->
    <div id="game" class="tab-content">
        <h2>🎮 LÄR DIG GENOM SPEL</h2>
        <div class="game-area">
            <div class="game-stats">
                <span>🏆 POÄNG: <span id="gameScore">0</span></span>
                <span>⭐ FRÅGA: <span id="gameCurrent">1</span>/<span id="gameTotal">10</span></span>
                <span>🏅 BÄSTA: <span id="highScore">0</span></span>
            </div>
            <div class="game-question" id="gameQuestion">Vad betyder ...?</div>
            <div class="game-options" id="gameOptions"></div>
            <div class="game-feedback" id="gameFeedback"></div>
            <button id="gameRestartBtn" class="game-btn">🔄 NY OMGÅNG</button>
        </div>
        <div class="card" style="margin-top: 20px;">
            <strong>🎮 Så här spelar du:</strong> Du får ett svenskt ord – välj rätt Kojano-översättning bland fyra alternativ. Varje rätt svar ger 10 poäng. Försök slå ditt högsta resultat!
        </div>
    </div>

    <!-- QUIZ (vanlig) -->
    <div id="quiz" class="tab-content">
        <h2>🎯 GLOSQUIZ</h2>
        <div class="card" style="text-align:center;">
            <div class="quiz-question" id="quizWord" style="font-size:1.8rem; background:#000; padding:12px; border-radius:60px;">laddar...</div>
            <input type="text" id="quizAnswer" class="search-input" style="width:80%; margin:16px auto;" placeholder="Skriv på Kojano">
            <div><button id="checkQuizBtn">KONTROLLERA</button> <button id="newQuizBtn">🎲 NYTT ORD</button></div>
            <div id="quizFeedback" style="margin-top: 16px; font-weight: bold;"></div>
        </div>
    </div>

    <!-- ÖVERSÄTT -->
    <div id="translate" class="tab-content">
        <h2>🔍 ÖVERSÄTT</h2>
        <div class="flex-row">
            <input type="text" id="searchInput" class="search-input" placeholder="Svenska eller Kojano...">
            <button id="searchBtn">ÖVERSÄTT</button>
        </div>
        <div id="translateResult" class="result-box">⭐ Skriv ett ord</div>
    </div>

    <!-- ADMINPANEL -->
    <div id="adminPanel" class="tab-content">
        <h2>⚙️ ADMIN</h2>
        <div class="card">
            <h3>🔎 SLÅ UPP ORD</h3>
            <div class="flex-row">
                <input type="text" id="adminLookupInput" class="search-input" placeholder="Svenska ord">
                <button id="adminLookupBtn">SÖK</button>
            </div>
            <div id="adminLookupResult" class="result-box" style="margin-top: 10px;"></div>
            <hr style="margin: 20px 0; border-color:#0ff3;">
            <h3>➕ LÄGG TILL NYTT ORD</h3>
            <div style="display: flex; flex-direction: column; gap: 12px;">
                <input type="text" id="newSvWord" placeholder="Svenska ord">
                <input type="text" id="newKoWord" placeholder="Kojano-översättning">
                <button id="addWordBtn">LÄGG TILL</button>
            </div>
            <hr>
            <h3>🔐 ÄNDRA ADMIN-LÖSENORD</h3>
            <div class="flex-row">
                <input type="password" id="newAdminPwd" placeholder="Nytt lösenord">
                <button id="changePwdBtn">ÄNDRA</button>
            </div>
            <hr>
            <h3>📋 EGNA ORD</h3>
            <div id="customWordList" style="max-height: 260px; overflow-y: auto;"></div>
            <button id="resetCustomWords" style="background:#a33; margin-top: 12px;">🗑️ RADERA ALLA EGNA ORD</button>
        </div>
        <p>💡 Grundord kan inte tas bort – egna ord sparas i webbläsaren.</p>
    </div>
    <footer>🏕️ KOJANO – neon mode | lär dig & spela</footer>
</div>

<script>
    // ---------- GRUNDORDBOK (samma stora) ----------
    const baseDict = new Map();
    function addBase(sv, ko) { baseDict.set(sv.toLowerCase(), ko); baseDict.set(ko.toLowerCase(), sv); }

    // [Full ordlista från tidigare version, samma som i förra svaret]
    (function populateBase() {
        const list = [
            ["vatten","aku"],["mjölk","lakt"],["juice","saft"],["bröd","pan"],["smör","butir"],["ost","kaz"],["ägg","ägg"],["kött","köt"],
            ["fisk","fisk"],["grönsaker","grön-sak"],["frukt","söt-sak"],["äpple","epl"],["banan","banan"],["glass","glac"],["kaka","tort"],["godis","söt"],
            ["hund","kan"],["katt","kat"],["kanin","hopp"],["hamster","hamstr"],["fågel","bird"],["häst","ekv"],["ko","bov"],["gris","pork"],
            ["ekorre","skvir"],["räv","vulp"],["uggla","strig"],["måndag","lun-di"],["tisdag","mar-di"],["onsdag","merk-di"],["torsdag","jov-di"],
            ["fredag","ven-di"],["lördag","sat-di"],["söndag","sol-di"],["idag","hodie"],["igår","heri"],["imorgon","kras"],["vecka","sept"],["månad","lun"],["år","an"],
            ["noll","nul"],["ett","un"],["två","du"],["tre","tri"],["fyra","kvar"],["fem","kvin"],["sex","ses"],["sju","sep"],["åtta","okt"],["nio","nov"],
            ["tio","dek"],["tjugo","du-dek"],["trettio","tri-dek"],["fyrtio","kvar-dek"],["femtio","kvin-dek"],["sextio","ses-dek"],["sjuttio","sep-dek"],
            ["åttio","okt-dek"],["nittio","nov-dek"],["hundra","cent"],["tusen","mil"],["röd","red"],["blå","blå"],["grön","verd"],["gul","gelb"],
            ["svart","svart"],["vit","vit"],["orange","oran"],["rosa","ros"],["lila","lila"],["brun","brun"],["grå","grå"],["glad","felic"],["ledsen","tris"],
            ["arg","arg"],["rädd","tim"],["lugn","kalm"],["kär","amor"],["trött","fatig"],["pigg","energ"],["förvånad","surp"],["orolig","anx"],["ensam","sol"],
            ["modig","kuraĝ"],["huvud","kap"],["hår","har"],["öga","ok"],["öra","or"],["näsa","naz"],["mun","munt"],["tand","dent"],["arm","arm"],["hand","man"],
            ["finger","fing"],["ben","gamb"],["fot","ped"],["hjärta","kord"],["mage","venter"],["rygg","dors"],["hus","hus"],["lägenhet","flat"],["rum","rum"],
            ["kök","kök"],["sovrum","dorm-rum"],["badrum","toa-rum"],["vardagsrum","stor-rum"],["dörr","dor"],["fönster","fen"],["vägg","val"],["golv","grund"],
            ["tak","takt"],["säng","säng"],["bord","bord"],["stol","sit"],["lampa","lamp"],["nyckel","klav"]
        ];
        for(let [sv,ko] of list) addBase(sv,ko);
    })();

    // Alfabet
    const letters = ["A","AO","E","EJ","I","O","U","Y","AJ","B","CH","D","F","G","H","J","K","L","M","N","NG","P","R","S","SH","T","V","W","Z"];
    const alphabetDiv = document.getElementById("alphabetGrid");
    letters.forEach(l => { let span = document.createElement("span"); span.className = "letter"; span.innerText = l; alphabetDiv.appendChild(span); });

    // ---------- ADMIN ----------
    let ADMIN_PASSWORD = "koja123";
    function loadAdminPassword() { let s = localStorage.getItem("kojano_admin_pwd"); if(s) ADMIN_PASSWORD = s; else localStorage.setItem("kojano_admin_pwd", ADMIN_PASSWORD); }
    function setAdminPassword(p) { ADMIN_PASSWORD = p; localStorage.setItem("kojano_admin_pwd", p); }

    let customWords = [];
    function loadCustomWords() { let s = localStorage.getItem("kojano_custom"); customWords = s ? JSON.parse(s) : []; }
    function saveCustomWords() { localStorage.setItem("kojano_custom", JSON.stringify(customWords)); refreshCustomList(); rebuildFullDict(); updateAll(); refreshGameWordPool(); }

    let fullDict = new Map();
    function rebuildFullDict() {
        fullDict.clear();
        for(let [k,v] of baseDict.entries()) fullDict.set(k,v);
        for(let w of customWords) { fullDict.set(w.sv.toLowerCase(), w.ko); fullDict.set(w.ko.toLowerCase(), w.sv); }
    }
    function refreshCustomList() {
        const container = document.getElementById("customWordList");
        if(!container) return;
        container.innerHTML = "";
        if(customWords.length===0) { container.innerHTML = "<p>✨ Inga egna ord än.</p>"; return; }
        customWords.forEach((item, idx) => {
            let div = document.createElement("div"); div.className = "word-card"; div.style.marginBottom="6px";
            div.innerHTML = `<span><strong>${item.sv}</strong> → ${item.ko}</span><button class="del-custom" data-idx="${idx}" style="background:#a33; padding:4px 12px;">🗑️</button>`;
            container.appendChild(div);
        });
        document.querySelectorAll(".del-custom").forEach(btn => {
            btn.addEventListener("click", (e) => { let i = parseInt(btn.getAttribute("data-idx")); customWords.splice(i,1); saveCustomWords(); });
        });
    }
    function addCustomWord(sv, ko) {
        sv = sv.trim().toLowerCase(); ko = ko.trim().toLowerCase();
        if(!sv||!ko) return false;
        if(customWords.some(w=>w.sv===sv)) return false;
        if(baseDict.has(sv) && !confirm("⚠️ Grundord finns redan. Ändå lägga till egen variant?")) return false;
        customWords.push({sv,ko}); saveCustomWords(); return true;
    }
    function resetCustomWords() { if(confirm("❌ Radera ALLA egna ord?")) { customWords = []; saveCustomWords(); } }

    let isAdmin = false;
    function setAdminMode(admin) {
        isAdmin = admin;
        let adminTab = document.getElementById("adminTabBtn");
        let statusDiv = document.getElementById("adminStatus");
        if(isAdmin) { adminTab.style.display = "inline-block"; statusDiv.innerHTML = '<div class="admin-badge">🔓 ADMIN MODE</div>'; }
        else { adminTab.style.display = "none"; statusDiv.innerHTML = ''; if(document.getElementById("adminPanel").classList.contains("active")) switchTab("learn"); }
    }
    function promptAdminLogin() { let pwd = prompt("🔐 Ange admin-lösenord:"); if(pwd===ADMIN_PASSWORD) setAdminMode(true); else alert("❌ Fel lösenord"); }
    function changeAdminPassword() { if(!isAdmin){ alert("Endast inloggad admin"); return; } let newPwd = document.getElementById("newAdminPwd").value.trim(); if(!newPwd) return; setAdminPassword(newPwd); alert("✅ Lösenord ändrat! Logga in igen."); setAdminMode(false); document.getElementById("newAdminPwd").value=""; }

    // ---------- KATEGORIER ----------
    const categories = {
        mat: ["vatten","mjölk","juice","bröd","smör","ost","ägg","kött","fisk","grönsaker","frukt","äpple","banan","glass","kaka","godis"],
        djur: ["hund","katt","kanin","hamster","fågel","häst","ko","gris","ekorre","räv","uggla"],
        tid: ["måndag","tisdag","onsdag","torsdag","fredag","lördag","söndag","idag","igår","imorgon","vecka","månad","år"],
        siffror: ["noll","ett","två","tre","fyra","fem","sex","sju","åtta","nio","tio","tjugo","trettio","fyrtio","femtio","sextio","sjuttio","åttio","nittio","hundra","tusen"],
        färger: ["röd","blå","grön","gul","svart","vit","orange","rosa","lila","brun","grå"],
        känslor: ["glad","ledsen","arg","rädd","lugn","kär","trött","pigg","förvånad","orolig","ensam","modig"],
        kropp: ["huvud","hår","öga","öra","näsa","mun","tand","arm","hand","finger","ben","fot","hjärta","mage","rygg"],
        hem: ["hus","lägenhet","rum","kök","sovrum","badrum","vardagsrum","dörr","fönster","vägg","golv","tak","säng","bord","stol","lampa","nyckel"]
    };
    const icons = { mat:"🍎", djur:"🐾", tid:"📅", siffror:"🔢", färger:"🎨", känslor:"😊", kropp:"🦵", hem:"🏠" };
    function buildCategories() {
        const container = document.getElementById("categoryContainer");
        container.innerHTML = "";
        for(let [cat, words] of Object.entries(categories)) {
            let section = document.createElement("div");
            section.innerHTML = `<h3>${icons[cat] || "📖"} ${cat.charAt(0).toUpperCase()+cat.slice(1)}</h3><div class="word-grid" id="grid-${cat}"></div>`;
            container.appendChild(section);
            let grid = section.querySelector(`.word-grid`);
            for(let sw of words) {
                let ko = fullDict.get(sw.toLowerCase());
                if(ko) {
                    let card = document.createElement("div"); card.className = "word-card";
                    card.innerHTML = `<span class="word-sv">${sw}</span> <span class="word-ko">${ko}</span>`;
                    grid.appendChild(card);
                }
            }
        }
        if(customWords.length) {
            let customSec = document.createElement("div");
            customSec.innerHTML = `<h3>✨ EGNA ORD</h3><div class="word-grid" id="custom-grid"></div>`;
            container.appendChild(customSec);
            let grid = customSec.querySelector(".word-grid");
            for(let w of customWords) {
                let card = document.createElement("div"); card.className = "word-card";
                card.innerHTML = `<span class="word-sv">${w.sv}</span> <span class="word-ko">${w.ko}</span>`;
                grid.appendChild(card);
            }
        }
    }

    function buildDictionary() {
        let filter = document.getElementById("dictSearch").value.trim().toLowerCase();
        let entries = [];
        for(let [sv,ko] of fullDict.entries()) {
            if(sv === ko) continue;
            if(!fullDict.has(ko) || sv < ko) entries.push({sv,ko});
        }
        let unique = new Map();
        for(let e of entries) if(!unique.has(e.sv)) unique.set(e.sv, e);
        let list = Array.from(unique.values());
        if(filter) list = list.filter(e => e.sv.includes(filter) || e.ko.includes(filter));
        list.sort((a,b)=>a.sv.localeCompare(b.sv));
        document.getElementById("wordCount").innerText = list.length;
        let container = document.getElementById("dictTableContainer");
        container.innerHTML = "";
        let table = document.createElement("table"); table.className = "dict-table";
        table.innerHTML = `<thead><tr><th>Svenska</th><th>Kojano</th></tr></thead><tbody></tbody>`;
        let tbody = table.querySelector("tbody");
        for(let e of list) {
            let row = tbody.insertRow();
            row.insertCell(0).innerText = e.sv;
            row.insertCell(1).innerText = e.ko;
        }
        container.appendChild(table);
    }

    // ---------- SPEL ----------
    let gameWords = [];      // array av {sv, ko}
    let currentGameIndex = 0;
    let gameScore = 0;
    let gameTotal = 10;
    let highScore = 0;
    let gameActive = true;
    let currentQuestion = null;
    let currentOptions = [];

    function refreshGameWordPool() {
        // hämta alla svenska ord (unique)
        let svSet = new Set();
        for(let [k,v] of fullDict.entries()) {
            if(k !== v && !fullDict.has(v) && k.length > 1 && !k.includes(' ')) svSet.add(k);
        }
        let svList = Array.from(svSet);
        // blanda
        for(let i=svList.length-1;i>0;i--){ let j=Math.floor(Math.random()*(i+1)); [svList[i],svList[j]]=[svList[j],svList[i]]; }
        gameWords = svList.slice(0,50).map(sv => ({ sv: sv, ko: fullDict.get(sv) }));
    }

    function getRandomDistractors(correctKo, count=3) {
        let candidates = Array.from(fullDict.keys()).filter(k => fullDict.get(k) !== correctKo && !fullDict.has(k) && k !== correctKo && k.length>1 && k !== fullDict.get(correctKo));
        let shuffled = [...candidates];
        for(let i=shuffled.length-1;i>0;i--){ let j=Math.floor(Math.random()*(i+1)); [shuffled[i],shuffled[j]]=[shuffled[j],shuffled[i]]; }
        return shuffled.slice(0,count);
    }

    function loadGameQuestion() {
        if(!gameActive) return;
        if(currentGameIndex >= gameTotal) {
            document.getElementById("gameFeedback").innerHTML = `🎉 SPELLET KLART! Du fick ${gameScore} poäng! 🎉`;
            if(gameScore > highScore) { highScore = gameScore; localStorage.setItem("kojano_highscore", highScore); document.getElementById("highScore").innerText = highScore; }
            document.getElementById("gameQuestion").innerText = "Spelet slut! Tryck på ny omgång.";
            document.getElementById("gameOptions").innerHTML = "";
            return;
        }
        let word = gameWords[currentGameIndex % gameWords.length];
        if(!word) { gameWords = gameWords.concat(gameWords); word = gameWords[0]; }
        currentQuestion = word;
        let correct = word.ko;
        let distractors = getRandomDistractors(correct, 3);
        let options = [correct, ...distractors];
        for(let i=options.length-1;i>0;i--){ let j=Math.floor(Math.random()*(i+1)); [options[i],options[j]]=[options[j],options[i]]; }
        currentOptions = options;
        document.getElementById("gameQuestion").innerHTML = `🇸🇪 ${word.sv} → ?`;
        let optsDiv = document.getElementById("gameOptions");
        optsDiv.innerHTML = "";
        options.forEach(opt => {
            let btn = document.createElement("div");
            btn.className = "game-option";
            btn.innerText = opt;
            btn.onclick = () => handleGameAnswer(opt);
            optsDiv.appendChild(btn);
        });
        document.getElementById("gameFeedback").innerHTML = "";
    }

    function handleGameAnswer(selected) {
        if(!gameActive) return;
        if(currentGameIndex >= gameTotal) return;
        if(selected === currentQuestion.ko) {
            gameScore += 10;
            document.getElementById("gameFeedback").innerHTML = "✅ RÄTT! +10 poäng";
        } else {
            document.getElementById("gameFeedback").innerHTML = `❌ FEL! Rätt svar: ${currentQuestion.ko}`;
        }
        document.getElementById("gameScore").innerText = gameScore;
        currentGameIndex++;
        document.getElementById("gameCurrent").innerText = currentGameIndex+1 > gameTotal ? gameTotal : currentGameIndex+1;
        if(currentGameIndex >= gameTotal) {
            loadGameQuestion(); // visar slut
        } else {
            loadGameQuestion();
        }
    }

    function restartGame() {
        gameScore = 0;
        currentGameIndex = 0;
        gameActive = true;
        document.getElementById("gameScore").innerText = "0";
        document.getElementById("gameCurrent").innerText = "1";
        refreshGameWordPool();
        loadGameQuestion();
    }

    function initGame() {
        let stored = localStorage.getItem("kojano_highscore");
        highScore = stored ? parseInt(stored) : 0;
        document.getElementById("highScore").innerText = highScore;
        refreshGameWordPool();
        restartGame();
    }

    // ---------- QUIZ ----------
    let currentQuizSv = "", currentQuizKo = "";
    function randomQuizWord() {
        let svWords = [];
        for(let [key,val] of fullDict.entries()) if(key.length>1 && !key.includes(' ') && !fullDict.has(val)) svWords.push(key);
        if(!svWords.length) svWords = ["koja","bygga","pinne"];
        let rand = svWords[Math.floor(Math.random()*svWords.length)];
        currentQuizSv = rand;
        currentQuizKo = fullDict.get(rand);
        document.getElementById("quizWord").innerText = currentQuizSv;
    }
    function checkQuiz() {
        let ans = document.getElementById("quizAnswer").value.trim().toLowerCase();
        let fb = document.getElementById("quizFeedback");
        if(!ans) { fb.innerHTML = "❓ Skriv ett svar"; return; }
        if(ans === currentQuizKo) fb.innerHTML = "✅ Rätt! 🏕️";
        else fb.innerHTML = `❌ Fel! Rätt svar: ${currentQuizKo}`;
    }

    // ---------- ÖVERSÄTT ----------
    function translate() {
        let inp = document.getElementById("searchInput").value.trim().toLowerCase();
        let res = document.getElementById("translateResult");
        if(!inp) { res.innerHTML = "⭐ Skriv ett ord"; return; }
        if(fullDict.has(inp)) res.innerHTML = `<strong>${inp}</strong> → <strong style="background:#0ff3; padding:4px 12px;">${fullDict.get(inp)}</strong>`;
        else res.innerHTML = `❓ "${inp}" finns inte i ordboken`;
    }

    function adminLookup() {
        let inp = document.getElementById("adminLookupInput").value.trim().toLowerCase();
        let out = document.getElementById("adminLookupResult");
        if(!inp) { out.innerHTML = "⭐ Skriv ett svenskt ord"; return; }
        if(fullDict.has(inp)) out.innerHTML = `<strong>${inp}</strong> → ${fullDict.get(inp)}`;
        else out.innerHTML = `❓ "${inp}" saknas`;
    }

    function updateAll() { buildCategories(); buildDictionary(); if(document.getElementById("dictionary").classList.contains("active")) buildDictionary(); }

    // FLIKHANTERING
    function switchTab(tabId) {
        document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
        document.getElementById(tabId).classList.add('active');
        document.querySelectorAll('nav button').forEach(btn => btn.classList.remove('active'));
        let activeBtn = document.querySelector(`nav button[data-tab="${tabId}"]`);
        if(activeBtn) activeBtn.classList.add('active');
        if(tabId === "adminPanel" && !isAdmin) { alert("Logga in som admin först"); switchTab("learn"); }
        if(tabId === "quiz") randomQuizWord();
        if(tabId === "categories") buildCategories();
        if(tabId === "dictionary") buildDictionary();
        if(tabId === "game") { initGame(); }
    }

    // INIT
    loadAdminPassword(); loadCustomWords(); rebuildFullDict(); refreshCustomList(); updateAll(); setAdminMode(false);
    initGame();

    document.getElementById("adminLoginBtn").onclick = () => { if(isAdmin) setAdminMode(false); else promptAdminLogin(); };
    document.getElementById("addWordBtn").onclick = () => { let sv=document.getElementById("newSvWord").value, ko=document.getElementById("newKoWord").value; if(addCustomWord(sv,ko)){ alert("Ord tillagt!"); document.getElementById("newSvWord").value=""; document.getElementById("newKoWord").value=""; updateAll(); refreshGameWordPool(); } else alert("Kunde inte lägga till"); };
    document.getElementById("resetCustomWords").onclick = resetCustomWords;
    document.getElementById("searchBtn").onclick = translate;
    document.getElementById("checkQuizBtn").onclick = checkQuiz;
    document.getElementById("newQuizBtn").onclick = () => { randomQuizWord(); document.getElementById("quizAnswer").value=""; document.getElementById("quizFeedback").innerHTML=""; };
    document.getElementById("changePwdBtn").onclick = changeAdminPassword;
    document.getElementById("adminLookupBtn").onclick = adminLookup;
    document.getElementById("gameRestartBtn").onclick = () => restartGame();
    document.getElementById("dictSearch").addEventListener("input", buildDictionary);
    document.getElementById("searchInput").addEventListener("keypress", e => { if(e.key==='Enter') translate(); });
    document.getElementById("quizAnswer").addEventListener("keypress", e => { if(e.key==='Enter') checkQuiz(); });
    document.querySelectorAll("nav button[data-tab]").forEach(btn => btn.addEventListener("click", () => switchTab(btn.getAttribute("data-tab"))));
    switchTab("learn");
</script>
</body>
</html>
