<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Night Shift: Brain Damage</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }

        body, html {
            width: 100%;
            height: 100%;
            background-color: #050505;
            color: #ffffff;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
            overflow: hidden;
        }

        .screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: opacity 0.4s ease;
        }

        .hidden {
            display: none !important;
        }

        /* --- MENU SCREEN --- */
        #menu-screen {
            background: linear-gradient(rgba(0,0,0,0.65), rgba(0,0,0,0.85)), 
                        url('https://lh3.googleusercontent.com/d/1yN_M8fE4H0Y_L0Y9vGf5V3R-M3p0e1q_') center/cover no-repeat;
            text-align: center;
            padding: 20px;
        }

        .menu-title-container {
            margin-bottom: 25px;
        }

        .main-title {
            font-size: clamp(2.5rem, 8vw, 4.5rem);
            font-weight: 900;
            letter-spacing: 2px;
            color: #ffffff;
            text-shadow: 0 0 20px rgba(220, 38, 38, 0.8), 2px 2px 4px #000;
            line-height: 1.1;
        }

        .sub-title {
            font-size: clamp(1.8rem, 6vw, 3.2rem);
            font-weight: 900;
            color: #ef4444;
            letter-spacing: 4px;
            text-shadow: 0 0 15px rgba(239, 68, 68, 0.9), 0 0 30px rgba(185, 28, 28, 0.6);
            margin-top: 5px;
        }

        .dept-tag {
            font-size: 0.95rem;
            color: #d1d5db;
            letter-spacing: 3px;
            margin-bottom: 8px;
            text-transform: uppercase;
        }

        .quote-tag {
            font-size: 0.85rem;
            color: #9ca3af;
            letter-spacing: 1px;
            margin-bottom: 30px;
        }

        .selector-section {
            margin: 15px 0 30px;
            width: 100%;
            max-width: 400px;
        }

        .section-label {
            font-size: 0.8rem;
            color: #ef4444;
            letter-spacing: 2px;
            margin-top: 8px;
            text-transform: uppercase;
        }

        .pill-button {
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.4);
            color: #ffffff;
            padding: 10px 24px;
            border-radius: 50px;
            font-size: 0.95rem;
            display: inline-block;
            margin: 5px;
            backdrop-filter: blur(4px);
            box-shadow: 0 0 10px rgba(0,0,0,0.5);
        }

        .pill-button.active {
            border-color: #ffffff;
            background: rgba(255, 255, 255, 0.2);
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.3);
        }

        .start-btn {
            background: #ffffff;
            color: #000000;
            border: none;
            padding: 12px 45px;
            border-radius: 6px;
            font-size: 1.1rem;
            font-weight: 700;
            cursor: pointer;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.4);
        }

        .start-btn:active {
            transform: scale(0.96);
            background: #e5e7eb;
        }

        /* --- GAME SCREEN --- */
        #game-screen {
            background-color: #000;
            justify-content: space-between;
        }

        .top-bar {
            width: 100%;
            padding: 10px 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(0,0,0,0.8);
            z-index: 10;
            border-bottom: 1px solid #222;
        }

        .top-bar-left {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        .game-title-small {
            font-size: 0.8rem;
            color: #ef4444;
            font-weight: bold;
        }

        .btn-icon {
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            color: #fff;
            padding: 5px 10px;
            border-radius: 4px;
            font-size: 0.75rem;
            cursor: pointer;
        }

        .viewport-container {
            position: relative;
            flex: 1;
            width: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: #050505;
            overflow: hidden;
        }

        .cctv-feed {
            width: 100%;
            height: 100%;
            object-fit: contain;
            filter: contrast(1.1) brightness(0.9);
        }

        .scanlines {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%);
            background-size: 100% 4px;
            pointer-events: none;
            opacity: 0.6;
        }

        .cam-overlay {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(0,0,0,0.6);
            padding: 4px 10px;
            border-radius: 4px;
            font-family: monospace;
            font-size: 0.85rem;
            color: #00ff66;
            letter-spacing: 1px;
            border: 1px solid rgba(0, 255, 102, 0.3);
            pointer-events: none;
        }

        /* Scan Effect Circle */
        .scan-circle {
            position: absolute;
            width: 70px;
            height: 70px;
            border-radius: 50%;
            border: 3px solid rgba(239, 68, 68, 0.3);
            border-top: 3px solid #ef4444;
            transform: translate(-50%, -50%) rotate(0deg);
            pointer-events: none;
            display: none;
            z-index: 20;
        }

        .scan-circle.active {
            display: block;
            animation: spin 1.8s linear infinite;
        }

        @keyframes spin {
            0% { transform: translate(-50%, -50%) rotate(0deg); }
            100% { transform: translate(-50%, -50%) rotate(360deg); }
        }

        /* Controls Panel */
        .controls-panel {
            width: 100%;
            padding: 12px;
            background: rgba(10, 10, 10, 0.95);
            border-top: 1px solid #222;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 10;
        }

        .cam-selector {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
        }

        .cam-btn {
            background: #1a1a1a;
            border: 1px solid #333;
            color: #888;
            padding: 10px 4px;
            border-radius: 6px;
            font-size: 0.75rem;
            font-weight: bold;
            cursor: pointer;
            text-align: center;
        }

        .cam-btn.active {
            background: #ef4444;
            color: #fff;
            border-color: #f87171;
            box-shadow: 0 0 10px rgba(239, 68, 68, 0.4);
        }

        .file-upload-row {
            display: flex;
            gap: 8px;
            align-items: center;
            justify-content: space-between;
        }

        .file-upload-btn {
            background: #222;
            border: 1px dashed #444;
            color: #aaa;
            padding: 6px 12px;
            border-radius: 4px;
            font-size: 0.7rem;
            cursor: pointer;
            flex: 1;
            text-align: center;
        }

        input[type="file"] {
            display: none;
        }
    </style>
