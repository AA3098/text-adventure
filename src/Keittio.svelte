<script>
  import { createEventDispatcher } from 'svelte';
  import Modal from './Modal.svelte';
  import inventory from './inventory';

  const dispatch = createEventDispatcher();

  const otapot = () => dispatch('takepot');
  const otakettle = () => dispatch('takekettle');
  const otacauldron = () => dispatch('takecauldron');
  const otatakapakki = () => dispatch('takapakki');
</script>

<Modal>
  <!--header slot-->
  <div slot="header">
    <div class="otsikko">
      <h1>You stand in front of the cookware</h1>
    </div>
    <hr class="hrviiva" />
  </div>
  <!--neutral slot-->
  <div class="laatikko">
    <h2>In front of you are three different cookwares</h2>
    <p>Small pots that could be used to make food for couple of people.</p>
    <p>Medium sized kettles that you could use to feed a small family.</p>
    <p>
      Comically large cauldron that, if full, could feed hundreds of people.
    </p>
    {#if $inventory[1] === 'pot'}
      <h2 class="item">*Pot acquired*</h2>
    {:else if $inventory[1] === 'kettle'}
      <h2 class="item">*Kettle acquired*</h2>
    {:else if $inventory[1] === 'cauldron'}
      <h2 class="item">*Cauldron acquired*</h2>
    {/if}
    <hr />
    <p>What do you do?</p>
  </div>
  <!--footer slot-->
  <div slot="footer">
    <button disabled={$inventory[1] === 'pot'} on:hover on:click={otapot}
      >Take the pot</button
    >
    <button disabled={$inventory[1] === 'kettle'} on:click={otakettle}
      >Take the kettle</button
    >
    <button disabled={$inventory[1] === 'cauldron'} on:click={otacauldron}
      >Take the cauldron</button
    >
    <button on:click={otatakapakki}>Take a step back</button>
  </div>
</Modal>

<style>
  h1 {
    color: rgb(255, 255, 255);
    font-size: 3em;
  }

  p,
  h2 {
    color: rgb(255, 255, 255);
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  }

  .item {
    color: rgb(204, 173, 0);
  }

  .otsikko {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: space-around;
  }

  .laatikko {
    max-width: 900px;
    margin: 1em auto;
    padding: 2em;
    background-color: black;
    border: 2px solid rgb(255, 255, 255);
    border-radius: 25px;
  }
</style>
