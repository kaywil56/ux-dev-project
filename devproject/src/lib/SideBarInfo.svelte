<script>
  import { students, currentStudent } from "../store";
  import AttendanceHistory from "./AttendanceHistory.svelte";

  $: thisPerson = $students[$currentStudent]; //Reactive to change in person coming from sidebar via store
</script>

<section>
  <div id="sidebar">    
  {#if thisPerson}
    <div>
      <img src={thisPerson.picture.large} alt="profile">
      <p>{thisPerson.name.first + " " + thisPerson.name.last}</p>
      <AttendanceHistory studentHistory={thisPerson.history} />
      <p>Student ID: {thisPerson.login.uuid.slice(0, 6)}</p>
      <p>Username: {thisPerson.login.username}</p>
      <address>Email: {thisPerson.email}</address>
      <p>Phone: {thisPerson.phone}</p>
      <p>Cell: {thisPerson.cell}</p>
      <p>DOB: {thisPerson.dob.date} ({thisPerson.dob.age})</p>
      <address>
        Street: {thisPerson.location.street.number}
        {thisPerson.location.street.name}
      </address>
      <address>State: {thisPerson.location.state}</address>
      <address>Postcode: {thisPerson.location.postcode}</address>
    </div>
  {:else}
    <h2>No student selected</h2>
  {/if}
</div>
</section>

<style>
  /* section {
    height: 100vh;
    position: sticky;
    top: 0;
  } */

  #sidebar{
    display: flex;
  }

</style>
