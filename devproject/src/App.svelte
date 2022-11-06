<script>
  import { onMount } from "svelte";
  import { students, tally } from "./store";
  import SideBarInfo from "./lib/SideBarInfo.svelte";
  import Popup from "./lib/Popup.svelte";
  import a11yChecker from "a11y-checker";
  import StudentTable from "./lib/StudentTable.svelte";
  import AreYouSureModal from "./lib/AreYouSureModal.svelte";

  const RANDOM_USER_API_URL = "https://randomuser.me/api/?results=";
  const USER_AMOUNT = 20;
  const STATUSES = ["present", "sick", "online", "absent"];

  const WEEKS_TOTAL = 16;
  const CURRENT_WEEK = 8;

  let showAlert = false;
  let hasSubmitted = false;
  let isSubmit;
  let isMarked;

  // Initialize all
  onMount(async () => {
    // Fetch users from random user api
    const res = await fetch(RANDOM_USER_API_URL + USER_AMOUNT);
    let studentList = await res.json();
    studentList = studentList.results;
    $students = studentList;
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

  // const allMarked = () => {
  //   $students.forEach((student) => {
  //     if(student.stat)
  //   })
  // }
</script>

{#if !hasSubmitted}
  <header>
    <h1>Programming 2 (ID786) - Stream B</h1>
    <h2>Semester: 2</h2>
    <p class="time-of-class"><b>Week: </b>{CURRENT_WEEK} of {WEEKS_TOTAL}</p>
    <p class="time-of-class"><b>Class: </b>1</p>
    <time class="time-of-class">20/04/2000</time>
  </header>
  <main>
    <form action="/">
      <StudentTable userAmount={USER_AMOUNT} />
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
          on:click|preventDefault={() => (isSubmit = true)}
          id="submit"
          disabled={$students.filter((student) => student.status).length != USER_AMOUNT}>Submit attendance</button
        >
        {#if isSubmit}
          <AreYouSureModal
            message={"submit the attendance"}
            close={() => (isSubmit = false)}
            method={() => (hasSubmitted = true)}
          />
        {/if}
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
</style>
