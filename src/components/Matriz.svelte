<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";
  import monumentos from "/src/data/monumentos.csv";
  import { fade, fly } from 'svelte/transition';

  let svgMap = {};
  let activeColumn = null;
  let tooltip;

  // Convierte los valores altura, anio y visitas a números
  monumentos.forEach(m => {
    m.altura = +m.altura;
    m.anio = +m.anio;
    m.visitas = +m.visitas;
  });

  // Asigna un color específico a cada calificación ("Malo", "Regular", "Bueno", "Excelente").
  const colorCalificacion = d3.scaleOrdinal()
    .domain(["Malo", "Regular", "Bueno", "Excelente"])
    .range(["#912F27", "#EA7B4D", "#A2D4D3", "#3B7B78"]);

  // Lista fija de continentes para estructurar la matriz visual (eje Y)
  const continentes = ["África", "América", "Asia", "Europa", "Oceanía"];

  // Genera los siglos del milenio (del 1000 al 1900, eje X)
  const siglos = Array.from({ length: 10 }, (_, i) => 1000 + i * 100);

  // Función para convertir siglos a números romanos
  function sigloARomano(siglo) {
    const sigloNumero = siglo / 100;
    const romanos = ["XI", "XII", "XIII", "XIV", "XV", "XVI", "XVII", "XVIII", "XIX", "XX"];
    return romanos[sigloNumero - 10];
  }

  // Función para obtener el siglo base desde un año
  function obtenerSiglo(anio) {
    if (!anio) return null;
    return Math.floor(anio / 100) * 100;
  }

  // Devuelve el nombre del archivo SVG según la altura
  function svgPorAltura(altura) {
    if (altura < 18) return "menosde18.svg";
    if (altura < 25) return "18a25.svg";
    if (altura < 30) return "25a30.svg";
    if (altura < 38) return "30a38.svg";
    if (altura < 47) return "38a47.svg";
    if (altura < 60) return "47a60.svg";
    if (altura < 80) return "60a80.svg";
    if (altura < 130) return "80a130.svg";
    return "masde130.svg";
  }

  async function fetchSVGs() { 
    const nombres = monumentos.map(m => svgPorAltura(m.altura));
    const unicos = Array.from(new Set(nombres));
    // Modifica el contenido del SVG para permitir cambiar el color con CSS
    for (const nombre of unicos) {
      const res = await fetch(`/images/${nombre}`);
      let raw = await res.text();
      raw = raw.replace(/fill="(?!none).*?"/g, 'fill="currentColor"');
      svgMap[nombre] = raw;
    }
  }

  onMount(() => {
    monumentos.forEach(m => {
      m.altura = +m.altura;
      m.anio = +m.anio;
      m.visitas = +m.visitas;
      m.calificacion = m.calificacion?.trim();
      m.continente = m.continente?.trim();
    });
    fetchSVGs();
  });

  function showTooltip(event, m) {
    if (!tooltip) return;
    tooltip.innerHTML = `
      <div class="tooltip-header" style="background-color: ${colorCalificacion(m.calificacion)}">
        <strong>${m.nombre}</strong>
      </div>
      <div class="tooltip-content">
        <div class="tooltip-row"><span class="tooltip-label">Continente:</span> ${m.continente}</div>
        <div class="tooltip-row"><span class="tooltip-label">Altura:</span> ${m.altura.toLocaleString()} m</div>
        <div class="tooltip-row"><span class="tooltip-label">Año:</span> ${m.anio}</div>
        <div class="tooltip-row"><span class="tooltip-label">Visitas:</span> ${m.visitas} M</div>
        <div class="tooltip-row"><span class="tooltip-label">Calificación:</span> ${m.calificacion}</div>
      </div>
    `;
    tooltip.style.display = "block";
    tooltip.style.left = event.pageX + 15 + "px";
    tooltip.style.top = event.pageY + 15 + "px";
  }

  function hideTooltip() {
    if (!tooltip) return;
    tooltip.style.display = "none";
  }

  function showSigloTooltip(event, siglo) {
    if (!tooltip) return;
    tooltip.innerHTML = `
      <div class="tooltip-header tooltip-header-siglo">
        <strong>Siglo ${sigloARomano(siglo)}</strong>
      </div>
      <div class="tooltip-content">
        <div class="tooltip-row">Años ${siglo} - ${siglo + 99}</div>
      </div>
    `;
    tooltip.style.display = "block";
    tooltip.style.left = event.pageX + 15 + "px";
    tooltip.style.top = event.pageY + 15 + "px";
  }

  function setActiveColumn(siglo) {
    activeColumn = siglo;
  }

  function clearActiveColumn() {
    activeColumn = null;
  }

  // Filtros
  let filtrosFormas = [];
  let filtrosColores = [];
  let filtrosSiglos = [];
  let filtrosContinentes = [];

  // Toggle de valores dentro de un filtro
  function toggleValor(array, valor) {
    console.log("Toggle valor:", valor, "en array:", array);
    const index = array.indexOf(valor);
    if (index === -1) {
      array.push(valor);
      console.log("Agregado. Array ahora:", array);
    } else {
      array.splice(index, 1);
      console.log("Removido. Array ahora:", array);
    }
    
    // Forzar reactividad de manera más explícita
    if (array === filtrosFormas) {
      filtrosFormas = [...filtrosFormas];
    } else if (array === filtrosColores) {
      filtrosColores = [...filtrosColores];
    } else if (array === filtrosSiglos) {
      filtrosSiglos = [...filtrosSiglos];
    } else if (array === filtrosContinentes) {
      filtrosContinentes = [...filtrosContinentes];
    }
  }

  $: monumentosFiltrados = monumentos.filter(m => {
    const forma = svgPorAltura(m.altura);
    const color = m.calificacion;
    const continente = m.continente;
    const siglo = obtenerSiglo(m.anio);

    return (!filtrosFormas.length || filtrosFormas.includes(forma)) &&
          (!filtrosColores.length || filtrosColores.includes(color)) &&
          (!filtrosContinentes.length || filtrosContinentes.includes(continente)) &&
          (!filtrosSiglos.length || filtrosSiglos.includes(siglo));
  });

  function cumpleFiltros(m) {
    const forma = svgPorAltura(m.altura)?.trim();
    const color = m.calificacion?.trim();
    const continente = m.continente?.trim();
    const siglo = Math.floor(m.anio / 100) * 100;

    return (!filtrosFormas.length || filtrosFormas.includes(forma)) &&
          (!filtrosColores.length || filtrosColores.includes(color)) &&
          (!filtrosContinentes.length || filtrosContinentes.includes(continente)) &&
          (!filtrosSiglos.length || filtrosSiglos.includes(siglo));
  }

  // Función para obtener monumentos por celda (limitado a 9)
  function getMonumentosPorCelda(continente, siglo) {
    return monumentos
      .filter(m => 
        m.continente === continente &&
        m.anio >= siglo &&
        m.anio < siglo + 100 &&
        cumpleFiltros(m)
      )
      .slice(0, 9);
  }

  function limpiarFiltros() {
    filtrosFormas = [];
    filtrosColores = [];
    filtrosSiglos = [];
    filtrosContinentes = [];
  }
