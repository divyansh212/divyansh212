<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Divyansh — 3D Scene</title>
<style>
  :root {
    --bg: #0c1218;
    --accent: #6e40c9;
    --accent2: #2dd4bf;
  }
  html, body {
    margin: 0;
    height: 100%;
    background: radial-gradient(circle at 50% 40%, #121b24 0%, var(--bg) 70%);
    overflow: hidden;
    font-family: "Fira Code", monospace;
  }
  /* Record this 600x320 box to a GIF (use ScreenToGif, Kap, or browser capture). */
  #stage {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 600px;
    height: 320px;
    border-radius: 14px;
    overflow: hidden;
    box-shadow: 0 0 80px rgba(110, 64, 201, 0.25);
  }
  canvas { display: block; }
  .label {
    position: absolute;
    bottom: 18px;
    width: 100%;
    text-align: center;
    color: #9aa7b4;
    letter-spacing: 3px;
    font-size: 13px;
    text-transform: uppercase;
  }
</style>
</head>
<body>
<div id="stage">
  <div class="label">DIVYANSH RAJ · AI PRODUCT ENGINEER</div>
</div>
 
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
  const stage = document.getElementById('stage');
  const W = 600, H = 320;
 
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(50, W / H, 0.1, 1000);
  camera.position.z = 6;
 
  const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
  renderer.setSize(W, H);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  stage.appendChild(renderer.domElement);
 
  // Wireframe icosahedron — "neural core"
  const geo = new THREE.IcosahedronGeometry(1.7, 1);
  const mat = new THREE.MeshBasicMaterial({ color: 0x6e40c9, wireframe: true, transparent: true, opacity: 0.85 });
  const core = new THREE.Mesh(geo, mat);
  scene.add(core);
 
  // Inner glowing sphere
  const innerGeo = new THREE.SphereGeometry(0.9, 32, 32);
  const innerMat = new THREE.MeshBasicMaterial({ color: 0x2dd4bf, transparent: true, opacity: 0.18 });
  scene.add(new THREE.Mesh(innerGeo, innerMat));
 
  // Orbiting particles
  const pGeo = new THREE.BufferGeometry();
  const N = 400, pos = new Float32Array(N * 3);
  for (let i = 0; i < N; i++) {
    const r = 2.6 + Math.random() * 1.6;
    const t = Math.random() * Math.PI * 2;
    const p = Math.acos(2 * Math.random() - 1);
    pos[i*3]   = r * Math.sin(p) * Math.cos(t);
    pos[i*3+1] = r * Math.sin(p) * Math.sin(t);
    pos[i*3+2] = r * Math.cos(p);
  }
  pGeo.setAttribute('position', new THREE.BufferAttribute(pos, 3));
  const particles = new THREE.Points(pGeo, new THREE.PointsMaterial({ color: 0x9aa7b4, size: 0.04 }));
  scene.add(particles);
 
  function animate() {
    requestAnimationFrame(animate);
    core.rotation.x += 0.004;
    core.rotation.y += 0.006;
    particles.rotation.y -= 0.0015;
    renderer.render(scene, camera);
  }
  animate();
</script>
</body>
</html>
