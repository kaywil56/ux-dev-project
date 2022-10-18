<script>
  import { tally } from "../store.js";

  export let student;
  export let idx;
</script>

{student.name.first + " " + student.name.last}
<fieldset>
  <legend>Status</legend>
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
</fieldset>
