<script>
  import { tally } from "../store.js";
  let positiveCount;
  let negativeCount;

  // Whenever the tally changes, re-count the amount of positives and negatives
  // in the tally store
  $: positiveCount = $tally.filter((x) => x == "positive").length;
  $: negativeCount = $tally.filter((x) => x == "negative").length;
</script>

<p>Postive: {positiveCount} | Negative: {negativeCount}</p>