</head>
<body>

    <!-- MAIN MENU SCREEN -->
    <div id="menu-screen" class="screen">
        <div class="menu-title-container">
            <h1 class="main-title">Night Shift</h1>
            <h2 class="sub-title">Brain Damage</h2>
        </div>

        <div class="dept-tag">(TH) · แผนกตรวจจับคนบ้า</div>
        <div class="quote-tag">ใครอนุญาตให้มันขยับ</div>

        <div class="selector-section">
            <div class="pill-button active">บ้านมหัศจรรย์</div>
            <div class="section-label">เลือกสถานที่</div>
        </div>

        <button class="start-btn" onclick="startGame()">เริ่มเวร</button>
    </div>

    <!-- GAMEPLAY SCREEN -->
    <div id="game-screen" class="screen hidden">
        <div class="top-bar">
            <div class="top-bar-left">
                <button class="btn-icon" onclick="exitGame()">❮ เมนู</button>
                <span class="game-title-small">NIGHT SHIFT</span>
            </div>
            <button class="btn-icon" onclick="toggleFullscreen()">เต็มจอ ⛶</button>
        </div>

        <div class="viewport-container" id="viewport" onmousedown="startScan(event)" onmouseup="cancelScan()" onmouseleave="cancelScan()" ontouchstart="startScan(event)" ontouchend="cancelScan()">
            <img id="cctv-img" class="cctv-feed" src="https://lh3.googleusercontent.com/d/1yN_M8fE4H0Y_L0Y9vGf5V3R-M3p0e1q_" alt="CCTV Feed">
            <div class="scanlines"></div>
            <div id="cam-label" class="cam-overlay">CAM1-หน้าบ้าน</div>
            <div id="scan-indicator" class="scan-circle"></div>
        </div>

        <div class="controls-panel">
            <div class="cam-selector">
                <button class="cam-btn active" id="btn-cam1" onclick="switchCam(1)">CAM 1</button>
                <button class="cam-btn" id="btn-cam2" onclick="switchCam(2)">CAM 2</button>
                <button class="cam-btn" id="btn-cam3" onclick="switchCam(3)">CAM 3</button>
                <button class="cam-btn" id="btn-cam4" onclick="switchCam(4)">CAM 4</button>
            </div>

            <div class="file-upload-row">
                <label for="img-uploader" class="file-upload-btn">📷 เปลี่ยนภาพกล้องนี้</label>
                <input type="file" id="img-uploader" accept="image/*" onchange="uploadImage(event)">
            </div>
        </div>
    </div>

    <script>
        const defaultImage = "https://lh3.googleusercontent.com/d/1yN_M8fE4H0Y_L0Y9vGf5V3R-M3p0e1q_";

        const camData = {
            1: { name: "CAM1-หน้าบ้าน", img: defaultImage },
            2: { name: "CAM2-ทางเข้า", img: defaultImage },
            3: { name: "CAM3-ห้องนอน", img: defaultImage },
            4: { name: "CAM4-ห้องครัว", img: defaultImage }
        };

        let currentCam = 1;
        let scanTimer = null;

        function startGame() {
            document.getElementById('menu-screen').classList.add('hidden');
            document.getElementById('game-screen').classList.remove('hidden');
            switchCam(1);
        }

        function exitGame() {
            document.getElementById('game-screen').classList.add('hidden');
            document.getElementById('menu-screen').classList.remove('hidden');
        }

        function switchCam(camNum) {
            currentCam = camNum;
            document.querySelectorAll('.cam-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById(`btn-cam${camNum}`).classList.add('active');

            const cam = camData[camNum];
            document.getElementById('cam-label').innerText = cam.name;
            document.getElementById('cctv-img').src = cam.img;
        }

        function uploadImage(e) {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(evt) {
                    camData[currentCam].img = evt.target.result;
                    document.getElementById('cctv-img').src = evt.target.result;
                };
                reader.readAsDataURL(file);
            }
        }

        function startScan(e) {
            if (e.type === 'touchstart') e.preventDefault();
            
            const viewport = document.getElementById('viewport');
            const rect = viewport.getBoundingClientRect();
            const indicator = document.getElementById('scan-indicator');

            let clientX = e.clientX || (e.touches && e.touches[0].clientX);
            let clientY = e.clientY || (e.touches && e.touches[0].clientY);

            if (!clientX || !clientY) return;

            const x = clientX - rect.left;
            const y = clientY - rect.top;

            indicator.style.left = `${x}px`;
            indicator.style.top = `${y}px`;
            indicator.classList.add('active');

            scanTimer = setTimeout(() => {
                cancelScan();
                alert(`ตรวจสแกนตำแหน่ง (${Math.round(x)}, ${Math.round(y)}) สำเร็จ!`);
            }, 1800);
        }

        function cancelScan() {
            if (scanTimer) clearTimeout(scanTimer);
            document.getElementById('scan-indicator').classList.remove('active');
        }

        function toggleFullscreen() {
            if (!document.fullscreenElement) {
                document.documentElement.requestFullscreen().catch(err => {});
            } else {
                if (document.exitFullscreen) document.exitFullscreen();
            }
        }
    </script>
</body>
</html>
