<script>
  import { onMount } from "svelte";
  import { students, currentStudent } from "./store";
  import Student from "./lib/Student.svelte";
  import Tally from "./lib/Tally.svelte";
  import SideBarInfo from "./lib/SideBarInfo.svelte";

  const RANDOM_USER_API_URL = "https://randomuser.me/api/?results=";
  const USER_AMOUNT = 10;
  const STATUSES = ["present", "sick", "online", "absent"];

  const WEEKS_TOTAL = 16;
  const CURRENT_WEEK = 8;

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

    let classes;
    let class1;
    let class2;

    // Initialize a students attendance history with random statuses
    for (let i = 0; i < USER_AMOUNT; i++) {
      classes = [];
      for (let j = 0; j < CURRENT_WEEK; j++) {
        if (j < CURRENT_WEEK - 1) {
          class1 = STATUSES[Math.floor(Math.random() * STATUSES.length)];
          class2 = STATUSES[Math.floor(Math.random() * STATUSES.length)];
          classes = [...classes, [class1, class2]];
        } else {
          classes = [...classes, [undefined, undefined]];
        }
      }
      $students[i].history = classes;
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

  const cancelClass = () => {
    for (let i = 0; i < USER_AMOUNT; i++) {
      $students[i].status = "canceled";
    }
  }

  // Clear all student statuses
  const clearAll = () => {
    selectedMarkAll = undefined;
    for (let i = 0; i < USER_AMOUNT; i++) {
      $students[i].status = undefined;
    }
  };
</script>

<header>
  <h1>Programming 2 - Semester 2</h1>
  <p>Week {CURRENT_WEEK} of {WEEKS_TOTAL} | Class 1</p>
  <time>20/04/2000</time>
</header>
<button on:click={() => cancelClass()} id="cancel-btn">Class Cancelled</button>
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
<main>
      <form action="/">
        <table>
          <thead>
            <tr>
              <th colspan="4">Students</th>
            </tr>
            <tr>
              <th>First name</th>
              <th>Last Name</th>
              <th>Status</th>
              <th>Selected status</th>
            </tr>
          </thead>
          <tbody>
            {#each $students as student, idx}
              <Student {student} {idx} {selectedMarkAll} />
            {/each}
          </tbody>
        </table>
        <button>Finish later</button>
        <button>submit attendance</button>
      </form>
      <SideBarInfo />
</main>

<style>
  main {
    display: grid;
    justify-content: space-evenly;
    grid-template-columns: 1fr 1fr;
  }
  th {
    padding: 20px 15px;
    text-align: left;
    font-weight: 500;
    font-size: 12px;
    text-transform: uppercase;
    padding: 12px 15px;
  }
  table {
    border-collapse: collapse;
    margin: 25px 0;
    font-size: 0.9em;
    min-width: 400px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
    width: 100%;
  }
  thead tr:first-child {
    background-color: rgb(0, 82, 255);
    color: #ffffff;
    text-align: left;
  }
  thead tr:nth-child(2) {
    background-color: rgb(219, 215, 215);
    text-align: left;
  }
</style>
