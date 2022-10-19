<script>
  import { tally, currentStudent } from "../store.js";

  export let student;
  export let idx;
  export let selectedMarkAll;

  let statusInput;

  $: console.log(statusInput)

  $: if (statusInput) {
    $tally[idx] = statusInput;
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
    <input
      type="radio"
      id="present-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      bind:group={statusInput}
      value="present"
      checked={selectedMarkAll == "present"}
    />
    <label for="present-radio-button-{idx}">Present</label>
    <input
      type="radio"
      id="absent-radio-button-{idx}"
      name="status-radio-grp-{idx}"
      bind:group={statusInput}
      value={"absent"}
      checked={selectedMarkAll == "absent"}
    />
    <label for="absent-radio-button-{idx}">Absent</label>
    <select
      bind:value={statusInput}
      name="more-options"
      id="more-options-select"
    >
      <option selected>--More options--</option>
      <option selected={selectedMarkAll == 'online'} value={"online"}>Online</option>
      <option selected={selectedMarkAll == 'sick'} value={"sick"}>Sick</option>
      <option selected={selectedMarkAll == 'explained'} value={"explained"}>Explained</option>
    </select>
    <!-- <input
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
  </td> -->
  </td></tr
>
