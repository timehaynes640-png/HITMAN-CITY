# HITMAN-CITY
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<meta name="theme-color" content="#0a0c0f">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Hitman City">
<meta name="description" content="Hitman City - A shooting game where you play as a hitman, buy cars, property and complete contracts.">
<link rel="manifest" href="manifest.json">
<link rel="apple-touch-icon" href="icons/icon-192.png">
<title>Hitman City</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Black+Ops+One&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
:root{
  --bg:#0a0c0f;--bg2:#111419;--bg3:#1a1f27;
  --accent:#c8a84b;--danger:#e03535;--safe:#2ecc71;
  --muted:#4a5568;--text:#e2e8f0;--text2:#a0aec0;
}
html,body{
  background:var(--bg);color:var(--text);
  font-family:'Share Tech Mono',monospace;
  height:100%;overflow:hidden;
  touch-action:manipulation;
}
#app{display:flex;flex-direction:column;height:100vh;height:100dvh}

/* HUD */
#hud{
  display:flex;justify-content:space-between;align-items:center;
  padding:6px 12px;background:rgba(0,0,0,0.9);
  border-bottom:1px solid #1e2530;flex-shrink:0;gap:6px;
  flex-wrap:wrap;
}
.hud-block{display:flex;flex-direction:column;gap:1px}
.hud-label{font-size:8px;color:var(--muted);letter-spacing:2px;text-transform:uppercase}
.hud-val{font-size:13px;font-family:'Black Ops One',sans-serif;color:var(--accent)}
.hud-val.red{color:var(--danger)}
.hud-val.green{color:var(--safe)}
#health-bar{width:70px;height:5px;background:#1e2530;border-radius:2px;overflow:hidden}
#health-fill{height:100%;background:linear-gradient(90deg,#e03535,#e07a35,#2ecc71);transition:width .3s;width:100%}
#ammo-bar{display:flex;gap:2px;align-items:center;flex-wrap:wrap;max-width:80px}
.ammo-pip{width:4px;height:9px;background:var(--accent);border-radius:1px;transition:background .2s}
.ammo-pip.used{background:var(--muted)}

/* Game area */
#game-area{flex:1;position:relative;overflow:hidden;background:var(--bg)}
#city-bg{position:absolute;inset:0;overflow:hidden}
.building{position:absolute;bottom:0;background:var(--bg2);border:1px solid #1e2530}
.win-light{position:absolute;width:3px;height:3px;background:rgba(200,168,75,0.6);border-radius:50%}
#road{position:absolute;bottom:0;left:0;right:0;height:70px;background:#0d1117;border-top:2px solid #1e2530}
.road-line{position:absolute;height:3px;background:var(--accent);opacity:0.3;bottom:32px;left:0;right:0}
#player{position:absolute;bottom:70px;width:28px;height:48px}
#targets-layer{position:absolute;inset:0}
.target{position:absolute;width:24px;height:40px;cursor:pointer}
.npc{position:absolute;width:24px;height:40px}
#bullets-layer{position:absolute;inset:0;pointer-events:none}
.bullet{position:absolute;width:6px;height:3px;background:var(--accent);border-radius:2px;pointer-events:none}
.hit-flash{position:absolute;width:30px;height:30px;border-radius:50%;background:radial-gradient(circle,rgba(224,53,53,0.8),transparent);pointer-events:none;animation:pop .3s forwards}
@keyframes pop{0%{transform:scale(0);opacity:1}100%{transform:scale(2);opacity:0}}

/* Toast */
#toast{
  position:absolute;top:60px;left:50%;transform:translateX(-50%);
  background:rgba(200,168,75,0.97);color:#000;
  padding:7px 14px;border-radius:4px;
  font-size:11px;font-family:'Black Ops One',sans-serif;
  opacity:0;pointer-events:none;transition:opacity .3s;z-index:100;
  white-space:nowrap;
}
#reload-hint{
  position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);
  font-family:'Black Ops One',sans-serif;color:var(--danger);font-size:18px;
  pointer-events:none;opacity:0;transition:opacity .2s;z-index:50;
}

