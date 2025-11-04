<script lang="ts">
  import Map from '$lib/Map.svelte';
  import '$lib/style.css';
</script>

<svelte:head>
  <title>Map Embed</title>
</svelte:head>

<div class="embed-container">
  <Map />
</div>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    overflow: hidden;
  }

  .embed-container {
    width: 100vw;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
  }

  /* Ocultar elementos innecesarios en embed */
  :global(.navbar),
  :global(.donate-popup),
  :global(.info-button) {
    display: none !important;
  }

  /* Ajustar el mapa para ocupar todo el espacio */
  :global(.map-container) {
    height: 100vh !important;
    width: 100vw !important;
  }
</style>
