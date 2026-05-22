---
type: multipleChoice
notes: |
  Upstream JSON field: `partnerTalking`. U-SHAPED VALENCE — neither
  extreme is good; the middle option ("About the right amount") is the
  best outcome.

  The 0–1 numeric encoding follows display order from "Hardly at all"
  (0) through "About the right amount" (0.5) to "Far too much" (1). The
  raw numeric value alone is therefore NOT directly interpretable as
  "good vs bad" — values close to 0.5 mean the partner talked an
  appropriate amount, values near 0 or 1 are both sub-optimal (partner
  talked too little or too much, respectively).

  For a "talked appropriately" composite score, transform via
  `1 - abs(0.5 - value) * 2` (which produces 1 at the midpoint and 0 at
  either extreme). For raw "how much did they talk" usage, the value
  works as-is.
---

### How much did they talk during the discussion?

---

- 0: Hardly at all
- 0.25: Not quite enough
- 0.5: About the right amount
- 0.75: A little too much
- 1: Far too much
