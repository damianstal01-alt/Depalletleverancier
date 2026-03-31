# Depalletleverancier
<!DOCTYPE html>
<html lang="nl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>De Palletleverancier — Winsttool</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#ffffff;--bg2:#f7f7f5;--bg3:#f0f0ec;
  --text:#1a1a1a;--text2:#666;--text3:#999;
  --border:#e0e0d8;--border2:#d0d0c8;
  --success:#1D9E75;--success-bg:#EAF3DE;
  --warning:#BA7517;--warning-bg:#FAEEDA;
  --danger:#E24B4A;--danger-bg:#FCEBEB;
  --info:#185FA5;--info-bg:#E6F1FB;
  --radius:8px;--radius-lg:12px;
}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;font-size:15px;color:var(--text);background:var(--bg3);min-height:100vh}
.app{display:flex;min-height:100vh}

/* Sidebar */
.sidebar{width:220px;flex-shrink:0;background:var(--bg);border-right:1px solid var(--border);padding:1.5rem 0;position:fixed;top:0;left:0;height:100vh;overflow-y:auto;z-index:100}
.sidebar-logo{padding:0 1.25rem 0.25rem;font-size:11px;font-weight:600;color:var(--text2);text-transform:uppercase;letter-spacing:0.08em}
.sidebar-name{padding:0 1.25rem 1.5rem;font-size:16px;font-weight:600;color:var(--text)}
.nav-item{display:flex;align-items:center;gap:10px;padding:10px 1.25rem;font-size:14px;cursor:pointer;color:var(--text2);border-left:3px solid transparent;transition:all .15s}
.nav-item:hover{color:var(--text);background:var(--bg2)}
.nav-item.active{color:var(--text);border-left-color:var(--text);background:var(--bg2);font-weight:500}
.nav-icon{width:18px;text-align:center;font-size:16px}
.sidebar-section{padding:1.5rem 1.25rem 0.5rem;font-size:11px;font-weight:600;color:var(--text3);text-transform:uppercase;letter-spacing:0.06em}

/* Main */
.main{flex:1;margin-left:220px;padding:2rem;max-width:1100px}
.page{display:none}.page.active{display:block}
.page-title{font-size:22px;font-weight:600;margin-bottom:0.25rem}
.page-sub{font-size:13px;color:var(--text2);margin-bottom:2rem}

