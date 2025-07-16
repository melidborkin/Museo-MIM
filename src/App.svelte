<script>
  import { onMount } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  import * as d3 from "d3";
  import monumentos from "./data/monumentos.csv";
  import {monumentosPorSiglo} from "./data/monumentosPorSiglo.js";
  import { monumentImages } from "./data/images.js"
  import { flourishCharts } from "./data/flourishCharts.js"


  // Componentes

  import Header from './components/Header.svelte';
  import LandingPage from './components/LandingPage.svelte';
  import Explanation from './components/Explanation.svelte';
  import CenturySection from './components/CenturySection.svelte';
  import Matriz from './components/Matriz.svelte';
  import Map from './components/Map.svelte';
  import Footer from './components/Footer.svelte';

  // Estado de la aplicación
  let scrollY = 0;
  let innerHeight = 0; 
  let isMenuOpen = false; 
  let svgMap = {};
  let activeColumn = null;
  let tooltip;
  let floatingElements = [];
  let salaActual = null
  let showHeader = false
  let activeCentury = 0;


  // Datos de las salas (siglos)
  const rooms = [
    { id: 1, century: 'XI', years: '1000-1099', title: 'Siglo XI: Los Cimientos', description: 'Los primeros grandes monumentos del milenio', images: monumentImages[1] }, 
    { id: 2, century: 'XII', years: '1100-1199', title: 'Siglo XII: El Románico', description: 'Catedrales y fortalezas medievales', images: monumentImages[2] },
    { id: 3, century: 'XIII', years: '1200-1299', title: 'Siglo XIII: El Gótico', description: 'Torres que tocan el cielo', images: monumentImages[3] }, 
    { id: 4, century: 'XIV', years: '1300-1399', title: 'Siglo XIV: Transición', description: 'Entre lo medieval y lo renacentista', images: monumentImages[4]  },
    { id: 5, century: 'XV', years: '1400-1499', title: 'Siglo XV: Renacimiento Temprano', description: 'El despertar artístico', images: monumentImages[5]  },
    { id: 6, century: 'XVI', years: '1500-1599', title: 'Siglo XVI: Renacimiento Pleno', description: 'La perfección arquitectónica', images: monumentImages[6]  },
    { id: 7, century: 'XVII', years: '1600-1699', title: 'Siglo XVII: Barroco', description: 'Ornamentación y grandeza', images: monumentImages[7]  },
    { id: 8, century: 'XVIII', years: '1700-1799', title: 'Siglo XVIII: Neoclásico', description: 'Vuelta a los clásicos', images: monumentImages[8]  },
    { id: 9, century: 'XIX', years: '1800-1899', title: 'Siglo XIX: Revolución Industrial', description: 'Hierro, acero y nuevas posibilidades', images: monumentImages[9]  },
    { id: 10, century: 'XX', years: '1900-1999', title: 'Siglo XX: Modernidad', description: 'Rascacielos y arquitectura contemporánea', images: monumentImages[10]  }
  ];


  
  const centuries = [
    {
      id: "xi",
      number: "XI",
      title: "Siglo XI",
      description: "Los primeros monumentos medievales que marcaron el inicio de una nueva era arquitectónica. El arte románico florece con sus características bóvedas de cañón y muros gruesos.",
      period: "1001-1100",
      keyFeatures: ["Arte Románico", "Bóvedas de Cañón", "Muros Gruesos", "Peregrinaciones"],
      monuments: [
        { name: "Catedral de Santiago de Compostela", location: "España", year: "1075" },
        { name: "Abadía de Cluny", location: "Francia", year: "1088" },
        { name: "Torre de Londres", location: "Inglaterra", year: "1078" }
      ],
      images: monumentImages[1],
      graphs: flourishCharts[1]

    },
    {
      id: "xii",
      number: "XII",
      title: "Siglo XII",
      description: "El apogeo del arte románico y los primeros destellos del gótico. Las catedrales comienzan a elevarse hacia el cielo con nuevas técnicas constructivas.",
      period: "1101-1200",
      keyFeatures: ["Transición Gótica", "Arcos Apuntados", "Vidrieras", "Verticalidad"],
      monuments: [
        { name: "Notre-Dame de París", location: "Francia", year: "1163" },
        { name: "Catedral de Chartres", location: "Francia", year: "1194" },
        { name: "Angkor Wat", location: "Camboya", year: "1113" }
      ],
      images: monumentImages[2],
      graphs: flourishCharts[2] 
    },
    {
      id: "xiii",
      number: "XIII",
      title: "Siglo XIII",
      description: "La consolidación del gótico y la expansión arquitectónica. Las vidrieras alcanzan su máximo esplendor iluminando los interiores sagrados.",
      period: "1201-1300",
      keyFeatures: ["Gótico Clásico", "Rosetones", "Arbotantes", "Luz Divina"],
      monuments: [
        { name: "Sainte-Chapelle", location: "Francia", year: "1248" },
        { name: "Catedral de Reims", location: "Francia", year: "1211" },
        { name: "Alhambra", location: "España", year: "1238" }
      ],
      images: monumentImages[3] ,
      graphs: flourishCharts[3]
    },
    {
      id: "xiv",
      number: "XIV",
      title: "Siglo XIV",
      description: "Transición hacia nuevas formas arquitectónicas. El gótico tardío experimenta con formas más decorativas y complejas.",
      period: "1301-1400",
      keyFeatures: ["Gótico Tardío", "Decoración Flamígera", "Complejidad", "Ornamentación"],
      monuments: [
        { name: "Palacio de los Papas", location: "Francia", year: "1335" },
        { name: "Catedral de Milán", location: "Italia", year: "1386" },
        { name: "Mezquita Azul", location: "Turquía", year: "1356" }
      ],
      images: monumentImages[4],
      graphs: flourishCharts[4] 
    },
    {
      id: "xv",
      number: "XV",
      title: "Siglo XV",
      description: "El Renacimiento transforma la arquitectura europea. Regreso a los órdenes clásicos y nuevas técnicas de construcción.",
      period: "1401-1500",
      keyFeatures: ["Renacimiento", "Órdenes Clásicos", "Perspectiva", "Humanismo"],
      monuments: [
        { name: "Cúpula de Brunelleschi", location: "Italia", year: "1436" },
        { name: "Palacio Medici", location: "Italia", year: "1444" },
        { name: "Machu Picchu", location: "Perú", year: "1450" }
      ],
      images: monumentImages[5],
      graphs: flourishCharts[5]
    },
    {
      id: "xvi",
      number: "XVI",
      title: "Siglo XVI",
      description: "El Renacimiento pleno y la expansión global. Grandes maestros como Miguel Ángel redefinen la arquitectura.",
      period: "1501-1600",
      keyFeatures: ["Alto Renacimiento", "Manierismo", "Expansión Global", "Maestros"],
      monuments: [
        { name: "Basílica de San Pedro", location: "Vaticano", year: "1506" },
        { name: "Palacio de Versalles", location: "Francia", year: "1559" },
        { name: "Taj Mahal", location: "India", year: "1632" }
      ],
      images: monumentImages[6],
      graphs: flourishCharts[6]
    },
    {
      id: "xvii",
      number: "XVII",
      title: "Siglo XVII",
      description: "El Barroco revoluciona el arte y la arquitectura. Dramatismo, movimiento y ornamentación exuberante.",
      period: "1601-1700",
      keyFeatures: ["Barroco", "Dramatismo", "Movimiento", "Ornamentación"],
      monuments: [
        { name: "Palacio de Schönbrunn", location: "Austria", year: "1696" },
        { name: "Plaza de San Pedro", location: "Vaticano", year: "1656" },
        { name: "Palacio de Peterhof", location: "Rusia", year: "1714" }
      ],
      images: monumentImages[7],
      graphs: flourishCharts[7] 
    },
    {
      id: "xviii",
      number: "XVIII",
      title: "Siglo XVIII",
      description: "Neoclasicismo y la búsqueda de la perfección clásica. Vuelta a la simplicidad y proporción griega y romana.",
      period: "1701-1800",
      keyFeatures: ["Neoclasicismo", "Simplicidad", "Proporción", "Ilustración"],
      monuments: [
        { name: "Panteón de París", location: "Francia", year: "1758" },
        { name: "Casa Blanca", location: "Estados Unidos", year: "1792" },
        { name: "Puerta de Brandeburgo", location: "Alemania", year: "1791" }
      ],
      images: monumentImages[8],
      graphs: flourishCharts[8] 
    },
    {
      id: "xix",
      number: "XIX",
      title: "Siglo XIX",
      description: "Revolución industrial y nuevos materiales arquitectónicos. El hierro y el acero transforman las posibilidades constructivas.",
      period: "1801-1900",
      keyFeatures: ["Revolución Industrial", "Hierro y Acero", "Ingeniería", "Modernidad"],
      monuments: [
        { name: "Torre Eiffel", location: "Francia", year: "1889" },
        { name: "Big Ben", location: "Inglaterra", year: "1859" },
        { name: "Estatua de la Libertad", location: "Estados Unidos", year: "1886" }
      ],
      images: monumentImages[9],
      graphs: flourishCharts[9] 
    },
    {
      id: "xx",
      number: "XX",
      title: "Siglo XX",
      description: "Modernismo y la arquitectura contemporánea. Ruptura con el pasado y búsqueda de nuevas formas de expresión.",
      period: "1901-2000",
      keyFeatures: ["Modernismo", "Funcionalismo", "Nuevos Materiales", "Vanguardia"],
      monuments: [
        { name: "Casa de la Cascada", location: "Estados Unidos", year: "1935" },
        { name: "Ópera de Sídney", location: "Australia", year: "1973" },
        { name: "Guggenheim Bilbao", location: "España", year: "1997" }
      ],
      images: monumentImages[10],
      graphs: flourishCharts[10] 
    }
  ];

  // Monumentos por siglo
  const sigloXI = monumentosPorSiglo["1000"];
  const sigloXII = monumentosPorSiglo["1100"];
  const sigloXIII = monumentosPorSiglo["1200"];
  const sigloXIV = monumentosPorSiglo["1300"];
  const sigloXV = monumentosPorSiglo["1400"];
  const sigloXVI = monumentosPorSiglo["1500"];
  const sigloXVII = monumentosPorSiglo["1600"];
  const sigloXVIII = monumentosPorSiglo["1700"];
  const sigloXIX = monumentosPorSiglo["1800"];
  const sigloXX = monumentosPorSiglo["1900"];

  // Variables para el scroller
  let count;
  let index = 0; 
  let offset;
  let progress;
  let top = 0.1;
  let threshold = 0.5;
  let bottom = 0.9;

  
  function scrollToSection(sectionId) {
    const element = document.getElementById(sectionId);
    if (element) {
      element.scrollIntoView({ behavior: 'smooth' });
    }
    isMenuOpen = false;
  }
  
  function startTour() {
    scrollToSection('explanation');
  }

  let previousCentury = -1;

  $: if (activeCentury !== previousCentury) {
    const sectionEl = document.getElementById('century-section');
    if (sectionEl) {
      sectionEl.scrollIntoView({ behavior: 'smooth', block: 'start' });
    }
    previousCentury = activeCentury;
  }

  // Funciones de accesibilidad
  function handleKeydown(event, callback) { 
    if (event.key === 'Enter' || event.key === ' ') {
      event.preventDefault(); 
      callback();
    }
  }

  function handleEscapeKey(event) {
    if (event.key === 'Escape') {
      isMenuOpen = false;
    }
  }
  

