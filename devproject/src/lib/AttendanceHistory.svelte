<script>
  export let studentHistory;
</script>

<ol>
  {#each studentHistory as status, i}
    {#if status[0]}
        <li>Week: {i + 1} | Class 1: {status[0]} | Class 2: {status[1]}</li>
    {:else}
        <li>Week: {i + 1}</li>
    {/if}        
  {/each}
</ol>

<style>
    ol{
        list-style: none;
        padding: 0;
        margin: 0;
    }
</style>