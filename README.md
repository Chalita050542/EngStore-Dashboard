<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EngStore Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;600&family=IBM+Plex+Sans+Thai:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
:root {
  --bg:        #f0f4f8;
  --surface:   #ffffff;
  --surface2:  #f7f9fc;
  --border:    #d8e2ec;
  --border2:   #c5d4e2;
  --text:      #1a2b3c;
  --text-dim:  #7a96b0;
  --text-mid:  #4a6a84;
  --cons:      #0e8f6a;
  --cons-dim:  #d4f0e8;
  --cons-mid:  #1ab589;
  --eng:       #6b4ed4;
  --eng-dim:   #ebe5ff;
  --eng-mid:   #8b6ce8;
  --up:        #0f9e63;
  --down:      #e03c3c;
  --neutral:   #7a96b0;
  --accent:    #1a7fc1;
  --week-a:    #c47a00;
  --week-b:    #1a7fc1;
  --part-bg:   #eaf2fb;
  --part-text: #1a5a8a;
  --shadow:    rgba(0,0,0,0.07);
}

* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: 'IBM Plex Sans Thai', sans-serif;
  background: var(--bg);
  color: var(--text);
  height: 100vh;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

/* ── TOPBAR ── */
.topbar {
  background: var(--surface);
  border-bottom: 2px solid var(--border);
  padding: 0 20px;
  height: 52px;
  display: flex;
  align-items: center;
  gap: 16px;
  flex-shrink: 0;
  box-shadow: 0 2px 8px var(--shadow);
}
.logo {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 13px;
  font-weight: 600;
  color: var(--accent);
  letter-spacing: 2px;
  text-transform: uppercase;
  white-space: nowrap;
}
.logo span { color: var(--text-mid); font-weight: 400; }
.divider { width: 1px; height: 24px; background: var(--border2); flex-shrink: 0; }

