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
  let toggleSortFirstName = true;
  let toggleSortLastName;

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

    sortStudents("last", "asc");
  });

  // Sort table ascending by given param
  const sortStudents = (sortBy, order) => {
    if (order == "asc") {
      $students = $students.sort((a, b) =>
        a["name"][sortBy].localeCompare(b["name"][sortBy])
      );
    } else {
      $students = $students.sort((a, b) =>
        b["name"][sortBy].localeCompare(a["name"][sortBy])
      );
    }
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
<Tally />
<main>
  <form action="/">
    <table>
      <thead>
        <tr>
          <th id="t-header" colspan="7">Students </th>
        </tr>
        <tr>
          <td id="table-options" colspan="7">
            <div id="options">
                <input
                  id="student-search"
                  bind:value={searchValue}
                  type="text"
                  placeholder="Search for a student."
                />
                <button on:click|preventDefault={() => fillDown()}>Fill down</button
                  >
                <button
                on:click|preventDefault={() => cancelClass()}
                id="cancel-btn">Cancel Class</button
              >
                <button on:click={() => clearAll()} id="clear-btn">Clear All</button
                >
            </div>
          </td>
        </tr>
        <tr>
          <th
            on:click={() => (toggleSortFirstName = !toggleSortFirstName)}
            on:click|preventDefault={() =>
              toggleSortFirstName
                ? sortStudents("first", "asc")
                : sortStudents("first", "desc")}>First name</th
          >
          <th
            on:click={() => (toggleSortLastName = !toggleSortLastName)}
            on:click|preventDefault={() =>
              toggleSortLastName
                ? sortStudents("last", "asc")
                : sortStudents("last", "desc")}>Last Name</th
          >
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
    <div id="finish-grp">
      <button on:click|preventDefault>Finish later</button>
      <button disabled={$tally.length != USER_AMOUNT} type="submit"
        >Submit attendance</button
      >
    </div>
  </form>
  <SideBarInfo />
</main>

<style>
  #finish-grp {
    align-self: flex-end;
    margin-top: 10px;
  }
  #clear-btn{
    justify-self: flex-end;
  }
  .time-of-class {
    display: inline;
  }
  main {
    /* display: flex; */
    /* justify-content: space-around; */
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
    cursor: pointer;
    font-weight: bold;
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
  thead tr:nth-child(3) {
    background-color: rgb(219, 215, 215);
    text-align: left;
  }
  form {
    display: flex;
    flex-direction: column;
  }
  #student-search{
    margin-right: 5px;
    padding: 7px 10px 7px 10px;
    flex-grow: 1;
  }
  button {
    margin-right: 5px;
    padding: 5px 10px 5px 10px;
    border-radius: 5%;
    color: inherit;
    border: 1px solid #333;
    font: inherit;
    cursor: pointer;
    outline: inherit;
  }
  #options{
    display: flex;
    padding: 5px;
  }
</style>
