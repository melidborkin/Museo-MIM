<section id="mapa" class="map-section">
  <div class="container">
    <div class="section-header">
      <h2 class="section-title">Mapa Interactivo</h2>
      <p class="section-description">
        Descubrí la ubicación geográfica de todos los monumentos y explorá cómo se distribuyen a través de los
        continentes y las épocas.
      </p>
    </div>

    <div class="map-card">
      <div class="map-content">
        <div class="map-placeholder">
          
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  .map-section {
    padding: 3rem 2rem;
    background: var(--background-color);

  }
  
  .container {
    max-width: 1200px;
    margin: 0 auto;
  }
  
  .section-header {
    text-align: center;
    margin-bottom: 4rem;
    align-content: center;
  }
  
  .section-title {
    font-size: 2.5rem;
    font-weight: bold;
    color: var(--text-color);
    margin-bottom: 0.5rem;
  }
  
  .section-description {
    font-size: 1.125rem;
    line-height: 1.7;
    color: var(--text-color);
  }
  
  .map-card {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 20px rgba(0,0,0,0.1);
  }
  
  .map-header {
    background: linear-gradient(135deg, var(--clear-color) 0%, var(--grey-color) 100%);
    padding: 2rem;
  }
  
  .header-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .map-title {
    font-size: 1.5rem;
    color: var(--text-color);
    margin-bottom: 0.5rem;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  
  .map-description {
    color: var(--text-color);
    opacity: 0.8;
    font-size: 1rem;
  }
  
  .fullscreen-btn {
    background: var(--background-color);
    border: 1px solid var(--grey-color);
    color: var(--text-color);
    padding: 0.5rem 1rem;
    border-radius: 6px;
    font-family: var(--font-family);
    font-size: 0.875rem;
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    transition: all 0.3s ease;
  }
  
  .fullscreen-btn:hover {
    background: var(--clear-color);
    border-color: var(--brand-color);
  }
  
  .map-content {
    padding: 0;
  }
  
  .map-placeholder {
    height: 500px;
    background: linear-gradient(135deg, #e3f2fd 0%, #f1f8e9 50%, #fff3e0 100%);
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }
  
  .map-placeholder::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-image: url('/placeholder.svg?height=500&width=1200&text=Mapa+Mundial');
    background-size: cover;
    background-position: center;
    opacity: 0.1;
  }
  
  .placeholder-content {
    position: relative;
    z-index: 10;
    text-align: center;
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    border-radius: 12px;
    padding: 2rem;
    border: 1px solid var(--grey-color);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
    max-width: 400px;
  }
  
  .map-icon {
    font-size: 4rem;
    margin-bottom: 1rem;
  }
  
  .placeholder-content h4 {
    font-size: 1.5rem;
    color: var(--text-color);
    margin-bottom: 1rem;
    font-weight: 600;
  }
  
  .placeholder-content p {
    color: var(--text-color);
    opacity: 0.8;
    margin-bottom: 2rem;
    line-height: 1.5;
  }
  
  .legend {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    justify-content: center;
  }
  
  .legend-item {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 0.875rem;
    color: var(--text-color);
  }
  
  .legend-color {
    width: 16px;
    height: 16px;
    border-radius: 50%;
  }
  
  @media (max-width: 768px) {
    .map-section {
      padding: 4rem 1rem;
    }
    
    .map-placeholder {
      height: 400px;
    }
    
    .header-content {
      flex-direction: column;
      gap: 1rem;
      text-align: center;
    }
    
    .legend {
      flex-direction: column;
      align-items: center;
    }
  }
</style>
