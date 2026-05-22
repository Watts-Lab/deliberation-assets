---
type: multipleChoice
notes: |
  Upstream JSON field: `discussionTension`. Captures the affective
  atmosphere of the discussion.

  ENCODING DIFFERS FROM UPSTREAM. Our local 0-1 encoding follows display
  order — "Very tense" = 0, "Completely relaxed" = 1, so higher values
  mean a more relaxed atmosphere. The upstream `discussionGeneral.json`
  uses the opposite convention ("Very tense" = 1.0, "Completely relaxed"
  = 0.0). Analysts applying the upstream `discussionGeneral.score.js`
  directly need to invert this item via `1 - value` to match upstream;
  analysts using this module's stored values should treat higher as
  more positive (consistent with the rest of the battery).
---
### How would you describe the emotional atmosphere of the discussion?

---
- 0: Very tense and uncomfortable
- 0.25: Moderately tense
- 0.5: Somewhat relaxed, with multiple moments of tension
- 0.75: Mostly relaxed, with rare moments of tension
- 1: Completely relaxed and calm
