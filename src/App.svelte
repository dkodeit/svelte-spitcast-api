<script>
  import Spots from "./Spots.svelte";
  import Spot from "./Spot.svelte";
  import { fade } from "svelte/transition";

  let selected;
  let county = "san-diego";
  let spot;
  let name;

  let counties = [
    { id: "san-diego", title: "San Diego" },
    { id: "orange-county", title: "Orange County" },
    { id: "marin", title: "Marin" },
    { id: "san-francisco", title: "San Francisco" },
    { id: "san-mateo", title: "San Mateo" },
    { id: "santa-cruz", title: "Santa Cruz" },
    { id: "monterey", title: "Monterey" },
    { id: "san-luis-obispo", title: "San Luis Obispo" },
    { id: "santa-barbara", title: "Santa Barbara" },
    { id: "ventura", title: "Ventura" },
    { id: "los-angeles", title: "Los Angeles" }
  ];

  function handleForecast(event) {
    spot = event.detail.id;
    window.scrollTo({
      top: 0,
      behavior: "smooth"
    });
  }

  function handleSpotName(event) {
    name = event.detail.name;
  }
</script>

<style>
  .container {
    max-width: 1000px;
    margin: 0 auto;
    padding: 2rem;
  }
  .container > div:first-child {
    display: flex;
    align-items: center;
  }
  h3 {
    margin: 0 0 0.5rem 1rem;
    background: var(--lighter);
    color: var(--dark);
    width: 100%;
    padding: 0.35rem;
    text-align: center;
    border: 1px solid var(--light);
  }
  .grid {
    display: flex;
  }
  aside {
    width: calc(200px - 1rem);
    padding-right: 1rem;
  }
  main {
    width: 800px;
    background: var(--lighter);
  }
  select {
    width: calc(200px - 1rem);
  }
  .intro {
    background: var(--lighter);
    text-align: center;
    text-transform: uppercase;
  }
</style>

<div class="container">
  <div>
    <form>
      <select bind:value={selected} on:change={() => (county = selected)}>
        {#each counties as county}
          <option value={county.id}>{county.title}</option>
        {/each}
      </select>
    </form>
    <h3>{spot ? name : 'Select a Surf Spot'}</h3>
  </div>
  <div class="grid">
    <aside>
      <Spots {county} on:forecast={handleForecast} on:name={handleSpotName} />
    </aside>
    <main>
      {#if spot}
        <div transition:fade={{ delay: 25, duration: 300 }}>
          <Spot {spot} />
        </div>
      {:else}
        <div class="intro">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320">
            <path
              fill="#0099ff"
              fill-opacity="1"
              d="M0,160L14.1,170.7C28.2,181,56,203,85,202.7C112.9,203,141,181,169,149.3C197.6,117,226,75,254,74.7C282.4,75,311,117,339,117.3C367.1,117,395,75,424,85.3C451.8,96,480,160,508,170.7C536.5,181,565,139,593,149.3C621.2,160,649,224,678,261.3C705.9,299,734,309,762,293.3C790.6,277,819,235,847,229.3C875.3,224,904,256,932,240C960,224,988,160,1016,117.3C1044.7,75,1073,53,1101,69.3C1129.4,85,1158,139,1186,144C1214.1,149,1242,107,1271,106.7C1298.8,107,1327,149,1355,170.7C1383.5,192,1412,192,1426,192L1440,192L1440,0L1425.9,0C1411.8,0,1384,0,1355,0C1327.1,0,1299,0,1271,0C1242.4,0,1214,0,1186,0C1157.6,0,1129,0,1101,0C1072.9,0,1045,0,1016,0C988.2,0,960,0,932,0C903.5,0,875,0,847,0C818.8,0,791,0,762,0C734.1,0,706,0,678,0C649.4,0,621,0,593,0C564.7,0,536,0,508,0C480,0,452,0,424,0C395.3,0,367,0,339,0C310.6,0,282,0,254,0C225.9,0,198,0,169,0C141.2,0,113,0,85,0C56.5,0,28,0,14,0L0,0Z" />
          </svg>
        </div>
      {/if}
    </main>
  </div>
</div>
