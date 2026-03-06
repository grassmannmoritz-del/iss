<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="apple-mobile-web-app-title" content="iss">
  <title>iss tracker</title>
  <link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Syne:wght@300;400;700&display=swap" rel="stylesheet">
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #030508;
      --surface: rgba(255,255,255,0.03);
      --border: rgba(255,255,255,0.06);
      --text: #e8edf5;
      --muted: rgba(232,237,245,0.35);
      --accent: #4fc3f7;
      --accent-dim: rgba(79,195,247,0.15);
      --green: #69f0ae;
    }

    html, body {
      height: 100%;
      background: var(--bg);
      color: var(--text);
      font-family: 'Syne', sans-serif;
      overflow: hidden;
    }

    /* Starfield */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image:
        radial-gradient(1px 1px at 10% 20%, rgba(255,255,255,0.6) 0%, transparent 100%),
        radial-gradient(1px 1px at 30% 60%, rgba(255,255,255,0.4) 0%, transparent 100%),
        radial-gradient(1px 1px at 50% 10%, rgba(255,255,255,0.5) 0%, transparent 100%),
        radial-gradient(1px 1px at 70% 80%, rgba(255,255,255,0.3) 0%, transparent 100%),
        radial-gradient(1px 1px at 85% 35%, rgba(255,255,255,0.6) 0%, transparent 100%),
        radial-gradient(1px 1px at 15% 85%, rgba(255,255,255,0.4) 0%, transparent 100%),
        radial-gradient(1px 1px at 60% 45%, rgba(255,255,255,0.3) 0%, transparent 100%),
        radial-gradient(1px 1px at 90% 65%, rgba(255,255,255,0.5) 0%, transparent 100%),
        radial-gradient(1px 1px at 25% 40%, rgba(255,255,255,0.4) 0%, transparent 100%),
        radial-gradient(1px 1px at 75% 15%, rgba(255,255,255,0.6) 0%, transparent 100%),
        radial-gradient(1px 1px at 40% 75%, rgba(255,255,255,0.3) 0%, transparent 100%),
        radial-gradient(1px 1px at 55% 90%, rgba(255,255,255,0.5) 0%, transparent 100%),
        radial-gradient(1px 1px at 5% 50%, rgba(255,255,255,0.4) 0%, transparent 100%),
        radial-gradient(1px 1px at 95% 25%, rgba(255,255,255,0.3) 0%, transparent 100%),
        radial-gradient(1px 1px at 45% 30%, rgba(255,255,255,0.6) 0%, transparent 100%);
      pointer-events: none;
      z-index: 0;
    }

    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100dvh;
      padding: env(safe-area-inset-top, 20px) 24px env(safe-area-inset-bottom, 20px);
      position: relative;
    }

    .wrapper {
      position: relative;
      z-index: 1;
      width: 100%;
      max-width: 380px;
      display: flex;
      flex-direction: column;
      gap: 16px;
    }

    /* Header */
    .header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 4px;
    }

    .title {
      font-family: 'Share Tech Mono', monospace;
      font-size: 11px;
      letter-spacing: 0.3em;
      text-transform: uppercase;
      color: var(--accent);
    }

    .live-badge {
      display: flex;
      align-items: center;
      gap: 6px;
      font-family: 'Share Tech Mono', monospace;
      font-size: 10px;
      letter-spacing: 0.2em;
      color: var(--green);
    }

    .live-dot {
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: var(--green);
      animation: livepulse 1s ease-in-out infinite;
    }

    @keyframes livepulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.4; transform: scale(0.8); }
    }

    /* ISS Icon card */
    .iss-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 32px 24px;
      text-align: center;
      position: relative;
      overflow: hidden;
    }

    .iss-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 1px;
      background: linear-gradient(90deg, transparent, var(--accent), transparent);
      opacity: 0.4;
    }

    .iss-icon {
      font-size: 48px;
      display: block;
      margin-bottom: 20px;
      animation: orbit 8s linear infinite;
      filter: drop-shadow(0 0 12px rgba(79,195,247,0.5));
    }

    @keyframes orbit {
      0% { transform: translateX(-4px) rotate(-3deg); }
      25% { transform: translateX(0px) rotate(0deg); }
      50% { transform: translateX(4px) rotate(3deg); }
      75% { transform: translateX(0px) rotate(0deg); }
      100% { transform: translateX(-4px) rotate(-3deg); }
    }

    .coords {
      display: flex;
      justify-content: center;
      gap: 24px;
      margin-bottom: 16px;
    }

    .coord-item {
      text-align: center;
    }

    .coord-label {
      font-family: 'Share Tech Mono', monospace;
      font-size: 9px;
      letter-spacing: 0.2em;
      color: var(--muted);
      text-transform: uppercase;
      margin-bottom: 4px;
    }

    .coord-value {
      font-family: 'Share Tech Mono', monospace;
      font-size: 22px;
      color: var(--accent);
      letter-spacing: 0.05em;
      transition: all 0.3s ease;
    }

    .coord-unit {
      font-size: 11px;
      color: var(--muted);
    }

    .region {
      font-family: 'Share Tech Mono', monospace;
      font-size: 11px;
      color: var(--muted);
      letter-spacing: 0.1em;
      min-height: 16px;
    }

    /* Stats grid */
    .stats-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    .stat-tile {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 16px;
    }

    .stat-label {
      font-family: 'Share Tech Mono', monospace;
      font-size: 9px;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--muted);
      margin-bottom: 8px;
    }

    .stat-value {
      font-family: 'Share Tech Mono', monospace;
      font-size: 20px;
      color: var(--text);
      letter-spacing: 0.05em;
    }

    .stat-value.highlight {
      color: var(--green);
    }

    .stat-sub {
      font-size: 10px;
      color: var(--muted);
      margin-top: 2px;
      font-family: 'Share Tech Mono', monospace;
    }

    /* Astronauts */
    .astronaut-tile {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 16px 20px;
    }

    .astronaut-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 12px;
    }

    .astronaut-label {
      font-family: 'Share Tech Mono', monospace;
      font-size: 9px;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--muted);
    }

    .astronaut-count {
      font-family: 'Share Tech Mono', monospace;
      font-size: 14px;
      color: var(--accent);
    }

    .astronaut-list {
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .astronaut-item {
      font-family: 'Share Tech Mono', monospace;
      font-size: 11px;
      color: var(--muted);
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .astronaut-item::before {
      content: '›';
      color: var(--accent);
      font-size: 14px;
    }

    /* Footer */
    .footer {
      text-align: center;
      font-family: 'Share Tech Mono', monospace;
      font-size: 10px;
      color: rgba(255,255,255,0.15);
      letter-spacing: 0.1em;
      padding: 0 4px;
    }

    /* Loading state */
    .loading {
      animation: loadpulse 1.2s ease-in-out infinite;
    }

    @keyframes loadpulse {
      0%, 100% { opacity: 0.3; }
      50% { opacity: 0.8; }
    }

    /* Fade in */
    .fade-in {
      animation: fadeIn 0.6s ease forwards;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(8px); }
      to { opacity: 1; transform: translateY(0); }
    }

    /* Update flash */
    .flash {
      animation: flash 0.3s ease;
    }

    @keyframes flash {
      0% { opacity: 1; }
      50% { opacity: 0.4; }
      100% { opacity: 1; }
    }
  </style>
</head>
<body>

<div class="wrapper">

  <div class="header fade-in">
    <div class="title">ISS · Tracker</div>
    <div class="live-badge">
      <div class="live-dot"></div>
      LIVE
    </div>
  </div>

  <!-- Main ISS card -->
  <div class="iss-card fade-in">
    <span class="iss-icon">🛸</span>
    <div class="coords">
      <div class="coord-item">
        <div class="coord-label">Breitengrad</div>
        <div class="coord-value loading" id="lat">···</div>
      </div>
      <div class="coord-item">
        <div class="coord-label">Längengrad</div>
        <div class="coord-value loading" id="lng">···</div>
      </div>
    </div>
    <div class="region loading" id="region">Standort wird geladen…</div>
  </div>

  <!-- Stats -->
  <div class="stats-grid fade-in">
    <div class="stat-tile">
      <div class="stat-label">Geschwindigkeit</div>
      <div class="stat-value highlight">27.600</div>
      <div class="stat-sub">km/h</div>
    </div>
    <div class="stat-tile">
      <div class="stat-label">Höhe</div>
      <div class="stat-value">408</div>
      <div class="stat-sub">km über Erde</div>
    </div>
    <div class="stat-tile">
      <div class="stat-label">Umlaufzeit</div>
      <div class="stat-value">92</div>
      <div class="stat-sub">min pro Orbit</div>
    </div>
    <div class="stat-tile">
      <div class="stat-label">Aktualisiert</div>
      <div class="stat-value" id="lastUpdate" style="font-size:16px;">···</div>
      <div class="stat-sub">Uhr</div>
    </div>
  </div>

  <!-- Astronauts -->
  <div class="astronaut-tile fade-in" id="astronautTile">
    <div class="astronaut-header">
      <div class="astronaut-label">Besatzung an Bord</div>
      <div class="astronaut-count" id="astronautCount">···</div>
    </div>
    <div class="astronaut-list" id="astronautList">
      <div class="astronaut-item loading">Wird geladen…</div>
    </div>
  </div>

  <div class="footer fade-in">
    DATEN: OPEN-NOTIFY.ORG · ISS POSITION API
  </div>

</div>

<script>
  let astronautsLoaded = false;

  async function fetchISS() {
    try {
      const res = await fetch('https://api.open-notify.org/iss-now.json');
      const data = await res.json();

      const lat = parseFloat(data.iss_position.latitude).toFixed(2);
      const lng = parseFloat(data.iss_position.longitude).toFixed(2);

      const latEl = document.getElementById('lat');
      const lngEl = document.getElementById('lng');

      latEl.textContent = lat + '°';
      lngEl.textContent = lng + '°';
      latEl.className = 'coord-value flash';
      lngEl.className = 'coord-value flash';
      setTimeout(() => {
        latEl.className = 'coord-value';
        lngEl.className = 'coord-value';
      }, 300);

      // Region beschreiben
      const latN = parseFloat(lat);
      const lngN = parseFloat(lng);
      let region = '';
      if (latN > 60) region = 'Arktis';
      else if (latN < -60) region = 'Antarktis';
      else if (lngN > -30 && lngN < 60) region = latN > 0 ? 'Europa / Afrika' : 'Afrika / Indischer Ozean';
      else if (lngN >= 60 && lngN < 150) region = latN > 0 ? 'Asien' : 'Australien / Pazifik';
      else region = latN > 0 ? 'Nordamerika / Atlantik' : 'Südamerika / Pazifik';

      document.getElementById('region').textContent = '📍 ' + region;
      document.getElementById('region').className = 'region';

      // Uhrzeit
      const now = new Date();
      document.getElementById('lastUpdate').textContent =
        now.toLocaleTimeString('de-DE', { hour: '2-digit', minute: '2-digit', second: '2-digit' });

    } catch (err) {
      document.getElementById('region').textContent = 'Verbindungsfehler';
    }
  }

  async function fetchAstronauts() {
    try {
      const res = await fetch('https://api.open-notify.org/astros.json');
      const data = await res.json();

      // Nur ISS Besatzung
      const issCreq = data.people.filter(p => p.craft === 'ISS');
      document.getElementById('astronautCount').textContent = issCreq.length + ' Personen';

      const list = document.getElementById('astronautList');
      list.innerHTML = '';
      issCreq.forEach(p => {
        const div = document.createElement('div');
        div.className = 'astronaut-item';
        div.textContent = p.name;
        list.appendChild(div);
      });
    } catch (err) {
      document.getElementById('astronautCount').textContent = '–';
    }
  }

  // Start
  fetchISS();
  fetchAstronauts();

  // ISS Position jede Sekunde
  setInterval(fetchISS, 1000);

  // Astronauten nur alle 5 Minuten (ändert sich selten)
  setInterval(fetchAstronauts, 5 * 60 * 1000);
</script>
</body>
</html>
