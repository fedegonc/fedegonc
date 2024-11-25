<canvas id="particleCanvas"></canvas>
<style>
  canvas {
    background: #000;
    width: 100%;
    height: 400px;
  }
</style>
<script>
const canvas = document.getElementById('particleCanvas');
const ctx = canvas.getContext('2d');

// Ajustar tamaño
function resizeCanvas() {
  canvas.width = canvas.offsetWidth;
  canvas.height = canvas.offsetHeight;
}
resizeCanvas();
window.addEventListener('resize', resizeCanvas);

class Particle {
  constructor() {
    this.x = Math.random() * canvas.width;
    this.y = Math.random() * canvas.height;
    this.targetX = 0;
    this.targetY = 0;
    this.vx = 0;
    this.vy = 0;
    this.radius = 2;
  }

  setTarget(x, y) {
    this.targetX = x;
    this.targetY = y;
  }

  update() {
    let dx = this.targetX - this.x;
    let dy = this.targetY - this.y;
    
    this.vx += dx * 0.01;
    this.vy += dy * 0.01;
    
    this.vx *= 0.9;
    this.vy *= 0.9;
    
    this.x += this.vx;
    this.y += this.vy;
  }

  draw() {
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
    ctx.fillStyle = 'white';
    ctx.fill();
  }
}

// Crear partículas
const particles = Array(1000).fill().map(() => new Particle());

// Generar puntos en forma de cubo
function generateCubePoints() {
  const time = Date.now() * 0.001;
  const size = 100;
  const centerX = canvas.width / 2;
  const centerY = canvas.height / 2;
  
  particles.forEach((particle, i) => {
    const t = i / particles.length;
    const angle = time + t * Math.PI * 2;
    
    // Distribución en forma de cubo
    const face = Math.floor(t * 6);
    let x, y;
    
    switch(face) {
      case 0: // frente
        x = centerX + Math.cos(angle) * size;
        y = centerY + Math.sin(angle) * size;
        break;
      case 1: // derecha
        x = centerX + size;
        y = centerY + Math.sin(angle) * size;
        break;
      case 2: // atrás
        x = centerX - Math.cos(angle) * size;
        y = centerY + Math.sin(angle) * size;
        break;
      case 3: // izquierda
        x = centerX - size;
        y = centerY + Math.sin(angle) * size;
        break;
      case 4: // arriba
        x = centerX + Math.cos(angle) * size;
        y = centerY - size;
        break;
      case 5: // abajo
        x = centerX + Math.cos(angle) * size;
        y = centerY + size;
        break;
    }
    
    particle.setTarget(x, y);
  });
}

// Animación
function animate() {
  ctx.fillStyle = 'rgba(0, 0, 0, 0.1)';
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  
  generateCubePoints();
  
  particles.forEach(particle => {
    particle.update();
    particle.draw();
  });
  
  requestAnimationFrame(animate);
}

animate();
</script>
