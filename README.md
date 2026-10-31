<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>System Diagnostic</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
            -webkit-user-select: none;
        }
        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #000;
            font-family: 'Courier New', Courier, monospace;
            /* Side swipe aur pull-to-refresh ko block karne ke liye */
            touch-action: none;
            overscroll-behavior: none;
        }
        canvas {
            position: absolute;
            top: 0;
            left: 0;
            z-index: 1;
        }
        #start-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            z-index: 9999;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #00ff00;
            font-size: 18px;
            text-align: center;
            padding: 20px;
            cursor: pointer;
        }
        #content {
            position: relative;
            z-index: 2;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            color: #00ff00;
            padding: 20px;
            padding-bottom: 70px; /* Fake nav bar ke liye space */
        }
        .glitch-text {
            font-size: 28px;
            font-weight: bold;
            color: #00ff00;
            text-shadow: 0 0 10px #00ff00, 0 0 20px #00ff00;
            margin-bottom: 15px;
            animation: blink 0.8s infinite alternate;
            text-transform: uppercase;
        }
        .tag-text {
            font-size: 22px;
            color: #ffffff;
            background: #008800;
            padding: 5px 20px;
            border-radius: 5px;
            margin-bottom: 25px;
            font-weight: bold;
            letter-spacing: 1px;
        }
        .status-box {
            font-size: 14px;
            color: #ff3333;
            border: 1px solid #ff3333;
            padding: 15px;
            background: rgba(255, 0, 0, 0.15);
            width: 90%;
            max-width: 400px;
            margin-bottom: 20px;
            text-align: left;
            line-height: 1.5;
        }
        .reveal-text {
            font-size: 26px;
            color: #ffff00;
            text-shadow: 0 0 15px #ffff00;
            font-weight: bold;
            display: none;
            z-index: 999;
        }
        /* Fake Android Navigation Bar */
        .fake-nav-bar {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 55px;
            background-color: rgba(0, 0, 0, 0.95);
            display: flex;
            justify-content: space-around;
            align-items: center;
            z-index: 1000;
            border-top: 1px solid #222;
        }
        .fake-btn {
            color: #888;
            font-size: 24px;
            width: 33%;
            text-align: center;
            cursor: pointer;
            padding: 10px 0;
        }
        .fake-btn:active {
            color: #ff0000;
        }
        @keyframes blink {
            0% { opacity: 1; }
            100% { opacity: 0.3; }
        }
    </style>
