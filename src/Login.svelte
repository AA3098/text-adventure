<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  let name = '';

  //eventit
  const nimeys = () => dispatch('nimeys', name);

  //validointi
  const onkoValidi = (teksti) => teksti.trim().length > 0;
  $: validiNimi = onkoValidi(name);

  $: virhe = false;

  const virhetoggle = () => {
    virhe = true;
    virhe = virhe;
  };
</script>

<div>
  <h1>What is thy name?</h1>
  {#if virhe & !validiNimi}
    <p class="virhe">Thou must write thy name!</p>
  {/if}
  <input
    type="text"
    placeholder="Adventurer"
    bind:value={name}
    on:blur={virhetoggle}
    class:tyhja={virhe && !validiNimi}
  />
  <button on:click={nimeys} disabled={!validiNimi}>is my name</button>
</div>

<style>
  .tyhja {
    border: 2px solid red;
  }

  .virhe {
    color: red;
    margin: 0.5rem 0;
  }
</style>
