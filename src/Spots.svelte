<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let county;

  let spots;
  let current;

  $: fetch(`http://api.spitcast.com/api/county/spots/${county}/`)
    .then(r => r.json())
    .then(data => {
      spots = data;
    });

  function getForecast(id) {
    dispatch("forecast", {
      id: id
    });
  }

  function setSpotName(name) {
    dispatch("name", {
      name: name
    });
  }
</script>

<style>
  .active {
    background: var(--dark);
    color: var(--lighter);
  }
  .spots {
    display: grid;
    grid-gap: 0.5rem;
  }
  button {
    margin: 0;
    width: 100%;
    text-align: left;
    cursor: pointer;
    padding: 0.5rem 0.75rem;
  }
</style>

<!-- <pre>{JSON.stringify(spots, null, 2)}</pre> -->

<div class="spots">
  {#if spots}
    {#each spots as item, i}
      <div class="spot">
        <button
          class:active={current === item.spot_name}
          on:click={() => (current = item.spot_name)}
          on:click={getForecast(item.spot_id)}
          on:click={setSpotName(item.spot_name)}>
          {item.spot_name}
        </button>
      </div>
    {/each}
  {:else}
    <div>
      <p class="loading">fetching...</p>
    </div>
  {/if}
</div>
