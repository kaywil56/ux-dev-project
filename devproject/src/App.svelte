<script>
  import { onMount } from "svelte";
  import { students, tally } from "./store";
  import Student from "./lib/Student.svelte";
  import Tally from "./lib/Tally.svelte";
  import SideBarInfo from "./lib/SideBarInfo.svelte";

  const RANDOM_USER_API_URL = "https://randomuser.me/api/?results=";
  const USER_AMOUNT = 30;

  let selectedMarkAll;

  onMount(async () => {
    const res = await fetch(RANDOM_USER_API_URL + USER_AMOUNT);
    let studentList = await res.json();
    studentList = studentList.results;
    $students = studentList;
    console.log($students)
  });

  const sortStudents = (sortBy) => {
    console.log("clicked")
    $students = $students.sort((a, b) => a["name"][sortBy].localeCompare(b["name"][sortBy]));
    console.log($students)
  }
</script>

<main>
  <section>
    <header>
      <h1>Programming 2 - Semester 2</h1>
      <p>Week 8 | Class 2</p>
      <time>20/04/2000</time>
    </header>
    <button id="cancel-btn">Class Cancelled</button>
    <button on:click={() => selectedMarkAll = undefined } id="clear-btn">Clear All</button>
    <button on:click={() => sortStudents("first")}>Sort by first name</button>
    <button on:click={() => sortStudents("last")}>Sort by last name</button>
    <select
      bind:value={selectedMarkAll}
      name="mark-all-as"
      id="mark-all-as-select"
    >
      <option>--Mark all as--</option>
      <option value={"present"}>Present</option>
      <option value={"absent"}>Absent</option>
      <option value={"online"}>Online</option>
      <option value={"sick"}>Sick</option>
    </select>
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
        {#each $students as student, idx}
          <Student {student} {idx} {selectedMarkAll} />
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
