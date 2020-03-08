<script>
  let currentImage = 0;
  let images = new Array(60);
  let interval;
  let loaded = false;

  function load() {
    loaded = true;
  }

  function next() {
    currentImage++;
    if (currentImage > images.length - 1) {
      currentImage = 0;
    }
  }
  function back() {
    currentImage--;
    if (currentImage < 0) {
      currentImage = images.length - 1;
    }
  }

  function startLooping({ target }) {
    interval = setInterval(() => {
      target.dataset.direction === "next" ? next() : back();
    }, 80);
  }
  function stopLooping() {
    clearInterval(interval);
  }
</script>

<style>
  .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: centerß;
  }
  .player {
    display: grid;
  }
  .player img {
    grid-area: 1/1;
    max-width: 1000px;
  }
  .appearing {
    opacity: 1;
  }
  .disappearing {
    opacity: 0;
    transition: opacity 0.1s linear;
  }
  .buttons {
    margin: 1em;
  }
</style>

<div class="wrapper">
  {#if loaded}
    <div class="player">
      {#each images as image, index}
        <img
          src="frames/frame{index}.jpg"
          class={index === currentImage ? 'appearing' : 'disappearing'}
          alt="Frame" />
      {/each}
    </div>
    <div class="buttons">
      <button on:mousedown={startLooping}>Back</button>
      <button data-direction="next" on:mousedown={startLooping}>Next</button>
    </div>
  {:else}
    <p>Loading...</p>
  {/if}
</div>

<svelte:window on:mouseup={stopLooping} on:load={load} />
