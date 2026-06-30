<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>KELEN SOLAR — Outil de dimensionnement</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600&family=IBM+Plex+Mono:wght@500;600&display=swap');

:root{
  --green:#1F5C3F;
  --green-dark:#143F2B;
  --gold:#F2A93B;
  --ochre:#B5481F;
  --water:#2D6E8E;
  --sand:#F2E9D8;
  --sand-dark:#E7D9BD;
  --ink:#2A2118;
  --paper:#FBF6EC;
  --line:#D9C9A6;
  --ok:#2E7D4F;
  --err:#B5481F;
  --radius:10px;
}
*{box-sizing:border-box;}
body{margin:0;background:var(--sand);color:var(--ink);font-family:'Inter',sans-serif;-webkit-font-smoothing:antialiased;}
h1,h2,h3,.display{font-family:'Space Grotesk',sans-serif;}
.mono{font-family:'IBM Plex Mono',monospace;}
a{color:inherit;}
button{font-family:inherit;cursor:pointer;}
:focus-visible{outline:3px solid var(--water);outline-offset:2px;}

/* header */
.brand-bar{background:var(--green);color:var(--paper);padding:22px 20px 18px;display:flex;flex-direction:column;align-items:center;gap:2px;position:relative;overflow:hidden;}
.brand-bar::before{content:"";position:absolute;right:-40px;top:-60px;width:180px;height:180px;border-radius:50%;background:radial-gradient(circle, rgba(242,169,59,0.35), transparent 70%);}
.brand-mark{font-size:1.5rem;font-weight:700;letter-spacing:0.04em;}
.brand-mark span{color:var(--gold);}
.brand-tagline{font-size:0.78rem;letter-spacing:0.18em;text-transform:uppercase;color:var(--sand-dark);}
.brand-sub{font-size:0.72rem;color:var(--sand-dark);opacity:.85;margin-top:6px;}

