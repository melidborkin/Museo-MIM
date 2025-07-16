<script>
  const teamMembers = [
    {
      name: "Guadalupe Koruk",
      email: "gkoruk@mail.utdt.edu",
      linkedin: "https://www.linkedin.com/in/guadalupe-koruk"
    },
    {
      name: "Paz Yunes",
      email: "pyunes@mail.utdt.edu", 
      linkedin: "https://www.linkedin.com/in/paz-yunes"
    },
    {
      name: "Melina Dborkin",
      email: "mdborkin@mail.utdt.edu",
      linkedin: "https://www.linkedin.com/in/melinadborkin"
    }
  ];
</script>

<section id="footer" class="footer">
  <div class="container">
    <div class="footer-content">
      
      <div class="footer-section" style="color: black">   
        <p style='margin: 0' ><strong>Visualización de datos</strong> | Tecnología Digital - Universidad Torcuato Di Tella</p>
        <br>
        <br>
        <div class="team-grid" style='margin: 0' >
          {#each teamMembers as member}
            <div class="team-member" >
              <strong>{member.name}</strong>
              <div class="member-links">
                <a href="mailto:{member.email}" class="email-link">{member.email}</a>
                {#if member.linkedin}
                  <span class="separator">|</span>
                  <a href={member.linkedin} target="_blank" rel="noopener noreferrer" class="linkedin-link">
                    LinkedIn
                  </a>
                {/if}
              </div>
            </div>
          {/each}
        </div>
        <br>
        
        <div class="github-section"  style='margin-top: 0rem' >
          <a href="https://github.com/melidborkin/Monumentos-Interactivo" target="_blank" rel="noopener noreferrer" class="github-link">
            Ver proyecto en GitHub
          </a>
        </div>
      </div>
    </div>
    
    <div class="footer-bottom">
      <p>&copy; 2025 Museo Internacional de Monumentos. Todos los derechos reservados.</p>
    </div>
  </div>
</section>

<style>
  .footer {
    background: radial-gradient(transparent 70%, rgba(0,0,0,0.2) 100%);
    color: black;
    font-family: "Montserrat";
    padding: 4rem 2rem 4rem;
    align-items: center;
    display: flex;
    justify-content: center;
    height: max-content;
  }
  
  .container {
    max-width: 1200px;
    margin: 0 auto;
  }
  
  .footer-content {
    display: flex;
    flex-direction: wrap;
    text-align: center;
    gap: 3rem;
    margin-bottom: 3rem;
  }
  
  .footer-section {
    font-size: 1rem;
    text-align: center;
    line-height: 1.6;
    color: black;
  }

  .team-grid {
    display: flex;
    flex-direction: row;
    gap: 3rem;
    align-items: center;
    margin: 1rem 0;
  }

  .team-member {
    text-align: center;
  }

  .member-links {
    margin-top: 0.5rem;
    font-size: 0.9rem;
  }

  .email-link, .linkedin-link {
    color: var(--brand-color);
    text-decoration: none;
    transition: color 0.3s ease;
  }

  .email-link:hover, .linkedin-link:hover {
    color: var(--bad-color);
    text-decoration: underline;
  }

  .separator {
    margin: 0 0.5rem;
    color: var(--text-color);
  }

  .github-section {
    margin-top: 2rem;
  }

  .github-link {
    display: inline-flex;
    align-items: center;
    color: #db574D;
    text-decoration: none;
    font-weight: 600;
    border: 2px solid var(--brand-color);
    border-radius: 8px;
    transition: all 0.3s ease;
  }

  .github-link:hover {
    color: #db574D;
    transform: translateY(-2px);
  }

  .footer-bottom {
    border-top: 1px solid rgba(255,255,255,0.2);
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
  }
  
  .footer-bottom p {
    opacity: 0.7;
    font-size: 0.9rem;
  }
  
  
  @media (max-width: 768px) {
    .footer {
      padding: 3rem 1rem 2rem;
    }
    
    .team-grid {
      gap: 1.5rem;
    }
    
    .footer-bottom {
      flex-direction: column;
      text-align: center;
    }
  }
</style>
