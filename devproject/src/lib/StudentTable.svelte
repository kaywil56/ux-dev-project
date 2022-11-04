<script>
  import { onMount } from "svelte";
  import { students, currentStudent, tally } from "../store.js";
  import Student from "../lib/Student.svelte";
  import Tally from "../lib/Tally.svelte";

  export let userAmount;

  let searchValue;
  let toggleSortFirstName = undefined;
  let toggleSortLastName = true;

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
    for (let i = $currentStudent; i < userAmount; i++) {
      if ($students[i].status == undefined) {
        $students[i].status = $students[$currentStudent].status;
      }
    }
  };

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
          <button on:click|preventDefault={() => fillDown()}>Fill down</button>
          <button on:click|preventDefault={() => cancelClass()} id="cancel-btn"
            >Cancel Class</button
          >
          <button on:click|preventDefault={() => clearAll()} id="clear-btn"
            >Clear All</button
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
</table>

<style>
  th {
    /* padding: 20px 15px; */
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
    background: url("./public/assets/search-svgrepo-com.svg");
    background-size: 15px;
    background-position: 10px center;
    background-repeat: no-repeat;
  }
  #options {
    display: flex;
    padding: 5px;
  }
  .sort {
    background: url("../public/assets/sort-svgrepo-com.svg");
    background-size: 20px;
    background-position: right center;
    background-repeat: no-repeat;
  }
  .sort-asc {
    background: url("../public/assets/sort-up-solid-svgrepo-com.svg");
    background-size: 20px;
    background-position: right center;
    background-repeat: no-repeat;
  }
  .sort-desc {
    background: url("../public/assets/sort-down-solid-svgrepo-com.svg");
    background-size: 20px;
    background-position: right center;
    background-repeat: no-repeat;
  }
  @media only screen and (max-width: 600px) {
    .sort {
      background-size: 17px;
    }
    .sort-asc {
      background-size: 17px;
    }
    .sort-desc {
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