onMount(() => {
  window.addEventListener('scroll', calcularSeccionActual);
});
  onMount(() => {
    
    const handleScroll = () => {
      scrollY = window.scrollY;
      updateWalkingPerson();
    };
    
    window.addEventListener('scroll', handleScroll);
    window.addEventListener('keydown', handleEscapeKey);
    
    return () => {
      window.removeEventListener('scroll', handleScroll);
      window.removeEventListener('keydown', handleEscapeKey);
    };
  });
  
  $: showHeader = scrollY > innerHeight * 0.8;


</script>

<svelte:window bind:scrollY bind:innerHeight /> 

<div class="app">

  <main>
  <Header {showHeader} />

  <LandingPage />

  <Explanation />

  <Matriz monuments={monumentos} />

  <hr class="divider" />


  <div class="sections-container">

    <div class="selector-salas">
      {#each centuries as century, i}
        <button 
          class:selected={i === activeCentury}
          on:click={() => activeCentury = i}
        >
          {century.number}
        </button>
      {/each}
    </div>
    <p class="description"> Para entrar a la sala deseada presioná en el Siglo correspondiente </p>

    <div id="century-section">
      <CenturySection 
        century={centuries[activeCentury]} 
        index={activeCentury} 
      />
    </div>

    <div class="nav-container">
      <div class="nav-salas">
        <button on:click={() => activeCentury = Math.max(0, activeCentury - 1)} disabled={activeCentury === 0}>
          ← Anterior
        </button>

        <span>Sala {activeCentury + 1} / {centuries.length}</span>

        <button on:click={() => activeCentury = Math.min(centuries.length - 1, activeCentury + 1)} disabled={activeCentury === centuries.length - 1}>
          Siguiente →
        </button>
      </div>
      </div>

  </div>
    
  <Map />
  
  <Footer />
</main>

</div>

<style>
  main {
    position: relative;
  }
  
  .sections-container {
    position: relative;
  }

  .divider {
    max-width: 1200px;
    align-content: center;
    margin: auto;
  }
  
  .selector-salas {
  display: flex;
  gap: 1rem;
  margin: 8rem auto 2rem auto;
  max-width: 1200px;
  padding: 3rem, 2rem;
  font-size: 2rem;
  font-weight: bold;
  color: var(--text-color);
  font-family: "Montserrat";
  }
  
  .description {
    font-size: 1.125rem;
    line-height: 1.7;
    color: var(--text-color);
    margin: 4rem auto 2rem auto;
    max-width: 1200px;
    padding: 3rem, 2rem;
    font-family: "Montserrat";
  }

.selector-salas button {
  border-radius: 1rem;
  background: white;
  border: 1px solid var(--grey-color);
  cursor: pointer;
  transition: all 0.2s ease;
}

.selector-salas button.selected {
  background: var(--brand-color);
  color: #db574d;
  font-weight: bold;
  transform: scale(1.05);
}
.nav-container{
  align-items: flex-start;
  margin: 4rem auto 2rem auto;
}

.selector-salas {
  display: flex;
  gap: 1rem;
  margin: 8rem auto 2rem auto;
  max-width: 1200px;
  padding: 3rem, 2rem;
  font-size: 2rem;
  font-weight: bold;
  color: var(--text-color);
  font-family: "Montserrat";
  }
  

.nav-salas {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1.5rem;
  margin: 4rem auto 2rem auto;
  padding: 1rem;
  border-radius: 1rem;
  background: linear-gradient(135deg, #fbeae8, #ffffff);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
  max-width: 1200px;
  font-family: Montserrat, sans-serif;
}

.nav-salas span {
  font-size: 1rem;
  font-weight: 500;
  color: var(--text-color);
}

.nav-salas button {
  background-color: #db574d;
  color: white;
  border: none;
  padding: 0.6rem 1.2rem;
  border-radius: 2rem;
  font-weight: 600;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.nav-salas button:hover:not(:disabled) {
  background-color: #c44e45;
  transform: scale(1.05);
}

.nav-salas button:disabled {
  background-color: #f3c1bd;
  cursor: not-allowed;
  opacity: 0.6;
}


</style>