/* Mobile controls */
#mobile-controls{
  position:absolute;bottom:75px;left:0;right:0;
  display:flex;justify-content:space-between;align-items:flex-end;
  padding:0 12px;pointer-events:none;
}
.dpad{display:flex;gap:4px;pointer-events:all}
.ctrl-btn{
  width:48px;height:48px;background:rgba(200,168,75,0.15);
  border:1px solid rgba(200,168,75,0.4);border-radius:6px;
  display:flex;align-items:center;justify-content:center;
  font-size:18px;cursor:pointer;user-select:none;
  -webkit-user-select:none;
}
.ctrl-btn:active{background:rgba(200,168,75,0.35)}
#shoot-ctrl{
  width:60px;height:60px;background:rgba(224,53,53,0.25);
  border-color:rgba(224,53,53,0.6);border-radius:50%;font-size:22px;
  pointer-events:all;
}
#shoot-ctrl:active{background:rgba(224,53,53,0.5)}
#menu-btns{
  position:absolute;top:6px;right:8px;
  display:flex;flex-direction:column;gap:4px;
}
.menu-btn{
  padding:5px 10px;font-size:9px;
  font-family:'Share Tech Mono',monospace;
  background:rgba(26,31,39,0.95);
  border:1px solid var(--muted);border-radius:3px;
  color:var(--text2);cursor:pointer;letter-spacing:1px;text-align:center;
}
.menu-btn:hover,.menu-btn:active{border-color:var(--accent);color:var(--accent)}

