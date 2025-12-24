<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>FlowPro | Sovereign V12</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        /* 🎨 1. THE COLOR SYSTEM */
        :root {
            --bg: #05070a;
            --card: rgba(255, 255, 255, 0.03);
            --card-border: rgba(255, 255, 255, 0.08);
            --text-main: #f8fafc;
            --text-dim: #64748b;
            
            --primary: #00ffd5;  /* Flow */
            --break: #bb86fc;    /* Recovery */
            --warn: #ffcc00;     /* Locked */
            --danger: #ff5e57;   /* Interrupted */
            
            --status-color: var(--text-dim);
        }

        /* 🧠 2. FONT & BASE */
        * { 
            box-sizing: border-box; 
            font-family: 'Inter', system-ui, -apple-system, sans-serif; 
            -webkit-font-smoothing: antialiased;
        }

        body { 
            margin: 0; 
            background: var(--bg); 
            color: var(--text-main); 
            display: flex; 
            justify-content: center; 
            min-height: 100vh; 
            padding: 24px;
            transition: background 0.5s ease;
        }

        .app { width: 100%; max-width: 400px; display: flex; flex-direction: column; gap: 24px; }

        /* 🔤 3. UI COMPONENTS */
        .header { display: flex; justify-content: space-between; align-items: center; padding: 0 4px; }
        .logo { font-size: 0.7rem; font-weight: 900; letter-spacing: 4px; color: var(--text-dim); text-transform: uppercase; }
        .streak { font-size: 0.7rem; font-weight: 700; color: var(--warn); }

        .kpi-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
        .kpi-card { 
            background: var(--card); 
            border: 1px solid var(--card-border); 
            border-radius: 14px; 
            padding: 16px; 
        }
        .kpi-label { 
            font-size: 0.55rem; 
            font-weight: 700; 
            color: var(--text-dim); 
            text-transform: uppercase; 
            letter-spacing: 1.5px; 
            margin-bottom: 6px; 
            display: block; 
        }
        .kpi-val { font-size: 1.2rem; font-weight: 800; font-variant-numeric: tabular-nums; }

        .timer-module { 
            background: var(--card); 
            border: 1px solid var(--card-border); 
            border-radius: 20px; 
            padding: 48px 24px; 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            position: relative;
        }
        
        .status-tag { 
            font-size: 0.65rem; 
            font-weight: 900; 
            color: var(--status-color); 
            text-transform: uppercase; 
            letter-spacing: 3px; 
            margin-bottom: 12px;
            transition: color 0.3s ease;
        }
        
        .timer-display { 
            font-size: 6rem; 
            font-weight: 800; 
            line-height: 1; 
            letter-spacing: -5px; 
            margin: 10px 0; 
            font-variant-numeric: tabular-nums; 
        }
        
        .buyin-container { width: 100%; margin: 24px 0; text-align: center; }
        .buyin-bar { width: 100%; height: 4px; background: rgba(255,255,255,0.05); border-radius: 2px; overflow: hidden; margin-bottom: 10px; }
        .buyin-progress { height: 100%; background: var(--status-color); width: 0%; transition: width 0.4s linear; }
        .buyin-text { font-size: 0.6rem; font-weight: 700; color: var(--text-dim); text-transform: uppercase; letter-spacing: 1px; }

        .btn { 
            border: none; 
            padding: 20px; 
            border-radius: 14px; 
            font-weight: 900; 
            font-size: 0.8rem; 
            cursor: pointer; 
            transition: 0.3s ease; 
            text-transform: uppercase; 
            width: 100%; 
            letter-spacing: 1px;
        }
        .btn-primary { background: var(--status-color, var(--text-main)); color: #000; }
        .btn-secondary { background: rgba(255,255,255,0.05); color: var(--text-main); border: 1px solid var(--card-border); margin-top: 12px; }
        .btn:disabled { opacity: 0.2; cursor: not-allowed; }

        .lock-notice { 
            font-size: 0.6rem; 
            color: var(--danger); 
            font-weight: 700; 
            margin-top: 12px; 
            height: 1em; 
            text-transform: uppercase; 
            text-align: center;
            letter-spacing: 1px;
        }

        .analytics-card { 
            background: var(--card); 
            border: 1px solid var(--card-border); 
            border-radius: 20px; 
            padding: 20px; 
        }
        .chart-container { height: 64px; display: flex; align-items: flex-end; justify-content: space-between; gap: 8px; margin: 20px 0; }
        .bar-wrapper { flex: 1; height: 100%; display: flex; flex-direction: column; justify-content: flex-end; position: relative; cursor: help; }
        .bar { width: 100%; background: var(--primary); border-radius: 3px; opacity: 0.2; transition: 0.3s ease; }
        .bar-wrapper:hover .bar { opacity: 1; }
        
        .heatmap { display: grid; grid-template-columns: repeat(7, 1fr); gap: 6px; margin-top: 12px; }
        .day { aspect-ratio: 1; background: rgba(255,255,255,0.05); border-radius: 4px; position: relative; cursor: help; transition: 0.3s ease; }

        [data-tip]:hover::after {
            content: attr(data-tip); position: absolute; bottom: 135%; left: 50%; transform: translateX(-50%);
            background: #1a1d23; color: #fff; padding: 6px 10px; border-radius: 8px; font-size: 10px;
            white-space: nowrap; border: 1px solid var(--card-border); z-index: 10;
        }

        .pip-row { display: flex; gap: 8px; margin-top: 32px; }
        .pip { width: 42px; height: 4px; border-radius: 2px; background: var(--card-border); transition: 0.5s ease; }
        .pip.active { background: var(--primary); box-shadow: 0 0 12px var(--primary); }
    </style>
</head>
<body>

<div class="app">
    <div class="header">
        <div class="logo">Sovereign V12</div>
        <div id="streak" class="streak">🔥 0 DAY STREAK</div>
    </div>

    <div class="kpi-grid">
        <div class="kpi-card">
            <span class="kpi-label">Neural Depth</span>
            <span id="depthVal" class="kpi-val">0%</span>
        </div>
        <div class="kpi-card">
            <span class="kpi-label">Cycle Phase</span>
            <span id="cycleVal" class="kpi-val">0/4</span>
        </div>
    </div>

    <div class="timer-module">
        <div id="modeTag" class="status-tag">Standby</div>
        <div id="timerDisplay" class="timer-display">00:00</div>
        
        <div class="buyin-container">
            <div class="buyin-bar"><div id="buyinProgress" class="buyin-progress"></div></div>
            <div id="buyinText" class="buyin-text">Ready</div>
        </div>

        <button id="mainBtn" class="btn btn-primary" onclick="handleAction()">Commit to Flow</button>
        <button id="pauseBtn" class="btn btn-secondary" style="display:none" onclick="togglePause()">Pause Session</button>
        <div id="lockNotice" class="lock-notice"></div>

        <div class="pip-row">
            <div class="pip" id="pip1"></div><div class="pip" id="pip2"></div><div class="pip" id="pip3"></div><div class="pip" id="pip4"></div>
        </div>
    </div>

    <div class="analytics-card">
        <span class="kpi-label">7-Day Momentum</span>
        <div id="trendChart" class="chart-container"></div>
        <span class="kpi-label" style="margin-top: 24px;">Monthly Heatmap</span>
        <div id="heatmap" class="heatmap"></div>
    </div>
</div>

<audio id="audioBeep" src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg"></audio>

<script>
    const STORAGE_KEY = 'flowpro_v12_sovereign';
    let state = JSON.parse(localStorage.getItem(STORAGE_KEY)) || { 
        history: [], daily: {}, sessionIdx: 0, streak: 0, lastDate: null, 
        failedToday: 0, pauseCount: 0, breakUntil: null, lastActivity: null 
    };

    let mode = 'idle', totalSec = 0, startTime = null, timerInt = null, paused = false;
    let isLocked = false;

    // Check for expired breaks on load
    if (state.breakUntil && Date.now() > state.breakUntil) {
        state.breakUntil = null;
        state.sessionIdx = state.sessionIdx >= 4 ? 0 : state.sessionIdx;
    }

    function handleAction() {
        if (['idle','break_over','rejected'].includes(mode)) startWork();
        else if (mode === 'work') endWork();
    }

    function startWork() {
        mode = 'work'; paused = false; totalSec = 0;
        startTime = Date.now();
        const unlockAt = Date.now() + 60000; 
        isLocked = true;
        
        const mBtn = document.getElementById('mainBtn');
        const pBtn = document.getElementById('pauseBtn');
        const lockNotice = document.getElementById('lockNotice');
        
        mBtn.textContent = "Exit Flow";
        mBtn.disabled = true;
        pBtn.disabled = true;

        const lockInt = setInterval(() => {
            const remaining = Math.ceil((unlockAt - Date.now()) / 1000);
            if (remaining > 0) {
                lockNotice.textContent = `Exit unlocks in ${remaining}s — committing attention`;
            } else {
                lockNotice.textContent = "";
                mBtn.disabled = false;
                pBtn.disabled = false;
                isLocked = false;
                clearInterval(lockInt);
            }
        }, 100);

        pBtn.style.display = 'block';
        updateStateUI('work_locked');
        runTimer();
    }

    function togglePause() {
        if (mode !== 'work' || isLocked) return;
        paused = !paused;
        if (paused) {
            state.pauseCount++;
            const sessionElapsed = Math.floor((Date.now() - startTime) / 1000);
            trackWork(sessionElapsed);
            totalSec += sessionElapsed;
            clearInterval(timerInt);
            document.getElementById('pauseBtn').textContent = "Resume Session";
            updateStateUI('paused');
        } else {
            startTime = Date.now();
            runTimer();
            document.getElementById('pauseBtn').textContent = "Pause Session";
            updateStateUI('work_flow');
        }
        save(); render();
    }

    function runTimer() {
        clearInterval(timerInt);
        timerInt = setInterval(() => {
            if (mode === 'break') { 
                let rem = Math.round((state.breakUntil - Date.now()) / 1000);
                if (rem <= 0) endBreak(); 
                updateDisplay(rem); 
            } else if (mode === 'work' && !paused) { 
                let elapsed = totalSec + Math.floor((Date.now() - startTime) / 1000);
                updateDisplay(elapsed); 
                updateBuyinUI(elapsed);
                // Increment tracking every 10s for accuracy
                if (elapsed > 0 && elapsed % 10 === 0) trackWork(1);
            }
        }, 1000);
    }

    function trackWork(seconds) {
        const today = new Date().toISOString().split('T')[0];
        state.daily[today] = (state.daily[today] || 0) + seconds;
    }

    function updateBuyinUI(s) {
        if (mode !== 'work') return;
        const prog = document.getElementById('buyinProgress');
        const txt = document.getElementById('buyinText');
        if (s < 300) {
            const rem = 300 - s;
            prog.style.width = `${(s / 300) * 100}%`;
            txt.textContent = `${Math.floor(rem/60)}:${(rem%60).toString().padStart(2,'0')} to Buy-in`;
            if(!isLocked) updateStateUI('work_locked');
        } else {
            prog.style.width = '100%';
            txt.textContent = `Gate Open`;
            updateStateUI('work_flow');
        }
    }

    function updateStateUI(status) {
        const root = document.documentElement;
        const tag = document.getElementById('modeTag');
        switch(status) {
            case 'work_locked': root.style.setProperty('--status-color', 'var(--warn)'); tag.textContent = "Neural Gate Locked"; break;
            case 'work_flow': root.style.setProperty('--status-color', 'var(--primary)'); tag.textContent = "Cognitive Coherence"; break;
            case 'paused': root.style.setProperty('--status-color', 'var(--danger)'); tag.textContent = "Cognitive Rupture"; break;
            case 'break': root.style.setProperty('--status-color', 'var(--break)'); tag.textContent = "Neural Recovery"; break;
            case 'idle': root.style.setProperty('--status-color', 'var(--text-dim)'); tag.textContent = "System Standby"; break;
        }
    }

    function endWork() {
        clearInterval(timerInt);
        const curSessionTime = !paused ? Math.floor((Date.now() - startTime) / 1000) : 0;
        trackWork(curSessionTime);
        totalSec += curSessionTime;
        
        if (totalSec < 300) {
            state.failedToday++;
            if (state.failedToday >= 2) state.streak = Math.max(0, state.streak - 1);
            mode = 'rejected';
            updateStateUI('idle');
            document.getElementById('mainBtn').textContent = "Restart Flow";
            document.getElementById('pauseBtn').style.display = 'none';
        } else {
            const earned = Math.min(900, Math.floor(totalSec / 5));
            state.history.unshift({ work: totalSec, date: Date.now() });
            state.sessionIdx++;
            checkStreak();
            startBreak(state.sessionIdx >= 4 ? 1800 : earned);
        }
        save(); render();
    }

    function startBreak(dur) {
        mode = 'break';
        state.breakUntil = Date.now() + (dur * 1000);
        updateStateUI('break');
        document.getElementById('mainBtn').disabled = true;
        document.getElementById('pauseBtn').style.display = 'none';
        save(); runTimer();
    }

    function endBreak() {
        clearInterval(timerInt);
        if (state.sessionIdx >= 4) state.sessionIdx = 0;
        mode = 'break_over';
        state.breakUntil = null;
        document.getElementById('audioBeep').play();
        document.getElementById('mainBtn').disabled = false;
        document.getElementById('mainBtn').textContent = "Begin Cycle";
        updateStateUI('idle');
        save(); render();
    }

    function updateDisplay(s) {
        const abs = Math.abs(s);
        document.getElementById('timerDisplay').textContent = 
            `${Math.floor(abs/60).toString().padStart(2,'0')}:${(abs%60).toString().padStart(2,'0')}`;
    }

    function checkStreak() {
        const today = new Date().toDateString();
        if (state.lastDate !== today) {
            state.failedToday = 0;
            const yesterday = new Date(); yesterday.setDate(yesterday.getDate()-1);
            state.streak = (state.lastDate === yesterday.toDateString()) ? state.streak + 1 : 1;
            state.lastDate = today;
        }
    }

    function renderAnalytics() {
        const trend = document.getElementById('trendChart');
        trend.innerHTML = '';
        const days = ['S', 'M', 'T', 'W', 'T', 'F', 'S'];
        let maxSec = 3600;
        for(let i=6; i>=0; i--) {
            const d = new Date(); d.setDate(d.getDate() - i);
            const key = d.toISOString().split('T')[0];
            maxSec = Math.max(maxSec, state.daily[key] || 0);
        }
        for(let i=6; i>=0; i--) {
            const d = new Date(); d.setDate(d.getDate() - i);
            const key = d.toISOString().split('T')[0];
            const sec = state.daily[key] || 0;
            const h = Math.floor(sec/3600), m = Math.floor((sec%3600)/60);
            const wrapper = document.createElement('div');
            wrapper.className = 'bar-wrapper';
            wrapper.setAttribute('data-tip', `${d.toDateString()}: ${h}h ${m}m`);
            wrapper.innerHTML = `<div class="bar" style="height: ${(sec/maxSec)*100}%"></div><span style="font-size: 8px; color: var(--text-dim); text-align:center; margin-top:8px">${days[d.getDay()]}</span>`;
            trend.appendChild(wrapper);
        }
        const heat = document.getElementById('heatmap');
        heat.innerHTML = '';
        for(let i=27; i>=0; i--) {
            const d = new Date(); d.setDate(d.getDate() - i);
            const key = d.toISOString().split('T')[0];
            const sec = state.daily[key] || 0;
            const h = Math.floor(sec/3600), m = Math.floor((sec%3600)/60);
            const day = document.createElement('div');
            day.className = 'day';
            day.setAttribute('data-tip', `${d.toLocaleDateString([], {month:'short', day:'numeric'})}: ${h}h ${m}m`);
            if (sec > 0) day.style.background = `rgba(0, 255, 213, ${Math.min(1, 0.15 + (sec/14400))})`;
            heat.appendChild(day);
        }
    }

    function render() {
        document.getElementById('cycleVal').textContent = `${state.sessionIdx}/4`;
        document.getElementById('streak').textContent = `🔥 ${state.streak} DAY STREAK`;
        
        const totalW = Object.values(state.daily).reduce((a,b) => a+b, 0);
        const depth = totalW ? Math.round((totalW / (totalW + (state.failedToday * 300) + (state.pauseCount * 600))) * 100) : 0;
        document.getElementById('depthVal').textContent = `${depth}%`;
        
        for(let i=1; i<=4; i++) document.getElementById(`pip${i}`).className = `pip ${i <= state.sessionIdx ? 'active' : ''}`;
        
        if (state.breakUntil && Date.now() < state.breakUntil) {
            mode = 'break'; updateStateUI('break');
            document.getElementById('mainBtn').disabled = true;
            runTimer();
        }
        renderAnalytics();
    }

    function save() { 
        state.lastActivity = Date.now();
        localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); 
    }

    render();
</script>
</body>
</html>
