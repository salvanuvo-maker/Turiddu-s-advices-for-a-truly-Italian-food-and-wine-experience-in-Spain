<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Turiddu's advices for a truly Italian food and wine experience in Spain</title>
  <meta name="description" content="Discover the best Italian restaurants, pizzerias, and wine experiences in Spain with Turiddu's personal recommendations." />
  <meta name="theme-color" content="#ef4444" />
  <!-- Favicon SVG minimal -->
  <link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 128 128'%3E%3Cdefs%3E%3ClinearGradient id='g' x1='0' x2='1' y1='0' y2='1'%3E%3Cstop offset='0' stop-color='%23f97316'/%3E%3Cstop offset='1' stop-color='%23ef4444'/%3E%3C/linearGradient%3E%3C/defs%3E%3Crect width='128' height='128' rx='28' fill='url(%23g)'/%3E%3Ctext x='64' y='78' font-size='52' text-anchor='middle' fill='%23fff' font-family='system-ui,sans-serif'%3EIT%3C/text%3E%3C/svg%3E" />

  <style>
    :root { --bg:#fafafa; --card:#fff; --ink:#111; --muted:#6b7280; --brand:#ef4444; --line:#e5e7eb; --radius:16px; }
    *{box-sizing:border-box}
    body{margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,Cantarell,Noto Sans,sans-serif;background:var(--bg);color:var(--ink)}
    .container{max-width:1200px;margin:auto;padding:16px}
    header{display:flex;align-items:center;gap:12px;margin:8px 0 16px}
    .logo{width:40px;height:40px;border-radius:10px;background:linear-gradient(135deg,#f97316,#ef4444);display:grid;place-items:center;color:#fff;font-weight:700}
    h1{font-size:18px;margin:0;line-height:1.2}
    .grid{display:grid;grid-template-columns:1fr;gap:16px}
    @media(min-width:900px){.grid{grid-template-columns:420px 1fr}}
    .card{background:var(--card);border:1px solid var(--line);border-radius:var(--radius);box-shadow:0 1px 2px rgba(0,0,0,.04)}
    .card h2{font-size:18px;margin:0}
    .card .body{padding:16px}
    .controls{display:grid;gap:12px}
    .row{display:grid;grid-template-columns:1fr auto;align-items:center;gap:12px}
    label{font-size:14px;color:var(--muted)}
    input[type="text"],select{width:100%;padding:10px 12px;border:1px solid var(--line);border-radius:10px;font:inherit}
    .chips{display:flex;flex-wrap:wrap;gap:8px}
    .chip{padding:6px 10px;border:1px solid var(--line);border-radius:999px;background:#f6f6f6;cursor:pointer}
    .chip[aria-pressed="true"]{background:var(--brand);color:#fff;border-color:var(--brand)}
    .muted{color:var(--muted)}
    .results{display:flex;flex-direction:column;gap:10px}
    .item{display:flex;gap:12px;border:1px solid var(--line);border-radius:12px;padding:12px}
    .item a{color:inherit;text-decoration:none}
    .item a:hover{text-decoration:underline}
    .badges{display:flex;flex-wrap:wrap;gap:6px}
    .badge{font-size:12px;border:1px solid var(--line);padding:2px 8px;border-radius:999px;background:#f9fafb}
    .stars{display:inline-flex;gap:2px}
    .star{width:16px;height:16px;display:inline-block}
    .map{height:70vh;border-radius:var(--radius);overflow:hidden}
    .skip{position:absolute;left:-9999px;top:auto;width:1px;height:1px;overflow:hidden}
    .skip:focus{position:fixed;left:12px;top:12px;background:#fff;padding:8px 10px;border-radius:8px;border:1px solid var(--line);box-shadow:0 2px 6px rgba(0,0,0,.12)}
    footer{font-size:12px;color:var(--muted);margin-top:12px}
  </style>

  <!-- Leaflet -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
</head>
<body>
  <a href="#results" class="skip">Skip to results list</a>
  <div class="container">
    <header>
      <div class="logo" aria-hidden="true">IT</div>
      <h1>Turiddu's advices for a truly Italian food and wine experience in Spain</h1>
    </header>

    <main class="grid" aria-label="Italian restaurants finder" role="main">
      <!-- Filtros y resultados -->
      <section class="card" aria-label="Filters and results" role="region">
        <div class="body controls">
          <div>
            <label for="q">Search by name or city</label>
            <input id="q" type="text" placeholder="e.g., NAP Barcelona" aria-label="Search by name or city" />
          </div>

          <div>
            <span class="muted" id="cats-label">Categories</span>
            <div class="chips" role="group" aria-labelledby="cats-label">
              <button class="chip" data-cat="pizzería" aria-pressed="false">Pizzeria</button>
              <button class="chip" data-cat="ristorante-pizzería" aria-pressed="false">Ristorante-pizzeria</button>
              <button class="chip" data-cat="ristorante" aria-pressed="false">Ristorante</button>
            </div>
          </div>

          <div class="row" role="group" aria-label="Search radius in kilometers">
            <label for="radius">Radius (km)</label>
            <output id="radiusOut">50</output>
            <input id="radius" type="range" min="5" max="150" step="5" value="50" aria-label="Radius in kilometers" />
          </div>

          <div class="row">
            <label for="order">Sort by</label>
            <select id="order" aria-label="Sort results">
              <option value="score">Best score</option>
              <option value="distance">Closest</option>
            </select>
          </div>

          <fieldset>
            <legend class="muted">Source weighting</legend>
            <div class="row">
              <label for="wg">Google <span id="wgVal">50</span>%</label>
              <input id="wg" type="range" min="0" max="100" step="5" value="50" />
            </div>
            <div class="row">
              <label for="wt">Tripadvisor <span id="wtVal">30</span>%</label>
              <input id="wt" type="range" min="0" max="100" step="5" value="30" />
            </div>
            <div class="row">
              <label for="wf">TheFork <span id="wfVal">20</span>%</label>
              <input id="wf" type="range" min="0" max="100" step="5" value="20" />
            </div>
            <p class="muted">Ratings are normalized to 0–5 (TheFork 0–10 → 0–5).</p>
          </fieldset>

          <h2 id="title-results" style="margin-top:8px">Results (<span id="count">0</span>)</h2>
          <div id="results" class="results" role="list" aria-labelledby="title-results"></div>
        </div>
      </section>

      <!-- Mapa -->
      <section class="card" aria-label="Map" role="region">
        <div class="body">
          <div id="map" class="map" aria-label="Restaurants map"></div>
        </div>
      </section>
    </main>

    <footer>
      <p><strong>Note:</strong> Demo with static data. For production, connect your own API consolidating ratings from Google, Tripadvisor and TheFork (respect their TOS).</p>
    </footer>
  </div>

  <script>
    // === Datos de ejemplo (cámbialos por tu API) ===
    const DATA = [
      { id:"1", name:"NAP Neapolitan Authentic Pizza", city:"Barcelona", category:"pizzería", coords:{lat:41.3925,lng:2.1699}, ratings:{google:4.6,tripadvisor:4.5,thefork:9.0}, price:"€€", url:"https://nap.pizza" },
      { id:"2", name:"Trattoria Popolare", city:"Madrid", category:"ristorante-pizzería", coords:{lat:40.421,lng:-3.704}, ratings:{google:4.5,tripadvisor:4.0,thefork:8.5}, price:"€€", url:"#"},
      { id:"3", name:"Da Enzo Ristorante", city:"València", category:"ristorante", coords:{lat:39.47,lng:-0.376}, ratings:{google:4.7,tripadvisor:4.5,thefork:9.2}, price:"€€€", url:"#"},
      { id:"4", name:"Fratelli Figurato", city:"Madrid", category:"pizzería", coords:{lat:40.4339,lng:-3.7058}, ratings:{google:4.8,tripadvisor:4.5,thefork:9.4}, price:"€€", url:"#"},
      { id:"5", name:"La Balmesina", city:"Barcelona", category:"ristorante-pizzería", coords:{lat:41.3983,lng:2.149}, ratings:{google:4.6,tripadvisor:4.5,thefork:8.9}, price:"€€€", url:"#"},
    ];

    // === Utilidades ===
    const toRad = d => d*Math.PI/180;
    function haversineKm(a,b){
      const R=6371, dLat=toRad(b.lat-a.lat), dLon=toRad(b.lng-a.lng);
      const lat1=toRad(a.lat), lat2=toRad(b.lat);
      const x=Math.sin(dLat/2)**2 + Math.cos(lat1)*Math.cos(lat2)*Math.sin(dLon/2)**2;
      return 2*R*Math.atan2(Math.sqrt(x),Math.sqrt(1-x));
    }
    const normFork = v => typeof v === 'number' ? (v/10)*5 : undefined;
    function weightedScore(r, w){
      const g=r.google, t=r.tripadvisor, f=normFork(r.thefork);
      const pairs=[[g,w.g],[t,w.t],[f,w.f]].filter(x=>typeof x[0]==='number');
      if(!pairs.length) return 0;
      const tw=pairs.reduce((s,[,ww])=>s+ww,0);
      const s=pairs.reduce((s,[v,ww])=>s+v*ww,0)/tw;
      return Math.round(s*100)/100;
    }
    function normalizeWeights(w){
      const total=w.g+w.t+w.f; if(total===0) return {g:0.5,t:0.3,f:0.2};
      return {g:w.g/total,t:w.t/total,f:w.f/total};
    }

    // === Estado ===
    let userPos = { lat: 40.4168, lng: -3.7038 }; // Madrid por defecto
    let weights = { g:0.5, t:0.3, f:0.2 };
    let radiusKm = 50;
    let category = 'todas';
    let query = '';
    let order = 'score';

    // === Geolocalización ===
    if ('geolocation' in navigator) {
      navigator.geolocation.getCurrentPosition(p => {
        userPos = { lat:p.coords.latitude, lng:p.coords.longitude };
        map.setView([userPos.lat,userPos.lng], 12);
        userMarker.setLatLng([userPos.lat,userPos.lng]);
        render();
      }, () => {}, { enableHighAccuracy:true, timeout:10000, maximumAge:60000 });
    }

    // === Mapa (Leaflet) ===
    const map = L.map('map').setView([userPos.lat,userPos.lng], 12);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { attribution: '&copy; OpenStreetMap contrib.' }).addTo(map);
    const userMarker = L.marker([userPos.lat,userPos.lng]).addTo(map).bindPopup('Your approximate location');
    let restMarkers = [];

    // === DOM refs ===
    const elQ = document.getElementById('q');
    const elCount = document.getElementById('count');
    const elRes = document.getElementById('results');
    const elRadius = document.getElementById('radius');
    const elRadiusOut = document.getElementById('radiusOut');
    const elOrder = document.getElementById('order');
    const chipButtons = Array.from(document.querySelectorAll('.chip'));
    const wg = document.getElementById('wg'), wt = document.getElementById('wt'), wf = document.getElementById('wf');
    const wgVal = document.getElementById('wgVal'), wtVal = document.getElementById('wtVal'), wfVal = document.getElementById('wfVal');

    // Accesibilidad: atajo '/'
    window.addEventListener('keydown', e => { if (e.key==='/' && document.activeElement.tagName!=='INPUT') { e.preventDefault(); elQ.focus(); }});

    // Listeners
    elQ.addEventListener('input', e => { query = e.target.value.toLowerCase(); render(); });
    elRadius.addEventListener('input', e => { radiusKm = +e.target.value; elRadiusOut.textContent = radiusKm; render(); });
    elOrder.addEventListener('change', e => { order = e.target.value; render(); });

    wg.addEventListener('input', e => { wgVal.textContent = e.target.value; weights = normalizeWeights({ g:+wg.value/100, t:+wt.value/100, f:+wf.value/100 }); render(); });
    wt.addEventListener('input', e => { wtVal.textContent = e.target.value; weights = normalizeWeights({ g:+wg.value/100, t:+wt.value/100, f:+wf.value/100 }); render(); });
    wf.addEventListener('input', e => { wfVal.textContent = e.target.value; weights = normalizeWeights({ g:+wf.value/100, t:+wt.value/100, f:+wf.value/100 }); render(); });

    chipButtons.forEach(btn => {
      btn.addEventListener('click', () => {
        const sel = btn.getAttribute('data-cat');
        if (category === sel) { category = 'todas'; chipButtons.forEach(b => b.setAttribute('aria-pressed','false')); }
        else { category = sel; chipButtons.forEach(b => b.setAttribute('aria-pressed', b === btn ? 'true' : 'false')); }
        render();
      });
    });

    function starSVG(fill){
      return `<svg class="star" viewBox="0 0 24 24" aria-hidden="true"><path d="M12 17.27 18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z" fill="${fill}" stroke="#e5e7eb"/></svg>`;
    }

    function render(){
      const enriched = DATA.map(r => ({
        ...r,
        distance: haversineKm(userPos, r.coords),
        score: weightedScore(r.ratings, weights)
      }));
      let list = enriched
        .filter(r => category==='todas' ? true : r.category===category)
        .filter(r => r.distance <= radiusKm)
        .filter(r => query ? (r.name + " " + r.city).toLowerCase().includes(query) : true);
      list.sort((a,b) => order==='score' ? b.score - a.score : a.distance - b.distance);

      elRes.innerHTML = '';
      list.forEach(r => {
        const div = document.createElement('div');
        div.className = 'item';
        div.setAttribute('role','listitem');
        div.innerHTML = `
          <div style="margin-top:2px">📍</div>
          <div style="flex:1">
            <div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap">
              <a href="${r.url}" target="_blank" rel="noreferrer" class="link">${r.name}</a>
              <span class="badge" aria-label="City ${r.city}">${r.city}</span>
              <span class="badge" aria-label="Category ${r.category}">${r.category}</span>
            </div>
            <div class="muted" style="margin-top:4px;display:flex;gap:8px;align-items:center;font-size:14px">
              <span class="stars" aria-label="Score ${r.score} out of 5" title="${r.score} / 5">
                ${starSVG('#fbbf24').repeat(Math.floor(r.score))}
                ${(r.score - Math.floor(r.score))>=0.5 ? starSVG('#fde68a') : ''}
                ${starSVG('#e5e7eb').repeat(5 - Math.floor(r.score) - ((r.score - Math.floor(r.score))>=0.5?1:0))}
              </span>
              <span>${r.score} / 5</span>
              <span>•</span>
              <span aria-label="Distance ${r.distance.toFixed(1)} kilometers">${r.distance.toFixed(1)} km</span>
              <span>•</span>
              <span>Price ${r.price}</span>
            </div>
            <div class="badges" style="margin-top:6px">
              <span class="badge">Google: ${(r.ratings.google??'N/D')}</span>
              <span class="badge">Tripadvisor: ${(r.ratings.tripadvisor??'N/D')}</span>
              <span class="badge">TheFork: ${(typeof r.ratings.thefork==='number' ? r.ratings.thefork.toFixed(1)+' / 10' : 'N/D')}</span>
            </div>
          </div>
          <div><a class="muted" href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(r.name+' '+r.city)}" target="_blank" rel="noreferrer">Open in Maps</a></div>
        `;
        elRes.appendChild(div);
      });
      elCount.textContent = String(list.length);

      restMarkers.forEach(m => map.removeLayer(m));
      restMarkers = list.map(r => L.marker([r.coords.lat, r.coords.lng]).addTo(map)
        .bindPopup(`<b>${r.name}</b><br>${r.city} • ${r.category}<br><b>${r.score}</b> / 5<br><a href='https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(r.name+' '+r.city)}' target='_blank' rel='noreferrer'>Open in Maps</a>`));
    }

    render();
  </script>
</body>
</html>
