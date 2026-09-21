const sack = document.getElementById('sack-container');
const counterUI = document.getElementById('counter-ui');
const countSpan = document.getElementById('flower-count');
const sunsetBG = document.getElementById('sunset-background');
const poemCard = document.getElementById('poem-card');
const canvas = document.getElementById('bouquetCanvas');
const ctx = canvas.getContext('2d');

let isSackOpened = false;
let flowerCount = 0;
const maxFlowers = 15; // Flores necesarias para completar el ramo
const flowers = [];

let windAngle = 0;
let isSunsetActive = false;

// Coordenadas base del ramo
let bouquetCenter = { x: 0, y: 0, radius: 0 };

function resizeCanvas() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  // Definir la zona del ramo en el centro inferior
  bouquetCenter = {
    x: canvas.width / 2,
    y: canvas.height * 0.42,
    radiusX: Math.min(canvas.width * 0.38, 160),
    radiusY: Math.min(canvas.height * 0.18, 110)
  };
}
window.addEventListener('resize', resizeCanvas);
resizeCanvas();

// Abrir el saco
sack.addEventListener('click', (e) => {
  e.stopPropagation();
  sack.classList.add('sack-minimized');
  counterUI.classList.remove('ui-hidden');
  isSackOpened = true;
});

// Colocar flores exactamente dentro de la copa del ramo
window.addEventListener('click', (e) => {
  if (!isSackOpened || flowerCount >= maxFlowers) return;

  // Generar posición equilibrada en la copa del ramo
  const angle = Math.random() * Math.PI * 2;
  const distanceFactor = Math.sqrt(Math.random()); 
  
  const targetX = bouquetCenter.x + Math.cos(angle) * (bouquetCenter.radiusX * distanceFactor);
  const targetY = bouquetCenter.y + Math.sin(angle) * (bouquetCenter.radiusY * distanceFactor);

  // Escala y variación
  const scale = 0.75 + Math.random() * 0.35;
  const isEucalyptus = Math.random() < 0.25; // 25% hojitas verdes decorativas

  flowers.push({
    x: targetX,
    y: targetY,
    scale: scale,
    isEucalyptus: isEucalyptus,
    swayOffset: Math.random() * Math.PI * 2
  });

  flowerCount++;
  countSpan.textContent = flowerCount;

  // Ordenar de arriba a abajo para profundidad
  flowers.sort((a, b) => a.y - b.y);

  if (flowerCount >= maxFlowers) {
    counterUI.classList.add('ui-hidden');
    setTimeout(() => {
      sunsetBG.classList.add('sunset-visible');
      isSunsetActive = true;
      setTimeout(() => {
        poemCard.classList.add('poem-visible');
      }, 1200);
    }, 400);
  }
});

// Dibujar el envoltorio del ramo (Papel rosa/blanco y moño plateado)
function drawBouquetWrapper() {
  const cx = bouquetCenter.x;
  const cy = bouquetCenter.y + 40;

  ctx.save();

  // Papel plisado traseros
  ctx.fillStyle = '#fce7f3';
  ctx.strokeStyle = '#f472b6';
  ctx.lineWidth = 1.5;

  ctx.beginPath();
  ctx.moveTo(cx - 180, cy - 80);
  ctx.lineTo(cx - 60, cy + 220);
  ctx.lineTo(cx + 60, cy + 220);
  ctx.lineTo(cx + 180, cy - 80);
  ctx.lineTo(cx + 120, cy - 120);
  ctx.lineTo(cx, cy - 60);
  ctx.lineTo(cx - 120, cy - 120);
  ctx.closePath();
  ctx.fill();
  ctx.stroke();

  // Papel frontal doblado
  ctx.fillStyle = '#fbcfe8';
  ctx.beginPath();
  ctx.moveTo(cx - 150, cy - 40);
  ctx.lineTo(cx - 40, cy + 200);
  ctx.lineTo(cx + 40, cy + 200);
  ctx.lineTo(cx + 150, cy - 40);
  ctx.lineTo(cx, cy + 60);
  ctx.closePath();
  ctx.fill();
  ctx.stroke();

  // Tallo / Amarre
  ctx.fillStyle = '#e2e8f0';
  ctx.strokeStyle = '#cbd5e1';
  
  // Moño / Cinta
  ctx.beginPath();
  ctx.ellipse(cx - 25, cy + 180, 20, 10, -Math.PI / 6, 0, Math.PI * 2);
  ctx.ellipse(cx + 25, cy + 180, 20, 10, Math.PI / 6, 0, Math.PI * 2);
  ctx.fill();
  ctx.stroke();

  ctx.beginPath();
  ctx.arc(cx, cy + 180, 8, 0, Math.PI * 2);
  ctx.fillStyle = '#ffffff';
  ctx.fill();
  ctx.stroke();

  // Tiras del moño
  ctx.beginPath();
  ctx.moveTo(cx - 5, cy + 185);
  ctx.quadraticCurveTo(cx - 30, cy + 230, cx - 45, cy + 260);
  ctx.moveTo(cx + 5, cy + 185);
  ctx.quadraticCurveTo(cx + 30, cy + 230, cx + 45, cy + 260);
  ctx.lineWidth = 3;
  ctx.stroke();

  ctx.restore();
}

