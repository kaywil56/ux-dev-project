<script>
  import { onMount } from "svelte";
  import { fade } from 'svelte/transition';
    export let close;
  let time = 0;
  let start_calling;
  onMount(async () => {
    showPopUp();
  });

  const showPopUp = () => {
    // start time update after popup modal show
    start_calling = setInterval(countdownTime, 1000); // call every one sec
  };

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