/* Side Panel */
#panel{
  position:absolute;top:0;right:0;bottom:0;width:260px;
  background:rgba(10,12,15,0.98);border-left:1px solid #1e2530;
  transform:translateX(260px);transition:transform .3s;
  overflow-y:auto;padding:14px;flex-shrink:0;z-index:60;
}
#panel.open{transform:translateX(0)}
.panel-title{font-family:'Black Ops One',sans-serif;color:var(--accent);font-size:15px;margin-bottom:10px;border-bottom:1px solid #1e2530;padding-bottom:7px}
.panel-item{
  background:var(--bg3);border:1px solid #1e2530;
  border-radius:4px;padding:9px;margin-bottom:7px;
  cursor:pointer;transition:border-color .2s;
}
.panel-item:hover,.panel-item:active{border-color:var(--accent)}
.panel-item-name{font-size:12px;color:var(--text);margin-bottom:3px}
.panel-item-price{font-size:11px;color:var(--accent)}
.panel-item-desc{font-size:10px;color:var(--text2);margin-top:2px}
.owned-badge{font-size:9px;background:var(--safe);color:#000;padding:1px 4px;border-radius:2px;margin-left:6px}
#tab-bar{display:flex;gap:4px;margin-bottom:10px}
.tab{flex:1;padding:5px;font-size:9px;background:var(--bg2);border:1px solid #1e2530;border-radius:3px;cursor:pointer;text-align:center;color:var(--text2);letter-spacing:1px}
.tab.active{background:var(--accent);color:#000;border-color:var(--accent)}

/* Missions overlay */
#missions-overlay{
  position:absolute;inset:0;background:rgba(0,0,0,0.93);
  display:none;flex-direction:column;align-items:center;
  justify-content:flex-start;gap:8px;padding:20px 16px;
  overflow-y:auto;z-index:70;
}
#missions-overlay.show{display:flex}
.mission-card{
  width:100%;max-width:420px;
  background:var(--bg3);border:1px solid var(--accent);
  border-radius:4px;padding:13px;cursor:pointer;transition:background .2s;
}
.mission-card:hover,.mission-card:active{background:#1e2530}
.m-title{font-family:'Black Ops One',sans-serif;color:var(--accent);font-size:13px;margin-bottom:3px}
.m-desc{font-size:11px;color:var(--text2);margin-bottom:6px}
.m-reward{font-size:11px;color:var(--safe)}
.m-status{font-size:10px;color:var(--muted);margin-top:3px}

/* Start screen */
#start-screen{
  position:absolute;inset:0;background:var(--bg);
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  gap:16px;z-index:200;
}
.start-title{font-family:'Black Ops One',sans-serif;font-size:36px;color:var(--accent);letter-spacing:4px;text-align:center}
.start-sub{font-size:11px;color:var(--text2);letter-spacing:3px;text-align:center}
.start-btn{
  padding:14px 36px;font-family:'Black Ops One',sans-serif;font-size:16px;
  background:var(--accent);color:#000;border:none;border-radius:4px;
  cursor:pointer;letter-spacing:3px;margin-top:8px;
}
.start-hint{font-size:10px;color:var(--muted);text-align:center;line-height:2;letter-spacing:1px}

/* Controls hint */
#controls-hint{
  position:absolute;bottom:125px;left:8px;
  font-size:8px;color:var(--muted);line-height:2;letter-spacing:1px;
  pointer-events:none;
}
@media(max-width:480px){
  #controls-hint{display:none}
}

/* Scrollbar */
::-webkit-scrollbar{width:4px}
::-webkit-scrollbar-track{background:var(--bg2)}
::-webkit-scrollbar-thumb{background:var(--muted);border-radius:2px}
</style>
</head>
<body>

<div id="app">
  <div id="hud">
    <div class="hud-block">
      <span class="hud-label">Agent</span>
      <span class="hud-val">GHOST</span>
    </div>
    <div class="hud-block">
      <span class="hud-label">Health</span>
      <div id="health-bar"><div id="health-fill"></div></div>
    </div>
    <div class="hud-block">
      <span class="hud-label">Funds</span>
      <span class="hud-val green" id="money-hud">$5,000</span>
    </div>
    <div class="hud-block">
      <span class="hud-label">Kills</span>
      <span class="hud-val red" id="kills-hud">0</span>
    </div>
    <div class="hud-block">
      <span class="hud-label">Ammo</span>
      <div id="ammo-bar"></div>
    </div>
    <div class="hud-block">
      <span class="hud-label">Mission</span>
      <span class="hud-val" id="mission-hud" style="font-size:9px;max-width:80px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">IDLE</span>
    </div>
  </div>

  <div id="game-area">
    <div id="city-bg"></div>
    <div id="road"><div class="road-line"></div></div>
    <div id="targets-layer"></div>
    <div id="bullets-layer"></div>
    <div id="player">
      <svg width="28" height="48" viewBox="0 0 28 48" fill="none" xmlns="http://www.w3.org/2000/svg">
        <ellipse cx="14" cy="9" rx="6" ry="6" fill="#c8a84b"/>
        <rect x="8" y="16" width="12" height="18" rx="2" fill="#1a1f27" stroke="#c8a84b" stroke-width="1"/>
        <rect x="5" y="17" width="4" height="12" rx="1" fill="#1a1f27" stroke="#4a5568" stroke-width="1"/>
        <rect x="19" y="17" width="4" height="12" rx="1" fill="#1a1f27" stroke="#4a5568" stroke-width="1"/>
        <rect x="21" y="23" width="5" height="3" rx="1" fill="#c8a84b"/>
        <rect x="9" y="34" width="4" height="12" rx="1" fill="#111419" stroke="#4a5568" stroke-width="1"/>
        <rect x="15" y="34" width="4" height="12" rx="1" fill="#111419" stroke="#4a5568" stroke-width="1"/>
      </svg>
    </div>
    <div id="reload-hint">RELOAD! [R]</div>
    <div id="controls-hint">← → MOVE &nbsp; SPACE SHOOT<br>R RELOAD &nbsp; M MISSIONS<br>B BUY MENU</div>

    <!-- Mobile controls -->
    <div id="mobile-controls">
      <div class="dpad">
        <div class="ctrl-btn" id="btn-left">◀</div>
        <div class="ctrl-btn" id="btn-right">▶</div>
      </div>
      <div style="display:flex;gap:8px;pointer-events:all;align-items:flex-end">
        <div class="ctrl-btn" onclick="reload()" style="font-size:14px;pointer-events:all">↺</div>
        <div class="ctrl-btn" id="shoot-ctrl">🔫</div>
      </div>
    </div>

    <!-- Menu buttons -->
    <div id="menu-btns">
      <div class="menu-btn" onclick="openMissions()">M MISSIONS</div>
      <div class="menu-btn" onclick="openPanel('buy')">B BUY</div>
    </div>

    <div id="toast"></div>

    <!-- Buy Panel -->
    <div id="panel">
      <div id="tab-bar">
        <div class="tab" onclick="switchTab('cars')">CARS</div>
        <div class="tab" onclick="switchTab('props')">PROP</div>
        <div class="tab" onclick="switchTab('weapons')">GUNS</div>
      </div>
      <div class="panel-title" id="panel-title">BUY CARS</div>
      <div id="panel-content"></div>
      <div style="margin-top:10px">
        <div class="menu-btn" onclick="closePanel()" style="text-align:center;width:100%">✕ CLOSE</div>
      </div>
    </div>

    <!-- Missions overlay -->
    <div id="missions-overlay">
      <div style="font-family:'Black Ops One',sans-serif;color:var(--accent);font-size:20px;letter-spacing:3px">CONTRACTS</div>
      <div style="font-size:9px;color:var(--text2);margin-bottom:4px;letter-spacing:2px">SELECT A MISSION</div>
      <div id="mission-list" style="width:100%;max-width:420px"></div>
      <div class="menu-btn" style="margin-top:6px;padding:8px 20px" onclick="closeMissions()">✕ CLOSE</div>
    </div>

    <!-- Start screen -->
    <div id="start-screen">
      <div class="start-title">HITMAN<br>CITY</div>
      <div class="start-sub">CONTRACTS. KILLS. CASH.</div>
      <button class="start-btn" onclick="startGame()">START GAME</button>
      <div class="start-hint">
        DESKTOP: ← → MOVE &nbsp; SPACE SHOOT &nbsp; R RELOAD<br>
        M MISSIONS &nbsp; B BUY MENU<br>
        MOBILE: USE ON-SCREEN CONTROLS
      </div>
    </div>
  </div>
</div>

<script>
const state={
  money:5000,health:100,kills:0,ammo:8,maxAmmo:8,reloading:false,
  playerX:200,moving:{left:false,right:false},
  bullets:[],targets:[],npcs:[],
  activeMission:null,missionKills:0,
  owned:{cars:[],props:[],weapons:['Pistol']},
  currentWeapon:'Pistol',
  panelOpen:false,activeTab:'cars',
  gameW:0,gameH:0,started:false
};

const cars=[
  {id:'sedan',name:'Sedan',price:8000,desc:'Reliable getaway. +10% escape bonus.',owned:false},
  {id:'suv',name:'Armored SUV',price:22000,desc:'Bulletproof. +25% health on mission start.',owned:false},
  {id:'sports',name:'Sports Car',price:45000,desc:'Top speed. Reduces mission timer 30%.',owned:false},
  {id:'truck',name:'Weapons Truck',price:35000,desc:'Mobile armory. Full ammo after missions.',owned:false},
];
const props=[
  {id:'safehouse',name:'Safehouse',price:15000,desc:'Restore health to 100% after missions.',owned:false},
  {id:'garage',name:'Underground Garage',price:30000,desc:'Store and upgrade vehicles.',owned:false},
  {id:'penthouse',name:'Penthouse Suite',price:80000,desc:'Prestige. Unlocks elite contracts.',owned:false},
  {id:'bunker',name:'Black Site Bunker',price:120000,desc:'Fortified HQ. Maximum security.',owned:false},
];
const weapons=[
  {id:'pistol',name:'Pistol',price:0,desc:'Standard sidearm. 8 rounds.',owned:true,ammo:8,dmg:25},
  {id:'smg',name:'SMG',price:5000,desc:'Rapid fire. 20 rounds.',owned:false,ammo:20,dmg:15},
  {id:'sniper',name:'Sniper Rifle',price:12000,desc:'One shot kill. 5 rounds.',owned:false,ammo:5,dmg:100},
  {id:'shotgun',name:'Shotgun',price:8000,desc:'Spread shot. 6 rounds.',owned:false,ammo:6,dmg:60},
];
const missions=[
  {id:'m1',title:'FIRST CONTRACT',desc:'Eliminate 3 hostile targets in the city.',reward:3000,kills:3,difficulty:'EASY',done:false},
  {id:'m2',title:'CITY CLEANUP',desc:'Eliminate 6 armed targets.',reward:7500,kills:6,difficulty:'MEDIUM',done:false},
  {id:'m3',title:'RED LIST',desc:'Eliminate 10 high-value targets.',reward:18000,kills:10,difficulty:'HARD',done:false},
  {id:'m4',title:'THE SYNDICATE',desc:'Take down 15 syndicate members.',reward:35000,kills:15,difficulty:'ELITE',done:false},
];

const ga=document.getElementById('game-area');
const tl=document.getElementById('targets-layer');
const bl=document.getElementById('bullets-layer');
const playerEl=document.getElementById('player');
const panel=document.getElementById('panel');

function startGame(){
  document.getElementById('start-screen').style.display='none';
  state.started=true;
  init();
}

function init(){
  state.gameW=ga.offsetWidth;
  state.gameH=ga.offsetHeight;
  state.playerX=Math.min(200,state.gameW/2);
  buildCity();
  buildNPCs();
  updateHUD();
  updateAmmoBar();
  spawnLoop();
  gameLoop();
}

function buildCity(){
  const bg=document.getElementById('city-bg');
  bg.innerHTML='';
  const W=Math.max(state.gameW,900);
  const bldgs=[
    {x:0,w:80,h:200,l:4},{x:90,w:60,h:150,l:3},{x:165,w:100,h:280,l:6},
    {x:280,w:70,h:180,l:3},{x:365,w:90,h:320,l:8},{x:470,w:65,h:200,l:4},
    {x:550,w:110,h:260,l:6},{x:680,w:80,h:170,l:3},{x:780,w:100,h:290,l:7},
    {x:900,w:90,h:220,l:5},{x:1010,w:70,h:300,l:6},
  ];
  bldgs.forEach(b=>{
    if(b.x>W) return;
    const d=document.createElement('div');
    d.className='building';
    d.style.cssText=`left:${b.x}px;width:${b.w}px;height:${b.h}px;bottom:70px`;
    for(let i=0;i<b.l;i++){
      const l=document.createElement('div');
      l.className='win-light';
      l.style.cssText=`left:${8+Math.random()*(b.w-16)}px;bottom:${20+Math.random()*(b.h-30)}px`;
      d.appendChild(l);
    }
    bg.appendChild(d);
  });
}

function buildNPCs(){
  tl.innerHTML='';
  state.npcs=[];
  for(let i=0;i<4;i++){
    const x=80+i*Math.floor(state.gameW/4)+Math.random()*40;
    const el=document.createElement('div');
    el.className='npc';
    el.style.cssText=`left:${x}px;bottom:70px`;
    el.innerHTML=`<svg width="24" height="40" viewBox="0 0 24 40"><ellipse cx="12" cy="7" rx="5" ry="5" fill="#718096"/><rect x="7" y="13" width="10" height="15" rx="2" fill="#4a5568"/><rect x="4" y="14" width="3" height="10" rx="1" fill="#4a5568"/><rect x="17" y="14" width="3" height="10" rx="1" fill="#4a5568"/><rect x="7" y="28" width="4" height="10" rx="1" fill="#2d3748"/><rect x="13" y="28" width="4" height="10" rx="1" fill="#2d3748"/></svg>`;
    tl.appendChild(el);
    state.npcs.push({el,x,dir:Math.random()>0.5?1:-1,speed:0.4+Math.random()*0.3});
  }
}

function spawnTarget(){
  if(!state.activeMission||state.targets.length>=3) return;
  const x=40+Math.random()*(state.gameW-80);
  const el=document.createElement('div');
  el.className='target';
  el.style.cssText=`left:${x}px;bottom:70px`;
  el.innerHTML=`<svg width="24" height="40" viewBox="0 0 24 40"><ellipse cx="12" cy="7" rx="5" ry="5" fill="#e03535"/><rect x="7" y="13" width="10" height="15" rx="2" fill="#c53030"/><rect x="4" y="14" width="3" height="10" rx="1" fill="#c53030"/><rect x="17" y="14" width="3" height="10" rx="1" fill="#c53030"/><rect x="17" y="20" width="5" height="2" rx="1" fill="#e2e8f0"/><rect x="7" y="28" width="4" height="10" rx="1" fill="#742a2a"/><rect x="13" y="28" width="4" height="10" rx="1" fill="#742a2a"/></svg>`;
  const t={el,x,health:100,dir:Math.random()>0.5?1:-1,speed:0.8+Math.random()*0.8};
  el.addEventListener('click',()=>{ if(state.ammo>0&&!state.reloading) hitTarget(t,getWeaponDmg()*2); });
  el.addEventListener('touchend',(e)=>{ e.preventDefault(); if(state.ammo>0&&!state.reloading) hitTarget(t,getWeaponDmg()*2); });
  tl.appendChild(el);
  state.targets.push(t);
}

function getWeaponDmg(){
  const w=weapons.find(x=>x.name===state.currentWeapon);
  return w?w.dmg:25;
}

let lastSpawn=0;
function spawnLoop(){
  const now=Date.now();
  if(state.activeMission&&now-lastSpawn>2500){
    spawnTarget();lastSpawn=now;
  }
  setTimeout(spawnLoop,500);
}

function shoot(){
  if(state.ammo<=0||state.reloading){showReloadHint();return;}
  state.ammo--;updateAmmoBar();
  const bx=state.playerX+22;
  const by=state.gameH-90;
  const bel=document.createElement('div');
  bel.className='bullet';
  bel.style.cssText=`left:${bx}px;top:${by}px`;
  bl.appendChild(bel);
  state.bullets.push({el:bel,x:bx,y:by,speed:14});
  if(state.ammo===0) showReloadHint();
}

function showReloadHint(){
  const rh=document.getElementById('reload-hint');
  rh.style.opacity='1';
  clearTimeout(showReloadHint._t);
  showReloadHint._t=setTimeout(()=>rh.style.opacity='0',700);
}

function reload(){
  if(state.reloading||state.ammo===state.maxAmmo) return;
  state.reloading=true;showToast('RELOADING...');
  setTimeout(()=>{
    state.ammo=state.maxAmmo;state.reloading=false;
    updateAmmoBar();showToast('RELOADED ✓');
  },1800);
}

function hitTarget(t,dmg){
  t.health-=dmg;
  const flash=document.createElement('div');
  flash.className='hit-flash';
  flash.style.cssText=`left:${t.x}px;top:${state.gameH-120}px`;
  bl.appendChild(flash);
  setTimeout(()=>flash.remove(),300);
  if(t.health<=0) killTarget(t);
}

function killTarget(t){
  t.el.remove();
  state.targets=state.targets.filter(x=>x!==t);
  state.kills++;
  const reward=state.activeMission?Math.floor(state.activeMission.reward/state.activeMission.kills):200;
  state.money+=reward;
  if(state.activeMission){
    state.missionKills++;
    showToast('+$'+reward+' TARGET ELIMINATED');
    if(state.missionKills>=state.activeMission.kills) completeMission();
  } else {
    showToast('+$'+reward+' BONUS KILL');
  }
  updateHUD();
}

function completeMission(){
  state.activeMission.done=true;
  const bonus=state.owned.props.includes('safehouse')?Math.floor(state.activeMission.reward*0.2):0;
  if(bonus>0) state.money+=bonus;
  showToast('✓ MISSION COMPLETE! +$'+state.activeMission.reward+(bonus>0?' +BONUS':''));
  state.activeMission=null;state.missionKills=0;
  state.targets.forEach(t=>t.el.remove());state.targets=[];
  document.getElementById('mission-hud').textContent='IDLE';
  if(state.owned.props.includes('safehouse')){state.health=100;document.getElementById('health-fill').style.width='100%';}
  if(state.owned.cars.includes('truck')){state.ammo=state.maxAmmo;updateAmmoBar();}
  updateHUD();
}

function gameLoop(){
  if(!state.started){requestAnimationFrame(gameLoop);return;}
  if(state.moving.left) state.playerX=Math.max(0,state.playerX-3);
  if(state.moving.right) state.playerX=Math.min(state.gameW-30,state.playerX+3);
  playerEl.style.left=state.playerX+'px';

  for(let i=state.bullets.length-1;i>=0;i--){
    const b=state.bullets[i];
    b.x+=b.speed;b.el.style.left=b.x+'px';
    if(b.x>state.gameW){b.el.remove();state.bullets.splice(i,1);continue;}
    let hit=false;
    for(const t of state.targets){
      if(Math.abs(b.x-t.x)<20){
        hitTarget(t,getWeaponDmg());
        b.el.remove();state.bullets.splice(i,1);hit=true;break;
      }
    }
  }

  state.targets.forEach(t=>{
    t.x+=t.dir*t.speed;
    if(t.x<10||t.x>state.gameW-30) t.dir*=-1;
    t.el.style.left=t.x+'px';
    if(Math.abs(t.x-state.playerX)<24&&state.activeMission){
      state.health=Math.max(0,state.health-0.2);
      document.getElementById('health-fill').style.width=state.health+'%';
      if(state.health<=0){
        showToast('YOU DIED - MISSION FAILED');
        state.activeMission=null;state.missionKills=0;
        state.targets.forEach(t=>t.el.remove());state.targets=[];
        state.health=50;
        document.getElementById('health-fill').style.width='50%';
        document.getElementById('mission-hud').textContent='IDLE';
      }
    }
  });

  state.npcs.forEach(n=>{
    n.x+=n.dir*n.speed;
    if(n.x<10||n.x>state.gameW-30) n.dir*=-1;
    n.el.style.left=n.x+'px';
  });

  requestAnimationFrame(gameLoop);
}

function updateHUD(){
  document.getElementById('money-hud').textContent='$'+state.money.toLocaleString();
  document.getElementById('kills-hud').textContent=state.kills;
}

function updateAmmoBar(){
  const bar=document.getElementById('ammo-bar');
  bar.innerHTML='';
  for(let i=0;i<state.maxAmmo;i++){
    const p=document.createElement('div');
    p.className='ammo-pip'+(i>=state.ammo?' used':'');
    bar.appendChild(p);
  }
}

function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg;t.style.opacity='1';
  clearTimeout(showToast._t);
  showToast._t=setTimeout(()=>t.style.opacity='0',2200);
}

function openPanel(type){
  if(type==='missions'){openMissions();return;}
  state.panelOpen=true;panel.classList.add('open');switchTab('cars');
}
function closePanel(){state.panelOpen=false;panel.classList.remove('open');}

function switchTab(tab){
  state.activeTab=tab;
  document.querySelectorAll('.tab').forEach((t,i)=>{
    t.classList.toggle('active',['cars','props','weapons'][i]===tab);
  });
  const titles={cars:'BUY CARS',props:'BUY PROPERTY',weapons:'WEAPONS'};
  document.getElementById('panel-title').textContent=titles[tab];
  renderPanelContent();
}

function renderPanelContent(){
  const pc=document.getElementById('panel-content');
  let items=state.activeTab==='cars'?cars:state.activeTab==='props'?props:weapons;
  pc.innerHTML='';
  items.forEach(item=>{
    const isOwned=state.activeTab==='cars'?state.owned.cars.includes(item.id):
                  state.activeTab==='props'?state.owned.props.includes(item.id):
                  state.owned.weapons.includes(item.name);
    const isActive=state.currentWeapon===item.name;
    const d=document.createElement('div');
    d.className='panel-item';
    if(isActive) d.style.borderColor='var(--accent)';
    d.innerHTML=`<div class="panel-item-name">${item.name}${isOwned?'<span class="owned-badge">OWNED</span>':''}</div>
      <div class="panel-item-price">${item.price===0?'FREE':'$'+item.price.toLocaleString()}</div>
      <div class="panel-item-desc">${item.desc}</div>`;
    d.onclick=()=>buyItem(item,isOwned);
    pc.appendChild(d);
  });
}

function buyItem(item,isOwned){
  if(isOwned){
    if(state.activeTab==='weapons'){
      state.currentWeapon=item.name;state.maxAmmo=item.ammo;state.ammo=item.ammo;
      updateAmmoBar();showToast('EQUIPPED: '+item.name);renderPanelContent();
    } else showToast('ALREADY OWNED');
    return;
  }
  if(state.money<item.price){showToast('INSUFFICIENT FUNDS');return;}
  state.money-=item.price;
  if(state.activeTab==='cars') state.owned.cars.push(item.id);
  else if(state.activeTab==='props') state.owned.props.push(item.id);
  else{
    state.owned.weapons.push(item.name);
    state.currentWeapon=item.name;state.maxAmmo=item.ammo;state.ammo=item.ammo;
    updateAmmoBar();
  }
  item.owned=true;showToast('PURCHASED: '+item.name+'!');updateHUD();renderPanelContent();
}

function openMissions(){
  const ol=document.getElementById('missions-overlay');
  const ml=document.getElementById('mission-list');
  ol.classList.add('show');ml.innerHTML='';
  missions.forEach(m=>{
    const d=document.createElement('div');d.className='mission-card';
    const diffColor={EASY:'#2ecc71',MEDIUM:'#f6c90e',HARD:'#e03535',ELITE:'#c8a84b'}[m.difficulty];
    d.innerHTML=`<div class="m-title">${m.title} <span style="font-size:10px;color:${diffColor}">[${m.difficulty}]</span></div>
      <div class="m-desc">${m.desc}</div>
      <div class="m-reward">REWARD: $${m.reward.toLocaleString()} &nbsp; KILLS: ${m.kills}</div>
      <div class="m-status">${m.done?'✓ COMPLETED':state.activeMission?.id===m.id?'► ACTIVE':''}</div>`;
    if(!m.done) d.onclick=()=>startMission(m);
    else d.style.opacity='0.5';
    ml.appendChild(d);
  });
}

function closeMissions(){document.getElementById('missions-overlay').classList.remove('show');}

function startMission(m){
  state.activeMission=m;state.missionKills=0;
  state.targets.forEach(t=>t.el.remove());state.targets=[];
  document.getElementById('mission-hud').textContent=m.title;
  closeMissions();showToast('▶ MISSION STARTED: '+m.title);lastSpawn=0;
}

// Keyboard controls
document.addEventListener('keydown',e=>{
  if(e.key==='ArrowLeft'||e.key==='a') state.moving.left=true;
  if(e.key==='ArrowRight'||e.key==='d') state.moving.right=true;
  if(e.key===' '){e.preventDefault();shoot();}
  if(e.key==='r'||e.key==='R') reload();
  if(e.key==='m'||e.key==='M') openMissions();
  if(e.key==='b'||e.key==='B') openPanel('buy');
});
document.addEventListener('keyup',e=>{
  if(e.key==='ArrowLeft'||e.key==='a') state.moving.left=false;
  if(e.key==='ArrowRight'||e.key==='d') state.moving.right=false;
});

// Mobile controls
const btnLeft=document.getElementById('btn-left');
const btnRight=document.getElementById('btn-right');
const btnShoot=document.getElementById('shoot-ctrl');
btnLeft.addEventListener('touchstart',e=>{e.preventDefault();state.moving.left=true;},{passive:false});
btnLeft.addEventListener('touchend',e=>{e.preventDefault();state.moving.left=false;},{passive:false});
btnRight.addEventListener('touchstart',e=>{e.preventDefault();state.moving.right=true;},{passive:false});
btnRight.addEventListener('touchend',e=>{e.preventDefault();state.moving.right=false;},{passive:false});
btnShoot.addEventListener('touchstart',e=>{e.preventDefault();shoot();},{passive:false});
btnShoot.addEventListener('click',shoot);

window.addEventListener('resize',()=>{
  state.gameW=ga.offsetWidth;state.gameH=ga.offsetHeight;
});

// Expose globals
window.openPanel=openPanel;window.closePanel=closePanel;window.switchTab=switchTab;
window.openMissions=openMissions;window.closeMissions=closeMissions;
window.shoot=shoot;window.reload=reload;window.startGame=startGame;

// Service Worker registration
if('serviceWorker' in navigator){
  window.addEventListener('load',()=>{
    navigator.serviceWorker.register('sw.js').then(reg=>{
      console.log('SW registered');
    }).catch(err=>{
      console.log('SW registration failed:',err);
    });
  });
}
</script>
</body>
</html>
