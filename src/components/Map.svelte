<script>
  import { onMount } from 'svelte';

  onMount(() => {
    const script = document.createElement('script');
    script.src = "https://public.flourish.studio/resources/embed.js";
    script.async = true;
    document.body.appendChild(script);
  });
</script>

<section id="mapa" class="map-section">
  <div class="container">
    <div class="section-header">
      <h2 class="section-title">Mapa Interactivo</h2>
    </div>
    <p class="section-description">
        Descubrí la ubicación geográfica de todos los monumentos y explorá cómo se distribuyen a través de los
        continentes y las épocas.
      </p>

    <div class="map-card">
      <div
        class="flourish-embed flourish-story"
        data-src="story/3177558"
        data-url="https://flo.uri.sh/story/3177558/embed"
        data-height="100vh"
      ></div>
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
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  .section-title {
   font-size: 2.5rem;
    font-weight: bold;
    color: var(--text-color);
    margin-bottom: 0.5rem;
    flex: 1;
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
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
    padding: 2rem;
  }

  .flourish-embed {
    width: 100%;
    min-height: 600px;
  }

  @media (max-width: 768px) {
    .flourish-embed {
      min-height: 400px;
    }
  }
</style>