/* nav switches */
.switch-row{display:flex;justify-content:center;gap:10px;padding:16px 16px 0;flex-wrap:wrap;}
.profile-row{display:flex;justify-content:center;align-items:center;gap:10px;padding:10px 16px 0;font-size:0.82rem;color:#6b5e47;}
.mode-btn{border:1.5px solid var(--green);background:var(--paper);color:var(--green-dark);padding:10px 18px;border-radius:999px;font-weight:600;font-size:0.92rem;display:flex;align-items:center;gap:8px;transition:background .15s,color .15s;}
.mode-btn.active{background:var(--green);color:#fff;}
.profile-btn{border:1px solid var(--line);background:transparent;color:var(--ink);padding:5px 14px;border-radius:999px;font-size:0.8rem;font-weight:600;}
.profile-btn.active{background:var(--gold);border-color:var(--gold);color:#3a2900;}

main{max-width:1120px;margin:0 auto;padding:20px 16px 60px;}
.shared-card{max-width:1120px;margin:18px auto 0;padding:0 16px;}
.calc-panel{display:none;}
.calc-panel.active{display:block;}
.grid{display:grid;grid-template-columns:1.1fr 1fr;gap:24px;align-items:start;}
@media (max-width:780px){.grid{grid-template-columns:1fr;}}

.card{background:var(--paper);border:1px solid var(--line);border-radius:var(--radius);padding:18px 18px 20px;margin-bottom:18px;}
.card h2{font-size:1rem;margin:0 0 14px;display:flex;align-items:center;gap:8px;}
.card h2 .num{width:22px;height:22px;border-radius:50%;background:var(--green);color:#fff;font-size:0.72rem;font-weight:700;display:flex;align-items:center;justify-content:center;font-family:'IBM Plex Mono',monospace;flex-shrink:0;}
.card .sub{font-size:0.78rem;color:#897a5c;margin:-8px 0 14px;}

label{display:block;font-size:0.78rem;font-weight:600;color:#5d5038;margin-bottom:4px;text-transform:uppercase;letter-spacing:0.03em;}
input[type="number"],input[type="text"],select{width:100%;padding:8px 10px;border:1px solid var(--line);border-radius:6px;background:#fff;font-size:0.92rem;font-family:'IBM Plex Mono',monospace;color:var(--ink);}
input[type="text"]{font-family:'Inter',sans-serif;}
.field{margin-bottom:12px;}
.field-row{display:flex;gap:10px;flex-wrap:wrap;}
.field-row .field{flex:1;min-width:140px;}
.hint{font-size:0.74rem;color:#897a5c;margin-top:3px;}
.checkbox-row{display:flex;align-items:center;gap:7px;font-size:0.82rem;color:var(--ink);}
.checkbox-row input{width:auto;}

/* tabs */
.tabs{display:flex;gap:6px;margin-bottom:14px;border-bottom:1px solid var(--line);}
.tab-btn{background:none;border:none;padding:8px 14px;font-size:0.84rem;font-weight:600;color:#897a5c;border-bottom:2px solid transparent;margin-bottom:-1px;}
.tab-btn.active{color:var(--green-dark);border-bottom-color:var(--gold);}
.tab-panel{display:none;}
.tab-panel.active{display:block;}

/* appliance table */
.app-table{width:100%;border-collapse:collapse;font-size:0.83rem;}
.app-table th{text-align:left;font-size:0.65rem;text-transform:uppercase;color:#897a5c;padding:4px 5px;border-bottom:1px solid var(--line);}
.app-table td{padding:5px 5px;vertical-align:middle;}
.app-table input[type="text"]{padding:5px 7px;font-size:0.83rem;}
.app-table input[type="number"]{padding:5px 6px;font-size:0.83rem;}
.app-table input[type="checkbox"]{width:18px;height:18px;}
.rm-btn{background:none;border:none;color:var(--ochre);font-size:1.1rem;line-height:1;padding:2px 6px;}
.add-btn{margin-top:8px;background:var(--green);color:#fff;border:none;padding:8px 14px;border-radius:6px;font-size:0.82rem;font-weight:600;}
.add-btn:hover{background:var(--green-dark);}
.preset-row{display:flex;flex-wrap:wrap;gap:6px;margin-bottom:10px;}
.preset-btn{background:#fff;border:1px solid var(--line);border-radius:999px;padding:4px 11px;font-size:0.74rem;color:var(--ink);}
.preset-btn:hover{border-color:var(--gold);}

.tech-only{display:none;}
[data-profile-mode="tech"] .tech-only{display:block;}
[data-profile-mode="tech"] .tech-only.field-row{display:flex;}
[data-profile-mode="tech"] .tech-only.checkbox-row{display:flex;}
[data-profile-mode="tech"] .tech-only-cell{visibility:visible;}
.tech-only-cell{visibility:hidden;}

/* location card */
.loc-row{display:flex;gap:8px;flex-wrap:wrap;align-items:flex-end;}
.loc-row .field{flex:2;min-width:200px;margin-bottom:0;}
.loc-btn{background:var(--water);color:#fff;border:none;padding:9px 14px;border-radius:6px;font-size:0.82rem;font-weight:600;white-space:nowrap;height:38px;}
.loc-btn.alt{background:var(--gold);color:#3a2900;}
.loc-status{font-size:0.8rem;margin-top:10px;padding:8px 10px;border-radius:6px;background:#fff;border:1px solid var(--line);}
.loc-status.ok{color:var(--ok);border-color:var(--ok);}
.loc-status.err{color:var(--err);border-color:var(--err);}
.loc-status.idle{color:#897a5c;}

/* result chain (signature element) */
.chain{position:relative;display:flex;justify-content:space-between;padding:10px 4px 26px;margin-bottom:6px;}
.chain::before{content:"";position:absolute;top:24px;left:24px;right:24px;height:2px;background-image:linear-gradient(to right, var(--ochre) 0 6px, transparent 6px 12px);background-size:12px 2px;animation:trace 2.4s linear infinite;}
@keyframes trace{from{background-position:0 0;}to{background-position:240px 0;}}
.chain-node{position:relative;z-index:1;display:flex;flex-direction:column;align-items:center;width:20%;text-align:center;}
.chain-node .dot{width:26px;height:26px;border-radius:50%;background:var(--gold);color:#3a2900;font-family:'IBM Plex Mono',monospace;font-weight:700;font-size:0.7rem;display:flex;align-items:center;justify-content:center;border:3px solid var(--paper);box-shadow:0 0 0 1.5px var(--ochre);margin-bottom:6px;}
.chain-node .val{font-family:'IBM Plex Mono',monospace;font-weight:600;font-size:0.88rem;color:var(--green-dark);}
.chain-node .lbl{font-size:0.62rem;color:#897a5c;text-transform:uppercase;letter-spacing:0.02em;margin-top:1px;}

.result-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
.result-box{background:#fff;border:1px solid var(--line);border-radius:8px;padding:11px 13px;}
.result-box .rl{font-size:0.7rem;color:#897a5c;text-transform:uppercase;letter-spacing:0.02em;}
.result-box .rv{font-family:'IBM Plex Mono',monospace;font-weight:600;font-size:1.02rem;color:var(--ink);margin-top:2px;}
.result-box .rv small{font-size:0.68rem;font-weight:500;color:#897a5c;}
.result-box.highlight{border-color:var(--gold);background:#FFF7E8;}
.result-box.water{border-color:var(--water);}

/* devis table */
.devis-table{width:100%;border-collapse:collapse;font-size:0.84rem;margin-bottom:10px;}
.devis-table th{text-align:left;font-size:0.65rem;text-transform:uppercase;color:#897a5c;padding:5px 6px;border-bottom:1px solid var(--line);}
.devis-table td{padding:6px 6px;border-bottom:1px dashed var(--line);}
.devis-table input[type="number"]{padding:5px 6px;font-size:0.83rem;}
.devis-total-row{font-weight:700;font-family:'IBM Plex Mono',monospace;}
.devis-total-row td{border-bottom:none;padding-top:10px;}

.kpi-row{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-top:6px;}
@media (max-width:600px){.kpi-row{grid-template-columns:1fr;}}
.kpi{background:var(--green);color:#fff;border-radius:8px;padding:12px 14px;text-align:center;}
.kpi .kv{font-family:'IBM Plex Mono',monospace;font-weight:700;font-size:1.15rem;}
.kpi .kl{font-size:0.66rem;text-transform:uppercase;letter-spacing:0.03em;color:var(--sand-dark);margin-top:2px;}
.kpi.gold{background:var(--gold);color:#3a2900;}
.kpi.gold .kl{color:#5c3f00;}
.kpi.water{background:var(--water);}

.budget-toggle{background:none;border:1px solid var(--line);border-radius:6px;padding:7px 12px;font-size:0.8rem;font-weight:600;color:var(--water);display:flex;align-items:center;gap:6px;width:100%;text-align:left;}
.budget-body{display:none;margin-top:12px;}
.budget-body.open{display:block;}

.export-row{display:flex;gap:8px;flex-wrap:wrap;margin-top:14px;}
.export-btn{background:var(--green);color:#fff;border:none;border-radius:6px;padding:9px 13px;font-size:0.8rem;font-weight:600;display:flex;align-items:center;gap:6px;}
.export-btn:hover{background:var(--green-dark);}
.export-btn.whatsapp{background:#2E9E5B;}
.export-btn.email{background:var(--water);}

.disclaimer{max-width:1120px;margin:0 auto;padding:0 16px 40px;font-size:0.78rem;color:#897a5c;line-height:1.5;}
.disclaimer strong{color:var(--ochre);}

.pump-accent .card h2 .num{background:var(--water);}
.pump-accent .chain-node .dot{box-shadow:0 0 0 1.5px var(--water);}
</style>
</head>
<body>

<header class="brand-bar">
  <div class="brand-mark">KELEN <span>SOLAR</span></div>
  <div class="brand-tagline">La solution solaire</div>
  <div class="brand-sub">Outil de dimensionnement — photovoltaïque & pompage solaire</div>
</header>

<div class="shared-card">
  <div class="card">
    <h2><span class="num">①</span> Localisation & ensoleillement</h2>
    <div class="sub">L'ensoleillement réel de votre site change tout le dimensionnement. Cherchez une ville/adresse, utilisez votre position, ou saisissez la valeur manuellement.</div>
    <div class="loc-row">
      <div class="field">
        <label for="loc-query">Ville, adresse ou pays</label>
        <input type="text" id="loc-query" placeholder="ex : Bobo-Dioulasso, Burkina Faso">
      </div>
      <button class="loc-btn" id="loc-search-btn">Rechercher</button>
      <button class="loc-btn alt" id="loc-geo-btn">📍 Ma position</button>
    </div>
    <div class="loc-status idle" id="loc-status">Aucune recherche effectuée — valeur par défaut : 5,5 kWh/m²/jour (moyenne Sahel).</div>
    <div class="field-row" style="margin-top:12px;">
      <div class="field">
        <label for="shared-psh">Ensoleillement retenu (kWh/m²/jour = h de soleil équivalentes)</label>
        <input type="number" id="shared-psh" value="5.5" step="0.1" min="2.5" max="7.5">
      </div>
      <div class="field">
        <label for="shared-currency">Devise</label>
        <input type="text" id="shared-currency" value="FCFA">
      </div>
      <div class="field">
        <label for="shared-tariff">Tarif réseau (par kWh)</label>
        <input type="number" id="shared-tariff" value="100" step="1" min="0">
      </div>
    </div>
  </div>
</div>

<nav class="switch-row">
  <button class="mode-btn active" data-mode="pv"><span>☀</span> Photovoltaïque</button>
  <button class="mode-btn" data-mode="pump"><span>💧</span> Pompage solaire</button>
</nav>

<div class="profile-row">
  Profil d'utilisation :
  <button class="profile-btn active" data-profile="client">Client</button>
  <button class="profile-btn" data-profile="tech">Technicien</button>
</div>

<main data-profile-mode="client">

  <!-- ===================== PV PANEL ===================== -->
  <section class="calc-panel active" id="panel-pv">
    <div class="grid">
      <div class="form-col">

        <div class="card">
          <h2><span class="num">②</span> Consommation journalière</h2>
          <div class="tabs">
            <button class="tab-btn active" data-tab="pv-tab-app">Par appareils</button>
            <button class="tab-btn" data-tab="pv-tab-bill">Par facture</button>
          </div>

          <div class="tab-panel active" id="pv-tab-app">
            <div class="preset-row" id="pv-presets"></div>
            <table class="app-table">
              <thead>
                <tr><th style="width:30%">Appareil</th><th>W</th><th>Qté</th><th>h/j</th><th class="tech-only-cell" title="Démarrage moteur (pic de courant)">Moteur</th><th></th></tr>
              </thead>
              <tbody id="pv-app-rows"></tbody>
            </table>
            <button class="add-btn" id="pv-add-row">+ Ajouter un appareil</button>
            <div class="field-row tech-only" style="margin-top:12px;">
              <div class="field">
                <label for="pv-surge-mult">Facteur de pic au démarrage (moteurs)</label>
                <input type="number" id="pv-surge-mult" value="3" step="0.1" min="1.5" max="6">
                <div class="hint">Compresseurs, pompes, climatiseurs : 2,5 à 6x la puissance nominale au démarrage.</div>
              </div>
            </div>
          </div>

          <div class="tab-panel" id="pv-tab-bill">
            <div class="field-row">
              <div class="field">
                <label for="pv-bill-kwh">Consommation moyenne mensuelle (kWh)</label>
                <input type="number" id="pv-bill-kwh" value="150" step="1" min="0">
              </div>
              <div class="field">
                <label for="pv-bill-amount">Montant moyen facture (devise)</label>
                <input type="number" id="pv-bill-amount" value="15000" step="100" min="0">
              </div>
            </div>
            <div class="field">
              <label for="pv-bill-peak">Puissance maximale simultanée estimée (W)</label>
              <input type="number" id="pv-bill-peak" value="1500" step="50" min="0">
              <div class="hint">Estimation de la puissance de tous les appareils qui peuvent tourner en même temps — utile pour l'onduleur.</div>
            </div>
          </div>
        </div>

        <div class="card">
          <h2><span class="num">③</span> Système & batterie</h2>
          <div class="field-row">
            <div class="field">
              <label for="pv-autonomy">Jours d'autonomie sans soleil</label>
              <input type="number" id="pv-autonomy" value="1" min="1" max="4" step="1">
            </div>
            <div class="field">
              <label for="pv-batt-unit-ah">Capacité unitaire batterie (Ah)</label>
              <input type="number" id="pv-batt-unit-ah" value="200" min="20" step="10">
              <div class="hint">Capacité d'une seule batterie du commerce.</div>
            </div>
          </div>
          <div class="tech-only field-row">
            <div class="field">
              <label for="pv-voltage">Tension du système</label>
              <select id="pv-voltage">
                <option value="12">12 V</option>
                <option value="24" selected>24 V</option>
                <option value="48">48 V</option>
              </select>
            </div>
            <div class="field">
              <label for="pv-battery-type">Type de batterie</label>
              <select id="pv-battery-type">
                <option value="0.9">Lithium (LiFePO4) — DoD 90%</option>
                <option value="0.5">Plomb (AGM/gel) — DoD 50%</option>
              </select>
            </div>
          </div>
          <div class="tech-only field-row">
            <div class="field">
              <label for="pv-ratio">Ratio de performance système</label>
              <input type="number" id="pv-ratio" value="0.75" step="0.01" min="0.5" max="0.9">
            </div>
            <div class="field">
              <label for="pv-panel-w">Puissance unitaire panneau (Wc)</label>
              <select id="pv-panel-w">
                <option value="300">300 Wc</option>
                <option value="350" selected>350 Wc</option>
                <option value="400">400 Wc</option>
                <option value="450">450 Wc</option>
                <option value="500">500 Wc</option>
              </select>
            </div>
          </div>
        </div>

        <div class="card tech-only">
          <h2><span class="num">④</span> Câbles & protections</h2>
          <div class="field-row">
            <div class="field">
              <label for="pv-len-pv-bat">Distance panneaux → régulateur (m)</label>
              <input type="number" id="pv-len-pv-bat" value="10" min="1">
            </div>
            <div class="field">
              <label for="pv-len-bat-inv">Distance batterie → onduleur (m)</label>
              <input type="number" id="pv-len-bat-inv" value="2" min="0.5">
            </div>
          </div>
          <div class="field-row">
            <div class="field">
              <label for="pv-drop">Chute de tension max admise (%)</label>
              <input type="number" id="pv-drop" value="3" step="0.5" min="1" max="5">
            </div>
            <div class="field">
              <label for="pv-isc-module">Courant Isc par panneau (A)</label>
              <input type="number" id="pv-isc-module" value="10" step="0.1" min="1">
            </div>
          </div>
          <div class="field">
            <label for="pv-ac-voltage">Tension AC sortie onduleur (V)</label>
            <input type="number" id="pv-ac-voltage" value="230" step="1" min="100">
          </div>
        </div>

      </div>

      <div class="result-col">
        <div class="card">
          <h2>Chaîne de dimensionnement</h2>
          <div class="chain">
            <div class="chain-node"><div class="dot">1</div><div class="val" id="chain-pv-energy">—</div><div class="lbl">kWh/jour</div></div>
            <div class="chain-node"><div class="dot">2</div><div class="val" id="chain-pv-wc">—</div><div class="lbl">kWc panneaux</div></div>
            <div class="chain-node"><div class="dot">3</div><div class="val" id="chain-pv-batt">—</div><div class="lbl">kWh batterie</div></div>
            <div class="chain-node"><div class="dot">4</div><div class="val" id="chain-pv-inv">—</div><div class="lbl">VA onduleur</div></div>
            <div class="chain-node"><div class="dot">5</div><div class="val" id="chain-pv-prod">—</div><div class="lbl">MWh/an</div></div>
          </div>

          <div class="result-grid">
            <div class="result-box"><div class="rl">Énergie / jour</div><div class="rv" id="r-pv-energy">—</div></div>
            <div class="result-box"><div class="rl">Puissance simultanée</div><div class="rv" id="r-pv-power">—</div></div>
            <div class="result-box highlight"><div class="rl">Panneaux nécessaires</div><div class="rv" id="r-pv-panels">—</div></div>
            <div class="result-box"><div class="rl">Capacité batterie</div><div class="rv" id="r-pv-ah">—</div></div>
            <div class="result-box highlight"><div class="rl">Nombre de batteries</div><div class="rv" id="r-pv-nbbatt">—</div></div>
            <div class="result-box highlight"><div class="rl">Onduleur recommandé</div><div class="rv" id="r-pv-inv">—</div></div>
            <div class="result-box"><div class="rl">Puissance de pointe (démarrage)</div><div class="rv" id="r-pv-peak">—</div></div>
            <div class="result-box"><div class="rl">Régulateur / MPPT</div><div class="rv" id="r-pv-reg">—</div></div>
            <div class="result-box tech-only"><div class="rl">Câble PV→régulateur</div><div class="rv" id="r-pv-cable1">—</div></div>
            <div class="result-box tech-only"><div class="rl">Câble batterie→onduleur</div><div class="rv" id="r-pv-cable2">—</div></div>
            <div class="result-box tech-only"><div class="rl">Fusible par string PV</div><div class="rv" id="r-pv-fuse">—</div></div>
            <div class="result-box tech-only"><div class="rl">Disjoncteur DC batterie</div><div class="rv" id="r-pv-brk-dc">—</div></div>
            <div class="result-box tech-only"><div class="rl">Disjoncteur AC sortie</div><div class="rv" id="r-pv-brk-ac">—</div></div>
          </div>

          <div class="kpi-row">
            <div class="kpi"><div class="kv" id="kpi-pv-prod">—</div><div class="kl">Production annuelle</div></div>
            <div class="kpi gold"><div class="kv" id="kpi-pv-save">—</div><div class="kl">Économie annuelle</div></div>
            <div class="kpi water" id="kpi-pv-roi-box"><div class="kv" id="kpi-pv-roi">—</div><div class="kl">Retour sur invest.</div></div>
          </div>
        </div>

        <div class="card tech-only">
          <h2><span class="num">⑤</span> Devis estimatif</h2>
          <div class="field">
            <label for="dv-pv-client">Nom du client / projet</label>
            <input type="text" id="dv-pv-client" placeholder="ex : M. Traoré — Villa Bobo-Dioulasso">
          </div>
          <table class="devis-table">
            <thead><tr><th>Poste</th><th>Qté</th><th>Prix unitaire</th><th>Total</th></tr></thead>
            <tbody>
              <tr><td>Panneaux solaires (<span id="dv-pv-panel-w-label">350</span> Wc/u)</td><td class="mono" id="dv-pv-q-panel">—</td><td><input type="number" id="dv-pv-p-panel" value="0"></td><td class="mono" id="dv-pv-t-panel">0</td></tr>
              <tr><td>Batteries (<span id="dv-pv-batt-ah-label">200</span> Ah/u)</td><td class="mono" id="dv-pv-q-batt">—</td><td><input type="number" id="dv-pv-p-batt" value="0"></td><td class="mono" id="dv-pv-t-batt">0</td></tr>
              <tr><td>Onduleur</td><td class="mono">1</td><td><input type="number" id="dv-pv-p-inv" value="0"></td><td class="mono" id="dv-pv-t-inv">0</td></tr>
              <tr><td>Régulateur / MPPT</td><td class="mono">1</td><td><input type="number" id="dv-pv-p-reg" value="0"></td><td class="mono" id="dv-pv-t-reg">0</td></tr>
              <tr><td>Câblage & protections (forfait)</td><td class="mono">1</td><td><input type="number" id="dv-pv-p-cable" value="0"></td><td class="mono" id="dv-pv-t-cable">0</td></tr>
              <tr><td>Structure de support</td><td class="mono">1</td><td><input type="number" id="dv-pv-p-struct" value="0"></td><td class="mono" id="dv-pv-t-struct">0</td></tr>
              <tr><td>Main d'œuvre & installation</td><td class="mono">1</td><td><input type="number" id="dv-pv-p-labor" value="0"></td><td class="mono" id="dv-pv-t-labor">0</td></tr>
              <tr><td>Étude & déplacement</td><td class="mono">1</td><td><input type="number" id="dv-pv-p-study" value="0"></td><td class="mono" id="dv-pv-t-study">0</td></tr>
              <tr class="devis-total-row"><td colspan="3">TOTAL DEVIS</td><td class="mono" id="dv-pv-total">0</td></tr>
            </tbody>
          </table>
          <div class="export-row">
            <button class="export-btn" id="pv-devis-print">🖨 Enregistrer en PDF</button>
            <button class="export-btn whatsapp" id="pv-devis-whatsapp">💬 Envoyer par WhatsApp</button>
            <button class="export-btn email" id="pv-devis-email">✉ Envoyer par e-mail</button>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ===================== PUMPING PANEL ===================== -->
  <section class="calc-panel pump-accent" id="panel-pump">
    <div class="grid">
      <div class="form-col">

        <div class="card">
          <h2><span class="num">②</span> Besoin en eau</h2>
          <div class="field">
            <label for="pp-volume">Volume d'eau nécessaire (m³/jour)</label>
            <input type="number" id="pp-volume" value="10" step="0.5" min="0.5">
          </div>
          <div id="pp-hmt-simple">
            <div class="field">
              <label for="pp-hmt">Hauteur totale de refoulement (m)</label>
              <input type="number" id="pp-hmt" value="30" step="1" min="1">
              <div class="hint">Profondeur du forage + hauteur du réservoir + pertes estimées.</div>
            </div>
          </div>
          <div class="tech-only" id="pp-hmt-tech">
            <div class="field-row">
              <div class="field">
                <label for="pp-hstat">Hauteur statique (m)</label>
                <input type="number" id="pp-hstat" value="25" step="1" min="0">
              </div>
              <div class="field">
                <label for="pp-hperte">Pertes de charge / hauteur dynamique (m)</label>
                <input type="number" id="pp-hperte" value="5" step="1" min="0">
              </div>
            </div>
          </div>
        </div>

        <div class="card">
          <h2><span class="num">③</span> Rendement</h2>
          <div class="tech-only field-row">
            <div class="field">
              <label for="pp-eff-pump">Rendement pompe</label>
              <input type="number" id="pp-eff-pump" value="0.45" step="0.01" min="0.2" max="0.7">
              <div class="hint">Centrifuge submersible ≈ 0,40–0,50</div>
            </div>
            <div class="field">
              <label for="pp-eff-conv">Rendement variateur/convertisseur</label>
              <input type="number" id="pp-eff-conv" value="0.9" step="0.01" min="0.7" max="0.98">
            </div>
          </div>
          <div class="tech-only field-row">
            <div class="field">
              <label for="pp-ratio">Ratio de performance PV</label>
              <input type="number" id="pp-ratio" value="0.75" step="0.01" min="0.5" max="0.9">
            </div>
            <div class="field">
              <label for="pp-panel-w">Puissance unitaire panneau (Wc)</label>
              <select id="pp-panel-w">
                <option value="300">300 Wc</option>
                <option value="350" selected>350 Wc</option>
                <option value="400">400 Wc</option>
                <option value="450">450 Wc</option>
                <option value="500">500 Wc</option>
              </select>
            </div>
          </div>
          <div class="field">
            <label for="pp-diesel-cost">Coût actuel de pompage (diesel/réseau) — devise/mois</label>
            <input type="number" id="pp-diesel-cost" value="0" step="500" min="0">
            <div class="hint">Optionnel : pour estimer l'économie si vous remplacez une pompe diesel ou un raccordement réseau.</div>
          </div>
        </div>

        <div class="card tech-only">
          <h2><span class="num">④</span> Câbles & protections</h2>
          <div class="field-row">
            <div class="field">
              <label for="pp-len">Distance panneaux → pompe/variateur (m)</label>
              <input type="number" id="pp-len" value="15" min="1">
            </div>
            <div class="field">
              <label for="pp-drop">Chute de tension max admise (%)</label>
              <input type="number" id="pp-drop" value="3" step="0.5" min="1" max="5">
            </div>
          </div>
          <div class="field">
            <label for="pp-volt">Tension nominale pompe (V)</label>
            <input type="number" id="pp-volt" value="96" step="1" min="12">
            <div class="hint">Tension DC ou AC du bus pompe selon le modèle retenu.</div>
          </div>
        </div>

      </div>

      <div class="result-col">
        <div class="card">
          <h2>Chaîne de dimensionnement</h2>
          <div class="chain">
            <div class="chain-node"><div class="dot">1</div><div class="val" id="chain-pp-hyd">—</div><div class="lbl">kWh hydraulique</div></div>
            <div class="chain-node"><div class="dot">2</div><div class="val" id="chain-pp-elec">—</div><div class="lbl">kWh électrique</div></div>
            <div class="chain-node"><div class="dot">3</div><div class="val" id="chain-pp-wc">—</div><div class="lbl">kWc panneaux</div></div>
            <div class="chain-node"><div class="dot">4</div><div class="val" id="chain-pp-pump">—</div><div class="lbl">W pompe</div></div>
            <div class="chain-node"><div class="dot">5</div><div class="val" id="chain-pp-prod">—</div><div class="lbl">m³/an</div></div>
          </div>

          <div class="result-grid">
            <div class="result-box"><div class="rl">Énergie hydraulique / jour</div><div class="rv" id="r-pp-hyd">—</div></div>
            <div class="result-box"><div class="rl">Énergie électrique requise</div><div class="rv" id="r-pp-elec">—</div></div>
            <div class="result-box highlight"><div class="rl">Puissance crête PV</div><div class="rv" id="r-pp-wc">—</div></div>
            <div class="result-box highlight"><div class="rl">Panneaux nécessaires</div><div class="rv" id="r-pp-panels">—</div></div>
            <div class="result-box"><div class="rl">Puissance pompe conseillée</div><div class="rv" id="r-pp-pump">—</div></div>
            <div class="result-box"><div class="rl">HMT retenue</div><div class="rv" id="r-pp-hmt">—</div></div>
            <div class="result-box tech-only"><div class="rl">Câble PV → pompe</div><div class="rv" id="r-pp-cable">—</div></div>
            <div class="result-box tech-only"><div class="rl">Disjoncteur / fusible</div><div class="rv" id="r-pp-brk">—</div></div>
          </div>

          <div class="kpi-row">
            <div class="kpi water"><div class="kv" id="kpi-pp-prod">—</div><div class="kl">Eau pompée / an</div></div>
            <div class="kpi gold"><div class="kv" id="kpi-pp-save">—</div><div class="kl">Économie annuelle</div></div>
            <div class="kpi" id="kpi-pp-roi-box"><div class="kv" id="kpi-pp-roi">—</div><div class="kl">Retour sur invest.</div></div>
          </div>
        </div>

        <div class="card tech-only">
          <h2><span class="num">⑤</span> Devis estimatif</h2>
          <div class="field">
            <label for="dv-pp-client">Nom du client / projet</label>
            <input type="text" id="dv-pp-client" placeholder="ex : Coopérative maraîchère — Forage Sourou">
          </div>
          <table class="devis-table">
            <thead><tr><th>Poste</th><th>Qté</th><th>Prix unitaire</th><th>Total</th></tr></thead>
            <tbody>
              <tr><td>Panneaux solaires (<span id="dv-pp-panel-w-label">350</span> Wc/u)</td><td class="mono" id="dv-pp-q-panel">—</td><td><input type="number" id="dv-pp-p-panel" value="0"></td><td class="mono" id="dv-pp-t-panel">0</td></tr>
              <tr><td>Pompe + variateur</td><td class="mono">1</td><td><input type="number" id="dv-pp-p-pump" value="0"></td><td class="mono" id="dv-pp-t-pump">0</td></tr>
              <tr><td>Câblage & protections</td><td class="mono">1</td><td><input type="number" id="dv-pp-p-cable" value="0"></td><td class="mono" id="dv-pp-t-cable">0</td></tr>
              <tr><td>Forage / réservoir / tuyauterie</td><td class="mono">1</td><td><input type="number" id="dv-pp-p-infra" value="0"></td><td class="mono" id="dv-pp-t-infra">0</td></tr>
              <tr><td>Structure de support</td><td class="mono">1</td><td><input type="number" id="dv-pp-p-struct" value="0"></td><td class="mono" id="dv-pp-t-struct">0</td></tr>
              <tr><td>Main d'œuvre & installation</td><td class="mono">1</td><td><input type="number" id="dv-pp-p-labor" value="0"></td><td class="mono" id="dv-pp-t-labor">0</td></tr>
              <tr class="devis-total-row"><td colspan="3">TOTAL DEVIS</td><td class="mono" id="dv-pp-total">0</td></tr>
            </tbody>
          </table>
          <div class="export-row">
            <button class="export-btn" id="pp-devis-print">🖨 Enregistrer en PDF</button>
            <button class="export-btn whatsapp" id="pp-devis-whatsapp">💬 Envoyer par WhatsApp</button>
            <button class="export-btn email" id="pp-devis-email">✉ Envoyer par e-mail</button>
          </div>
        </div>
      </div>
    </div>
  </section>

</main>

<p class="disclaimer">
  <strong>Note :</strong> cet outil donne un pré-dimensionnement basé sur des hypothèses standard et sur les données d'ensoleillement récupérées en ligne (ou une valeur régionale par défaut si la recherche échoue). Le dimensionnement définitif d'une installation doit toujours être validé avec les fiches techniques réelles des équipements retenus (courbe HMT/débit de la pompe, datasheet des panneaux, tension réelle des batteries, etc.), une visite de site, et conformément aux normes électriques en vigueur localement.
</p>

<script>
(function(){
  "use strict";

  var mainEl = document.querySelector('main');

  /* ---------- mode + profile switching ---------- */
  document.querySelectorAll('.mode-btn').forEach(function(btn){
    btn.addEventListener('click', function(){
      document.querySelectorAll('.mode-btn').forEach(function(b){b.classList.remove('active');});
      btn.classList.add('active');
      var mode = btn.getAttribute('data-mode');
      document.getElementById('panel-pv').classList.toggle('active', mode === 'pv');
      document.getElementById('panel-pump').classList.toggle('active', mode === 'pump');
    });
  });

  document.querySelectorAll('.profile-btn').forEach(function(btn){
    btn.addEventListener('click', function(){
      document.querySelectorAll('.profile-btn').forEach(function(b){b.classList.remove('active');});
      btn.classList.add('active');
      mainEl.setAttribute('data-profile-mode', btn.getAttribute('data-profile'));
      recalcPV(); recalcPump();
    });
  });

  document.querySelectorAll('.tab-btn').forEach(function(btn){
    btn.addEventListener('click', function(){
      var group = btn.closest('.card');
      group.querySelectorAll('.tab-btn').forEach(function(b){b.classList.remove('active');});
      btn.classList.add('active');
      var target = btn.getAttribute('data-tab');
      group.querySelectorAll('.tab-panel').forEach(function(p){p.classList.toggle('active', p.id === target);});
      recalcPV();
    });
  });

  /* ---------- helpers ---------- */
  function fmt(n, d){ if (!isFinite(n)) return '—'; d = d===undefined?2:d; return n.toLocaleString('fr-FR', {minimumFractionDigits:d, maximumFractionDigits:d}); }
  function fmtInt(n){ if (!isFinite(n)) return '—'; return Math.round(n).toLocaleString('fr-FR'); }
  function val(id){ var el = document.getElementById(id); return el ? (parseFloat(el.value) || 0) : 0; }
  function txt(id){ var el = document.getElementById(id); return el ? el.value : ''; }
  function nearestStandard(value, list){
    for (var i=0;i<list.length;i++){ if (list[i] >= value) return list[i]; }
    return list[list.length-1] + ' (hors barème, vérifier)';
  }
  var CABLE_SECTIONS = [1.5,2.5,4,6,10,16,25,35,50,70,95,120,150];
  var BREAKER_RATINGS = [2,4,6,10,16,20,25,32,40,50,63,80,100,125,160,200];

  function cableSection(current, oneWayLengthM, voltage, dropPercent){
    var rho = 0.0175; // ohm.mm2/m copper
    var dU = voltage * (dropPercent/100);
    if (dU <= 0) return {section:0, label:'—'};
    var s = (2 * rho * oneWayLengthM * current) / dU;
    var std = nearestStandard(s, CABLE_SECTIONS);
    return {section:s, label: (typeof std === 'number' ? std + ' mm²' : std)};
  }
  function breakerRating(current){
    var target = current * 1.25;
    var std = nearestStandard(target, BREAKER_RATINGS);
    return (typeof std === 'number' ? std + ' A' : std);
  }

  /* ===================== shared location & tariff ===================== */
  var sharedPSH = document.getElementById('shared-psh');
  sharedPSH.addEventListener('input', function(){ recalcPV(); recalcPump(); });
  document.getElementById('shared-currency').addEventListener('input', function(){ recalcPV(); recalcPump(); });
  document.getElementById('shared-tariff').addEventListener('input', function(){ recalcPV(); recalcPump(); });

  var FALLBACK_PSH = {
    "bobo": 5.6, "ouagadougou": 5.7, "bamako": 5.8, "dakar": 5.9, "niamey": 6.0,
    "abidjan": 4.8, "cotonou": 4.9, "lome": 4.8, "accra": 5.0, "conakry": 4.7,
    "n'djamena": 6.1, "ndjamena": 6.1, "nouakchott": 6.2
  };

  function setLocStatus(msg, type){
    var el = document.getElementById('loc-status');
    el.textContent = msg;
    el.className = 'loc-status ' + (type || 'idle');
  }

  function applyFallback(query){
    var q = (query || '').toLowerCase();
    for (var key in FALLBACK_PSH){
      if (q.indexOf(key) !== -1){
        sharedPSH.value = FALLBACK_PSH[key];
        recalcPV(); recalcPump();
        return true;
      }
    }
    return false;
  }

  function fetchRadiationForCoords(lat, lon, label){
    setLocStatus("Récupération des données d'ensoleillement pour " + label + '…', 'idle');
    var end = new Date(); end.setDate(end.getDate() - 10);
    var start = new Date(end); start.setDate(start.getDate() - 365);
    function iso(d){ return d.toISOString().slice(0,10); }
    var url = 'https://archive-api.open-meteo.com/v1/archive?latitude=' + lat + '&longitude=' + lon +
      '&start_date=' + iso(start) + '&end_date=' + iso(end) +
      '&daily=shortwave_radiation_sum&timezone=auto';

    fetch(url).then(function(r){ if(!r.ok) throw new Error('HTTP '+r.status); return r.json(); })
      .then(function(data){
        var arr = data && data.daily && data.daily.shortwave_radiation_sum;
        if (!arr || !arr.length) throw new Error('Pas de données');
        var sum = 0, n = 0;
        arr.forEach(function(v){ if (v !== null && isFinite(v)){ sum += v; n++; } });
        if (n === 0) throw new Error('Pas de données valides');
        var avgMJ = sum / n;
        var psh = avgMJ / 3.6;
        sharedPSH.value = psh.toFixed(2);
        setLocStatus('Ensoleillement moyen calculé sur 1 an pour ' + label + ' : ' + psh.toFixed(2) + ' kWh/m²/jour.', 'ok');
        recalcPV(); recalcPump();
      })
      .catch(function(err){
        setLocStatus('Données en ligne indisponibles (' + err.message + '). Valeur régionale par défaut utilisée si reconnue, sinon ajustez manuellement.', 'err');
        applyFallback(label);
      });
  }

  document.getElementById('loc-search-btn').addEventListener('click', function(){
    var q = document.getElementById('loc-query').value.trim();
    if (!q){ setLocStatus('Saisissez une ville ou une adresse.', 'err'); return; }
    setLocStatus('Recherche de "' + q + '"…', 'idle');
    var url = 'https://geocoding-api.open-meteo.com/v1/search?name=' + encodeURIComponent(q) + '&count=1&language=fr&format=json';
    fetch(url).then(function(r){ if(!r.ok) throw new Error('HTTP '+r.status); return r.json(); })
      .then(function(data){
        if (!data.results || !data.results.length) throw new Error('Lieu introuvable');
        var res = data.results[0];
        var label = res.name + (res.country ? ', ' + res.country : '');
        fetchRadiationForCoords(res.latitude, res.longitude, label);
      })
      .catch(function(err){
        setLocStatus('Recherche impossible (' + err.message + '). Tentative avec une valeur régionale par défaut.', 'err');
        if (!applyFallback(q)){
          setLocStatus("Lieu non reconnu et pas de connexion. Ajustez l'ensoleillement manuellement ci-dessous.", 'err');
        }
      });
  });

  document.getElementById('loc-geo-btn').addEventListener('click', function(){
    if (!navigator.geolocation){ setLocStatus('Géolocalisation non disponible sur cet appareil/navigateur.', 'err'); return; }
    setLocStatus('Localisation en cours…', 'idle');
    navigator.geolocation.getCurrentPosition(function(pos){
      var lat = pos.coords.latitude, lon = pos.coords.longitude;
      fetchRadiationForCoords(lat, lon, 'votre position (' + lat.toFixed(2) + ', ' + lon.toFixed(2) + ')');
    }, function(err){
      setLocStatus('Géolocalisation refusée ou indisponible (' + err.message + '). Utilisez la recherche par ville.', 'err');
    }, {timeout:10000});
  });

  /* ===================== PV appliances ===================== */
  var PRESETS = [
    {name:"Ampoule LED", power:10, motor:false},
    {name:"Réfrigérateur", power:150, motor:true},
    {name:"Congélateur", power:200, motor:true},
    {name:"Téléviseur", power:80, motor:false},
    {name:"Ventilateur", power:60, motor:true},
    {name:"Climatiseur 1CV", power:1200, motor:true},
    {name:"Ordinateur", power:65, motor:false},
    {name:"Pompe immergée", power:750, motor:true},
    {name:"Chargeur téléphone", power:10, motor:false}
  ];

  var pvApps = [
    {name:"Ampoules LED", power:10, qty:5, hours:5, motor:false},
    {name:"Réfrigérateur", power:150, qty:1, hours:24, motor:true},
    {name:"Téléviseur", power:80, qty:1, hours:4, motor:false},
    {name:"Ventilateur", power:60, qty:2, hours:6, motor:true}
  ];

  var pvRowsEl = document.getElementById('pv-app-rows');
  var presetsEl = document.getElementById('pv-presets');

  PRESETS.forEach(function(p){
    var b = document.createElement('button');
    b.type = 'button';
    b.className = 'preset-btn';
    b.textContent = '+ ' + p.name;
    b.addEventListener('click', function(){
      pvApps.push({name:p.name, power:p.power, qty:1, hours:p.motor?6:4, motor:p.motor});
      renderPvRows(); recalcPV();
    });
    presetsEl.appendChild(b);
  });

  function renderPvRows(){
    pvRowsEl.innerHTML = '';
    pvApps.forEach(function(app, idx){
      var tr = document.createElement('tr');
      tr.innerHTML =
        '<td><input type="text" data-i="'+idx+'" data-f="name" value="'+app.name+'"></td>' +
        '<td><input type="number" data-i="'+idx+'" data-f="power" value="'+app.power+'" min="0"></td>' +
        '<td><input type="number" data-i="'+idx+'" data-f="qty" value="'+app.qty+'" min="0"></td>' +
        '<td><input type="number" data-i="'+idx+'" data-f="hours" value="'+app.hours+'" min="0" max="24" step="0.5"></td>' +
        '<td class="tech-only-cell"><input type="checkbox" data-i="'+idx+'" data-f="motor" '+(app.motor?'checked':'')+'></td>' +
        '<td><button class="rm-btn" data-i="'+idx+'" aria-label="Supprimer">✕</button></td>';
      pvRowsEl.appendChild(tr);
    });
  }

  pvRowsEl.addEventListener('input', function(e){
    var t = e.target; var i = t.getAttribute('data-i'), f = t.getAttribute('data-f');
    if (i === null) return; i = parseInt(i,10);
    if (f === 'name') pvApps[i].name = t.value;
    else if (f === 'motor') pvApps[i].motor = t.checked;
    else pvApps[i][f] = parseFloat(t.value) || 0;
    recalcPV();
  });
  pvRowsEl.addEventListener('click', function(e){
    if (e.target.classList.contains('rm-btn')){
      var i = parseInt(e.target.getAttribute('data-i'),10);
      pvApps.splice(i,1); renderPvRows(); recalcPV();
    }
  });
  document.getElementById('pv-add-row').addEventListener('click', function(){
    pvApps.push({name:"Nouvel appareil", power:0, qty:1, hours:1, motor:false});
    renderPvRows(); recalcPV();
  });

  /* ===================== PV calculation ===================== */
  function recalcPV(){
    var profile = mainEl.getAttribute('data-profile-mode');
    var billMode = document.getElementById('pv-tab-bill').classList.contains('active');

    var dailyWh, simulPower, peakPower;

    if (billMode){
      var kwh = val('pv-bill-kwh');
      dailyWh = (kwh * 1000) / 30.4;
      simulPower = val('pv-bill-peak');
      peakPower = simulPower * 2; // heuristic surge margin when no appliance breakdown
    } else {
      dailyWh = 0; simulPower = 0;
      var surgeMult = (profile === 'tech') ? (val('pv-surge-mult') || 3) : 3;
      var largestMotorNominal = 0, largestMotorSurgeExtra = 0;
      pvApps.forEach(function(a){
        dailyWh += a.power * a.qty * a.hours;
        simulPower += a.power * a.qty;
        if (a.motor){
          var nominal = a.power * a.qty;
          if (nominal > largestMotorNominal){
            largestMotorNominal = nominal;
          }
        }
      });
      // worst case: biggest motor starts while everything else already running
      peakPower = (simulPower - largestMotorNominal) + (largestMotorNominal * surgeMult);
      if (peakPower < simulPower) peakPower = simulPower;
    }

    var sun = parseFloat(sharedPSH.value) || 5.5;
    var autonomy = val('pv-autonomy') || 1;

    var ratio, panelW, voltage, dod;
    if (profile === 'tech'){
      ratio = val('pv-ratio') || 0.75;
      panelW = val('pv-panel-w') || 350;
      voltage = val('pv-voltage') || 24;
      dod = val('pv-battery-type') || 0.9;
    } else {
      ratio = 0.75; panelW = 350;
      voltage = simulPower <= 800 ? 12 : (simulPower <= 3000 ? 24 : 48);
      dod = 0.85;
    }

    var wcNeeded = sun > 0 ? dailyWh / (sun * ratio) : 0;
    var nbPanels = panelW > 0 ? Math.ceil(wcNeeded / panelW) : 0;
    var wcInstalled = nbPanels * panelW;
    var battWh = dailyWh * autonomy;
    var battAh = (voltage > 0 && dod > 0) ? battWh / (voltage * dod * 0.9) : 0;
    var unitAh = val('pv-batt-unit-ah') || 200;
    var nbBatteries = unitAh > 0 ? Math.ceil(battAh / unitAh) : 0;
    var invVA = peakPower * 1.1;
    var regA = (voltage > 0) ? (wcInstalled * 1.25) / voltage : 0;
    var annualProdKwh = (wcInstalled/1000) * sun * 365 * ratio;

    document.getElementById('r-pv-energy').innerHTML = fmt(dailyWh/1000) + ' <small>kWh</small>';
    document.getElementById('r-pv-power').innerHTML = fmtInt(simulPower) + ' <small>W</small>';
    document.getElementById('r-pv-panels').innerHTML = (nbPanels||0) + ' <small>x ' + panelW + ' Wc</small>';
    document.getElementById('r-pv-ah').innerHTML = fmtInt(battAh) + ' <small>Ah @ ' + voltage + 'V</small>';
    document.getElementById('r-pv-nbbatt').innerHTML = (nbBatteries||0) + ' <small>x ' + unitAh + ' Ah</small>';
    document.getElementById('r-pv-inv').innerHTML = fmtInt(invVA) + ' <small>VA</small>';
    document.getElementById('r-pv-peak').innerHTML = fmtInt(peakPower) + ' <small>W</small>';
    document.getElementById('r-pv-reg').innerHTML = fmt(regA,1) + ' <small>A</small>';

    // cables & protections (tech)
    var len1 = val('pv-len-pv-bat') || 10, len2 = val('pv-len-bat-inv') || 2;
    var drop = val('pv-drop') || 3;
    var arrayCurrent = voltage > 0 ? wcInstalled / voltage : 0;
    var battCurrent = voltage > 0 ? invVA / voltage : 0;
    var cable1 = cableSection(arrayCurrent, len1, voltage, drop);
    var cable2 = cableSection(battCurrent, len2, voltage, drop);
    document.getElementById('r-pv-cable1').innerHTML = cable1.label + ' <small>(' + fmt(arrayCurrent,1) + ' A, ' + len1 + ' m)</small>';
    document.getElementById('r-pv-cable2').innerHTML = cable2.label + ' <small>(' + fmt(battCurrent,1) + ' A, ' + len2 + ' m)</small>';
    var iscModule = val('pv-isc-module') || 10;
    document.getElementById('r-pv-fuse').textContent = breakerRating(iscModule);
    document.getElementById('r-pv-brk-dc').textContent = breakerRating(battCurrent);
    var acVoltage = val('pv-ac-voltage') || 230;
    var acCurrent = acVoltage > 0 ? invVA / acVoltage : 0;
    document.getElementById('r-pv-brk-ac').textContent = breakerRating(acCurrent);

    // chain
    document.getElementById('chain-pv-energy').textContent = fmt(dailyWh/1000,1);
    document.getElementById('chain-pv-wc').textContent = fmt(wcInstalled/1000,1);
    document.getElementById('chain-pv-batt').textContent = fmt(battWh/1000,1);
    document.getElementById('chain-pv-inv').textContent = fmtInt(invVA);
    document.getElementById('chain-pv-prod').textContent = fmt(annualProdKwh/1000,1);

    // KPIs: production, savings, ROI
    var currency = txt('shared-currency') || 'FCFA';
    var tariff = val('shared-tariff') || 0;
    var annualConsoKwh = (dailyWh/1000) * 365;
    var savedKwh = Math.min(annualProdKwh, annualConsoKwh);
    var annualSavings = savedKwh * tariff;
    document.getElementById('kpi-pv-prod').textContent = fmtInt(annualProdKwh) + ' kWh';
    document.getElementById('kpi-pv-save').textContent = fmtInt(annualSavings) + ' ' + currency;

    // devis
    var priceWc = val('dv-pv-p-panel'), priceAh = val('dv-pv-p-batt'), priceInv = val('dv-pv-p-inv'),
        priceReg = val('dv-pv-p-reg'), priceCable = val('dv-pv-p-cable'), priceStruct = val('dv-pv-p-struct'),
        priceLabor = val('dv-pv-p-labor'), priceStudy = val('dv-pv-p-study');
    var tPanel = nbPanels * priceWc, tBatt = nbBatteries * priceAh;
    var total = tPanel + tBatt + priceInv + priceReg + priceCable + priceStruct + priceLabor + priceStudy;
    document.getElementById('dv-pv-panel-w-label').textContent = panelW;
    document.getElementById('dv-pv-batt-ah-label').textContent = unitAh;
    document.getElementById('dv-pv-q-panel').textContent = (nbPanels||0) + ' u';
    document.getElementById('dv-pv-q-batt').textContent = (nbBatteries||0) + ' u';
    document.getElementById('dv-pv-t-panel').textContent = fmtInt(tPanel);
    document.getElementById('dv-pv-t-batt').textContent = fmtInt(tBatt);
    document.getElementById('dv-pv-t-inv').textContent = fmtInt(priceInv);
    document.getElementById('dv-pv-t-reg').textContent = fmtInt(priceReg);
    document.getElementById('dv-pv-t-cable').textContent = fmtInt(priceCable);
    document.getElementById('dv-pv-t-struct').textContent = fmtInt(priceStruct);
    document.getElementById('dv-pv-t-labor').textContent = fmtInt(priceLabor);
    document.getElementById('dv-pv-t-study').textContent = fmtInt(priceStudy);
    document.getElementById('dv-pv-total').textContent = fmtInt(total) + ' ' + currency;

    var roiEl = document.getElementById('kpi-pv-roi');
    if (total > 0 && annualSavings > 0){
      roiEl.textContent = fmt(total/annualSavings,1) + ' ans';
    } else {
      roiEl.textContent = '—';
    }
  }

  /* PV listeners */
  ['pv-autonomy','pv-batt-unit-ah','pv-voltage','pv-battery-type','pv-ratio','pv-panel-w','pv-surge-mult',
   'pv-bill-kwh','pv-bill-amount','pv-bill-peak',
   'pv-len-pv-bat','pv-len-bat-inv','pv-drop','pv-isc-module','pv-ac-voltage',
   'dv-pv-p-panel','dv-pv-p-batt','dv-pv-p-inv','dv-pv-p-reg','dv-pv-p-cable','dv-pv-p-struct','dv-pv-p-labor','dv-pv-p-study'
  ].forEach(function(id){ var el = document.getElementById(id); if (el) el.addEventListener('input', recalcPV); });

  /* ===================== Pumping calculation ===================== */
  function recalcPump(){
    var profile = mainEl.getAttribute('data-profile-mode');
    var volume = val('pp-volume');
    var hmt = (profile === 'tech') ? (val('pp-hstat') + val('pp-hperte')) : val('pp-hmt');

    var sun = parseFloat(sharedPSH.value) || 5.5;
    var effPump, effConv, ratio, panelW;
    if (profile === 'tech'){
      effPump = val('pp-eff-pump') || 0.45;
      effConv = val('pp-eff-conv') || 0.9;
      ratio = val('pp-ratio') || 0.75;
      panelW = val('pp-panel-w') || 350;
    } else {
      effPump = 0.45; effConv = 0.9; ratio = 0.75; panelW = 350;
    }

    var hydraulicWh = 2.725 * volume * hmt;
    var elecWh = (effPump > 0 && effConv > 0) ? hydraulicWh / (effPump * effConv) : 0;
    var wcNeeded = sun > 0 ? elecWh / (sun * ratio) : 0;
    var nbPanels = panelW > 0 ? Math.ceil(wcNeeded / panelW) : 0;
    var wcInstalled = nbPanels * panelW;
    var pumpW = sun > 0 ? elecWh / sun : 0;
    var annualVolume = volume * 365;

    document.getElementById('r-pp-hyd').innerHTML = fmt(hydraulicWh/1000) + ' <small>kWh</small>';
    document.getElementById('r-pp-elec').innerHTML = fmt(elecWh/1000) + ' <small>kWh</small>';
    document.getElementById('r-pp-wc').innerHTML = fmt(wcInstalled/1000,2) + ' <small>kWc</small>';
    document.getElementById('r-pp-panels').innerHTML = (nbPanels||0) + ' <small>x ' + panelW + ' Wc</small>';
    document.getElementById('r-pp-pump').innerHTML = fmtInt(pumpW) + ' <small>W</small>';
    document.getElementById('r-pp-hmt').innerHTML = fmt(hmt,0) + ' <small>m</small>';

    var len = val('pp-len') || 15, drop = val('pp-drop') || 3, voltP = val('pp-volt') || 96;
    var current = voltP > 0 ? pumpW / voltP : 0;
    var cable = cableSection(current, len, voltP, drop);
    document.getElementById('r-pp-cable').innerHTML = cable.label + ' <small>(' + fmt(current,1) + ' A, ' + len + ' m)</small>';
    document.getElementById('r-pp-brk').textContent = breakerRating(current);

    document.getElementById('chain-pp-hyd').textContent = fmt(hydraulicWh/1000,1);
    document.getElementById('chain-pp-elec').textContent = fmt(elecWh/1000,1);
    document.getElementById('chain-pp-wc').textContent = fmt(wcInstalled/1000,2);
    document.getElementById('chain-pp-pump').textContent = fmtInt(pumpW);
    document.getElementById('chain-pp-prod').textContent = fmtInt(annualVolume);

    var currency = txt('shared-currency') || 'FCFA';
    var dieselCost = val('pp-diesel-cost');
    var annualSavings = dieselCost * 12;
    document.getElementById('kpi-pp-prod').textContent = fmtInt(annualVolume) + ' m³';
    document.getElementById('kpi-pp-save').textContent = fmtInt(annualSavings) + ' ' + currency;

    var priceWc = val('dv-pp-p-panel'), pricePump = val('dv-pp-p-pump'), priceCable = val('dv-pp-p-cable'),
        priceInfra = val('dv-pp-p-infra'), priceStruct = val('dv-pp-p-struct'), priceLabor = val('dv-pp-p-labor');
    var tPanel = nbPanels * priceWc;
    var total = tPanel + pricePump + priceCable + priceInfra + priceStruct + priceLabor;
    document.getElementById('dv-pp-panel-w-label').textContent = panelW;
    document.getElementById('dv-pp-q-panel').textContent = (nbPanels||0) + ' u';
    document.getElementById('dv-pp-t-panel').textContent = fmtInt(tPanel);
    document.getElementById('dv-pp-t-pump').textContent = fmtInt(pricePump);
    document.getElementById('dv-pp-t-cable').textContent = fmtInt(priceCable);
    document.getElementById('dv-pp-t-infra').textContent = fmtInt(priceInfra);
    document.getElementById('dv-pp-t-struct').textContent = fmtInt(priceStruct);
    document.getElementById('dv-pp-t-labor').textContent = fmtInt(priceLabor);
    document.getElementById('dv-pp-total').textContent = fmtInt(total) + ' ' + currency;

    var roiEl = document.getElementById('kpi-pp-roi');
    if (total > 0 && annualSavings > 0){ roiEl.textContent = fmt(total/annualSavings,1) + ' ans'; }
    else { roiEl.textContent = '—'; }
  }

  ['pp-volume','pp-hmt','pp-hstat','pp-hperte','pp-eff-pump','pp-eff-conv','pp-ratio','pp-panel-w','pp-diesel-cost',
   'pp-len','pp-drop','pp-volt',
   'dv-pp-p-panel','dv-pp-p-pump','dv-pp-p-cable','dv-pp-p-infra','dv-pp-p-struct','dv-pp-p-labor'
  ].forEach(function(id){ var el = document.getElementById(id); if (el) el.addEventListener('input', recalcPump); });

  /* ===================== Devis export / share ===================== */
  function collectDevisRows(prefix){
    var rows = [];
    document.querySelectorAll('#panel-' + (prefix === 'pv' ? 'pv' : 'pump') + ' .devis-table tbody tr').forEach(function(tr){
      if (tr.classList.contains('devis-total-row')) return;
      var label = tr.children[0].textContent.trim();
      var qtyCell = tr.children[1];
      var qty = qtyCell.textContent.trim();
      var priceInput = tr.children[2].querySelector('input');
      var price = priceInput ? priceInput.value : '';
      var totalCell = tr.children[3];
      var totalTxt = totalCell.textContent.trim();
      rows.push({label:label, qty:qty, price:price, total:totalTxt});
    });
    return rows;
  }

  function buildDevisDoc(prefix){
    var client = txt('dv-' + prefix + '-client') || 'Client';
    var currency = txt('shared-currency') || 'FCFA';
    var rows = collectDevisRows(prefix);
    var total = document.getElementById('dv-' + prefix + '-total').textContent;
    var today = new Date().toLocaleDateString('fr-FR');
    var modeLabel = prefix === 'pv' ? 'Installation photovoltaïque' : 'Pompage solaire';

    var prodKpi = prefix === 'pv' ? document.getElementById('kpi-pv-prod').textContent : document.getElementById('kpi-pp-prod').textContent;
    var saveKpi = prefix === 'pv' ? document.getElementById('kpi-pv-save').textContent : document.getElementById('kpi-pp-save').textContent;
    var roiKpi = prefix === 'pv' ? document.getElementById('kpi-pv-roi').textContent : document.getElementById('kpi-pp-roi').textContent;

    var rowsHtml = rows.map(function(r){
      return '<tr><td>'+r.label+'</td><td style="text-align:center">'+r.qty+'</td><td style="text-align:right">'+r.price+'</td><td style="text-align:right">'+r.total+'</td></tr>';
    }).join('');

    var html = '<!DOCTYPE html><html lang="fr"><head><meta charset="UTF-8"><title>Devis KELEN SOLAR — ' + client + '</title>' +
      '<style>' +
      'body{font-family:Arial,sans-serif;color:#2A2118;padding:30px;max-width:760px;margin:0 auto;}' +
      '.head{display:flex;justify-content:space-between;align-items:flex-start;border-bottom:4px solid #1F5C3F;padding-bottom:14px;margin-bottom:20px;}' +
      '.brand{font-size:1.5rem;font-weight:bold;color:#1F5C3F;} .brand span{color:#F2A93B;}' +
      '.tag{font-size:0.78rem;letter-spacing:0.12em;text-transform:uppercase;color:#897a5c;}' +
      '.meta{text-align:right;font-size:0.85rem;color:#5d5038;}' +
      'h2{font-size:1.05rem;color:#1F5C3F;border-left:4px solid #F2A93B;padding-left:8px;margin-top:26px;}' +
      'table{width:100%;border-collapse:collapse;margin-top:10px;font-size:0.88rem;}' +
      'th{text-align:left;background:#F2E9D8;padding:7px 8px;font-size:0.72rem;text-transform:uppercase;color:#5d5038;}' +
      'td{padding:7px 8px;border-bottom:1px solid #E7D9BD;}' +
      '.total-row td{font-weight:bold;font-size:1.05rem;border-top:2px solid #1F5C3F;border-bottom:none;}' +
      '.kpis{display:flex;gap:12px;margin-top:18px;}' +
      '.kpi{flex:1;background:#1F5C3F;color:#fff;border-radius:8px;padding:12px;text-align:center;}' +
      '.kpi b{display:block;font-size:1.1rem;}' +
      '.kpi span{font-size:0.68rem;text-transform:uppercase;opacity:.85;}' +
      'footer{margin-top:30px;font-size:0.72rem;color:#897a5c;border-top:1px solid #E7D9BD;padding-top:12px;}' +
      '@media print{ body{padding:10px;} }' +
      '</style></head><body>' +
      '<div class="head"><div><div class="brand">KELEN <span>SOLAR</span></div><div class="tag">La solution solaire</div></div>' +
      '<div class="meta">Date : ' + today + '<br>Devis : ' + modeLabel + '<br>Client : ' + client + '</div></div>' +
      '<h2>Détail du devis</h2>' +
      '<table><thead><tr><th>Poste</th><th>Qté</th><th>Prix unitaire</th><th>Total</th></tr></thead><tbody>' +
      rowsHtml +
      '<tr class="total-row"><td colspan="3">TOTAL ESTIMÉ</td><td>' + total + '</td></tr>' +
      '</tbody></table>' +
      '<div class="kpis">' +
        '<div class="kpi"><b>' + prodKpi + '</b><span>' + (prefix==='pv' ? 'Production annuelle' : 'Eau pompée / an') + '</span></div>' +
        '<div class="kpi"><b>' + saveKpi + '</b><span>Économie annuelle</span></div>' +
        '<div class="kpi"><b>' + roiKpi + '</b><span>Retour sur investissement</span></div>' +
      '</div>' +
      '<footer>Devis indicatif généré par l\'outil de dimensionnement KELEN SOLAR. Les montants ci-dessus sont des estimations basées sur les hypothèses saisies et doivent être confirmés avant signature. Contact : kelensolar@gmail.com — WhatsApp/Tél : +226 44 04 18 17.</footer>' +
      '</body></html>';
    return {html:html, total:total, client:client, currency:currency, rows:rows, modeLabel:modeLabel};
  }

  function buildDevisText(prefix){
    var d = buildDevisDoc(prefix);
    var lines = ['*KELEN SOLAR — La solution solaire*', d.modeLabel + ' — ' + d.client, ''];
    d.rows.forEach(function(r){ lines.push('• ' + r.label + ' x' + r.qty + ' : ' + r.total); });
    lines.push('');
    lines.push('TOTAL ESTIMÉ : ' + d.total);
    lines.push('');
    lines.push('Contact : kelensolar@gmail.com — +226 44 04 18 17');
    return lines.join('\n');
  }

  function setupDevisExport(prefix){
    var printBtn = document.getElementById(prefix + '-devis-print');
    var waBtn = document.getElementById(prefix + '-devis-whatsapp');
    var mailBtn = document.getElementById(prefix + '-devis-email');

    if (printBtn) printBtn.addEventListener('click', function(){
      var d = buildDevisDoc(prefix);
      var w = window.open('', '_blank');
      if (!w){ alert('Veuillez autoriser les fenêtres pop-up pour générer le PDF.'); return; }
      w.document.open(); w.document.write(d.html); w.document.close();
      setTimeout(function(){ w.focus(); w.print(); }, 350);
    });

    if (waBtn) waBtn.addEventListener('click', function(){
      var text = buildDevisText(prefix);
      window.open('https://wa.me/?text=' + encodeURIComponent(text), '_blank');
    });

    if (mailBtn) mailBtn.addEventListener('click', function(){
      var d = buildDevisDoc(prefix);
      var subject = 'Devis KELEN SOLAR — ' + d.modeLabel + ' — ' + d.client;
      var body = buildDevisText(prefix);
      window.location.href = 'mailto:?subject=' + encodeURIComponent(subject) + '&body=' + encodeURIComponent(body);
    });
  }

  setupDevisExport('pv');
  setupDevisExport('pp');

  /* ---------- init ---------- */
  renderPvRows();
  recalcPV();
  recalcPump();
})();
</script>
</body>
</htm
Outil de dimensionnement 
