<script>
  import { students } from "../store.js";
  let positiveCount;
  let negativeCount;
  let neutralCount;

  $: positiveCount = $students.filter((student) => student.status == "present" || student.status == "online" || student.status == "canceled").length;
  $: negativeCount = $students.filter((student) => student.status == "absent").length;
  $: neutralCount = $students.filter((student) => student.status == "sick" || student.status == "explained").length;
</script>

<p>
  Postive: {positiveCount} | Negative: {negativeCount} | Neutral: {neutralCount}
</p>

<style>
  p{
    display: inline;
  }
</style>
