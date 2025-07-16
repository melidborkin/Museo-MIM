<section id="explanation" class="explanation">
  <div class="container">
    
    <div class="content">
      <p class="intro">
        Bienvenidos al Museo Internacional de Monumentos, un espacio digital que celebra 
        la evolución arquitectónica desde el siglo XI hasta el XX. A través de diez salas 
        temáticas, exploraremos los monumentos más emblemáticos de cada época.
      </p>
      
      <div class="features">
        <div class="feature">
          <h3>10 Salas Temáticas</h3>
          <p>Cada siglo representado con sus monumentos más representativos</p>
        </div>
        
        <div class="feature">
          <h3>Visualizaciones</h3>
          <p>Gráficos y mapas interactivos que revelan patrones y tendencias arquitectónicas</p>
        </div>
        
        <div class="feature">
          <h3>Perspectiva Global</h3>
          <p>Monumentos de todos los continentes y culturas</p>
        </div>
      </div>
      
      
    </div>
    
    <p class="closing">
        Navega a través del tiempo y descubre cómo la arquitectura ha reflejado 
        las aspiraciones, creencias y avances tecnológicos de cada civilización.
      </p>
  </div>
</section>

<style>
  .explanation {
    padding: 6rem 2rem 2rem 2rem;
    background: radial-gradient(transparent 70%, rgba(0,0,0,0.2) 100%);
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
  
  .container {
    max-width: 1200px;
    margin: 0 auto;
    text-align: center;
  }

  .content {
    max-width: 1200px;
    margin: 0 auto;
  }
  
  .intro {
    font-size: 1.25rem;
    line-height: 1.8;
    color: var(--text-color);
    margin-bottom: 8rem;
    font-weight: 400;
  }
  
  .features {
    margin-top: 2rem;
    margin-bottom: 2rem;
    display: flex;
    gap: 3rem;
    margin-bottom: 4rem;
    justify-content: space-evenly;
  }
  
  .feature {
    text-align: center;
    width: 400px;
    justify-content: space-between;
  }
  
  .feature h3 {
    font-size: 1.5rem;
    color: var(--brand-color);
    margin-bottom: 1rem;
    font-weight: 600;
  }
  
  .feature p {
    color: var(--text-color);
    line-height: 1.6;
  }
  
  .closing {
    margin-top: 8rem;
    margin-bottom: 4rem;
    font-size: 1.1rem;
    line-height: 1.7;
    color: var(--text-color);
    font-style: italic;
    font-weight: 500;
    width: 1200px;
    text-align: center;
    
  }
  
  @media (max-width: 768px) {
    .explanation {
      padding: 4rem 1rem;
    }
    
    .features {
      grid-template-columns: 1fr;
      gap: 2rem;
    }
  }
</style>
