<script>
  // Array of students history
  export let studentHistory;
</script>

<table>
  <thead>
    <th colspan="3"> Attendance History </th>
    <tr>
      <th> Week </th>
      <th> Class 1 </th>
      <th> Class 2 </th>
    </tr>
  </thead>
  <tbody>
    {#each studentHistory as status, i}
      <tr>
        <td><b>{i + 1}</b></td>
        {#if status[0]}
          <td>{status[0]}</td>
        {:else}
          <td>--</td>
        {/if}
        {#if status[1]}
          <td>{status[1]}</td>
        {:else}
          <td>--</td>
        {/if}    
      </tr>
    {/each}
  </tbody>
</table>
<style>
  th {
    /* padding: 20px 15px; */
    text-align: left;
    font-weight: 500;
    font-size: 12px;
    text-transform: uppercase;
    padding: 12px 15px;
  }
  table {
    border-collapse: collapse;
    font-size: 0.9em;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
    margin: 10px;
  }
  thead tr:first-child {
    background-color: rgb(0, 82, 255);
    color: #ffffff;
    text-align: left;
  }
  thead tr:nth-child(2) {
    background-color: rgb(219, 215, 215);
    text-align: left;
  }
  tr:nth-of-type(even) {
    background-color: #f2f2f2;
  }

  @media only screen and (max-width: 600px) {
  
    table, th{
      font-size: 12px;
    }
    th, td {
    padding: 3px;
  }
  }
</style>
