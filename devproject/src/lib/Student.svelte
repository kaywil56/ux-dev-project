<script>
  import { tally, currentStudent } from "../store.js";

  export let student;
  export let idx;
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
    />
    <label for="present-radio-button-{idx}">Present</label>
    <input
      type="radio"
      id="absent-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      on:click={() => ($tally[idx] = "negative")}
    />
    <label for="absent-radio-button-{idx}">Absent</label>
    <input
      type="radio"
      id="present-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      on:click={() => ($tally[idx] = "positive")}
    />
    <label for="present-radio-button-{idx}">Online</label>
    <input
      type="radio"
      id="absent-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      on:click={() => ($tally[idx] = "neutral")}
    />
    <label for="absent-radio-button-{idx}">Sick</label>
  </td>
</tr>
