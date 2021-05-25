<script>
  import { createEventDispatcher } from 'svelte';
  import { tweened } from 'svelte/motion';

  const dispatch = createEventDispatcher();

  export let killervaroitus;

  const ajastin = tweened(1, { duration: 10000 });

  if (killervaroitus) {
    ajastin.set(0);
  }

  setTimeout(() => {
    dispatch('hide');
  }, 10000);
</script>

<div>
  <div class="harmaa">
    <p>You hear loud footsteps... You should hide!</p>
    <br />
    <progress value={$ajastin} />
  </div>
</div>

<style>
  .harmaa {
    position: fixed;
    top: 2rem;
    left: 2rem;
    width: 200px;
    height: 100px;
    background-color: gray;
    border: 2px solid darkgray;
    border-radius: 25px;
    padding: 1rem;
    text-align: center;
  }
</style>
