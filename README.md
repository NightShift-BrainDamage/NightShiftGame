
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>Night Shift: Brain Damage</title>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  user-select: none;
  -webkit-user-select: none;
}

html, body {
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: #050505;
  color: white;
  font-family: Arial, sans-serif;
}

button { cursor: pointer; }

.hidden { display: none !important; }

.screen {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

#menu-screen {
  background: #0a0a0a;
  text-align: center;
  padding: 20px;
}

.main-title {
  font-size: clamp(2.5rem, 9vw, 4.5rem);
  font-weight: 900;
  text-shadow: 0 0 20px #dc2626;
}

.sub-title {
  font-size: clamp(1.8rem, 7vw, 3.2rem);
  color: #ef4444;
  letter-spacing: 3px;
  text-shadow: 0 0 20px #b91c1c;
  margin-bottom: 25px;
}

.dept-tag { color: #ddd; margin-bottom: 10px; }
.quote-tag { color: #999; margin-bottom: 30px; }

.pill-button {
  display: inline-block;
  border: 1px solid #777;
  border-radius: 30px;
  padding: 10px 24px;
  margin: 15px;
}

.section-label {
  color: #ef4444;
  font-size: 12px;
}

.start-btn {
  padding: 13px 45px;
  border: none;
  border-radius: 6px;
  font-size: 18px;
  font-weight: bold;
}

#game-screen {
  background: black;
  justify-content: space-between;
}

.top-bar {
  width: 100%;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #101010;
  border-bottom: 1px solid #333;
  z-index: 5;
}

.top-bar-left {
  display: flex;
  gap: 10px;
  align-items: center;
}

.game-title-small {
  color: #ef4444;
  font-weight: bold;
  font-size: 12px;
}

.btn-icon {
  background: #222;
  border: 1px solid #444;
  color: white;
  padding: 8px 10px;
  border-radius: 4px;
}

.viewport-container {
  position: relative;
  flex: 1;
  width: 100%;
  min-height: 0;
  overflow: hidden;
  background: #080808;
  display: flex;
  justify-content: center;
  align-items: center;
  touch-action: none;
}

.cctv-feed {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: contain;
  filter: contrast(1.1) brightness(.85);
  pointer-events: none;
}

.scanlines {
  position: absolute;
  inset: 0;
  background: repeating-linear-gradient(
    to bottom,
    transparent 0px,
    transparent 3px,
    rgba(0,0,0,.23) 4px
  );
  pointer-events: none;
  z-index: 2;
}

.cam-overlay {
  position: absolute;
  top: 12px;
  right: 12px;
  padding: 5px 9px;
  color: #00ff66;
  background: rgba(0,0,0,.75);
  border: 1px solid #176c3b;
  font: 13px monospace;
  z-index: 4;
  pointer-events: none;
}

/* Anomaly ที่สร้างด้วย CSS ไม่ต้องใช้รูปเพิ่ม */
.anomaly-visual {
  position: absolute;
  width: 9%;
  height: 32%;
  min-width: 24px;
  min-height: 55px;
  transform: translate(-50%, -50%);
  z-index: 1;
  pointer-events: none;
  filter: blur(1px);
}

.anomaly-visual::before {
  content: "";
  position: absolute;
  width: 38%;
  aspect-ratio: 1;
  top: 0;
  left: 31%;
  border-radius: 50%;
  background: #050505;
  box-shadow: 0 0 8px rgba(0,0,0,.8);
}

.anomaly-visual::after {
  content: "";
  position: absolute;
  top: 28%;
  left: 12%;
  width: 76%;
  height: 68%;
  background: #050505;
  border-radius: 45% 45% 8% 8%;
  clip-path: polygon(25% 0,75% 0,100% 100%,0 100%);
}

.scan-circle {
  position: absolute;
  width: 70px;
  height: 70px;
  border-radius: 50%;
  border: 3px solid rgba(239,68,68,.35);
  border-top-color: #ef4444;
  transform: translate(-50%, -50%);
  display: none;
  pointer-events: none;
  z-index: 6;
}

.scan-circle.active {
  display: block;
  animation: spin 3s linear infinite;
}

@keyframes spin {
  to { transform: translate(-50%,-50%) rotate(360deg); }
}

.status-bar {
  width: 100%;
  padding: 8px;
  text-align: center;
  color: #aaa;
  background: #080808;
  font-size: 12px;
}

.controls-panel {
  width: 100%;
  padding: 10px;
  background: #101010;
  border-top: 1px solid #333;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.cam-selector {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 7px;
}

.cam-btn {
  background: #1a1a1a;
  border: 1px solid #444;
  color: #aaa;
  padding: 12px 2px;
  border-radius: 5px;
  font-size: 12px;
  font-weight: bold;
}

.cam-btn.active {
  background: #b91c1c;
  color: white;
  border-color: #ef4444;
}

.file-upload-btn {
  display: block;
  text-align: center;
  padding: 9px;
  border: 1px dashed #555;
  color: #aaa;
  font-size: 12px;
}

#img-uploader { display: none; }

@media (max-height: 500px) {
  .controls-panel { padding: 5px; gap: 5px; }
  .cam-btn { padding: 7px 2px; }
  .top-bar { padding: 5px; }
}
</style>
</head>

<body>

<!-- เมนู -->
<div id="menu-screen" class="screen">
  <h1 class="main-title">Night Shift</h1>
  <h2 class="sub-title">Brain Damage</h2>

  <div class="dept-tag">(TH) · แผนกตรวจจับคนบ้า</div>
  <div class="quote-tag">ใครอนุญาตให้มันขยับ</div>

  <div class="pill-button">บ้านมหัศจรรย์</div>
  <div class="section-label">เลือกสถานที่</div>

  <button class="start-btn" onclick="startGame()">เริ่มเวร</button>
</div>

<!-- เกม -->
<div id="game-screen" class="screen hidden">

  <div class="top-bar">
    <div class="top-bar-left">
      <button class="btn-icon" onclick="exitGame()">❮ เมนู</button>
      <span class="game-title-small">NIGHT SHIFT</span>
    </div>
    <button class="btn-icon" onclick="toggleFullscreen()">เต็มจอ ⛶</button>
  </div>

  <div class="viewport-container" id="viewport">
    <img id="cctv-img" class="cctv-feed" alt="CCTV Feed">
    <div id="anomaly-layer" class="anomaly-visual hidden"></div>
    <div class="scanlines"></div>
    <div id="cam-label" class="cam-overlay">CAM1-หน้าบ้าน</div>
    <div id="scan-indicator" class="scan-circle"></div>
  </div>

  <div class="status-bar" id="status">
    กดค้างบริเวณที่สงสัย 3 วินาที เพื่อรายงาน
  </div>

  <div class="controls-panel">
    <div class="cam-selector">
      <button class="cam-btn active" id="btn-cam1" onclick="switchCam(1)">CAM 1</button>
      <button class="cam-btn" id="btn-cam2" onclick="switchCam(2)">CAM 2</button>
      <button class="cam-btn" id="btn-cam3" onclick="switchCam(3)">CAM 3</button>
      <button class="cam-btn" id="btn-cam4" onclick="switchCam(4)">CAM 4</button>
    </div>

    <label for="img-uploader" class="file-upload-btn">
      📷 เปลี่ยนภาพกล้องนี้ชั่วคราว
    </label>
    <input type="file" id="img-uploader" accept="image/*">
  </div>
</div>

<script>
// รูปจากลิงก์ที่ให้มา
const camData = {
  1: {
    name: "CAM1-หน้าบ้าน",
    normalImg: "https://i.ibb.co/21mfB9mJ/file-000000003c988211b0c3ff039547d798.png",
    hasAnomaly: false,
    anomalyArea: { x: 72, y: 58 }
  },
  2: {
    name: "CAM2-ทางเข้า",
    normalImg: "https://i.ibb.co/vC8RxnjN/75287-FFB-AF44-4791-B2D1-656-A75-BE7-B92.png",
    hasAnomaly: false,
    anomalyArea: { x: 70, y: 60 }
  },
  3: {
    name: "CAM3-ห้องนอน",
    normalImg: "https://i.ibb.co/0RDpWjDM/file-000000002d2c82118cc9bb6d294dda70.png",
    hasAnomaly: false,
    anomalyArea: { x: 74, y: 58 }
  },
  4: {
    name: "CAM4-ห้องครัว",
    normalImg: "https://i.ibb.co/Mk0y7SPf/66617-C32-C425-4-BEA-ACC5-4-D537-C5-A4-EAC.png",
    hasAnomaly: false,
    anomalyArea: { x: 68, y: 57 }
  }
};

let currentCam = 1;
let scanTimer = null;
let scanPosition = null;
let score = 0;
let foundCount = 0;

const viewport = document.getElementById("viewport");
const cameraImage = document.getElementById("cctv-img");
const anomalyLayer = document.getElementById("anomaly-layer");
const indicator = document.getElementById("scan-indicator");
const statusText = document.getElementById("status");

function startGame() {
  document.getElementById("menu-screen").classList.add("hidden");
  document.getElementById("game-screen").classList.remove("hidden");

  score = 0;
  foundCount = 0;

  // สุ่มให้มี Anomaly อย่างน้อยหนึ่งกล้อง
  let anomalyCount = 0;

  for (let i = 1; i <= 4; i++) {
    camData[i].hasAnomaly = Math.random() < 0.5;
    if (camData[i].hasAnomaly) anomalyCount++;
  }

  if (anomalyCount === 0) {
    camData[Math.ceil(Math.random() * 4)].hasAnomaly = true;
  }

  switchCam(1);
}

function exitGame() {
  cancelScan();
  document.getElementById("game-screen").classList.add("hidden");
  document.getElementById("menu-screen").classList.remove("hidden");
}

function switchCam(number) {
  cancelScan();
  currentCam = number;

  document.querySelectorAll(".cam-btn").forEach(button => {
    button.classList.remove("active");
  });

  document.getElementById("btn-cam" + number).classList.add("active");

  const cam = camData[number];
  document.getElementById("cam-label").textContent = cam.name;
  cameraImage.src = cam.normalImg;

  updateAnomaly();
}

function updateAnomaly() {
  const cam = camData[currentCam];

  if (cam.hasAnomaly) {
    anomalyLayer.classList.remove("hidden");
    anomalyLayer.style.left = cam.anomalyArea.x + "%";
    anomalyLayer.style.top = cam.anomalyArea.y + "%";
  } else {
    anomalyLayer.classList.add("hidden");
  }
}

// เลือกรูปอื่นจากมือถือได้ แต่จะเปลี่ยนเฉพาะรอบที่เปิดอยู่
document.getElementById("img-uploader").addEventListener("change", function(event) {
  const file = event.target.files[0];
  if (!file) return;

  const reader = new FileReader();

  reader.onload = function(e) {
    camData[currentCam].normalImg = e.target.result;
    cameraImage.src = e.target.result;
  };

  reader.readAsDataURL(file);
  event.target.value = "";
});

// กดค้าง 3 วินาทีเพื่อสแกน
viewport.addEventListener("pointerdown", function(event) {
  if (event.target.closest("button")) return;

  event.preventDefault();
  cancelScan();

  const rect = viewport.getBoundingClientRect();

  const x = event.clientX - rect.left;
  const y = event.clientY - rect.top;

  scanPosition = {
    x: x,
    y: y,
    pointerId: event.pointerId
  };

  indicator.style.left = x + "px";
  indicator.style.top = y + "px";
  indicator.classList.add("active");

  statusText.textContent = "กำลังตรวจสอบ... กดค้างไว้ให้ครบ 3 วินาที";

  scanTimer = setTimeout(() => {
    const cam = camData[currentCam];

    const px = (scanPosition.x / rect.width) * 100;
    const py = (scanPosition.y / rect.height) * 100;

    const dx = px - cam.anomalyArea.x;
    const dy = py - cam.anomalyArea.y;
    const distance = Math.sqrt(dx * dx + dy * dy);

    indicator.classList.remove("active");
    scanTimer = null;

    if (cam.hasAnomaly && distance < 12) {
      cam.hasAnomaly = false;
      score += 100;
      foundCount++;

      updateAnomaly();

      statusText.textContent =
        "ตรวจพบ Anomaly! คะแนน " + score;

      alert("ตรวจพบความผิดปกติใน " + cam.name + " สำเร็จ! คะแนน +100");
    } else {
      statusText.textContent = "ไม่พบความผิดปกติในตำแหน่งนี้";
      alert("ไม่พบ Anomaly ลองตรวจสอบกล้องอีกครั้ง");
    }
  }, 3000);
});

function cancelScan() {
  if (scanTimer !== null) {
    clearTimeout(scanTimer);
    scanTimer = null;
  }

  indicator.classList.remove("active");
}

viewport.addEventListener("pointerup", cancelScan);
viewport.addEventListener("pointercancel", cancelScan);
viewport.addEventListener("pointerleave", cancelScan);

function toggleFullscreen() {
  if (!document.fullscreenElement) {
    document.documentElement.requestFullscreen?.().catch(() => {});
  } else {
    document.exitFullscreen?.();
  }
}

// แจ้งเตือนเมื่อโหลดภาพไม่สำเร็จ
cameraImage.addEventListener("error", function() {
  statusText.textContent =
    "โหลดรูปไม่สำเร็จ กรุณาตรวจสอบลิงก์อินเทอร์เน็ต";
});

cameraImage.addEventListener("load", function() {
  statusText.textContent =
    "กดค้างบริเวณที่สงสัย 3 วินาที เพื่อรายงาน";
});
</script>

</body>
</html>
