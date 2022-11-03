<script>
  import { students, currentStudent } from "../store";
  import AttendanceHistory from "./AttendanceHistory.svelte";

  $: thisPerson = $students[$currentStudent]; //Reactive to change in person coming from sidebar via store
</script>

<section>
  <div id="sidebar">
    {#if thisPerson}
      <div>
        <img src={thisPerson.picture.large} alt="profile" />
        <h2>{thisPerson.name.first + " " + thisPerson.name.last}</h2>
        <AttendanceHistory studentHistory={thisPerson.history} />
        <p><b>Student ID: </b>{thisPerson.login.uuid.slice(0, 6)}</p>
        <p><b>Username:</b>{thisPerson.login.username}</p>
        <address><b>Email: </b> {thisPerson.email}</address>
        <p><b>Phone: </b> {thisPerson.phone}</p>
        <p><b>Cell: </b> {thisPerson.cell}</p>
        <p><b>DOB: </b>{new Date(thisPerson.dob.date).toDateString()}</p>
        <address>
          <b>Street: </b>
          {thisPerson.location.street.number}
          {thisPerson.location.street.name}
        </address>
        <address><b>State: </b> {thisPerson.location.state}</address>
        <address><b>Postcode: </b> {thisPerson.location.postcode}</address>
      </div>
    {:else}
      <h2>No student selected</h2>
    {/if}
  </div>
</section>

<style>
  /* section {
    position: absolute;
    bottom: 0;
    z-index: 100;
  } */

  #sidebar {
    display: flex;
  }
</style>
