<script lang="ts">
  import { marked } from 'marked';
  import { onMount } from 'svelte';
  import blurbMd from './assets/blurb.md?raw';
  import aboutMd from './assets/about.md?raw';
  import linkedinLogo from './assets/linkedin-logo.png';
  import githubLogo from './assets/github-logo.png';
  import linktreeLogo from './assets/linktree-logo.png';
  import chevron from './assets/chevron.png';

  // State ================
  let isLoaded = $state<boolean>(false);

  // ======================
  
  // For client-side, just construct URLs directly
  const urlEndpoint = "https://ik.imagekit.io/3sz0exlmi";
  const motorcycleImagePath = `${urlEndpoint}/motorcycle`;
  
  // Convert markdown to HTML
  const blurbHtml = marked(blurbMd);
  const aboutHtml = marked(aboutMd);

  // typewriter animation ================
  const textValue = "Melvin Gruschow"
  const intervalValue = 130;
  const delay = textValue.length * intervalValue;

  onMount(() => {
    const nameElement = document.getElementById('name');

    const textArray = textValue?.split('');

    let count = 1;

    let interval = setInterval(() => {
      if(textArray && nameElement) {
        nameElement.textContent = textArray.slice(0,count).join('');

        if(count === textArray.length) {

          setTimeout(() => {
            document.getElementById('blinking-cursor')?.setAttribute('hidden', 'true');
          }, 2000)

          clearInterval(interval)
        } else {
          count++;
        };
      }

    }, intervalValue)

    setTimeout(() => {
      isLoaded = true;
    }, 10)

    setTimeout(() => {
      document.getElementById('links')!.classList.add('fade-in');
    }, delay)
  })


  // =====================================
</script>

<header>
  <div class="name-container">
    <span id="name"></span>
    &nbsp;
    <span id="blinking-cursor">_</span>
  </div>

  <div class="links-container" id="links">
    <a class="social-link" href="https://www.linkedin.com/in/melvin-gruschow/" target="_blank"><img width="2rem" height="2rem" src={linkedinLogo} alt="LinkedIn" aria-label="Link to Melvin Gruschow's LinkedIn profile" title="LinkedIn" /></a>
    <a class="social-link" href="https://www.github.com/mfgruschow" target="_blank"><img width="2rem" height="2rem" src={githubLogo} alt="GitHub" aria-label="Link to Melvin Gruschow's GitHub profile" title="GitHub" /></a>
    <a class="social-link" href="https://linktr.ee/fealios" target="_blank"><img width="2rem" height="2rem" src={linktreeLogo} alt="LinkTree" aria-label="Link to Melvin Gruschow's LinkTree profile" title="LinkTree" /></a>
  </div>
</header>

<main>
    <div id="image-blurb-wrapper">
      <div class="blurb" id="first-blurb">
        {@html blurbHtml}
      </div>
      {#if !isLoaded}
      <div id="image-skeleton">
      </div>
      {:else} 
      <img fetchpriority="high" width="1600px" height="900px" id="main-image" class="main-image" src={motorcycleImagePath} alt="Main" aria-label="Main" />
      <img id="chevron" src={chevron} alt="Chevron" aria-label="Chevron"  />
      {/if}

    </div>

    {#if isLoaded}
      <div id="about-container">
        <div>
          {@html aboutHtml}
        </div>
      </div>
    {/if}
  
</main>

<style>
  .links-container {
    visibility: hidden;
    margin-top: 10px;

    .social-link {
      color: initial;
      text-decoration: none;

      img {
        width: 2rem;
        height: auto;
      }
    }
  }

  #image-skeleton {
    position: relative;
    height: calc(100vh - var(--header-height));
    width: 100vw;
    background-color: var(--color-denim);
  }

  #image-blurb-wrapper {
    position: relative;
    #chevron {
      position: absolute;
      left: 50%;
      bottom: 3rem;
      width: 2rem;
    }
  }

  @media (max-width: 768px) {
  #image-blurb-wrapper {
    display: flex;
    flex-direction: column;
    flex-flow: column-reverse;

    #chevron {
      display: none;
    }
  }

  #image-blurb-wrapper .blurb {
    position: relative;
    top: 0;
    left:0;
    width: 100vw;
    padding: 1rem;
  }

  #image-blurb-wrapper .blurb :global(h2) {
    font-size: 1.5rem;
  }

  #image-blurb-wrapper .blurb :global(p) {
    font-size: 1rem;
    word-break: break-all;
  }

    #image-skeleton {
      height: 50vh;
    }

    header {
      font-size: .7rem;
    }
}
  
  #about-container {
    color: white;
    z-index: var(--main-z-value);
  }


</style>
