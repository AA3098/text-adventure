<script>
  import inventory from './inventory';
</script>

<div class="reppu">
  <h1>\ Backpack /</h1>
  {#each $inventory as tavara}
    <p>- {tavara}</p>
  {/each}
</div>

<style>
  h1 {
    text-decoration: underline;
  }

  .reppu {
    /*paikoitus*/
    position: fixed;
    top: 2rem;
    right: 2rem;
    /*ulkonäkö*/
    width: 200px;
    background-color: rgb(83, 40, 0);
    color: rgb(255, 255, 255);
    border: 3px solid rgb(34, 16, 0);
    border-radius: 25px;
    padding: 1em;
    text-align: center;
  }
</style>