/* week pills */
.week-group { display: flex; gap: 6px; align-items: center; }
.week-label { font-size: 11px; color: var(--text-dim); font-weight: 600; letter-spacing: 1px; text-transform: uppercase; margin-right: 4px; }
.wpill {
  padding: 4px 14px;
  border-radius: 20px;
  border: 1.5px solid var(--border2);
  background: var(--surface2);
  color: var(--text-mid);
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.15s;
  letter-spacing: 0.5px;
}
.wpill:hover { border-color: var(--accent); color: var(--accent); background: #e8f3fb; }
.wpill.sel-a { background: #fff8e8; border-color: var(--week-a); color: var(--week-a); }
.wpill.sel-b { background: #e6f3fb; border-color: var(--week-b); color: var(--week-b); }

.compare-tag {
  font-size: 11px;
  color: var(--text-mid);
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 4px 12px;
  font-family: 'IBM Plex Mono', monospace;
}
.compare-tag .wa { color: var(--week-a); font-weight: 600; }
.compare-tag .wb { color: var(--week-b); font-weight: 600; }

/* search */
.search-wrap { margin-left: auto; position: relative; }
.search-input {
  background: var(--surface2);
  border: 1.5px solid var(--border2);
  border-radius: 20px;
  padding: 5px 14px 5px 32px;
  color: var(--text);
  font-size: 12px;
  font-family: 'IBM Plex Sans Thai', sans-serif;
  width: 210px;
  outline: none;
  transition: border-color 0.15s, box-shadow 0.15s;
}
.search-input:focus { border-color: var(--accent); box-shadow: 0 0 0 3px rgba(26,127,193,0.12); }
.search-input::placeholder { color: var(--text-dim); }
.search-icon {
  position: absolute; left: 11px; top: 50%;
  transform: translateY(-50%);
  color: var(--text-dim); font-size: 13px; pointer-events: none;
}

/* upload btn */
.upload-btn {
  display: flex; align-items: center; gap: 6px;
  padding: 5px 14px;
  background: var(--surface2);
  border: 1.5px solid var(--border2);
  border-radius: 20px;
  color: var(--text-mid);
  font-size: 12px;
  font-family: 'IBM Plex Sans Thai', sans-serif;
  cursor: pointer;
  transition: all 0.15s;
  white-space: nowrap;
}
.upload-btn:hover { border-color: var(--cons); color: var(--cons); background: #edfaf5; }
#fileInput { display: none; }

/* ── PANELS ── */
.panels {
  display: grid;
  grid-template-columns: 1fr 1fr;
  flex: 1;
  overflow: hidden;
  gap: 12px;
  padding: 12px;
}

.panel {
  display: flex;
  flex-direction: column;
  background: var(--surface);
  border-radius: 12px;
  border: 1px solid var(--border);
  overflow: hidden;
  box-shadow: 0 2px 10px var(--shadow);
}

/* panel header */
.panel-head {
  padding: 12px 16px 10px;
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.panel-head.cons { background: linear-gradient(90deg, #edfaf5 0%, #ffffff 60%); border-bottom-color: #b2e8d8; }
.panel-head.eng  { background: linear-gradient(90deg, #f0ecff 0%, #ffffff 60%); border-bottom-color: #cfc0f5; }

.panel-name {
  display: flex; align-items: center; gap: 8px;
  font-size: 14px; font-weight: 700; letter-spacing: 0.5px;
}
.panel-name.cons { color: var(--cons); }
.panel-name.eng  { color: var(--eng); }
.dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }
.dot.cons { background: var(--cons); }
.dot.eng  { background: var(--eng); }
.panel-sub { font-size: 10px; color: var(--text-dim); font-weight: 400; margin-left: 2px; }

.panel-meta { display: flex; gap: 10px; align-items: center; }
.meta-chip {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
  color: var(--text-mid);
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 2px 8px;
}
.meta-chip span { color: var(--text); font-weight: 600; }

/* col headers */
.col-header {
  display: grid;
  padding: 6px 16px;
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  background: var(--surface2);
}
.col-header.has-b { grid-template-columns: 26px 90px 1fr 78px 78px 88px 60px; }
.col-header.no-b  { grid-template-columns: 26px 90px 1fr 78px; }

.ch {
  font-size: 10px;
  font-weight: 600;
  color: var(--text-dim);
  letter-spacing: 0.8px;
  text-transform: uppercase;
}
.ch.r { text-align: right; }
.ch.sortable { cursor: pointer; }
.ch.sortable:hover { color: var(--accent); }
.ch.wa { color: var(--week-a); }
.ch.wb { color: var(--week-b); }

/* rows */
.rows-wrap {
  flex: 1;
  overflow-y: auto;
  scrollbar-width: thin;
  scrollbar-color: var(--border2) transparent;
}
.rows-wrap::-webkit-scrollbar { width: 5px; }
.rows-wrap::-webkit-scrollbar-track { background: transparent; }
.rows-wrap::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 5px; }

.data-row {
  display: grid;
  padding: 5px 16px;
  align-items: center;
  border-bottom: 1px solid var(--border);
  transition: background 0.1s;
  gap: 0;
}
.data-row:hover { background: var(--surface2); }
.data-row.has-b { grid-template-columns: 26px 90px 1fr 78px 78px 88px 60px; }
.data-row.no-b  { grid-template-columns: 26px 90px 1fr 78px; }

.row-num {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
  color: var(--text-dim);
  text-align: center;
}

/* Part number badge */
.part-badge {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
  font-weight: 600;
  color: var(--part-text);
  background: var(--part-bg);
  border: 1px solid #b8d4ee;
  border-radius: 4px;
  padding: 2px 5px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 88px;
}

.item-name {
  font-size: 12px;
  color: var(--text);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  padding-right: 6px;
  line-height: 1.3;
}

.cell-num {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12px;
  text-align: right;
  color: var(--text);
  padding-right: 2px;
}
.cell-num.zero { color: var(--text-dim); opacity: 0.5; }

.zero { color: var(--text-dim); opacity: 0.5; }

/* arrow cells */
.arrow-cell {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  text-align: right;
  font-weight: 600;
  padding-right: 4px;
}
.arr-up   { color: var(--up); }
.arr-down { color: var(--down); }
.arr-same { color: var(--text-dim); }

.pct-cell {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
  text-align: right;
  border-radius: 4px;
  padding: 2px 4px;
}
.pct-up   { color: var(--up);   background: #e6faf3; }
.pct-down { color: var(--down); background: #fdeaea; }
.pct-same { color: var(--text-dim); }

/* footer */
.panel-foot {
  padding: 8px 16px;
  border-top: 1px solid var(--border);
  background: var(--surface2);
  display: flex;
  gap: 14px;
  flex-wrap: wrap;
  flex-shrink: 0;
}
.foot-stat {
  font-size: 11px;
  color: var(--text-mid);
  font-family: 'IBM Plex Mono', monospace;
}
.foot-stat b { font-weight: 700; color: var(--text); }
.foot-stat.up b { color: var(--up); }
.foot-stat.dn b { color: var(--down); }

/* no selection */
.no-sel {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 200px;
  gap: 10px;
  color: var(--text-dim);
  font-size: 13px;
}
.no-sel .big { font-size: 36px; }

/* ── OVERLAY ── */
.overlay {
  display: none;
  position: fixed; inset: 0;
  background: rgba(200,215,230,0.6);
  backdrop-filter: blur(4px);
  z-index: 100;
  align-items: center;
  justify-content: center;
}
.overlay.show { display: flex; }
.overlay-box {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 32px;
  max-width: 480px;
  width: 90%;
  box-shadow: 0 12px 40px rgba(0,60,100,0.15);
}
.overlay-box h2 { font-size: 18px; font-weight: 700; margin-bottom: 8px; color: var(--text); }
.overlay-box p  { font-size: 13px; color: var(--text-mid); margin-bottom: 20px; line-height: 1.6; }
.overlay-box ul { font-size: 12px; color: var(--text-mid); margin: 0 0 20px 18px; line-height: 2; }
.overlay-box ul li b { color: var(--accent); font-family: 'IBM Plex Mono', monospace; }
.btn-row { display: flex; gap: 10px; }
.btn-upload {
  flex: 1; padding: 10px;
  background: var(--accent);
  color: white;
  border: none; border-radius: 8px;
  font-size: 13px; font-weight: 600;
  font-family: 'IBM Plex Sans Thai', sans-serif;
  cursor: pointer;
  transition: background 0.15s;
}
.btn-upload:hover { background: #155f94; }
.btn-cancel {
  padding: 10px 18px;
  background: var(--surface2);
  color: var(--text-mid);
  border: 1.5px solid var(--border2); border-radius: 8px;
  font-size: 13px; font-weight: 600;
  font-family: 'IBM Plex Sans Thai', sans-serif;
  cursor: pointer;
}
.btn-cancel:hover { border-color: var(--accent); color: var(--accent); }
</style>
</head>
<body>

<!-- Upload Overlay -->
<div class="overlay" id="uploadOverlay">
  <div class="overlay-box">
    <h2>📂 อัพโหลดข้อมูลสัปดาห์ใหม่</h2>
    <p>เลือกไฟล์ Excel (.xlsx) ที่ต้องการอัพเดทข้อมูล Dashboard</p>
    <ul>
      <li>Sheet ต้องชื่อ <b>"Data base"</b></li>
      <li>ต้องมีคอลัมน์: <b>G/L Date, Part Number, Description, Units, CONS/ENG, Week</b></li>
    </ul>
    <div class="btn-row">
      <button class="btn-upload" onclick="document.getElementById('fileInput').click()">📁 เลือกไฟล์</button>
      <button class="btn-cancel" onclick="closeOverlay()">ยกเลิก</button>
    </div>
    <input type="file" id="fileInput" accept=".xlsx,.xls" onchange="handleUpload(event)">
  </div>
</div>

<!-- Topbar -->
<div class="topbar">
  <div class="logo">EngStore<span> Dashboard</span></div>
  <div class="divider"></div>
  <div class="week-group">
    <span class="week-label">Week</span>
    <div id="weekPills"></div>
  </div>
  <div class="compare-tag" id="compareTag"></div>
  <div class="divider"></div>
  <div class="search-wrap">
    <span class="search-icon">🔍</span>
    <input class="search-input" id="searchInput" placeholder="ค้นหา ชื่อ / Part No..." oninput="render()">
  </div>
  <button class="upload-btn" onclick="document.getElementById('uploadOverlay').classList.add('show')">
    ⬆ อัพเดทข้อมูล
  </button>
  <input type="file" id="fileInputDirect" accept=".xlsx,.xls" style="display:none" onchange="handleUpload(event)">
</div>

<!-- Panels -->
<div class="panels">
  <!-- CONS -->
  <div class="panel">
    <div class="panel-head cons">
      <div class="panel-name cons"><div class="dot cons"></div> CONS <span class="panel-sub">Consumables</span></div>
      <div class="panel-meta">
        <div class="meta-chip" id="metaCONS"><span>—</span> รายการ</div>
      </div>
    </div>
    <div class="col-header" id="chCONS"></div>
    <div class="rows-wrap"><div id="rowsCONS"></div></div>
    <div class="panel-foot" id="footCONS"></div>
  </div>

  <!-- ENG -->
  <div class="panel">
    <div class="panel-head eng">
      <div class="panel-name eng"><div class="dot eng"></div> ENG <span class="panel-sub">Engineering</span></div>
      <div class="panel-meta">
        <div class="meta-chip" id="metaENG"><span>—</span> รายการ</div>
      </div>
    </div>
    <div class="col-header" id="chENG"></div>
    <div class="rows-wrap"><div id="rowsENG"></div></div>
    <div class="panel-foot" id="footENG"></div>
  </div>
</div>

<script>
// ── DATA ─────────────────────────────────────────────────────────────────────
let DB = {"weeks": [8, 9, 10, 11], "data": {"CONS": [{"name": "LABEL FIFO COLOUR 200X100MM 2L", "part": "136976", "w8": 15500.0, "w9": 6000.0, "w10": 13000.0, "w11": 6000.0}, {"name": "LABEL 100X200MM KANBAN", "part": "813431", "w8": 1500.0, "w9": 4000.0, "w10": 1000.0, "w11": 0}, {"name": "GLOVES NITRILE", "part": "264821", "w8": 1950.0, "w9": 950.0, "w10": 1400.0, "w11": 0}, {"name": "CARBON FACE MASK", "part": "129194", "w8": 950.0, "w9": 450.0, "w10": 800.0, "w11": 0}, {"name": "577F ROLOC 2\"G80", "part": "137711", "w8": 600.0, "w9": 700.0, "w10": 400.0, "w11": 0}, {"name": "TAPE PKG 35MMX100MM 372 BRN", "part": "137160", "w8": 480.0, "w9": 480.0, "w10": 576.0, "w11": 0}, {"name": "GLOVES COTTON", "part": "902415", "w8": 528.0, "w9": 120.0, "w10": 480.0, "w11": 0}, {"name": "ROLOC 2XNH MED SCOTBRITE", "part": "136472", "w8": 450.0, "w9": 300.0, "w10": 250.0, "w11": 0}, {"name": "HYDROCHLORIC ACID 22%", "part": "130617", "w8": 1.0, "w9": 999.0, "w10": 0, "w11": 0}, {"name": "LABEL REWORK STICKER (YE)", "part": "472964", "w8": 0, "w9": 1000.0, "w10": 0, "w11": 0}, {"name": "PINK STICKER", "part": "129542", "w8": 0, "w9": 0, "w10": 1000.0, "w11": 0}, {"name": "GLOVE NYLON PU SIZE L", "part": "127569", "w8": 440.0, "w9": 187.0, "w10": 365.0, "w11": 0}, {"name": "GLOVE NYLON PU SIZE M", "part": "127568", "w8": 331.0, "w9": 317.0, "w10": 335.0, "w11": 0}, {"name": "3M SANDPAPER DISC 775L #80 5\"", "part": "138465", "w8": 550.0, "w9": 50.0, "w10": 250.0, "w11": 0}, {"name": "PLUG EAR", "part": "902464", "w8": 400.0, "w9": 200.0, "w10": 200.0, "w11": 0}, {"name": "BAG PLASTIC 780X780MM T=0.07MM", "part": "479579", "w8": 0, "w9": 250.0, "w10": 500.0, "w11": 0}, {"name": "DYNEEMA GLOVES+PU S5 SIZE L", "part": "130353", "w8": 301.0, "w9": 191.0, "w10": 248.0, "w11": 0}, {"name": "GLOVE SIZE 7 HYFLEX FOAM", "part": "119551", "w8": 175.0, "w9": 235.0, "w10": 214.0, "w11": 0}, {"name": "HS CUT3 GLOVES M", "part": "138731", "w8": 178.0, "w9": 145.0, "w10": 225.0, "w11": 0}, {"name": "DYNEEMA GLOVES+PU S5 SIZE M", "part": "130352", "w8": 183.0, "w9": 157.0, "w10": 162.0, "w11": 0}, {"name": "PLASTIC BAG 16X27", "part": "137754", "w8": 500.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "GLOVE NYLON PU SIZE S", "part": "130943", "w8": 170.0, "w9": 125.0, "w10": 180.0, "w11": 0}, {"name": "GLOVE SIZE 8 HYFLEX FOAM", "part": "119552", "w8": 138.0, "w9": 159.0, "w10": 149.0, "w11": 0}, {"name": "PLASTIC BAG 40 MIC 1000X1000MM", "part": "136084", "w8": 175.0, "w9": 75.0, "w10": 175.0, "w11": 0}, {"name": "CABLE TIE 10\"", "part": "265616", "w8": 100.0, "w9": 0, "w10": 300.0, "w11": 0}, {"name": "GLOVE DYNEEMA XL", "part": "136261", "w8": 162.0, "w9": 96.0, "w10": 126.0, "w11": 0}, {"name": "RAG INTERLOCK TR17", "part": "125461", "w8": 125.0, "w9": 75.0, "w10": 175.0, "w11": 0}, {"name": "ISOPROPYL ALCOHOL - 5 Litre", "part": "121040006", "w8": 180.0, "w9": 80.0, "w10": 100.0, "w11": 0}, {"name": "STRETCH FILM 15MICX50CM 300M", "part": "127907", "w8": 84.0, "w9": 126.0, "w10": 102.0, "w11": 6.0}, {"name": "PAPER CUP 650Z", "part": "129368", "w8": 100.0, "w9": 200.0, "w10": 0, "w11": 0}, {"name": "HS CUT3 GLOVES S", "part": "138730", "w8": 75.0, "w9": 84.0, "w10": 75.0, "w11": 0}, {"name": "PLASTIC BAG 20X40", "part": "137756", "w8": 200.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "CHEMETALL METALL SPRAY 395L", "part": "130551", "w8": 0, "w9": 200.0, "w10": 0, "w11": 0}, {"name": "ROUND SAND PAPER DIA75MM P8000", "part": "130805", "w8": 0, "w9": 0, "w10": 200.0, "w11": 0}, {"name": "CABLE TIE 12\"", "part": "265617", "w8": 0, "w9": 0, "w10": 200.0, "w11": 0}, {"name": "HYDRATE LIME GRADE A 90%", "part": "130672", "w8": 0, "w9": 0, "w10": 200.0, "w11": 0}, {"name": "CABLE TIE 8\"", "part": "265615", "w8": 0, "w9": 0, "w10": 200.0, "w11": 0}, {"name": "EMERY P400 255P HOOK IT LD600A", "part": "265838", "w8": 0, "w9": 0, "w10": 200.0, "w11": 0}, {"name": "SAFETY DUCT MASK KN95", "part": "137926", "w8": 42.0, "w9": 50.0, "w10": 76.0, "w11": 0}, {"name": "WATER DISTILLED", "part": "127431", "w8": 0, "w9": 80.0, "w10": 40.0, "w11": 0}, {"name": "SAFETY ARM SLEEVES SIZE M", "part": "138007", "w8": 51.0, "w9": 12.0, "w10": 48.0, "w11": 1.0}, {"name": "CLEANER SPRAY KLEAR K-101", "part": "138839", "w8": 60.0, "w9": 48.0, "w10": 0, "w11": 0}, {"name": "CUTTER BLADE ISS207", "part": "136408", "w8": 100.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "WHITE PE PLASTIC BAG 50X65X100", "part": "140282", "w8": 50.0, "w9": 0, "w10": 50.0, "w11": 0}, {"name": "STEEL COIL STRAPPING", "part": "129583", "w8": 0, "w9": 100.0, "w10": 0, "w11": 0}, {"name": "CARTON CAVITY CAN", "part": "252746", "w8": 0, "w9": 0, "w10": 100.0, "w11": 0}, {"name": "CABLE TIE 4\"", "part": "265613", "w8": 0, "w9": 0, "w10": 100.0, "w11": 0}, {"name": "PLASTIC BAG 40 NUC 500X600MM", "part": "136092", "w8": 0, "w9": 0, "w10": 100.0, "w11": 0}, {"name": "MAGIE CLEAN", "part": "118907", "w8": 37.0, "w9": 44.0, "w10": 18.0, "w11": 0}, {"name": "LABEL REMOVER SPRAY", "part": "130744", "w8": 16.0, "w9": 48.0, "w10": 28.0, "w11": 0}, {"name": "HS CUT3 GLOVES L", "part": "138732", "w8": 27.0, "w9": 22.0, "w10": 40.0, "w11": 0}, {"name": "S-74 XEON 203 ANTI-RUST WHITE", "part": "129379", "w8": 24.0, "w9": 60.0, "w10": 0, "w11": 0}, {"name": "HEX DRIVE MAGNETIC TYPE", "part": "130540", "w8": 40.0, "w9": 10.0, "w10": 30.0, "w11": 0}, {"name": "WHEEL CUT OFF 100X1.0MM", "part": "817945", "w8": 30.0, "w9": 0, "w10": 50.0, "w11": 0}, {"name": "WHITE RAG SIZE A4", "part": "265692", "w8": 50.0, "w9": 10.0, "w10": 20.0, "w11": 0}, {"name": "CAP CARTON CAVITY CAN", "part": "252747", "w8": 0, "w9": 0, "w10": 75.0, "w11": 0}, {"name": "GLUE MACHINE MIXER LONG TYPE", "part": "130614", "w8": 10.0, "w9": 40.0, "w10": 20.0, "w11": 0}, {"name": "ML-11 MAINTAIN LUBRICANT 360ML", "part": "136737", "w8": 37.0, "w9": 17.0, "w10": 15.0, "w11": 0}, {"name": "TRAFFIC VEST V-SHAPE GREEN", "part": "137491", "w8": 32.0, "w9": 27.0, "w10": 1.0, "w11": 0}, {"name": "CYL GAS ARGON 130D", "part": "126529", "w8": 18.0, "w9": 16.0, "w10": 20.0, "w11": 0}, {"name": "SAFETY GLASSES DELTA", "part": "119685", "w8": 14.0, "w9": 12.0, "w10": 27.0, "w11": 0}, {"name": "WHITE PE PLASTIC BAG 36X45X100", "part": "140283", "w8": 25.0, "w9": 0, "w10": 25.0, "w11": 0}, {"name": "SAND PAPER WET&DRY T450,P2000", "part": "264115", "w8": 0, "w9": 50.0, "w10": 0, "w11": 0}, {"name": "THINNER CHEMETALL 501H", "part": "136755", "w8": 3.0, "w9": 44.0, "w10": 1.0, "w11": 0}, {"name": "SPRAY CRC BRAKLEEN  500g", "part": "113275", "w8": 12.0, "w9": 17.0, "w10": 19.0, "w11": 0}, {"name": "SPRAY SILICON CONCENTRATED", "part": "813446", "w8": 0, "w9": 48.0, "w10": 0, "w11": 0}, {"name": "SAFETY CUT RESISTANT ARM SIZEL", "part": "138225", "w8": 34.0, "w9": 0, "w10": 13.0, "w11": 0}, {"name": "MARKER BLUE BROAD & FINE PER", "part": "140305", "w8": 16.0, "w9": 13.0, "w10": 17.0, "w11": 0}, {"name": "BIT SQUARE 50MM GFB98-B", "part": "803683", "w8": 0, "w9": 20.0, "w10": 25.0, "w11": 0}, {"name": "GLOVE SIZE 9 HYFLEX FOAM", "part": "119553", "w8": 14.0, "w9": 15.0, "w10": 14.0, "w11": 0}, {"name": "BROOM", "part": "266808", "w8": 11.0, "w9": 17.0, "w10": 15.0, "w11": 0}, {"name": "SAFETY CUT RESISTANT GLOVES S", "part": "138006", "w8": 32.0, "w9": 9.0, "w10": 0, "w11": 0}, {"name": "LION PVC TAPE BLUE 2\" W GLUE", "part": "130630", "w8": 9.0, "w9": 15.0, "w10": 16.0, "w11": 0}, {"name": "GRINDING 4\"X2MM", "part": "136890", "w8": 10.0, "w9": 10.0, "w10": 20.0, "w11": 0}, {"name": "WIREMESH OD115MM 6LAYERS", "part": "140039", "w8": 0, "w9": 20.0, "w10": 20.0, "w11": 0}, {"name": "PAINT MARKER YELLOW", "part": "130558", "w8": 1.0, "w9": 0, "w10": 37.0, "w11": 0}, {"name": "SAFETY STICKER PVC HAZ CHEM", "part": "138234", "w8": 0, "w9": 20.0, "w10": 14.0, "w11": 0}, {"name": "BIT TORX T15 L100", "part": "140182", "w8": 10.0, "w9": 10.0, "w10": 10.0, "w11": 0}, {"name": "WOOD BRUSH", "part": "130879", "w8": 15.0, "w9": 15.0, "w10": 0, "w11": 0}, {"name": "LAMINATED PLASTIC ADHESIVE", "part": "612205", "w8": 11.0, "w9": 5.0, "w10": 11.0, "w11": 0}, {"name": "ARGONGLOVE", "part": "136600", "w8": 6.0, "w9": 10.0, "w10": 10.0, "w11": 0}, {"name": "BITS PHILLIPS#2X150 VESSEL", "part": "130602", "w8": 20.0, "w9": 5.0, "w10": 0, "w11": 0}, {"name": "CORROSION INHIBITOR HL410", "part": "136330", "w8": 0, "w9": 0, "w10": 25.0, "w11": 0}, {"name": "FABRIC", "part": "136766", "w8": 0, "w9": 0, "w10": 25.0, "w11": 0}, {"name": "GLOVES ANTISTATIC DUSTFREE SM", "part": "175040102", "w8": 12.0, "w9": 12.0, "w10": 0, "w11": 0}, {"name": "GREEN CARBON MASK", "part": "129193", "w8": 0, "w9": 0, "w10": 24.0, "w11": 0}, {"name": "INDUSTRIAL FAN 24IN DFP600 TW", "part": "130569", "w8": 4.0, "w9": 10.0, "w10": 9.0, "w11": 0}, {"name": "BIT DRIVER PHILLIPS", "part": "901271", "w8": 0, "w9": 20.0, "w10": 2.0, "w11": 0}, {"name": "RAG TR13", "part": "472649", "w8": 11.0, "w9": 4.0, "w10": 6.0, "w11": 0}, {"name": "BLADE HACKSAW ECLIPSE 24T", "part": "160481", "w8": 10.0, "w9": 10.0, "w10": 0, "w11": 0}, {"name": "ABRASIVE BELT #237AA", "part": "130192", "w8": 0, "w9": 5.0, "w10": 15.0, "w11": 0}, {"name": "SAFETY GLASSES", "part": "137642", "w8": 0, "w9": 20.0, "w10": 0, "w11": 0}, {"name": "TAG HOLD A5 YELLOW", "part": "140177", "w8": 5.0, "w9": 10.0, "w10": 1.0, "w11": 0}, {"name": "DUSTPAN", "part": "266814", "w8": 11.0, "w9": 0, "w10": 5.0, "w11": 0}, {"name": "ANTI STATIC SAFETY SHOES #42", "part": "130303", "w8": 8.0, "w9": 4.0, "w10": 4.0, "w11": 0}, {"name": "TAPE ELECT BLACK1600 19XMMX20M", "part": "130466", "w8": 0, "w9": 16.0, "w10": 0, "w11": 0}, {"name": "PROTECTION FILM FAB", "part": "130144", "w8": 5.0, "w9": 5.0, "w10": 5.0, "w11": 0}, {"name": "LPG GAS 48KG 2VALVE TYPE", "part": "130566", "w8": 5.0, "w9": 5.0, "w10": 5.0, "w11": 0}, {"name": "APRON", "part": "129467", "w8": 4.0, "w9": 6.0, "w10": 5.0, "w11": 0}, {"name": "FOAM TAPE 3X95X20", "part": "136610", "w8": 6.0, "w9": 4.0, "w10": 4.0, "w11": 0}, {"name": "TAPE 84MM L40YARD", "part": "137448", "w8": 2.0, "w9": 6.0, "w10": 6.0, "w11": 0}, {"name": "BUBBLE WRAP", "part": "136180", "w8": 2.0, "w9": 6.0, "w10": 6.0, "w11": 0}, {"name": "PADLOCK 32MM BOX L28", "part": "125204", "w8": 1.0, "w9": 7.0, "w10": 5.0, "w11": 0}, {"name": "MARKER RED BROAD & FINE PER", "part": "140306", "w8": 6.0, "w9": 3.0, "w10": 3.0, "w11": 0}, {"name": "CLEANER CLEANING 900", "part": "118906", "w8": 3.0, "w9": 1.0, "w10": 8.0, "w11": 0}, {"name": "ABRASIVE WHEELS 8X1X1 #80", "part": "130821", "w8": 0, "w9": 12.0, "w10": 0, "w11": 0}, {"name": "CORROSION SCALE INHIBITORHL308", "part": "136331", "w8": 0, "w9": 0, "w10": 12.0, "w11": 0}, {"name": "MICROBIOCIDE HL 309", "part": "136332", "w8": 0, "w9": 0, "w10": 12.0, "w11": 0}, {"name": "SFC BLT A CRS 5.5\"X8300MM", "part": "138761", "w8": 7.0, "w9": 2.0, "w10": 2.0, "w11": 0}, {"name": "LEAK DETECTOR SPRAY FOAM", "part": "130743", "w8": 5.0, "w9": 5.0, "w10": 1.0, "w11": 0}, {"name": "BABY POWDER D-NEE 15oz", "part": "244904", "w8": 3.0, "w9": 5.0, "w10": 3.0, "w11": 0}, {"name": "FORM ISSUING PART ENGINEERING", "part": "130383", "w8": 1.0, "w9": 4.0, "w10": 6.0, "w11": 0}, {"name": "SAFETY STICKER PVC", "part": "138232", "w8": 0, "w9": 11.0, "w10": 0, "w11": 0}, {"name": "SCOTCH BRITE BELT BRUSH", "part": "130874", "w8": 9.0, "w9": 1.0, "w10": 0, "w11": 0}, {"name": "ANTI STATIC SAFETY SHOES #43", "part": "130304", "w8": 3.0, "w9": 4.0, "w10": 3.0, "w11": 0}, {"name": "CHEMICAL PROTECTIVE CLOTH (L)", "part": "129306", "w8": 7.0, "w9": 3.0, "w10": 0, "w11": 0}], "ENG": [{"name": "SALT", "part": "129362", "w8": 0, "w9": 0, "w10": 500.0, "w11": 0}, {"name": "O-RING 015 N70 9/16*1/16", "part": "112014", "w8": 0, "w9": 300.0, "w10": 0, "w11": 0}, {"name": "OIL SORBENT", "part": "266348", "w8": 100.0, "w9": 0, "w10": 100.0, "w11": 0}, {"name": "TUBE 4MM POLYURETHANE  BLUE", "part": "124283", "w8": 100.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "PRESS SPRING #9 FP 053053", "part": "130219", "w8": 100.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "LUG PIN STARFIX 1.0MM RED", "part": "161774", "w8": 100.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "SCREW CAP HT BLACK M6X40", "part": "160090", "w8": 0, "w9": 0, "w10": 80.0, "w11": 0}, {"name": "APPLIANCE FLEX 16SQMM EARTH", "part": "129469", "w8": 0, "w9": 50.0, "w10": 0, "w11": 0}, {"name": "SCREW CAP HT BLACK M6X80", "part": "160138", "w8": 0, "w9": 0, "w10": 50.0, "w11": 0}, {"name": "VCT CABLE 2X2.5/1.5", "part": "266325", "w8": 40.0, "w9": 0, "w10": 4.0, "w11": 0}, {"name": "ELBOW CONNECTOR QPL10-02", "part": "138082", "w8": 40.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "ELBOW CONNECTOR QPL12-02", "part": "138083", "w8": 40.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "STRAIGHT CONNECTOR QPC10-02", "part": "138078", "w8": 40.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "STRAIGHT CONNECTOR QPC12-02", "part": "138079", "w8": 40.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "CLAMP HOSE JUBILEE 13-20MM ZP", "part": "160722", "w8": 0, "w9": 40.0, "w10": 0, "w11": 0}, {"name": "WASHER FLAT 6MM ZNPL", "part": "150279", "w8": 0, "w9": 40.0, "w10": 0, "w11": 0}, {"name": "WASHER FLAT 8MM ZNPL", "part": "150280", "w8": 0, "w9": 40.0, "w10": 0, "w11": 0}, {"name": "WASHER FLAT 10MM ZNPL", "part": "150281", "w8": 0, "w9": 40.0, "w10": 0, "w11": 0}, {"name": "WASHER FLAT 12MM ZNPL", "part": "150282", "w8": 0, "w9": 40.0, "w10": 0, "w11": 0}, {"name": "WASHER FLAT 16MM ZNPL", "part": "150283", "w8": 0, "w9": 40.0, "w10": 0, "w11": 0}, {"name": "ORBIT 1A SIZE 22-29MM", "part": "136335", "w8": 0, "w9": 0, "w10": 40.0, "w11": 0}, {"name": "LAPP CABLE GLAND CABLE 20X1.5", "part": "130344", "w8": 29.0, "w9": 6.0, "w10": 2.0, "w11": 0}, {"name": "OSRAM 26W/840 DULUX D", "part": "129048", "w8": 10.0, "w9": 0, "w10": 20.0, "w11": 0}, {"name": "QUICK COUPLING ST 45 HOSE 13MM", "part": "140315", "w8": 0, "w9": 30.0, "w10": 0, "w11": 0}, {"name": "BALL LIFTER PRESS MACHINE", "part": "136747", "w8": 25.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "PLUG 3 PIN 16A 240V", "part": "129020", "w8": 15.0, "w9": 2.0, "w10": 4.0, "w11": 0}, {"name": "TUBE 12MM POLYURTHANE  BLUE", "part": "124287", "w8": 0, "w9": 20.0, "w10": 0, "w11": 0}, {"name": "QUICK COUPLING BRASS HOSE 13MM", "part": "140314", "w8": 0, "w9": 20.0, "w10": 0, "w11": 0}, {"name": "BRASS BRUSH 0DD SUB ASSEMBLY", "part": "130660", "w8": 0, "w9": 12.0, "w10": 7.0, "w11": 0}, {"name": "STATIC MIXER", "part": "140102", "w8": 5.0, "w9": 10.0, "w10": 0, "w11": 0}, {"name": "CUP VACUUM VC89A", "part": "160939", "w8": 0, "w9": 0, "w10": 15.0, "w11": 0}, {"name": "PLIER MINI KEIBA MNA044", "part": "136217", "w8": 5.0, "w9": 5.0, "w10": 3.0, "w11": 0}, {"name": "MALE ELBOW KQL08-02S F/TUBE", "part": "161812", "w8": 12.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "SCREW SOCKET HEAD CAP M12X90", "part": "160217", "w8": 0, "w9": 12.0, "w10": 0, "w11": 0}, {"name": "TWIN SOCKET 16A 240V", "part": "129025", "w8": 11.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "PLUG JIFFY TITE JP 352 SV", "part": "162245", "w8": 0, "w9": 11.0, "w10": 0, "w11": 0}, {"name": "CUP VACUUM S/ LOADER GEISS", "part": "160937", "w8": 0, "w9": 11.0, "w10": 0, "w11": 0}, {"name": "OIL SAMPLE BOTTLE", "part": "129164", "w8": 0, "w9": 0, "w10": 11.0, "w11": 0}, {"name": "HEADSET 8801432 LOGITECH960USB", "part": "138417", "w8": 8.0, "w9": 1.0, "w10": 1.0, "w11": 0}, {"name": "BUSH REDUC 3/8-1/4 BSP BRASS", "part": "160554", "w8": 10.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "CONNECTOR MALE SMC M6 X 1/8", "part": "128608", "w8": 10.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "CONNECTOR MALE SMC M4X1/4", "part": "126593", "w8": 10.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "ELBOW SMC - MALE M8 X 1/8", "part": "132654", "w8": 10.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "PLUG KQP-06 F/TUBE SMC", "part": "162248", "w8": 10.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "STRAIGHT UNION KQH04-00 F/TUBE", "part": "162798", "w8": 10.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "MALE CONNECTOR KQH08-01 F/TUBE", "part": "161801", "w8": 0, "w9": 10.0, "w10": 0, "w11": 0}, {"name": "CLIP LGE CROC/ALIG", "part": "160748", "w8": 0, "w9": 10.0, "w10": 0, "w11": 0}, {"name": "POLYPROPYLENE FILTER PMB5-40\"", "part": "130525", "w8": 0, "w9": 0, "w10": 10.0, "w11": 0}, {"name": "POLYPROPYLENE FILTER PMB5-10\"", "part": "130503", "w8": 0, "w9": 0, "w10": 10.0, "w11": 0}, {"name": "ELBOW SMC - MALE  M6 X 1/4", "part": "128652", "w8": 0, "w9": 0, "w10": 10.0, "w11": 0}, {"name": "CONNECTOR MALE SMC M6 X 1/4", "part": "132609", "w8": 0, "w9": 0, "w10": 10.0, "w11": 0}, {"name": "ELBOW KQLO4-M5 F/TUBE", "part": "161058", "w8": 0, "w9": 0, "w10": 10.0, "w11": 0}, {"name": "SAFETY PADLOCK", "part": "137708", "w8": 9.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "NIPPLE 1/4 BSP GALV FITTING", "part": "161921", "w8": 0, "w9": 9.0, "w10": 0, "w11": 0}, {"name": "OIL HYSPIN AWS 46 200 LITRE", "part": "123866", "w8": 4.0, "w9": 0, "w10": 4.0, "w11": 0}, {"name": "ELBOW AS1201F-M5-6MM", "part": "161045", "w8": 8.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "LAPP CBLE GLAND CABLE 25X1.5", "part": "130345", "w8": 2.0, "w9": 6.0, "w10": 0, "w11": 0}, {"name": "DRILL 2.5mm HSS", "part": "112436", "w8": 0, "w9": 3.0, "w10": 5.0, "w11": 0}, {"name": "BOLT BLACK HEX 8.8 M10X50", "part": "160060", "w8": 0, "w9": 0, "w10": 8.0, "w11": 0}, {"name": "NUT HEX BLACK 8.8 M10", "part": "160147", "w8": 0, "w9": 0, "w10": 8.0, "w11": 0}, {"name": "UNION  SMC  \"Y\" M10 KQU10-00", "part": "128679", "w8": 7.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "4050-3P QUICK COUPLING 3/8", "part": "130318", "w8": 3.0, "w9": 0, "w10": 3.0, "w11": 0}, {"name": "BUSH REDUC 1/2-3/8 BSP BRASS", "part": "160545", "w8": 6.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "PLUG-IN REDUC KQR04-06 F/TUBE", "part": "162274", "w8": 6.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "ELBOW MALE KQL04-01S F/TUBE", "part": "161064", "w8": 6.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "FILTER MANN C 15 124/1", "part": "121523", "w8": 6.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "SWITCH BOX 2\"X4\"COLOR WHITE", "part": "137007", "w8": 6.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "NIPPLE 3/8 BSP GALV FITTING", "part": "161928", "w8": 0, "w9": 6.0, "w10": 0, "w11": 0}, {"name": "EXHAUST FILTER 0532 140 157", "part": "267048", "w8": 0, "w9": 0, "w10": 6.0, "w11": 0}, {"name": "ROLLER BEARING 3201A-2Z WAFIOS", "part": "138899", "w8": 0, "w9": 0, "w10": 6.0, "w11": 0}, {"name": "TEE BRANCH SMC M8 X 1/4", "part": "128666", "w8": 0, "w9": 0, "w10": 6.0, "w11": 0}, {"name": "DRILL 3.2mm HSS", "part": "112431", "w8": 3.0, "w9": 2.0, "w10": 0, "w11": 0}, {"name": "ELBOW KQL06-M5 TUBE SMC", "part": "161057", "w8": 5.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "SUCTION CUP GEISS10 SIZE 60MM", "part": "138879", "w8": 5.0, "w9": 0, "w10": 0, "w11": 0}, {"name": "DRILL 5.2mm HSS", "part": "112401", "w8": 0, "w9": 5.0, "w10": 0, "w11": 0}]}};

// ── STATE ─────────────────────────────────────────────────────────────────────
let selWeeks = [10, 11];
let sortKey  = 'total';
let sortDir  = -1;

// ── WEEK PILLS ────────────────────────────────────────────────────────────────
function buildPills() {
  const c = document.getElementById('weekPills');
  c.innerHTML = '';
  c.style.display = 'flex';
  c.style.gap = '6px';
  DB.weeks.forEach(w => {
    const b = document.createElement('button');
    b.className = 'wpill';
    b.textContent = 'W' + w;
    b.dataset.week = w;
    b.onclick = () => toggleWeek(w);
    c.appendChild(b);
  });
  updatePillStyles();
}

function updatePillStyles() {
  document.querySelectorAll('.wpill').forEach(b => {
    const w = +b.dataset.week;
    b.classList.remove('sel-a','sel-b');
    if (selWeeks[0] === w) b.classList.add('sel-a');
    if (selWeeks[1] === w) b.classList.add('sel-b');
  });
  const tag = document.getElementById('compareTag');
  if (selWeeks.length === 2) {
    tag.style.display = '';
    tag.innerHTML = `เปรียบเทียบ <span class="wa">W${selWeeks[0]}</span> vs <span class="wb">W${selWeeks[1]}</span>`;
  } else if (selWeeks.length === 1) {
    tag.style.display = '';
    tag.innerHTML = `แสดง <span class="wa">W${selWeeks[0]}</span>`;
  } else {
    tag.style.display = 'none';
  }
}

function toggleWeek(w) {
  const idx = selWeeks.indexOf(w);
  if (idx >= 0) {
    selWeeks.splice(idx, 1);
  } else {
    if (selWeeks.length >= 2) selWeeks.shift();
    selWeeks.push(w);
    selWeeks.sort((a,b) => a-b);
  }
  updatePillStyles();
  render();
}

// ── RENDER ────────────────────────────────────────────────────────────────────
function fmt(n) {
  if (!n || n === 0) return '<span class="zero">—</span>';
  return n.toLocaleString('en-US');
}

function renderGroup(group, chEl, rowsEl, footEl, metaEl) {
  const search = document.getElementById('searchInput').value.toLowerCase();
  const hasB = selWeeks.length === 2;
  const wA = selWeeks[0], wB = selWeeks[1];

  if (selWeeks.length === 0) {
    chEl.innerHTML = '';
    rowsEl.innerHTML = '<div class="no-sel"><div class="big">📅</div><p>เลือก Week ด้านบน</p></div>';
    footEl.innerHTML = '';
    metaEl.innerHTML = '<span>—</span> รายการ';
    return;
  }

  // Col headers
  if (hasB) {
    chEl.className = 'col-header has-b';
    chEl.innerHTML = `
      <span class="ch">#</span>
      <span class="ch">Part No.</span>
      <span class="ch sortable" onclick="setSort('name')" style="color:${sortKey==='name'?'var(--accent)':''}">รายการ ${sortKey==='name'?'↕':''}</span>
      <span class="ch r wa">W${wA}</span>
      <span class="ch r wb">W${wB}</span>
      <span class="ch r sortable" onclick="setSort('change')" style="color:${sortKey==='change'?'var(--accent)':'var(--text-dim)'}">เปลี่ยน ${sortKey==='change'?'↕':''}</span>
      <span class="ch r" style="color:var(--text-dim)">%</span>
    `;
  } else {
    chEl.className = 'col-header no-b';
    chEl.innerHTML = `
      <span class="ch">#</span>
      <span class="ch">Part No.</span>
      <span class="ch sortable" onclick="setSort('name')">รายการ</span>
      <span class="ch r wa">W${wA}</span>
    `;
  }

  let items = DB.data[group].filter(item => {
    if (search) {
      const matchName = item.name.toLowerCase().includes(search);
      const matchPart = (item.part||'').toLowerCase().includes(search);
      if (!matchName && !matchPart) return false;
    }
    const vA = item[`w${wA}`] || 0;
    const vB = hasB ? (item[`w${wB}`] || 0) : 0;
    return vA > 0 || vB > 0;
  });

  // Sort
  items = [...items];
  if (sortKey === 'name') {
    items.sort((a,b) => sortDir * a.name.localeCompare(b.name));
  } else if (sortKey === 'change' && hasB) {
    items.sort((a,b) => sortDir * (((b[`w${wB}`]||0)-(b[`w${wA}`]||0)) - ((a[`w${wB}`]||0)-(a[`w${wA}`]||0))));
  } else {
    items.sort((a,b) => sortDir * ((b[`w${wA}`]||0) - (a[`w${wA}`]||0)));
  }

  if (items.length === 0) {
    rowsEl.innerHTML = '<div class="no-sel"><p>ไม่พบรายการ</p></div>';
    metaEl.innerHTML = '<span>0</span> รายการ';
    footEl.innerHTML = '';
    return;
  }

  let totalA = 0, totalB = 0, cUp = 0, cDn = 0, html = '';

  items.forEach((item, i) => {
    const vA = item[`w${wA}`] || 0;
    const vB = hasB ? (item[`w${wB}`] || 0) : 0;
    totalA += vA; totalB += vB;

    let arrowCell = '', pctCell = '';
    if (hasB) {
      const diff = vB - vA;
      const pct  = vA > 0 ? ((diff/vA)*100).toFixed(1) : (vB > 0 ? null : 0);
      if (diff > 0) {
        cUp++;
        arrowCell = `<div class="arrow-cell arr-up">▲ +${diff.toLocaleString('en-US')}</div>`;
        pctCell   = `<div class="pct-cell pct-up">${pct !== null ? '+'+pct+'%' : '—'}</div>`;
      } else if (diff < 0) {
        cDn++;
        arrowCell = `<div class="arrow-cell arr-down">▼ ${diff.toLocaleString('en-US')}</div>`;
        pctCell   = `<div class="pct-cell pct-down">${pct !== null ? pct+'%' : '—'}</div>`;
      } else {
        arrowCell = `<div class="arrow-cell arr-same">━ 0</div>`;
        pctCell   = `<div class="pct-cell pct-same">0.0%</div>`;
      }
    }

    const rowClass = hasB ? 'data-row has-b' : 'data-row no-b';
    const partBadge = `<div class="part-badge" title="${item.part||''}">${item.part||'—'}</div>`;
    const vBCell = hasB ? `<div class="cell-num ${vB===0?'zero':''}">${fmt(vB)}</div>` : '';

    html += `<div class="${rowClass}">
      <div class="row-num">${i+1}</div>
      ${partBadge}
      <div class="item-name">${item.name}</div>
      <div class="cell-num ${vA===0?'zero':''}">${fmt(vA)}</div>
      ${vBCell}${arrowCell}${pctCell}
    </div>`;
  });

  rowsEl.innerHTML = html;
  metaEl.innerHTML = `<span>${items.length}</span> รายการ`;

  // Footer
  let foot = `<span class="foot-stat">รวม W${wA}: <b>${totalA.toLocaleString()}</b></span>`;
  if (hasB) {
    const d = totalB - totalA;
    foot += `<span class="foot-stat">รวม W${wB}: <b>${totalB.toLocaleString()}</b></span>`;
    foot += `<span class="foot-stat">ผลต่าง: <b style="color:${d>0?'var(--up)':d<0?'var(--down)':'var(--neutral)'}">${d>=0?'+':''}${d.toLocaleString()}</b></span>`;
    foot += `<span class="foot-stat up">▲ เพิ่ม: <b>${cUp}</b></span>`;
    foot += `<span class="foot-stat dn">▼ ลด: <b>${cDn}</b></span>`;
  }
  footEl.innerHTML = foot;
}

function render() {
  renderGroup('CONS',
    document.getElementById('chCONS'),
    document.getElementById('rowsCONS'),
    document.getElementById('footCONS'),
    document.getElementById('metaCONS')
  );
  renderGroup('ENG',
    document.getElementById('chENG'),
    document.getElementById('rowsENG'),
    document.getElementById('footENG'),
    document.getElementById('metaENG')
  );
}

function setSort(key) {
  if (sortKey === key) sortDir *= -1;
  else { sortKey = key; sortDir = -1; }
  render();
}

// ── UPLOAD ────────────────────────────────────────────────────────────────────
function handleUpload(e) {
  const file = e.target.files[0];
  if (!file) return;
  closeOverlay();

  const reader = new FileReader();
  reader.onload = function(ev) {
    try {
      if (typeof XLSX === 'undefined') {
        const s = document.createElement('script');
        s.src = 'https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js';
        s.onload = () => parseExcel(ev.target.result);
        document.head.appendChild(s);
      } else {
        parseExcel(ev.target.result);
      }
    } catch(err) { alert('อ่านไฟล์ไม่ได้: ' + err.message); }
  };
  reader.readAsArrayBuffer(file);
  e.target.value = '';
}

function parseExcel(buffer) {
  const wb = XLSX.read(buffer, { type: 'array', cellDates: true });

  // Support both sheet names
  const wsName = wb.SheetNames.includes('Data base') ? 'Data base' : wb.SheetNames[0];
  const ws = wb.Sheets[wsName];
  if (!ws) { alert('ไม่พบ Sheet ที่ใช้งานได้'); return; }

  const rows = XLSX.utils.sheet_to_json(ws, { raw: false });
  if (!rows.length) { alert('ไม่มีข้อมูลใน Sheet'); return; }

  // Detect columns
  const sampleRow = rows[0];
  const colKeys = Object.keys(sampleRow);

  // Find column mappings
  const findCol = (...names) => colKeys.find(k => names.some(n => k.trim().toLowerCase() === n.toLowerCase())) || null;
  const colWeek = findCol('Week', 'WeekNumber');
  const colName = findCol('Description', 'ItemName');
  const colPart = findCol('Part Number', 'ItemCode', 'PartNumber');
  const colGrp  = findCol('CONS/ENG', 'ProductGroup');
  const colQty  = findCol('Units', 'Quantity');

  const agg = {};
  const weeks = new Set();

  rows.forEach(r => {
    const w    = parseInt(r[colWeek] || 0);
    const name = (r[colName] || '').toString().trim();
    const part = (r[colPart] || '').toString().trim();
    const grp  = (r[colGrp]  || '').toString().trim();
    const qty  = Math.abs(parseFloat(r[colQty] || 0));
    if (!w || !name || !grp || !qty) return;
    if (!['CONS','ENG'].includes(grp)) return;
    weeks.add(w);
    const k = `${grp}||${name}`;
    if (!agg[k]) agg[k] = { name, part, grp, weeks: {} };
    agg[k].weeks[w] = (agg[k].weeks[w] || 0) + qty;
    // update part if we have one
    if (part && (!agg[k].part || agg[k].part === 'None')) agg[k].part = part;
  });

  const sortedWeeks = [...weeks].sort((a,b) => a-b).filter(w => {
    const cnt = Object.values(agg).filter(x => x.weeks[w]).length;
    return cnt > 3;
  });

  if (!sortedWeeks.length) { alert('ไม่พบข้อมูลที่สมบูรณ์'); return; }

  const newData = { CONS: [], ENG: [] };
  Object.values(agg).forEach(({ name, part, grp, weeks: wd }) => {
    if (!newData[grp]) return;
    const item = { name, part: part || '' };
    sortedWeeks.forEach(w => { item[`w${w}`] = wd[w] || 0; });
    newData[grp].push(item);
  });
  for (const g of ['CONS','ENG']) {
    newData[g].sort((a,b) => {
      const sa = sortedWeeks.reduce((s,w) => s+(a[`w${w}`]||0), 0);
      const sb = sortedWeeks.reduce((s,w) => s+(b[`w${w}`]||0), 0);
      return sb - sa;
    });
  }

  DB = { weeks: sortedWeeks, data: newData };
  selWeeks = sortedWeeks.length >= 2
    ? [sortedWeeks[sortedWeeks.length-2], sortedWeeks[sortedWeeks.length-1]]
    : sortedWeeks.slice();

  buildPills();
  render();
  alert(`✅ โหลดข้อมูลสำเร็จ! Week: ${sortedWeeks.map(w=>'W'+w).join(', ')}`);
}

function closeOverlay() {
  document.getElementById('uploadOverlay').classList.remove('show');
}

// close overlay on background click
document.getElementById('uploadOverlay').addEventListener('click', function(e) {
  if (e.target === this) closeOverlay();
});

// ── INIT ──────────────────────────────────────────────────────────────────────
buildPills();
render();
</script>
</body>
</html>