/* Cards & sections */
.card{background:var(--bg);border:1px solid var(--border);border-radius:var(--radius-lg);padding:1.25rem;margin-bottom:1rem}
.card-title{font-size:12px;font-weight:600;color:var(--text2);text-transform:uppercase;letter-spacing:0.05em;margin-bottom:1rem}
.veeke-card{background:#E1F5EE;border:1px solid #9FE1CB;border-radius:var(--radius-lg);padding:1.25rem;margin-bottom:1rem}
.scania-card{background:#E1F5EE;border:1px solid #9FE1CB;border-radius:var(--radius-lg);padding:1.25rem;margin-bottom:1rem}

/* Fields */
.field{display:flex;flex-direction:column;gap:5px}
.field label{font-size:12px;color:var(--text2);font-weight:500}
.field input,.field select,.field textarea{padding:8px 10px;border:1px solid var(--border2);border-radius:var(--radius);font-size:14px;background:var(--bg);color:var(--text);width:100%;font-family:inherit}
.field input:focus,.field select:focus,.field textarea:focus{outline:none;border-color:#888}
.field textarea{resize:vertical;min-height:64px}
.field-hint{font-size:11px;color:var(--text3)}
.field input.big{font-size:22px;font-weight:500;padding:10px 14px;text-align:center;height:52px}

/* Grids */
.g2{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.g3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px}
.g4{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:12px}
.g2-1{display:grid;grid-template-columns:2fr 1fr;gap:12px}
.g1-2{display:grid;grid-template-columns:1fr 2fr;gap:12px}
@media(max-width:700px){.g2,.g3,.g4,.g2-1,.g1-2{grid-template-columns:1fr}}

/* Buttons */
.btn{padding:8px 16px;border:1px solid var(--border2);border-radius:var(--radius);font-size:13px;cursor:pointer;background:var(--bg);color:var(--text);transition:all .15s;white-space:nowrap;font-family:inherit}
.btn:hover{background:var(--bg2)}
.btn-primary{background:var(--text);color:var(--bg);border-color:var(--text)}
.btn-primary:hover{opacity:.85}
.btn-danger{color:var(--danger);border-color:#f0c0c0}
.btn-danger:hover{background:var(--danger-bg)}
.btn-sm{padding:5px 10px;font-size:12px}
.btn-icon{width:32px;height:32px;padding:0;display:flex;align-items:center;justify-content:center;font-size:14px}
.btn-green{border-color:#9FE1CB;color:#085041}
.btn-green:hover{background:#E1F5EE}

/* Divider */
.divider{height:1px;background:var(--border);margin:12px 0}

/* Badges */
.badge{font-size:11px;padding:2px 8px;border-radius:var(--radius);font-weight:500}
.badge-info{background:var(--info-bg);color:var(--info)}
.badge-success{background:var(--success-bg);color:var(--success)}
.badge-warning{background:var(--warning-bg);color:var(--warning)}
.badge-danger{background:var(--danger-bg);color:var(--danger)}
.badge-repair{background:#9FE1CB;color:#085041}
.badge-km{background:var(--info-bg);color:var(--info);font-weight:600}

/* KPI cards */
.kpi-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:1.25rem}
.kpi{background:var(--bg2);border-radius:var(--radius);padding:14px 16px}
.kpi-label{font-size:11px;color:var(--text2);margin-bottom:6px;text-transform:uppercase;letter-spacing:0.04em}
.kpi-val{font-size:24px;font-weight:600}
.kpi-sub{font-size:12px;color:var(--text2);margin-top:3px}
@media(max-width:700px){.kpi-grid{grid-template-columns:1fr 1fr}}

/* Stoplicht */
.stoplicht{display:flex;align-items:center;gap:10px;padding:12px 16px;border-radius:var(--radius);font-size:14px;font-weight:500;margin-bottom:1rem}
.sl-groen{background:var(--success-bg);color:var(--success)}
.sl-oranje{background:var(--warning-bg);color:var(--warning)}
.sl-rood{background:var(--danger-bg);color:var(--danger)}
.sl-grijs{background:var(--bg2);color:var(--text2)}

/* Route */
.route-visual{display:flex;align-items:center;gap:6px;flex-wrap:wrap;font-size:12px;padding:10px 12px;background:var(--bg2);border-radius:var(--radius);border:1px solid var(--border);margin-bottom:10px}
.rn-home{padding:4px 10px;border-radius:var(--radius);font-size:12px;font-weight:600;background:var(--info-bg);color:var(--info)}
.rn-stop{padding:4px 10px;border-radius:var(--radius);font-size:12px;background:var(--bg);border:1px solid var(--border2);color:var(--text)}
.rn-repair{padding:4px 10px;border-radius:var(--radius);font-size:12px;background:#9FE1CB;color:#085041;font-weight:500}
.rn-arrow{color:var(--text3)}

/* Day block */
.day-block{border:1px solid var(--border);border-radius:var(--radius-lg);margin-bottom:10px;overflow:hidden;background:var(--bg)}
.day-header{display:flex;align-items:center;justify-content:space-between;padding:12px 16px;background:var(--bg2);cursor:pointer;user-select:none}
.day-header-left{display:flex;align-items:center;gap:10px;font-size:15px;font-weight:600}
.day-body{padding:14px 16px}

/* Stop cards */
.stop-card{border:1px solid var(--border);border-radius:var(--radius);padding:12px 14px;margin-bottom:10px}
.stop-levering{background:var(--bg2)}
.stop-reparatie{background:#E1F5EE;border-color:#9FE1CB}
.stop-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:10px}

/* Kaan toggle */
.kaan-toggle{display:flex;align-items:center;gap:6px;font-size:13px;color:var(--text2);cursor:pointer}
.kaan-toggle input{width:15px;height:15px;cursor:pointer}

/* Cost breakdown */
.cost-row{display:flex;justify-content:space-between;padding:6px 0;border-bottom:1px solid var(--border);font-size:13px}
.cost-row:last-child{border-bottom:none;font-weight:600;padding-top:8px;font-size:14px}

/* Break-even */
.be-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:1rem}
.be-card{background:var(--bg2);border-radius:var(--radius);padding:12px 14px;text-align:center}
.be-val{font-size:28px;font-weight:600;margin-bottom:2px}
.be-label{font-size:11px;color:var(--text2)}

/* Client table */
.data-table{width:100%;border-collapse:collapse;font-size:13px}
.data-table th{text-align:left;padding:8px 10px;font-size:11px;font-weight:600;color:var(--text2);text-transform:uppercase;letter-spacing:0.04em;border-bottom:1px solid var(--border)}
.data-table td{padding:10px 10px;border-bottom:1px solid var(--border);vertical-align:middle}
.data-table tr:last-child td{border-bottom:none}
.data-table tr:hover td{background:var(--bg2)}
.row-actions{display:flex;gap:6px;opacity:0;transition:opacity .15s}
.data-table tr:hover .row-actions{opacity:1}

/* Pallet row */
.pallet-row{display:grid;grid-template-columns:1.5fr 1fr 1fr 1fr 36px;gap:8px;align-items:end;padding:8px 10px;border:1px solid var(--border);border-radius:var(--radius);margin-bottom:8px;background:var(--bg2)}

/* Initials */
.initials{width:32px;height:32px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:12px;font-weight:600;flex-shrink:0}

/* Modal */
.modal-bg{position:fixed;inset:0;background:rgba(0,0,0,0.35);display:flex;align-items:center;justify-content:center;z-index:200}
.modal{background:var(--bg);border:1px solid var(--border2);border-radius:var(--radius-lg);padding:1.75rem;width:540px;max-width:95vw;max-height:90vh;overflow-y:auto}
.modal-title{font-size:17px;font-weight:600;margin-bottom:1.25rem}
.modal-footer{display:flex;justify-content:flex-end;gap:8px;margin-top:1.25rem;padding-top:1rem;border-top:1px solid var(--border)}
.section-label{font-size:11px;font-weight:600;color:var(--text2);text-transform:uppercase;letter-spacing:0.05em;margin:1rem 0 8px}

/* Filter bar */
.filter-bar{display:flex;gap:8px;margin-bottom:1.25rem;flex-wrap:wrap}
.filter-btn{padding:6px 14px;border:1px solid var(--border2);border-radius:var(--radius);font-size:13px;cursor:pointer;background:var(--bg);color:var(--text2);transition:all .15s}
.filter-btn:hover{background:var(--bg2)}
.filter-btn.active{background:var(--text);color:var(--bg);border-color:var(--text)}

/* Save bar */
.save-bar{display:flex;align-items:center;justify-content:space-between;margin-top:1.5rem;padding-top:1.25rem;border-top:1px solid var(--border)}
.save-msg{font-size:13px;color:var(--success)}

/* Misc */
.spin{display:inline-block;animation:spin 1s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
.dot{width:8px;height:8px;border-radius:50%;display:inline-block;margin-right:4px}
.dot-g{background:var(--success)}.dot-o{background:var(--warning)}.dot-r{background:var(--danger)}
.empty{text-align:center;padding:3rem 1rem;color:var(--text2);font-size:14px}
.api-hint{font-size:12px;color:var(--text2);background:var(--bg2);border-radius:var(--radius);padding:8px 12px;margin-bottom:10px}
.marge-pill{font-size:11px;padding:2px 7px;border-radius:4px;margin-top:2px;display:inline-block}
.mb{margin-bottom:10px}
.veeke-metric-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:1rem}
.veeke-metric{background:rgba(255,255,255,0.6);border-radius:var(--radius);padding:10px 14px}
.veeke-metric-label{font-size:11px;color:#0F6E56;margin-bottom:4px}
.veeke-metric-val{font-size:22px;font-weight:600;color:#04342C}
</style>
</head>
<body>
<div class="app">

<!-- SIDEBAR -->
<div class="sidebar">
  <div class="sidebar-logo">De Palletleverancier</div>
  <div class="sidebar-name">Winsttool</div>
  <div class="nav-item active" onclick="nav('instellingen')"><span class="nav-icon">⚙️</span> Instellingen</div>
  <div class="nav-item" onclick="nav('klanten')"><span class="nav-icon">👥</span> Klanten</div>
  <div class="nav-item" onclick="nav('weekinvoer')"><span class="nav-icon">📋</span> Weekinvoer</div>
  <div class="nav-item" onclick="nav('dashboard')"><span class="nav-icon">📊</span> Dashboard</div>
</div>

<!-- MAIN -->
<div class="main">

<!-- ===== PAGE: INSTELLINGEN ===== -->
<div id="page-instellingen" class="page active">
  <div class="page-title">Instellingen</div>
  <div class="page-sub">Eenmalig invullen — vormt de basis van alle berekeningen</div>

  <div class="card">
    <div class="card-title">Bedrijf & locatie</div>
    <div class="g2 mb">
      <div class="field" style="grid-column:1/-1"><label>Startlocatie (vaste vertrekpunt)</label><input type="text" id="s_startloc" placeholder="Havenkant 2, 4781AA Moerdijk" /></div>
    </div>
    <div class="g2">
      <div class="field"><label>Winstdoel per maand (EUR excl. btw)</label><input type="number" id="s_winstdoel" placeholder="4000" min="0" /></div>
      <div class="field"><label>Actuele dieselprijs (EUR/liter)</label><input type="number" id="s_diesel" step="0.01" placeholder="1.75" /><span class="field-hint" id="diesel-datum"></span></div>
    </div>
  </div>

  <div class="card">
    <div class="card-title">Huurvrachtwagen</div>
    <div class="g4">
      <div class="field"><label>Dagtarief (EUR excl. btw)</label><input type="number" id="s_huur_dag" placeholder="230" oninput="calcVaste()" /></div>
      <div class="field"><label>Verbruik (liter/100 km)</label><input type="number" id="s_huur_verbruik" placeholder="28" step="0.1" /></div>
      <div class="field"><label>Rijdagen/week (standaard)</label><input type="number" id="s_huur_dagen" placeholder="2" min="1" max="7" oninput="calcVaste()" /></div>
      <div class="field"><label>Wegenbelasting (EUR/km)</label><input type="number" id="s_wegenbelasting" placeholder="0.20" step="0.01" /></div>
    </div>
  </div>

  <div class="card">
    <div class="card-title">Scania (eigen vrachtwagen)</div>
    <div class="g4">
      <div class="field"><label>Activatiedatum</label><input type="date" id="s_scania_datum" oninput="calcVaste()" /></div>
      <div class="field"><label>Maandlease (EUR excl. btw)</label><input type="number" id="s_scania_lease" placeholder="1850" oninput="calcVaste()" /></div>
      <div class="field"><label>Onderhoudscontract (EUR excl./mnd)</label><input type="number" id="s_scania_onderhoud" placeholder="200" oninput="calcVaste()" /></div>
      <div class="field"><label>Verbruik (liter/100 km)</label><input type="number" id="s_scania_verbruik" placeholder="28" step="0.1" /></div>
    </div>
    <div id="scania-preview" class="api-hint" style="margin-top:10px"></div>
  </div>

  <div class="card">
    <div class="card-title">Trailer (financial lease)</div>
    <div class="g4">
      <div class="field"><label>Maandlease (EUR incl. btw)</label><input type="number" id="s_trailer_lease" placeholder="500" oninput="calcVaste()" /></div>
    </div>
  </div>

  <div class="card">
    <div class="card-title">Overige vaste lasten per maand</div>
    <div class="g3 mb">
      <div class="field"><label>Verzekeringen (EUR/mnd)</label><input type="number" id="s_verzekering" placeholder="500" oninput="calcVaste()" /></div>
      <div class="field"><label>Administratiekosten (EUR incl. btw/mnd)</label><input type="number" id="s_admin" placeholder="600" oninput="calcVaste()" /></div>
      <div class="field"><label>Overig (EUR/mnd)</label><input type="number" id="s_overig_lasten" placeholder="0" oninput="calcVaste()" /></div>
    </div>
    <div class="divider"></div>
    <div style="font-size:13px;color:var(--text2)">Vaste lasten per week (excl. transport): <strong id="vaste-per-week" style="color:var(--text)">—</strong></div>
  </div>

  <div class="card">
    <div class="card-title">Commissie Kaan</div>
    <div class="g4">
      <div class="field"><label>Commissie per vracht (EUR excl. btw)</label><input type="number" id="s_commissie" placeholder="100" /></div>
    </div>
  </div>

  <div class="card">
    <div class="card-title">Pallettypen & reparatie</div>
    <div style="display:grid;grid-template-columns:1.5fr 1fr 1fr 1fr 36px;gap:8px;padding:0 10px;margin-bottom:8px">
      <span style="font-size:11px;color:var(--text2);font-weight:600">Naam</span>
      <span style="font-size:11px;color:var(--text2);font-weight:600">Inkoop (€ excl.)</span>
      <span style="font-size:11px;color:var(--text2);font-weight:600">Reparatie (€ excl.)</span>
      <span style="font-size:11px;color:var(--text2);font-weight:600">Verkoop (€ excl.)</span>
      <span></span>
    </div>
    <div id="pallet-rows"></div>
    <button class="btn btn-sm" onclick="addPallet()" style="margin-top:8px">+ Pallettype toevoegen</button>
  </div>

  <div class="save-bar">
    <span class="save-msg" id="s-save-msg" style="display:none">✓ Instellingen opgeslagen</span>
    <div style="display:flex;gap:8px;margin-left:auto">
      <button class="btn" onclick="loadDefaults()">Standaardwaarden</button>
      <button class="btn btn-primary" onclick="saveSettings()">Opslaan</button>
    </div>
  </div>
</div>

<!-- ===== PAGE: KLANTEN ===== -->
<div id="page-klanten" class="page">
  <div class="page-title">Klantenbeheer</div>
  <div class="page-sub">Adresgegevens worden gebruikt voor automatische routeberekening</div>
  <div style="display:flex;gap:10px;margin-bottom:1.25rem;align-items:center">
    <input type="text" id="klant-search" placeholder="Zoek op naam, bedrijf of plaats..." oninput="renderKlanten()" style="flex:1;padding:8px 12px;border:1px solid var(--border2);border-radius:var(--radius);font-size:14px;background:var(--bg)" />
    <span style="font-size:12px;color:var(--text2);white-space:nowrap" id="klant-count"></span>
    <button class="btn btn-primary" onclick="openKlantModal()">+ Klant toevoegen</button>
  </div>
  <div class="card" style="padding:0;overflow:hidden">
    <table class="data-table">
      <thead><tr>
        <th style="width:36px"></th>
        <th>Bedrijfsnaam</th>
        <th>Contactpersoon</th>
        <th>Adres</th>
        <th>Standaard pallet</th>
        <th>Notities</th>
        <th style="width:90px"></th>
      </tr></thead>
      <tbody id="klant-tbody"></tbody>
    </table>
    <div class="empty" id="klant-empty" style="display:none">Nog geen klanten — klik op <strong>+ Klant toevoegen</strong></div>
  </div>
</div>

<!-- ===== PAGE: WEEKINVOER ===== -->
<div id="page-weekinvoer" class="page">
  <div class="page-title">Weekinvoer</div>
  <div class="page-sub">Route: De Palletleverancier BV → stops → terug naar De Palletleverancier BV</div>

  <div class="card">
    <div class="g3 mb">
      <div class="field"><label>Weeknummer</label><input type="number" id="w_week" min="1" max="53" /></div>
      <div class="field"><label>Jaar</label><input type="number" id="w_jaar" /></div>
      <div class="field"><label>Dieselprijs (EUR/liter)</label><input type="number" id="w_diesel" step="0.01" placeholder="1.75" oninput="herbereken()" /></div>
    </div>
    <div class="field"><label>OpenRouteService API key</label><input type="text" id="w_apikey" placeholder="Plak hier je ORS API key..." oninput="localStorage.setItem('pl_ors_key',this.value)" /></div>
    <div class="api-hint" style="margin-top:8px">Gratis account via openrouteservice.org → Dashboard → API key kopiëren. Wordt lokaal opgeslagen.</div>
  </div>

  <!-- Veeke -->
  <div class="veeke-card">
    <div style="font-size:14px;font-weight:600;color:#085041;margin-bottom:1rem">🔧 Veeke Palletreparatie — Voorraad</div>
    <div class="veeke-metric-grid">
      <div class="veeke-metric"><div class="veeke-metric-label">Huidig bij Veeke</div><div class="veeke-metric-val" id="veeke-huidig">0</div></div>
      <div class="veeke-metric"><div class="veeke-metric-label">Gebracht deze week</div><div class="veeke-metric-val" id="veeke-gebracht">0</div></div>
      <div class="veeke-metric"><div class="veeke-metric-label">Opgehaald deze week</div><div class="veeke-metric-val" id="veeke-opgehaald">0</div></div>
    </div>
    <div class="g2">
      <div class="field"><label style="color:#0F6E56">Beginstand deze week (pallets bij Veeke)</label><input type="number" id="veeke-beginstand" min="0" placeholder="0" oninput="updateVeeke()" style="background:rgba(255,255,255,0.7)" /></div>
      <div class="field"><label style="color:#0F6E56">Reparatietarief Veeke (EUR excl./pallet)</label><input type="number" id="veeke-tarief" step="0.01" placeholder="3.25" oninput="herbereken()" style="background:rgba(255,255,255,0.7)" /></div>
    </div>
    <div id="veeke-log" style="font-size:12px;color:#085041;margin-top:8px"></div>
  </div>

  <!-- Dag 1 -->
  <div class="day-block">
    <div class="day-header" onclick="toggleDay(1)">
      <div class="day-header-left"><span>Dag 1</span><span class="badge badge-km" id="km-dag1">0 km</span></div>
      <div style="display:flex;align-items:center;gap:8px">
        <button class="btn btn-sm" onclick="event.stopPropagation();berekenRoute(1)">📍 Bereken route</button>
        <span id="chev1">▾</span>
      </div>
    </div>
    <div class="day-body" id="day-body-1">
      <div id="rv1" class="route-visual" style="display:none"></div>
      <div id="stops-1"></div>
      <div style="display:flex;gap:8px;margin-top:6px">
        <button class="btn btn-sm" onclick="addStop(1,'levering')">+ Levering</button>
        <button class="btn btn-sm btn-green" onclick="addStop(1,'reparatie')">🔧 Veeke stop</button>
      </div>
      <div id="rr1" style="display:none;margin-top:10px"></div>
    </div>
  </div>

  <!-- Dag 2 -->
  <div class="day-block">
    <div class="day-header" onclick="toggleDay(2)">
      <div class="day-header-left"><span>Dag 2</span><span class="badge badge-km" id="km-dag2">0 km</span></div>
      <div style="display:flex;align-items:center;gap:8px">
        <button class="btn btn-sm" onclick="event.stopPropagation();berekenRoute(2)">📍 Bereken route</button>
        <span id="chev2">▾</span>
      </div>
    </div>
    <div class="day-body" id="day-body-2">
      <div id="rv2" class="route-visual" style="display:none"></div>
      <div id="stops-2"></div>
      <div style="display:flex;gap:8px;margin-top:6px">
        <button class="btn btn-sm" onclick="addStop(2,'levering')">+ Levering</button>
        <button class="btn btn-sm btn-green" onclick="addStop(2,'reparatie')">🔧 Veeke stop</button>
      </div>
      <div id="rr2" style="display:none;margin-top:10px"></div>
    </div>
  </div>

  <!-- Weekoverzicht -->
  <div class="card">
    <div class="card-title">Weekoverzicht</div>
    <div class="kpi-grid">
      <div class="kpi"><div class="kpi-label">Totale omzet</div><div class="kpi-val" id="sum-omzet">€0</div></div>
      <div class="kpi"><div class="kpi-label">Inkoopkosten</div><div class="kpi-val" id="sum-inkoop">€0</div></div>
      <div class="kpi"><div class="kpi-label">Alle kosten</div><div class="kpi-val" id="sum-kosten">€0</div></div>
      <div class="kpi"><div class="kpi-label">Nettowinst</div><div class="kpi-val" id="sum-winst">€0</div></div>
    </div>
    <div class="stoplicht sl-grijs" id="w-stoplicht">Voer leveringen in om resultaat te zien</div>
    <div class="divider"></div>
    <div class="card-title">Kostenverdeling</div>
    <div id="w-cost-breakdown"></div>
  </div>

  <div style="display:flex;justify-content:flex-end;gap:8px">
    <button class="btn" onclick="nieuweWeek()">Nieuwe week</button>
    <button class="btn btn-primary" onclick="slaWeekOp()">Week opslaan</button>
  </div>
  <div id="w-save-msg" style="font-size:13px;color:var(--success);text-align:right;margin-top:6px;display:none">✓ Week opgeslagen</div>
</div>

<!-- ===== PAGE: DASHBOARD ===== -->
<div id="page-dashboard" class="page">
  <div class="page-title">Dashboard</div>
  <div class="page-sub">Overzicht van je winst, kosten en trends</div>

  <div class="filter-bar">
    <button class="filter-btn active" onclick="setFilter('2w',this)">Laatste 2 weken</button>
    <button class="filter-btn" onclick="setFilter('4w',this)">Laatste 4 weken</button>
    <button class="filter-btn" onclick="setFilter('8w',this)">Laatste 8 weken</button>
    <button class="filter-btn" onclick="setFilter('12w',this)">Laatste 12 weken</button>
    <button class="filter-btn" onclick="setFilter('jaar',this)">Dit jaar</button>
  </div>

  <div class="kpi-grid">
    <div class="kpi"><div class="kpi-label">Totale omzet</div><div class="kpi-val" id="kpi-omzet">—</div><div class="kpi-sub" id="kpi-omzet-sub"></div></div>
    <div class="kpi"><div class="kpi-label">Nettowinst</div><div class="kpi-val" id="kpi-winst">—</div><div class="kpi-sub" id="kpi-winst-sub"></div></div>
    <div class="kpi"><div class="kpi-label">Gemiddelde marge</div><div class="kpi-val" id="kpi-marge">—</div><div class="kpi-sub" id="kpi-marge-sub"></div></div>
    <div class="kpi"><div class="kpi-label">Totaal gereden km</div><div class="kpi-val" id="kpi-km">—</div><div class="kpi-sub" id="kpi-km-sub"></div></div>
  </div>

  <div class="stoplicht sl-grijs" id="d-stoplicht">Nog geen weekdata — sla eerst een week op via Weekinvoer</div>

  <div class="card">
    <div class="card-title">Break-even & doelstelling</div>
    <div class="be-grid">
      <div class="be-card"><div class="be-val" id="be-pallets" style="color:var(--danger)">—</div><div class="be-label">Pallets voor break-even / week</div></div>
      <div class="be-card"><div class="be-val" id="be-doel" style="color:var(--success)">—</div><div class="be-label">Pallets voor winstdoel / week</div></div>
      <div class="be-card"><div class="be-val" id="be-verschil">—</div><div class="be-label">Gem. pallets boven/onder doel</div></div>
    </div>
    <div style="font-size:12px;color:var(--text2)" id="be-toelichting"></div>
  </div>

  <div class="card"><div class="card-title">Omzet vs kosten per week</div><div><canvas id="cv-bar" height="160"></canvas></div></div>
  <div class="card"><div class="card-title">Nettomarge % per week</div><div><canvas id="cv-marge" height="130"></canvas></div></div>

  <div class="g2" style="margin-bottom:1rem">
    <div class="card" style="margin-bottom:0"><div class="card-title">Kostenverdeling (periode)</div><canvas id="cv-donut" height="200"></canvas></div>
    <div class="card" style="margin-bottom:0"><div class="card-title">Nettowinst per maand</div><canvas id="cv-maand" height="200"></canvas></div>
  </div>

  <div class="scania-card" id="d-scania" style="display:none">
    <div style="font-size:14px;font-weight:600;color:#085041;margin-bottom:0.75rem">🚛 Scania overgangsmodule</div>
    <div id="d-scania-inhoud" style="font-size:13px;color:#085041;line-height:1.9"></div>
  </div>

  <div class="card">
    <div class="card-title">Weken detail</div>
    <div id="d-week-tabel"></div>
  </div>
</div>

</div><!-- /main -->
</div><!-- /app -->

<!-- KLANT MODAL -->
<div class="modal-bg" id="klant-modal" style="display:none" onclick="if(event.target===this)closeKlantModal()">
  <div class="modal">
    <div class="modal-title" id="km-title">Klant toevoegen</div>
    <div class="section-label">Bedrijfsgegevens</div>
    <div class="g2 mb"><div class="field"><label>Bedrijfsnaam *</label><input type="text" id="kf_bedrijf" /></div><div class="field"><label>Contactpersoon</label><input type="text" id="kf_contact" /></div></div>
    <div class="g2 mb"><div class="field"><label>Telefoon</label><input type="tel" id="kf_tel" /></div><div class="field"><label>E-mail</label><input type="email" id="kf_email" /></div></div>
    <div class="section-label">Afleveradres (voor routeberekening)</div>
    <div class="field mb"><label>Straat + huisnummer *</label><input type="text" id="kf_straat" /></div>
    <div style="display:grid;grid-template-columns:2fr 1fr 1fr;gap:10px" class="mb">
      <div class="field"><label>Plaats *</label><input type="text" id="kf_plaats" /></div>
      <div class="field"><label>Postcode</label><input type="text" id="kf_postcode" /></div>
      <div class="field"><label>Land</label><select id="kf_land"><option value="NL">Nederland</option><option value="BE">België</option><option value="DE">Duitsland</option><option value="FR">Frankrijk</option></select></div>
    </div>
    <div class="section-label">Voorkeursinstellingen</div>
    <div class="g2 mb"><div class="field"><label>Standaard pallettype</label><select id="kf_pallet"></select></div><div class="field"><label>Standaard aantal</label><input type="number" id="kf_aantal" min="0" /></div></div>
    <div class="field mb"><label>Notities</label><textarea id="kf_notities" placeholder="Tijdvenster, rijinstructies, bijzonderheden..."></textarea></div>
    <div id="kf_error" style="font-size:13px;color:var(--danger);display:none;margin-top:6px"></div>
    <div class="modal-footer"><button class="btn" onclick="closeKlantModal()">Annuleren</button><button class="btn btn-primary" onclick="saveKlant()">Opslaan</button></div>
  </div>
</div>

<script>
// ========================
// STATE
// ========================
let wDays={1:[],2:[]};
let wKmDag={1:0,2:0};
let dayOpen={1:true,2:true};
let pallets=[];
let editKlantId=null;
let dFilter='2w';
let dCharts={};

// ========================
// NAV
// ========================
function nav(p){
  document.querySelectorAll('.page').forEach(el=>el.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(el=>el.classList.remove('active'));
  document.getElementById('page-'+p).classList.add('active');
  const pages=['instellingen','klanten','weekinvoer','dashboard'];
  document.querySelectorAll('.nav-item').forEach((el,i)=>{if(pages[i]===p)el.classList.add('active');});
  if(p==='dashboard')renderDashboard();
  if(p==='weekinvoer')initWeek();
}

// ========================
// HELPERS
// ========================
function S(){return JSON.parse(localStorage.getItem('pl_settings')||'{}');}
function C(){return JSON.parse(localStorage.getItem('pl_clients')||'[]');}
function saveC(a){localStorage.setItem('pl_clients',JSON.stringify(a));}
function getPallets(){return S().pallets||[{naam:'EPAL',inkoop:2.5,reparatie:3.25,reparatie_aan:true,verkoop:7.5}];}
function getDepot(){return S().startloc||'Havenkant 2, 4781AA Moerdijk';}
function kp(naam){const p=getPallets().find(x=>x.naam===naam);if(!p)return 0;return p.inkoop+(p.reparatie_aan?(p.reparatie||0):0);}
function gv(id){return document.getElementById(id)?.value||'';}
function sv(id,v){const el=document.getElementById(id);if(el)el.value=v;}
function wn(d){const date=new Date(Date.UTC(d.getFullYear(),d.getMonth(),d.getDate()));date.setUTCDate(date.getUTCDate()+4-(date.getUTCDay()||7));return Math.ceil((((date-new Date(Date.UTC(date.getUTCFullYear(),0,1)))/86400000)+1)/7);}
const COLORS=[{bg:'#E6F1FB',text:'#0C447C'},{bg:'#EAF3DE',text:'#27500A'},{bg:'#FAEEDA',text:'#633806'},{bg:'#FAECE7',text:'#712B13'},{bg:'#E1F5EE',text:'#085041'},{bg:'#EEEDFE',text:'#3C3489'},{bg:'#FBEAF0',text:'#72243E'}];
function colorFor(n){let h=0;for(let c of n)h=(h*31+c.charCodeAt(0))&0xffff;return COLORS[h%COLORS.length];}
function initials(n){const w=n.trim().split(/\s+/);return w.length>=2?(w[0][0]+w[w.length-1][0]).toUpperCase():n.slice(0,2).toUpperCase();}

// ========================
// INSTELLINGEN
// ========================
const DEFAULTS={startloc:'Havenkant 2, 4781AA Moerdijk',winstdoel:4000,diesel:1.75,huur_dag:230,huur_verbruik:28,huur_dagen:2,wegenbelasting:0.20,scania_datum:'',scania_lease:1850,scania_onderhoud:200,scania_verbruik:28,trailer_lease:500,verzekering:500,admin:600,overig_lasten:0,commissie:100,
pallets:[
  {naam:'EPAL (De Zwaluw)',inkoop:2.50,reparatie:3.25,reparatie_aan:true,verkoop:7.50},
  {naam:'Blokpallet (Kompak)',inkoop:0.60,reparatie:0,reparatie_aan:false,verkoop:3.75},
  {naam:'80/120 (Kompak)',inkoop:0.60,reparatie:0,reparatie_aan:false,verkoop:3.25},
  {naam:'CP3 (Ter Slaa)',inkoop:8.00,reparatie:0,reparatie_aan:false,verkoop:10.50},
  {naam:'102/120 (Modon)',inkoop:5.00,reparatie:0,reparatie_aan:false,verkoop:7.50},
]};

const S_FIELDS=['startloc','winstdoel','diesel','huur_dag','huur_verbruik','huur_dagen','wegenbelasting','scania_datum','scania_lease','scania_onderhoud','scania_verbruik','trailer_lease','verzekering','admin','overig_lasten','commissie'];

function loadSettings(){
  const raw=localStorage.getItem('pl_settings');
  const s=raw?JSON.parse(raw):DEFAULTS;
  S_FIELDS.forEach(f=>{if(s[f]!==undefined&&s[f]!=='')sv('s_'+f,s[f]);});
  pallets=s.pallets?JSON.parse(JSON.stringify(s.pallets)):JSON.parse(JSON.stringify(DEFAULTS.pallets));
  pallets.forEach(p=>{if(p.reparatie===undefined)p.reparatie=0;if(p.reparatie_aan===undefined)p.reparatie_aan=false;});
  if(s.diesel_datum)document.getElementById('diesel-datum').textContent='Laatste update: '+s.diesel_datum;
  renderPallets();calcVaste();
}

function loadDefaults(){
  S_FIELDS.forEach(f=>{if(DEFAULTS[f]!==undefined)sv('s_'+f,DEFAULTS[f]);});
  pallets=JSON.parse(JSON.stringify(DEFAULTS.pallets));
  renderPallets();calcVaste();
}

function saveSettings(){
  const s={};
  S_FIELDS.forEach(f=>s[f]=gv('s_'+f));
  s.pallets=JSON.parse(JSON.stringify(pallets));
  s.diesel_datum=new Date().toLocaleDateString('nl-NL');
  localStorage.setItem('pl_settings',JSON.stringify(s));
  document.getElementById('diesel-datum').textContent='Laatste update: '+s.diesel_datum;
  const msg=document.getElementById('s-save-msg');msg.style.display='block';
  setTimeout(()=>msg.style.display='none',3000);
  calcVaste();
}

function calcVaste(){
  const s=S();
  const trailer=parseFloat(gv('s_trailer_lease'))||0;
  const verz=parseFloat(gv('s_verzekering'))||0;
  const admin=parseFloat(gv('s_admin'))||0;
  const overig=parseFloat(gv('s_overig_lasten'))||0;
  const datum=gv('s_scania_datum');
  const scaniaLease=parseFloat(gv('s_scania_lease'))||0;
  const scaniaOndh=parseFloat(gv('s_scania_onderhoud'))||0;
  const scaniaActief=datum&&new Date(datum)<=new Date();
  const scaniaMnd=scaniaActief?scaniaLease+scaniaOndh:0;
  const totMnd=trailer+verz+admin+overig+scaniaMnd;
  document.getElementById('vaste-per-week').textContent='€'+(totMnd/4.33).toFixed(2)+'/week';
  // Scania preview
  const huurDag=parseFloat(gv('s_huur_dag'))||230;
  const huurDagen=parseFloat(gv('s_huur_dagen'))||2;
  const huurMnd=huurDag*huurDagen*4.33;
  const scaniaTotaal=scaniaLease+scaniaOndh;
  const besparing=huurMnd-scaniaTotaal;
  const el=document.getElementById('scania-preview');
  if(!datum){el.textContent='Vul activatiedatum in voor een vooruitblik.';return;}
  const d=new Date(datum);
  const ds=d.toLocaleDateString('nl-NL',{day:'numeric',month:'long',year:'numeric'});
  if(scaniaActief){el.innerHTML=`Scania actief — vaste lasten omgeschakeld naar lease.`;}
  else{el.innerHTML=`Vanaf <strong>${ds}</strong> overschakelen. Huurwagen nu: ca. €${Math.round(huurMnd)}/mnd → Scania: €${Math.round(scaniaTotaal)}/mnd. Besparing: <strong style="color:var(--success)">€${Math.round(besparing)}/mnd</strong>`;}
}

function renderPallets(){
  const c=document.getElementById('pallet-rows');c.innerHTML='';
  pallets.forEach((p,i)=>{
    const kpv=(p.inkoop+(p.reparatie_aan?p.reparatie:0));
    const m=p.verkoop&&kpv>0?((p.verkoop-kpv)/kpv*100).toFixed(0):null;
    const ms=m!==null?(parseFloat(m)>=30?'background:var(--success-bg);color:var(--success)':parseFloat(m)>=10?'background:var(--warning-bg);color:var(--warning)':'background:var(--danger-bg);color:var(--danger)'):'';
    const row=document.createElement('div');row.className='pallet-row';
    row.innerHTML=`
      <div class="field"><input type="text" value="${p.naam}" placeholder="Naam" oninput="pallets[${i}].naam=this.value" /></div>
      <div class="field"><input type="number" value="${p.inkoop}" step="0.01" min="0" placeholder="0.00" oninput="pallets[${i}].inkoop=parseFloat(this.value)||0;renderPallets()" /></div>
      <div class="field">
        <div style="display:flex;align-items:center;gap:6px">
          <input type="checkbox" ${p.reparatie_aan?'checked':''} onchange="pallets[${i}].reparatie_aan=this.checked;renderPallets()" title="Reparatie inbegrepen" style="width:15px;height:15px" />
          <input type="number" value="${p.reparatie||''}" ${p.reparatie_aan?'':'disabled'} step="0.01" min="0" placeholder="0.00" oninput="pallets[${i}].reparatie=parseFloat(this.value)||0;renderPallets()" style="${p.reparatie_aan?'':'opacity:.4'}" />
        </div>
        ${p.reparatie_aan?`<span style="font-size:10px;color:var(--text2)">kostprijs: €${kpv.toFixed(2)}</span>`:''}
      </div>
      <div class="field">
        <input type="number" value="${p.verkoop}" step="0.01" min="0" placeholder="0.00" oninput="pallets[${i}].verkoop=parseFloat(this.value)||0;renderPallets()" />
        ${m!==null?`<span class="marge-pill" style="${ms}">${m}% marge</span>`:''}
      </div>
      <button class="btn btn-icon btn-danger" onclick="pallets.splice(${i},1);renderPallets()" title="Verwijder">✕</button>
    `;
    c.appendChild(row);
  });
}
function addPallet(){pallets.push({naam:'',inkoop:0,reparatie:0,reparatie_aan:false,verkoop:0});renderPallets();}

// ========================
// KLANTEN
// ========================
function renderKlanten(){
  const q=(gv('klant-search')||'').toLowerCase();
  let all=C();
  document.getElementById('klant-count').textContent=all.length+' klanten';
  let cl=q?all.filter(c=>(c.bedrijf||'').toLowerCase().includes(q)||(c.contact||'').toLowerCase().includes(q)||(c.plaats||'').toLowerCase().includes(q)):all;
  const tbody=document.getElementById('klant-tbody');
  const empty=document.getElementById('klant-empty');
  if(cl.length===0){tbody.innerHTML='';empty.style.display='block';return;}
  empty.style.display='none';
  tbody.innerHTML=cl.map(c=>{
    const col=colorFor(c.bedrijf||'?');
    const adres=[c.straat,c.postcode,c.plaats].filter(Boolean).join(', ');
    return `<tr>
      <td><div class="initials" style="background:${col.bg};color:${col.text}">${initials(c.bedrijf||'?')}</div></td>
      <td><strong style="font-weight:500">${c.bedrijf||'—'}</strong></td>
      <td style="color:var(--text2)">${c.contact||'—'}</td>
      <td style="color:var(--text2);font-size:12px">${adres||'—'}</td>
      <td>${c.pallet?`<span class="badge badge-info">${c.pallet}</span>`:'—'}</td>
      <td style="color:var(--text3);font-size:12px;max-width:130px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${c.notities||''}</td>
      <td><div class="row-actions"><button class="btn btn-sm" onclick="openKlantModal('${c.id}')">Bewerken</button><button class="btn btn-sm btn-danger" onclick="deleteKlant('${c.id}')">✕</button></div></td>
    </tr>`;
  }).join('');
}

function openKlantModal(id){
  editKlantId=id||null;
  document.getElementById('km-title').textContent=id?'Klant bewerken':'Klant toevoegen';
  // fill pallet select
  const sel=document.getElementById('kf_pallet');
  sel.innerHTML='<option value="">— geen voorkeur —</option>';
  getPallets().forEach(p=>{const o=document.createElement('option');o.value=p.naam;o.textContent=p.naam;sel.appendChild(o);});
  if(id){
    const c=C().find(x=>x.id===id);
    if(c){sv('kf_bedrijf',c.bedrijf||'');sv('kf_contact',c.contact||'');sv('kf_tel',c.tel||'');sv('kf_email',c.email||'');sv('kf_straat',c.straat||'');sv('kf_plaats',c.plaats||'');sv('kf_postcode',c.postcode||'');sv('kf_land',c.land||'NL');sv('kf_pallet',c.pallet||'');sv('kf_aantal',c.aantal||'');sv('kf_notities',c.notities||'');}
  } else {
    ['kf_bedrijf','kf_contact','kf_tel','kf_email','kf_straat','kf_plaats','kf_postcode','kf_aantal','kf_notities'].forEach(f=>sv(f,''));
    sv('kf_land','NL');sv('kf_pallet','');
  }
  document.getElementById('kf_error').style.display='none';
  document.getElementById('klant-modal').style.display='flex';
}
function closeKlantModal(){document.getElementById('klant-modal').style.display='none';}
function saveKlant(){
  const bedrijf=gv('kf_bedrijf').trim();const straat=gv('kf_straat').trim();const plaats=gv('kf_plaats').trim();
  const err=document.getElementById('kf_error');
  if(!bedrijf){err.textContent='Bedrijfsnaam is verplicht.';err.style.display='block';return;}
  if(!straat||!plaats){err.textContent='Straat en plaats zijn verplicht voor routeberekening.';err.style.display='block';return;}
  err.style.display='none';
  const clients=C();
  const obj={id:editKlantId||(Date.now().toString(36)+Math.random().toString(36).slice(2)),bedrijf,contact:gv('kf_contact').trim(),tel:gv('kf_tel').trim(),email:gv('kf_email').trim(),straat,plaats,postcode:gv('kf_postcode').trim(),land:gv('kf_land'),pallet:gv('kf_pallet'),aantal:parseInt(gv('kf_aantal'))||0,notities:gv('kf_notities').trim()};
  if(editKlantId){const idx=clients.findIndex(x=>x.id===editKlantId);if(idx>=0)clients[idx]=obj;}
  else clients.push(obj);
  saveC(clients);closeKlantModal();renderKlanten();
}
function deleteKlant(id){if(!confirm('Klant verwijderen?'))return;saveC(C().filter(x=>x.id!==id));renderKlanten();}

// ========================
// WEEKINVOER
// ========================
function initWeek(){
  const now=new Date();
  if(!gv('w_week'))sv('w_week',wn(now));
  if(!gv('w_jaar'))sv('w_jaar',now.getFullYear());
  const s=S();
  if(!gv('w_diesel'))sv('w_diesel',s.diesel||1.75);
  sv('w_apikey',localStorage.getItem('pl_ors_key')||'');
  const epal=s.pallets?.find(p=>p.naam?.toLowerCase().includes('epal'));
  if(!gv('veeke-tarief'))sv('veeke-tarief',epal?.reparatie||3.25);
  const vd=JSON.parse(localStorage.getItem('pl_veeke')||'{"voorraad":0}');
  if(!gv('veeke-beginstand'))sv('veeke-beginstand',vd.voorraad||0);
  if(wDays[1].length===0)addStop(1,'levering');
  if(wDays[2].length===0)addStop(2,'levering');
  updateVeeke();
}

function toggleDay(d){
  dayOpen[d]=!dayOpen[d];
  document.getElementById('day-body-'+d).style.display=dayOpen[d]?'block':'none';
  document.getElementById('chev'+d).textContent=dayOpen[d]?'▾':'▸';
}

function updateVeeke(){
  const begin=parseInt(gv('veeke-beginstand'))||0;
  let bracht=0,opgehaald=0;const log=[];
  [1,2].forEach(dag=>{wDays[dag].forEach((s,i)=>{
    if(s.type==='reparatie'){
      if(s.rep_actie==='brengen'||s.rep_actie==='beide'){bracht+=s.rep_brengen||0;if(s.rep_brengen>0)log.push(`Dag ${dag}: ${s.rep_brengen} gebracht`);}
      if(s.rep_actie==='ophalen'||s.rep_actie==='beide'){opgehaald+=s.rep_ophalen||0;if(s.rep_ophalen>0)log.push(`Dag ${dag}: ${s.rep_ophalen} opgehaald`);}
    }
  });});
  document.getElementById('veeke-huidig').textContent=begin+bracht-opgehaald;
  document.getElementById('veeke-gebracht').textContent=bracht;
  document.getElementById('veeke-opgehaald').textContent=opgehaald;
  document.getElementById('veeke-log').innerHTML=log.map(l=>`<div>${l}</div>`).join('');
  herbereken();
}

function updateRouteVisual(dag){
  const vis=document.getElementById('rv'+dag);
  const stops=wDays[dag];
  if(!stops.length){vis.style.display='none';return;}
  vis.style.display='flex';
  let h=`<span class="rn-home">🏠 Depot</span>`;
  stops.forEach((s,i)=>{
    if(s.type==='reparatie'){h+=`<span class="rn-arrow">→</span><span class="rn-repair">🔧 Veeke</span>`;}
    else{const label=s.clientId?(C().find(c=>c.id===s.clientId)?.bedrijf||'Stop '+(i+1)):'Stop '+(i+1);h+=`<span class="rn-arrow">→</span><span class="rn-stop">${label}</span>`;}
  });
  h+=`<span class="rn-arrow">→</span><span class="rn-home">🏠 Depot</span>`;
  vis.innerHTML=h;
}

function renderStops(dag){
  const c=document.getElementById('stops-'+dag);c.innerHTML='';
  const s=S();
  wDays[dag].forEach((stop,i)=>{
    const div=document.createElement('div');
    if(stop.type==='reparatie'){
      div.className='stop-card stop-reparatie';
      div.innerHTML=`
        <div class="stop-header">
          <div style="display:flex;align-items:center;gap:8px"><span class="badge badge-repair">🔧 Veeke stop ${i+1}</span></div>
          <button class="btn btn-sm btn-danger" onclick="removeStop(${dag},${i})">✕</button>
        </div>
        <div class="g3 mb">
          <div class="field"><label style="color:#0F6E56">Actie</label>
            <select onchange="wDays[${dag}][${i}].rep_actie=this.value;renderStops(${dag})" style="background:rgba(255,255,255,0.7)">
              <option value="brengen" ${stop.rep_actie==='brengen'?'selected':''}>Pallets brengen</option>
              <option value="ophalen" ${stop.rep_actie==='ophalen'?'selected':''}>Pallets ophalen</option>
              <option value="beide" ${stop.rep_actie==='beide'?'selected':''}>Brengen + ophalen</option>
            </select>
          </div>
          ${stop.rep_actie==='brengen'||stop.rep_actie==='beide'?`<div class="field"><label style="color:#0F6E56">Aantal te brengen</label><input type="number" class="big" min="0" value="${stop.rep_brengen||''}" placeholder="0" oninput="wDays[${dag}][${i}].rep_brengen=parseInt(this.value)||0;updateVeeke()" style="background:rgba(255,255,255,0.7)" /></div>`:'<div></div>'}
          ${stop.rep_actie==='ophalen'||stop.rep_actie==='beide'?`<div class="field"><label style="color:#0F6E56">Aantal op te halen</label><input type="number" class="big" min="0" value="${stop.rep_ophalen||''}" placeholder="0" oninput="wDays[${dag}][${i}].rep_ophalen=parseInt(this.value)||0;updateVeeke()" style="background:rgba(255,255,255,0.7)" /></div>`:'<div></div>'}
        </div>
        ${stop.rep_actie==='ophalen'||stop.rep_actie==='beide'?`<div style="font-size:12px;color:#085041">Reparatiekosten: <strong>€${((stop.rep_ophalen||0)*(parseFloat(gv('veeke-tarief'))||3.25)).toFixed(2)}</strong></div>`:''}
      `;
    } else {
      const kkp=kp(stop.pallet);
      const m=stop.verkoop&&kkp>0?((stop.verkoop-kkp)/kkp*100).toFixed(0):null;
      const ms=m!==null?(parseFloat(m)>=30?'background:var(--success-bg);color:var(--success)':parseFloat(m)>=10?'background:var(--warning-bg);color:var(--warning)':'background:var(--danger-bg);color:var(--danger)'):'';
      div.className='stop-card stop-levering';
      div.innerHTML=`
        <div class="stop-header">
          <div style="display:flex;align-items:center;gap:8px">
            <span class="badge badge-info">Levering ${i+1}</span>
            ${stop.adres?`<span style="font-size:11px;color:var(--text3)">${stop.adres}</span>`:''}
          </div>
          <button class="btn btn-sm btn-danger" onclick="removeStop(${dag},${i})">✕</button>
        </div>
        <div class="g2 mb">
          <div class="field"><label>Klant</label><select onchange="onClientChange(${dag},${i},this.value)" id="sc_${dag}_${i}"></select></div>
          <div class="field"><label>Pallettype</label><select onchange="wDays[${dag}][${i}].pallet=this.value;wDays[${dag}][${i}].inkoop=kp(this.value);herbereken()" id="sp_${dag}_${i}"></select></div>
        </div>
        <div class="g3 mb">
          <div class="field"><label>Aantal pallets</label><input type="number" class="big" min="0" value="${stop.aantal||''}" placeholder="0" oninput="wDays[${dag}][${i}].aantal=parseInt(this.value)||0;herbereken()" /></div>
          <div class="field"><label>Inkoop p/st (€ excl.)</label>
            <input type="number" step="0.01" min="0" value="${stop.inkoop||''}" placeholder="0.00" oninput="wDays[${dag}][${i}].inkoop=parseFloat(this.value)||0;herbereken()" />
            ${kkp>0?`<span style="font-size:11px;color:var(--text2)">kostprijs: €${kkp.toFixed(2)}</span>`:''}
          </div>
          <div class="field"><label>Verkoop p/st (€ excl.)</label>
            <input type="number" step="0.01" min="0" value="${stop.verkoop||''}" placeholder="0.00" oninput="wDays[${dag}][${i}].verkoop=parseFloat(this.value)||0;herbereken()" />
            ${m!==null?`<span class="marge-pill" style="${ms}">${m}% marge</span>`:''}
          </div>
        </div>
        <div style="display:flex;align-items:center;gap:12px;flex-wrap:wrap;padding-top:8px;border-top:1px solid var(--border)">
          <label class="kaan-toggle"><input type="checkbox" ${stop.kaan?'checked':''} onchange="wDays[${dag}][${i}].kaan=this.checked;herbereken()" /> Geregeld door Kaan</label>
          ${stop.kaan?`<span style="font-size:12px;color:var(--text2)">Commissie: €${(parseFloat(s.commissie)||100).toFixed(2)}</span>`:''}
          ${stop.aantal&&stop.verkoop?`<span style="font-size:13px;color:var(--text2)">Omzet: <strong style="color:var(--text)">€${((stop.aantal||0)*(stop.verkoop||0)).toFixed(2)}</strong></span>`:''}
        </div>
      `;
    }
    c.appendChild(div);
    if(stop.type!=='reparatie'){
      const csel=document.getElementById(`sc_${dag}_${i}`);
      csel.innerHTML='<option value="">— kies klant —</option>';
      C().forEach(cl=>{const o=document.createElement('option');o.value=cl.id;o.textContent=cl.bedrijf;if(cl.id===stop.clientId)o.selected=true;csel.appendChild(o);});
      const psel=document.getElementById(`sp_${dag}_${i}`);
      psel.innerHTML='<option value="">— type —</option>';
      getPallets().forEach(p=>{const o=document.createElement('option');o.value=p.naam;o.textContent=p.naam;if(p.naam===stop.pallet)o.selected=true;psel.appendChild(o);});
    }
  });
  updateRouteVisual(dag);herbereken();
}

function onClientChange(dag,i,clientId){
  const cl=C().find(x=>x.id===clientId);
  wDays[dag][i].clientId=clientId;wDays[dag][i].coords=null;
  if(cl){
    wDays[dag][i].adres=[cl.straat,cl.postcode,cl.plaats].filter(Boolean).join(', ');
    if(!wDays[dag][i].pallet&&cl.pallet){wDays[dag][i].pallet=cl.pallet;wDays[dag][i].inkoop=kp(cl.pallet);}
    if(!wDays[dag][i].aantal&&cl.aantal)wDays[dag][i].aantal=cl.aantal;
  }
  renderStops(dag);
}

function addStop(dag,type){
  if(type==='reparatie')wDays[dag].push({type:'reparatie',rep_actie:'brengen',rep_brengen:0,rep_ophalen:0,adres:'Veeke Palletreparatie, Moerdijk',coords:null});
  else wDays[dag].push({type:'levering',clientId:'',pallet:'',aantal:0,inkoop:0,verkoop:0,kaan:false,adres:'',coords:null});
  renderStops(dag);
}
function removeStop(dag,i){wDays[dag].splice(i,1);renderStops(dag);}

async function geocode(adres,apikey){
  const r=await fetch(`https://api.openrouteservice.org/geocode/search?api_key=${apikey}&text=${encodeURIComponent(adres)}&boundary.country=NL,BE,DE&size=1`);
  const d=await r.json();
  if(d.features&&d.features[0])return d.features[0].geometry.coordinates;
  return null;
}

async function berekenRoute(dag){
  const apikey=gv('w_apikey').trim();
  if(!apikey){alert('Vul eerst je ORS API key in.');return;}
  const rr=document.getElementById('rr'+dag);rr.style.display='block';rr.innerHTML='<span class="spin">◌</span> Route berekenen...';
  try{
    const dc=await geocode(getDepot(),apikey);
    if(!dc){rr.innerHTML='⚠ Depotlocatie niet gevonden.';return;}
    const stops=wDays[dag].filter(x=>x.adres);
    if(!stops.length){rr.innerHTML='⚠ Geen stops met adres.';return;}
    for(const s of stops)if(!s.coords)s.coords=await geocode(s.adres,apikey);
    const coords=[dc,...stops.filter(s=>s.coords).map(s=>s.coords),dc];
    const resp=await fetch('https://api.openrouteservice.org/v2/directions/driving-hgv',{method:'POST',headers:{'Content-Type':'application/json','Authorization':apikey},body:JSON.stringify({coordinates:coords,units:'km'})});
    const data=await resp.json();
    if(data.routes&&data.routes[0]){
      const km=Math.round(data.routes[0].summary.distance);
      const mins=Math.round(data.routes[0].summary.duration/60);
      const u=Math.floor(mins/60);const m=mins%60;
      wKmDag[dag]=km;document.getElementById('km-dag'+dag).textContent=km+' km';
      rr.style.cssText='display:flex;align-items:center;gap:10px;padding:8px 12px;background:var(--success-bg);border-radius:var(--radius);font-size:13px;flex-wrap:wrap;margin-top:10px';
      rr.innerHTML=`✓ <strong>${km} km</strong> · Ca. <strong>${u>0?u+'u ':''} ${m} min</strong> · <span style="color:var(--text2);font-size:12px">Start + eindpunt: Depot · ${stops.filter(s=>s.coords).length} stops</span>`;
      herbereken();
    } else rr.innerHTML='⚠ '+(data.error?.message||'Route niet berekend');
  }catch(e){rr.innerHTML='⚠ Fout: '+e.message;}
}

function herbereken(){
  const s=S();
  const diesel=parseFloat(gv('w_diesel'))||parseFloat(s.diesel)||1.75;
  const totKm=(wKmDag[1]||0)+(wKmDag[2]||0);
  const isScania=s.scania_datum&&new Date(s.scania_datum)<=new Date();
  const verbruik=parseFloat(isScania?s.scania_verbruik:s.huur_verbruik)||28;
  const dieselK=totKm*(verbruik/100)*diesel;
  const wegenbK=totKm*(parseFloat(s.wegenbelasting)||0.20);
  const huurK=isScania?0:(parseFloat(s.huur_dag)||230)*2;
  const transportK=dieselK+wegenbK+huurK;
  const vasteMnd=(parseFloat(s.trailer_lease)||0)+(parseFloat(s.verzekering)||0)+(parseFloat(s.admin)||0)+(parseFloat(s.overig_lasten)||0)+(isScania?(parseFloat(s.scania_lease)||0)+(parseFloat(s.scania_onderhoud)||0):0);
  const vasteWeek=vasteMnd/4.33;
  const veekeTarief=parseFloat(gv('veeke-tarief'))||3.25;
  let veekeK=0,omzet=0,inkoop=0,commissie=0;
  [1,2].forEach(dag=>{wDays[dag].forEach(stop=>{
    if(stop.type==='reparatie'){if(stop.rep_actie==='ophalen'||stop.rep_actie==='beide')veekeK+=(stop.rep_ophalen||0)*veekeTarief;}
    else{omzet+=(stop.aantal||0)*(stop.verkoop||0);inkoop+=(stop.aantal||0)*(stop.inkoop||0);if(stop.kaan)commissie+=parseFloat(s.commissie)||100;}
  });});
  const bruto=omzet-inkoop;
  const totK=transportK+vasteWeek+commissie+veekeK;
  const netto=bruto-totK;
  const doelWeek=(parseFloat(s.winstdoel)||4000)/4.33;
  document.getElementById('sum-omzet').textContent='€'+omzet.toFixed(2);
  document.getElementById('sum-inkoop').textContent='€'+inkoop.toFixed(2);
  document.getElementById('sum-kosten').textContent='€'+totK.toFixed(2);
  document.getElementById('sum-winst').textContent='€'+netto.toFixed(2);
  const sl=document.getElementById('w-stoplicht');
  if(omzet===0){sl.className='stoplicht sl-grijs';sl.textContent='Voer leveringen in om resultaat te zien';}
  else if(netto>=doelWeek){sl.className='stoplicht sl-groen';sl.textContent='✓ Op koers — nettowinst boven weekdoelstelling (€'+Math.round(doelWeek)+')';}
  else if(netto>0){sl.className='stoplicht sl-oranje';sl.textContent='▲ Positief maar onder weekdoel — tekort: €'+Math.round(doelWeek-netto);}
  else{sl.className='stoplicht sl-rood';sl.textContent='✕ Verlies deze week — actie vereist';}
  document.getElementById('w-cost-breakdown').innerHTML=[
    ['Omzet','€'+omzet.toFixed(2)],['Inkoopkosten pallets','- €'+inkoop.toFixed(2)],
    ['Reparatiekosten Veeke','- €'+veekeK.toFixed(2)],['Brutowinst','€'+(bruto-veekeK).toFixed(2)],
    [isScania?'Scania lease/week':'Huurwagen (2 dagen)','- €'+(isScania?(parseFloat(s.scania_lease)||0+(parseFloat(s.scania_onderhoud)||0))/4.33:huurK).toFixed(2)],
    ['Diesel ('+totKm+' km)','- €'+dieselK.toFixed(2)],['Wegenbelasting','- €'+wegenbK.toFixed(2)],
    ['Vaste lasten (weekdeel)','- €'+vasteWeek.toFixed(2)],['Commissie Kaan','- €'+commissie.toFixed(2)],
    ['Nettowinst','€'+netto.toFixed(2)],
  ].map(([l,v],i,a)=>`<div class="cost-row" ${i===a.length-1?'style="font-weight:600"':''}><span>${l}</span><span>${v}</span></div>`).join('');
}

function slaWeekOp(){
  const week=gv('w_week');const jaar=gv('w_jaar');const key='pl_week_'+jaar+'_'+week;
  const eindstand=parseInt(document.getElementById('veeke-huidig').textContent)||0;
  localStorage.setItem('pl_veeke',JSON.stringify({voorraad:eindstand,bijgewerkt:new Date().toISOString()}));
  localStorage.setItem(key,JSON.stringify({week,jaar,days:wDays,kmDag:wKmDag,diesel:gv('w_diesel'),savedAt:new Date().toISOString()}));
  const idx=JSON.parse(localStorage.getItem('pl_weken_index')||'[]');
  if(!idx.includes(key))idx.push(key);
  localStorage.setItem('pl_weken_index',JSON.stringify(idx));
  const msg=document.getElementById('w-save-msg');msg.style.display='block';setTimeout(()=>msg.style.display='none',3000);
}

function nieuweWeek(){
  if(!confirm('Nieuwe week starten? Huidige invoer wordt gewist.'))return;
  const eindstand=parseInt(document.getElementById('veeke-huidig').textContent)||0;
  wDays={1:[],2:[]};wKmDag={1:0,2:0};
  [1,2].forEach(d=>{document.getElementById('rr'+d).style.display='none';document.getElementById('km-dag'+d).textContent='0 km';document.getElementById('rv'+d).style.display='none';});
  sv('w_week',wn(new Date()));sv('w_jaar',new Date().getFullYear());
  sv('veeke-beginstand',eindstand);
  addStop(1,'levering');addStop(2,'levering');updateVeeke();
}

// ========================
// DASHBOARD
// ========================
function calcWeek(w){
  const s=S();
  const diesel=parseFloat(w.diesel)||parseFloat(s.diesel)||1.75;
  const days=w.days||{1:[],2:[]};const kmDag=w.kmDag||{1:0,2:0};
  const totKm=(kmDag[1]||0)+(kmDag[2]||0);
  const isScania=s.scania_datum&&new Date(s.scania_datum)<=new Date();
  const verbruik=parseFloat(isScania?s.scania_verbruik:s.huur_verbruik)||28;
  const dieselK=totKm*(verbruik/100)*diesel;
  const wegenbK=totKm*(parseFloat(s.wegenbelasting)||0.20);
  const huurK=isScania?0:(parseFloat(s.huur_dag)||230)*2;
  const transportK=dieselK+wegenbK+huurK;
  const vasteMnd=(parseFloat(s.trailer_lease)||0)+(parseFloat(s.verzekering)||0)+(parseFloat(s.admin)||0)+(parseFloat(s.overig_lasten)||0)+(isScania?(parseFloat(s.scania_lease)||0)+(parseFloat(s.scania_onderhoud)||0):0);
  const vasteWeek=vasteMnd/4.33;
  const veekeTarief=parseFloat(s.pallets?.find(p=>p.naam?.toLowerCase().includes('epal'))?.reparatie)||3.25;
  let veekeK=0,omzet=0,inkoop=0,commissie=0,nPallets=0;
  [1,2].forEach(dag=>{(days[dag]||[]).forEach(stop=>{
    if(stop.type==='reparatie'){if(stop.rep_actie==='ophalen'||stop.rep_actie==='beide')veekeK+=(stop.rep_ophalen||0)*veekeTarief;}
    else{omzet+=(stop.aantal||0)*(stop.verkoop||0);inkoop+=(stop.aantal||0)*(stop.inkoop||0);nPallets+=(stop.aantal||0);if(stop.kaan)commissie+=parseFloat(s.commissie)||100;}
  });});
  const bruto=omzet-inkoop;const totK=transportK+vasteWeek+commissie+veekeK;const netto=bruto-totK;
  return{label:'W'+w.week+"'"+String(w.jaar).slice(2),week:parseInt(w.week),jaar:parseInt(w.jaar),sortkey:parseInt(w.jaar)*100+parseInt(w.week),omzet,inkoop,bruto,transportK,vasteWeek,commissie,veekeK,totK,netto,marge:omzet>0?netto/omzet*100:0,totKm,nPallets,dieselK,wegenbK,huurK};
}

function getAllWeken(){
  return JSON.parse(localStorage.getItem('pl_weken_index')||'[]').map(key=>{const raw=localStorage.getItem(key);return raw?calcWeek(JSON.parse(raw)):null;}).filter(Boolean).sort((a,b)=>a.sortkey-b.sortkey);
}

function filterWeken(weken){
  if(dFilter==='jaar')return weken.filter(w=>w.jaar===new Date().getFullYear());
  const n=dFilter==='2w'?2:dFilter==='4w'?4:dFilter==='8w'?8:12;
  return weken.slice(-n);
}

function setFilter(f,el){
  dFilter=f;
  document.querySelectorAll('.filter-btn').forEach(b=>b.classList.remove('active'));
  el.classList.add('active');renderDashboard();
}

function dc(id){if(dCharts[id]){dCharts[id].destroy();delete dCharts[id];}}

function renderDashboard(){
  const all=getAllWeken();const weken=filterWeken(all);const s=S();
  const doelWeek=(parseFloat(s.winstdoel)||4000)/4.33;
  if(!weken.length){
    ['kpi-omzet','kpi-winst','kpi-marge','kpi-km'].forEach(id=>document.getElementById(id).textContent='—');
    document.getElementById('d-stoplicht').className='stoplicht sl-grijs';
    document.getElementById('d-stoplicht').textContent='Nog geen weekdata — sla eerst een week op via Weekinvoer';
    renderScaniaDash(s);return;
  }
  const totO=weken.reduce((a,w)=>a+w.omzet,0);
  const totW=weken.reduce((a,w)=>a+w.netto,0);
  const totKm=weken.reduce((a,w)=>a+w.totKm,0);
  const gemM=totO>0?totW/totO*100:0;
  document.getElementById('kpi-omzet').textContent='€'+Math.round(totO).toLocaleString('nl-NL');
  document.getElementById('kpi-omzet-sub').textContent='gem. €'+Math.round(totO/weken.length).toLocaleString('nl-NL')+'/week';
  document.getElementById('kpi-winst').textContent='€'+Math.round(totW).toLocaleString('nl-NL');
  document.getElementById('kpi-winst-sub').textContent='gem. €'+Math.round(totW/weken.length).toLocaleString('nl-NL')+'/week';
  document.getElementById('kpi-marge').textContent=gemM.toFixed(1)+'%';
  document.getElementById('kpi-marge-sub').textContent='netto over omzet';
  document.getElementById('kpi-km').textContent=totKm.toLocaleString('nl-NL')+' km';
  document.getElementById('kpi-km-sub').textContent='gem. '+Math.round(totKm/weken.length)+' km/week';
  const laats=weken[weken.length-1];
  const sl=document.getElementById('d-stoplicht');
  if(laats.netto>=doelWeek){sl.className='stoplicht sl-groen';sl.textContent='✓ Laatste week op koers — nettowinst €'+Math.round(laats.netto).toLocaleString('nl-NL');}
  else if(laats.netto>0){sl.className='stoplicht sl-oranje';sl.textContent='▲ Laatste week positief maar onder weekdoel (€'+Math.round(doelWeek)+') — tekort: €'+Math.round(doelWeek-laats.netto);}
  else{sl.className='stoplicht sl-rood';sl.textContent='✕ Laatste week verlies: €'+Math.round(laats.netto).toLocaleString('nl-NL')+' — actie vereist';}
  // Break-even
  const gemInkoop=weken.reduce((a,w)=>a+w.inkoop,0)/Math.max(weken.reduce((a,w)=>a+w.nPallets,0),1);
  const gemVerk=weken.reduce((a,w)=>a+w.omzet,0)/Math.max(weken.reduce((a,w)=>a+w.nPallets,0),1);
  const gemVasteK=weken.reduce((a,w)=>a+w.totK-w.inkoop,0)/weken.length;
  const mpp=gemVerk-gemInkoop;
  const beBE=mpp>0?Math.ceil(gemVasteK/mpp):0;
  const beDoel=mpp>0?Math.ceil((gemVasteK+doelWeek)/mpp):0;
  const gemP=Math.round(weken.reduce((a,w)=>a+w.nPallets,0)/weken.length);
  const versch=gemP-beDoel;
  document.getElementById('be-pallets').textContent=beBE||'—';
  document.getElementById('be-doel').textContent=beDoel||'—';
  document.getElementById('be-verschil').textContent=(versch>=0?'+':'')+versch;
  document.getElementById('be-verschil').style.color=versch>=0?'var(--success)':'var(--danger)';
  document.getElementById('be-toelichting').textContent=`Gebaseerd op gem. verkoopprijs €${gemVerk.toFixed(2)}/pallet en kostprijs €${gemInkoop.toFixed(2)}/pallet over geselecteerde periode.`;
  // Bar chart
  dc('bar');const bctx=document.getElementById('cv-bar').getContext('2d');
  dCharts['bar']=new Chart(bctx,{type:'bar',data:{labels:weken.map(w=>w.label),datasets:[{label:'Omzet',data:weken.map(w=>Math.round(w.omzet)),backgroundColor:'#378ADD',borderRadius:3},{label:'Kosten',data:weken.map(w=>Math.round(w.totK)),backgroundColor:'#F09595',borderRadius:3},{label:'Nettowinst',data:weken.map(w=>Math.round(w.netto)),backgroundColor:'#1D9E75',borderRadius:3}]},options:{responsive:true,plugins:{legend:{position:'bottom',labels:{font:{size:11},boxWidth:12}}},scales:{x:{grid:{display:false},ticks:{font:{size:11}}},y:{ticks:{font:{size:11},callback:v=>'€'+v.toLocaleString('nl-NL')}}}}});
  // Marge chart
  dc('marge');const mctx=document.getElementById('cv-marge').getContext('2d');
  dCharts['marge']=new Chart(mctx,{type:'line',data:{labels:weken.map(w=>w.label),datasets:[{label:'Nettomarge %',data:weken.map(w=>parseFloat(w.marge.toFixed(1))),borderColor:'#1D9E75',backgroundColor:'rgba(29,158,117,0.08)',tension:0.3,pointRadius:4,fill:true}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{x:{grid:{display:false},ticks:{font:{size:11}}},y:{ticks:{font:{size:11},callback:v=>v+'%'}}}}});
  // Donut
  dc('donut');const dctx=document.getElementById('cv-donut').getContext('2d');
  dCharts['donut']=new Chart(dctx,{type:'doughnut',data:{labels:['Transport','Vaste lasten','Inkoop','Commissie Kaan','Veeke reparatie'],datasets:[{data:[Math.round(weken.reduce((a,w)=>a+w.transportK,0)),Math.round(weken.reduce((a,w)=>a+w.vasteWeek,0)),Math.round(weken.reduce((a,w)=>a+w.inkoop,0)),Math.round(weken.reduce((a,w)=>a+w.commissie,0)),Math.round(weken.reduce((a,w)=>a+w.veekeK,0))],backgroundColor:['#378ADD','#7F77DD','#F09595','#EF9F27','#5DCAA5'],borderWidth:0}]},options:{responsive:true,plugins:{legend:{position:'bottom',labels:{font:{size:11},boxWidth:12}}}}});
  // Maand chart
  dc('maand');const maanden={};const mn=['Jan','Feb','Mrt','Apr','Mei','Jun','Jul','Aug','Sep','Okt','Nov','Dec'];
  all.forEach(w=>{const d=new Date(w.jaar,0,1+((w.week-1)*7));const key=w.jaar+'-'+d.getMonth();if(!maanden[key])maanden[key]={label:mn[d.getMonth()]+" '"+String(w.jaar).slice(2),winst:0};maanden[key].winst+=w.netto;});
  const mkeys=Object.keys(maanden).sort();const mdata=mkeys.map(k=>Math.round(maanden[k].winst));const doelMnd=parseFloat(s.winstdoel)||4000;
  const mactx=document.getElementById('cv-maand').getContext('2d');
  dCharts['maand']=new Chart(mactx,{type:'bar',data:{labels:mkeys.map(k=>maanden[k].label),datasets:[{data:mdata,backgroundColor:mdata.map(v=>v>=doelMnd?'#1D9E75':v>0?'#EF9F27':'#E24B4A'),borderRadius:3}]},options:{responsive:true,plugins:{legend:{display:false},tooltip:{callbacks:{label:ctx=>'€'+ctx.raw.toLocaleString('nl-NL')}}},scales:{x:{grid:{display:false},ticks:{font:{size:11}}},y:{ticks:{font:{size:11},callback:v=>'€'+v.toLocaleString('nl-NL')}}}}});
  // Tabel
  const doelW=(parseFloat(s.winstdoel)||4000)/4.33;
  document.getElementById('d-week-tabel').innerHTML=`<table class="data-table"><thead><tr><th>Week</th><th>Omzet</th><th>Inkoop</th><th>Kosten</th><th>Nettowinst</th><th>Marge</th><th>Km</th><th>Status</th></tr></thead><tbody>${weken.slice().reverse().map(w=>`<tr><td>${w.label}</td><td>€${Math.round(w.omzet).toLocaleString('nl-NL')}</td><td>€${Math.round(w.inkoop).toLocaleString('nl-NL')}</td><td>€${Math.round(w.totK).toLocaleString('nl-NL')}</td><td style="font-weight:600;color:${w.netto>=doelW?'var(--success)':w.netto>0?'var(--warning)':'var(--danger)'}">€${Math.round(w.netto).toLocaleString('nl-NL')}</td><td>${w.marge.toFixed(1)}%</td><td>${w.totKm} km</td><td><span class="dot ${w.netto>=doelW?'dot-g':w.netto>0?'dot-o':'dot-r'}"></span>${w.netto>=doelW?'Op koers':w.netto>0?'Onder doel':'Verlies'}</td></tr>`).join('')}</tbody></table>`;
  renderScaniaDash(s);
}

function renderScaniaDash(s){
  const box=document.getElementById('d-scania');const inh=document.getElementById('d-scania-inhoud');
  if(!s.scania_datum){box.style.display='none';return;}
  const d=new Date(s.scania_datum);const nu=new Date();const actief=d<=nu;
  const ds=d.toLocaleDateString('nl-NL',{day:'numeric',month:'long',year:'numeric'});
  const huurMnd=(parseFloat(s.huur_dag)||230)*(parseFloat(s.huur_dagen)||2)*4.33;
  const scMnd=(parseFloat(s.scania_lease)||0)+(parseFloat(s.scania_onderhoud)||0);
  const besp=huurMnd-scMnd;
  box.style.display='block';
  if(actief)inh.innerHTML=`Scania actief vanaf ${ds}. Vaste lasten omgeschakeld naar lease. Maandlast: <strong>€${Math.round(scMnd).toLocaleString('nl-NL')}</strong>`;
  else{const dag=Math.ceil((d-nu)/(86400000));inh.innerHTML=`Scania verwacht op <strong>${ds}</strong> — nog <strong>${dag} dagen</strong>.<br>Huurwagen nu: ca. <strong>€${Math.round(huurMnd).toLocaleString('nl-NL')}/mnd</strong> → Scania: <strong>€${Math.round(scMnd).toLocaleString('nl-NL')}/mnd</strong><br>Verwachte besparing: <strong style="color:#085041">€${Math.round(besp).toLocaleString('nl-NL')}/mnd · €${Math.round(besp*12).toLocaleString('nl-NL')}/jaar</strong>`;}
}

// ========================
// INIT
// ========================
loadSettings();renderKlanten();
document.getElementById('w_apikey')?.addEventListener('change',function(){localStorage.setItem('pl_ors_key',this.value);});
</script>
</body>
</html>
