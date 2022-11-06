<script>
  import { onMount } from "svelte";
  import { fade } from 'svelte/transition';
    export let close;
  let time = 0;
  let start_calling;
  onMount(async () => {
    showPopUp();
  });
  
  // start time update after popup modal show
  const showPopUp = () => {
    start_calling = setInterval(countdownTime, 1000); // call every one sec
  };

  // Once time reaches 2 close notifiction
  const countdownTime = () => {
    time++;
    if (time == 2) {
      clearInterval(start_calling); // stop calling
      close()
    }
  };
</script>

<p transition:fade>Saved</p>

<style>
  p{
    color: green;
    display: inline;
  }
</style>
