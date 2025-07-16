<script>
  export let century;
  export let index;
  
  let currentImageIndex = 0;

  
  const images = century.images || []
  function nextImage() {
    currentImageIndex = (currentImageIndex + 1) % century.images.length;
  }
  
  function prevImage() {
    currentImageIndex = (currentImageIndex - 1 + images.length) % images.length;
  }

  let carouselContainer;

  const scroll = (direction) => {
      const scrollAmount = 300;
      if (carouselContainer) {
          carouselContainer.scrollBy({ left: direction === 'left' ? -scrollAmount : scrollAmount, behavior: 'smooth' });
      }
  };

</script>

<section
  id="siglo-{century.id}"
  class="century-section"
  class:even={index % 2 === 0}
  data-section
>
  <div class="container" style="font-family: Montserrat;">
    <div class="grid">
      <!-- Content -->
      <div class="content" class:order-2={index % 2 === 1}>
        <div class="header">
          <div class="title-section">
            <h2 class="century-title">{century.title}</h2>
            <p class="century-period">
              {century.period}
            </p>
          </div>
        </div>

        <p class="description" style="width: 850px;">{century.description}</p>

        <!-- Key Features -->
        <div class="features-section">
          <h3 class="features-title">
            Características Principales
          </h3>
          <div class="features-list">
            {#each century.keyFeatures as feature}
              <span class="feature-badge">{feature}</span>
            {/each}
          </div>
        </div>

        <div class="paired-section">
            <!-- Image Carousel -->
             <h3 class="features-title">
            Imágenes de los monumentos
          </h3>
            <div class="carousel-container">

              <button class="arrow left" on:click={() => scroll('left')} aria-label="Anterior">

                <svg viewBox="0 0 24 24" width="30" height="30" stroke="black" fill="none" stroke-width="2">
                  <path d="M15 6L9 12l6 6" />
                </svg>
              </button>


              <div class="carousel" bind:this={carouselContainer} > 
                {#each century.images as image}
                  <div class="image"> 
                    <img
                    class="carousel-image"
                    src={image.url}
                    alt={image.alt}
                    on:error={() => console.error("No se pudo cargar la imagen:", images[currentImageIndex])}
                    />
                    <div class="text">
                        <div class="title">{image.alt}</div>
                    </div>
                  </div>
                {/each}
              </div>
             

              <button class="arrow right" on:click={() => scroll('right')} aria-label="Siguiente">
                <svg viewBox="0 0 24 24" width="30" height="30" stroke="black" fill="none" stroke-width="2">
                  <path d="M9 6l6 6-6 6" />
                </svg>
              </button>



              </div>

            <!-- Flourish Chart Placeholder -->
            <h3 class="features-title" style="margin-bottom: 1rem;">
              Visualizaciones de los monumentos
            </h3>
            <h4 class="chart-title">{century.graphs.description}</h4>
            <div class="chart-card">
              <div class="grafico-flourish">
                <iframe
                  src={century.graphs.url}
                  title={century.graphs.description}
                  class="flourish-embed-iframe"
                  frameborder="0"
                  scrolling="no"
                  style="width: 100%; height: 600px;"
                  sandbox="allow-same-origin allow-forms allow-scripts allow-downloads allow-popups allow-popups-to-escape-sandbox allow-top-navigation-by-user-activation">
                </iframe>
                </div>
            </div>


        </div>
      </div>
    </div>
  </div>
</section>

<style>

  .century-section {
    padding: 3rem 2rem;
    background: var(--background-color);
    transition: all 0.6s ease;
    overflow-x: hidden;
  }
  
  .century-section.even {
    background: var(--clear-color);
  }
  
  .container {
    max-width: 1200px;
    margin: 0 auto;
  }
  
  .grid {
    display: grid;
    gap: 4rem;
    align-items: center;
  }
  
  .content {
    display: flex;
    flex-direction: column;
  }
  
  .order-2 {
    order: 2;
  }
  
  .header {
    display: flex;
    align-items: center;
    gap: 1rem;
  }
  
  .title-section {
    flex: 1;
  }
  
  .century-title {
    font-size: 2.5rem;
    font-weight: bold;
    color: var(--text-color);
    margin-bottom: 0.5rem;
  }
  
  .century-period {
    color: #db574D;
    font-weight: 500;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  
  .description {
    font-size: 1.125rem;
    line-height: 1.7;
    color: var(--text-color);
  }
  
  .features-section {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
  }
  
  .features-title {
    font-size: 1.25rem;
    font-weight: 600;
    color: var(--text-color);
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  
  .features-list {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
  }
  
  .feature-badge {
    background: #db574d53;
    color: var(--text-color);
    padding: 0.5rem 1rem;
    border-radius: 1rem;
    font-size: 0.875rem;
    font-weight: 500;
    border: 1px solid var(--grey-color);
  }

  .paired-section {
    display: column;
    width: 1380px;
    gap: 2rem;
    align-items: center;
    margin-top: 4rem;
  }
  
  .chart-card {
    background: white;
    border-radius: 0.75rem;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    width: 900px;
  }
  
  
  .chart-title {
    font-size: 1rem;
    font-weight: 500;
    color: var(--text-color);
    margin-bottom: 2rem;
  }
  
  .grafico-flourish {
  max-width: 800px;;
  margin: 2rem auto;
  font-family: "Montserrat";
}

  .carousel-container {
    position: relative;
    width: 80%;
    height: 400px;
    overflow: auto;
    display: flex;
    align-items: center;
  }

  .carousel {
    display: flex;
    gap: 2rem;
    overflow-x: hidden;
    scroll-behavior: smooth;
    padding: 1rem;
    scrollbar-width: thin;
    scrollbar-color: #db574d;
    overflow-y: hidden; 
    margin-top: 1px;
    z-index: 1;
  }

  .image {
    position: relative;
    flex: 0 0 200px;
    height: 300px;
    border-radius: 10px;
    overflow: hidden;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .image img {
      width: 100%;
      height: 100%;
      object-fit: cover; 
      display: block; 
  }

  .image:hover {
    transform: scale(1.1);
    box-shadow: 0 4px 15px rgba(250, 250, 249, 0.5);
    z-index: 5;
    border-radius: 10px;
  }

  .text {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    padding: 0.8rem;
    color: #383838;
    padding-bottom: 25px;
    transform: translateY(100%);
    transition: transform 0.3s ease-in-out;
    font-family: "Montserrat";
    background: #db574d;
    align-items: center;
    justify-content: center;
  }

  .image:hover .text {
    transform: translateY(0);
  }

  .title {
    font-size: 16px;
    font-family: "Montserrat";
    text-align: center;
  }


  .carousel-image {
    width: 500px;
    height: 500px;
    object-fit: cover;
    border-radius: 12px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
    transition: opacity 0.5s ease;
  }

  

  .arrow {
    background: transparent;
    border: none;
    cursor: pointer;
    padding: 0.5rem;
    border-radius: 50%;
    transition: background 0.3s ease;
    z-index: 10;
  }

  
  @media (max-width: 1024px) {
    .grid {
      grid-template-columns: 1fr;
      gap: 3rem;
    }
    
    .order-2 {
      order: unset;
    }
  }
  
  @media (max-width: 768px) {
    .century-section {
      padding: 4rem 1rem;
    }
    
    .header {
      flex-direction: column;
      text-align: center;
      gap: 1rem;
    }
    
    .century-title {
      font-size: 2rem;
    }
  }
</style>
