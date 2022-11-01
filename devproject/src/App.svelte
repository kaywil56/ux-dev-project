<script>
  import { onMount } from "svelte";
  import { students, currentStudent, tally } from "./store";
  import Student from "./lib/Student.svelte";
  import Tally from "./lib/Tally.svelte";
  import SideBarInfo from "./lib/SideBarInfo.svelte";

  const RANDOM_USER_API_URL = "https://randomuser.me/api/?results=";
  const USER_AMOUNT = 10;
  const STATUSES = ["present", "sick", "online", "absent"];

  const WEEKS_TOTAL = 16;
  const CURRENT_WEEK = 8;

  let searchValue;

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
  };

  // Clear all student statuses
  const clearAll = () => {
    $tally = [];
    for (let i = 0; i < USER_AMOUNT; i++) {
      $students[i].status = undefined;
    }
  };
</script>

<header>
  <h1>Programming 2 (ID786)</h1>
  <h2>Semester: 2</h2>
  <p class="time-of-class"><b>Week: </b>{CURRENT_WEEK} of {WEEKS_TOTAL}</p>
  <p class="time-of-class"><b>Class: </b>1</p>
  <time class="time-of-class">20/04/2000</time>
</header>
<button on:click={() => cancelClass()} id="cancel-btn">Class Cancelled</button>
<button on:click={() => clearAll()} id="clear-btn">Clear All</button>
<button on:click={() => sortStudents("first")}>Sort by first name</button>
<button on:click={() => sortStudents("last")}>Sort by last name</button>
<button on:click={() => fillDown()}>Fill down</button>
<Tally />
<main>
  <form action="/">
    <input
      id="search-student"
      bind:value={searchValue}
      type="text"
      placeholder="Search for a student."
    />
    <table>
      <thead>
        <tr>
          <th id="t-header" colspan="7">Students</th>
        </tr>
        <tr>
          <th>First name</th>
          <th>Last Name</th>
          <th>Student ID</th>
          <th>Status</th>
          <th>Selected status</th>
        </tr>
      </thead>
      <tbody>
        {#each $students as student, idx}
          {#if searchValue}
            {#if (student.name.first.toUpperCase() + " " + student.name.last.toUpperCase()).includes(searchValue.toUpperCase())}
              <Student {student} {idx} />
            {/if}
          {:else}
            <Student {student} {idx} />
          {/if}
        {/each}
      </tbody>
    </table>
    <button on:click|preventDefault>Finish later</button>
    <button disabled={$tally.length != USER_AMOUNT} type="submit"
      >Submit attendance</button
    >
  </form>
  <SideBarInfo />
</main>

<style>
  #search-student {
    height: 20px;
    padding: 2px 23px 2px 30px;
    align-self: flex-end;
  }
  .time-of-class {
    display: inline;
  }
  main {
    display: flex;
    justify-content: space-around;
    /* display: grid;
    justify-content: space-evenly;
    grid-template-columns: 1fr 1fr; */
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
  form{
    display: flex;
    flex-direction: column;
  }
</style>
