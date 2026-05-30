<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>MCQ Master - Firebase</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }
        body { background: linear-gradient(145deg, #0a0f1e 0%, #0f172a 100%); min-height: 100vh; padding: 20px; }
        .app-main { max-width: 1300px; margin: 0 auto; }
        .premium-header { background: rgba(255,255,255,0.05); border-radius: 32px; padding: 20px 30px; margin-bottom: 30px; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; }
        .logo h1 { font-size: 1.8rem; background: linear-gradient(135deg, #FFD166, #FF6B6B); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .nav-buttons { display: flex; gap: 15px; }
        .nav-btn-premium { background: rgba(255,255,255,0.1); border: none; padding: 10px 24px; border-radius: 40px; color: white; font-weight: 600; cursor: pointer; }
        .nav-btn-premium.active { background: linear-gradient(135deg, #FF6B6B, #FF8E53); }
        .main-options { display: flex; gap: 20px; margin-bottom: 30px; flex-wrap: wrap; justify-content: center; }
        .main-options.hidden { display: none; }
        .option-card { flex: 1; min-width: 200px; background: linear-gradient(135deg, #1e293b, #0f172a); border-radius: 24px; padding: 30px 20px; text-align: center; cursor: pointer; border: 1px solid rgba(255,255,255,0.1); transition: 0.3s; }
        .option-card:hover { transform: translateY(-5px); background: linear-gradient(135deg, #FF6B6B, #FF8E53); }
        .option-icon { font-size: 3rem; margin-bottom: 15px; }
        .option-title { font-size: 1.5rem; font-weight: 700; color: white; }
        .option-desc { color: #94a3b8; font-size: 0.8rem; margin-top: 8px; }
        .content-panel { background: rgba(255,255,255,0.05); border-radius: 32px; padding: 30px; border: 1px solid rgba(255,255,255,0.1); min-height: 500px; }
        .content-panel.hidden { display: none; }
        .back-to-menu { background: #475569; border: none; padding: 10px 24px; border-radius: 40px; color: white; cursor: pointer; margin-bottom: 25px; }
        .subject-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 25px; margin-top: 20px; }
        .subject-card { background: rgba(255,255,255,0.08); border-radius: 28px; padding: 28px; border: 1px solid rgba(255,255,255,0.1); cursor: pointer; transition: 0.3s; }
        .subject-card:hover { transform: translateY(-5px); background: rgba(255,255,255,0.12); }
        .subject-icon { font-size: 2.5rem; margin-bottom: 12px; }
        .subject-name { font-size: 1.4rem; font-weight: 700; color: white; }
        .subject-stats { color: #cbd5e1; font-size: 0.8rem; margin-top: 8px; }
        .exam-container { background: rgba(255,255,255,0.05); border-radius: 32px; padding: 30px; border: 1px solid rgba(255,255,255,0.1); }
        .question-area { background: rgba(0,0,0,0.3); border-radius: 24px; padding: 30px; margin: 20px 0; }
        .question-text { font-size: 1.3rem; color: white; margin-bottom: 25px; }
        .option { background: rgba(255,255,255,0.08); border-radius: 16px; padding: 12px 18px; margin-bottom: 10px; cursor: pointer; color: #e2e8f0; }
        .option.correct { background: #10b981; }
        .option.wrong { background: #ef4444; }
        .exam-btn { background: #334155; border: none; padding: 10px 24px; border-radius: 40px; color: white; cursor: pointer; font-weight: 600; }
        .exam-btn.primary { background: linear-gradient(135deg, #FF6B6B, #FF8E53); }
        .score-show { background: linear-gradient(135deg, #FFD166, #FF6B6B); padding: 8px 18px; border-radius: 40px; display: inline-block; font-weight: bold; margin-bottom: 15px; }
        .result-box { background: #0f172a; padding: 20px; border-radius: 24px; text-align: center; margin-top: 20px; }
        .empty-state { text-align: center; padding: 80px 20px; color: #94a3b8; }
        .empty-state .icon { font-size: 5rem; margin-bottom: 20px; }
        .back-btn { background: #475569; border: none; padding: 8px 20px; border-radius: 30px; color: white; cursor: pointer; margin-bottom: 20px; }
        h2 { color: white; margin-bottom: 15px; }
        .admin-section { background: rgba(0,0,0,0.6); border-radius: 28px; padding: 25px; margin-top: 20px; }
        .admin-form input, .admin-form textarea, .admin-form select { width: 100%; padding: 12px; margin: 8px 0; border-radius: 16px; border: none; background: #1e293b; color: white; }
        .question-item { background: #1e293b; padding: 15px; border-radius: 16px; margin-bottom: 10px; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; }
        .hidden { display: none; }
        .flex-between { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px; }
        .loading { text-align: center; padding: 40px; color: white; font-size: 1.2rem; }
        .connection-status { background: #1e293b; padding: 10px 20px; border-radius: 40px; font-size: 0.8rem; margin-bottom: 20px; text-align: center; }
        .status-connected { color: #10b981; }
        .status-error { color: #ef4444; }
        .status-warning { color: #f59e0b; }
    </style>
</head>
<body>
<div class="app-main">
    <div class="premium-header">
        <div class="logo"><h1>☁️ MCQ Master Pro</h1><p>Firebase Cloud Database</p></div>
        <div class="nav-buttons">
            <button class="nav-btn-premium active" id="studentBtn">🎓 Student</button>
            <button class="nav-btn-premium" id="adminBtn">👑 Admin</button>
        </div>
    </div>

    <div id="connectionStatus" class="connection-status">🔄 Firebase এ সংযোগ হচ্ছে...</div>

    <div id="mainOptions" class="main-options">
        <div class="option-card" data-opt="class"><div class="option-icon">📖</div><div class="option-title">SSC Class</div><div class="option-desc">অধ্যায়ভিত্তিক পড়াশোনা</div></div>
        <div class="option-card" data-opt="suggestion"><div class="option-icon">⭐</div><div class="option-title">SSC Suggestion</div><div class="option-desc">গুরুত্বপূর্ণ সাজেশন</div></div>
        <div class="option-card" data-opt="mcq"><div class="option-icon">🎯</div><div class="option-title">MCQ Practice</div><div class="option-desc">এমসিকিউ প্র্যাকটিস</div></div>
    </div>

    <div id="classPanel" class="content-panel hidden"><button class="back-to-menu" id="backFromClass">← ৩টি অপশনে ফিরুন</button><div class="empty-state"><div class="icon">📖</div><p>শীঘ্রই আসছে...</p></div></div>
    <div id="suggestionPanel" class="content-panel hidden"><button class="back-to-menu" id="backFromSuggestion">← ৩টি অপশনে ফিরুন</button><div class="empty-state"><div class="icon">⭐</div><p>শীঘ্রই আসছে...</p></div></div>

    <div id="mcqPanel" class="content-panel hidden">
        <button class="back-to-menu" id="backFromMcq">← ৩টি অপশনে ফিরুন</button>
        <div id="mcqSubjectList"><div class="loading">📡 ডাটা লোড হচ্ছে...</div><div class="subject-grid" id="mcqSubjects"></div></div>
        <div id="mcqExamArea" class="hidden">
            <button class="back-btn" id="backToMcqSubjects">← সব সাবজেক্ট</button>
            <div class="exam-container"><h2 id="mcqSubjectTitle">Subject</h2><div class="score-show" id="liveScore">Score: 0/0</div>
            <div class="question-area"><div class="question-text" id="examQuestion">Loading...</div><div id="examOptions"></div></div>
            <div class="flex-between"><button class="exam-btn" id="prevExamBtn">◀ আগের</button><button class="exam-btn primary" id="nextExamBtn">পরবর্তী ▶</button></div>
            <div id="examResult" class="result-box hidden"></div></div>
        </div>
    </div>

    <div id="adminPanel" class="hidden">
        <div class="admin-section">
            <h2>⚙️ Admin Panel</h2>
            <div style="background:#0f172a; padding:20px; border-radius:24px; margin-bottom:30px;">
                <h3>➕ নতুন সাবজেক্ট যোগ করুন</h3>
                <input type="text" id="newSubjectId" placeholder="সাবজেক্ট আইডি" style="width:100%; padding:10px; margin:5px 0; border-radius:12px; background:#1e293b; color:white;">
                <input type="text" id="newSubjectName" placeholder="সাবজেক্ট নাম" style="width:100%; padding:10px; margin:5px 0; border-radius:12px; background:#1e293b; color:white;">
                <input type="text" id="newSubjectIcon" placeholder="আইকন" style="width:100%; padding:10px; margin:5px 0; border-radius:12px; background:#1e293b; color:white;">
                <button class="exam-btn primary" id="addSubjectBtn">✅ যোগ করুন</button>
            </div>
            <div style="background:#0f172a; padding:20px; border-radius:24px;">
                <h3>✏️ MCQ যোগ করুন</h3>
                <div class="admin-form">
                    <select id="adminSubject"></select>
                    <input type="text" id="adminQuestion" placeholder="প্রশ্ন" />
                    <input type="text" id="adminOpt1" placeholder="Option A" />
                    <input type="text" id="adminOpt2" placeholder="Option B" />
                    <input type="text" id="adminOpt3" placeholder="Option C" />
                    <input type="text" id="adminOpt4" placeholder="Option D" />
                    <select id="adminCorrect"><option value="0">A</option><option value="1">B</option><option value="2">C</option><option value="3">D</option></select>
                    <textarea id="adminExplanation" placeholder="ব্যাখ্যা"></textarea>
                    <button class="exam-btn primary" id="addMcqBtn">➕ MCQ যোগ করুন</button>
                </div>
            </div>
            <h3 style="margin-top:30px;">📋 MCQ তালিকা</h3>
            <select id="viewSubject"></select>
            <button class="exam-btn" id="loadQuestionsBtn">লোড করুন</button>
            <div id="questionsList" style="margin-top:20px;"></div>
        </div>
    </div>
</div>

<script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-database-compat.js"></script>

<script>
    // Firebase Configuration
    const firebaseConfig = {
        apiKey: "AIzaSyD3Sqrghzj2lQTEPlS4ma8XOUaUYEISzEs",
        authDomain: "mcq-striker.firebaseapp.com",
        projectId: "mcq-striker",
        storageBucket: "mcq-striker.firebasestorage.app",
        messagingSenderId: "909666274032",
        appId: "1:909666274032:web:1cd8e2c966d54659525035",
        measurementId: "G-HH1S9RVLJD"
    };

    // Initialize Firebase
    let db;
    let connected = false;

    try {
        firebase.initializeApp(firebaseConfig);
        db = firebase.database();
        console.log("Firebase initialized successfully");
        
        // Check connection
        const connectedRef = firebase.database().ref(".info/connected");
        connectedRef.on("value", (snap) => {
            if (snap.val() === true) {
                connected = true;
                document.getElementById("connectionStatus").innerHTML = '✅ Firebase এ সংযুক্ত! ডাটা লোড হচ্ছে...';
                document.getElementById("connectionStatus").style.background = '#064e3b';
                loadAllData();
            } else {
                connected = false;
                document.getElementById("connectionStatus").innerHTML = '⚠️ Firebase এ সংযোগ বিচ্ছিন্ন! ইন্টারনেট চেক করুন।';
                document.getElementById("connectionStatus").style.background = '#7c2d12';
            }
        });
    } catch(e) {
        console.error("Firebase init error:", e);
        document.getElementById("connectionStatus").innerHTML = '❌ Firebase初始化失败! কনফিগারেশন চেক করুন।';
        document.getElementById("connectionStatus").style.background = '#7f1d1d';
    }

    let subjectsList = [];
    let mcqDatabase = {};
    let currentSubject = null, currentSubjectName = null, currentQuestions = [], currentIndex = 0, userAnswers = {};

    async function loadAllData() {
        try {
            // Load subjects
            const subjectsSnapshot = await db.ref('subjects').once('value');
            if (subjectsSnapshot.exists()) {
                subjectsList = Object.values(subjectsSnapshot.val());
                console.log("Subjects loaded:", subjectsList.length);
            } else {
                subjectsList = [
                    { id: "bangla", name: "বাংলা", icon: "📖" },
                    { id: "english", name: "ইংরেজি", icon: "🇬🇧" },
                    { id: "math", name: "গণিত", icon: "🧮" }
                ];
                const subjectsObj = {};
                subjectsList.forEach((s,i)=>subjectsObj[i]=s);
                await db.ref('subjects').set(subjectsObj);
            }

            // Load MCQs
            const mcqSnapshot = await db.ref('mcqs').once('value');
            if (mcqSnapshot.exists()) {
                mcqDatabase = mcqSnapshot.val();
                console.log("MCQs loaded:", Object.keys(mcqDatabase).length);
            } else {
                mcqDatabase = {
                    bangla: [{ text: "‘সঞ্চিতা’ কাব্যগ্রন্থের রচয়িতা কে?", options: ["কাজী নজরুল ইসলাম", "সুকান্ত ভট্টাচার্য", "জীবনানন্দ দাশ", "শামসুর রাহমান"], correct: 0, explanation: "সঞ্চিতা - কাজী নজরুল ইসলাম" }],
                    english: [{ text: "Synonym of 'Beautiful'?", options: ["Ugly", "Pretty", "Bad", "Worst"], correct: 1, explanation: "Beautiful = Pretty" }],
                    math: [{ text: "x + 10 = 20, x = ?", options: ["5", "10", "15", "20"], correct: 1, explanation: "x = 10" }]
                };
                await db.ref('mcqs').set(mcqDatabase);
            }
            
            document.getElementById("connectionStatus").innerHTML = '✅ Firebase সংযুক্ত! ডাটা সফলভাবে লোড হয়েছে।';
            setTimeout(()=>{
                if(document.getElementById("connectionStatus")) 
                    document.getElementById("connectionStatus").style.display = 'none';
            }, 3000);
            
            await renderMcqSubjects();
            await refreshAllDropdowns();
            return true;
        } catch(e) {
            console.error("Error:", e);
            document.getElementById("connectionStatus").innerHTML = '❌ ডাটা লোডে সমস্যা! Firebase রুলস চেক করুন।';
            return false;
        }
    }

    async function addSubjectToFirebase(subject) {
        const s = await db.ref('subjects').once('value');
        const count = s.exists() ? Object.keys(s.val()).length : 0;
        await db.ref(`subjects/${count}`).set(subject);
    }

    async function addMcqToFirebase(subjectId, mcq) {
        const s = await db.ref(`mcqs/${subjectId}`).once('value');
        const count = s.exists() ? Object.keys(s.val()).length : 0;
        await db.ref(`mcqs/${subjectId}/${count}`).set(mcq);
    }

    async function deleteMcqFromFirebase(subjectId, idx) {
        await db.ref(`mcqs/${subjectId}/${idx}`).remove();
    }

    const mainOptions = document.getElementById("mainOptions");
    const classPanel = document.getElementById("classPanel");
    const suggestionPanel = document.getElementById("suggestionPanel");
    const mcqPanel = document.getElementById("mcqPanel");
    const adminPanel = document.getElementById("adminPanel");

    function showMenu() {
        mainOptions.classList.remove("hidden");
        classPanel.classList.add("hidden");
        suggestionPanel.classList.add("hidden");
        mcqPanel.classList.add("hidden");
    }

    function hideMenuAndShow(panelId) {
        mainOptions.classList.add("hidden");
        classPanel.classList.add("hidden");
        suggestionPanel.classList.add("hidden");
        mcqPanel.classList.add("hidden");
        document.getElementById(panelId).classList.remove("hidden");
    }

    document.querySelectorAll(".option-card").forEach(card => {
        card.onclick = () => {
            const opt = card.getAttribute("data-opt");
            if(opt === "class") hideMenuAndShow("classPanel");
            else if(opt === "suggestion") hideMenuAndShow("suggestionPanel");
            else if(opt === "mcq") { hideMenuAndShow("mcqPanel"); renderMcqSubjects(); }
        };
    });

    document.getElementById("backFromClass").onclick = showMenu;
    document.getElementById("backFromSuggestion").onclick = showMenu;
    document.getElementById("backFromMcq").onclick = showMenu;

    async function renderMcqSubjects() {
        const grid = document.getElementById("mcqSubjects");
        const loading = document.querySelector("#mcqSubjectList .loading");
        if(loading) loading.style.display = "block";
        grid.innerHTML = "";
        document.getElementById("mcqSubjectList").classList.remove("hidden");
        document.getElementById("mcqExamArea").classList.add("hidden");
        
        subjectsList.forEach(sub => {
            const qCount = mcqDatabase[sub.id]?.length || 0;
            const card = document.createElement("div");
            card.className = "subject-card";
            card.innerHTML = `<div class="subject-icon">${sub.icon}</div><div class="subject-name">${sub.name}</div><div class="subject-stats">📝 ${qCount} টি MCQ</div>`;
            card.onclick = () => startMcqExam(sub.id, sub.name);
            grid.appendChild(card);
        });
        if(loading) loading.style.display = "none";
    }

    function startMcqExam(subjectId, subjectName) {
        currentSubject = subjectId;
        currentSubjectName = subjectName;
        currentQuestions = [...(mcqDatabase[subjectId] || [])];
        currentIndex = 0;
        userAnswers = {};
        document.getElementById("mcqSubjectList").classList.add("hidden");
        document.getElementById("mcqExamArea").classList.remove("hidden");
        document.getElementById("mcqSubjectTitle").innerText = subjectName;
        updateMcqScore();
        renderMcqQuestion();
    }

    function renderMcqQuestion() {
        if(!currentQuestions.length) {
            document.getElementById("examQuestion").innerText = "❌ কোনো MCQ নেই। অ্যাডমিন প্যানেলে যোগ করুন!";
            document.getElementById("examOptions").innerHTML = "";
            return;
        }
        const q = currentQuestions[currentIndex];
        document.getElementById("examQuestion").innerText = `${currentIndex+1}. ${q.text}`;
        const optionsDiv = document.getElementById("examOptions");
        optionsDiv.innerHTML = "";
        const savedAns = userAnswers[currentIndex];
        q.options.forEach((opt, idx) => {
            const optDiv = document.createElement("div");
            optDiv.className = "option";
            optDiv.innerHTML = `${String.fromCharCode(65+idx)}. ${opt}`;
            if(savedAns !== undefined){
                if(idx === q.correct) optDiv.classList.add("correct");
                if(idx === savedAns && idx !== q.correct) optDiv.classList.add("wrong");
            }
            if(savedAns === undefined) optDiv.onclick = () => { userAnswers[currentIndex] = idx; renderMcqQuestion(); updateMcqScore(); };
            optionsDiv.appendChild(optDiv);
        });
        let expl = document.getElementById("tempExplanation");
        if(expl) expl.remove();
        if(savedAns !== undefined){
            expl = document.createElement("div");
            expl.id = "tempExplanation";
            expl.style.cssText = "background:#0f172a;padding:15px;border-radius:16px;margin-top:15px;color:#cbd5e1";
            expl.innerHTML = `💡 ব্যাখ্যা: ${currentQuestions[currentIndex].explanation || "কোন ব্যাখ্যা নেই"}`;
            optionsDiv.parentNode.appendChild(expl);
        }
    }

    function updateMcqScore() {
        let a=0,c=0;
        currentQuestions.forEach((q,i)=>{
            if(userAnswers[i]!==undefined){ a++; if(userAnswers[i]===q.correct) c++; }
        });
        document.getElementById("liveScore").innerText = `📊 Score: ${c}/${currentQuestions.length}`;
        if(a === currentQuestions.length && currentQuestions.length > 0){
            const p = (c/currentQuestions.length)*100;
            const res = document.getElementById("examResult");
            res.classList.remove("hidden");
            res.innerHTML = `<h3>🎯 পরীক্ষা শেষ!</h3><p style="font-size:1.5rem;margin:15px;">${c}/${currentQuestions.length}</p><p>নম্বর: ${p.toFixed(1)}%</p><button class="exam-btn primary" id="restartExamBtn">🔄 আবার শুরু করুন</button>`;
            document.getElementById("restartExamBtn")?.addEventListener("click",()=>{ userAnswers={}; currentIndex=0; res.classList.add("hidden"); renderMcqQuestion(); updateMcqScore(); });
        }
    }

    function nextQuestion() { if(currentIndex+1 < currentQuestions.length) currentIndex++; else if(Object.keys(userAnswers).length !== currentQuestions.length) alert("⚠️ সব প্রশ্নের উত্তর দিন!"); renderMcqQuestion(); }
    function prevQuestion() { if(currentIndex > 0) currentIndex--; renderMcqQuestion(); }

    document.getElementById("nextExamBtn").onclick = nextQuestion;
    document.getElementById("prevExamBtn").onclick = prevQuestion;
    document.getElementById("backToMcqSubjects").onclick = renderMcqSubjects;

    async function refreshAllDropdowns() {
        const selects = ["adminSubject", "viewSubject"];
        selects.forEach(id => {
            const s = document.getElementById(id);
            if(s){
                s.innerHTML = "";
                subjectsList.forEach(sub => {
                    const opt = document.createElement("option");
                    opt.value = sub.id;
                    opt.textContent = sub.name;
                    s.appendChild(opt);
                });
            }
        });
    }

    async function addNewSubject() {
        const id = document.getElementById("newSubjectId").value.trim();
        const name = document.getElementById("newSubjectName").value.trim();
        const icon = document.getElementById("newSubjectIcon").value.trim();
        if(!id || !name){ alert("আইডি ও নাম দিন!"); return; }
        if(subjectsList.find(s=>s.id===id)){ alert("এই আইডি আগে আছে!"); return; }
        const newSub = { id, name, icon: icon || "📚" };
        subjectsList.push(newSub);
        if(!mcqDatabase[id]) mcqDatabase[id] = [];
        await addSubjectToFirebase(newSub);
        await db.ref('mcqs').set(mcqDatabase);
        await renderMcqSubjects();
        await refreshAllDropdowns();
        alert("নতুন সাবজেক্ট যোগ হয়েছে!");
        document.getElementById("newSubjectId").value = "";
        document.getElementById("newSubjectName").value = "";
        document.getElementById("newSubjectIcon").value = "";
    }

    async function addSingleMCQ() {
        const sub = document.getElementById("adminSubject").value;
        const text = document.getElementById("adminQuestion").value.trim();
        const o1 = document.getElementById("adminOpt1").value.trim();
        const o2 = document.getElementById("adminOpt2").value.trim();
        const o3 = document.getElementById("adminOpt3").value.trim();
        const o4 = document.getElementById("adminOpt4").value.trim();
        const cor = parseInt(document.getElementById("adminCorrect").value);
        const exp = document.getElementById("adminExplanation").value.trim();
        if(!text || !o1 || !o2 || !o3 || !o4){ alert("সব ঘর পূরণ করুন!"); return; }
        if(!mcqDatabase[sub]) mcqDatabase[sub] = [];
        const newMcq = { text, options: [o1,o2,o3,o4], correct: cor, explanation: exp || "ব্যাখ্যা নেই" };
        mcqDatabase[sub].push(newMcq);
        await addMcqToFirebase(sub, newMcq);
        alert("MCQ যোগ হয়েছে!");
        document.getElementById("adminQuestion").value = "";
        document.getElementById("adminOpt1").value = "";
        document.getElementById("adminOpt2").value = "";
        document.getElementById("adminOpt3").value = "";
        document.getElementById("adminOpt4").value = "";
        document.getElementById("adminExplanation").value = "";
        await loadQuestionsListForAdmin();
        await renderMcqSubjects();
    }

    async function loadQuestionsListForAdmin() {
        const sub = document.getElementById("viewSubject").value;
        const qs = mcqDatabase[sub] || [];
        const cont = document.getElementById("questionsList");
        if(!qs.length){ cont.innerHTML = "<p style='color:white'>কোনো প্রশ্ন নেই</p>"; return; }
        cont.innerHTML = "";
        qs.forEach((q,idx)=>{
            const div = document.createElement("div");
            div.className = "question-item";
            div.innerHTML = `<div><strong>${idx+1}. ${q.text.substring(0,70)}</strong><br><small>✅ ${q.options[q.correct]}</small></div><button class="exam-btn del-q" data-sub="${sub}" data-idx="${idx}" style="background:#ef4444;padding:6px 16px;">Delete</button>`;
            cont.appendChild(div);
        });
        document.querySelectorAll(".del-q").forEach(btn=>{
            btn.addEventListener("click", async (e)=>{
                const sub = btn.getAttribute("data-sub");
                const idx = parseInt(btn.getAttribute("data-idx"));
                mcqDatabase[sub].splice(idx,1);
                await deleteMcqFromFirebase(sub, idx);
                await db.ref('mcqs').set(mcqDatabase);
                await loadQuestionsListForAdmin();
                await renderMcqSubjects();
                alert("Deleted");
            });
        });
    }

    document.getElementById("studentBtn").onclick = () => {
        document.getElementById("studentBtn").classList.add("active");
        document.getElementById("adminBtn").classList.remove("active");
        adminPanel.classList.add("hidden");
        showMenu();
    };
    
    document.getElementById("adminBtn").onclick = () => {
        const pwd = prompt("Admin password:");
        if(pwd === "admin123"){
            document.getElementById("adminBtn").classList.add("active");
            document.getElementById("studentBtn").classList.remove("active");
            adminPanel.classList.remove("hidden");
            mainOptions.classList.add("hidden");
            classPanel.classList.add("hidden");
            suggestionPanel.classList.add("hidden");
            mcqPanel.classList.add("hidden");
            refreshAllDropdowns();
            loadQuestionsListForAdmin();
        } else alert("ভুল পাসওয়ার্ড");
    };

    document.getElementById("addMcqBtn").onclick = addSingleMCQ;
    document.getElementById("addSubjectBtn").onclick = addNewSubject;
    document.getElementById("loadQuestionsBtn").onclick = loadQuestionsListForAdmin;
</script>
</body>
</html>
