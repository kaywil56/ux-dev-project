<script>
  import { currentStudent, students } from "../store.js";
  export let student;
  export let idx;
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
        bind:group={$students[idx].status}
        value="present"
      /></label
    >
    <label for="absent-radio-button-{idx}"
      >Absent <input
        type="radio"
        id="absent-radio-button-{idx}"
        name="status-radio-grp-{idx}"
        bind:group={$students[idx].status}
        value={"absent"}
      /></label
    >
    <select
      bind:value={$students[idx].status}
      name="more-options"
      id="more-options-select"
    >
      <option value={undefined}>More</option>
      <option value={"online"}>Online</option>
      <option value={"sick"}>Sick</option>
      <option value={"explained"}>Explained</option>
    </select>
  </td>
  <td>
    {#if student.status}
     <b>{student.status}</b>
    {:else}
      None
    {/if}
  </td>
</tr>

<style>
  .status-td {
    display: flex;
    flex-wrap: wrap;
  }
  td {
    padding: 12px 15px;
  }
  tr:nth-of-type(even) {
    background-color: #f2f2f2;
  }
  tr:hover {
    background-color: rgb(206, 206, 206);
    border-bottom: 1px solid #333;
  }

  #selected {
    background-color: #333;
    color: white;
    border: 1px solid white;
  }

  @media only screen and (max-width: 600px) {
    td {
      padding: 6px 10px;
    }
  }
</style>
