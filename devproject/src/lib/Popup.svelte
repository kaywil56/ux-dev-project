<script>
  import { onMount } from "svelte";
    export let close;
  let time = 0;
  let start_calling;
  onMount(async () => {
    showPopUp();
  });

  const showPopUp = () => {
    // start time update after popup modal show
    start_calling = setInterval(countdownTime, 250); // call every one sec
  };

  const countdownTime = () => {
    time++;
    if (time == 10) {
      clearInterval(start_calling); // stop calling
      close()
    }
  };
</script>

<div>Dialog</div>

<style>
  div {
    z-index: 100;
    position: absolute;
    top: 50%;
    left: 50%;
  }
</style>
