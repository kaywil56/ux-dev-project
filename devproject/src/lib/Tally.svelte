<script>
  import { tally } from "../store.js";
  let positiveCount;
  let negativeCount;
  let neutralCount;

  // Whenever the tally changes, re-count the amount of positives, negatives and neutrals
  // in the tally store
  $: positiveCount = $tally.filter((x) => x == "positive").length;
  $: negativeCount = $tally.filter((x) => x == "negative").length;
  $: neutralCount = $tally.filter((x) => x == "neutral").length;
</script>

<p>Postive: {positiveCount} | Negative: {negativeCount} | Neutral: {neutralCount}</p>
