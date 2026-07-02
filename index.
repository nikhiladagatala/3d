<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>College Solar Carport Parking — 3D Model</title>
<style>
  html,body{margin:0;padding:0;overflow:hidden;height:100%;background:#bfe3ff;font-family:Arial,Helvetica,sans-serif;}
  #hud{position:absolute;top:12px;left:12px;color:#fff;background:rgba(20,30,40,.55);
       padding:10px 14px;border-radius:10px;font-size:13px;line-height:1.5;backdrop-filter:blur(4px);}
  #hud b{color:#ffd479;}
  canvas{display:block;}
  #loading{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;
           color:#333;font-size:16px;background:#bfe3ff;z-index:5;}
</style>
</head>
<body>
<div id="loading">Building the parking lot…</div>
<div id="hud">
  <b>Solar Carport Parking Lot — 3D (realistic + green)</b><br>
  Drag: rotate &nbsp;•&nbsp; Scroll: zoom &nbsp;•&nbsp; Right-drag: pan
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
window.onload = function() {
document.getElementById('loading').style.display='none';

// ---------- BASIC SETUP ----------
const scene = new THREE.Scene();
scene.fog = new THREE.Fog(0xcfe9ff, 130, 360);

const camera = new THREE.PerspectiveCamera(45, window.innerWidth/window.innerHeight, 0.1, 1000);
let camDist = 95, camTheta = 0.85, camPhi = 0.95, camTarget = new THREE.Vector3(0,4,0);
function updateCamera(){
  camera.position.set(
    camTarget.x + camDist*Math.sin(camPhi)*Math.cos(camTheta),
    camTarget.y + camDist*Math.cos(camPhi),
    camTarget.z + camDist*Math.sin(camPhi)*Math.sin(camTheta)
  );
  camera.lookAt(camTarget);
}
updateCamera();

const renderer = new THREE.WebGLRenderer({antialias:true});
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(Math.min(devicePixelRatio,2));
renderer.shadowMap.enabled = true;
renderer.shadowMap.type = THREE.PCFSoftShadowMap;
renderer.outputEncoding = THREE.sRGBEncoding;
renderer.toneMapping = THREE.ACESFilmicToneMapping;
renderer.toneMappingExposure = 1.05;
document.body.appendChild(renderer.domElement);

window.addEventListener('resize', ()=>{
  camera.aspect = window.innerWidth/window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});

// simple orbit controls (mouse)
let dragging=false, panning=false, lastX=0, lastY=0;
renderer.domElement.addEventListener('mousedown', e=>{
  dragging = e.button===0; panning = e.button===2;
  lastX=e.clientX; lastY=e.clientY;
});
window.addEventListener('mouseup', ()=>{dragging=false; panning=false;});
window.addEventListener('mousemove', e=>{
  const dx=e.clientX-lastX, dy=e.clientY-lastY; lastX=e.clientX; lastY=e.clientY;
  if(dragging){
    camTheta -= dx*0.005;
    camPhi = Math.min(Math.max(camPhi - dy*0.004, 0.25), 1.45);
    updateCamera();
  } else if(panning){
    const right = new THREE.Vector3(Math.sin(camTheta-Math.PI/2),0,Math.cos(camTheta-Math.PI/2));
    camTarget.addScaledVector(right, -dx*0.05);
    camTarget.y = Math.max(0, camTarget.y + dy*0.03);
    updateCamera();
  }
});
renderer.domElement.addEventListener('contextmenu', e=>e.preventDefault());
renderer.domElement.addEventListener('wheel', e=>{
  camDist = Math.min(Math.max(camDist + e.deltaY*0.05, 25), 220);
  updateCamera();
}, {passive:true});
// touch support
let touchLastX=0,touchLastY=0,touchDist=0;
renderer.domElement.addEventListener('touchstart', e=>{
  if(e.touches.length===1){ touchLastX=e.touches[0].clientX; touchLastY=e.touches[0].clientY; }
  else if(e.touches.length===2){
    touchDist = Math.hypot(e.touches[0].clientX-e.touches[1].clientX, e.touches[0].clientY-e.touches[1].clientY);
  }
});
renderer.domElement.addEventListener('touchmove', e=>{
  e.preventDefault();
  if(e.touches.length===1){
    const dx=e.touches[0].clientX-touchLastX, dy=e.touches[0].clientY-touchLastY;
    touchLastX=e.touches[0].clientX; touchLastY=e.touches[0].clientY;
    camTheta -= dx*0.006;
    camPhi = Math.min(Math.max(camPhi - dy*0.005, 0.25), 1.45);
    updateCamera();
  } else if(e.touches.length===2){
    const d = Math.hypot(e.touches[0].clientX-e.touches[1].clientX, e.touches[0].clientY-e.touches[1].clientY);
    camDist = Math.min(Math.max(camDist - (d-touchDist)*0.1, 25), 220);
    touchDist = d;
    updateCamera();
  }
}, {passive:false});

// ---------- LIGHTS ----------
const hemi = new THREE.HemisphereLight(0xbfe3ff, 0x6b5a45, 0.7);
scene.add(hemi);
const sun = new THREE.DirectionalLight(0xfff2d8, 1.15);
sun.position.set(60, 90, 30);
sun.castShadow = true;
sun.shadow.mapSize.set(2048,2048);
sun.shadow.camera.left=-110; sun.shadow.camera.right=110;
sun.shadow.camera.top=110; sun.shadow.camera.bottom=-110;
sun.shadow.camera.far=260;
sun.shadow.bias=-0.0004;
scene.add(sun);
const fill = new THREE.DirectionalLight(0xdce8ff, 0.28);
fill.position.set(-50,40,-40);
scene.add(fill);
scene.add(new THREE.AmbientLight(0xffffff,0.2));

// ---------- SKY DOME ----------
const skyCanvasTex = (function(){
  const c=document.createElement('canvas'); c.width=2; c.height=512;
  const ctx=c.getContext('2d');
  const g=ctx.createLinearGradient(0,0,0,512);
  g.addColorStop(0,'#4f94d4');
  g.addColorStop(0.45,'#8fc3ea');
  g.addColorStop(0.75,'#d9edf9');
  g.addColorStop(1,'#f3f9ff');
  ctx.fillStyle=g; ctx.fillRect(0,0,2,512);
  const t=new THREE.CanvasTexture(c); t.needsUpdate=true; return t;
})();
const skyDome = new THREE.Mesh(
  new THREE.SphereGeometry(320,32,20),
  new THREE.MeshBasicMaterial({map:skyCanvasTex, side:THREE.BackSide, fog:false})
);
scene.add(skyDome);

function makeCloud(x,y,z,scale){
  const g=new THREE.Group();
  const mat=new THREE.MeshStandardMaterial({color:0xffffff, roughness:1, transparent:true, opacity:0.9, fog:false});
  const puffs=5+Math.floor(Math.random()*3);
  for(let i=0;i<puffs;i++){
    const s=new THREE.Mesh(new THREE.SphereGeometry(2+Math.random()*1.6,8,8), mat);
    s.position.set((Math.random()-0.5)*7,(Math.random()-0.5)*1.2,(Math.random()-0.5)*3);
    g.add(s);
  }
  g.position.set(x,y,z); g.scale.setScalar(scale);
  scene.add(g);
}
for(let i=0;i<10;i++){
  makeCloud(-150+Math.random()*300, 55+Math.random()*30, -150+Math.random()*300, 1+Math.random()*1.3);
}

// simple environment map for subtle reflections on paint/glass
const pmrem = new THREE.PMREMGenerator(renderer);
pmrem.compileEquirectangularShader();
const envScene = new THREE.Scene();
const envGrad = new THREE.Mesh(new THREE.SphereGeometry(50,16,16),
  new THREE.MeshBasicMaterial({map:skyCanvasTex, side:THREE.BackSide}));
envScene.add(envGrad);
const envTex = pmrem.fromScene(envScene, 0.04).texture;
scene.environment = envTex;

// ---------- HELPERS ----------
function canvasTexture(draw, w=256,h=256){
  const c=document.createElement('canvas'); c.width=w; c.height=h;
  const ctx=c.getContext('2d'); draw(ctx,w,h);
  const t=new THREE.CanvasTexture(c); t.needsUpdate=true; return t;
}
function textPlaneMaterial(lines, bg, fg, opts={}){
  const w=opts.w||512, h=opts.h||160;
  const tex = canvasTexture((ctx,W,H)=>{
    ctx.fillStyle=bg; ctx.fillRect(0,0,W,H);
    if(opts.border){ ctx.strokeStyle=opts.border; ctx.lineWidth=6; ctx.strokeRect(4,4,W-8,H-8); }
    ctx.fillStyle=fg; ctx.textAlign='center'; ctx.textBaseline='middle';
    const fs = opts.fontSize||42;
    ctx.font = 'bold '+fs+'px Arial';
    const lineH = fs*1.25;
    const startY = H/2 - (lines.length-1)*lineH/2;
    lines.forEach((l,i)=> ctx.fillText(l, W/2, startY+i*lineH));
  }, w,h);
  return new THREE.MeshStandardMaterial({map:tex, roughness:0.6});
}

// ---------- GROUND ----------
const paverTex = canvasTexture((ctx,W,H)=>{
  ctx.fillStyle='#c98f66'; ctx.fillRect(0,0,W,H);
  const tones=['#b87c53','#d29a6d','#c08558','#ba7f52'];
  const bw=16,bh=8;
  for(let y=0;y<H;y+=bh){
    const offset = (y/bh)%2===0?0:bw/2;
    for(let x=-bw;x<W;x+=bw){
      ctx.fillStyle = tones[Math.floor(Math.random()*tones.length)];
      ctx.fillRect(x+offset+1,y+1,bw-2,bh-2);
    }
  }
}, 512,512);
paverTex.wrapS = paverTex.wrapT = THREE.RepeatWrapping;
paverTex.repeat.set(24,24);
const ground = new THREE.Mesh(
  new THREE.PlaneGeometry(400,400),
  new THREE.MeshStandardMaterial({map:paverTex, roughness:0.95})
);
ground.rotation.x=-Math.PI/2;
ground.receiveShadow=true;
scene.add(ground);

// road strip (asphalt) at far left/front
const road = new THREE.Mesh(new THREE.PlaneGeometry(40,400),
  new THREE.MeshStandardMaterial({color:0x2b2b2e, roughness:0.9}));
road.rotation.x=-Math.PI/2; road.position.set(-75,0.01,0);
road.receiveShadow=true;
scene.add(road);
for(let z=-180;z<180;z+=14){
  const dash=new THREE.Mesh(new THREE.PlaneGeometry(0.4,6), new THREE.MeshStandardMaterial({color:0xffffff}));
  dash.rotation.x=-Math.PI/2; dash.position.set(-75,0.02,z);
  scene.add(dash);
}

// grass patches
function grassPatch(x,z,w,d){
  const g=new THREE.Mesh(new THREE.CircleGeometry(Math.max(w,d)/1.6,16),
    new THREE.MeshStandardMaterial({color:0x5c9a4a, roughness:1}));
  g.rotation.x=-Math.PI/2; g.position.set(x,0.015,z); g.receiveShadow=true;
  g.scale.set(w/(Math.max(w,d)/1.6*2), d/(Math.max(w,d)/1.6*2), 1);
  scene.add(g);
}

// painted parking lines
function paintLine(x,z,len,rotY,color=0xffffff){
  const l=new THREE.Mesh(new THREE.PlaneGeometry(len,0.35),
    new THREE.MeshStandardMaterial({color}));
  l.rotation.x=-Math.PI/2; l.rotation.z=rotY; l.position.set(x,0.02,z);
  scene.add(l);
}

// ---------- TREES ----------
function makeTree(x,z,scale=1){
  const g=new THREE.Group();
  const trunk=new THREE.Mesh(new THREE.CylinderGeometry(0.25,0.35,2.2,8),
    new THREE.MeshStandardMaterial({color:0x6b4a2f,roughness:0.9}));
  trunk.position.y=1.1; trunk.castShadow=true;
  g.add(trunk);
  const leafColors=[0x3f8a3f,0x4fa04a,0x357a35];
  for(let i=0;i<3;i++){
    const s=new THREE.Mesh(new THREE.SphereGeometry(1.6-i*0.25,10,10),
      new THREE.MeshStandardMaterial({color:leafColors[i%3], roughness:1}));
    s.position.set((Math.random()-0.5)*0.8, 2.6+i*1.1, (Math.random()-0.5)*0.8);
    s.castShadow=true;
    g.add(s);
  }
  g.position.set(x,0,z); g.scale.setScalar(scale);
  scene.add(g);
}

// ---------- BUSHES & FLOWER BEDS ----------
function makeBush(x,z,scale=1){
  const g=new THREE.Group();
  const cols=[0x3d8a44,0x347a3c,0x4a9a4f];
  for(let i=0;i<4;i++){
    const s=new THREE.Mesh(new THREE.SphereGeometry(0.55+Math.random()*0.25,8,8),
      new THREE.MeshStandardMaterial({color:cols[i%cols.length], roughness:1}));
    s.position.set((Math.random()-0.5)*0.9, 0.4+Math.random()*0.25, (Math.random()-0.5)*0.9);
    s.castShadow=true; s.receiveShadow=true;
    g.add(s);
  }
  g.position.set(x,0,z); g.scale.setScalar(scale);
  scene.add(g);
}
function makeFlowerBed(x,z,w,d){
  const mulch=new THREE.Mesh(new THREE.CircleGeometry(Math.max(w,d)/1.7,20),
    new THREE.MeshStandardMaterial({color:0x5a3d28, roughness:1}));
  mulch.rotation.x=-Math.PI/2; mulch.position.set(x,0.02,z); mulch.receiveShadow=true;
  mulch.scale.set(w/(Math.max(w,d)/1.7*2), d/(Math.max(w,d)/1.7*2),1);
  scene.add(mulch);
  const flowerColors=[0xe6524a,0xf2b632,0xf0e14a,0xe86fa0,0xffffff,0x8a5fd6];
  const count = Math.round(w*d*1.1);
  for(let i=0;i<count;i++){
    const fx = x+(Math.random()-0.5)*w*0.85;
    const fz = z+(Math.random()-0.5)*d*0.85;
    const petal=new THREE.Mesh(new THREE.SphereGeometry(0.16,6,6),
      new THREE.MeshStandardMaterial({color:flowerColors[Math.floor(Math.random()*flowerColors.length)], roughness:0.7}));
    petal.position.set(fx,0.18,fz);
    petal.castShadow=true;
    scene.add(petal);
    if(Math.random()>0.5){
      const leaf=new THREE.Mesh(new THREE.ConeGeometry(0.14,0.3,6),
        new THREE.MeshStandardMaterial({color:0x3f8a44, roughness:1}));
      leaf.position.set(fx+0.05,0.12,fz+0.05);
      scene.add(leaf);
    }
  }
}
function makeHedgeRow(x1,z1,x2,z2,seg=1.1){
  const dx=x2-x1, dz=z2-z1, len=Math.hypot(dx,dz);
  const n=Math.max(1,Math.round(len/seg));
  const mat=new THREE.MeshStandardMaterial({color:0x3f8a48, roughness:1});
  for(let i=0;i<=n;i++){
    const t=i/n;
    const hx=x1+dx*t, hz=z1+dz*t;
    const s=new THREE.Mesh(new THREE.BoxGeometry(seg*1.05,0.7,0.65,2,2,2), mat);
    s.position.set(hx,0.35,hz); s.castShadow=true; s.receiveShadow=true;
    scene.add(s);
  }
}

// ---------- CARS ----------
const carPalette = [0x2a5fb0,0xb0242c,0xdedede,0x1c2a4a,0x2fb0a3,0x6e6e6e,
                     0x9a1f2b,0x3a7fd6,0xefefef,0x232323,0x35a0d1,0xc23b2e];
let carColorIdx=0;
function nextColor(){ const c=carPalette[carColorIdx%carPalette.length]; carColorIdx++; return c; }

const shadowBlobTex = canvasTexture((ctx,W,H)=>{
  const g=ctx.createRadialGradient(W/2,H/2,0,W/2,H/2,W/2);
  g.addColorStop(0,'rgba(0,0,0,0.38)'); g.addColorStop(1,'rgba(0,0,0,0)');
  ctx.fillStyle=g; ctx.fillRect(0,0,W,H);
},128,128);
function makeCar(x,z,rotY,color){
  const g=new THREE.Group();
  const blob=new THREE.Mesh(new THREE.PlaneGeometry(5.6,2.6),
    new THREE.MeshBasicMaterial({map:shadowBlobTex, transparent:true, depthWrite:false}));
  blob.rotation.x=-Math.PI/2; blob.position.y=0.025;
  g.add(blob);
  const bodyMat = new THREE.MeshPhysicalMaterial({color, roughness:0.25, metalness:0.65, clearcoat:0.9, clearcoatRoughness:0.15, envMapIntensity:0.9});
  const glassMat = new THREE.MeshPhysicalMaterial({color:0x0d151d, roughness:0.08, metalness:0.3, clearcoat:1, envMapIntensity:1.1});
  const wheelMat = new THREE.MeshStandardMaterial({color:0x111111, roughness:0.75});

  const body = new THREE.Mesh(new THREE.BoxGeometry(4.4,0.9,1.9), bodyMat);
  body.position.y=0.65; body.castShadow=true; body.receiveShadow=true;
  g.add(body);

  const cabin = new THREE.Mesh(new THREE.BoxGeometry(2.3,0.7,1.7), glassMat);
  cabin.position.set(-0.1,1.35,0); cabin.castShadow=true;
  g.add(cabin);

  const hood = new THREE.Mesh(new THREE.BoxGeometry(4.4,0.35,1.95), bodyMat);
  hood.position.y=1.02;
  g.add(hood);

  const wheelGeo = new THREE.CylinderGeometry(0.42,0.42,0.35,16);
  [[1.4,0.95],[1.4,-0.95],[-1.4,0.95],[-1.4,-0.95]].forEach(([wx,wz])=>{
    const w = new THREE.Mesh(wheelGeo, wheelMat);
    w.rotation.z=Math.PI/2; w.position.set(wx,0.42,wz); w.castShadow=true;
    g.add(w);
  });

  g.position.set(x,0,z); g.rotation.y=rotY;
  scene.add(g);
}

// ---------- SOLAR CARPORT ----------
const panelTex = canvasTexture((ctx,W,H)=>{
  ctx.fillStyle='#101a2e'; ctx.fillRect(0,0,W,H);
  ctx.strokeStyle='#3a5a8a'; ctx.lineWidth=2;
  const cell=W/8;
  for(let i=0;i<=8;i++){
    ctx.beginPath(); ctx.moveTo(i*cell,0); ctx.lineTo(i*cell,H); ctx.stroke();
  }
  for(let j=0;j<=4;j++){
    ctx.beginPath(); ctx.moveTo(0,j*(H/4)); ctx.lineTo(W,j*(H/4)); ctx.stroke();
  }
  // subtle blue sheen
  const grad=ctx.createLinearGradient(0,0,W,H);
  grad.addColorStop(0,'rgba(90,140,220,0.15)');
  grad.addColorStop(1,'rgba(20,30,60,0.05)');
  ctx.fillStyle=grad; ctx.fillRect(0,0,W,H);
}, 256,128);
panelTex.wrapS = THREE.RepeatWrapping; panelTex.wrapT = THREE.RepeatWrapping;

function makeCarport(startX, startZ, length, poleEveryN=4){
  const g = new THREE.Group();
  const poleMat = new THREE.MeshStandardMaterial({color:0x8a8f96, roughness:0.4, metalness:0.7});
  const beamMat = new THREE.MeshStandardMaterial({color:0x777d85, roughness:0.4, metalness:0.6});
  const roofDepth = 11;         // covers 2 rows of cars
  const poleHeight = 4.6;
  const tiltRad = 0.11;

  const nPoles = Math.round(length/6)+1;
  for(let i=0;i<nPoles;i++){
    const zx = startZ + i*(length/(nPoles-1));
    [ -roofDepth/2+0.6, roofDepth/2-0.6 ].forEach(dx=>{
      const pole = new THREE.Mesh(new THREE.CylinderGeometry(0.22,0.22,poleHeight,10), poleMat);
      pole.position.set(startX+dx, poleHeight/2, zx);
      pole.castShadow=true; pole.receiveShadow=true;
      g.add(pole);
      const base = new THREE.Mesh(new THREE.CylinderGeometry(0.4,0.4,0.3,10),
        new THREE.MeshStandardMaterial({color:0x555555}));
      base.position.set(startX+dx,0.15,zx);
      g.add(base);
    });
  }
  // longitudinal beams
  [ -roofDepth/2+0.6, roofDepth/2-0.6 ].forEach(dx=>{
    const beam=new THREE.Mesh(new THREE.BoxGeometry(0.3,0.3,length), beamMat);
    beam.position.set(startX+dx, poleHeight+0.15, startZ+length/2-length/(nPoles-1)/2 +0.001);
    g.add(beam);
  });

  // roof panel array (tilted single slope)
  const roof = new THREE.Mesh(new THREE.BoxGeometry(roofDepth, 0.18, length),
    new THREE.MeshStandardMaterial({map:panelTex, roughness:0.35, metalness:0.5}));
  roof.position.set(startX, poleHeight+0.55, startZ+length/2 - length/(nPoles-1)/2);
  roof.rotation.z = tiltRad;
  roof.castShadow=true; roof.receiveShadow=true;
  panelTex.repeat.set(2, Math.max(4, Math.round(length/6)));
  g.add(roof);

  scene.add(g);
  return {startX,startZ,length,roofDepth};
}

// ---------- BUILD LAYOUT ----------
// Two long carport rows, cars parked underneath, running along Z.
const rowLength = 90;
const rowA = makeCarport(-14, -40, rowLength);
const rowB = makeCarport(6, -40, rowLength);

// cars under row A (two sub-rows)
let z = -36;
while(z < 46){
  makeCar(rowA.startX-2.6, z, Math.PI/2, nextColor());
  makeCar(rowA.startX+2.9, z, -Math.PI/2, nextColor());
  z += 4.6;
}
// cars under row B
z = -36;
while(z < 46){
  makeCar(rowB.startX-2.9, z, Math.PI/2, nextColor());
  makeCar(rowB.startX+2.6, z, -Math.PI/2, nextColor());
  z += 4.6;
}

// parking line paint under each car slot (front row, open area)
for(let zx=-36; zx<48; zx+=4.6){
  paintLine(rowA.startX-6.0, zx, 3.2, Math.PI/2);
  paintLine(rowB.startX+6.0, zx, 3.2, Math.PI/2);
}

// perimeter grass & trees (left side)
for(let zx=-60; zx<40; zx+=10){
  grassPatch(-34, zx, 6,5);
  if(Math.random()>0.35) makeTree(-36+Math.random()*3, zx+Math.random()*4, 0.9+Math.random()*0.5);
}

// entrance area: greenery only, no trees — keeps the sign clearly visible
grassPatch(20,60,50,14);
makeHedgeRow(-16,54,-16,72);
makeHedgeRow(16,54,16,44);
makeFlowerBed(-4,50,7,3.2);
makeFlowerBed(4,50,7,3.2);
makeBush(-9,58,1.1); makeBush(-9,63,1.0); makeBush(9,58,1.1); makeBush(9,63,1.0);

// dense back-of-lot tree line (far end, opposite the entrance)
for(let xx=-32; xx<32; xx+=6){
  makeTree(xx+(Math.random()-0.5)*3, -46-Math.random()*6, 1.1+Math.random()*0.6);
  if(Math.random()>0.4) makeTree(xx+(Math.random()-0.5)*3, -55-Math.random()*8, 1.0+Math.random()*0.7);
}
grassPatch(0,-50,70,18);
makeHedgeRow(-30,-42,30,-42);

// scattered bushes & flower beds along the carport medians
for(let zx=-30; zx<44; zx+=9){
  makeBush(-14, zx+2, 0.7+Math.random()*0.3);
  makeBush(6, zx-2, 0.7+Math.random()*0.3);
}
makeFlowerBed(-22,-10,4,10);
makeFlowerBed(24,-10,4,10);
makeFlowerBed(-22,20,4,10);
makeFlowerBed(24,20,4,10);

// entrance sign (like reference "COLLEGE PARKING / WELCOME")
(function(){
  const g=new THREE.Group();
  const poleMat=new THREE.MeshStandardMaterial({color:0x555555, metalness:0.5, roughness:0.5});
  const p1=new THREE.Mesh(new THREE.CylinderGeometry(0.28,0.28,5.2,10), poleMat);
  p1.position.set(-4,2.6,58); p1.castShadow=true; g.add(p1);
  const p2=new THREE.Mesh(new THREE.CylinderGeometry(0.28,0.28,5.2,10), poleMat);
  p2.position.set(4,2.6,58); p2.castShadow=true; g.add(p2);

  const board1 = new THREE.Mesh(new THREE.BoxGeometry(9.5,1.1,0.15),
    textPlaneMaterial(['◄  COLLEGE PARKING  ►'], '#1c2126','#ffffff',{fontSize:34}));
  board1.position.set(0,4.6,58); board1.castShadow=true;
  g.add(board1);
  const board2 = new THREE.Mesh(new THREE.BoxGeometry(6.2,0.85,0.15),
    textPlaneMaterial(['WELCOME'], '#1c2126','#ffd479',{fontSize:34}));
  board2.position.set(0,3.55,58);
  g.add(board2);
  scene.add(g);
})();

// gatehouse + barrier + stop sign near the "exit"
(function(){
  const g=new THREE.Group();
  const booth = new THREE.Mesh(new THREE.BoxGeometry(2.6,2.6,2.6),
    new THREE.MeshStandardMaterial({color:0x9aa0a6, roughness:0.6}));
  booth.position.set(14,1.3,66); booth.castShadow=true; booth.receiveShadow=true;
  g.add(booth);
  const roof=new THREE.Mesh(new THREE.BoxGeometry(3,0.25,3),
    new THREE.MeshStandardMaterial({color:0x4a4f55}));
  roof.position.set(14,2.75,66);
  g.add(roof);
  const window_=new THREE.Mesh(new THREE.BoxGeometry(1.6,1,0.1),
    new THREE.MeshStandardMaterial({color:0x1b2733, roughness:0.2, metalness:0.4}));
  window_.position.set(14,1.6,67.31);
  g.add(window_);

  // barrier arm
  const post=new THREE.Mesh(new THREE.CylinderGeometry(0.18,0.18,1.6,8),
    new THREE.MeshStandardMaterial({color:0x333333}));
  post.position.set(11,0.8,63);
  g.add(post);
  const arm=new THREE.Mesh(new THREE.BoxGeometry(7,0.18,0.28),
    textPlaneMaterial(['— — — — — —'], '#e3e3e3','#c62828',{fontSize:26,w:512,h:64}));
  arm.position.set(7.4,1.55,63);
  g.add(arm);

  // stop sign
  const pole=new THREE.Mesh(new THREE.CylinderGeometry(0.12,0.12,2.2,8),
    new THREE.MeshStandardMaterial({color:0x777777}));
  pole.position.set(10,1.1,60);
  g.add(pole);
  const sign=new THREE.Mesh(new THREE.CylinderGeometry(0.9,0.9,0.08,8),
    textPlaneMaterial(['STOP'],'#c62828','#ffffff',{fontSize:52,w:256,h:256}));
  sign.rotation.x=Math.PI/2; sign.rotation.z=Math.PI/8;
  sign.position.set(10,2.4,60);
  g.add(sign);

  scene.add(g);
})();

// EXIT ONLY / STOP ground text
function groundText(text,x,z,rot=0,w=6,h=2){
  const m = new THREE.Mesh(new THREE.PlaneGeometry(w,h),
    textPlaneMaterial([text], 'rgba(0,0,0,0)','#ffffff',{fontSize:44,w:512,h:170}));
  m.material.transparent=true;
  m.rotation.x=-Math.PI/2; m.rotation.z=rot;
  m.position.set(x,0.03,z);
  scene.add(m);
}
groundText('EXIT ONLY', 4, 63, 0);
groundText('STOP', 14, 71, 0, 4,1.6);

// background buildings (right side, far)
function makeBuilding(x,z,w,h,d,color){
  const b=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),
    new THREE.MeshStandardMaterial({color, roughness:0.8}));
  b.position.set(x,h/2,z); b.castShadow=true; b.receiveShadow=true;
  scene.add(b);
}
makeBuilding(55,20,26,14,16,0xb5b9bd);
makeBuilding(70,45,22,20,18,0xa4552e);
makeBuilding(60,70,20,10,14,0xc9cdd0);

// distant tree line (left boundary, full depth)
for(let zx=-95; zx<130; zx+=8){
  makeTree(-40+Math.random()*6, zx, 0.8+Math.random()*0.6);
  if(Math.random()>0.5) makeTree(-46+Math.random()*5, zx+3, 0.7+Math.random()*0.5);
}
// greenery around the background buildings
makeHedgeRow(42,10,42,32);
makeBush(46,15,1.2); makeBush(48,25,1.1); makeBush(50,60,1.2); makeBush(52,72,1.0);
grassPatch(48,25,16,50);

// bike rack near entrance (simple detail)
(function(){
  const rackMat=new THREE.MeshStandardMaterial({color:0x444444});
  for(let i=0;i<4;i++){
    const loop=new THREE.Mesh(new THREE.TorusGeometry(0.5,0.05,8,16,Math.PI),rackMat);
    loop.rotation.x=Math.PI/2; loop.position.set(-30+i*0.9,0.5,44);
    scene.add(loop);
  }
})();

// ---------- ANIMATE ----------
function animate(){
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
}
animate();

};
</script>
</body>
</html>
