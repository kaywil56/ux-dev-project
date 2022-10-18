<script>
  import { onMount } from "svelte";

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
    {#each students as student}
      <li>
        {student.name.first + " " + student.name.last}
      </li>
    {/each}
  </ul>
</main>

<style>
</style>