// Dibujar un Girasol detallado estilo foto
function drawSunflower(x, y, scale, sway) {
  ctx.save();
  ctx.translate(x, y);
  ctx.rotate(sway);
  ctx.scale(scale, scale);

  const petals = 20;
  const radius = 24;

  // Pétalos exteriores (Capa 1)
  for (let i = 0; i < petals; i++) {
    ctx.beginPath();
    ctx.rotate((Math.PI * 2) / petals);
    ctx.fillStyle = '#f59e0b';
    ctx.ellipse(0, radius + 8, 7, 22, 0, 0, Math.PI * 2);
    ctx.fill();
  }

  // Pétalos interiores (Capa 2)
  for (let i = 0; i < petals; i++) {
    ctx.beginPath();
    ctx.rotate(((Math.PI * 2) / petals) + 0.15);
    ctx.fillStyle = '#fbbf24';
    ctx.ellipse(0, radius + 4, 6, 18, 0, 0, Math.PI * 2);
    ctx.fill();
  }

  // Centro Oscuro Moteado (Semillas)
  ctx.beginPath();
  ctx.arc(0, 0, radius * 0.85, 0, Math.PI * 2);
  ctx.fillStyle = '#291507';
  ctx.fill();

  ctx.beginPath();
  ctx.arc(0, 0, radius * 0.7, 0, Math.PI * 2);
  ctx.fillStyle = '#422006';
  ctx.fill();

  // Textura del centro
  ctx.fillStyle = '#78350f';
  for (let j = 0; j < 12; j++) {
    const r = Math.random() * (radius * 0.6);
    const a = Math.random() * Math.PI * 2;
    ctx.beginPath();
    ctx.arc(Math.cos(a) * r, Math.sin(a) * r, 1.8, 0, Math.PI * 2);
    ctx.fill();
  }

  ctx.restore();
}

// Dibujar hojitas de eucalipto verde decorativas
function drawEucalyptus(x, y, scale, sway) {
  ctx.save();
  ctx.translate(x, y);
  ctx.rotate(sway);
  ctx.scale(scale, scale);

  ctx.strokeStyle = '#334155';
  ctx.lineWidth = 2;
  ctx.beginPath();
  ctx.moveTo(0, 20);
  ctx.lineTo(0, -30);
  ctx.stroke();

  ctx.fillStyle = '#475569';
  for (let i = -20; i <= 10; i += 12) {
    ctx.beginPath();
    ctx.ellipse(-8, i, 7, 5, 0, 0, Math.PI * 2);
    ctx.ellipse(8, i, 7, 5, 0, 0, Math.PI * 2);
    ctx.fill();
  }

  ctx.restore();
}

// Bucle de renderizado
function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // 1. Dibujar envoltorio del ramo de fondo
  drawBouquetWrapper();

  if (isSunsetActive) {
    windAngle += 0.02;
  }

  // 2. Dibujar las flores concentradas dentro del ramo
  flowers.forEach((f) => {
    const sway = isSunsetActive ? Math.sin(windAngle + f.swayOffset) * 0.08 : 0;
    if (f.isEucalyptus) {
      drawEucalyptus(f.x, f.y, f.scale, sway);
    } else {
      drawSunflower(f.x, f.y, f.scale, sway);
    }
  });

  requestAnimationFrame(animate);
}

animate();