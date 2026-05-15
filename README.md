<!DOCTYPE html>
<html lang="nl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>De Palletleverancier — Platform</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600;700&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<style>
:root{
  --bg:#F2F0EB;--surface:#FFFFFF;--surface2:#F7F5F0;--border:#E2DFD8;--border2:#EAE8E2;
  --text:#1A1916;--text2:#6B6860;--text3:#A8A49C;--accent:#1A1916;
  --green:#2D7A4F;--green-bg:#EAF4EE;--green-border:#B8DCCA;
  --orange:#B85C00;--orange-bg:#FDF2E6;--orange-border:#F0C98A;
  --red:#A32020;--red-bg:#FBECEC;--red-border:#E8B4B4;
  --blue:#1A4F8A;--blue-bg:#EBF2FB;--blue-border:#A8C8F0;
  --purple:#5B3FA6;--purple-bg:#F0ECFB;--purple-border:#C8B8F0;
  --sidebar-w:220px;--r:10px;
  --shadow:0 1px 3px rgba(0,0,0,.07),0 1px 2px rgba(0,0,0,.04);
  --shadow-md:0 4px 12px rgba(0,0,0,.09),0 2px 4px rgba(0,0,0,.05);
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'DM Sans',sans-serif;font-size:14px;background:var(--bg);color:var(--text);min-height:100vh;-webkit-font-smoothing:antialiased}
.sidebar{position:fixed;top:0;left:0;width:var(--sidebar-w);height:100vh;background:var(--surface);border-right:1px solid var(--border);display:flex;flex-direction:column;z-index:100;overflow-y:auto}
.sb-head{padding:20px 18px 12px}
.sb-brand{font-size:10px;font-weight:600;color:var(--text3);text-transform:uppercase;letter-spacing:.12em;margin-bottom:4px}
.sb-title{font-size:17px;font-weight:700;color:var(--text);line-height:1.2}
.sb-div{height:1px;background:var(--border2);margin:8px 0}
.sb-section{padding:4px 12px;font-size:10px;font-weight:600;color:var(--text3);text-transform:uppercase;letter-spacing:.1em;margin-top:8px}
.sb-item{display:flex;align-items:center;gap:10px;padding:9px 16px;font-size:13.5px;font-weight:500;color:var(--text2);cursor:pointer;border-left:2.5px solid transparent;transition:all .12s;border-radius:0 6px 6px 0;margin:1px 6px 1px 0}
.sb-item:hover{color:var(--text);background:var(--surface2)}
.sb-item.on{color:var(--text);border-left-color:var(--text);background:var(--surface2);font-weight:600}
.sb-bottom{margin-top:auto;padding:12px;border-top:1px solid var(--border2)}
.sb-action{display:flex;align-items:center;gap:8px;padding:8px 10px;font-size:12.5px;color:var(--blue);cursor:pointer;border-radius:7px;transition:background .12s;font-weight:500}
.sb-action:hover{background:var(--blue-bg)}
.main{margin-left:var(--sidebar-w);padding:28px 32px;min-height:100vh}
.page{display:none}.page.on{display:block}
.page-header{margin-bottom:24px}
.page-title{font-size:22px;font-weight:700}
.page-sub{font-size:13px;color:var(--text3);margin-top:3px}
.card{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:20px;margin-bottom:16px;box-shadow:var(--shadow)}
.card-t{font-size:10.5px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:.08em;margin-bottom:14px}
.card-green{background:var(--green-bg);border-color:var(--green-border)}
.card-purple{background:var(--purple-bg);border-color:var(--purple-border)}
.f{display:flex;flex-direction:column;gap:4px}
.f label{font-size:11.5px;font-weight:600;color:var(--text2)}
.f input,.f select,.f textarea{padding:8px 11px;border:1px solid var(--border);border-radius:7px;font-size:13.5px;background:var(--surface);color:var(--text);font-family:'DM Sans',sans-serif;width:100%;transition:border-color .12s}
.f input:focus,.f select:focus,.f textarea:focus{outline:none;border-color:#888}
.f textarea{resize:vertical;min-height:60px}
.f input.big{font-size:22px;font-weight:700;padding:10px 14px;text-align:center}
.g2{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.g3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px}
.g4{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:12px}
.mb{margin-bottom:12px}
.btn{padding:8px 16px;border:1px solid var(--border);border-radius:7px;font-size:13px;font-weight:500;cursor:pointer;background:var(--surface);color:var(--text);font-family:'DM Sans',sans-serif;transition:all .12s;white-space:nowrap}
.btn:hover{background:var(--surface2)}
.btn-primary{background:var(--text);color:#fff;border-color:var(--text)}.btn-primary:hover{opacity:.85}
.btn-green{background:var(--green-bg);color:var(--green);border-color:var(--green-border)}
.btn-red{background:var(--red-bg);color:var(--red);border-color:var(--red-border)}
.btn-blue{background:var(--blue-bg);color:var(--blue);border-color:var(--blue-border)}
.btn-sm{padding:5px 10px;font-size:12px}
.btn-xs{padding:3px 8px;font-size:11px}
.row-btns{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
.kpi-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:16px}
.kpi{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:16px 18px;box-shadow:var(--shadow)}
.kpi-label{font-size:10.5px;font-weight:600;color:var(--text3);text-transform:uppercase;letter-spacing:.07em;margin-bottom:6px}
.kpi-value{font-size:24px;font-weight:700;font-variant-numeric:tabular-nums;line-height:1}
.kpi-sub{font-size:11.5px;color:var(--text3);margin-top:4px}
.banner{display:flex;align-items:center;gap:10px;padding:11px 16px;border-radius:8px;font-size:13.5px;font-weight:600;margin-bottom:16px}
.banner-green{background:var(--green-bg);color:var(--green);border:1px solid var(--green-border)}
.banner-orange{background:var(--orange-bg);color:var(--orange);border:1px solid var(--orange-border)}
.banner-red{background:var(--red-bg);color:var(--red);border:1px solid var(--red-border)}
.banner-gray{background:var(--surface2);color:var(--text2);border:1px solid var(--border)}
.tbl-wrap{overflow-x:auto;border-radius:var(--r);border:1px solid var(--border);background:var(--surface)}
table.tbl{width:100%;border-collapse:collapse;font-size:13px}
table.tbl th{text-align:left;padding:9px 14px;font-size:10.5px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:.07em;border-bottom:1px solid var(--border);background:var(--surface2);white-space:nowrap}
table.tbl td{padding:11px 14px;border-bottom:1px solid var(--border2);vertical-align:middle}
table.tbl tr:last-child td{border-bottom:none}
table.tbl tr:hover td{background:var(--surface2)}
.racts{display:flex;gap:5px;opacity:0;transition:opacity .12s}
table.tbl tr:hover .racts{opacity:1}
.badge{display:inline-block;font-size:11px;font-weight:600;padding:3px 9px;border-radius:20px;white-space:nowrap}
.b-green{background:var(--green-bg);color:var(--green)}
.b-orange{background:var(--orange-bg);color:var(--orange)}
.b-red{background:var(--red-bg);color:var(--red)}
.b-blue{background:var(--blue-bg);color:var(--blue)}
.b-purple{background:var(--purple-bg);color:var(--purple)}
.b-gray{background:var(--surface2);color:var(--text2)}
.day-block{border:1px solid var(--border);border-radius:var(--r);margin-bottom:10px;background:var(--surface);overflow:hidden;box-shadow:var(--shadow)}
.day-head{display:flex;align-items:center;justify-content:space-between;padding:13px 16px;background:var(--surface2);cursor:pointer;user-select:none}
.day-head-l{display:flex;align-items:center;gap:10px;font-size:15px;font-weight:700}
.day-body{padding:16px}
.stop-card{border:1px solid var(--border2);border-radius:8px;padding:14px;margin-bottom:10px;background:#FAFAF8}
.stop-card.kaan{background:var(--blue-bg);border-color:var(--blue-border)}
.route-vis{display:flex;align-items:center;gap:6px;flex-wrap:wrap;font-size:12px;padding:10px 14px;background:var(--surface2);border-radius:8px;border:1px solid var(--border);margin-bottom:12px}
.rnode-d{padding:4px 10px;border-radius:6px;font-size:12px;font-weight:700;background:var(--blue-bg);color:var(--blue)}
.rnode-s{padding:4px 10px;border-radius:6px;font-size:12px;background:var(--surface);border:1px solid var(--border);color:var(--text)}
.rnode-ok{background:var(--green-bg);color:var(--green);font-weight:600;border:1px solid var(--green-border)}
.rarr{color:var(--text3);font-size:14px}
.veeke-box{background:var(--green-bg);border:1px solid var(--green-border);border-radius:var(--r);padding:18px;margin-bottom:16px}
.veeke-title{font-size:13.5px;font-weight:700;color:var(--green);margin-bottom:12px}
.veeke-stats{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:14px}
.vstat{background:rgba(255,255,255,.55);border-radius:8px;padding:10px 14px}
.vstat-l{font-size:11px;color:var(--green);margin-bottom:3px;font-weight:500}
.vstat-v{font-size:22px;font-weight:700;color:#1a4a30}
.wsum-row{display:flex;justify-content:space-between;padding:7px 0;border-bottom:1px solid var(--border2);font-size:13px}
.wsum-row:last-child{border-bottom:none;font-weight:700;font-size:14px;padding-top:10px}
.be-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:12px}
.be-cell{background:var(--surface2);border-radius:8px;padding:14px;text-align:center}
.be-val{font-size:28px;font-weight:700;line-height:1}
.be-lbl{font-size:11px;color:var(--text3);margin-top:4px}
.filter-bar{display:flex;gap:6px;margin-bottom:16px;flex-wrap:wrap;align-items:center}
.fb{padding:6px 14px;border:1px solid var(--border);border-radius:20px;font-size:12.5px;cursor:pointer;background:var(--surface);color:var(--text2);transition:all .12s;font-weight:500}
.fb:hover{background:var(--surface2)}
.fb.on{background:var(--text);color:#fff;border-color:var(--text)}
.p-row{display:grid;grid-template-columns:1.6fr 1fr 1fr 1fr 34px;gap:8px;align-items:end;padding:8px 12px;border:1px solid var(--border2);border-radius:8px;margin-bottom:8px;background:var(--surface2)}
.savebar{display:flex;align-items:center;justify-content:flex-end;gap:10px;margin-top:20px;padding-top:16px;border-top:1px solid var(--border2)}
.ok-msg{font-size:13px;color:var(--green)}
.modal-bg{position:fixed;inset:0;background:rgba(0,0,0,.38);display:flex;align-items:center;justify-content:center;z-index:300;padding:20px}
.modal{background:var(--surface);border-radius:12px;padding:26px;width:520px;max-width:100%;max-height:90vh;overflow-y:auto;box-shadow:var(--shadow-md)}
.modal-title{font-size:17px;font-weight:700;margin-bottom:18px}
.modal-footer{display:flex;justify-content:flex-end;gap:8px;margin-top:18px;padding-top:16px;border-top:1px solid var(--border2)}
.sec-label{font-size:10.5px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:.07em;margin:14px 0 8px}
.dropzone{background:var(--surface);border:2px dashed var(--border);border-radius:var(--r);padding:24px;text-align:center;cursor:pointer;position:relative;transition:border-color .15s,background .15s;margin-bottom:16px}
.dropzone:hover,.dropzone.over{border-color:var(--blue);background:var(--blue-bg)}
.dropzone.busy{border-color:var(--orange);background:var(--orange-bg)}
.dropzone input[type=file]{position:absolute;inset:0;opacity:0;cursor:pointer;width:100%;height:100%}
.dz-icon{font-size:28px;margin-bottom:8px}
.dz-title{font-size:14px;font-weight:600;margin-bottom:3px}
.dz-sub{font-size:12px;color:var(--text3)}
.spinner{width:24px;height:24px;border:2px solid var(--border);border-top-color:var(--orange);border-radius:50%;animation:spin .7s linear infinite;margin:0 auto 8px;display:none}
.dropzone.busy .spinner{display:block}
.dropzone.busy .dz-icon{display:none}
@keyframes spin{to{transform:rotate(360deg)}}
.api-bar{display:flex;align-items:center;gap:10px;padding:10px 14px;background:var(--surface);border:1px solid var(--border);border-radius:8px;margin-bottom:14px;flex-wrap:wrap}
.api-bar label{font-size:12px;color:var(--text2);white-space:nowrap;font-weight:500}
.api-bar input{flex:1;min-width:200px;font-size:12px;font-family:'DM Mono',monospace;padding:6px 9px;border:1px solid var(--border);border-radius:6px;background:var(--surface2);color:var(--text)}
.fact-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);overflow:hidden;box-shadow:var(--shadow)}
.toast{position:fixed;bottom:20px;right:20px;background:var(--text);color:#fff;font-size:13px;padding:10px 18px;border-radius:8px;z-index:999;opacity:0;transform:translateY(8px);transition:all .2s;pointer-events:none;font-weight:500;box-shadow:var(--shadow-md)}
.toast.show{opacity:1;transform:translateY(0)}
.chart-legend{display:flex;gap:16px;margin-top:10px;flex-wrap:wrap}
.chart-legend-item{display:flex;align-items:center;gap:5px;font-size:12px;color:var(--text2)}
.chart-dot{width:10px;height:10px;border-radius:2px;flex-shrink:0}
.scania-box{background:var(--green-bg);border:1px solid var(--green-border);border-radius:var(--r);padding:16px;margin-bottom:16px}
.search-input{padding:8px 12px;border:1px solid var(--border);border-radius:7px;font-size:13.5px;background:var(--surface);color:var(--text);font-family:'DM Sans',sans-serif}
.search-input:focus{outline:none;border-color:#888}
.divider{height:1px;background:var(--border2);margin:12px 0}
.empty{text-align:center;padding:40px;color:var(--text3);font-size:13.5px}
.hint-box{font-size:12px;color:var(--text2);background:var(--surface2);border-radius:7px;padding:9px 12px;border:1px solid var(--border2)}
.dot{width:8px;height:8px;border-radius:50%;display:inline-block;margin-right:5px}
.dot-g{background:var(--green)}.dot-o{background:var(--orange)}.dot-r{background:var(--red)}
.mono{font-family:'DM Mono',monospace;font-size:12px}
.note-text{font-size:11px;color:var(--orange);margin-top:2px}
/* Fiscaal */
.tax-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px;margin-bottom:16px}
.tax-cell{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:16px;text-align:center;box-shadow:var(--shadow)}
.tax-val{font-size:26px;font-weight:700;margin-bottom:4px}
.tax-lbl{font-size:11px;color:var(--text3);text-transform:uppercase;letter-spacing:.06em}
.tax-note{font-size:11px;color:var(--text2);margin-top:4px}
/* Periode tabs */
.period-tabs{display:flex;gap:4px;margin-bottom:16px;flex-wrap:wrap}
.ptab{padding:7px 16px;border:1px solid var(--border);border-radius:20px;font-size:12.5px;cursor:pointer;background:var(--surface);color:var(--text2);font-weight:500;transition:all .12s}
.ptab:hover{background:var(--surface2)}
.ptab.on{background:var(--text);color:#fff;border-color:var(--text)}
/* Periode tabel */
.period-tbl{width:100%;border-collapse:collapse;font-size:13px}
.period-tbl th{text-align:left;padding:9px 12px;font-size:10.5px;font-weight:700;color:var(--text3);text-transform:uppercase;border-bottom:1px solid var(--border);background:var(--surface2)}
.period-tbl td{padding:10px 12px;border-bottom:1px solid var(--border2)}
.period-tbl tr:last-child td{border-bottom:none}
.period-tbl tr:hover td{background:var(--surface2)}
/* Btw */
.btw-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px;margin-bottom:16px}
/* Export */
.export-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px}
.export-btn{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:20px;text-align:center;cursor:pointer;transition:all .15s;box-shadow:var(--shadow)}
.export-btn:hover{background:var(--surface2);border-color:#aaa}
.export-btn .ico{font-size:28px;margin-bottom:8px}
.export-btn .lbl{font-size:14px;font-weight:600;margin-bottom:3px}
.export-btn .sub{font-size:12px;color:var(--text3)}
@media(max-width:720px){
  .sidebar{display:none}.main{margin-left:0;padding:16px}
  .g2,.g3,.g4,.kpi-grid,.be-grid,.veeke-stats,.tax-grid,.btw-grid,.export-grid,.period-tabs{grid-template-columns:1fr}
  .p-row{grid-template-columns:1fr 1fr;gap:6px}
}
</style>
</head>
<body>

<nav class="sidebar">
  <div class="sb-head">
    <div class="sb-brand">De Palletleverancier</div>
    <div class="sb-title">Bedrijfsplatform</div>
  </div>
  <div class="sb-div"></div>
  <div class="sb-section">Operationeel</div>
  <div class="sb-item on" onclick="nav('inst')">⚙️ &nbsp;Instellingen</div>
  <div class="sb-item" onclick="nav('kl')">👥 &nbsp;Klanten</div>
  <div class="sb-item" onclick="nav('week')">🚛 &nbsp;Weekinvoer</div>
  <div class="sb-div"></div>
  <div class="sb-section">Financieel</div>
  <div class="sb-item" onclick="nav('dash')">📊 &nbsp;Dashboard</div>
  <div class="sb-item" onclick="nav('periode')">📅 &nbsp;Maand / Kwartaal</div>
  <div class="sb-item" onclick="nav('fiscaal')">💼 &nbsp;Winst &amp; Belasting</div>
  <div class="sb-item" onclick="nav('btw')">🧮 &nbsp;Btw-overzicht</div>
  <div class="sb-item" onclick="nav('fact')">🧾 &nbsp;Facturen</div>
  <div class="sb-div"></div>
  <div class="sb-bottom">
    <div class="sb-action" onclick="nav('export')">💾 &nbsp;Back-up &amp; Export</div>
  </div>
</nav>

<div class="main">

<!-- ══ INSTELLINGEN ══ -->
<div id="pg-inst" class="page on">
<div class="page-header"><div class="page-title">Instellingen</div><div class="page-sub">Eenmalig invullen — vormt de basis van alle berekeningen</div></div>

<div class="card"><div class="card-t">Bedrijf &amp; locatie</div>
<div class="g2 mb">
  <div class="f"><label>Startlocatie (depot)</label><input type="text" id="s_startloc" placeholder="Havenkant 2, 4781AA Moerdijk"></div>
  <div class="f"><label>Winstdoel per maand (EUR excl.)</label><input type="number" id="s_winstdoel" placeholder="4000"></div>
</div>
<div class="g2">
  <div class="f"><label>Dieselprijs (EUR/liter)</label><input type="number" id="s_diesel" step="0.01" placeholder="1.75"></div>
  <div class="f"><label>Wegenbelasting (EUR/km)</label><input type="number" id="s_wegbel" step="0.01" placeholder="0.20"></div>
</div></div>

<div class="card"><div class="card-t">Huurvrachtwagen</div>
<div class="g4">
  <div class="f"><label>Dagtarief (EUR excl.)</label><input type="number" id="s_huur_dag" placeholder="230" oninput="calcVaste()"></div>
  <div class="f"><label>Verbruik (l/100km)</label><input type="number" id="s_huur_vbr" placeholder="28" step="0.1"></div>
  <div class="f"><label>Rijdagen per week</label>
    <select id="s_huur_dgn" oninput="calcVaste();updateDagBlocks()">
      <option value="1">1 dag</option>
      <option value="2" selected>2 dagen</option>
      <option value="3">3 dagen</option>
      <option value="4">4 dagen</option>
      <option value="5">5 dagen</option>
    </select>
  </div>
  <div></div>
</div></div>

<div class="card"><div class="card-t">Scania (toekomstig)</div>
<div class="g4 mb">
  <div class="f"><label>Activatiedatum</label><input type="date" id="s_sc_datum" oninput="calcVaste()"></div>
  <div class="f"><label>Maandlease (EUR excl.)</label><input type="number" id="s_sc_lease" placeholder="1850" oninput="calcVaste()"></div>
  <div class="f"><label>Onderhoud (EUR excl./mnd)</label><input type="number" id="s_sc_ondh" placeholder="200" oninput="calcVaste()"></div>
  <div class="f"><label>Verbruik (l/100km)</label><input type="number" id="s_sc_vbr" placeholder="28" step="0.1"></div>
</div>
<div class="hint-box" id="sc-prev">Vul activatiedatum in voor vooruitblik.</div></div>

<div class="card"><div class="card-t">Vaste lasten per maand</div>
<div class="g4 mb">
  <div class="f"><label>Trailer lease (EUR incl.)</label><input type="number" id="s_trailer" placeholder="500" oninput="calcVaste()"></div>
  <div class="f"><label>Verzekeringen (EUR/mnd)</label><input type="number" id="s_verz" placeholder="500" oninput="calcVaste()"></div>
  <div class="f"><label>Administratie (EUR incl.)</label><input type="number" id="s_admin" placeholder="600" oninput="calcVaste()"></div>
  <div class="f"><label>Overig (EUR/mnd)</label><input type="number" id="s_overig" placeholder="0" oninput="calcVaste()"></div>
</div>
<div style="font-size:13px;color:var(--text2)">Vaste lasten/week (excl. transport): <strong id="vaste-w">—</strong></div></div>

<div class="card"><div class="card-t">Commissie Kaan</div>
<div style="max-width:220px"><div class="f"><label>Per vracht (EUR excl.)</label><input type="number" id="s_comm" placeholder="100"></div></div></div>

<div class="card"><div class="card-t">Pallettypen</div>
<div style="display:grid;grid-template-columns:1.6fr 1fr 1fr 1fr 34px;gap:8px;padding:0 12px 8px;font-size:10.5px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:.06em">
  <span>Naam</span><span>Inkoop</span><span>Reparatie</span><span>Verkoop std.</span><span></span>
</div>
<div id="prows"></div>
<button class="btn btn-sm" onclick="addP()" style="margin-top:8px">+ Pallettype</button></div>

<div class="card"><div class="card-t">OpenRouteService API</div>
<div class="f"><label>ORS API Key (voor routeberekening)</label><input type="text" id="w_ors" placeholder="Plak hier je ORS API key..." oninput="localStorage.setItem('pl_ors',this.value)"></div>
<div class="hint-box" style="margin-top:8px">Gratis via openrouteservice.org → Dashboard → API key</div></div>

<div class="savebar">
  <span class="ok-msg" id="s-ok" style="display:none">✓ Opgeslagen</span>
  <button class="btn" onclick="loadDef()">Standaardwaarden</button>
  <button class="btn btn-primary" onclick="saveS()">Instellingen opslaan</button>
</div>
</div>

<!-- ══ KLANTEN ══ -->
<div id="pg-kl" class="page">
<div class="page-header"><div class="page-title">Klantenbeheer</div><div class="page-sub">Adresgegevens worden gebruikt voor routeberekening</div></div>
<div class="row-btns mb">
  <input type="text" class="search-input" id="kl-s" placeholder="Zoek naam, bedrijf of plaats..." oninput="renderK()" style="max-width:340px">
  <span style="font-size:12px;color:var(--text3)" id="kl-cnt"></span>
  <button class="btn btn-primary" onclick="openKM()" style="margin-left:auto">+ Klant toevoegen</button>
</div>
<div class="tbl-wrap">
<table class="tbl"><thead><tr><th style="width:36px"></th><th>Bedrijfsnaam</th><th>Contactpersoon</th><th>Adres</th><th>Std. pallet</th><th>Notities</th><th style="width:110px"></th></tr></thead>
<tbody id="kl-tb"></tbody></table>
<div class="empty" id="kl-empty" style="display:none">Nog geen klanten — klik op <strong>+ Klant toevoegen</strong></div>
</div></div>

<!-- ══ WEEKINVOER ══ -->
<div id="pg-week" class="page">
<div class="page-header"><div class="page-title">Weekinvoer</div><div class="page-sub">Route: Depot → stops → Depot</div></div>
<div class="card">
<div class="g3 mb">
  <div class="f"><label>Weeknummer</label><input type="number" id="w_wk" min="1" max="53"></div>
  <div class="f"><label>Jaar</label><input type="number" id="w_jr"></div>
  <div class="f"><label>Dieselprijs (EUR/liter)</label><input type="number" id="w_diesel" step="0.01" placeholder="1.75" oninput="herb()"></div>
</div>
<div id="w-huur-info" style="display:flex;align-items:center;gap:12px;padding:12px 16px;background:var(--orange-bg);border:1px solid var(--orange-border);border-radius:8px;flex-wrap:wrap">
  <span style="font-size:20px">🚛</span>
  <div style="flex:1">
    <div style="font-size:13px;font-weight:700;color:var(--orange)" id="w-huur-label">Laden...</div>
    <div style="font-size:12px;color:var(--text2)" id="w-huur-detail"></div>
  </div>
  <div style="text-align:right">
    <div style="font-size:10.5px;color:var(--text3);text-transform:uppercase;letter-spacing:.06em;margin-bottom:2px">Huurkosten deze week</div>
    <div style="font-size:22px;font-weight:700;color:var(--orange)" id="w-huur-kosten">—</div>
  </div>
</div>
</div>
<div class="veeke-box">
  <div class="veeke-title">🔧 Veeke Palletreparatie — Voorraad</div>
  <div class="veeke-stats">
    <div class="vstat"><div class="vstat-l">Huidig bij Veeke</div><div class="vstat-v" id="v-huidig">0</div></div>
    <div class="vstat"><div class="vstat-l">Gebracht (+)</div><div class="vstat-v" id="v-bracht" style="color:var(--green)">+0</div></div>
    <div class="vstat"><div class="vstat-l">Opgehaald (−)</div><div class="vstat-v" id="v-opgh" style="color:var(--red)">−0</div></div>
  </div>
  <div class="g3 mb">
    <div class="f"><label style="color:var(--green)">Beginstand</label><input type="number" id="v_begin" min="0" placeholder="0" oninput="updV()" style="background:rgba(255,255,255,.6)"></div>
    <div class="f"><label style="color:var(--green)">Gebracht deze week</label><input type="number" id="v_bracht_in" min="0" placeholder="0" oninput="updV()" style="background:rgba(255,255,255,.6)"></div>
    <div class="f"><label style="color:var(--green)">Opgehaald deze week</label><input type="number" id="v_opgh_in" min="0" placeholder="0" oninput="updV()" style="background:rgba(255,255,255,.6)"></div>
  </div>
  <div style="font-size:12px;color:var(--green)" id="v-log"></div>
</div>
<div id="dag-blocks"></div>
<div style="display:flex;gap:8px;margin-bottom:10px">
  <button class="btn btn-sm btn-green" onclick="addDag()">+ Dag toevoegen</button>
  <button class="btn btn-sm btn-red" id="btn-rem-dag" onclick="remDag()" style="display:none">− Dag verwijderen</button>
</div>
<div class="card"><div class="card-t">Weekoverzicht</div>
<div class="kpi-grid">
  <div class="kpi"><div class="kpi-label">Omzet</div><div class="kpi-value" id="w-omz">€0</div></div>
  <div class="kpi"><div class="kpi-label">Inkoop</div><div class="kpi-value" id="w-ink">€0</div></div>
  <div class="kpi"><div class="kpi-label">Alle kosten</div><div class="kpi-value" id="w-kos">€0</div></div>
  <div class="kpi"><div class="kpi-label">Nettowinst</div><div class="kpi-value" id="w-win">€0</div></div>
</div>
<div class="banner banner-gray" id="w-sl">Voer leveringen in om resultaat te zien</div>
<div class="divider"></div>
<div class="card-t">Kostenverdeling</div>
<div id="w-kv"></div></div>
<div class="row-btns" style="justify-content:flex-end">
  <button class="btn btn-red btn-sm" onclick="resetWeek()">↩ Reset week</button>
  <button class="btn btn-sm" onclick="nwWeek()">Nieuwe week</button>
  <button class="btn btn-primary" onclick="slaOp()">💾 Week opslaan</button>
</div>
<div id="w-ok" style="font-size:13px;color:var(--green);text-align:right;margin-top:6px;display:none">✓ Week opgeslagen</div>
<div id="w-reset-ok" style="font-size:13px;color:var(--red);text-align:right;margin-top:6px;display:none">↩ Week gereset</div>
</div>

<!-- ══ DASHBOARD ══ -->
<div id="pg-dash" class="page">
<div class="page-header"><div class="page-title">Dashboard</div><div class="page-sub">Winst, kosten en trends in één oogopslag</div></div>
<div class="filter-bar">
  <button class="fb on" onclick="setFil('2w',this)">2 weken</button>
  <button class="fb" onclick="setFil('4w',this)">4 weken</button>
  <button class="fb" onclick="setFil('8w',this)">8 weken</button>
  <button class="fb" onclick="setFil('12w',this)">12 weken</button>
  <button class="fb" onclick="setFil('jaar',this)">Dit jaar</button>
</div>
<div class="kpi-grid">
  <div class="kpi"><div class="kpi-label">Totale omzet</div><div class="kpi-value" id="d-omz">—</div><div class="kpi-sub" id="d-omz-s"></div></div>
  <div class="kpi"><div class="kpi-label">Nettowinst</div><div class="kpi-value" id="d-win">—</div><div class="kpi-sub" id="d-win-s"></div></div>
  <div class="kpi"><div class="kpi-label">Gem. marge</div><div class="kpi-value" id="d-marg">—</div><div class="kpi-sub" id="d-marg-s"></div></div>
  <div class="kpi"><div class="kpi-label">Totaal km</div><div class="kpi-value" id="d-km">—</div><div class="kpi-sub" id="d-km-s"></div></div>
</div>
<div class="banner banner-gray" id="d-sl">Nog geen weekdata — sla eerst een week op via Weekinvoer</div>
<div class="card"><div class="card-t">Break-even &amp; doelstelling</div>
<div class="be-grid">
  <div class="be-cell"><div class="be-val" id="be-be" style="color:var(--red)">—</div><div class="be-lbl">Pallets break-even/week</div></div>
  <div class="be-cell"><div class="be-val" id="be-doel" style="color:var(--green)">—</div><div class="be-lbl">Pallets winstdoel/week</div></div>
  <div class="be-cell"><div class="be-val" id="be-ver">—</div><div class="be-lbl">Gem. boven/onder doel</div></div>
</div>
<div style="font-size:12px;color:var(--text3)" id="be-hint"></div></div>
<div class="card"><div class="card-t">Omzet vs kosten per week</div><div id="ch-bar"></div>
<div class="chart-legend">
  <div class="chart-legend-item"><div class="chart-dot" style="background:#378ADD"></div>Omzet</div>
  <div class="chart-legend-item"><div class="chart-dot" style="background:#F09595"></div>Kosten</div>
  <div class="chart-legend-item"><div class="chart-dot" style="background:#1D9E75"></div>Nettowinst</div>
</div></div>
<div class="card"><div class="card-t">Nettomarge % per week</div><div id="ch-marg"></div></div>
<div class="g2" style="margin-bottom:16px">
  <div class="card" style="margin-bottom:0"><div class="card-t">Kostenverdeling</div><div id="ch-don"></div></div>
  <div class="card" style="margin-bottom:0"><div class="card-t">Nettowinst per maand</div><div id="ch-mnd"></div></div>
</div>
<div class="scania-box" id="d-sc" style="display:none">
  <div style="font-size:13.5px;font-weight:700;color:var(--green);margin-bottom:8px">🚛 Scania overgangsmodule</div>
  <div id="d-sc-t" style="font-size:13px;color:var(--green);line-height:1.9"></div>
</div>
<div class="card"><div class="card-t">Weken detail</div><div id="d-tbl"></div></div>
</div>

<!-- ══ PERIODE ══ -->
<div id="pg-periode" class="page">
<div class="page-header"><div class="page-title">Maand &amp; Kwartaal</div><div class="page-sub">Omzet en winst per maand en kwartaal — automatisch gesegmenteerd op basis van weeknummer</div></div>
<div class="period-tabs">
  <button class="ptab on" onclick="setPeriod('maand',this)">Per maand</button>
  <button class="ptab" onclick="setPeriod('kwartaal',this)">Per kwartaal</button>
</div>
<div id="periode-content"></div>
</div>

<!-- ══ FISCAAL ══ -->
<div id="pg-fiscaal" class="page">
<div class="page-header"><div class="page-title">Winst &amp; Belasting</div><div class="page-sub">Schatting op basis van ingevoerde weekdata — vóór holding/DGA-structuur. Altijd controleren met accountant.</div></div>
<div class="filter-bar">
  <button class="fb on" onclick="setFiscFil('ytd',this)">Dit jaar (YTD)</button>
  <button class="fb" onclick="setFiscFil('12w',this)">Laatste 12 weken</button>
</div>
<div id="fisc-content"></div>
</div>

<!-- ══ BTW ══ -->
<div id="pg-btw" class="page">
<div class="page-header"><div class="page-title">Btw-overzicht</div><div class="page-sub">Schatting verschuldigde omzetbelasting op basis van weekdata. 21% btw-tarief.</div></div>
<div class="filter-bar">
  <button class="fb on" onclick="setBtwFil('kwartaal',this)">Huidig kwartaal</button>
  <button class="fb" onclick="setBtwFil('ytd',this)">Dit jaar</button>
  <button class="fb" onclick="setBtwFil('all',this)">Alles</button>
</div>
<div id="btw-content"></div>
</div>

<!-- ══ FACTUREN ══ -->
<div id="pg-fact" class="page">
<div class="page-header"><div class="page-title">Facturen</div><div class="page-sub">Sleep een PDF — bedrag en vervaldatum worden automatisch uitgelezen via AI</div></div>
<div class="kpi-grid">
  <div class="kpi"><div class="kpi-label">Openstaand</div><div class="kpi-value" id="fm1">—</div></div>
  <div class="kpi"><div class="kpi-label">Vervallen</div><div class="kpi-value" id="fm2" style="color:var(--red)">—</div></div>
  <div class="kpi"><div class="kpi-label">Vervalt deze week</div><div class="kpi-value" id="fm3" style="color:var(--orange)">—</div></div>
  <div class="kpi"><div class="kpi-label">Betaald (30d)</div><div class="kpi-value" id="fm4" style="color:var(--green)">—</div></div>
</div>
<div class="api-bar">
  <label>🔑 Anthropic API key:</label>
  <input type="password" id="f_apikey" placeholder="sk-ant-api03-..." oninput="onFKey(this.value)">
  <span style="font-size:11px;color:var(--green)" id="f_keystatus"></span>
</div>
<div class="dropzone" id="fdz">
  <input type="file" accept=".pdf" multiple onchange="handleFiles(this.files)" id="ffileinput">
  <div class="spinner"></div>
  <div class="dz-icon">📄</div>
  <div class="dz-title" id="fdztitle">Sleep factuur-PDF's hierheen</div>
  <div class="dz-sub">of klik om te bladeren — bedrag en vervaldatum worden automatisch uitgelezen</div>
</div>
<div class="filter-bar">
  <button class="fb on" onclick="setFFilter('open',this)">Openstaand</button>
  <button class="fb" onclick="setFFilter('week',this)">Deze week</button>
  <button class="fb" onclick="setFFilter('vervallen',this)">Vervallen</button>
  <button class="fb" onclick="setFFilter('betaald',this)">Betaald</button>
  <button class="fb" onclick="setFFilter('all',this)">Alles</button>
  <button class="btn btn-sm" onclick="openFactModal()" style="margin-left:auto">+ Handmatig toevoegen</button>
</div>
<div class="fact-card">
<table class="tbl"><thead><tr><th>Leverancier</th><th>Factuurnr.</th><th>Bedrag excl.</th><th>Vervaldatum</th><th>Status</th><th></th></tr></thead>
<tbody id="ftbody"></tbody></table>
<div class="empty" id="fempty" style="display:none">Geen facturen in deze weergave</div>
</div></div>

<!-- ══ EXPORT / BACKUP ══ -->
<div id="pg-export" class="page">
<div class="page-header"><div class="page-title">Back-up &amp; Export</div><div class="page-sub">Exporteer je data voor back-up of analyse. JSON is volledig herstelbaar — exporteer regelmatig.</div></div>
<div class="card"><div class="card-t">Back-up (volledig herstelbaar)</div>
<div class="export-grid" style="margin-bottom:16px">
  <div class="export-btn" onclick="exportJSON()">
    <div class="ico">📦</div><div class="lbl">JSON back-up</div><div class="sub">Volledig herstelbaar — aanbevolen</div>
  </div>
  <div class="export-btn" onclick="document.getElementById('import-file').click()">
    <div class="ico">📥</div><div class="lbl">JSON importeren</div><div class="sub">Herstel vanuit back-up</div>
  </div>
  <div style="display:flex;align-items:center;justify-content:center;color:var(--text3);font-size:13px;border:1px dashed var(--border);border-radius:var(--r);padding:20px;text-align:center">
    Exporteer minstens 1x per week voor zekerheid
  </div>
</div>
<input type="file" id="import-file" accept=".json" style="display:none" onchange="importData(this)">
</div>
<div class="card"><div class="card-t">Weekdata exporteren</div>
<div class="export-grid">
  <div class="export-btn" onclick="exportCSV()">
    <div class="ico">📊</div><div class="lbl">CSV exporteren</div><div class="sub">Open in Excel of Google Sheets</div>
  </div>
  <div class="export-btn" onclick="exportExcel()">
    <div class="ico">📗</div><div class="lbl">Excel exporteren</div><div class="sub">.xlsx bestand</div>
  </div>
  <div class="export-btn" onclick="exportPeriodeCSV()">
    <div class="ico">📅</div><div class="lbl">Maand/kwartaal CSV</div><div class="sub">Gegroepeerd per periode</div>
  </div>
</div></div>
<div class="card"><div class="card-t">Facturen exporteren</div>
<div class="export-grid">
  <div class="export-btn" onclick="exportFactCSV()">
    <div class="ico">🧾</div><div class="lbl">Facturen CSV</div><div class="sub">Alle facturen als spreadsheet</div>
  </div>
</div></div>
</div>

</div><!-- /main -->

<!-- ══ KLANT MODAL ══ -->
<div class="modal-bg" id="km-modal" style="display:none" onclick="if(event.target===this)closeKM()">
<div class="modal">
  <div class="modal-title" id="km-ttl">Klant toevoegen</div>
  <div class="sec-label">Bedrijfsgegevens</div>
  <div class="g2 mb"><div class="f"><label>Bedrijfsnaam *</label><input type="text" id="kf_bedr"></div><div class="f"><label>Contactpersoon</label><input type="text" id="kf_cont"></div></div>
  <div class="g2 mb"><div class="f"><label>Telefoon</label><input type="tel" id="kf_tel"></div><div class="f"><label>E-mail</label><input type="email" id="kf_email"></div></div>
  <div class="sec-label">Afleveradres</div>
  <div class="f mb"><label>Straat + huisnummer *</label><input type="text" id="kf_str"></div>
  <div style="display:grid;grid-template-columns:2fr 1fr 1fr;gap:10px" class="mb">
    <div class="f"><label>Plaats *</label><input type="text" id="kf_pl"></div>
    <div class="f"><label>Postcode</label><input type="text" id="kf_pc"></div>
    <div class="f"><label>Land</label><select id="kf_land"><option value="NL">Nederland</option><option value="BE">België</option><option value="DE">Duitsland</option></select></div>
  </div>
  <div class="sec-label">Voorkeur</div>
  <div class="g2 mb"><div class="f"><label>Std. pallettype</label><select id="kf_pal"></select></div><div class="f"><label>Std. aantal</label><input type="number" id="kf_ant" min="0"></div></div>
  <div class="f mb"><label>Notities</label><textarea id="kf_not" placeholder="Tijdvenster, bijzonderheden..."></textarea></div>
  <div id="kf_err" style="font-size:13px;color:var(--red);display:none;margin-top:6px"></div>
  <div class="modal-footer"><button class="btn" onclick="closeKM()">Annuleren</button><button class="btn btn-primary" onclick="saveK()">Opslaan</button></div>
</div></div>

<!-- ══ FACTUUR MODAL ══ -->
<div class="modal-bg" id="fact-modal" style="display:none" onclick="if(event.target===this)closeFactModal()">
<div class="modal">
  <div class="modal-title" id="fact-modal-title">Factuur toevoegen</div>
  <input type="hidden" id="feid">
  <div class="f mb"><label>Leverancier</label><input type="text" id="fflev" placeholder="bv. Veeke Palletreparatie"></div>
  <div class="g2 mb">
    <div class="f"><label>Factuurnummer</label><input type="text" id="ffnr" placeholder="F01099"></div>
    <div class="f"><label>Bedrag excl. btw (€)</label><input type="number" id="ffbed" placeholder="0.00" step="0.01" min="0"></div>
  </div>
  <div class="g2 mb">
    <div class="f"><label>Factuurdatum</label><input type="date" id="ffdat"></div>
    <div class="f"><label>Vervaldatum</label><input type="date" id="ffver"></div>
  </div>
  <div class="f mb"><label>Notitie</label><input type="text" id="ffnot" placeholder="optioneel"></div>
  <div class="modal-footer"><button class="btn" onclick="closeFactModal()">Annuleren</button><button class="btn btn-primary" onclick="saveFactuur()">Opslaan</button></div>
</div></div>

<div class="toast" id="toast"></div>

<script>
pdfjsLib.GlobalWorkerOptions.workerSrc='https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';

// ═══════════════════════════════════════════════
// STATE
// ═══════════════════════════════════════════════
var days={}, kmD={}, dOpen={};
var pallets=[], editKId=null, dFil='2w';
var fRows=[], fFilter='open', fBusy=false;
var periodeMode='maand', fiscFil='ytd', btwFil='kwartaal';

var DEF={
  startloc:'Havenkant 2, 4781AA Moerdijk',winstdoel:4000,diesel:1.75,wegbel:0.20,
  huur_dag:230,huur_vbr:28,huur_dgn:2,sc_datum:'',sc_lease:1850,sc_ondh:200,sc_vbr:28,
  trailer:500,verz:500,admin:600,overig:0,comm:100,
  pallets:[
    {naam:'EPAL (De Zwaluw)',inkoop:2.50,reparatie:3.25,rep_aan:true,verkoop:7.50},
    {naam:'Blokpallet (Kompak)',inkoop:0.60,reparatie:0,rep_aan:false,verkoop:3.75},
    {naam:'80/120 (Kompak)',inkoop:0.60,reparatie:0,rep_aan:false,verkoop:3.25},
    {naam:'CP3 (Ter Slaa)',inkoop:8.00,reparatie:0,rep_aan:false,verkoop:10.50},
    {naam:'102/120 (Modon)',inkoop:5.00,reparatie:0,rep_aan:false,verkoop:7.50}
  ]
};
var FACT_INIT=[
  {id:1,lev:'Veeke Palletreparatie',nr:'F01042',bed:null,dat:'2026-03-27',ver:null,st:'open',not:''},
  {id:2,lev:'Veeke Palletreparatie',nr:'F01076',bed:null,dat:'2026-03-27',ver:null,st:'open',not:''},
  {id:3,lev:'Veeke Palletreparatie',nr:'F01099',bed:null,dat:'2026-03-27',ver:null,st:'open',not:''},
  {id:4,lev:'Veeke Palletreparatie',nr:'F01102',bed:null,dat:'2026-03-27',ver:null,st:'open',not:''},
  {id:5,lev:'Ter Slaa Pallets',nr:'26700422',bed:null,dat:'2026-03-27',ver:null,st:'open',not:'⚠️ Nieuw rekeningnummer per 25 mrt'},
  {id:6,lev:'Kaan / ARKA Digital',nr:'PAL-2026-001',bed:null,dat:'2026-04-01',ver:null,st:'open',not:''},
  {id:7,lev:'Beequip (trailer lease)',nr:'VF699133',bed:null,dat:'2026-03-30',ver:null,st:'incasso',not:'Automatische incasso'}
];

// ═══════════════════════════════════════════════
// HELPERS
// ═══════════════════════════════════════════════
function GS(){try{return JSON.parse(localStorage.getItem('pl_s')||'{}')}catch(e){return{}}}
function GC(){try{return JSON.parse(localStorage.getItem('pl_c')||'[]')}catch(e){return[]}}
function SC(a){localStorage.setItem('pl_c',JSON.stringify(a))}
function gv(id){var el=document.getElementById(id);return el?el.value:''}
function sv(id,v){var el=document.getElementById(id);if(el)el.value=v}
function fe(v){return'€\u202f'+Math.round(Math.abs(v)).toLocaleString('nl-NL')}
function feDec(v){return'€\u202f'+Number(v).toLocaleString('nl-NL',{minimumFractionDigits:2,maximumFractionDigits:2})}
function getPals(){var s=GS();return(s.pallets&&s.pallets.length)?s.pallets:DEF.pallets}
function getDepot(){return GS().startloc||'Havenkant 2, 4781AA Moerdijk'}
function esc(s){return(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;')}
function wkNum(d){var dt=new Date(Date.UTC(d.getFullYear(),d.getMonth(),d.getDate()));dt.setUTCDate(dt.getUTCDate()+4-(dt.getUTCDay()||7));return Math.ceil((((dt-new Date(Date.UTC(dt.getUTCFullYear(),0,1)))/86400000)+1)/7)}
function showToast(msg){var t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');setTimeout(function(){t.classList.remove('show')},3000)}
function numDagen(){return parseInt(GS().huur_dgn)||2}

// ═══════════════════════════════════════════════
// NAVIGATION
// ═══════════════════════════════════════════════
var PAGES=['inst','kl','week','dash','periode','fiscaal','btw','fact','export'];
function nav(id){
  PAGES.forEach(function(p){var el=document.getElementById('pg-'+p);if(el)el.classList.remove('on')});
  var pg=document.getElementById('pg-'+id);if(pg)pg.classList.add('on');
  document.querySelectorAll('.sb-item').forEach(function(el){el.classList.remove('on')});
  var map={'inst':0,'kl':1,'week':2,'dash':3,'periode':4,'fiscaal':5,'btw':6,'fact':7};
  var idx=map[id];
  if(idx!==undefined)document.querySelectorAll('.sb-item')[idx].classList.add('on');
  if(id==='dash')renderDash();
  if(id==='periode')renderPeriode();
  if(id==='fiscaal')renderFiscaal();
  if(id==='btw')renderBtw();
  if(id==='fact')renderFact();
  if(id==='kl')renderK();
}

// ═══════════════════════════════════════════════
// INSTELLINGEN
// ═══════════════════════════════════════════════
function loadDef(){
  var d=DEF;
  sv('s_startloc',d.startloc);sv('s_winstdoel',d.winstdoel);sv('s_diesel',d.diesel);
  sv('s_wegbel',d.wegbel);sv('s_huur_dag',d.huur_dag);sv('s_huur_vbr',d.huur_vbr);
  sv('s_huur_dgn',d.huur_dgn);sv('s_sc_lease',d.sc_lease);sv('s_sc_ondh',d.sc_ondh);
  sv('s_sc_vbr',d.sc_vbr);sv('s_trailer',d.trailer);sv('s_verz',d.verz);
  sv('s_admin',d.admin);sv('s_overig',d.overig);sv('s_comm',d.comm);
  pallets=d.pallets.map(function(p){return Object.assign({},p)});
  renderP();calcVaste();
}
function loadS(){
  var s=GS();
  ['startloc','winstdoel','diesel','wegbel','huur_dag','huur_vbr','huur_dgn','sc_datum','sc_lease','sc_ondh','sc_vbr','trailer','verz','admin','overig','comm'].forEach(function(k){if(s[k]!==undefined&&s[k]!=='')sv('s_'+k,s[k])});
  pallets=((s.pallets&&s.pallets.length)?s.pallets:DEF.pallets).map(function(p){return Object.assign({},p)});
  renderP();calcVaste();
  var orsEl=document.getElementById('w_ors');if(orsEl)orsEl.value=localStorage.getItem('pl_ors')||'';
}
function saveS(){
  var s={
    startloc:gv('s_startloc'),winstdoel:parseFloat(gv('s_winstdoel'))||4000,
    diesel:parseFloat(gv('s_diesel'))||1.75,wegbel:parseFloat(gv('s_wegbel'))||0.20,
    huur_dag:parseFloat(gv('s_huur_dag'))||230,huur_vbr:parseFloat(gv('s_huur_vbr'))||28,
    huur_dgn:parseInt(gv('s_huur_dgn'))||2,sc_datum:gv('s_sc_datum'),
    sc_lease:parseFloat(gv('s_sc_lease'))||1850,sc_ondh:parseFloat(gv('s_sc_ondh'))||200,
    sc_vbr:parseFloat(gv('s_sc_vbr'))||28,trailer:parseFloat(gv('s_trailer'))||0,
    verz:parseFloat(gv('s_verz'))||0,admin:parseFloat(gv('s_admin'))||0,
    overig:parseFloat(gv('s_overig'))||0,comm:parseFloat(gv('s_comm'))||100,pallets:pallets
  };
  localStorage.setItem('pl_s',JSON.stringify(s));
  var ok=document.getElementById('s-ok');ok.style.display='block';setTimeout(function(){ok.style.display='none'},3000);
  calcVaste();updateDagBlocks();
}
function calcVaste(){
  var s=GS();
  var isS=gv('s_sc_datum')&&new Date(gv('s_sc_datum'))<=new Date();
  var tr=parseFloat(gv('s_trailer'))||parseFloat(s.trailer)||0;
  var ve=parseFloat(gv('s_verz'))||parseFloat(s.verz)||0;
  var ad=parseFloat(gv('s_admin'))||parseFloat(s.admin)||0;
  var ov=parseFloat(gv('s_overig'))||parseFloat(s.overig)||0;
  var sl=parseFloat(gv('s_sc_lease'))||parseFloat(s.sc_lease)||0;
  var so=parseFloat(gv('s_sc_ondh'))||parseFloat(s.sc_ondh)||0;
  var vMnd=tr+ve+ad+ov+(isS?sl+so:0);
  var el=document.getElementById('vaste-w');
  if(el)el.textContent='€'+(vMnd/4.33).toFixed(2)+'/week (€'+Math.round(vMnd).toLocaleString('nl-NL')+'/mnd)';
  var sd=gv('s_sc_datum'),prev=document.getElementById('sc-prev');
  if(prev){
    if(!sd){prev.textContent='Vul activatiedatum in voor vooruitblik.';return}
    var d=new Date(sd),nu=new Date();
    var hd=parseFloat(gv('s_huur_dag'))||parseFloat(s.huur_dag)||230;
    var hn=parseInt(gv('s_huur_dgn'))||parseInt(s.huur_dgn)||2;
    var hm=hd*hn*4.33,sm=sl+so,bs=hm-sm;
    if(d<=nu){prev.innerHTML='<strong style="color:var(--green)">✓ Scania actief</strong>. Maandlast: €'+Math.round(sm)+'/mnd'}
    else{var dag=Math.ceil((d-nu)/86400000);prev.innerHTML='Verwacht over <strong>'+dag+' dagen</strong>. Huurwagen: ca. €'+Math.round(hm)+'/mnd → Scania: €'+Math.round(sm)+'/mnd. Besparing: <strong style="color:var(--green)">€'+Math.round(bs)+'/mnd</strong>'}
  }
}
function renderP(){
  var c=document.getElementById('prows');if(!c)return;c.innerHTML='';
  pallets.forEach(function(p,i){
    var d=document.createElement('div');d.className='p-row';
    d.innerHTML='<div class="f"><input type="text" value="'+esc(p.naam||'')+'" placeholder="Naam" oninput="pallets['+i+'].naam=this.value"></div>'
      +'<div class="f"><input type="number" step="0.01" value="'+(p.inkoop||'')+'" placeholder="0.00" oninput="pallets['+i+'].inkoop=parseFloat(this.value)||0"></div>'
      +'<div class="f"><input type="number" step="0.01" value="'+(p.reparatie||'')+'" placeholder="0.00" oninput="pallets['+i+'].reparatie=parseFloat(this.value)||0"></div>'
      +'<div class="f"><input type="number" step="0.01" value="'+(p.verkoop||'')+'" placeholder="0.00" oninput="pallets['+i+'].verkoop=parseFloat(this.value)||0"></div>'
      +'<button class="btn btn-sm btn-red" onclick="pallets.splice('+i+',1);renderP()" style="padding:6px 8px">✕</button>';
    c.appendChild(d);
  });
}
function addP(){pallets.push({naam:'',inkoop:0,reparatie:0,rep_aan:false,verkoop:0});renderP()}
function kp(naam){var p=getPals().find(function(x){return x.naam===naam});return p?(parseFloat(p.inkoop)||0)+(p.rep_aan?(parseFloat(p.reparatie)||0):0):0}

// ═══════════════════════════════════════════════
// DAGBLOKKEN (dynamisch op basis van rijdagen)
// ═══════════════════════════════════════════════
function addDag(){
  var n=numDagen()+1;
  if(n>7)return;
  // Sla n op in settings tijdelijk
  var s=GS();s.huur_dgn=n;localStorage.setItem('pl_s',JSON.stringify(s));
  var sel=document.getElementById('s_huur_dgn');if(sel)sel.value=n;
  days[n]=[];kmD[n]=0;dOpen[n]=true;
  renderDagBlocks();herb();
}
function remDag(){
  var n=numDagen();
  if(n<=1)return;
  delete days[n];delete kmD[n];delete dOpen[n];
  var s=GS();s.huur_dgn=n-1;localStorage.setItem('pl_s',JSON.stringify(s));
  var sel=document.getElementById('s_huur_dgn');if(sel)sel.value=n-1;
  renderDagBlocks();herb();
}
function updateDagBlocks(){
  var n=numDagen();
  // Init missing days
  for(var d=1;d<=n;d++){if(!days[d])days[d]=[];if(!kmD[d])kmD[d]=0;if(dOpen[d]===undefined)dOpen[d]=true}
  // Remove extra days
  Object.keys(days).forEach(function(d){if(parseInt(d)>n){delete days[d];delete kmD[d];delete dOpen[d]}});
  renderDagBlocks();
}
function renderDagBlocks(){
  var n=numDagen();
  var c=document.getElementById('dag-blocks');if(!c)return;
  c.innerHTML='';
  for(var d=1;d<=n;d++){
    (function(dag){
      var div=document.createElement('div');div.className='day-block';div.id='dagblock-'+dag;
      div.innerHTML='<div class="day-head" onclick="togD('+dag+')">'
        +'<div class="day-head-l">Dag '+dag+' <span class="badge b-blue" id="km'+dag+'">0 km</span></div>'
        +'<div style="display:flex;align-items:center;gap:8px">'
        +'<button class="btn btn-sm" onclick="event.stopPropagation();doRoute('+dag+')">📍 Route</button>'
        +'<span id="chev'+dag+'" style="color:var(--text3)">▾</span></div></div>'
        +'<div class="day-body" id="db'+dag+'">'
        +'<div id="rv'+dag+'" class="route-vis" style="display:none"></div>'
        +'<div id="st'+dag+'"></div>'
        +'<div style="margin-top:10px"><button class="btn btn-sm" onclick="addStop('+dag+')">+ Levering toevoegen</button></div>'
        +'<div id="rr'+dag+'" style="display:none;margin-top:10px"></div>'
        +'</div>';
      c.appendChild(div);
      if(!days[dag].length)addStop(dag);else renderSt(dag);
    })(d);
  }
  // Verwijderknop tonen als er meer dan 1 dag is
  var remBtn=document.getElementById('btn-rem-dag');
  if(remBtn)remBtn.style.display=n>1?'':'none';
}
function togD(d){
  dOpen[d]=!dOpen[d];
  var b=document.getElementById('db'+d),ch=document.getElementById('chev'+d);
  if(b)b.style.display=dOpen[d]?'block':'none';
  if(ch)ch.textContent=dOpen[d]?'▾':'▸';
}

// ═══════════════════════════════════════════════
// KLANTEN
// ═══════════════════════════════════════════════
function renderK(){
  var q=(gv('kl-s')||'').toLowerCase(),cls=GC();
  var list=q?cls.filter(function(c){return(c.bedrijf+' '+(c.contact||'')+' '+(c.plaats||'')).toLowerCase().includes(q)}):cls;
  var cnt=document.getElementById('kl-cnt');if(cnt)cnt.textContent=list.length+' klanten';
  var tb=document.getElementById('kl-tb'),em=document.getElementById('kl-empty');
  if(!list.length){if(tb)tb.innerHTML='';if(em)em.style.display='';return}
  if(em)em.style.display='none';
  var cols=['#E8D5C4','#C4D5E8','#C4E8D5','#E8E8C4','#D5C4E8'];
  if(tb)tb.innerHTML=list.map(function(c,i){
    var ini=(c.bedrijf||'?')[0].toUpperCase(),col=cols[i%cols.length];
    var adr=[c.straat,c.postcode,c.plaats].filter(Boolean).join(', ');
    return'<tr><td><div style="width:32px;height:32px;border-radius:50%;background:'+col+';display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:700">'+ini+'</div></td>'
      +'<td><strong>'+esc(c.bedrijf||'—')+'</strong></td>'
      +'<td style="color:var(--text2)">'+esc(c.contact||'—')+'</td>'
      +'<td style="font-size:12px;color:var(--text2)">'+esc(adr||'—')+'</td>'
      +'<td>'+(c.pallet?'<span class="badge b-blue">'+esc(c.pallet)+'</span>':'<span style="color:var(--text3)">—</span>')+'</td>'
      +'<td style="font-size:12px;color:var(--text3)">'+esc((c.notities||'').slice(0,40))+'</td>'
      +'<td><div class="racts"><button class="btn btn-xs" onclick="openKM(\''+c.id+'\')">Bewerk</button><button class="btn btn-xs btn-red" onclick="delK(\''+c.id+'\')">✕</button></div></td></tr>';
  }).join('');
}
function openKM(id){
  editKId=id||null;
  var c=id?GC().find(function(x){return x.id===id}):null;
  document.getElementById('km-ttl').textContent=c?'Klant bewerken':'Klant toevoegen';
  ['kf_bedr','kf_cont','kf_tel','kf_email','kf_str','kf_pl','kf_pc','kf_not'].forEach(function(fid){
    var map={kf_bedr:'bedrijf',kf_cont:'contact',kf_tel:'tel',kf_email:'email',kf_str:'straat',kf_pl:'plaats',kf_pc:'postcode',kf_not:'notities'};
    sv(fid,c?(c[map[fid]]||''):'');
  });
  sv('kf_land',c?c.land||'NL':'NL');sv('kf_ant',c?c.aantal||'':'');
  var ps=document.getElementById('kf_pal');ps.innerHTML='<option value="">— geen voorkeur —</option>';
  getPals().forEach(function(p){var o=document.createElement('option');o.value=p.naam;o.textContent=p.naam;if(c&&c.pallet===p.naam)o.selected=true;ps.appendChild(o)});
  document.getElementById('kf_err').style.display='none';
  document.getElementById('km-modal').style.display='flex';
}
function closeKM(){document.getElementById('km-modal').style.display='none'}
function saveK(){
  var bedr=gv('kf_bedr').trim(),str=gv('kf_str').trim(),pl=gv('kf_pl').trim();
  var err=document.getElementById('kf_err');
  if(!bedr||!str||!pl){err.style.display='block';err.textContent='Vul bedrijfsnaam, straat en plaats in.';return}
  err.style.display='none';
  var obj={id:editKId||(Date.now().toString()),bedrijf:bedr,contact:gv('kf_cont'),tel:gv('kf_tel'),email:gv('kf_email'),straat:str,plaats:pl,postcode:gv('kf_pc'),land:gv('kf_land'),pallet:gv('kf_pal'),aantal:parseInt(gv('kf_ant'))||0,notities:gv('kf_not')};
  var cls=GC();
  if(editKId){var i=cls.findIndex(function(c){return c.id===editKId});if(i>-1)cls[i]=obj;else cls.push(obj)}else cls.push(obj);
  SC(cls);closeKM();renderK();showToast(editKId?'Klant bijgewerkt':'Klant toegevoegd');
}
function delK(id){if(!confirm('Klant verwijderen?'))return;SC(GC().filter(function(x){return x.id!==id}));renderK()}

// ═══════════════════════════════════════════════
// WEEKINVOER
// ═══════════════════════════════════════════════
function updV(){
  var b=parseInt(gv('v_begin'))||0,br=parseInt(gv('v_bracht_in'))||0,op=parseInt(gv('v_opgh_in'))||0;
  var hui=b+br-op;
  document.getElementById('v-huidig').textContent=hui;
  document.getElementById('v-bracht').textContent='+'+br;
  document.getElementById('v-opgh').textContent='−'+op;
  var lg=document.getElementById('v-log');
  if(lg)lg.textContent=hui>=0?'Eindstand: '+hui+' pallets bij Veeke':'⚠ Negatieve stand — controleer invoer';
}
function updRV(dag){
  var rv=document.getElementById('rv'+dag);if(!rv)return;
  var depot=getDepot().split(',')[0];
  var stops=(days[dag]||[]).filter(function(s){return s.adres});
  if(!stops.length){rv.style.display='none';return}
  rv.style.display='flex';
  rv.innerHTML='<span class="rnode-d">'+esc(depot)+'</span>';
  stops.forEach(function(s){rv.innerHTML+='<span class="rarr">→</span><span class="rnode-s '+(s.coords?'rnode-ok':'')+'" title="'+esc(s.adres)+'">'+esc(s.adres.split(',')[0])+'</span>'});
  rv.innerHTML+='<span class="rarr">→</span><span class="rnode-d">'+esc(depot)+'</span>';
}
function renderSt(dag){
  var c=document.getElementById('st'+dag);if(!c)return;
  var s=GS();c.innerHTML='';
  (days[dag]||[]).forEach(function(stop,i){
    var kkp=stop.pallet?(function(){var p=getPals().find(function(x){return x.naam===stop.pallet});return p?(parseFloat(p.inkoop)||0)+(p.rep_aan?(parseFloat(p.reparatie)||0):0):0})():0;
    var m=stop.inkoop&&stop.verkoop?((stop.verkoop-stop.inkoop)/stop.verkoop*100).toFixed(1):null;
    var ms=m!==null?(parseFloat(m)>=20?'background:var(--green-bg);color:var(--green)':parseFloat(m)>=10?'background:var(--orange-bg);color:var(--orange)':'background:var(--red-bg);color:var(--red)'):'';
    var div=document.createElement('div');
    div.className='stop-card'+(stop.kaan?' kaan':'');
    div.innerHTML='<div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:12px">'
      +'<div style="display:flex;align-items:center;gap:8px"><span class="badge b-blue">Levering '+(i+1)+'</span>'+(stop.adres?'<span style="font-size:11px;color:var(--text3)">'+esc(stop.adres)+'</span>':'')+'</div>'
      +'<button class="btn btn-xs btn-red" onclick="remStop('+dag+','+i+')">✕</button></div>'
      +'<div class="g2 mb"><div class="f"><label>Klant</label><select onchange="onCC('+dag+','+i+',this.value)" id="sc'+dag+'_'+i+'"></select></div>'
      +'<div class="f"><label>Pallettype</label><select onchange="days['+dag+']['+i+'].pallet=this.value;days['+dag+']['+i+'].inkoop=kp(this.value);herb()" id="sp'+dag+'_'+i+'"></select></div></div>'
      +'<div class="g3 mb">'
      +'<div class="f"><label>Aantal pallets</label><input type="number" class="big" min="0" value="'+(stop.aantal||'')+'" placeholder="0" oninput="days['+dag+']['+i+'].aantal=parseInt(this.value)||0;herb()"></div>'
      +'<div class="f"><label>Inkoop p/st (EUR)</label><input type="number" step="0.01" min="0" value="'+(stop.inkoop||'')+'" placeholder="0.00" oninput="days['+dag+']['+i+'].inkoop=parseFloat(this.value)||0;herb()">'+(kkp>0?'<span style="font-size:11px;color:var(--text3)">kostprijs: €'+kkp.toFixed(2)+'</span>':'')+'</div>'
      +'<div class="f"><label>Verkoop p/st (EUR)</label><input type="number" step="0.01" min="0" value="'+(stop.verkoop||'')+'" placeholder="0.00" oninput="days['+dag+']['+i+'].verkoop=parseFloat(this.value)||0;herb()">'+(m!==null?'<span style="font-size:11px;padding:2px 7px;border-radius:4px;display:inline-block;margin-top:2px;'+ms+'">'+m+'% marge</span>':'')+'</div></div>'
      +'<div style="display:flex;align-items:center;gap:14px;flex-wrap:wrap;padding-top:10px;border-top:1px solid var(--border2)">'
      +'<label style="display:flex;align-items:center;gap:6px;font-size:13px;cursor:pointer"><input type="checkbox" '+(stop.kaan?'checked':'')+' onchange="days['+dag+']['+i+'].kaan=this.checked;herb();renderSt('+dag+')" style="width:15px;height:15px"> Geregeld door Kaan</label>'
      +(stop.kaan?'<span style="font-size:12px;color:var(--blue)">Commissie: €'+((parseFloat(s.comm)||100).toFixed(2))+'</span>':'')
      +((stop.aantal&&stop.verkoop)?'<span style="font-size:13px;color:var(--text2)">Omzet: <strong>€'+((stop.aantal||0)*(stop.verkoop||0)).toFixed(2)+'</strong></span>':'')
      +'</div>';
    c.appendChild(div);
    var csel=document.getElementById('sc'+dag+'_'+i);
    csel.innerHTML='<option value="">— kies klant —</option>';
    GC().forEach(function(cl){var o=document.createElement('option');o.value=cl.id;o.textContent=cl.bedrijf;if(cl.id===stop.cId)o.selected=true;csel.appendChild(o)});
    var psel=document.getElementById('sp'+dag+'_'+i);
    psel.innerHTML='<option value="">— type —</option>';
    getPals().forEach(function(p){var o=document.createElement('option');o.value=p.naam;o.textContent=p.naam;if(p.naam===stop.pallet)o.selected=true;psel.appendChild(o)});
  });
  updRV(dag);herb();
}
function onCC(dag,i,cId){
  var cl=GC().find(function(x){return x.id===cId});
  days[dag][i].cId=cId;days[dag][i].coords=null;
  if(cl){days[dag][i].adres=[cl.straat,cl.postcode,cl.plaats].filter(Boolean).join(', ');
    if(!days[dag][i].pallet&&cl.pallet){days[dag][i].pallet=cl.pallet;days[dag][i].inkoop=kp(cl.pallet)}
    if(!days[dag][i].aantal&&cl.aantal)days[dag][i].aantal=cl.aantal}
  renderSt(dag);
}
function addStop(dag){
  if(!days[dag])days[dag]=[];
  days[dag].push({type:'lev',cId:'',pallet:'',aantal:0,inkoop:0,verkoop:0,kaan:false,adres:'',coords:null});
  renderSt(dag);
}
function remStop(dag,i){days[dag].splice(i,1);renderSt(dag)}

async function geocode(adr,key){
  try{var r=await fetch('https://api.openrouteservice.org/geocode/search?api_key='+key+'&text='+encodeURIComponent(adr)+'&boundary.country=NL,BE,DE&size=1');
  var d=await r.json();if(d.features&&d.features[0])return d.features[0].geometry.coordinates}catch(e){}return null;
}
async function doRoute(dag){
  var key=gv('w_ors').trim();if(!key){alert('Vul eerst je ORS API key in (Instellingen).');return}
  var rr=document.getElementById('rr'+dag);rr.style.display='block';
  rr.innerHTML='<span style="display:inline-block;animation:spin 1s linear infinite">◌</span> Route berekenen...';
  rr.style.cssText='display:block;margin-top:10px;padding:8px 12px;background:var(--surface2);border-radius:8px;font-size:13px';
  try{
    var dc=await geocode(getDepot(),key);if(!dc){rr.innerHTML='⚠ Depot niet gevonden. Controleer adres in Instellingen.';return}
    var stops=(days[dag]||[]).filter(function(x){return x.adres});if(!stops.length){rr.innerHTML='⚠ Geen stops met adres.';return}
    for(var i=0;i<stops.length;i++){if(!stops[i].coords)stops[i].coords=await geocode(stops[i].adres,key)}
    var coords=[dc].concat(stops.filter(function(s){return s.coords}).map(function(s){return s.coords})).concat([dc]);
    var resp=await fetch('https://api.openrouteservice.org/v2/directions/driving-car',{method:'POST',headers:{'Content-Type':'application/json','Authorization':key},body:JSON.stringify({coordinates:coords,units:'km'})});
    var data=await resp.json();
    if(data.routes&&data.routes[0]){
      var km=Math.round(data.routes[0].summary.distance),mins=Math.round(data.routes[0].summary.duration/60),u=Math.floor(mins/60),m=mins%60;
      kmD[dag]=km;document.getElementById('km'+dag).textContent=km+' km';
      rr.style.cssText='display:block;margin-top:10px;padding:8px 14px;background:var(--green-bg);border-radius:8px;font-size:13px;color:var(--green);border:1px solid var(--green-border)';
      rr.innerHTML='✓ <strong>'+km+' km</strong> · Ca. <strong>'+(u>0?u+'u ':'')+m+' min</strong> · <span style="color:var(--text3);font-size:12px">Depot → '+stops.filter(function(s){return s.coords}).length+' stops → Depot</span>';
      herb();
    }else{rr.innerHTML='⚠ '+(data.error&&data.error.message?data.error.message:'Route niet berekend')}
  }catch(e){rr.innerHTML='⚠ Fout: '+e.message}
}

function herb(){
  var s=GS(),diesel=parseFloat(gv('w_diesel'))||parseFloat(s.diesel)||1.75;
  var n=numDagen();
  var totKm=0;for(var d=1;d<=n;d++)totKm+=(kmD[d]||0);
  var isS=s.sc_datum&&new Date(s.sc_datum)<=new Date();
  var vbr=parseFloat(isS?s.sc_vbr:s.huur_vbr)||28;
  var dK=totKm*(vbr/100)*diesel,wK=totKm*(parseFloat(s.wegbel)||0.20);
  var hK=isS?0:(parseFloat(s.huur_dag)||230)*n;
  var vMnd=(parseFloat(s.trailer)||0)+(parseFloat(s.verz)||0)+(parseFloat(s.admin)||0)+(parseFloat(s.overig)||0)+(isS?(parseFloat(s.sc_lease)||0)+(parseFloat(s.sc_ondh)||0):0);
  var vWk=vMnd/4.33,omz=0,ink=0,com=0;
  for(var dd=1;dd<=n;dd++){(days[dd]||[]).forEach(function(st){omz+=(st.aantal||0)*(st.verkoop||0);ink+=(st.aantal||0)*(st.inkoop||0);if(st.kaan)com+=parseFloat(s.comm)||100})}
  var bruto=omz-ink,totK=dK+wK+hK+vWk+com,netto=bruto-totK,doelW=(parseFloat(s.winstdoel)||4000)/4.33;
  // Update huurinfo balk
  var lbl=document.getElementById('w-huur-label');
  var det=document.getElementById('w-huur-detail');
  var kos=document.getElementById('w-huur-kosten');
  var dagTarief=parseFloat(s.huur_dag)||230;
  if(lbl&&det&&kos){
    if(isS){
      lbl.textContent='Scania actief';
      det.textContent='Vaste lease — geen huurwagen';
      kos.textContent='€0';
      document.getElementById('w-huur-info').style.background='var(--green-bg)';
      document.getElementById('w-huur-info').style.borderColor='var(--green-border)';
      lbl.style.color='var(--green)';kos.style.color='var(--green)';
    } else {
      lbl.textContent=n+' rijdag'+(n>1?'en':'')+' deze week';
      det.textContent='€'+dagTarief.toFixed(2)+' excl. btw per dag × '+n+' dag'+(n>1?'en':'');
      kos.textContent='€'+(hK).toFixed(2);
      document.getElementById('w-huur-info').style.background='var(--orange-bg)';
      document.getElementById('w-huur-info').style.borderColor='var(--orange-border)';
      lbl.style.color='var(--orange)';kos.style.color='var(--orange)';
    }
  }
  document.getElementById('w-omz').textContent=feDec(omz);
  document.getElementById('w-ink').textContent=feDec(ink);
  document.getElementById('w-kos').textContent=feDec(totK);
  var wwin=document.getElementById('w-win');wwin.textContent=feDec(netto);
  wwin.style.color=netto>=doelW?'var(--green)':netto>0?'var(--orange)':'var(--red)';
  var sl=document.getElementById('w-sl');
  if(omz===0){sl.className='banner banner-gray';sl.textContent='Voer leveringen in om resultaat te zien'}
  else if(netto>=doelW){sl.className='banner banner-green';sl.textContent='✓ Op koers — boven weekdoelstelling (€'+Math.round(doelW)+')'}
  else if(netto>0){sl.className='banner banner-orange';sl.textContent='▲ Positief maar onder weekdoel — tekort: €'+Math.round(doelW-netto)}
  else{sl.className='banner banner-red';sl.textContent='✕ Verlies deze week — actie vereist'}
  var rows=[['Omzet','€'+omz.toFixed(2)],['Inkoopkosten','− €'+ink.toFixed(2)],['Brutowinst','€'+bruto.toFixed(2),'','fw'],
    [isS?'Scania lease/week':'Huurwagen ('+n+' dag'+(n>1?'en':'')+')', '− €'+hK.toFixed(2)],
    ['Diesel ('+totKm+' km)','− €'+dK.toFixed(2)],['Wegenbelasting','− €'+wK.toFixed(2)],
    ['Vaste lasten (weekdeel)','− €'+vWk.toFixed(2)],['Commissie Kaan','− €'+com.toFixed(2)],
    ['Nettowinst','€'+netto.toFixed(2)]];
  document.getElementById('w-kv').innerHTML=rows.map(function(r,i){
    var last=i===rows.length-1;
    return'<div class="wsum-row"'+(last?' style="font-weight:700;font-size:14px"':'')+'><span>'+r[0]+'</span><span style="color:'+(last?(netto>=0?'var(--green)':'var(--red)'):'inherit')+'">'+r[1]+'</span></div>';
  }).join('');
}

function slaOp(){
  var wk=gv('w_wk'),jr=gv('w_jr'),key='pl_w_'+jr+'_'+wk;
  var vhuidig=parseInt(document.getElementById('v-huidig').textContent)||0;
  localStorage.setItem('pl_veeke',JSON.stringify({voorraad:vhuidig,begin:parseInt(gv('v_begin'))||0,bracht:parseInt(gv('v_bracht_in'))||0,opgh:parseInt(gv('v_opgh_in'))||0,ts:new Date().toISOString()}));
  localStorage.setItem(key,JSON.stringify({week:wk,jaar:jr,days:days,kmDay:kmD,diesel:gv('w_diesel'),dgn:numDagen(),veeke:{begin:parseInt(gv('v_begin'))||0,bracht:parseInt(gv('v_bracht_in'))||0,opgh:parseInt(gv('v_opgh_in'))||0},ts:new Date().toISOString()}));
  var idx=JSON.parse(localStorage.getItem('pl_wi')||'[]');if(idx.indexOf(key)<0)idx.push(key);
  localStorage.setItem('pl_wi',JSON.stringify(idx));
  var ok=document.getElementById('w-ok');ok.style.display='block';setTimeout(function(){ok.style.display='none'},3000);
  showToast('Week '+wk+'/'+jr+' opgeslagen');
}
function resetWeek(){
  var wk=gv('w_wk'),jr=gv('w_jr'),key='pl_w_'+jr+'_'+wk;
  if(!confirm('Week '+wk+'/'+jr+' verwijderen?'))return;
  localStorage.removeItem(key);
  var idx=JSON.parse(localStorage.getItem('pl_wi')||'[]').filter(function(k){return k!==key});
  localStorage.setItem('pl_wi',JSON.stringify(idx));
  var ok=document.getElementById('w-reset-ok');ok.style.display='block';setTimeout(function(){ok.style.display='none'},4000);
}
function nwWeek(){
  if(!confirm('Nieuwe week starten?'))return;
  var vhuidig=parseInt(document.getElementById('v-huidig').textContent)||0;
  var n=numDagen();
  days={};kmD={};dOpen={};
  for(var d=1;d<=n;d++){days[d]=[];kmD[d]=0;dOpen[d]=true}
  var now=new Date();sv('w_wk',wkNum(now));sv('w_jr',now.getFullYear());
  sv('v_begin',vhuidig);sv('v_bracht_in',0);sv('v_opgh_in',0);
  renderDagBlocks();updV();
}

// ═══════════════════════════════════════════════
// BEREKENINGEN
// ═══════════════════════════════════════════════
function calcW(w){
  var s=GS(),diesel=parseFloat(w.diesel)||parseFloat(s.diesel)||1.75;
  var dd=w.days||{},kd=w.kmDay||{};
  var dgn=parseInt(w.dgn)||parseInt(s.huur_dgn)||2;
  var totKm=0;Object.keys(kd).forEach(function(d){totKm+=kd[d]||0});
  var isS=s.sc_datum&&new Date(s.sc_datum)<=new Date(parseInt(w.jaar),0,1+(parseInt(w.week)-1)*7);
  var vbr=parseFloat(isS?s.sc_vbr:s.huur_vbr)||28;
  var dK=totKm*(vbr/100)*diesel,wK=totKm*(parseFloat(s.wegbel)||0.20);
  var hK=isS?0:(parseFloat(s.huur_dag)||230)*dgn;
  var vMnd=(parseFloat(s.trailer)||0)+(parseFloat(s.verz)||0)+(parseFloat(s.admin)||0)+(parseFloat(s.overig)||0)+(isS?(parseFloat(s.sc_lease)||0)+(parseFloat(s.sc_ondh)||0):0);
  var vWk=vMnd/4.33,omz=0,ink=0,com=0,nP=0;
  Object.keys(dd).forEach(function(dag){(dd[dag]||[]).forEach(function(st){omz+=(st.aantal||0)*(st.verkoop||0);ink+=(st.aantal||0)*(st.inkoop||0);nP+=(st.aantal||0);if(st.kaan)com+=parseFloat(s.comm)||100})});
  var bruto=omz-ink,totK=dK+wK+hK+vWk+com,netto=bruto-totK;
  // Automatisch maand en kwartaal bepalen
  var wkInt=parseInt(w.week),jrInt=parseInt(w.jaar);
  var approxDate=new Date(jrInt,0,1+(wkInt-1)*7);
  var maand=approxDate.getMonth(); // 0-11
  var kwartaal=Math.floor(maand/3)+1; // 1-4
  return{lbl:'W'+w.week+"'"+String(w.jaar).slice(2),week:wkInt,jaar:jrInt,sk:jrInt*100+wkInt,
    omz:omz,ink:ink,bruto:bruto,tK:dK+wK+hK,vWk:vWk,com:com,totK:totK,netto:netto,
    marge:omz>0?netto/omz*100:0,totKm:totKm,nP:nP,dK:dK,wK:wK,hK:hK,
    maand:maand,kwartaal:kwartaal,maandNaam:['Jan','Feb','Mrt','Apr','Mei','Jun','Jul','Aug','Sep','Okt','Nov','Dec'][maand]};
}
function getAll(){
  return JSON.parse(localStorage.getItem('pl_wi')||'[]')
    .map(function(k){var r=localStorage.getItem(k);return r?calcW(JSON.parse(r)):null})
    .filter(Boolean).sort(function(a,b){return a.sk-b.sk});
}
function filW(weken){
  if(dFil==='jaar')return weken.filter(function(w){return w.jaar===new Date().getFullYear()});
  var n=dFil==='2w'?2:dFil==='4w'?4:dFil==='8w'?8:12;return weken.slice(-n);
}
function setFil(f,el){dFil=f;document.querySelectorAll('#pg-dash .fb').forEach(function(b){b.classList.remove('on')});el.classList.add('on');renderDash()}

// ═══════════════════════════════════════════════
// DASHBOARD
// ═══════════════════════════════════════════════
function renderDash(){
  var all=getAll(),weken=filW(all),s=GS(),doelW=(parseFloat(s.winstdoel)||4000)/4.33;
  if(!weken.length){
    ['d-omz','d-win','d-marg','d-km'].forEach(function(id){var el=document.getElementById(id);if(el)el.textContent='—'});
    var dsl=document.getElementById('d-sl');if(dsl){dsl.className='banner banner-gray';dsl.textContent='Nog geen weekdata — sla eerst een week op via Weekinvoer'}
    renderScaniaBox(s);return;
  }
  var tO=weken.reduce(function(a,w){return a+w.omz},0),tW=weken.reduce(function(a,w){return a+w.netto},0);
  var tK=weken.reduce(function(a,w){return a+w.totKm},0),gM=tO>0?tW/tO*100:0;
  var omzEl=document.getElementById('d-omz');if(omzEl)omzEl.textContent=fe(tO);
  var winEl=document.getElementById('d-win');if(winEl){winEl.textContent=fe(tW);winEl.style.color=tW>=0?'var(--green)':'var(--red)'}
  var margEl=document.getElementById('d-marg');if(margEl)margEl.textContent=gM.toFixed(1)+'%';
  var kmEl=document.getElementById('d-km');if(kmEl)kmEl.textContent=tK.toLocaleString('nl-NL')+' km';
  var os=document.getElementById('d-omz-s');if(os)os.textContent='gem. '+fe(tO/weken.length)+'/week';
  var ws=document.getElementById('d-win-s');if(ws)ws.textContent='gem. '+fe(tW/weken.length)+'/week';
  var ms=document.getElementById('d-marg-s');if(ms)ms.textContent='netto over omzet';
  var ks=document.getElementById('d-km-s');if(ks)ks.textContent='gem. '+Math.round(tK/weken.length)+' km/week';
  var laats=weken[weken.length-1],dsl=document.getElementById('d-sl');
  if(dsl){
    if(laats.netto>=doelW){dsl.className='banner banner-green';dsl.textContent='✓ Laatste week op koers — '+fe(laats.netto)}
    else if(laats.netto>0){dsl.className='banner banner-orange';dsl.textContent='▲ Positief maar onder weekdoel (€'+Math.round(doelW)+') — tekort: €'+Math.round(doelW-laats.netto)}
    else{dsl.className='banner banner-red';dsl.textContent='✕ Laatste week verlies: '+fe(laats.netto)+' — actie vereist'}
  }
  var gI=weken.reduce(function(a,w){return a+w.ink},0)/Math.max(weken.reduce(function(a,w){return a+w.nP},0),1);
  var gV=weken.reduce(function(a,w){return a+w.omz},0)/Math.max(weken.reduce(function(a,w){return a+w.nP},0),1);
  var gVK=weken.reduce(function(a,w){return a+w.totK-w.ink},0)/weken.length,mpp=gV-gI;
  var beBE=mpp>0?Math.ceil(gVK/mpp):0,beDoel=mpp>0?Math.ceil((gVK+doelW)/mpp):0;
  var gP=Math.round(weken.reduce(function(a,w){return a+w.nP},0)/weken.length),ver=gP-beDoel;
  var bebe=document.getElementById('be-be');if(bebe)bebe.textContent=beBE||'—';
  var bedoel=document.getElementById('be-doel');if(bedoel)bedoel.textContent=beDoel||'—';
  var bever=document.getElementById('be-ver');if(bever){bever.textContent=(ver>=0?'+':'')+ver;bever.style.color=ver>=0?'var(--green)':'var(--red)'}
  var behint=document.getElementById('be-hint');if(behint)behint.textContent='Gem. verkoopprijs €'+gV.toFixed(2)+'/pallet, kostprijs €'+gI.toFixed(2)+'/pallet.';
  drawBar(weken);drawMarg(weken);drawDon(weken);drawMnd(all,parseFloat(s.winstdoel)||4000);drawTblDash(weken,doelW);renderScaniaBox(s);
}
function renderScaniaBox(s){
  var box=document.getElementById('d-sc'),txt=document.getElementById('d-sc-t');
  if(!s.sc_datum){if(box)box.style.display='none';return}
  var d=new Date(s.sc_datum),nu=new Date(),act=d<=nu;
  var hm=(parseFloat(s.huur_dag)||230)*(parseInt(s.huur_dgn)||2)*4.33;
  var sm=(parseFloat(s.sc_lease)||0)+(parseFloat(s.sc_ondh)||0),bs=hm-sm;
  var ds=d.toLocaleDateString('nl-NL',{day:'numeric',month:'long',year:'numeric'});
  if(box)box.style.display='block';
  if(txt){if(act){txt.innerHTML='Scania actief vanaf '+ds+'. Maandlast: <strong>€'+Math.round(sm).toLocaleString('nl-NL')+'</strong>'}
  else{var dag=Math.ceil((d-nu)/86400000);txt.innerHTML='Verwacht op <strong>'+ds+'</strong> — nog <strong>'+dag+' dagen</strong>.<br>Huurwagen: ca. <strong>€'+Math.round(hm).toLocaleString('nl-NL')+'/mnd</strong> → Scania: <strong>€'+Math.round(sm).toLocaleString('nl-NL')+'/mnd</strong><br>Besparing: <strong style="color:var(--green)">€'+Math.round(bs).toLocaleString('nl-NL')+'/mnd · €'+Math.round(bs*12).toLocaleString('nl-NL')+'/jaar</strong>'}}
}
function drawTblDash(weken,doelW){
  var el=document.getElementById('d-tbl');if(!el)return;
  if(!weken.length){el.innerHTML='<div class="empty">Geen data in deze periode</div>';return}
  el.innerHTML='<div class="tbl-wrap"><table class="tbl"><thead><tr><th>Week</th><th>Omzet</th><th>Inkoop</th><th>Kosten</th><th>Nettowinst</th><th>Marge</th><th>Km</th><th>Status</th></tr></thead><tbody>'
    +weken.slice().reverse().map(function(w){var col=w.netto>=doelW?'var(--green)':w.netto>0?'var(--orange)':'var(--red)';var dot=w.netto>=doelW?'dot-g':w.netto>0?'dot-o':'dot-r';
    return'<tr><td><strong>'+w.lbl+'</strong></td><td>'+fe(w.omz)+'</td><td>'+fe(w.ink)+'</td><td>'+fe(w.totK)+'</td><td style="font-weight:700;color:'+col+'">'+fe(w.netto)+'</td><td>'+w.marge.toFixed(1)+'%</td><td>'+w.totKm+' km</td><td><span class="dot '+dot+'"></span>'+(w.netto>=doelW?'Op koers':w.netto>0?'Onder doel':'Verlies')+'</td></tr>'
    }).join('')+'</tbody></table></div>';
}

// ═══════════════════════════════════════════════
// GRAFIEKEN
// ═══════════════════════════════════════════════
function svgBar(weken){
  var W=560,H=180,pl=55,pr=15,pt=10,pb=35,cw=W-pl-pr,ch=H-pt-pb;
  var allV=[];weken.forEach(function(w){allV.push(w.omz,w.totK,w.netto)});
  var mx=Math.max.apply(null,allV.map(Math.abs))||1,mn=Math.min.apply(null,allV)||0;
  var rMin=Math.min(mn,0),rMax=mx*1.1,range=rMax-rMin||1,n=weken.length,bw=Math.floor(cw/n),gbw=Math.max(4,Math.floor((bw-8)/3));
  var zy=pt+ch*(1-(-rMin)/range),svg='<svg viewBox="0 0 '+W+' '+H+'" xmlns="http://www.w3.org/2000/svg" style="width:100%">';
  [0,.25,.5,.75,1].forEach(function(t){var y=pt+ch*(1-t),v=rMin+range*t;svg+='<line x1="'+pl+'" y1="'+y+'" x2="'+(W-pr)+'" y2="'+y+'" stroke="#E8E8E0" stroke-width="1"/><text x="'+(pl-5)+'" y="'+(y+4)+'" text-anchor="end" font-size="10" fill="#AAA">€'+Math.round(v/1000)+'k</text>'});
  svg+='<line x1="'+pl+'" y1="'+zy+'" x2="'+(W-pr)+'" y2="'+zy+'" stroke="#CCC" stroke-width="1.5"/>';
  weken.forEach(function(w,i){var x=pl+i*bw+4;[[w.omz,'#378ADD'],[w.totK,'#F09595'],[w.netto,'#1D9E75']].forEach(function(p,j){var v=p[0],col=p[1],bx=x+j*gbw,by,bh;if(v>=0){by=pt+ch*(1-(v-rMin)/range);bh=zy-by}else{by=zy;bh=Math.abs(pt+ch*(1-(v-rMin)/range)-zy)}if(bh<1)bh=1;svg+='<rect x="'+bx+'" y="'+by+'" width="'+(gbw-2)+'" height="'+bh+'" fill="'+col+'" rx="2" opacity=".9"/>'});svg+='<text x="'+(x+bw/2-8)+'" y="'+(H-4)+'" text-anchor="middle" font-size="10" fill="#AAA">'+w.lbl+'</text>'});
  return svg+'</svg>';
}
function drawBar(w){var el=document.getElementById('ch-bar');if(el)el.innerHTML=svgBar(w)}
function svgMarg(weken){
  var W=560,H=150,pl=45,pr=15,pt=10,pb=30,cw=W-pl-pr,ch=H-pt-pb;
  var vals=weken.map(function(w){return w.marge}),mx=Math.max.apply(null,vals.map(Math.abs))||10,mn=Math.min.apply(null,vals)||0;
  var rMin=Math.min(mn,0)-2,rMax=mx*1.2+2,range=rMax-rMin||1,n=weken.length;
  var svg='<svg viewBox="0 0 '+W+' '+H+'" xmlns="http://www.w3.org/2000/svg" style="width:100%">';
  [0,.5,1].forEach(function(t){var y=pt+ch*(1-t),v=rMin+range*t;svg+='<line x1="'+pl+'" y1="'+y+'" x2="'+(W-pr)+'" y2="'+y+'" stroke="#E8E8E0" stroke-width="1"/><text x="'+(pl-4)+'" y="'+(y+4)+'" text-anchor="end" font-size="10" fill="#AAA">'+Math.round(v)+'%</text>'});
  if(n>1){var pts=weken.map(function(w,i){var x=pl+(i/(n-1))*cw,y=pt+ch*(1-(w.marge-rMin)/range);return x+','+y});svg+='<polyline points="'+pts.join(' ')+'" fill="none" stroke="#1D9E75" stroke-width="2.5" stroke-linejoin="round" stroke-linecap="round"/>';weken.forEach(function(w,i){var x=pl+(i/(n-1))*cw,y=pt+ch*(1-(w.marge-rMin)/range);svg+='<circle cx="'+x+'" cy="'+y+'" r="4" fill="#1D9E75"/>'})}
  weken.forEach(function(w,i){var x=pl+(n>1?(i/(n-1))*cw:cw/2);svg+='<text x="'+x+'" y="'+(H-3)+'" text-anchor="middle" font-size="10" fill="#AAA">'+w.lbl+'</text>'});
  return svg+'</svg>';
}
function drawMarg(w){var el=document.getElementById('ch-marg');if(el)el.innerHTML=svgMarg(w)}
function svgDon(weken){
  var tots=[weken.reduce(function(a,w){return a+w.tK},0),weken.reduce(function(a,w){return a+w.vWk},0),weken.reduce(function(a,w){return a+w.ink},0),weken.reduce(function(a,w){return a+w.com},0)];
  var lbls=['Transport','Vaste lasten','Inkoop','Commissie'],cols=['#378ADD','#7F77DD','#F09595','#EF9F27'];
  var total=tots.reduce(function(a,v){return a+v},0)||1,W=300,H=260,cx=130,cy=110,r=85,ri=50,ang=-Math.PI/2;
  var svg='<svg viewBox="0 0 '+W+' '+H+'" xmlns="http://www.w3.org/2000/svg" style="width:100%">';
  tots.forEach(function(v,i){var sw=v/total*Math.PI*2,x1=cx+r*Math.cos(ang),y1=cy+r*Math.sin(ang),x2=cx+r*Math.cos(ang+sw),y2=cy+r*Math.sin(ang+sw),xi1=cx+ri*Math.cos(ang),yi1=cy+ri*Math.sin(ang),xi2=cx+ri*Math.cos(ang+sw),yi2=cy+ri*Math.sin(ang+sw),lg=sw>Math.PI?1:0;if(v>0)svg+='<path d="M '+xi1+' '+yi1+' L '+x1+' '+y1+' A '+r+' '+r+' 0 '+lg+' 1 '+x2+' '+y2+' L '+xi2+' '+yi2+' A '+ri+' '+ri+' 0 '+lg+' 0 '+xi1+' '+yi1+' Z" fill="'+cols[i]+'"/>';ang+=sw});
  tots.forEach(function(v,i){var lx=10,ly=H-60+i*18;svg+='<rect x="'+lx+'" y="'+(ly-8)+'" width="10" height="10" fill="'+cols[i]+'" rx="2"/><text x="'+(lx+14)+'" y="'+ly+'" font-size="11" fill="#888">'+lbls[i]+' (€'+Math.round(v).toLocaleString('nl-NL')+')</text>'});
  return svg+'</svg>';
}
function drawDon(w){var el=document.getElementById('ch-don');if(el)el.innerHTML=svgDon(w)}
function svgMnd(all,doelMnd){
  var mnd={},mn=['Jan','Feb','Mrt','Apr','Mei','Jun','Jul','Aug','Sep','Okt','Nov','Dec'];
  all.forEach(function(w){var k=w.jaar+'-'+w.maand;if(!mnd[k])mnd[k]={lbl:mn[w.maand]+"'"+(String(w.jaar).slice(2)),v:0};mnd[k].v+=w.netto});
  var keys=Object.keys(mnd).sort(),data=keys.map(function(k){return Math.round(mnd[k].v)}),lbls=keys.map(function(k){return mnd[k].lbl});
  var W=380,H=200,pl=50,pr=10,pt=10,pb=35,cw=W-pl-pr,ch=H-pt-pb;
  var mx=Math.max.apply(null,data.map(Math.abs).concat([doelMnd]))||1,mn2=Math.min.apply(null,data.concat([0]))||0;
  var rMin=Math.min(mn2,0),rMax=mx*1.15,range=rMax-rMin||1,n=data.length||1,bw=Math.floor(cw/n),bp=Math.max(2,Math.floor(bw*.2));
  var zy=pt+ch*(1-(-rMin)/range),svg='<svg viewBox="0 0 '+W+' '+H+'" xmlns="http://www.w3.org/2000/svg" style="width:100%">';
  [0,.5,1].forEach(function(t){var y=pt+ch*(1-t),v=rMin+range*t;svg+='<line x1="'+pl+'" y1="'+y+'" x2="'+(W-pr)+'" y2="'+y+'" stroke="#E8E8E0" stroke-width="1"/><text x="'+(pl-4)+'" y="'+(y+4)+'" text-anchor="end" font-size="10" fill="#AAA">€'+Math.round(v/1000)+'k</text>'});
  var dy=pt+ch*(1-(doelMnd-rMin)/range);if(dy>=pt&&dy<=pt+ch)svg+='<line x1="'+pl+'" y1="'+dy+'" x2="'+(W-pr)+'" y2="'+dy+'" stroke="#2D7A4F" stroke-width="1" stroke-dasharray="4,3"/><text x="'+(W-pr-2)+'" y="'+(dy-3)+'" text-anchor="end" font-size="9" fill="#2D7A4F">doel</text>';
  svg+='<line x1="'+pl+'" y1="'+zy+'" x2="'+(W-pr)+'" y2="'+zy+'" stroke="#CCC" stroke-width="1.5"/>';
  data.forEach(function(v,i){var x=pl+i*bw+bp,bx=Math.max(bw-bp*2,2),col=v>=doelMnd?'#1D9E75':v>0?'#EF9F27':'#E24B4A',by,bh;if(v>=0){by=pt+ch*(1-(v-rMin)/range);bh=zy-by}else{by=zy;bh=Math.abs(pt+ch*(1-(v-rMin)/range)-zy)}if(bh<1)bh=1;svg+='<rect x="'+x+'" y="'+by+'" width="'+bx+'" height="'+bh+'" fill="'+col+'" rx="2" opacity=".9"/><text x="'+(x+bx/2)+'" y="'+(H-4)+'" text-anchor="middle" font-size="9" fill="#AAA">'+lbls[i]+'</text>'});
  return svg+'</svg>';
}
function drawMnd(a,d){var el=document.getElementById('ch-mnd');if(el)el.innerHTML=svgMnd(a,d)}

// ═══════════════════════════════════════════════
// PERIODE (MAAND / KWARTAAL)
// ═══════════════════════════════════════════════
function setPeriod(mode,btn){periodeMode=mode;document.querySelectorAll('.ptab').forEach(function(b){b.classList.remove('on')});btn.classList.add('on');renderPeriode()}
function renderPeriode(){
  var all=getAll(),s=GS(),doelMnd=parseFloat(s.winstdoel)||4000;
  var c=document.getElementById('periode-content');if(!c)return;
  if(!all.length){c.innerHTML='<div class="card"><div class="empty">Nog geen weekdata beschikbaar</div></div>';return}
  if(periodeMode==='maand') renderMaanden(all,doelMnd,c);
  else renderKwartalen(all,doelMnd,c);
}
function groeperMaand(all){
  var mnd={};
  all.forEach(function(w){var k=w.jaar+'-'+('0'+(w.maand+1)).slice(-2);if(!mnd[k])mnd[k]={lbl:w.maandNaam+' '+w.jaar,jaar:w.jaar,maand:w.maand,weken:[],omz:0,ink:0,netto:0,km:0,nP:0};mnd[k].weken.push(w);mnd[k].omz+=w.omz;mnd[k].ink+=w.ink;mnd[k].netto+=w.netto;mnd[k].km+=w.totKm;mnd[k].nP+=w.nP});
  return Object.keys(mnd).sort().map(function(k){return mnd[k]});
}
function groeperKwartaal(all){
  var kw={};
  all.forEach(function(w){var k=w.jaar+'-Q'+w.kwartaal;if(!kw[k])kw[k]={lbl:'Q'+w.kwartaal+' '+w.jaar,jaar:w.jaar,kwartaal:w.kwartaal,weken:[],omz:0,ink:0,netto:0,km:0,nP:0};kw[k].weken.push(w);kw[k].omz+=w.omz;kw[k].ink+=w.ink;kw[k].netto+=w.netto;kw[k].km+=w.totKm;kw[k].nP+=w.nP});
  return Object.keys(kw).sort().map(function(k){return kw[k]});
}
function renderMaanden(all,doelMnd,c){
  var maanden=groeperMaand(all);
  var html='';
  // KPI-totalen
  var totO=maanden.reduce(function(a,m){return a+m.omz},0),totN=maanden.reduce(function(a,m){return a+m.netto},0);
  html+='<div class="kpi-grid">';
  html+='<div class="kpi"><div class="kpi-label">Totale omzet</div><div class="kpi-value">'+fe(totO)+'</div><div class="kpi-sub">'+maanden.length+' maanden</div></div>';
  html+='<div class="kpi"><div class="kpi-label">Totale nettowinst</div><div class="kpi-value" style="color:'+(totN>=0?'var(--green)':'var(--red)')+'">'+fe(totN)+'</div></div>';
  html+='<div class="kpi"><div class="kpi-label">Gem. omzet/maand</div><div class="kpi-value">'+fe(totO/maanden.length)+'</div></div>';
  html+='<div class="kpi"><div class="kpi-label">Gem. winst/maand</div><div class="kpi-value" style="color:'+(totN/maanden.length>=0?'var(--green)':'var(--red)')+'">'+fe(totN/maanden.length)+'</div></div>';
  html+='</div>';
  html+='<div class="tbl-wrap"><table class="period-tbl"><thead><tr><th>Maand</th><th>Weken</th><th>Pallets</th><th>Omzet</th><th>Inkoop</th><th>Nettowinst</th><th>Marge</th><th>Km</th><th>vs. Doel</th></tr></thead><tbody>';
  maanden.slice().reverse().forEach(function(m){
    var marge=m.omz>0?m.netto/m.omz*100:0;
    var ok=m.netto>=doelMnd,pos=m.netto>0;
    var col=ok?'var(--green)':pos?'var(--orange)':'var(--red)';
    var diff=m.netto-doelMnd;
    html+='<tr>';
    html+='<td><strong>'+esc(m.lbl)+'</strong></td>';
    html+='<td>'+m.weken.length+'</td>';
    html+='<td>'+m.nP+'</td>';
    html+='<td>'+fe(m.omz)+'</td>';
    html+='<td>'+fe(m.ink)+'</td>';
    html+='<td style="font-weight:700;color:'+col+'">'+fe(m.netto)+'</td>';
    html+='<td>'+marge.toFixed(1)+'%</td>';
    html+='<td>'+m.km+' km</td>';
    html+='<td style="color:'+col+';font-weight:600">'+(diff>=0?'+':'')+fe(diff)+'</td>';
    html+='</tr>';
    // Weken uitsplitsing
    m.weken.forEach(function(w){
      html+='<tr style="background:var(--surface2);font-size:12px">';
      html+='<td style="padding-left:24px;color:var(--text3)">'+esc(w.lbl)+'</td>';
      html+='<td></td><td>'+w.nP+'</td><td>'+fe(w.omz)+'</td><td>'+fe(w.ink)+'</td>';
      html+='<td style="color:'+(w.netto>=0?'var(--green)':'var(--red)')+'">'+fe(w.netto)+'</td>';
      html+='<td>'+w.marge.toFixed(1)+'%</td><td>'+w.totKm+' km</td><td></td></tr>';
    });
  });
  html+='</tbody></table></div>';
  c.innerHTML=html;
}
function renderKwartalen(all,doelMnd,c){
  var kwartalen=groeperKwartaal(all);
  var doelKw=doelMnd*3;
  var html='';
  var totO=kwartalen.reduce(function(a,k){return a+k.omz},0),totN=kwartalen.reduce(function(a,k){return a+k.netto},0);
  html+='<div class="kpi-grid">';
  html+='<div class="kpi"><div class="kpi-label">Totale omzet</div><div class="kpi-value">'+fe(totO)+'</div></div>';
  html+='<div class="kpi"><div class="kpi-label">Totale nettowinst</div><div class="kpi-value" style="color:'+(totN>=0?'var(--green)':'var(--red)')+'">'+fe(totN)+'</div></div>';
  html+='<div class="kpi"><div class="kpi-label">Gem. omzet/kwartaal</div><div class="kpi-value">'+fe(totO/kwartalen.length)+'</div></div>';
  html+='<div class="kpi"><div class="kpi-label">Doel/kwartaal</div><div class="kpi-value" style="color:var(--green)">'+fe(doelKw)+'</div></div>';
  html+='</div>';
  html+='<div class="tbl-wrap"><table class="period-tbl"><thead><tr><th>Kwartaal</th><th>Maanden</th><th>Weken</th><th>Omzet</th><th>Inkoop</th><th>Nettowinst</th><th>Marge</th><th>vs. Kwartaaldoel</th></tr></thead><tbody>';
  kwartalen.slice().reverse().forEach(function(k){
    var marge=k.omz>0?k.netto/k.omz*100:0;
    var ok=k.netto>=doelKw;
    var col=ok?'var(--green)':k.netto>0?'var(--orange)':'var(--red)';
    var diff=k.netto-doelKw;
    var maandCount=[...new Set(k.weken.map(function(w){return w.maand}))].length;
    html+='<tr>';
    html+='<td><strong>'+esc(k.lbl)+'</strong></td>';
    html+='<td>'+maandCount+'</td><td>'+k.weken.length+'</td>';
    html+='<td>'+fe(k.omz)+'</td><td>'+fe(k.ink)+'</td>';
    html+='<td style="font-weight:700;color:'+col+'">'+fe(k.netto)+'</td>';
    html+='<td>'+marge.toFixed(1)+'%</td>';
    html+='<td style="color:'+col+';font-weight:600">'+(diff>=0?'+':'')+fe(diff)+'</td>';
    html+='</tr>';
  });
  html+='</tbody></table></div>';
  c.innerHTML=html;
}

// ═══════════════════════════════════════════════
// FISCAAL (WINST & BELASTING)
// ═══════════════════════════════════════════════
// VPB-tarieven NL (holding): 19% t/m €200.000, 25.8% daarboven
function berekenVPB(winst){
  if(winst<=0)return 0;
  if(winst<=200000)return winst*0.19;
  return 200000*0.19+(winst-200000)*0.258;
}
function setFiscFil(f,btn){fiscFil=f;document.querySelectorAll('#pg-fiscaal .fb').forEach(function(b){b.classList.remove('on')});btn.classList.add('on');renderFiscaal()}
function renderFiscaal(){
  var all=getAll();
  var weken;
  if(fiscFil==='ytd')weken=all.filter(function(w){return w.jaar===new Date().getFullYear()});
  else weken=all.slice(-12);
  var c=document.getElementById('fisc-content');if(!c)return;
  if(!weken.length){c.innerHTML='<div class="card"><div class="empty">Geen data beschikbaar voor deze periode</div></div>';return}
  var omz=weken.reduce(function(a,w){return a+w.omz},0);
  var ink=weken.reduce(function(a,w){return a+w.ink},0);
  var kosten=weken.reduce(function(a,w){return a+w.totK},0);
  var netto=weken.reduce(function(a,w){return a+w.netto},0);
  var vpb=berekenVPB(netto);
  var naVpb=netto-vpb;
  var vpbPerc=netto>0?(vpb/netto*100).toFixed(1):0;
  var html='';
  // Hoofdcijfers
  html+='<div class="tax-grid">';
  html+='<div class="tax-cell"><div class="tax-val" style="color:var(--blue)">'+fe(netto)+'</div><div class="tax-lbl">Winst vóór belasting</div><div class="tax-note">Zakelijk resultaat</div></div>';
  html+='<div class="tax-cell"><div class="tax-val" style="color:var(--orange)">'+fe(vpb)+'</div><div class="tax-lbl">Geschatte VPB</div><div class="tax-note">'+vpbPerc+'% effectief tarief</div></div>';
  html+='<div class="tax-cell"><div class="tax-val" style="color:var(--green)">'+fe(naVpb)+'</div><div class="tax-lbl">Winst ná belasting</div><div class="tax-note">In holding beschikbaar</div></div>';
  html+='</div>';
  // Toelichting tarief
  html+='<div class="card card-purple">';
  html+='<div class="card-t">VPB-tarieven (D. Stal Holding BV)</div>';
  html+='<div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;font-size:13px">';
  html+='<div><div style="font-size:11px;color:var(--text3);margin-bottom:4px">T/M €200.000 winst</div><div style="font-size:20px;font-weight:700;color:var(--purple)">19%</div></div>';
  html+='<div><div style="font-size:11px;color:var(--text3);margin-bottom:4px">Boven €200.000</div><div style="font-size:20px;font-weight:700;color:var(--purple)">25,8%</div></div>';
  html+='</div>';
  html+='<div style="font-size:12px;color:var(--text2);margin-top:12px;padding-top:10px;border-top:1px solid var(--purple-border)">⚠ Schatting op basis van ingevoerde omzet en kosten. Aftrekposten, afschrijvingen en andere correcties kunnen het werkelijke belastbare bedrag verlagen. Controleer altijd met je accountant.</div>';
  html+='</div>';
  // Resultatenrekening
  html+='<div class="card"><div class="card-t">Resultatenrekening (periode)</div>';
  html+='<table style="width:100%;font-size:13px;border-collapse:collapse"><tbody>';
  var items=[
    {lbl:'Omzet (excl. btw)',v:omz,style:''},
    {lbl:'Inkoopkosten',v:ink,style:'color:var(--red)',prefix:'−'},
    {lbl:'Brutowinst',v:omz-ink,style:'font-weight:700',prefix:''},
    {lbl:'Transportkosten',v:weken.reduce(function(a,w){return a+w.tK},0),style:'color:var(--orange)',prefix:'−'},
    {lbl:'Vaste lasten (weekdeel)',v:weken.reduce(function(a,w){return a+w.vWk},0),style:'color:var(--orange)',prefix:'−'},
    {lbl:'Commissies',v:weken.reduce(function(a,w){return a+w.com},0),style:'color:var(--orange)',prefix:'−'},
    {lbl:'Bedrijfswinst',v:netto,style:'font-weight:700',prefix:''},
    {lbl:'Geschatte VPB ('+vpbPerc+'%)',v:vpb,style:'color:var(--orange)',prefix:'−'},
    {lbl:'Netto winst ná belasting',v:naVpb,style:'font-weight:700;color:'+(naVpb>=0?'var(--green)':'var(--red)'),prefix:''}
  ];
  items.forEach(function(item,i){
    var border=i>0?'border-top:1px solid var(--border2)':'';
    html+='<tr style="'+border+'"><td style="padding:8px 0;'+item.style+'">'+esc(item.lbl)+'</td><td style="text-align:right;padding:8px 0;'+item.style+'">'+(item.prefix?item.prefix+' ':'')+fe(item.v)+'</td></tr>';
  });
  html+='</tbody></table></div>';
  // Prognose jaar
  if(weken.length>0&&fiscFil==='ytd'){
    var huidigMaand=new Date().getMonth()+1;
    var jaarPrognose=huidigMaand>0?(netto/huidigMaand*12):netto;
    var vpbProg=berekenVPB(jaarPrognose);
    html+='<div class="card card-green"><div class="card-t">Jaarprognose (op basis van '+weken.length+' weken YTD)</div>';
    html+='<div class="g3">';
    html+='<div><div style="font-size:11px;color:var(--text3);margin-bottom:4px">Verwachte jaaromzet</div><div style="font-size:22px;font-weight:700">'+fe(omz/huidigMaand*12)+'</div></div>';
    html+='<div><div style="font-size:11px;color:var(--text3);margin-bottom:4px">Verwachte jaarwinst</div><div style="font-size:22px;font-weight:700;color:var(--blue)">'+fe(jaarPrognose)+'</div></div>';
    html+='<div><div style="font-size:11px;color:var(--text3);margin-bottom:4px">Verwachte VPB</div><div style="font-size:22px;font-weight:700;color:var(--orange)">'+fe(vpbProg)+'</div></div>';
    html+='</div></div>';
  }
  c.innerHTML=html;
}

// ═══════════════════════════════════════════════
// BTW-OVERZICHT
// ═══════════════════════════════════════════════
var BTW=0.21;
function setBtwFil(f,btn){btwFil=f;document.querySelectorAll('#pg-btw .fb').forEach(function(b){b.classList.remove('on')});btn.classList.add('on');renderBtw()}
function huidigKwartaal(w){var now=new Date(),kw=Math.floor(now.getMonth()/3)+1;return w.kwartaal===kw&&w.jaar===now.getFullYear()}
function renderBtw(){
  var all=getAll();
  var weken;
  if(btwFil==='kwartaal')weken=all.filter(huidigKwartaal);
  else if(btwFil==='ytd')weken=all.filter(function(w){return w.jaar===new Date().getFullYear()});
  else weken=all;
  var c=document.getElementById('btw-content');if(!c)return;
  if(!weken.length){c.innerHTML='<div class="card"><div class="empty">Geen data beschikbaar voor deze periode</div></div>';return}
  var omz=weken.reduce(function(a,w){return a+w.omz},0);
  var ink=weken.reduce(function(a,w){return a+w.ink},0);
  var btwOntvangen=omz*BTW;
  var btwBetaald=ink*BTW;
  var btwSaldo=btwOntvangen-btwBetaald;
  var html='';
  html+='<div class="kpi-grid">';
  html+='<div class="kpi"><div class="kpi-label">Omzet excl.</div><div class="kpi-value">'+fe(omz)+'</div><div class="kpi-sub">basis voor btw berekening</div></div>';
  html+='<div class="kpi"><div class="kpi-label">Btw ontvangen (21%)</div><div class="kpi-value" style="color:var(--green)">'+fe(btwOntvangen)+'</div><div class="kpi-sub">van klanten</div></div>';
  html+='<div class="kpi"><div class="kpi-label">Btw betaald (21%)</div><div class="kpi-value" style="color:var(--orange)">'+fe(btwBetaald)+'</div><div class="kpi-sub">bij inkoop</div></div>';
  html+='<div class="kpi"><div class="kpi-label">Te betalen OB</div><div class="kpi-value" style="color:'+(btwSaldo>=0?'var(--red)':'var(--green)')+'">'+fe(btwSaldo)+'</div><div class="kpi-sub">'+(btwSaldo>=0?'afdragen aan Belastingdienst':'terugvorderen')+'</div></div>';
  html+='</div>';
  html+='<div class="card"><div class="card-t">Btw-aangifte overzicht</div>';
  html+='<table style="width:100%;font-size:13px;border-collapse:collapse"><tbody>';
  var items=[
    {lbl:'1a. Leveringen/diensten belast 21%',v:omz,btw:btwOntvangen},
    {lbl:'5b. Voorbelasting (inkoop)',v:ink,btw:btwBetaald},
    {lbl:'Btw-saldo (te betalen)',v:null,btw:btwSaldo,bold:true}
  ];
  items.forEach(function(item){
    var col=item.bold?(btwSaldo>=0?'var(--red)':'var(--green)'):'inherit';
    html+='<tr style="border-top:1px solid var(--border2)"><td style="padding:9px 0;'+(item.bold?'font-weight:700':'')+'">'+esc(item.lbl)+'</td>';
    html+='<td style="text-align:right;padding:9px 0;color:var(--text2)">'+(item.v!==null?fe(item.v):'')+'</td>';
    html+='<td style="text-align:right;padding:9px 0;font-weight:600;color:'+col+'">'+fe(item.btw)+'</td></tr>';
  });
  html+='</tbody></table></div>';
  html+='<div class="card" style="background:var(--green-bg);border-color:var(--green-border)"><div class="card-t" style="color:var(--green)">✓ BTW gaat niet van je winst af</div><div style="font-size:13px;color:var(--text2);line-height:1.7">Alle bedragen in het platform zijn <strong>excl. btw</strong>. De winst op het Fiscaal-scherm is dus al het juiste nettobedrag — btw zit er nooit in. De BTW ontvang je van klanten en draag je af aan de Belastingdienst. Het raakt je winst niet.</div></div>';
  html+='<div class="card card-orange"><div class="card-t">⚠ Let op</div><div style="font-size:13px;color:var(--text2);line-height:1.7">Deze btw-schatting is gebaseerd op weekdata in het platform. <strong>Niet alle inkoop heeft 21% btw</strong> (denk aan EU-leveranciers of vrijgestelde goederen). Controleer de werkelijke btw op je facturen. Gebruik de Facturen-module voor exacte bedragen.</div></div>';
  c.innerHTML=html;
}

// ═══════════════════════════════════════════════
// FACTUREN
// ═══════════════════════════════════════════════
var factToday=new Date();factToday.setHours(0,0,0,0);
var factWeek7=new Date(factToday);factWeek7.setDate(factWeek7.getDate()+7);
function loadFact(){try{var r=localStorage.getItem('dpl_fact_v1');fRows=r?JSON.parse(r):FACT_INIT.map(function(x){return Object.assign({},x)})}catch(e){fRows=FACT_INIT.map(function(x){return Object.assign({},x)})}}
function saveFact(){try{localStorage.setItem('dpl_fact_v1',JSON.stringify(fRows))}catch(e){}}
function getFactStatus(f){if(f.st==='betaald')return'betaald';if(f.st==='incasso')return'incasso';if(!f.ver)return'open';var v=new Date(f.ver+'T00:00:00');if(v<factToday)return'vervallen';if(v<=factWeek7)return'week';return'open'}
function factBadge(f){var s=getFactStatus(f);var m={betaald:['b-green','✓ Betaald'],incasso:['b-blue','Auto-incasso'],vervallen:['b-red','Vervallen'],week:['b-orange','Vervalt deze week'],open:f.ver?['b-blue','Openstaand']:['b-gray','Verval onbekend']};var p=m[s]||['b-gray',s];return'<span class="badge '+p[0]+'">'+p[1]+'</span>'}
function factFilterOk(f){var s=getFactStatus(f);if(fFilter==='all')return true;if(fFilter==='betaald')return s==='betaald'||s==='incasso';if(fFilter==='vervallen')return s==='vervallen';if(fFilter==='week')return s==='week';if(fFilter==='open')return s!=='betaald';return true}
function factEuro(v){return v!=null?'€\u202f'+Number(v).toLocaleString('nl-NL',{minimumFractionDigits:2,maximumFractionDigits:2}):'<span style="color:var(--text3)">?</span>'}
function factDat(d){return d?new Date(d+'T00:00:00').toLocaleDateString('nl-NL',{day:'numeric',month:'short',year:'numeric'}):'<span style="color:var(--text3)">onbekend</span>'}
function setFFilter(f,btn){fFilter=f;document.querySelectorAll('#pg-fact .fb').forEach(function(b){b.classList.remove('on')});btn.classList.add('on');renderFact()}
function renderFact(){
  var list=fRows.filter(factFilterOk);var tot=0,ov=0,wk=0,paid=0;
  fRows.forEach(function(f){var s=getFactStatus(f),b=f.bed||0;if(s==='betaald'||s==='incasso')paid+=b;else{tot+=b;if(s==='vervallen')ov+=b;if(s==='week')wk+=b}});
  function mfmt(v){return v>0?'€\u202f'+Math.round(v).toLocaleString('nl-NL'):'—'}
  document.getElementById('fm1').textContent=mfmt(tot);document.getElementById('fm2').textContent=mfmt(ov);document.getElementById('fm3').textContent=mfmt(wk);document.getElementById('fm4').textContent=mfmt(paid);
  var tb=document.getElementById('ftbody'),em=document.getElementById('fempty');
  if(!list.length){tb.innerHTML='';em.style.display='';return}em.style.display='none';
  tb.innerHTML=list.map(function(f){var s=getFactStatus(f),isPaid=s==='betaald'||s==='incasso';
    return'<tr><td><strong>'+esc(f.lev)+'</strong>'+(f.not?'<div class="note-text">'+esc(f.not)+'</div>':'')+'</td>'
      +'<td><span class="mono">'+esc(f.nr)+'</span></td><td>'+factEuro(f.bed)+'</td><td>'+factDat(f.ver)+'</td><td>'+factBadge(f)+'</td>'
      +'<td style="text-align:right;white-space:nowrap"><div class="racts" style="opacity:1">'
      +(isPaid?'':'<button class="btn btn-xs btn-green" onclick="markFactPaid('+f.id+')">✓ Betaald</button>')
      +'<button class="btn btn-xs" onclick="editFact('+f.id+')">Bewerk</button>'
      +'<button class="btn btn-xs btn-red" onclick="delFact('+f.id+')">✕</button></div></td></tr>';
  }).join('');
}
function markFactPaid(id){var f=fRows.find(function(x){return x.id===id});if(f){f.st='betaald';saveFact();renderFact();showToast('Factuur gemarkeerd als betaald')}}
function delFact(id){if(!confirm('Factuur verwijderen?'))return;fRows=fRows.filter(function(x){return x.id!==id});saveFact();renderFact()}
function openFactModal(f){
  document.getElementById('fact-modal-title').textContent=f?'Factuur bewerken':'Factuur toevoegen';
  document.getElementById('feid').value=f?f.id:'';
  document.getElementById('fflev').value=f?f.lev:'';document.getElementById('ffnr').value=f?f.nr:'';
  document.getElementById('ffbed').value=(f&&f.bed!=null)?f.bed:'';
  document.getElementById('ffdat').value=f?(f.dat||''):new Date().toISOString().split('T')[0];
  document.getElementById('ffver').value=f?(f.ver||''):'';document.getElementById('ffnot').value=f?(f.not||''):'';
  document.getElementById('fact-modal').style.display='flex';
}
function closeFactModal(){document.getElementById('fact-modal').style.display='none'}
function editFact(id){openFactModal(fRows.find(function(f){return f.id===id}))}
function saveFactuur(){
  var eid=document.getElementById('feid').value;
  var obj={lev:document.getElementById('fflev').value.trim(),nr:document.getElementById('ffnr').value.trim(),bed:parseFloat(document.getElementById('ffbed').value)||null,dat:document.getElementById('ffdat').value||null,ver:document.getElementById('ffver').value||null,not:document.getElementById('ffnot').value.trim(),st:'open'};
  if(!obj.lev||!obj.nr){alert('Vul leverancier en factuurnummer in.');return}
  if(eid){var i=fRows.findIndex(function(f){return f.id==eid});if(i>-1){obj.id=fRows[i].id;obj.st=fRows[i].st;fRows[i]=obj}}
  else{obj.id=Date.now();fRows.push(obj)}
  saveFact();closeFactModal();renderFact();showToast(eid?'Factuur bijgewerkt':'Factuur toegevoegd');
}
function onFKey(v){try{localStorage.setItem('dpl_apikey',v)}catch(e){}document.getElementById('f_keystatus').textContent=v.startsWith('sk-ant-')?'✓ Opgeslagen':''}
function getFKey(){return document.getElementById('f_apikey').value||(localStorage.getItem('dpl_apikey')||'')}
try{var k=localStorage.getItem('dpl_apikey');if(k){document.getElementById('f_apikey').value=k;document.getElementById('f_keystatus').textContent='✓ Opgeslagen'}}catch(e){}
var fdz=document.getElementById('fdz');
fdz.addEventListener('dragover',function(e){e.preventDefault();fdz.classList.add('over')});
fdz.addEventListener('dragleave',function(){fdz.classList.remove('over')});
fdz.addEventListener('drop',function(e){e.preventDefault();fdz.classList.remove('over');handleFiles(e.dataTransfer.files)});
function handleFiles(files){var pdfs=Array.from(files).filter(function(f){return f.name.toLowerCase().endsWith('.pdf')});if(!pdfs.length){showToast('Alleen PDF-bestanden');return}pdfs.forEach(processPdf)}
async function extractPdfText(file){var buf=await file.arrayBuffer();var pdf=await pdfjsLib.getDocument({data:buf}).promise;var txt='';for(var i=1;i<=Math.min(pdf.numPages,3);i++){var pg=await pdf.getPage(i);var cc=await pg.getTextContent();txt+=cc.items.map(function(x){return x.str}).join(' ')+'\n'}return txt}
async function processPdf(file){
  if(fBusy){showToast('Even wachten...');return}
  var key=getFKey();if(!key||!key.startsWith('sk-ant-')){showToast('Vul een geldige Anthropic API key in bij Facturen');return}
  fBusy=true;fdz.classList.add('busy');document.getElementById('fdztitle').textContent=file.name+' verwerken...';
  try{
    var txt=await extractPdfText(file);
    var prompt='Je bent een factuur-parser. Extraheer:\n- leverancier\n- factuurnummer\n- bedrag: totaal EXCLUSIEF btw als getal\n- factuurdatum: YYYY-MM-DD\n- vervaldatum: YYYY-MM-DD\n\nAntwoord UITSLUITEND met JSON:\n{"leverancier":"...","factuurnummer":"...","bedrag":0.00,"factuurdatum":"YYYY-MM-DD","vervaldatum":"YYYY-MM-DD"}\n\nGebruik null als ontbreekt.\n\nFactuur:\n'+txt.slice(0,3000);
    var res=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json','x-api-key':key,'anthropic-version':'2023-06-01','anthropic-dangerous-direct-browser-access':'true'},body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:300,messages:[{role:'user',content:prompt}]})});
    if(!res.ok){var e=await res.json();throw new Error(e.error?.message||'API fout '+res.status)}
    var json=await res.json();var p=JSON.parse((json.content[0]?.text||'{}').replace(/```json|```/g,'').trim());
    var existing=fRows.find(function(f){return(p.factuurnummer&&f.nr===p.factuurnummer)||(p.leverancier&&f.lev===p.leverancier&&f.dat===p.factuurdatum)});
    if(existing){if(p.bedrag!=null)existing.bed=p.bedrag;if(p.vervaldatum)existing.ver=p.vervaldatum;if(p.factuurdatum)existing.dat=p.factuurdatum;saveFact();renderFact();showToast(existing.nr+' bijgewerkt vanuit PDF')}
    else{var nw={id:Date.now(),lev:p.leverancier||file.name.replace('.pdf',''),nr:p.factuurnummer||file.name.replace('.pdf',''),bed:p.bedrag||null,dat:p.factuurdatum||null,ver:p.vervaldatum||null,st:'open',not:''};fRows.push(nw);saveFact();renderFact();showToast(nw.nr+' toegevoegd — '+nw.lev)}
  }catch(e){showToast('Fout: '+e.message);console.error(e)}
  finally{fBusy=false;fdz.classList.remove('busy');document.getElementById('fdztitle').textContent="Sleep factuur-PDF's hierheen";document.getElementById('ffileinput').value=''}
}

// ═══════════════════════════════════════════════
// EXPORT / BACKUP
// ═══════════════════════════════════════════════
function dl(content,filename,type){var blob=new Blob([content],{type:type});var url=URL.createObjectURL(blob);var a=document.createElement('a');a.href=url;a.download=filename;document.body.appendChild(a);a.click();document.body.removeChild(a);URL.revokeObjectURL(url)}
function exportJSON(){
  var data={exported:new Date().toISOString(),settings:localStorage.getItem('pl_s'),clients:localStorage.getItem('pl_c'),veeke:localStorage.getItem('pl_veeke'),weekIndex:localStorage.getItem('pl_wi'),facturen:localStorage.getItem('dpl_fact_v1'),weeks:{}};
  var idx=JSON.parse(localStorage.getItem('pl_wi')||'[]');idx.forEach(function(k){data.weeks[k]=localStorage.getItem(k)});
  dl(JSON.stringify(data,null,2),'palletleverancier-backup-'+new Date().toISOString().slice(0,10)+'.json','application/json');
  showToast('JSON back-up gedownload');
}
function importData(input){
  var file=input.files[0];if(!file)return;
  var reader=new FileReader();
  reader.onload=function(e){
    try{var data=JSON.parse(e.target.result);
      if(!data.settings&&!data.clients&&!data.facturen){alert('Ongeldig bestand.');return}
      if(!confirm('Alle huidige data overschrijven?'))return;
      if(data.settings)localStorage.setItem('pl_s',data.settings);if(data.clients)localStorage.setItem('pl_c',data.clients);if(data.veeke)localStorage.setItem('pl_veeke',data.veeke);if(data.weekIndex)localStorage.setItem('pl_wi',data.weekIndex);if(data.facturen)localStorage.setItem('dpl_fact_v1',data.facturen);
      if(data.weeks)Object.keys(data.weeks).forEach(function(k){localStorage.setItem(k,data.weeks[k])});
      alert('✓ Data geïmporteerd. Pagina wordt herladen.');location.reload();
    }catch(err){alert('Fout bij importeren: '+err.message)}
  };reader.readAsText(file);input.value='';
}
function exportCSV(){
  var all=getAll();if(!all.length){showToast('Geen data om te exporteren');return}
  var h=['Week','Jaar','Datum','Omzet','Inkoop','Brutowinst','Transportkosten','Vaste lasten','Commissie','Totale kosten','Nettowinst','Marge %','Km','Pallets'];
  var rows=all.map(function(w){
    var approx=new Date(w.jaar,0,1+(w.week-1)*7);
    return[w.lbl,w.jaar,approx.toLocaleDateString('nl-NL'),w.omz.toFixed(2),w.ink.toFixed(2),(w.omz-w.ink).toFixed(2),w.tK.toFixed(2),w.vWk.toFixed(2),w.com.toFixed(2),w.totK.toFixed(2),w.netto.toFixed(2),w.marge.toFixed(1),w.totKm,w.nP];
  });
  var csv=[h].concat(rows).map(function(r){return r.join(';')}).join('\n');
  dl('\uFEFF'+csv,'palletleverancier-weken-'+new Date().toISOString().slice(0,10)+'.csv','text/csv;charset=utf-8');
  showToast('CSV gedownload ('+all.length+' weken)');
}
function exportPeriodeCSV(){
  var all=getAll();if(!all.length){showToast('Geen data');return}
  var maanden=groeperMaand(all),kwartalen=groeperKwartaal(all);
  var csv='\uFEFFMAANDEN\n';
  csv+='Maand;Weken;Pallets;Omzet;Inkoop;Nettowinst;Marge %\n';
  maanden.forEach(function(m){csv+=m.lbl+';'+m.weken.length+';'+m.nP+';'+m.omz.toFixed(2)+';'+m.ink.toFixed(2)+';'+m.netto.toFixed(2)+';'+(m.omz>0?m.netto/m.omz*100:0).toFixed(1)+'\n'});
  csv+='\nKWARTALEN\n';
  csv+='Kwartaal;Weken;Pallets;Omzet;Inkoop;Nettowinst;Marge %\n';
  kwartalen.forEach(function(k){csv+=k.lbl+';'+k.weken.length+';'+k.nP+';'+k.omz.toFixed(2)+';'+k.ink.toFixed(2)+';'+k.netto.toFixed(2)+';'+(k.omz>0?k.netto/k.omz*100:0).toFixed(1)+'\n'});
  dl(csv,'palletleverancier-periodes-'+new Date().toISOString().slice(0,10)+'.csv','text/csv;charset=utf-8');
  showToast('Periode CSV gedownload');
}
function exportExcel(){
  // Simple XLSX via data URI (HTML table method)
  var all=getAll();if(!all.length){showToast('Geen data');return}
  var html='<html xmlns:o="urn:schemas-microsoft-com:office:office" xmlns:x="urn:schemas-microsoft-com:office:excel"><head><meta charset="UTF-8"></head><body>';
  html+='<table><tr><th>Week</th><th>Jaar</th><th>Omzet</th><th>Inkoop</th><th>Brutowinst</th><th>Transportkosten</th><th>Vaste lasten</th><th>Commissie</th><th>Nettowinst</th><th>Marge %</th><th>Km</th><th>Pallets</th><th>Maand</th><th>Kwartaal</th></tr>';
  all.forEach(function(w){html+='<tr><td>'+w.lbl+'</td><td>'+w.jaar+'</td><td>'+w.omz.toFixed(2)+'</td><td>'+w.ink.toFixed(2)+'</td><td>'+(w.omz-w.ink).toFixed(2)+'</td><td>'+w.tK.toFixed(2)+'</td><td>'+w.vWk.toFixed(2)+'</td><td>'+w.com.toFixed(2)+'</td><td>'+w.netto.toFixed(2)+'</td><td>'+w.marge.toFixed(1)+'</td><td>'+w.totKm+'</td><td>'+w.nP+'</td><td>'+w.maandNaam+' '+w.jaar+'</td><td>Q'+w.kwartaal+'</td></tr>'});
  html+='</table></body></html>';
  dl(html,'palletleverancier-weken-'+new Date().toISOString().slice(0,10)+'.xls','application/vnd.ms-excel');
  showToast('Excel bestand gedownload');
}
function exportFactCSV(){
  if(!fRows.length){showToast('Geen facturen');return}
  var h=['Leverancier','Factuurnr.','Bedrag excl.','Factuurdatum','Vervaldatum','Status','Notitie'];
  var rows=fRows.map(function(f){return[f.lev,f.nr,f.bed!=null?f.bed.toFixed(2):'',f.dat||'',f.ver||'',getFactStatus(f),f.not||'']});
  var csv=[h].concat(rows).map(function(r){return r.map(function(v){return'"'+String(v).replace(/"/g,'""')+'"'}).join(';')}).join('\n');
  dl('\uFEFF'+csv,'facturen-'+new Date().toISOString().slice(0,10)+'.csv','text/csv;charset=utf-8');
  showToast('Facturen CSV gedownload');
}

// ═══════════════════════════════════════════════
// INIT
// ═══════════════════════════════════════════════
loadS();
loadFact();
renderK();
// Init dagblokken op basis van opgeslagen instelling
var initN=parseInt(GS().huur_dgn)||2;
for(var d=1;d<=initN;d++){days[d]=[];kmD[d]=0;dOpen[d]=true}
renderDagBlocks();
var now=new Date();sv('w_wk',wkNum(now));sv('w_jr',now.getFullYear());
updV();
</script>
</body>
</html>
