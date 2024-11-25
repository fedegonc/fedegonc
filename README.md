<style>
  canvas {
    background: #000;
    width: 100%;
    height: 400px;
  }
</style>

<canvas id="particleCanvas"></canvas>

<script>
const canvas = document.getElementById('particleCanvas');
const ctx = canvas.getContext('2d');

// Ajustar tamaño del canvas
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
    this.color = `hsl(${Math.random() * 360}, 50%, 50%)`; // Color aleatorio
  }

  setTarget(x, y) {
    this.targetX = x;
    this.targetY = y;
  }

  update() {
    const dx = this.targetX - this.x;
    const dy = this.targetY - this.y;
    
    this.vx += dx * 0.05; // Aumentado para movimiento más rápido
    this.vy += dy * 0.05;
    
    this.vx *= 0.95;
    this.vy *= 0.95;
    
    this.x += this.vx;
    this.y += this.vy;
  }

  draw() {
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
    ctx.fillStyle = this.color;
    ctx.fill();
  }
}

// Crear más partículas para mejor efecto visual
const particles = Array(1500).fill().map(() => new Particle());

function generateCubePoints() {
  const time = Date.now() * 0.001;
  const size = 120;
  const centerX = canvas.width / 2;
  const centerY = canvas.height / 2;
  
  particles.forEach((particle, i) => {
    const t = i / particles.length;
    const angle = time + t * Math.PI * 2;
    
    // Distribución mejorada en forma de cubo
    const face = Math.floor(t * 6);
    let x, y;
    
    switch(face) {
      case 0: // frente
        x = centerX + Math.cos(angle) * size;
        y = centerY + Math.sin(angle) * size;
        break;
      case 1: // derecha
        x = centerX + size * Math.cos(Math.PI/4);
        y = centerY + Math.sin(angle) * size;
        break;
      case 2: // atrás
        x = centerX - Math.cos(angle) * size;
        y = centerY + Math.sin(angle) * size;
        break;
      case 3: // izquierda
        x = centerX - size * Math.cos(Math.PI/4);
        y = centerY + Math.sin(angle) * size;
        break;
      case 4: // arriba
        x = centerX + Math.cos(angle) * size;
        y = centerY - size * Math.cos(Math.PI/4);
        break;
      case 5: // abajo
        x = centerX + Math.cos(angle) * size;
        y = centerY + size * Math.cos(Math.PI/4);
        break;
    }
    
    particle.setTarget(x, y);
  });
}

function animate() {
  ctx.fillStyle = 'rgba(0, 0, 0, 0.2)';
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
