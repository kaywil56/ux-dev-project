<script>
  import { currentStudent, tally } from "../store.js";
  export let student;
  export let idx;

  $: if (student.status) {
    $tally[idx] = student.status;
  }
</script>

<tr
  id={$currentStudent == idx ? "selected" : undefined}
  on:click={() => ($currentStudent = idx)}
>
  <td>
    {student.name.first}
  </td>
  <td>
    {student.name.last}
  </td>
  <td class="status-td">
    <label for="present-radio-button-{idx}"
      >Present <input
        type="radio"
        id="present-radio-button-{idx}"
        name="status-radio-grp-{idx}"
        bind:group={student.status}
        value="present"
      /></label
    >
    <label for="absent-radio-button-{idx}"
      >Absent <input
        type="radio"
        id="absent-radio-button-{idx}"
        name="status-radio-grp-{idx}"
        bind:group={student.status}
        value={"absent"}
      /></label
    >
    <select
      bind:value={student.status}
      name="more-options"
      id="more-options-select"
    >
      <option value={undefined} selected>More</option>
      <option value={"online"}>Online</option>
      <option value={"sick"}>Sick</option>
      <option value={"explained"}>Explained</option>
    </select>
  </td>
  <td>
    {#if student.status}
      {student.status}
    {:else}
      None
    {/if}
  </td>
</tr>

<style>
  .status-td {
    display: flex;
    flex-wrap: wrap;
    /* flex-direction: column; */
  }
  td {
    /* padding: 12px 15px; */
  }
  tr:nth-of-type(even) {
    background-color: #f2f2f2;
  }
  tr:hover {
    background-color: rgb(206, 206, 206);
  }

  #selected {
    background-color: rgb(161, 161, 161);
  }
</style>