</head>
<body>

    <div id="start-overlay">
        <p>[ CLICK ANYWHERE TO CONTINUE ]<br><br><small>Checking system compatibility...</small></p>
    </div>

    <canvas id="matrix"></canvas>

    <div id="content">
        <div id="hack-ui">
            <div class="glitch-text">your phone is hacked</div>
            <div class="tag-text">@mr.imran</div>
            <div class="status-box" id="logs">
                > Injecting payload...<br>
                > Accessing internal storage...<br>
                > System locked.
            </div>
        </div>
        <div id="reveal-ui" class="reveal-text">
            chill yaar chalo chhod deta hun sorry 😁
        </div>
    </div>

    <!-- Fake Navigation Buttons -->
    <div class="fake-nav-bar" id="fakeNav" style="display: none;">
        <div class="fake-btn" onclick="fakeAction('back')">◀</div>
        <div class="fake-btn" onclick="fakeAction('home')">●</div>
        <div class="fake-btn" onclick="fakeAction('recents')">■</div>
    </div>

    <script>
        // 1-Time Access Check
        if (localStorage.getItem("imran_prank_done") === "true") {
            document.body.innerHTML = "<h1 style='color:white; text-align:center; margin-top:40%; font-family:sans-serif;'>404 Not Found<br><small style='font-size:14px; color:#888;'>The requested link has expired or been deleted.</small></h1>";
            throw new Error("Link expired.");
        }

        // Lock Back Button (History Trap)
        function trapBackButton() {
            history.pushState(null, null, location.href);
            window.onpopstate = function () {
                history.pushState(null, null, location.href);
            };
        }
        trapBackButton();

        // Block side swipe and screen pulling gestures
        document.addEventListener('touchmove', function (e) {
            e.preventDefault();
        }, { passive: false });
        document.addEventListener('contextmenu', event => event.preventDefault());

        // Fake Nav Button Logic
        function fakeAction(btnType) {
            if ("vibrate" in navigator) navigator.vibrate(100); // Short vibrate on fake button tap
            const logs = document.getElementById('logs');
            if (btnType === 'back') {
                logs.innerHTML += "<br><span style='color:yellow;'>[!] Back action blocked.</span>";
            } else if (btnType === 'home') {
                logs.innerHTML += "<br><span style='color:red;'>[!] Home override failed. System locked.</span>";
            } else if (btnType === 'recents') {
                logs.innerHTML += "<br><span style='color:yellow;'>[!] App switching disabled.</span>";
            }
        }

        // Matrix Effect
        const canvas = document.getElementById('matrix');
        const ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
        const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789@#$%&*';
        const fontSize = 16;
        const columns = Math.floor(canvas.width / fontSize);
        const drops = Array(columns).fill(1);

        function drawMatrix() {
            ctx.fillStyle = 'rgba(0, 0, 0, 0.08)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = '#0f0';
            ctx.font = fontSize + 'px monospace';
            for (let i = 0; i < drops.length; i++) {
                const text = chars.charAt(Math.floor(Math.random() * chars.length));
                ctx.fillText(text, i * fontSize, drops[i] * fontSize);
                if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) drops[i] = 0;
                drops[i]++;
            }
        }
        setInterval(drawMatrix, 33);

        // Siren Audio Generator
        let audioCtx;
        function playSiren() {
            if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            if (audioCtx.state === 'suspended') audioCtx.resume();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(800, audioCtx.currentTime);
            osc.frequency.exponentialRampToValueAtTime(400, audioCtx.currentTime + 0.5);
            gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.start();
            osc.stop(audioCtx.currentTime + 0.5);
        }

        // Start Prank on First Tap
        document.getElementById('start-overlay').addEventListener('click', function () {
            this.style.display = 'none';
            document.getElementById('fakeNav').style.display = 'flex'; // Show fake nav bar

            // 1. Fullscreen Request
            if (document.documentElement.requestFullscreen) {
                document.documentElement.requestFullscreen().catch(()=>{});
            } else if (document.documentElement.webkitRequestFullscreen) {
                document.documentElement.webkitRequestFullscreen().catch(()=>{});
            }

            // 2. Continuous Vibration
            let vibInterval = null;
            if ("vibrate" in navigator) {
                navigator.vibrate([500, 200, 500, 200, 800]);
                vibInterval = setInterval(() => {
                    navigator.vibrate([500, 200, 500, 200, 800]);
                }, 2000);
            }

            // 3. Sound Loop
            const soundInterval = setInterval(playSiren, 600);

            // 4. Fake Terminal Log Updates
            const logs = document.getElementById('logs');
            setTimeout(() => { logs.innerHTML += "<br>> Copying WhatsApp Media..."; }, 4000);
            setTimeout(() => { logs.innerHTML += "<br>> Connecting to remote server..."; }, 9000);
            setTimeout(() => { logs.innerHTML += "<br>> Uploading data... 45%"; }, 14000);
            setTimeout(() => { logs.innerHTML += "<br>> Uploading data... 89%"; }, 18000);
            setTimeout(() => { logs.innerHTML += "<br>> Wiping Partition (DO NOT TURN OFF)..."; }, 23000);

            // 5. Prank Reveal after 30 Seconds
            setTimeout(() => {
                clearInterval(soundInterval);
                if (vibInterval) clearInterval(vibInterval);
                if ("vibrate" in navigator) navigator.vibrate(0); // Stop vibration

                document.getElementById('hack-ui').style.display = 'none';
                document.getElementById('fakeNav').style.display = 'none'; // Hide fake nav
                document.getElementById('reveal-ui').style.display = 'block'; // Show Reveal Text

                // Mark as viewed so it doesn't run again
                localStorage.setItem("imran_prank_done", "true");
            }, 30000); // 30000 milliseconds = 30 seconds
        });
    </script>
</body>
</html>
