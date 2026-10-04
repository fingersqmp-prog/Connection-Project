<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Connections</title>
<style>
:root{--bg:#f3f5f9;--panel:#fff;--text:#1c2230;--muted:#5b6475;--accent:#1f7ae0;--grid:#dfe5ee;--wire:#8a94a8;--on:#f2a900;--obs:#b8c0cf;--bad:#e04848;--field:#eaeef5;
box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#0f131b;--panel:#1a202c;--text:#e8ecf4;--muted:#9aa4b8;--accent:#5aa2ff;--grid:#222b3b;--wire:#6b768c;--on:#ffcb3d;--obs:#3a4458;--field:#131925}}
:root[data-theme="dark"]{--bg:#0f131b;--panel:#1a202c;--text:#e8ecf4;--muted:#9aa4b8;--accent:#5aa2ff;--grid:#222b3b;--wire:#6b768c;--on:#ffcb3d;--obs:#3a4458;--field:#131925}
html,body{margin:0}
body{background:var(--bg);color:var(--text);font:16px/1.4 system-ui,-apple-system,"Segoe UI",Roboto,sans-serif}
main{max-width:860px;margin:0 auto;padding:14px}
header{display:flex;justify-content:space-between;align-items:center;gap:8px;flex-wrap:wrap}
h1{margin:0;font-size:1.4rem}
.lvl{color:var(--muted)}
.bar{height:14px;background:var(--panel);border-radius:8px;overflow:hidden;margin:10px 0 4px;border:1px solid var(--grid)}
.fill{height:100%;width:0;background:var(--accent);transition:width .15s,background .2s}
.row{display:flex;justify-content:space-between;color:var(--muted);font-size:.9rem;gap:8px}
canvas{display:block;width:100%;aspect-ratio:8/5;background:var(--field);border-radius:12px;border:1px solid var(--grid);touch-action:none;margin-top:8px}
#msg{min-height:1.4em;margin:8px 0;font-weight:600}
button{font:inherit;background:var(--accent);color:#fff;border:0;border-radius:8px;padding:8px 16px;cursor:pointer}
button.alt{background:var(--panel);color:var(--text);border:1px solid var(--grid)}
button:disabled{opacity:.4;cursor:default}
.btns{display:flex;gap:8px;flex-wrap:wrap}
p.help{color:var(--muted);font-size:.9rem;margin:10px 0 0}
.stage{position:relative}
#title{position:absolute;inset:8px 0 0 0;border-radius:12px;background:color-mix(in srgb,var(--field) 94%,transparent);backdrop-filter:blur(4px);display:flex;align-items:center;justify-content:center;text-align:center;z-index:2}
#title[hidden]{display:none}
.tbox h2{font-size:clamp(2rem,7vw,3.2rem);margin:.1em 0}
.tbox p{color:var(--muted);margin:0 0 16px}
.tbox button{font-size:1.1rem;padding:10px 32px;border-radius:99px}
.tbox label{display:block;margin:12px 0 4px;color:var(--muted);font-size:.9rem}
.tbox details{color:var(--muted);font-size:.85rem;max-width:380px;margin:6px auto 0;text-align:left}
.dots{display:flex;gap:10px;justify-content:center;margin-bottom:6px}
.dots i{width:22px;height:22px;border-radius:50%;animation:bob 2.2s ease-in-out infinite}
@keyframes bob{50%{transform:translateY(-8px)}}
#goal{color:var(--muted);margin:6px 0 0;font-size:.95rem}
#legend{display:flex;flex-wrap:wrap;gap:6px 12px;align-items:center;margin-top:8px;font-size:.85rem;color:var(--muted)}
.chip{display:inline-flex;align-items:center;gap:5px;color:var(--text);transition:opacity .2s}
.chip i{width:12px;height:12px;border-radius:50%;display:inline-block}
#vn{position:absolute;z-index:3;inset:8px 0 0 0;border-radius:12px;background:rgba(8,12,20,.62);display:flex;align-items:flex-end;padding:3%;cursor:pointer;animation:fade .3s}
#vn[hidden]{display:none}
@keyframes fade{from{opacity:0}to{opacity:1}}
.box{width:100%;background:var(--panel);border:2px solid var(--accent);border-radius:12px;padding:12px 16px 10px;box-shadow:0 6px 24px rgba(0,0,0,.35)}
.who{display:inline-block;font-weight:700;font-size:.85rem;background:var(--accent);color:#fff;border-radius:99px;padding:1px 12px;margin-bottom:6px}
.who.mara{background:var(--on);color:#222}
#txt{min-height:3.9em;font-size:1rem;line-height:1.5}
.hint{display:flex;justify-content:space-between;color:var(--muted);font-size:.78rem;margin-top:4px}
.hint button{background:none;color:var(--muted);padding:0;text-decoration:underline;font-size:.78rem}
</style>
</head>
<body>
<main>
<header><h1>Connections</h1><span class="lvl" id="lvl"></span></header>
<div id="goal"></div>
<div class="bar"><div class="fill" id="fill"></div></div>
<div class="row"><span id="wire">Wire: 0 / 0</span><span id="pow">Powered: 0 / 0</span></div>
<div class="stage"><canvas id="c"></canvas>
<div id="title"><div class="tbox"><div class="dots"><i style="background:#ff5fa2"></i><i style="background:#4c8dff;animation-delay:.2s"></i><i style="background:#ffd93d;animation-delay:.4s"></i><i style="background:#3ddc84;animation-delay:.6s"></i><i style="background:#b388ff;animation-delay:.8s"></i></div>
<h2>Connections</h2><p>A puzzle about reaching out, at your own pace.</p>
<button id="play">▶ Play</button>
<label><input type="checkbox" id="story" checked> Show the story</label>
<details><summary>How to play</summary><p style="margin-top:8px">Each circle is a person, and its color shows what they know. Drag from one to another to lay a wire and connect them. Each level has its own goal and limits, shown above the board. Stay within the wire budget. Wires can't cross or pass through blocks. Click a wire to remove it.</p></details></div></div>
<div id="vn" hidden><div class="box"><span class="who" id="who"></span><div id="txt"></div>
<div class="hint"><span>Click to continue ▸</span><button id="skip">Skip story</button></div></div></div></div>
<div id="msg"></div>
<div class="btns"><button id="reset" class="alt">Reset</button><button id="undo" class="alt">Undo</button><button id="next" disabled>Next level →</button></div>
<div id="legend"></div>
<p class="help">Each circle is a person, and its color shows what they know. Drag from one to another to lay a wire. Power every node starting from the glowing source using the wire budget. Wires can't cross each other, pass through blocks, or run through other nodes. Click or tap a wire to remove it.</p>
</main>
<script>
const W=800,H=500,R=16;
const LEVELS=[
 {n:[[100,250],[300,140],[300,360],[520,250],[700,250]],o:[[390,205,26,90]],k:['Design','Music','Backend','Writing'],g:{t:'skills',need:['Backend']},cap:{src:1},s:1.3,goal:'Find someone who knows Backend. Mara will only talk to 1 person directly, but that person can pass her along.'},
 {n:[[80,80],[250,200],[420,70],[430,330],[620,190],[720,420],[200,410]],o:[[320,120,26,150]],k:['Languages','Music','Design','Data','Writing','Art'],g:{t:'count',n:4},s:1.25,goal:'Jonas just wants to meet 4 people. Not everyone, just 4.'},
 {n:[[70,250],[210,90],[210,410],[400,250],[560,90],[560,410],[740,160],[740,340]],o:[[300,20,26,170],[300,310,26,170],[640,200,90,26]],k:['Math','Design','Backend','Security','Languages','Writing','Hardware'],g:{t:'all'},s:1.22,goal:'Priya wants to connect everyone, so no one is left out.'},
 {n:[[80,250],[240,120],[240,380],[420,250],[580,110],[580,390],[730,250]],o:[[330,80,26,100],[330,320,26,100]],k:['Design','Math','Security','Data','Writing','Art'],g:{t:'skills',need:['Math','Security','Writing']},cap:{all:2},s:1.4,goal:'Sam needs Math, Security and Writing. Everyone can only handle 2 connections.'},
 {n:[[60,250],[180,90],[180,410],[340,250],[470,70],[470,430],[600,170],[600,330],[740,90],[740,410]],o:[[260,130,26,240],[380,130,70,26],[380,344,70,26],[520,225,26,50]],k:['Design','Math','Music','Security','Data','Writing','Art','Languages','Backend'],g:{t:'all'},cap:{all:3},s:1.12,goal:'Light up everyone. Nobody can talk to more than 3 people.'}
];
const cv=document.getElementById('c'),ctx=cv.getContext('2d');
const $=id=>document.getElementById(id);
let li=0,L,links,budget,drag=null,mouse={x:0,y:0},powered,won=false,bad=null,t0=performance.now();
const dist=(a,b)=>Math.hypot(a[0]-b[0],a[1]-b[1]);
function segHitRect(a,b,r){const[x,y,w,h]=r;
 if(Math.min(a[0],b[0])>x+w||Math.max(a[0],b[0])<x||Math.min(a[1],b[1])>y+h||Math.max(a[1],b[1])<y)return false;
 const ed=[[[x,y],[x+w,y]],[[x+w,y],[x+w,y+h]],[[x+w,y+h],[x,y+h]],[[x,y+h],[x,y]]];
 const inside=p=>p[0]>=x&&p[0]<=x+w&&p[1]>=y&&p[1]<=y+h;
 return inside(a)||inside(b)||ed.some(e=>segX(a,b,e[0],e[1]));}
function ori(p,q,r){return Math.sign((q[0]-p[0])*(r[1]-p[1])-(q[1]-p[1])*(r[0]-p[0]));}
function segX(a,b,c,d){return ori(a,b,c)!==ori(a,b,d)&&ori(c,d,a)!==ori(c,d,b);}
function ptSeg(p,a,b){const dx=b[0]-a[0],dy=b[1]-a[1];let t=((p[0]-a[0])*dx+(p[1]-a[1])*dy)/(dx*dx+dy*dy);t=Math.max(0,Math.min(1,t));return Math.hypot(p[0]-a[0]-t*dx,p[1]-a[1]-t*dy);}
function reason(i,j,ignoreBudget){
 const a=L.n[i],b=L.n[j];
 if(links.some(l=>(l[0]==i&&l[1]==j)||(l[0]==j&&l[1]==i)))return'Already connected';
 if(L.o.some(r=>segHitRect(a,b,r)))return'Blocked by an obstacle';
 if(L.n.some((p,k)=>k!=i&&k!=j&&ptSeg(p,a,b)<R-2))return'Wire would run through a node';
 if(links.some(l=>l[0]!=i&&l[0]!=j&&l[1]!=i&&l[1]!=j&&segX(a,b,L.n[l[0]],L.n[l[1]])))return'Wires can\'t cross';
 if(links.some(l=>(l.includes(i)||l.includes(j))&&(()=>{const s=l[0]==i||l[0]==j?l[1]:l[0];const sh=l[0]==i||l[1]==i?i:j;const o=sh==i?j:i;const A=L.n[sh],P=L.n[s],Q=L.n[o];return ori(A,P,Q)==0&&((P[0]-A[0])*(Q[0]-A[0])+(P[1]-A[1])*(Q[1]-A[1]))>0;})()))return'Wires would overlap';
 for(const x of [i,j])if(deg(x)>=capOf(x))return (x==0?CHARS[li]:L.p[x][0])+' can only connect to '+capOf(x)+(capOf(x)==1?' person':' people');
 if(!ignoreBudget&&used()+dist(a,b)>budget+0.01)return'Not enough wire left';
 return null;}
const used=()=>links.reduce((s,l)=>s+dist(L.n[l[0]],L.n[l[1]]),0);
const deg=i=>links.filter(l=>l[0]==i||l[1]==i).length;
const capOf=i=>{const c=L.cap||{};return i==0?(c.src??c.all??99):(c.all??99);};
function vis(i,j){const a=L.n[i],b=L.n[j];return !L.o.some(r=>segHitRect(a,b,r))&&!L.n.some((p,k)=>k!=i&&k!=j&&ptSeg(p,a,b)<R-2);}
function goalOn(set){const g=L.g;
 if(g.t=='all')return set.size==L.n.length;
 if(g.t=='count')return set.size-1>=g.n;
 return g.need.every(sk=>[...set].some(k=>k>0&&L.p[k][0]==sk));}
function minCost(){const n=L.n.length;let best=Infinity;
 for(let m=0;m<(1<<(n-1));m++){const inc=[0];for(let k=1;k<n;k++)if(m>>(k-1)&1)inc.push(k);
  if(!goalOn(new Set(inc)))continue;
  const seen=new Set([0]);let tot=0,ok=true;
  while(seen.size<inc.length){let bd=Infinity,bj=-1;
   for(const i of seen)for(const j of inc){if(seen.has(j)||!vis(i,j))continue;const d=dist(L.n[i],L.n[j]);if(d<bd){bd=d;bj=j;}}
   if(bj<0){ok=false;break;}seen.add(bj);tot+=bd;}
  if(ok&&tot<best)best=tot;}
 return best;}

const goalMet=()=>goalOn(powered);
function load(i){li=i;L=LEVELS[i];L.p=L.n.map((_,k)=>k?SKM[L.k[k-1]]:null);links=[];won=false;drag=null;
 budget=Math.ceil(minCost()*L.s);$('goal').textContent='Goal: '+L.goal;$('next').disabled=true;
 $('lvl').textContent='Level '+(i+1)+' / '+LEVELS.length;msg('Connect every node to the glowing source.');update();
 if(started&&!seen[i]){seen[i]=1;if(showStory)say(STORY.start[i]);}}
function update(){
 const adj=L.n.map(()=>[]);links.forEach(l=>{adj[l[0]].push(l[1]);adj[l[1]].push(l[0]);});
 powered=new Set([0]);const q=[0];while(q.length){const u=q.pop();adj[u].forEach(v=>{if(!powered.has(v)){powered.add(v);q.push(v);}});}
 $('legend').innerHTML='<span>What each person knows:</span>'+L.p.map((q,k)=>q?'<span class="chip" style="opacity:'+(powered.has(k)?1:.35)+'"><i style="background:'+q[1]+'"></i>'+(L.g.need&&L.g.need.includes(q[0])?'★ ':'')+q[0]+'</span>':'').join('');
 const u=used(),pct=Math.min(100,u/budget*100);
 $('fill').style.width=pct+'%';$('fill').style.background=pct>90?'var(--bad)':'var(--accent)';
 $('wire').textContent='Wire: '+Math.round(u/10)+' / '+Math.round(budget/10);
 {const g=L.g;$('pow').textContent=g.t=='all'?'Powered: '+powered.size+' / '+L.n.length:g.t=='count'?'People met: '+(powered.size-1)+' / '+g.n:'Skills found: '+g.need.filter(sk=>[...powered].some(k=>k>0&&L.p[k][0]==sk)).length+' / '+g.need.length;}
 if(goalMet()&&!won){won=true;$('next').disabled=li>=LEVELS.length-1;
  msg(li>=LEVELS.length-1?'🎉 All levels complete! Everyone is connected.':'✅ Level complete!',true);if(li<LEVELS.length-1)$('next').disabled=false;setTimeout(()=>{if(showStory&&!seen['w'+li]){seen['w'+li]=1;say(STORY.win[li]);}},700);}}
function msg(t,good){const m=$('msg');m.textContent=t;m.style.color=good?'var(--on)':'var(--text)';}
function pos(e){const r=cv.getBoundingClientRect();return[(e.clientX-r.left)/r.width*W,(e.clientY-r.top)/r.height*H];}
function nodeAt(p){for(let i=0;i<L.n.length;i++)if(dist(p,L.n[i])<R+10)return i;return -1;}
cv.addEventListener('pointerdown',e=>{cv.setPointerCapture(e.pointerId);const p=pos(e);mouse={x:p[0],y:p[1]};const i=nodeAt(p);
 if(i>=0){drag=i;return;}
 const k=links.findIndex(l=>ptSeg(p,L.n[l[0]],L.n[l[1]])<10);
 if(k>=0){links.splice(k,1);won=false;$('next').disabled=true;msg('Wire removed.');update();}});
cv.addEventListener('pointermove',e=>{const p=pos(e);mouse={x:p[0],y:p[1]};});
cv.addEventListener('pointerup',e=>{if(drag===null)return;const j=nodeAt(pos(e));const i=drag;drag=null;
 if(j<0||j==i)return;const r=reason(i,j);
 if(r){bad={i,j,t:performance.now()};msg('⚠ '+r);}else{links.push([i,j]);msg('Connected.');update();}});
$('reset').onclick=()=>load(li);
$('undo').onclick=()=>{links.pop();won=false;$('next').disabled=true;update();};
$('next').onclick=()=>{if(li<LEVELS.length-1)load(li+1);};
function col(n){return getComputedStyle(document.documentElement).getPropertyValue(n).trim();}
function frame(now){
 const dpr=window.devicePixelRatio||1,w=cv.clientWidth;
 if(cv.width!==Math.round(w*dpr)){cv.width=Math.round(w*dpr);cv.height=Math.round(w*dpr*H/W);}
 const s=cv.width/W;ctx.setTransform(s,0,0,s,0,0);ctx.clearRect(0,0,W,H);
 const t=(now-t0)/1000;
 ctx.strokeStyle=col('--grid');ctx.lineWidth=1;
 for(let x=0;x<=W;x+=40){ctx.beginPath();ctx.moveTo(x,0);ctx.lineTo(x,H);ctx.stroke();}
 for(let y=0;y<=H;y+=40){ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(W,y);ctx.stroke();}
 ctx.fillStyle=col('--obs');L.o.forEach(r=>{ctx.beginPath();ctx.roundRect(r[0],r[1],r[2],r[3],6);ctx.fill();});
 ctx.lineCap='round';
 links.forEach(l=>{const a=L.n[l[0]],b=L.n[l[1]],on=powered.has(l[0])&&powered.has(l[1]);
  let g=col('--wire');if(on){const ca=l[0]?L.p[l[0]][1]:col('--text'),cb=l[1]?L.p[l[1]][1]:col('--text');g=ctx.createLinearGradient(a[0],a[1],b[0],b[1]);g.addColorStop(0,l[0]<l[1]?ca:cb);g.addColorStop(1,l[0]<l[1]?cb:ca);}ctx.strokeStyle=g;ctx.lineWidth=6;ctx.setLineDash([]);
  ctx.beginPath();ctx.moveTo(a[0],a[1]);ctx.lineTo(b[0],b[1]);ctx.stroke();
  if(on){ctx.strokeStyle='#fff';ctx.globalAlpha=.7;ctx.lineWidth=2;ctx.setLineDash([6,18]);ctx.lineDashOffset=-t*40;
   ctx.beginPath();ctx.moveTo(a[0],a[1]);ctx.lineTo(b[0],b[1]);ctx.stroke();ctx.globalAlpha=1;ctx.setLineDash([]);}});
 if(drag!==null){const a=L.n[drag],hov=nodeAt([mouse.x,mouse.y]);
  const ok=hov>=0&&hov!=drag?!reason(drag,hov):true;
  ctx.strokeStyle=ok?col('--accent'):col('--bad');ctx.lineWidth=4;ctx.setLineDash([10,8]);
  const e=hov>=0&&hov!=drag?L.n[hov]:[mouse.x,mouse.y];
  ctx.beginPath();ctx.moveTo(a[0],a[1]);ctx.lineTo(e[0],e[1]);ctx.stroke();ctx.setLineDash([]);}
 if(bad&&now-bad.t<500){const a=L.n[bad.i],b=L.n[bad.j];ctx.strokeStyle=col('--bad');ctx.globalAlpha=1-(now-bad.t)/500;ctx.lineWidth=5;
  ctx.beginPath();ctx.moveTo(a[0],a[1]);ctx.lineTo(b[0],b[1]);ctx.stroke();ctx.globalAlpha=1;}
 L.n.forEach((p,i)=>{const on=powered.has(i),src=i==0,c=src?col('--text'):L.p[i][1],r=(src?R*1.35:R)*(on?1+Math.sin(t*4+i)*.06:1);
  if(on){ctx.globalAlpha=.22;ctx.fillStyle=c;ctx.beginPath();ctx.arc(p[0],p[1],r*1.8,0,7);ctx.fill();}
  ctx.globalAlpha=on?1:.4;ctx.fillStyle=c;ctx.beginPath();ctx.arc(p[0],p[1],r,0,7);ctx.fill();
  ctx.globalAlpha=on?1:.6;ctx.fillStyle=col('--text');ctx.font='600 13px system-ui,sans-serif';ctx.textAlign='center';ctx.textBaseline='top';
  {const cp=capOf(i);ctx.fillText((src?CHARS[li]:L.p[i][0])+(cp<99?'  '+deg(i)+'/'+cp:''),p[0],p[1]+r+6);}ctx.globalAlpha=1;});
 requestAnimationFrame(frame);}
const N='Narrator',CHARS=['Mara','Jonas','Priya','Sam','Everyone'];
const SKM={};
const SK=[['Design','#ff5fa2'],['Backend','#4c8dff'],['Music','#b388ff'],['Data','#3ddc84'],['Languages','#ff9f43'],['Writing','#2ec4a5'],['Hardware','#ff5c5c'],['Math','#ffd93d'],['Art','#ff7ad9'],['Security','#40d0ef']];SK.forEach(q=>SKM[q[0]]=q);
const STORY={
start:[
 [['Mara',"Hi, I'm Mara. I like to code solo, with coffee and headphones."],['Mara',"But today I'm stuck on a really nasty bug. Maybe, just this once, I could ask around."],['Mara',"I don't need to talk to the whole room. Just one person who knows Backend. One chat, then back to my headphones."],[N,'Goal: find someone who knows Backend. Mara will only talk to 1 person directly, but that person might know someone else.']],
 [['Jonas',"Hey, I'm Jonas. I moved here two months ago for uni. My code is fine, my small talk, not so much."],['Jonas',"I'm not trying to meet the whole campus. If I found four people to hang out with, I'd be so happy."],[N,'Goal: meet 4 people. Anyone will do.']],
 [['Priya',"I'm Priya. I've worked in tech for a long time, and honestly a lot of it was luck, and the right people."],['Priya',"Now I run the mentoring table. I want everyone here to have someone to talk to. No one left out."],[N,'Goal: connect everyone.']],
 [['Sam',"I'm Sam. Big groups wear me out, and I've made peace with that."],['Sam',"I can really only talk with two people at a time before I need a break, so I pick my conversations carefully."],['Sam',"For our project I need someone for Math, someone for Security and someone for Writing. That's it."],[N,'Goal: find Math, Security and Writing. Everyone can only have 2 connections.']],
 [[N,"Last one. Let's put them all in the same room."],['Mara',"Some of us mostly work alone."],['Jonas',"Some of us are new, and looking for friends."],['Priya',"Some of us just love helping."],['Sam',"And some of us only need a few good people."],[N,'Goal: light up everyone. Nobody can talk to more than 3 people, so choose who connects to whom.']]],
win:[
 [['Mara',"Okay, that took five minutes. The fix was one line."],['Mara',"Headphones going back on now. But it was kind of nice to have someone to ask."],[N,"Connecting doesn't mean changing who you are. Sometimes it's one question, and then you carry on."]],
 [['Jonas',"A girl from my class introduced me to her friend, and he knew a study group. Just like that."],['Jonas',"Turns out most people were hoping someone else would say hi first."],[N,"You don't have to know everyone, just someone who knows someone."]],
 [['Priya',"Years ago, someone I barely knew mentioned a job opening. One coffee chat changed my whole career."],[N,'Sociologist Mark Granovetter found that acquaintances often lead to new opportunities more than close friends do, because they know people we do not.'],['Priya',"So now I make introductions. It costs me nothing, and it can mean a lot to them."]],
 [['Sam',"Three good conversations, and I got everything I needed. I actually feel great, not drained."],[N,"There's no right amount of networking. Quality and trust matter more than numbers, and protecting your energy counts."]],
 [[N,"Every light on this board was dark until someone chose to reach out. Or chose not to, and that's okay too."],['Mara',"Solo or together, the door's open for anyone who wants it."],[N,"A network isn't about how many people you know. It's about who can reach each other because of you."],[N,'Thanks for playing! If you feel like it, say hi to someone today. 💡']]]
};
let seen={},started=false,showStory=true,vq=null,vi=0,vdone=null,tw=null,cur='';
function say(b,done){if(!b||!b.length){done&&done();return;}vq=b;vi=0;vdone=done;$('vn').hidden=false;beat();}
function beat(){const[w,t]=vq[vi];cur=t;const e=$('who');e.textContent=w;e.className='who'+(w!==N?' mara':'');$('txt').textContent='';
 let k=0;clearInterval(tw);tw=setInterval(()=>{k++;$('txt').textContent=t.slice(0,k);if(k>=t.length){clearInterval(tw);tw=null;}},20);}
function endSay(){clearInterval(tw);tw=null;$('vn').hidden=true;const d=vdone;vdone=null;d&&d();}
$('vn').addEventListener('click',e=>{if(e.target.id==='skip'){endSay();return;}
 if(tw){clearInterval(tw);tw=null;$('txt').textContent=cur;return;}
 vi++;if(vi>=vq.length)endSay();else beat();});
$('play').onclick=()=>{started=true;showStory=$('story').checked;$('title').hidden=true;seen[0]=1;if(showStory)say(STORY.start[0]);};
load(0);requestAnimationFrame(frame);
</script>
</body>
</html>
