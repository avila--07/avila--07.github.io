<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Every new person adds more than one conversation</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@400;500;600&family=IBM+Plex+Sans+Condensed:wght@500;700&display=swap" rel="stylesheet">
<style>
:root{
  --paper:#EEF1F4; --ink:#1F2A3A; --muted:#5E6B7D; --rule:#CBD3DC;
  --calm:#2F8F83; --busy:#D9A23A; --over:#C8443B; --panel:#FFFFFF; --layer:#E3E8EE;
  box-sizing:border-box;
  padding-top:env(safe-area-inset-top,0px);
  padding-bottom:env(safe-area-inset-bottom,0px);
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){
    --paper:#18202C; --ink:#E6EBF1; --muted:#9AA7B8; --rule:#2E3A4B;
    --calm:#4FBFB0; --busy:#E8B65A; --over:#EE6A5F; --panel:#202A38; --layer:#273243;
  }
}
:root[data-theme="dark"]{
  --paper:#18202C; --ink:#E6EBF1; --muted:#9AA7B8; --rule:#2E3A4B;
  --calm:#4FBFB0; --busy:#E8B65A; --over:#EE6A5F; --panel:#202A38; --layer:#273243;
}
*,*::before,*::after{box-sizing:inherit}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{
  margin:0; background:var(--paper); color:var(--ink);
  font-family:"IBM Plex Sans",system-ui,-apple-system,"Segoe UI",sans-serif;
  font-size:17px; line-height:1.5;
}
main{max-width:1120px; margin:0 auto; padding:clamp(20px,4vw,48px)}
h1{
  font-family:"IBM Plex Sans Condensed","Arial Narrow",sans-serif; font-weight:700;
  font-size:clamp(30px,5vw,54px); line-height:1.02; letter-spacing:-.01em;
  margin:0 0 .35em; max-width:16ch;
}
.lede{color:var(--muted); max-width:60ch; margin:0 0 22px}
.modes{display:inline-flex; border:1px solid var(--rule); border-radius:999px; padding:4px; background:var(--panel); margin-bottom:22px; flex-wrap:wrap}
.modes button{border:0; background:transparent; padding:8px 16px}
.modes button[aria-pressed="true"]{background:var(--ink); color:var(--paper)}
.stage{display:grid; grid-template-columns:minmax(0,1.5fr) minmax(260px,1fr); gap:28px; align-items:start}
@media (max-width:820px){ .stage{grid-template-columns:1fr} }
.network{
  position:relative; aspect-ratio:1/1; width:100%;
  background:var(--panel); border:1px solid var(--rule); border-radius:18px; overflow:hidden;
}
.network canvas{position:absolute; inset:0; width:100%; height:100%}
.side{display:flex; flex-direction:column; gap:22px}
.readout{display:grid; grid-template-columns:repeat(3,auto); justify-content:start; gap:2px 26px}
.big{
  font-family:"IBM Plex Sans Condensed","Arial Narrow",sans-serif; font-weight:700;
  font-size:clamp(44px,6vw,76px); line-height:1; font-variant-numeric:tabular-nums;
  transition:color .4s;
}
.cap{color:var(--muted); font-size:15px; max-width:12ch; line-height:1.3}
.formula{font-size:16px; padding:12px 16px; border-left:3px solid var(--rule); color:var(--muted)}
.formula strong{color:var(--ink); font-weight:600}
.chartbox{background:var(--panel); border:1px solid var(--rule); border-radius:12px; padding:14px 14px 8px}
.chartbox canvas{width:100%; height:170px; display:block}
.legend{display:flex; gap:16px; font-size:14px; color:var(--muted); margin-top:6px; flex-wrap:wrap}
.legend i{display:inline-block; width:14px; height:3px; vertical-align:middle; margin-right:6px; border-radius:2px}
.legend i.dash{background:repeating-linear-gradient(90deg,var(--ink) 0 4px,transparent 4px 7px)!important}
.controls{display:flex; flex-wrap:wrap; gap:10px}
button{
  font:inherit; font-size:15px; font-weight:500; cursor:pointer;
  padding:10px 16px; border-radius:999px; border:1px solid var(--rule);
  background:var(--panel); color:var(--ink);
}
button.primary{background:var(--ink); color:var(--paper); border-color:var(--ink)}
.controls button:hover{border-color:var(--ink)}
button:focus-visible{outline:3px solid var(--calm); outline-offset:2px}
.roles{display:flex; gap:18px; font-size:14px; color:var(--muted); flex-wrap:wrap}
.roles span{display:inline-flex; align-items:center; gap:7px}
.dot{width:14px; height:14px; border-radius:50%; border:2px solid var(--ink); background:var(--panel)}
.dot.head{background:var(--ink)}
.dot.lead{border-width:4px}
.note{font-size:15px; color:var(--muted); max-width:62ch; margin-top:36px}
.note p{margin:0 0 .8em}
</style>
</head>
<body>
<main>
  <h1>Every new person adds more than one conversation</h1>
  <p class="lede">Watch a team grow one person at a time. In a flat team everyone stays in sync with everyone. Add layers, with squads, leads and a head, and the number of lines drops sharply, but messages now take more hops to get across.</p>

  <div class="modes" role="group" aria-label="How the team is organized">
    <button id="mFlat" aria-pressed="true">Everyone talks to everyone</button>
    <button id="mHier" aria-pressed="false">Squads with leads</button>
  </div>

  <div class="stage">
    <div class="network" aria-label="Animated network of people and the conversations between them">
      <canvas id="net"></canvas>
    </div>

    <div class="side">
      <div class="readout" aria-live="polite">
        <div class="big" id="nPeople">2</div>
        <div class="big" id="nLinks">1</div>
        <div class="big" id="nHops">1</div>
        <div class="cap">people</div>
        <div class="cap">conversations to keep in sync</div>
        <div class="cap">hops for the longest message</div>
      </div>

      <div class="formula" id="formula"></div>

      <div class="roles" id="roles" hidden>
        <span><i class="dot head"></i>head</span>
        <span><i class="dot lead"></i>squad lead</span>
        <span><i class="dot"></i>member</span>
      </div>

      <div class="chartbox">
        <canvas id="chart"></canvas>
        <div class="legend">
          <span><i style="background:var(--muted)"></i>people</span>
          <span><i style="background:var(--over)"></i>flat team</span>
          <span><i style="background:var(--calm)"></i>squads with leads</span>
        </div>
      </div>

      <div class="controls">
        <button class="primary" id="play">Pause</button>
        <button id="add">Add a person</button>
        <button id="remove">Remove a person</button>
        <button id="reset">Start over</button>
      </div>
    </div>
  </div>

  <div class="note">
    <p>In a flat team, coordination cost grows as n(n−1)/2. That's the core of Brooks's law: adding people to a late project makes it later, because each newcomer adds lines to everyone already there.</p>
    <p>Layers cap that growth. Members only stay in sync inside their squad of up to five, leads sync with each other, and the head talks to the leads. The price is distance: a message between two members of different squads has to go up through a lead and back down, which adds latency and room for things to get lost along the way.</p>
  </div>
