<script>
  import { onMount } from "svelte";
  import Student from "./lib/Student.svelte";

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
  <ul>
    {#each students as student, idx}
      <li>
        <Student student={student} idx={idx} />
      </li>
    {/each}
  </ul>
</main>

<style>
</style>
