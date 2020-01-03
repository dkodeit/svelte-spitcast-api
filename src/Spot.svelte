<script>
  export let spot;
  let forecast;

  $: fetch(`http://api.spitcast.com/api/spot/forecast/${spot}/`)
    .then(r => r.json())
    .then(data => {
      forecast = data;
    });
</script>

<style>
  .spot {
    background: var(--lighter);
    padding: 0.5rem;
    text-align: center;
    position: sticky;
    top: 1rem;
  }
  .details,
  .header {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    grid-gap: 0.5rem;
  }
  .header {
    background: var(--dark);
    color: var(--lighter);
    padding: 0.75rem 0.25rem;
    text-transform: uppercase;
  }
  .details {
    padding: 0.75rem 0.25rem;
  }
  .details:nth-child(odd) {
    background: var(--light);
  }
</style>

<!-- <pre>{JSON.stringify(forecast, null, 2)}</pre> -->

<div class="spot">
  {#if forecast}
    <div class="header">
      <div>day</div>
      <div>hour</div>
      <div>shape</div>
      <div>size</div>
      <div>swell</div>
      <div>tide</div>
      <div>wind</div>
    </div>
    {#each forecast as { day, hour, shape_full, size, shape_detail }, i}
      <div class="details">
        <div>{day}</div>
        <div>{hour}</div>
        <div>{shape_full}</div>
        <div>{size} ft</div>
        <div>{shape_detail.swell}</div>
        <div>{shape_detail.tide}</div>
        <div>{shape_detail.wind}</div>
      </div>
    {/each}
  {:else}
    <p>fetching...</p>
  {/if}
</div>
