# planting-1-
<!DOCTYPE html>

<html lang="nl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tuin Planner</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Mono:ital,wght@0,400;0,500;1,400&family=Fraunces:wght@700;900&display=swap');

:root {
–bg: #f2ebe0;
–surface: #fffdf7;
–border: #c8b49a;
–wood: #9b6e3a;
–wood-dark: #6b4420;
–soil: #8b5e2a;
–grass: #6a9e3a;
–text: #2d1f0e;
–muted: #8a7058;
–accent: #c44e1a;
–trellis: #c49a50;
–panel: #fffaf2;
}

- { margin: 0; padding: 0; box-sizing: border-box; }

body {
background: var(–bg);
font-family: ‘DM Mono’, monospace;
color: var(–text);
height: 100vh;
display: flex;
flex-direction: column;
overflow: hidden;
}

header {
background: var(–wood-dark);
padding: 10px 20px;
display: flex;
align-items: center;
gap: 16px;
flex-shrink: 0;
border-bottom: 3px solid var(–wood);
}

header h1 {
font-family: ‘Fraunces’, serif;
font-size: 22px;
color: #f5e8d0;
letter-spacing: 0.01em;
}

header span {
font-size: 10px;
color: #c4a472;
letter-spacing: 0.1em;
text-transform: uppercase;
margin-top: 2px;
}

.toolbar {
background: var(–wood);
padding: 8px 16px;
display: flex;
gap: 8px;
align-items: center;
flex-wrap: wrap;
flex-shrink: 0;
border-bottom: 2px solid var(–wood-dark);
}

.tool-label {
font-size: 9px;
color: #f5e8d0;
letter-spacing: 0.12em;
text-transform: uppercase;
margin-right: 4px;
opacity: 0.7;
}

.btn {
background: var(–wood-dark);
color: #f5e8d0;
border: 1.5px solid #f5e8d044;
border-radius: 3px;
padding: 5px 12px;
font-family: ‘DM Mono’, monospace;
font-size: 11px;
cursor: pointer;
letter-spacing: 0.06em;
transition: all 0.15s;
white-space: nowrap;
}

.btn:hover {
background: #f5e8d0;
color: var(–wood-dark);
border-color: #f5e8d0;
}

.btn.danger {
border-color: #ff6b4a88;
color: #ffb09a;
}
.btn.danger:hover {
background: #c44e1a;
color: white;
border-color: #c44e1a;
}

.divider {
width: 1px;
height: 24px;
background: #f5e8d033;
margin: 0 4px;
}

.main {
display: flex;
flex: 1;
overflow: hidden;
}

/* CANVAS */
.canvas-wrap {
flex: 1;
overflow: auto;
padding: 20px;
position: relative;
}

