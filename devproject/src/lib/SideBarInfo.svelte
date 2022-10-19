<script>
  import { students, currentStudent } from "../store";

  $: thisPerson = $students[$currentStudent]; //Reactive to change in person coming from sidebar via store
</script>

<section>
  <h2>Profile</h2>
  {#if thisPerson}
    <p>{thisPerson.name.first + " " + thisPerson.name.last}</p>
    <p>Student ID: {thisPerson.login.uuid.slice(0, 6)}</p>
    <p>Username: {thisPerson.login.username}</p>
    <address>Email: {thisPerson.email}</address>
    <p>Phone: {thisPerson.phone}</p>
    <p>Cell: {thisPerson.cell}</p>
    <p>DOB: {thisPerson.dob.date} ({thisPerson.dob.age})</p>
    <address>Street: {thisPerson.location.street.number} {thisPerson.location.street.name}</address>
    <address>State: {thisPerson.location.state}</address>
    <address>Postcode: {thisPerson.location.postcode}</address>
  {:else}
    <h2>No student selected</h2>
  {/if}
</section>

<style>
  section {
    height: 100vh;
    border: 1px solid #333;
    position: sticky;
    top: 0;
  }
</style>
