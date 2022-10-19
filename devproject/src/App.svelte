<script>
  import { onMount } from "svelte";
  import Student from "./lib/Student.svelte";
  import Tally from "./lib/Tally.svelte";
  import SideBarInfo from "./lib/SideBarInfo.svelte";

  const RANDOM_USER_API_URL = "https://randomuser.me/api/?results=";
  const USER_AMOUNT = 30;

  let students = [];

  onMount(async () => {
    const res = await fetch(RANDOM_USER_API_URL + USER_AMOUNT);
    students = await res.json();
    students = students.results;
  });
</script>

<main>
  <section>
    <header>
      <h1>Programming 2 - Semester 2</h1>
      <p>Week 8 | Class 2</p>
      <time>20/04/2000</time>
    </header>
    <Tally />
    <table>
      <thead>
        <tr>
          <th colspan="2">Students</th>
        </tr>
        <tr>
          <th>First name</th>
          <th>Last Name</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        {#each students as student, idx}
          <tr>
            <Student {student} {idx} />
          </tr>
        {/each}
      </tbody>
    </table>
  </section>
  <SideBarInfo />
</main>

<style>
  main {
    display: grid;
    grid-template-columns: 3fr 1fr;
  }
</style>