</main>

<script>
(function(){
  const MAX = 16, MIN = 2, STEP_MS = 1800, SQUAD = 5;
  const net = document.getElementById('net'), ctx = net.getContext('2d');
  const chart = document.getElementById('chart'), cctx = chart.getContext('2d');
  const $ = id => document.getElementById(id);
  const reduce = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  const names = ["Ana","Ben","Caro","Dani","Eli","Fede","Gabi","Hugo","Iris","Juan","Kai","Lu","Mar","Nico","Olga","Pau"];
  const css = v => getComputedStyle(document.documentElement).getPropertyValue(v).trim();

  let n = 2, mode = 'flat', playing = !reduce, lastStep = 0;
  let nodes = [], pulses = [], graph = null;

  // ---- team structure ----
  function build(k, m){
    const roles = [], squad = [], edges = [];
    if (m === 'flat'){
      for (let i=0;i<k;i++){ roles.push('member'); squad.push(-1); }
      for (let i=0;i<k;i++) for (let j=i+1;j<k;j++) edges.push([i,j]);
    } else {
      roles.push('head'); squad.push(-1);
      const squads = [];
      for (let i=1;i<k;i++){
        const s = Math.floor((i-1)/SQUAD);
        if (!squads[s]) { squads[s] = []; roles.push('lead'); } else roles.push('member');
        squads[s].push(i); squad.push(s);
      }
      const leads = squads.map(s=>s[0]);
      leads.forEach(l => edges.push([0,l]));                                   // head ↔ leads
      for (let a=0;a<leads.length;a++) for (let b=a+1;b<leads.length;b++) edges.push([leads[a],leads[b]]); // leads ↔ leads
      squads.forEach(s => { for (let a=0;a<s.length;a++) for (let b=a+1;b<s.length;b++) edges.push([s[a],s[b]]); }); // inside squad
    }
    return {roles, squad, edges, hops: diameter(k, edges)};
  }
  function diameter(k, edges){
    const adj = Array.from({length:k},()=>[]);
    edges.forEach(([a,b])=>{adj[a].push(b); adj[b].push(a);});
    let best = 0;
    for (let s=0;s<k;s++){
      const d = Array(k).fill(-1); d[s]=0; const q=[s];
      while(q.length){ const u=q.shift(); adj[u].forEach(v=>{ if(d[v]<0){d[v]=d[u]+1; q.push(v);} }); }
      best = Math.max(best, ...d);
    }
    return best;
  }
  const count = (k,m) => build(k,m).edges.length;

  // ---- sizing & layout ----
  function size(c){
    const r = c.getBoundingClientRect(), d = window.devicePixelRatio || 1;
    c.width = Math.round(r.width*d); c.height = Math.round(r.height*d);
    c.getContext('2d').setTransform(d,0,0,d,0,0);
    return r;
  }
  let W=0,H=0;
  function resize(){ const r = size(net); W=r.width; H=r.height; size(chart); layout(); drawChart(); }

  function layout(){
    const cx=W/2, cy=H/2;
    while(nodes.length < n) nodes.push({x:cx,y:cy,tx:cx,ty:cy,born:performance.now()});
    nodes.length = n;
    if (mode === 'flat'){
      const R=Math.min(W,H)*0.38;
      nodes.forEach((p,i)=>{
        const a = -Math.PI/2 + i*2*Math.PI/n;
        p.tx = cx + R*Math.cos(a); p.ty = cy + R*Math.sin(a);
      });
    } else {
      const S = Math.max(1, Math.ceil((n-1)/SQUAD));
      const yHead = H*0.14, yLead = H*0.42, yMem = H*0.74;
      nodes[0].tx = cx; nodes[0].ty = yHead;
      for (let i=1;i<n;i++){
        const s = graph.squad[i], colW = W/S, sx = colW*(s+0.5);
        if (graph.roles[i]==='lead'){ nodes[i].tx = sx; nodes[i].ty = yLead; }
        else {
          const idx = (i-1)%SQUAD - 1;                         // 0..3
          const members = Math.min(SQUAD-1, (n-1) - s*SQUAD - 1);
          const spread = Math.min(colW*0.78, W*0.55);
          const t = members<=1 ? 0.5 : idx/(members-1);
          nodes[i].tx = sx - spread/2 + spread*t;
          nodes[i].ty = yMem + (idx%2 ? H*0.08 : 0);
        }
      }
    }
    pulses = [];
  }

  function loadColor(l){
    if (l <= 10) return css('--calm');
    if (l <= 45) return css('--busy');
    return css('--over');
  }

  function update(){
    graph = build(n, mode);
    const l = graph.edges.length, flatL = count(n,'flat');
    $('nPeople').textContent = n;
    $('nLinks').textContent = l;
    $('nLinks').style.color = loadColor(l);
    $('nHops').textContent = graph.hops;
    $('roles').hidden = mode !== 'hier';
    if (mode === 'flat'){
      $('formula').innerHTML = `With <strong>${n}</strong> people, each one talks to <strong>${n-1}</strong> others: <strong>${n}</strong> × <strong>${n-1}</strong> ÷ 2 = <strong>${l}</strong> channels.` +
        (n>3 ? ` That's ${(l/n).toFixed(1)} per person.` : '');
    } else {
      const S = Math.ceil((n-1)/SQUAD);
      const saved = flatL - l;
      $('formula').innerHTML = `<strong>${S}</strong> squad${S>1?'s':''} of up to ${SQUAD}, with ${S>1?'their leads':'its lead'} reporting to one head: <strong>${l}</strong> channels instead of <strong>${flatL}</strong>` +
        (saved>0 ? `, ${saved} fewer.` : '.') + ` The trade-off is up to <strong>${graph.hops}</strong> hops for a message to cross the team.`;
    }
    layout(); drawChart();
  }

  function setN(k){ n = Math.max(MIN, Math.min(MAX, k)); update(); }
  function setMode(m){
    mode = m;
    $('mFlat').setAttribute('aria-pressed', m==='flat');
    $('mHier').setAttribute('aria-pressed', m==='hier');
    update();
  }

  // ---- chart ----
  function drawChart(){
    const r = chart.getBoundingClientRect(), w=r.width, h=r.height;
    cctx.clearRect(0,0,w,h);
    const pad = {l:28,r:8,t:8,b:20}, maxY = count(MAX,'flat');
    const X = k => pad.l + (k-MIN)/(MAX-MIN)*(w-pad.l-pad.r);
    const Y = v => h-pad.b - v/maxY*(h-pad.t-pad.b);
    cctx.strokeStyle = css('--rule'); cctx.lineWidth=1;
    cctx.beginPath(); cctx.moveTo(pad.l,Y(0)); cctx.lineTo(w-pad.r,Y(0)); cctx.stroke();
    cctx.fillStyle = css('--muted'); cctx.font = '12px "IBM Plex Sans", sans-serif'; cctx.textAlign='left';
    cctx.fillText(maxY, 0, Y(maxY)+10); cctx.fillText('0', 12, Y(0)+4);
    cctx.fillText(MIN, X(MIN)-3, h-4); cctx.fillText(MAX, X(MAX)-14, h-4);

    const line = (f, color, width, alpha, upto=MAX) => {
      cctx.globalAlpha = alpha; cctx.strokeStyle = color; cctx.lineWidth = width; cctx.beginPath();
      for(let k=MIN;k<=upto;k++){ const y=Y(f(k)); k===MIN?cctx.moveTo(X(k),y):cctx.lineTo(X(k),y); }
      cctx.stroke(); cctx.globalAlpha = 1;
    };
    line(k=>k, css('--muted'), 2, 1);
    const cur = mode, other = mode==='flat'?'hier':'flat';
    const colorOf = m => m==='flat'?css('--over'):css('--calm');
    line(k=>count(k,other), colorOf(other), 2, .35);
    line(k=>count(k,cur), colorOf(cur), 2, .25);
    line(k=>count(k,cur), colorOf(cur), 3, 1, n);
    cctx.fillStyle = colorOf(cur); cctx.beginPath(); cctx.arc(X(n),Y(count(n,cur)),5,0,Math.PI*2); cctx.fill();
  }

  // ---- animation ----
  function spawnPulses(){
    const E = graph.edges; if (!E.length) return;
    const target = Math.min(70, Math.round(E.length*0.9)+1);
    while(pulses.length < target){
      const [a,b] = E[Math.floor(Math.random()*E.length)];
      const flip = Math.random()<.5;
      pulses.push({a:flip?b:a, b:flip?a:b, t:0, speed:0.006+Math.random()*0.01});
    }
  }

  function frame(now){
    if (playing && now - lastStep > STEP_MS){
      lastStep = now;
      setN(n >= MAX ? MIN : n+1);
    }
    nodes.forEach(p=>{ p.x += (p.tx-p.x)*0.08; p.y += (p.ty-p.y)*0.08; });
    ctx.clearRect(0,0,W,H);

    // layer bands
    if (mode === 'hier'){
      ctx.fillStyle = css('--layer'); ctx.globalAlpha = .55;
      [[0.14,0.07],[0.42,0.07],[0.78,0.13]].forEach(([y,hh])=>{ ctx.fillRect(0,H*(y-hh),W,H*hh*2); });
      ctx.globalAlpha = 1;
      ctx.fillStyle = css('--muted'); ctx.font = '13px "IBM Plex Sans", sans-serif'; ctx.textAlign='left'; ctx.textBaseline='top';
      ctx.fillText('Head', 12, H*0.07+6);
      if (n>1) ctx.fillText('Leads', 12, H*0.35+6);
      if (n>2) ctx.fillText('Squads', 12, H*0.65+6);
    }

    const E = graph.edges, col = loadColor(E.length);
    ctx.strokeStyle = col; ctx.lineWidth = 1.4;
    ctx.globalAlpha = Math.max(0.14, 0.6 - E.length/240);
    ctx.beginPath();
    E.forEach(([a,b])=>{ ctx.moveTo(nodes[a].x,nodes[a].y); ctx.lineTo(nodes[b].x,nodes[b].y); });
    ctx.stroke(); ctx.globalAlpha = 1;

    if (!reduce){
      spawnPulses();
      ctx.fillStyle = col;
      pulses.forEach(p=>{
        p.t += p.speed;
        const A=nodes[p.a], B=nodes[p.b];
        ctx.beginPath(); ctx.arc(A.x+(B.x-A.x)*p.t, A.y+(B.y-A.y)*p.t, 2.6, 0, Math.PI*2); ctx.fill();
      });
      pulses = pulses.filter(p=>p.t<1);
    }

    const r = Math.max(12, Math.min(22, W/30));
    nodes.forEach((p,i)=>{
      const role = graph.roles[i];
      const age = Math.min(1,(now-p.born)/400), s = (reduce?1:(0.4+0.6*age)) * (role==='head'?1.25:role==='lead'?1.1:1);
      const ink = css('--ink'), panel = css('--panel');
      ctx.fillStyle = role==='head' ? ink : panel;
      ctx.strokeStyle = ink; ctx.lineWidth = role==='lead' ? 4 : 2;
      ctx.beginPath(); ctx.arc(p.x,p.y,r*s,0,Math.PI*2); ctx.fill(); ctx.stroke();
      ctx.fillStyle = role==='head' ? panel : ink;
      ctx.font = `600 ${Math.round(r*0.6)}px "IBM Plex Sans", sans-serif`;
      ctx.textAlign='center'; ctx.textBaseline='middle';
      ctx.fillText(names[i].slice(0,2), p.x, p.y+1);
    });
    requestAnimationFrame(frame);
  }

  $('play').onclick = () => { playing = !playing; $('play').textContent = playing ? 'Pause' : 'Play'; lastStep = performance.now(); };
  $('add').onclick = () => setN(n+1);
  $('remove').onclick = () => setN(n-1);
  $('reset').onclick = () => { setN(MIN); lastStep = performance.now(); };
  $('mFlat').onclick = () => setMode('flat');
  $('mHier').onclick = () => setMode('hier');
  if (!playing) $('play').textContent = 'Play';

  window.addEventListener('resize', resize);
  if (document.fonts) document.fonts.ready.then(drawChart);
  graph = build(n, mode);
  resize(); update(); requestAnimationFrame(frame);
})();
</script>
</body>
</html>
