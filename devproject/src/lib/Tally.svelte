<script>
  import { students } from "../store.js";
  let positiveCount;
  let negativeCount;
  let neutralCount;

  // Counts the occurrences of statuses in array of students
  $: positiveCount = $students.filter((student) => student.status == "present" || student.status == "online" || student.status == "canceled").length;
  $: negativeCount = $students.filter((student) => student.status == "absent").length;
  $: neutralCount = $students.filter((student) => student.status == "sick" || student.status == "explained").length;
</script>

<p>
  Postive: {positiveCount} | Neutral: {neutralCount} | Negative: {negativeCount} 
</p>

<style>
  p{
    display: inline;
  }
</style>
