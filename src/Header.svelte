<script>
  export let slogan;
</script>

<div class="header">
  <img src="./img/Torch_Gif.gif" alt="gargoyle left" width="100" height="100" />
  <div class="banner">
    <h1>{slogan}</h1>
  </div>
  <img
    src="./img/Torch_Gif.gif"
    alt="gargoyle right"
    width="100"
    height="100"
  />
</div>

<style>
  .header {
    /*border säädöt*/
    border-bottom: solid 1em;
    border-image: url('../img/wall2.png') 27 round stretch;
    /*flexbox järjestely*/
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: space-around;
    /*taustakuva*/
    background-color: rgb(0, 0, 0);
    background-size: cover;
    background-repeat: repeat;
    padding: 20px 20px 0px 20px;
  }

  .banner {
    padding: 20px;
    width: 1200px;
  }

  h1 {
    color: rgb(163, 0, 0);
    font-size: 3em;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  }
</style>
