<script>
  import { tally, currentStudent } from "../store.js";

  export let student;
  export let idx;
  export let selectedMarkAll;

  $: console.log(selectedMarkAll)
</script>

<tr on:click={() => ($currentStudent = idx)}>
  <td>
    {student.name.first}
  </td>
  <td>
    {student.name.last}
  </td>
  <td>
    <input
      type="radio"
      id="present-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      on:click={() => ($tally[idx] = "positive")}
      checked={selectedMarkAll == "present"}
    />
    <label for="present-radio-button-{idx}">Present</label>
    <input
      type="radio"
      id="absent-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      on:click={() => ($tally[idx] = "negative")}
      checked={selectedMarkAll == "absent"}
    />
    <label for="absent-radio-button-{idx}">Absent</label>
    <input
      type="radio"
      id="present-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      on:click={() => ($tally[idx] = "positive")}
      checked={selectedMarkAll == "online"}
    />
    <label for="present-radio-button-{idx}">Online</label>
    <input
      type="radio"
      id="absent-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      on:click={() => ($tally[idx] = "neutral")}
      checked={selectedMarkAll == "sick"}
    />
    <label for="absent-radio-button-{idx}">Sick</label>
  </td>
</tr>
