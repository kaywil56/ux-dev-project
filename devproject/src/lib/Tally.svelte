<script>
  import { tally } from "../store.js";
  let positiveCount;
  let negativeCount;
  let neutralCount;

  // Whenever the tally changes, re-count the amount of positives, negatives and neutrals
  // in the tally store

  $: positiveCount = $tally.filter((status) => status == "present" || status == "online" || status == "canceled").length;
  $: negativeCount = $tally.filter((status) => status == "absent").length;
  $: neutralCount = $tally.filter((status) => status == "sick" || status == "explained").length;
</script>

<p>
  Tally: Postive: {positiveCount} | Negative: {negativeCount} | Neutral: {neutralCount}
</p>
