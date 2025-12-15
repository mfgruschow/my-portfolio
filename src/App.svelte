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
  let mainImageBottomEdge = $state<undefined | number>(undefined);

  // ======================
  
  // For client-side, just construct URLs directly
  const urlEndpoint = "https://ik.imagekit.io/3sz0exlmi";
  const mainImagePath = `${urlEndpoint}/main-image`;
  const motorcycleImagePath = `${urlEndpoint}/motorcycle`;
  const fujiImagePath = `${urlEndpoint}/mt-fuji`;
  
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
      document.getElementById('links')!.classList.add('fade-in');
    }, delay)

    document.getElementById('main-image')!.addEventListener('load', () => {
      mainImageBottomEdge = document.getElementById('main-image')!.getBoundingClientRect().bottom;
    });
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
    <a class="social-link" href="https://www.linkedin.com/in/melvin-gruschow/" target="_blank"><img src={linkedinLogo} alt="LinkedIn" aria-label="Link to Melvin Gruschow's LinkedIn profile" title="LinkedIn" /></a>
    <a class="social-link" href="https://www.github.com/mfgruschow" target="_blank"><img src={githubLogo} alt="GitHub" aria-label="Link to Melvin Gruschow's GitHub profile" title="GitHub" /></a>
    <a class="social-link" href="https://linktr.ee/fealios" target="_blank"><img src={linktreeLogo} alt="LinkTree" aria-label="Link to Melvin Gruschow's LinkTree profile" title="LinkTree" /></a>
  </div>
</header>

<main>
  <div id="image-blurb-wrapper">
    <div class="blurb" id="first-blurb">
      {@html blurbHtml}
    </div>
    <img id="main-image" class="main-image" src={motorcycleImagePath} alt="Main" aria-label="Main" />
    <img id="chevron" src={chevron} alt="Chevron" aria-label="Chevron"  />
  </div>

  <div id="about-container">
    <div>
      {@html aboutHtml}
    </div>
  </div>
  
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

  #image-blurb-wrapper {
    position: relative;
    #chevron {
      position: absolute;
      left: 50%;
      bottom: 3rem;
      width: 4rem;
    }
  }
  
  #about-container {
    color: white;
    z-index: var(--main-z-value);
  }


</style>
