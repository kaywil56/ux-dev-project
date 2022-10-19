<script>
  import { onMount } from "svelte";
  import Student from "./lib/Student.svelte";
  import Tally from "./lib/Tally.svelte";

  const RANDOM_USER_API_URL = "https://randomuser.me/api/?results=";
  const USER_AMOUNT = 15;

  let students = [];

  onMount(async () => {
    const res = await fetch(RANDOM_USER_API_URL + USER_AMOUNT);
    students = await res.json();
    students = students.results;
  });
</script>

<main>
  <header>
    <h1>Programming 2 - Semester 2</h1>
    <p>Week 8 | Class 2</p>
    <time>20/04/2000</time>
  </header>
  <Tally />
  <ul>
    {#each students as student, idx}
      <li>
        <Student {student} {idx} />
      </li>
    {/each}
  </ul>
</main>

<style>
</style>
