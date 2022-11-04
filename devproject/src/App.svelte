<script>
  import { onMount } from "svelte";
  import { students, currentStudent, tally } from "./store";
  // import Student from "./lib/Student.svelte";
  // import Tally from "./lib/Tally.svelte";
  import SideBarInfo from "./lib/SideBarInfo.svelte";
  import Popup from "./lib/Popup.svelte";
  import a11yChecker from "a11y-checker";
  import StudentTable from "./lib/StudentTable.svelte";

  const RANDOM_USER_API_URL = "https://randomuser.me/api/?results=";
  const USER_AMOUNT = 20;
  const STATUSES = ["present", "sick", "online", "absent"];

  const WEEKS_TOTAL = 16;
  const CURRENT_WEEK = 8;

  // let searchValue;
  // let toggleSortFirstName = undefined;
  // let toggleSortLastName = true;
  let showAlert = false;
  let hasSubmitted = false;

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
    a11yChecker();
  });
</script>

{#if !hasSubmitted}
  <header>
    <h1>Programming 2 (ID786)</h1>

    <h2>Semester: 2</h2>
    <p class="time-of-class"><b>Week: </b>{CURRENT_WEEK} of {WEEKS_TOTAL}</p>
    <p class="time-of-class"><b>Class: </b>1</p>
    <time class="time-of-class">20/04/2000</time>
  </header>
  <main>
    <form action="/">
      <StudentTable userAmount={USER_AMOUNT} />
      <!-- <table>
        <thead>
          <tr>
            <th id="t-header" colspan="7">Students (<Tally />)</th>
          </tr>
          <tr>
            <td id="table-options" colspan="7">
              <div id="options">
                <input
                  id="student-search"
                  bind:value={searchValue}
                  type="text"
                  placeholder="Search."
                />
                <button on:click|preventDefault={() => fillDown()}
                  >Fill down</button
                >
                <button
                  on:click|preventDefault={() => cancelClass()}
                  id="cancel-btn">Cancel Class</button
                >
                <button
                  on:click|preventDefault={() => clearAll()}
                  id="clear-btn">Clear All</button
                >
              </div>
            </td>
          </tr>
          <tr>
            <th
              class={toggleSortFirstName === true
                ? "sort-desc"
                : toggleSortFirstName === false
                ? "sort-desc"
                : "sort"}
              on:click={() => (toggleSortLastName = undefined)}
              on:click={() => (toggleSortFirstName = !toggleSortFirstName)}
              on:click|preventDefault={() =>
                toggleSortFirstName
                  ? sortStudents("first", "asc")
                  : sortStudents("first", "desc")}>First name</th
            >
            <th
              class={toggleSortLastName === true
                ? "sort-desc"
                : toggleSortLastName === false
                ? "sort-asc"
                : "sort"}
              on:click={() => (toggleSortFirstName = undefined)}
              on:click={() => (toggleSortLastName = !toggleSortLastName)}
              on:click|preventDefault={() =>
                toggleSortLastName
                  ? sortStudents("last", "asc")
                  : sortStudents("last", "desc")}>Last Name</th
            >
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
      </table> -->
      <div id="finish-grp">
        {#if showAlert}
          <Popup close={() => (showAlert = false)} />
        {/if}
        <button
          id="finish-later"
          on:click|preventDefault={() => (showAlert = true)}
          >Finish later</button
        >
        <button
          on:click|preventDefault={() => (hasSubmitted = true)}
          id="submit"
          disabled={$tally.length != USER_AMOUNT}>Submit attendance</button
        >
      </div>
    </form>
    <SideBarInfo />
  </main>
{:else}
  <em>Submitted Attendance</em>
{/if}

<style>
  #finish-grp {
    align-self: flex-end;
    margin-top: 10px;
  }
  #clear-btn {
    justify-self: flex-end;
  }
  /* .time-of-class {
    display: inline;
  } */
  main {
    display: inline-flex;
  }

  form {
    display: flex;
    flex-direction: column;
  }

  button:not(#finish-later):not(#submit) {
    margin-right: 5px;
    padding: 5px 10px 5px 10px;
    border-radius: 5%;
    color: inherit;
    border: 1px solid #333;
    font: inherit;
    cursor: pointer;
    outline: inherit;
  }

  ::placeholder {
    margin-left: 20px;
  }

  @media only screen and (max-width: 600px) {
    h1 {
      font-size: large;
    }
    h2 {
      font-size: medium;
    }
    p {
      font-size: small;
    }
    time {
      font-size: small;
    }
    form {
      margin-bottom: 40vh;
    }
  }
  @media only screen and (max-width: 400px) {
    #student-search {
      padding-left: 5px;
      background: none;
    }
    th {
      padding: 10px;
    }
    #options {
      padding: 0;
    }
  }
</style>
