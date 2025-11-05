<script lang="ts">
  import { createEventDispatcher } from 'svelte';

  interface NominatimResult {
    lat: string;
    lon: string;
    display_name: string;
  }

  const dispatch = createEventDispatcher<{
    select: { lng: number; lat: number; name: string };
  }>();

  let searchQuery = '';
  let searchResults: NominatimResult[] = [];
  let isLoading = false;
  let showResults = false;

  async function searchLocation() {
    if (searchQuery.length < 3) {
      searchResults = [];
      return;
    }

    isLoading = true;
    const url = `https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(searchQuery)}&limit=5`;

    try {
      const response = await fetch(url, {
        headers: {
          'User-Agent': 'QueeringMap/1.0'
        }
      });

      searchResults = await response.json();
      showResults = true;
    } catch (error) {
      console.error('Error searching:', error);
    } finally {
      isLoading = false;
    }
  }

  function selectLocation(result: NominatimResult) {
    dispatch('select', {
      lng: parseFloat(result.lon),
      lat: parseFloat(result.lat),
      name: result.display_name
    });
    searchQuery = '';
    searchResults = [];
    showResults = false;
  }

  let debounceTimer: ReturnType<typeof setTimeout>;
  function handleInput() {
    clearTimeout(debounceTimer);
    debounceTimer = setTimeout(searchLocation, 500);
  }
</script>

<div class="search-container">
  <input
    type="text"
    bind:value={searchQuery}
    on:input={handleInput}
    on:focus={() => (showResults = searchResults.length > 0)}
    placeholder="Search for a city or location..."
    class="search-input"
  />

  {#if isLoading}
    <div class="search-loading">Searching...</div>
  {/if}

  {#if showResults && searchResults.length > 0}
    <div class="search-results">
      {#each searchResults as result}
        <button
          class="search-result-item"
          on:click={() => selectLocation(result)}
        >
          {result.display_name}
        </button>
      {/each}
    </div>
  {/if}
</div>

<style>
  .search-container {
    position: relative;
    width: 100%;
    max-width: 400px;
  }

  .search-input {
    width: 100%;
    padding: 10px 15px;
    font-size: 14px;
    border: 1.01px solid var(--color-dark);
    background-color: var(--color-pink);
    font-family: 'Apfel Grotezk', sans-serif;
    box-sizing: border-box;
  }

  .search-input:focus {
    outline: none;
    border-color: var(--color-dark);
  }

  .search-loading {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    padding: 10px;
    background: var(--color-pink);
    border: 1.01px solid var(--color-dark);
    border-top: none;
    font-size: 12px;
  }

  .search-results {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background: var(--color-pink);
    border: 1.01px solid var(--color-dark);
    border-top: none;
    max-height: 300px;
    overflow-y: auto;
    z-index: 1000;
  }

  .search-result-item {
    width: 100%;
    padding: 10px 15px;
    text-align: left;
    background: none;
    border: none;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
    cursor: pointer;
    font-size: 13px;
    font-family: 'Apfel Grotezk', sans-serif;
    color: var(--color-dark);
  }

  .search-result-item:hover {
    background-color: rgba(0, 0, 0, 0.05);
  }

  .search-result-item:last-child {
    border-bottom: none;
  }
</style>
