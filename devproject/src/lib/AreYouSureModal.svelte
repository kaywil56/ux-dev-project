<script>
  // Function to close modal from parent
  export let close;
  // Function passed by parent (either cancel class / clear all / submit attendance)
  export let method;
  // Message passed by parent
  export let message;
</script>

<div id="modal">
  <div id="modal-content">
    Are you sure you want to {message}?
    <button on:click|preventDefault={method} on:click={close}>Yes</button>
    <button on:click={close}>No</button>
  </div>
</div>

<style>
  #modal {
    z-index: 3;
    position: fixed;
    left: 0;
    top: 0;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.4);
    display: grid;
    place-items: center;
  }
  #modal-content {
    background-color: #fefefe;
    border-radius: 10px;
    margin: auto;
    padding: 20px;
    width: 40%;
  }
</style>
