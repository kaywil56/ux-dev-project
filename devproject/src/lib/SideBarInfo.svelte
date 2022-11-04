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
        <h2>{thisPerson.name.first + " " + thisPerson.name.last} ({thisPerson.login.uuid.slice(0, 6)})</h2>
        <p><b>Username:</b>{thisPerson.login.username}</p>
        <address><b>Email: </b> {thisPerson.email}</address>
        <p><b>Phone: </b> {thisPerson.phone}</p>
        <p><b>Cell: </b> {thisPerson.cell}</p>
        <p><b>DOB: </b>{new Date(thisPerson.dob.date).toDateString()}</p>
      </div>
      <AttendanceHistory studentHistory={thisPerson.history} />
    {:else}
      <h2>No student selected</h2>
    {/if}
  </div>
</section>

<style>
  #sidebar {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    padding: 10px;
    position: sticky;
    top: 0;
  }

  @media only screen and (max-width: 600px) {
    h2{
      font-size: small;
    }
    section {
      position: fixed;
      bottom: 0;
      width: 100%;
      z-index: 100;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    #sidebar {
      width: 100%;
      display: flex;
      flex-direction: row;
      background-color: gray;
      flex-wrap: nowrap;
      height: 35vh;
    }

    p,address{
      font-size: 13px;
    }

    img{
      height: 70px;
    }
  }
</style>