</script>

<section id="glosario" class="matrix-section">
  <div class="global-container">
    <div class="header">
      <div class="title-section">
        <h2 class="title">Glosario del MIM</h2>
      </div>
    </div>

        <p class="description">
        Antes de empezar el recorrido de cada sala <strong>visualizá los monumentos construidos entre el año 1000 y 1999</strong>.
        <br>Cada columna representa un siglo. 
        <br>Cada fila, un continente. 
        <br>Cada figura, un monumento.  
        <br>Al seleccionar las figuras que están a continuación, filtras el glosario según tus intereses y preferencias.     
      </p>
  
  <div class="sistema">
    <div class="leyenda-container">
      <div class="altura">
        <div class='titulo-sistema'>Altura (medida en metros)</div>

        <div class="formas">
          <button class="forma" 
            class:selected={filtrosFormas.includes("menosde18.svg")}  
            on:click={() => toggleValor(filtrosFormas, "menosde18.svg")}>
            <img src="/images/menosde18.svg" alt="Menos de 18 metros de altura"/>
            <span>Menos de 18</span>
          </button>
          <button class="forma"
            class:selected={filtrosFormas.includes("18a25.svg")}  
            on:click={() => toggleValor(filtrosFormas, "18a25.svg")}>
            <img src="/images/18a25.svg" alt="18–25 metros de altura"/>
            <span>18–25</span>
          </button>
          <button class="forma" 
            class:selected={filtrosFormas.includes("25a30.svg")}  
            on:click={() => toggleValor(filtrosFormas, "25a30.svg")}>
            <img src="/images/25a30.svg" alt="25–30 metros de altura"/>
            <span>25–30</span>
          </button>
          <button class="forma" 
            class:selected={filtrosFormas.includes("30a38.svg")}  
            on:click={() => toggleValor(filtrosFormas, "30a38.svg")}>
            <img src="/images/30a38.svg" alt="Entre 30 y 38 metros de altura"/>
            <span>30–38</span>
          </button>
          <button class="forma" 
            class:selected={filtrosFormas.includes("38a47.svg")}  
            on:click={() => toggleValor(filtrosFormas, "38a47.svg")}>
            <img src="/images/38a47.svg" alt="Entre 38 y 47 metros de altura"/>
            <span>38–47</span>
          </button>
          <button class="forma" 
            class:selected={filtrosFormas.includes("47a60.svg")}  
            on:click={() => toggleValor(filtrosFormas, "47a60.svg")}>
            <img src="/images/47a60.svg" alt="Entre 47 y 60 metros de altura"/>
            <span>47–60</span>
          </button>
          <button class="forma" 
            class:selected={filtrosFormas.includes("60a80.svg")}  
            on:click={() => toggleValor(filtrosFormas, "60a80.svg")}>
            <img src="/images/60a80.svg" alt="Entre 60 y 80 metros de altura"/>
            <span>60–80</span>
          </button>
          <button class="forma" 
            class:selected={filtrosFormas.includes("80a130.svg")}  
            on:click={() => toggleValor(filtrosFormas, "80a130.svg")}>
            <img src="/images/80a130.svg" alt="Entre 80 y 130 metros de altura"/>
            <span>80–130</span>
          </button>
          <button class="forma" 
            class:selected={filtrosFormas.includes("masde130.svg")}  
            on:click={() => toggleValor(filtrosFormas, "masde130.svg")}>
            <img src="/images/masde130.svg" alt="Más de 130 metros de altura"/>
            <span>Más de 130</span>
          </button>
        </div>
      </div>

      <div class="calificacion"> 
        <div class='titulo-sistema' style="margin-top: 2rem;">Calificación (medido en cantidad de visitas)</div>
        <div class="colores">
          <button class="color"
            class:selected={filtrosColores.includes("Malo")}
            on:click={() => toggleValor(filtrosColores, 'Malo')}>
            <div class="color-circle" style="background-color: #912F27;"></div>
            <span class="color-name">Malo</span>
          </button>
          <button class="color"
            class:selected={filtrosColores.includes("Regular")}
            on:click={() => toggleValor(filtrosColores, 'Regular')}>
            <div class="color-circle" style="background-color: #EA7B4D;"></div>
            <span class="color-name">Regular</span>
          </button>
          <button class="color"
            class:selected={filtrosColores.includes("Bueno")}
            on:click={() => toggleValor(filtrosColores, 'Bueno')}>
            <div class="color-circle" style="background-color: #A2D4D3;"></div>
            <span class="color-name">Bueno</span>
          </button>
          <button class="color"
            class:selected={filtrosColores.includes("Excelente")}
            on:click={() => toggleValor(filtrosColores, 'Excelente')}>
            <div class="color-circle" style="background-color: #3B7B78;"></div>
            <span class="color-name">Excelente</span>
          </button>
        </div>
      </div>

      <!-- Botón para limpiar filtros -->
      {#if filtrosFormas.length > 0 || filtrosColores.length > 0 || filtrosSiglos.length > 0 || filtrosContinentes.length > 0}
        <div class="filtros-actions">
          <button class="limpiar-btn" on:click={limpiarFiltros}>
            Limpiar todos los filtros
          </button>
        </div>
      {/if}
    </div>
  </div>

  <div class="matriz-container">
    
    <!-- Fila de siglos en números romanos (arriba) -->
    <div class="fila fila-siglos">
      <div class="label-y" style="font-size: 18px; font-weight: 600px">Siglos</div>
      <div class="celdas-header">
        {#each siglos as siglo}
          <button 
            type="button"
            class="siglo-label" 
            class:active={activeColumn === siglo}
            class:selected={filtrosSiglos.includes(siglo)}
            on:click={() => toggleValor(filtrosSiglos, siglo)}
            on:mouseenter={(e) => {
              setActiveColumn(siglo);
              showSigloTooltip(e, siglo);
            }}
            on:mouseleave={() => {
              clearActiveColumn();
              hideTooltip();
            }}
            aria-label={`Siglo ${sigloARomano(siglo)}, años ${siglo} a ${siglo + 99}`}
          >
            {sigloARomano(siglo)}
          </button>
        {/each}
      </div>
    </div>
  
    <div class="matriz">
      {#each continentes as cont}
        <div class="fila">
          <button type="button"
                  class="label-y"
                  class:selected={filtrosContinentes.includes(cont)}
                  on:click={() => toggleValor(filtrosContinentes, cont)}>
            {cont}
          </button>
          <div class="celdas">
            {#each siglos as siglo}
              <div class="celda" class:active-celda={activeColumn === siglo}>
                <div class="grid-3x3">
                  {#each monumentosFiltrados.filter(m => m.continente === cont && obtenerSiglo(m.anio) === siglo).slice(0, 9) as m}

                    <div class="monumento visible"
                        style="color: {colorCalificacion(m.calificacion)}"
                        role = "img"
                        on:mousemove={(e) => showTooltip(e, m)}
                        on:mouseleave={hideTooltip}>

                      {@html svgMap[svgPorAltura(m.altura)]}
                    </div>
                  {/each}
                </div>
              </div>
            {/each}
          </div>
        </div>
      {/each}
    </div>

  </div>

  <div class="tooltip" bind:this={tooltip}></div>
</div>
</section>

<style>

  .matrix-section {
    padding: 3rem 2rem;
    background: var(--clear-color);
    min-height: 100vh;
    transition: all 0.6s ease;
  }

  .global-container {
    max-width: 1200px;
    margin: 0 auto;
  }

  h2 {
    font-family: "Montserrat";
    font-size: 1.1rem;
    font-weight: 400;
    line-height: 1.6;
    color: var(--text-color);
    opacity: 0.8;
    margin: 0 auto;
  }

  .header {
    display: flex;
    align-items: center;
    justify-content: left;
    gap: 1rem;
    margin-top: 4rem;
  }
  
  .title-section {
    flex: 1;
  }
  
  .title {
    font-size: 2.5rem;
    font-weight: bold;
    color: black;
    margin-bottom: 0.5rem;
    font-family: "Montserrat";
  }
  
  .description {
    font-size: 1.125rem;
    line-height: 2.3;
    color: var(--text-color);
    font-family: "Montserrat";
    margin-bottom: 6rem;
  }

  .sistema {
    margin: -3rem auto 4rem;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    position: relative;
    z-index: 2;
    font-family: "Montserrat";
  }

  .leyenda-container {
    flex-direction: column;
    justify-content: left;
    align-items: flex-start;
    gap: 3rem;
    border-radius: 12px;
  }

  .titulo-sistema {
    font-size: 1.125rem;
    line-height: 1.7;
    color: var(--text-color);
    font-family: "Montserrat";
    font-weight: 600;
  }

  .altura, .calificacion {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .colores {
    display: flex;
    gap: 2.5rem;
    align-items: center;
    flex-wrap: wrap;
    justify-content: left;
    margin-bottom: 15px;
  }

  .color {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    min-width: 100px;
    background: none;
    border: none;
    cursor: pointer;
    font-family: var(--font-family);
  }

  .color-circle {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    margin-bottom: 12px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    transition: transform 0.3s ease;
  }

  .color:hover .color-circle {
    transform: scale(1.1);
  }

  .color.selected .color-circle {
    box-shadow: 0 0 0 4px var(--primary-color);
    transform: scale(1.1);
    transition: all 0.2s ease;
  }

  .color-name {
    font-family: var(--font-family);
    font-weight: 600;
    font-size: 14px;
    margin-bottom: 4px;
    color: var(--text-color);
    letter-spacing: 0.3px;
  }

  .formas {
    display: flex;
    gap: 1.8rem;
    align-items: center;
    margin-top: 1.5rem;
    flex-wrap: wrap;
    justify-content: center;
  }

  .forma {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    transition: transform 0.3s ease;
    font-family: var(--font-family);
    background: none;
    border: none;
    cursor: pointer;
  }

  .forma img {
    width: 55px;
    height: 55px;
    margin-bottom: 10px;
    transition: transform 0.3s ease;
  }

  .forma:hover {
    transform: translateY(-5px);
  }

  .forma.selected {
    filter: drop-shadow(0 0 5px var(--primary-color));
    transform: translateY(-4px);
    transition: all 0.2s ease;
  }

  .forma span {
    font-family: var(--font-family);
    font-weight: 500;
    font-size: 13px;
    color: var(--text-color);
    letter-spacing: 0.3px;
    text-align: center;
  }

  .limpiar-btn {
    background: var(--brand-color);
    color: #333;
    border: none;
    border-radius: 8px;
    font-family: var(--font-family);
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    text-align: left;
    align-items: flex-start;
  }

  .limpiar-btn:hover {
    transform: translateY(-2px);
  }

  .filtros-actions {
    display: flex;
    justify-content: center;
    margin-top: 1rem;
  }

  .matriz-container {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin-top: 4rem;
    min-width: min-content;
    width: 1280px;
    position: relative;
  }

  .fila {
    display: flex;
    align-items: center;
    gap: 10px;
    width: 100%;
    margin-bottom: 15px;
  }

  .fila-siglos {
    padding-bottom: 1rem;
    border-bottom: 1px solid var(--grey-color);
  }

  .label-y {
    width: 80px;
    text-align: left;
    font-size: 16px;
    color: var(--text-color);
    flex-shrink: 0;
    display: flex;
    align-items: flex-start;
    justify-content: flex-start;
    height: 100%;
    margin-right: 1rem;
    font-family: var(--font-family);
    background: none;
    border: none;
    cursor: pointer;
    padding: 4px 8px;
    border-radius: 4px;
    transition: all 0.3s ease;
  }

  .label-y.selected {
    background-color: var(--clear-color);
    color: var(--primary-color);
    font-weight: 700;
  }

  .celdas-header {
    display: grid;
    grid-template-columns: repeat(10, 90px);
    gap: 15px;
  }

  .siglo-label {
    text-align: center;
    font-size: 18px;
    font-weight: 500px;
    color: var(--text-color);
    padding: 8px 0;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.3s ease;
    font-family: var(--font-family);
    background: none;
    border: none;
  }

  .siglo-label:hover, .siglo-label.active {
    color: var(--brand-color);
    transform: translateY(-3px);
  }

  .siglo-label.selected {
    background-color: var(--clear-color);
    color: var(--primary-color);
    transform: translateY(-2px);
    font-weight: 600;
    border-radius: 6px;
  }

  .matriz {
    display: flex;
    flex-direction: column;
    gap: 15px;
    width: 100%;
  }

  .celdas {
    display: grid;
    grid-template-columns: repeat(10, 90px);
    gap: 15px;
  }

  .celda {
    width: 90px;
    height: 90px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    padding: 5px;
    background-color: var(--clear-color);
    border-radius: 8px;
    position: relative;
    transition: all 0.3s ease;
    box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  }

  .active-celda {
    background-color: var(--background-color);
    box-shadow: 0 0 0 2px var(--primary-color);
    transform: translateY(-3px);
  }

  .grid-3x3 {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    width: 100%;
    height: 100%;
    gap: 2px;
    justify-items: start;
  }

  .monumento {
    width: 25px;
    height: 25px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: transform 0.3s ease, filter 0.3s ease;
    opacity: 1;
  }

  .monumento:hover {
    transform: scale(1.2);
    filter: drop-shadow(0 3px 6px rgba(0,0,0,0.2));
    z-index: 5;
  }

  .tooltip {
    position: absolute;
    background: white;
    color: #333;
    border-radius: 8px;
    pointer-events: none;
    z-index: 100;
    display: none;
    max-width: 250px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.15);
    overflow: hidden;
    font-family: 'Montserrat', sans-serif;
    border: none;
    padding: auto;
  }

  

  @media (max-width: 1200px) {
    .formas, .colores {
      flex-wrap: wrap;
      justify-content: center;
    }
    .leyenda-container {
      padding: 2rem;
    }
  }

  @media (max-width: 768px) {
    .matriz-container {
      padding: 1.5rem;
    }
    .label-y {
      width: 60px;
      font-size: 14px;
    }
  }
</style>
