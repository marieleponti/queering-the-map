<script lang="ts">
  import '$lib/style.css';
  import '$lib/maplibre_style.css';
  import '$lib/navbar_buttons.css';
  import AddOverlay from '$lib/AddOverlay.svelte';
  import InfoOverlay from '$lib/InfoOverlay.svelte';
  import Map from '$lib/Map.svelte';
  import NavBar from '$lib/NavBar.svelte';
  import SearchBox from '$lib/SearchBox.svelte';
  import { addOverlayVisible, infoOverlayVisible } from '../stores';
  import qtm_sharing_image from '$lib/assets/qtm_sharing_image.jpg';

  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  let mapComponent: any;

  function handleLocationSelect(event: CustomEvent) {
    const { lng, lat } = event.detail;
    if (mapComponent?.flyTo) {
      mapComponent.flyTo(lng, lat, 13); // Zoom level 13
    }
  }
</script>

<svelte:head>
  <title>RESISTING BORDER TECH</title>
  <meta name="description" content="" />

  <meta property="og:type" content="website" />
  <meta property="og:url" content="" />
  <meta property="og:title" content="" />
  <meta property="og:image" content={qtm_sharing_image} />
  <meta property="og:description" content="" />
  <meta property="og:site_name" content="" />
  <meta property="og:locale" content="en_US" />
  <meta
    name="google-site-verification"
    content="J-oUgPBYWRXkPhvqF8XBrDtrA-qu8pXiMiYPd3QrfhE"
  />

  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:title" content="" />
  <meta name="twitter:description" content="" />
  <meta name="twitter:image" content={qtm_sharing_image} />
</svelte:head>

<NavBar></NavBar>
<div class="search-wrapper">
  <SearchBox on:select={handleLocationSelect} />
</div>
{#if $infoOverlayVisible}
  <InfoOverlay></InfoOverlay>
{/if}
{#if $addOverlayVisible}
  <AddOverlay></AddOverlay>
{/if}
<Map bind:this={mapComponent}></Map>

<style>
  .search-wrapper {
    position: fixed;
    top: 70px;
    left: 20px;
    z-index: 100;
  }

  @media (max-width: 800px) {
    .search-wrapper {
      top: 60px;
      left: 10px;
      right: 10px;
    }
  }
</style>
