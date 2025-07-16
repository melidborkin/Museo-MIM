<script>
  import { onMount } from 'svelte';
  
  let mounted = false;
  
  const floatingMonuments = [
    { name: 'Torre Eiffel', x: 10, y: 20, size: 150, delay: 0 },
    { name: 'Taj Mahal', x: 80, y: 15, size: 150, delay: 1 },
    { name: 'Coliseo', x: 15, y: 70, size: 150, delay: 2 },
    { name: 'Big Ben', x: 85, y: 75, size: 150, delay: 3 },
    { name: 'Estatua de la Libertad', x: 5, y: 45, size: 150, delay: 4 },
    { name: 'Sagrada Familia', x: 90, y: 45, size: 60, delay: 5 },
    { name: 'Machu Pichu', x: 25, y: 25, size: 150, delay: 6 },
    { name: 'Ópera Sidney', x: 75, y: 25, size: 55, delay: 7 },
    { name: 'Torre de Pisa', x: 20, y: 80, size: 150, delay: 8 },
    { name: 'Puerta de Brandeburgo', x: 70, y: 80, size: 150, delay: 9 },
    { name: 'Petra' , x: 30, y: 60, size: 150, delay: 11 },
    { name: 'Muro de Berlín', x: 60, y: 60, size: 150, delay: 12 },
    { name: 'Gran Muralla China', x: 10, y: 30, size: 150, delay: 1 },
    { name: 'Catedral de Notre Dame', x: 15, y: 40, size: 150, delay: 4 },
    { name: 'Torre de Londres', x: 65, y: 40, size: 150, delay: 5 }, 
    { name: 'Palacio de Versalles', x: 47, y: 20, size: 150, delay: 6 },
    { name: 'Mausoleo de Halicarnaso', x: 70, y: 10, size: 150, delay: 7 },
    { name: 'Catedral de San Basilio', x: 20, y: 30, size: 150, delay: 8 },
    { name: 'Torre de Pisa', x: 25, y: 50, size: 150, delay: 9 },
    { name: 'Partenón', x: 65, y: 55, size: 150, delay: 2 },
    { name: 'Pirámides de Giza', x: 30, y: 140, size: 150, delay: 11 },
    { name: 'Catedral de San Pedro', x: 70, y: 30, size: 150, delay: 12 },
    { name: 'Castillo de Neuschwanstein', x: 80, y: 60, size: 150, delay: 3 },
    { name: 'Alhambra', x: 40, y: 20, size: 150, delay: 5 },
  ];
  
  onMount(() => {
    mounted = true;
  });
</script>

<section class="landing">
  <div class="floating-monuments">
    {#each floatingMonuments as monument}
      <div 
        class="floating-monument"
        class:mounted
        style="
          left: {monument.x}%; 
          top: {monument.y}%; 
          width: 200px; 
          height: {monument.size}px;
          animation-delay: {monument.delay * 0.2}s;
          font-family: 'montserrat', sans-serif;
        "
      > 
      <div class="name"> {monument.name} </div>      
      </div>
    {/each}
  </div>
  
  <div class="content" style="font-family: Montserrat;">
    <div class="logo-section">
      <h1 class="main-logo logo-font">MIM</h1>
      <h2 class="main-title">Museo Internacional<br>de Monumentos</h2>
    </div>
    
    <p class="subtitle">
      Un viaje a través de 10 siglos de arquitectura monumental
    </p>
    
    <div class="scroll-indicator">
      <div class="scroll-text">Desliza para explorar</div>
      <div class="scroll-arrow">↓</div>
    </div>
  </div>
  
  <div class="gradient-overlay"></div>
</section>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Monoton&family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

  .landing {
    height: 100vh;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, var(--brand-color) 0%, var(--bad-color) 100%);
    overflow: hidden;
  }
  
  .floating-monuments {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
  }
  
  .floating-monument {
    position: absolute;
    opacity: 0;
    transform: translateY(50px) scale(0.8);
    transition: all 1s ease-out;
    animation: float 6s ease-in-out infinite;
    width: 150px;
  }
  
  .floating-monument.mounted {
    opacity: 0.3;
    transform: translateY(0) scale(1);
  }
  
  @keyframes float {
    0%, 100% { transform: translateY(0px) rotate(0deg); }
    33% { transform: translateY(-20px) rotate(1deg); }
    66% { transform: translateY(10px) rotate(-1deg); }
  }

  .name {
    width: 100%;
  }
  
  .content {
    text-align: center;
    color: white;
    z-index: 10;
    position: relative;
    max-width: 800px;
    padding: 2rem;
  }
  
  .logo-section {
    margin-bottom: 2rem;
  }
  
  .main-logo {
    font-size: 200px;
    font-family: "Monoton";
    margin-bottom: 1rem;
    text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
    color: #DB574D;
  }
  
  @keyframes logoGlow {
    from { text-shadow: 2px 2px 4px rgba(0,0,0,0.3); }
    to { text-shadow: 2px 2px 20px rgba(255,255,255,0.3); }
  }
  
  .main-title {
    font-size: clamp(1.5rem, 4vw, 3rem);
    font-weight: 400;
    line-height: 1.2;
    text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
    color: #DB574D
  }
  
  .subtitle {
    font-size: 20px;
    margin-top: 5rem;
    line-height: 1.5;
    max-width: 600px;
    margin-bottom: 3rem;
    font-weight: 400;
    color: rgba(128, 128, 128, 0.731)
  }
  
  .scroll-indicator {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
    animation: bounce 2s infinite;
    color: rgba(128, 128, 128, 0.731)
  }
  
  .scroll-text {
    font-size: 1rem;
    font-weight: 500;
    opacity: 0.8;
  }
  
  .scroll-arrow {
    font-size: 1.5rem;
    opacity: 0.8;
  }
  
  @keyframes bounce {
    0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
    40% { transform: translateY(-10px); }
    60% { transform: translateY(-5px); }
  }
  
  .gradient-overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: radial-gradient(circle at center, transparent 30%, rgba(0,0,0,0.2) 100%);
    pointer-events: none;
  }
  
  @media (max-width: 768px) {
    .floating-monument { opacity: 0.2; }
    .content { padding: 1rem; }
  }
</style>
