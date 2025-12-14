<script lang="ts">
  import { marked } from 'marked';
  import blurbMd from './assets/blurb.md?raw';
  import { onMount } from 'svelte';
  
  // For client-side, just construct URLs directly
  const urlEndpoint = "https://ik.imagekit.io/3sz0exlmi";
  const mainImagePath = `${urlEndpoint}/main-image`;
  const motorcycleImagePath = `${urlEndpoint}/motorcycle`;
  const fujiImagePath = `${urlEndpoint}/mt-fuji`;
  
  // Convert markdown to HTML
  const blurbHtml = marked(blurbMd);

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

  })


  // =====================================
</script>

<header>
  <div class="name-container">
    <span id="name"></span>
    &nbsp;
    <span id="blinking-cursor">_</span>
  </div>
</header>

<main>
  <div class="blurb" id="first-blurb">
    {@html blurbHtml}
  </div>
  <img class="main-image" src={motorcycleImagePath} alt="Main" aria-label="Main" />

</main>

<style>

</style>
