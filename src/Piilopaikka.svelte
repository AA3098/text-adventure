<script>
  import Modal from './Modal.svelte';
  import Nappi from './Nappi.svelte';
</script>

<Modal>
  <!--header slot-->
  <div slot="header">
    <div class="otsikko">
      <h1>You are hiding!</h1>
    </div>
    <hr class="hrviiva" />
  </div>
  <!--neutral slot-->
  <div class="laatikko">
    <h2>Shhh...</h2>
  </div>
  <!--footer slot-->
  <div slot="footer">
    <Nappi on:click>Exit hidingplace</Nappi>
  </div>
</Modal>
