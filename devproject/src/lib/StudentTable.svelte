<script>
  import { onMount } from "svelte";
  import { students, currentStudent, tally } from "../store.js";
  import Student from "../lib/Student.svelte";
  import Tally from "../lib/Tally.svelte";
  import AreYouSureModal from "./AreYouSureModal.svelte";

  export let userAmount;
  // Value for the search bar input
  let searchValue;
  // Value for toggle sort asc / desc (first name)
  let toggleSortFirstName = true;
   // Value for toggle sort asc / desc (last name)
  let toggleSortLastName = true;
  let isClearAll;
  let isCancelClass;

  // Sort table ascending by given param
  const sortStudents = (sortBy, order) => {
    // Sort asc
    if (order == "asc") {
      $students = $students.sort((a, b) =>
        a["name"][sortBy].localeCompare(b["name"][sortBy])
      );
    // Sort desc  
    } else {
      $students = $students.sort((a, b) =>
        b["name"][sortBy].localeCompare(a["name"][sortBy])
      );
    }
  };

  // Fill down values from the select student idx
  const fillDown = () => {
    for (let i = $currentStudent; i < userAmount; i++) {
      if ($students[i].status == undefined) {
        $students[i].status = $students[$currentStudent].status;
      }
    }
  };

  // Apply cancel status to each student
  const cancelClass = () => {
    for (let i = 0; i < userAmount; i++) {
      $students[i].status = "canceled";
    }
  };

  // Clear all student statuses
  const clearAll = () => {
    $tally = [];
    for (let i = 0; i < userAmount; i++) {
      $students[i].status = undefined;
    }
  };

  // Sort students by last name asc by default
  onMount(async () => {
    sortStudents("last", "asc");
  });
</script>

<table>
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
          <button id="fill-down" on:click|preventDefault={() => fillDown()}>Fill down</button>
          <button on:click|preventDefault={() => isCancelClass = true} id="cancel-btn"
            >Cancel Class</button
          >
          {#if isCancelClass}
          <AreYouSureModal
            message={"cancel class"}
            close={() => (isCancelClass = false)}
            method={cancelClass}
          />
        {/if}
          <button on:click|preventDefault={() => isClearAll = true} id="clear-btn"
            >Clear All</button
          >
          {#if isClearAll}
            <AreYouSureModal message={"clear the attendance"} close={() => isClearAll = false} method={clearAll} />
          {/if}
        </div>
      </td>
    </tr>
    <tr>
      <th
        class="sort"
        on:click={() => (toggleSortFirstName = !toggleSortFirstName)}
        on:click|preventDefault={() =>
          toggleSortFirstName
            ? sortStudents("first", "asc")
            : sortStudents("first", "desc")}>First name</th
      >
      <th
        class="sort"
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
</table>

<style>
  th {
    text-align: left;
    font-weight: 500;
    font-size: 12px;
    text-transform: uppercase;
    padding: 12px 15px;
    cursor: pointer;
    font-weight: bold;
    border-bottom: 1px solid #333;
  }
  table {
    border-collapse: collapse;
    font-size: 0.9em;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
    width: 100%;
  }
  tbody {
    height: 100px;
    overflow-y: auto;
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
  #student-search {
    margin-right: 5px;
    padding: 7px 10px 7px 40px;
    flex-grow: 1;
    background: url("../assets/search-svgrepo-com.svg");
    background-size: 15px;
    background-position: 10px center;
    background-repeat: no-repeat;
  }
  #options {
    display: flex;
    padding: 5px;
  }
  .sort {
    background: url("../assets/sort-svgrepo-com.svg");
    background-size: 15px;
    background-position: right center;
    background-repeat: no-repeat;
  }
  @media only screen and (max-width: 600px) {
    .sort {
      background-size: 17px;
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
