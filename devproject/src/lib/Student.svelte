<script>
  import { currentStudent, tally } from "../store.js";

  export let student;
  export let idx;
  export let selectedMarkAll

  $: if (student.status) {
    $tally[idx] = student.status;
  }
</script>

<tr on:click={() => ($currentStudent = idx)}>
  <td>
    {student.name.first}
  </td>
  <td>
    {student.name.last}
  </td>
  <td>
    {student.login.uuid.slice(0, 6)}
  </td>
  <td>
    <input
      type="radio"
      id="present-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      bind:group={student.status}
      value="present"
      checked={selectedMarkAll == "present"}
    />
    <label for="present-radio-button-{idx}">Present</label>
    <input
      type="radio"
      id="absent-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      bind:group={student.status}
      value={"absent"}
      checked={selectedMarkAll == "absent"}
    />
    <label for="absent-radio-button-{idx}">Absent</label>
    <select
      bind:value={student.status}
      name="more-options"
      id="more-options-select"
    >
      <option value={undefined} selected>--More options--</option>
      <option selected={selectedMarkAll == "online"} value={"online"}
        >Online</option
      >
      <option selected={selectedMarkAll == "sick"} value={"sick"}>Sick</option>
      <option selected={selectedMarkAll == "explained"} value={"explained"}
        >Explained</option
      >
    </select>
  </td>
  <td>
    {#if student.status}
    {student.status}
    {:else}
    None selected
    {/if}
  </td>
  <td><button>View more</button></td>
  </tr
>

<style>
  td {
    padding: 12px 15px;
  }
  tr:nth-of-type(even) {
    background-color: #f2f2f2;
  }
  tr:hover{
    background-color: rgb(206, 206, 206);
  }
</style>
