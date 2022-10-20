<script>
  import { onMount } from "svelte";
  import { students, currentStudent } from "./store";
  import Student from "./lib/Student.svelte";
  import Tally from "./lib/Tally.svelte";
  import SideBarInfo from "./lib/SideBarInfo.svelte";

  const RANDOM_USER_API_URL = "https://randomuser.me/api/?results=";
  const USER_AMOUNT = 10;
  const STATUSES = ["present", "sick", "online", "absent"];

  const WEEK_AMOUNT = 16;
  const CLASSES_PER_WEEK = 2;
  const CLASSSES_TOTAL = WEEK_AMOUNT * CLASSES_PER_WEEK;
  const CURRENT_CLASS = 16;

  let selectedMarkAll;

  onMount(async () => {
    const res = await fetch(RANDOM_USER_API_URL + USER_AMOUNT);
    let studentList = await res.json();
    studentList = studentList.results;
    $students = studentList;
    // Sort students by first name ascending on load
    $students = $students.sort((a, b) =>
      a["name"]["first"].localeCompare(b["name"]["first"])
    );
    // Init a status field for each student
    $students.forEach((student) => {
      student["status"] = undefined;
    });

    let currentHistory = []

    // Initialize a students attendance history with random statuses
    for (let i = 0; i < USER_AMOUNT; i++) {
      for (let j = 0; j < CLASSSES_TOTAL; j++) {
        if(j < CURRENT_CLASS){
          currentHistory = [...currentHistory, STATUSES[Math.floor(Math.random() * STATUSES.length)]]
        }
        else{
          currentHistory = [...currentHistory, undefined]
        }
      }
      $students[i].history = currentHistory;
    }
  });

  // Sort table ascending by given param
  const sortStudents = (sortBy) => {
    $students = $students.sort((a, b) =>
      a["name"][sortBy].localeCompare(b["name"][sortBy])
    );
  };

  // Fill down values from the select student idx
  const fillDown = () => {
    for (let i = $currentStudent; i < USER_AMOUNT; i++) {
      if ($students[i].status == undefined) {
        $students[i].status = $students[$currentStudent].status;
      }
    }
  };

  // Clear all student statuses
  const clearAll = () => {
    selectedMarkAll = undefined;
    for (let i = 0; i < USER_AMOUNT; i++) {
      $students[i].status = undefined;
    }
  };
</script>

<main>
  <section>
    <header>
      <h1>Programming 2 - Semester 2</h1>
      <p>Week {CURRENT_CLASS / 2} | Class 2</p>
      <time>20/04/2000</time>
    </header>
    <button id="cancel-btn">Class Cancelled</button>
    <button on:click={() => clearAll()} id="clear-btn">Clear All</button>
    <button on:click={() => sortStudents("first")}>Sort by first name</button>
    <button on:click={() => sortStudents("last")}>Sort by last name</button>
    <button on:click={() => fillDown()}>Fill down</button>
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
