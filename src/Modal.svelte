<script>
  import { fly, scale } from 'svelte/transition';
</script>

<div class="backdrop" on:click />

<div
  class="modal"
  in:fly={{ duration: 1000, x: 0, y: -500 }}
  out:scale={{ duration: 300 }}
>
  <header>
    <slot name="header" />
  </header>

  <slot />

  <footer>
    <slot name="footer">
      <button on:click>Sulje</button>
    </slot>
  </footer>
</div>

<style>
  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background: rgba(0, 0, 0, 0.75);
    z-index: 10;
  }

  .modal {
    padding: 1rem;
    position: fixed;
    top: 10vh;
    left: 10%;
    width: 80%;
    max-height: 80vh;
    background: rgb(0, 0, 0);
    background-size: cover;
    background-repeat: repeat;
    border-radius: 25px;
    z-index: 100;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  }

  header {
    border-bottom: 1px solid #ccc;
  }
</style>
