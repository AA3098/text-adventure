<script>
  import Modal from './Modal.svelte';
  import Nappi from './Nappi.svelte';
  import kuolema from './kuolema';

  export let deathreason;
</script>

<Modal>
  <!--header slot-->
  <div slot="header">
    <div class="otsikko">
      <img src="./img/skull1.png" alt="skull left" width="100" height="100" />

      <h1>The dungeon claims you as its own</h1>
      <img src="./img/skull2.png" alt="skull right" width="100" height="100" />
    </div>
    <hr class="hrviiva" />
  </div>
  <!--neutral slot-->
  <div class="laatikko">
    <h2>You have met a terrible fate, haven't you...</h2>
    <p>
      You died because {#if deathreason === 1}{$kuolema[0]}
      {:else if deathreason === 2}{$kuolema[1]}
      {:else if deathreason === 3}{$kuolema[2]}
      {:else if deathreason === 4}{$kuolema[3]}
      {/if}
    </p>
    <p>But death is not the end of your journey, my friend!</p>
    <hr />
    <p>Will you try again?</p>
  </div>
  <!--footer slot-->
  <div slot="footer">
    <Nappi on:click>Mama raised no quitter!</Nappi>
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