#garden {
position: relative;
background:
repeating-linear-gradient(0deg, transparent, transparent 39px, #8ab86422 39px, #8ab86422 40px),
repeating-linear-gradient(90deg, transparent, transparent 39px, #8ab86422 39px, #8ab86422 40px),
#a8c874;
border: 3px solid var(–wood-dark);
border-radius: 4px;
box-shadow: 6px 8px 0 var(–wood-dark);
cursor: crosshair;
overflow: hidden;
}

/* Grass texture overlay */
#garden::before {
content: ‘’;
position: absolute;
inset: 0;
background-image: radial-gradient(circle, #7ab85488 1px, transparent 1px);
background-size: 12px 12px;
pointer-events: none;
z-index: 0;
}

/* Scale ruler */
.ruler-x {
position: absolute;
top: 0; left: 0; right: 0;
height: 18px;
background: #f5e8d0cc;
border-bottom: 1px solid var(–border);
display: flex;
align-items: flex-end;
font-size: 8px;
color: var(–muted);
letter-spacing: 0.05em;
z-index: 50;
pointer-events: none;
}

/* BEDS */
.bed {
position: absolute;
cursor: move;
user-select: none;
z-index: 10;
}

.bed-inner {
width: 100%;
height: 100%;
border-radius: 3px;
border: 2.5px solid var(–wood-dark);
position: relative;
display: flex;
align-items: center;
justify-content: center;
flex-direction: column;
overflow: hidden;
transition: box-shadow 0.1s;
}

.bed.selected .bed-inner {
border-color: var(–accent);
box-shadow: 0 0 0 2px var(–accent), 0 4px 16px #00000033;
}

.bed.type-bed .bed-inner {
background:
radial-gradient(ellipse at 30% 40%, #8b5e2a55 0%, transparent 60%),
radial-gradient(ellipse at 70% 60%, #6b4420aa 0%, transparent 50%),
repeating-linear-gradient(45deg, #a07040 0px, #a07040 2px, #8b5e2a 2px, #8b5e2a 8px);
}

.bed.type-trellis .bed-inner {
background: #e8d8b8;
}

/* Trellis grid lines */
.bed.type-trellis .bed-inner::before {
content: ‘’;
position: absolute;
inset: 0;
background-image:
repeating-linear-gradient(0deg, transparent, transparent 13px, #c4943a99 13px, #c4943a99 15px),
repeating-linear-gradient(90deg, transparent, transparent 13px, #c4943a99 13px, #c4943a99 15px);
border-radius: 2px;
}

.bed.type-border .bed-inner {
background:
repeating-linear-gradient(90deg, #9a6030 0px, #9a6030 2px, #8b5020 2px, #8b5020 10px);
}

.bed-label {
font-size: 10px;
font-weight: 500;
color: #f5e8d0;
text-shadow: 1px 1px 0 #00000066;
text-align: center;
line-height: 1.3;
padding: 2px;
z-index: 1;
pointer-events: none;
}

.bed.type-trellis .bed-label {
color: #5a3a10;
text-shadow: none;
font-style: italic;
}

.bed-dims {
font-size: 8px;
color: #f5e8d0bb;
text-shadow: 1px 1px 0 #00000066;
margin-top: 1px;
z-index: 1;
pointer-events: none;
}

.bed.type-trellis .bed-dims {
color: #7a5a2abb;
text-shadow: none;
}

/* Resize handle */
.resize-handle {
position: absolute;
bottom: 0; right: 0;
width: 14px; height: 14px;
cursor: se-resize;
z-index: 20;
}

.resize-handle::after {
content: ‘’;
position: absolute;
bottom: 3px; right: 3px;
width: 8px; height: 8px;
border-right: 2.5px solid #f5e8d0;
border-bottom: 2.5px solid #f5e8d0;
border-radius: 0 0 2px 0;
}

.bed.type-trellis .resize-handle::after {
border-color: #7a5a2a;
}

/* SIDEBAR */
.sidebar {
width: 220px;
flex-shrink: 0;
background: var(–panel);
border-left: 2px solid var(–border);
display: flex;
flex-direction: column;
overflow-y: auto;
}

.sidebar-section {
padding: 14px;
border-bottom: 1px solid var(–border);
}

.sidebar-section h3 {
font-family: ‘Fraunces’, serif;
font-size: 13px;
color: var(–wood-dark);
margin-bottom: 10px;
letter-spacing: 0.02em;
}

.field-row {
margin-bottom: 8px;
}

.field-row label {
display: block;
font-size: 9px;
color: var(–muted);
text-transform: uppercase;
letter-spacing: 0.1em;
margin-bottom: 3px;
}

.field-row input[type=“text”],
.field-row input[type=“number”] {
width: 100%;
background: var(–bg);
border: 1.5px solid var(–border);
border-radius: 2px;
padding: 5px 8px;
font-family: ‘DM Mono’, monospace;
font-size: 11px;
color: var(–text);
outline: none;
transition: border-color 0.15s;
}

.field-row input:focus {
border-color: var(–wood);
}

.field-row .two-col {
display: grid;
grid-template-columns: 1fr 1fr;
gap: 6px;
}

.no-selection {
font-size: 10px;
color: var(–muted);
font-style: italic;
line-height: 1.6;
}

.stats-row {
display: flex;
justify-content: space-between;
font-size: 9px;
color: var(–muted);
margin-bottom: 4px;
}

.stats-row span:last-child {
color: var(–wood-dark);
font-weight: 500;
}

.type-pill {
display: inline-block;
padding: 2px 8px;
border-radius: 20px;
font-size: 9px;
letter-spacing: 0.08em;
text-transform: uppercase;
margin-bottom: 8px;
}

.type-pill.bed { background: #8b5e2a22; color: #8b5e2a; border: 1px solid #8b5e2a44; }
.type-pill.trellis { background: #c4943a22; color: #c4943a; border: 1px solid #c4943a44; }
.type-pill.border { background: #9a603022; color: #9a6030; border: 1px solid #9a603044; }

.materials-list {
font-size: 9px;
color: var(–muted);
line-height: 2;
}

.materials-list span {
color: var(–text);
font-weight: 500;
}

/* Snap indicator */
#snap-info {
position: fixed;
bottom: 12px;
left: 50%;
transform: translateX(-50%);
background: var(–wood-dark);
color: #f5e8d0;
font-size: 10px;
padding: 5px 14px;
border-radius: 20px;
letter-spacing: 0.08em;
opacity: 0;
transition: opacity 0.3s;
pointer-events: none;
z-index: 999;
}
</style>

</head>
<body>

<header>
  <div>
    <h1>🌱 Tuin Planner</h1>
  </div>
  <span>sleep · vergroot · bewerk</span>
</header>

<div class="toolbar">
  <span class="tool-label">Voeg toe:</span>
  <button class="btn" onclick="addBed('bed')">+ Plantenbak</button>
  <button class="btn" onclick="addBed('trellis')">+ Trellis</button>
  <button class="btn" onclick="addBed('border')">+ Randbak</button>
  <div class="divider"></div>
  <span class="tool-label">Tuin:</span>
  <button class="btn" onclick="changeGardenSize()">Tuingrootte</button>
  <button class="btn" onclick="resetLayout()">Reset</button>
  <div class="divider"></div>
  <button class="btn danger" onclick="deleteSelected()">✕ Verwijder</button>
</div>

<div class="main">
  <div class="canvas-wrap">
    <div id="garden"></div>
  </div>

  <div class="sidebar">
    <div class="sidebar-section">
      <h3>Geselecteerd</h3>
      <div id="selection-panel">
        <p class="no-selection">Klik op een bak om te bewerken.<br><br>Sleep om te verplaatsen.<br>Hoek slepen om te vergroten.</p>
      </div>
    </div>

```
<div class="sidebar-section">
  <h3>Tuininfo</h3>
  <div id="garden-stats"></div>
</div>

<div class="sidebar-section">
  <h3>Materiaal schatting</h3>
  <div id="materials-panel" class="materials-list"></div>
</div>
```

  </div>
</div>

<div id="snap-info">Vastklikken op raster</div>

<script>
// ─── CONFIG ───────────────────────────────────────────────────────────────
const GRID = 10;       // px per grid cell
const SCALE = 10;      // 1 grid cell = 10 cm
let gardenW = 600;     // cm
let gardenH = 500;     // cm

let beds = [];
let selectedId = null;
let nextId = 1;

// ─── INIT ──────────────────────────────────────────────────────────────────
function cmToPx(cm) { return (cm / SCALE) * GRID; }
function pxToCm(px) { return Math.round((px / GRID) * SCALE); }
function snapToCm(px) { return Math.round(px / GRID) * GRID; }

function initGarden() {
  const g = document.getElementById('garden');
  g.style.width  = cmToPx(gardenW) + 'px';
  g.style.height = cmToPx(gardenH) + 'px';
}

// Default layout based on photo
function resetLayout() {
  beds = [];
  nextId = 1;
  document.getElementById('garden').innerHTML = '';

  addBedData({ type:'border', label:'Randbak', x:20, y:10, w:560, h:50 });
  addBedData({ type:'bed',    label:'Bed A',   x:120, y:80,  w:220, h:110 });
  addBedData({ type:'bed',    label:'Bed B',   x:360, y:80,  w:180, h:110 });
  addBedData({ type:'trellis',label:'Trellis 1', x:20, y:80, w:50, h:140 });
  addBedData({ type:'trellis',label:'Trellis 2', x:20, y:230, w:50, h:140 });
  addBedData({ type:'bed',    label:'Bed C',   x:120, y:210, w:220, h:100 });
  addBedData({ type:'bed',    label:'Bed D',   x:360, y:210, w:180, h:100 });

  renderAll();
  updateStats();
}

function addBedData(d) {
  beds.push({
    id: nextId++,
    type: d.type,
    label: d.label,
    x: d.x, y: d.y,
    w: d.w, h: d.h
  });
}

// ─── RENDER ────────────────────────────────────────────────────────────────
function renderAll() {
  const g = document.getElementById('garden');
  // Remove beds that no longer exist
  g.querySelectorAll('.bed').forEach(el => {
    const id = parseInt(el.dataset.id);
    if (!beds.find(b => b.id === id)) el.remove();
  });
  beds.forEach(renderBed);
}

function renderBed(b) {
  let el = document.querySelector(`.bed[data-id="${b.id}"]`);
  if (!el) {
    el = document.createElement('div');
    el.className = `bed type-${b.type}`;
    el.dataset.id = b.id;
    el.innerHTML = `
      <div class="bed-inner">
        <div class="bed-label">${b.label}</div>
        <div class="bed-dims"></div>
        <div class="resize-handle"></div>
      </div>`;
    document.getElementById('garden').appendChild(el);
    attachEvents(el, b);
  }

  el.style.left   = cmToPx(b.x) + 'px';
  el.style.top    = cmToPx(b.y) + 'px';
  el.style.width  = cmToPx(b.w) + 'px';
  el.style.height = cmToPx(b.h) + 'px';
  el.querySelector('.bed-label').textContent = b.label;
  el.querySelector('.bed-dims').textContent  = `${b.w}×${b.h} cm`;
  el.classList.toggle('selected', b.id === selectedId);
}

// ─── EVENTS ────────────────────────────────────────────────────────────────
function attachEvents(el, b) {
  let dragStart = null, resizeStart = null;

  // Click to select
  el.addEventListener('mousedown', e => {
    if (e.target.classList.contains('resize-handle')) return;
    select(b.id);
    dragStart = { mx: e.clientX, my: e.clientY, bx: b.x, by: b.y };
    e.stopPropagation();
    e.preventDefault();
  });

  // Resize handle
  el.querySelector('.resize-handle').addEventListener('mousedown', e => {
    select(b.id);
    resizeStart = { mx: e.clientX, my: e.clientY, bw: b.w, bh: b.h };
    e.stopPropagation();
    e.preventDefault();
  });

  // Touch support
  el.addEventListener('touchstart', e => {
    if (e.target.classList.contains('resize-handle')) return;
    select(b.id);
    const t = e.touches[0];
    dragStart = { mx: t.clientX, my: t.clientY, bx: b.x, by: b.y };
    e.stopPropagation();
  }, { passive: true });

  el.querySelector('.resize-handle').addEventListener('touchstart', e => {
    select(b.id);
    const t = e.touches[0];
    resizeStart = { mx: t.clientX, my: t.clientY, bw: b.w, bh: b.h };
    e.stopPropagation();
  }, { passive: true });

  function onMove(cx, cy) {
    if (dragStart) {
      const dx = cx - dragStart.mx;
      const dy = cy - dragStart.my;
      const rawX = dragStart.bx + pxToCm(dx);
      const rawY = dragStart.by + pxToCm(dy);
      b.x = Math.max(0, Math.min(gardenW - b.w, Math.round(rawX / SCALE) * SCALE));
      b.y = Math.max(0, Math.min(gardenH - b.h, Math.round(rawY / SCALE) * SCALE));
      renderBed(b);
    }
    if (resizeStart) {
      const dx = cx - resizeStart.mx;
      const dy = cy - resizeStart.my;
      b.w = Math.max(SCALE*2, Math.round((resizeStart.bw + pxToCm(dx)) / SCALE) * SCALE);
      b.h = Math.max(SCALE*2, Math.round((resizeStart.bh + pxToCm(dy)) / SCALE) * SCALE);
      b.w = Math.min(b.w, gardenW - b.x);
      b.h = Math.min(b.h, gardenH - b.y);
      renderBed(b);
      updateSelectionPanel();
      updateStats();
    }
  }

  document.addEventListener('mousemove', e => onMove(e.clientX, e.clientY));
  document.addEventListener('touchmove', e => { const t = e.touches[0]; onMove(t.clientX, t.clientY); }, { passive: true });

  function onEnd() {
    if (dragStart || resizeStart) { updateStats(); updateSelectionPanel(); }
    dragStart = null; resizeStart = null;
  }
  document.addEventListener('mouseup', onEnd);
  document.addEventListener('touchend', onEnd);
}

// Deselect when clicking garden background
document.getElementById('garden').addEventListener('mousedown', e => {
  if (e.target.id === 'garden') { select(null); }
});

// ─── SELECTION ─────────────────────────────────────────────────────────────
function select(id) {
  selectedId = id;
  document.querySelectorAll('.bed').forEach(el => {
    el.classList.toggle('selected', parseInt(el.dataset.id) === id);
  });
  updateSelectionPanel();
}

function updateSelectionPanel() {
  const panel = document.getElementById('selection-panel');
  const b = beds.find(b => b.id === selectedId);
  if (!b) {
    panel.innerHTML = '<p class="no-selection">Klik op een bak om te bewerken.<br><br>Sleep om te verplaatsen.<br>Hoek slepen om te vergroten.</p>';
    return;
  }

  const typeLabels = { bed: 'Plantenbak', trellis: 'Trellis', border: 'Randbak' };
  panel.innerHTML = `
    <span class="type-pill ${b.type}">${typeLabels[b.type]}</span>
    <div class="field-row">
      <label>Naam</label>
      <input type="text" id="f-label" value="${b.label}" oninput="updateField('label', this.value)">
    </div>
    <div class="field-row">
      <label>Breedte × Diepte (cm)</label>
      <div class="two-col">
        <input type="number" id="f-w" value="${b.w}" step="10" min="20" oninput="updateField('w', +this.value)">
        <input type="number" id="f-h" value="${b.h}" step="10" min="20" oninput="updateField('h', +this.value)">
      </div>
    </div>
    <div class="field-row">
      <label>Positie X, Y (cm)</label>
      <div class="two-col">
        <input type="number" id="f-x" value="${b.x}" step="10" min="0" oninput="updateField('x', +this.value)">
        <input type="number" id="f-y" value="${b.y}" step="10" min="0" oninput="updateField('y', +this.value)">
      </div>
    </div>
  `;
}

function updateField(key, val) {
  const b = beds.find(b => b.id === selectedId);
  if (!b) return;
  if (key === 'label') { b.label = val; }
  else {
    b[key] = Math.max(0, val);
    if (key === 'w') b.w = Math.min(b.w, gardenW - b.x);
    if (key === 'h') b.h = Math.min(b.h, gardenH - b.y);
  }
  renderBed(b);
  updateStats();
}

// ─── ADD / DELETE ──────────────────────────────────────────────────────────
function addBed(type) {
  const labels = { bed: 'Bed', trellis: 'Trellis', border: 'Randbak' };
  const defaults = { bed: {w:120,h:80}, trellis: {w:50,h:120}, border: {w:300,h:40} };
  const d = defaults[type];
  const b = { id: nextId++, type, label: `${labels[type]} ${nextId-1}`,
               x: 20, y: 20, w: d.w, h: d.h };
  beds.push(b);
  renderBed(b);
  select(b.id);
  updateStats();
}

function deleteSelected() {
  if (!selectedId) return;
  beds = beds.filter(b => b.id !== selectedId);
  document.querySelector(`.bed[data-id="${selectedId}"]`)?.remove();
  select(null);
  updateStats();
}

// ─── GARDEN SIZE ───────────────────────────────────────────────────────────
function changeGardenSize() {
  const w = parseInt(prompt('Tuinbreedte in cm:', gardenW));
  const h = parseInt(prompt('Tuindiepte in cm:', gardenH));
  if (w > 0 && h > 0) {
    gardenW = w; gardenH = h;
    initGarden();
    updateStats();
  }
}

// ─── STATS ─────────────────────────────────────────────────────────────────
function updateStats() {
  const totalArea = gardenW * gardenH;
  const bedArea = beds.filter(b => b.type !== 'trellis').reduce((s,b) => s + b.w*b.h, 0);
  const pct = Math.round(bedArea/totalArea*100);
  const nBeds = beds.filter(b => b.type === 'bed' || b.type === 'border').length;
  const nTrellis = beds.filter(b => b.type === 'trellis').length;

  document.getElementById('garden-stats').innerHTML = `
    <div class="stats-row"><span>Tuingrootte</span><span>${gardenW}×${gardenH} cm</span></div>
    <div class="stats-row"><span>Bakken</span><span>${nBeds}</span></div>
    <div class="stats-row"><span>Trellis</span><span>${nTrellis}</span></div>
    <div class="stats-row"><span>Benut</span><span>${pct}%</span></div>
  `;

  // Estimate lumber
  let totalPerimeter = 0;
  beds.filter(b => b.type !== 'trellis').forEach(b => {
    totalPerimeter += 2 * (b.w + b.h);
  });
  const planks = Math.ceil(totalPerimeter / 300 * 3); // 3 layers high, 300cm planks
  const posts   = beds.filter(b => b.type !== 'trellis').length * 4;
  const tPanels = beds.filter(b => b.type === 'trellis').length * 6;
  const soil    = beds.filter(b => b.type !== 'trellis').reduce((s,b) => s + Math.ceil(b.w*b.h*30/1000000*1.2), 0);

  document.getElementById('materials-panel').innerHTML = `
    <div class="stats-row"><span>2×6" planken (~300cm)</span><span>${planks} stuks</span></div>
    <div class="stats-row"><span>2×4" hoekpalen</span><span>${posts} stuks</span></div>
    <div class="stats-row"><span>Trellis lat strips</span><span>${tPanels} stuks</span></div>
    <div class="stats-row"><span>Tuinaarde (zakken)</span><span>~${soil} zak</span></div>
    <div class="stats-row" style="margin-top:6px; padding-top:6px; border-top:1px solid var(--border)">
      <span>Schatting (ANG)</span><span>~$${(planks*25 + posts*15 + tPanels*10 + soil*8).toLocaleString()}</span>
    </div>
  `;
}

// ─── BOOT ──────────────────────────────────────────────────────────────────
initGarden();
resetLayout();
</script>

</body>
</html>