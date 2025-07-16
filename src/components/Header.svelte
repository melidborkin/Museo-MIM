<script>
  import { createEventDispatcher } from 'svelte';
  
  export let showHeader = false;
  export let onfoo;
  
  let isMenuOpen = false;
  
  const dispatch = createEventDispatcher();
  
  function scrollToSection(sectionId) {
    const element = document.getElementById(sectionId);
    if (element) {
      element.scrollIntoView({ behavior: 'smooth' });
    }
    isMenuOpen = false; 
  }
  
  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
    if (onfoo) {
      onfoo();
    }
  }
</script>

{#if showHeader}
  <header class="header" style="font-family: Montserrat;">
    <div class="header-content">
      <div class="logo logo-font">MIM</div>

      
      <nav class="desktop-nav">
        <button on:click={() => scrollToSection('explanation')}>Home</button>
        <button on:click={() => scrollToSection('glosario')}>Glosario</button>
        <button on:click={() => scrollToSection('siglo-xi')}>Salas</button>
        <button on:click={() => scrollToSection('mapa')}>Mapa</button>
      </nav>
      
      <button class="mobile-menu-btn" on:click={toggleMenu} aria-label="Toggle menu">
        <span></span>
        <span></span>
        <span></span>
      </button>
    </div>
    
    {#if isMenuOpen}
      <div class="mobile-menu">
        <button on:click={() => scrollToSection('explanation')}>Explicación</button>
        <button on:click={() => scrollToSection('siglo-xi')}>Salas</button>
        <button on:click={() => scrollToSection('glosario')}>Matriz</button>
        <button on:click={() => scrollToSection('mapa')}>Mapa</button>
      </div>
    {/if}
  </header>
{/if}

<style>

.header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    border-bottom: 1px solid var(--grey-color);
    animation: slideDown 0.3s ease-out;
  }
  
  @keyframes slideDown {
    from { transform: translateY(-100%); }
    to { transform: translateY(0); }
  }
  
  .header-content {
    max-width: vw;
    margin: 0 auto;
    padding: 1rem 2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .logo {
    font-size: 2.5rem;
    color: #db574d;
    font-size: 40px;
    font-weight: 400;
    font-family: 'Monoton', cursive;
  }
  
  .desktop-nav {
    display: flex;
    gap: 2rem;
  }
  
  .desktop-nav button {
    background: none;
    border: none;
    color: var(--text-color);
    font-family: var(--font-family);
    font-size: 1rem;
    font-weight: 500;
    cursor: pointer;
    padding: 0.5rem 1rem;
    border-radius: 8px;
    transition: all 0.3s ease;
  }
  
  .desktop-nav button:hover {
    color: #db574d;
    background: var(--clear-color);
  }
  
  .mobile-menu-btn {
    display: none;
    flex-direction: column;
    gap: 4px;
    background: none;
    border: none;
    cursor: pointer;
    padding: 8px;
  }
  
  .mobile-menu-btn span {
    width: 25px;
    height: 3px;
    background: var(--text-color);
    border-radius: 2px;
    transition: all 0.3s ease;
  }
  
  .mobile-menu {
    background: white;
    border-top: 1px solid var(--grey-color);
    padding: 1rem 2rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
  
  .mobile-menu button {
    background: none;
    border: none;
    color: var(--text-color);
    font-family: var(--font-family);
    font-size: 1rem;
    font-weight: 500;
    cursor: pointer;
    padding: 0.75rem;
    text-align: left;
    border-radius: 8px;
    transition: all 0.3s ease;
  }
  
  .mobile-menu button:hover {
    color: var(--brand-color);
    background: var(--clear-color);
  }
  
  @media (max-width: 768px) {
    .desktop-nav { display: none; }
    .mobile-menu-btn { display: flex; }
  }
</style>
