# overall_affect module

Three-item composite measuring participants' affective response to a
cross-partisan dialogue partner. Authored for pilot_3 as a candidate
**third primary DV** (alongside understanding-of-partner and
willingness-to-return). Status: **exploratory** — see ADR
[2026-05-11-overall-affect-and-forecast-design.md](../../../../docs/decisions/2026-05-11-overall-affect-and-forecast-design.md).

The module also supports a **forecast variant** asked once
pre-discussion (after the intervention, before the actual conversation).
Originally designed for a within-subjects forecast/realized comparison
across THREE time points, but per ADR
[2026-05-11-drop-t1-reframe-t2-as-confidence.md](../../../../docs/decisions/2026-05-11-drop-t1-reframe-t2-as-confidence.md)
the pre-intervention measurement (T1) was dropped before pilot launch.
The surviving pre-discussion measurement (T2) is now interpreted as a
**between-subjects pre-discussion confidence** measure, not as the
second leg of a within-subjects diff. The interesting analytic
question is the joint pattern across arms: which interventions move
T2 (confidence) without moving T3 (realized affect), and vice versa.
Pilot data may further lead us to drop the forecast version entirely
for the main study.

## Files

```
overall_affect/
├── overall_affect.stagebook.yaml   ← module entry (two templates)
├── quality_realized.prompt.md       (T3, post-discussion)
├── liking_realized.prompt.md
├── connection_realized.prompt.md
├── quality_forecast.prompt.md       (T2, post-intervention pre-discussion)
├── liking_forecast.prompt.md
├── connection_forecast.prompt.md
├── _preview_.stagebook.yaml         standalone tester
└── README.md (this file)
```

## What's in the battery

| # | Storage key suffix | Realized item                                                | Forecast item                                                                |
|---|--------------------|--------------------------------------------------------------|-------------------------------------------------------------------------------|
| 1 | `quality`          | How good was the conversation?                               | How much do you expect to enjoy the discussion?                              |
| 2 | `liking`           | How much did you like your conversation partner?             | How much do you expect to like your conversation partner?                    |
| 3 | `connection`       | How socially connected do you feel to your partner?          | How socially connected do you expect to feel to your partner?                |

All items are 7-point Likert with anchors at **1 (Not very …) / 4
(Somewhat …) / 7 (Very …)** and bare-number positions in between.

### Quality wording asymmetry (intentional)

The realized quality item asks "How **good** was the conversation"
but the forecast version asks "How much do you expect to **enjoy**
the discussion." "Expect the conversation to be good" reads
awkwardly as a forecast, so we use enjoyment-anchored phrasing on
the forecast side. The construct shift is small but worth flagging
at scoring time — they're not perfectly comparable items.

## Two templates, two flavors

| Template | When to use | Tense |
|---|---|---|
| `overall_affect_realized` | Post-discussion (T3) | Past — "how good was…" |
| `overall_affect_forecast` | Pre-discussion, post-intervention (T2) — measures pre-discussion confidence | Future — "how much do you expect…" |

In pilot_3:

- **T2**: `affect_expectation` gameStage, runs after
  `pre_discussion_intervention` and before `discussion`.
- **T3**: `affect_post_discussion` gameStage, runs after
  `describe_partner_views` and before `willingness_to_return_partner_post`.

T1 (a pre-intervention forecast originally planned to run between
`attitude_attributes` and `pre_discussion_intervention`) was dropped
before pilot launch — see the drop-T1 ADR.

Each invocation uses a distinct `prefix` so the storage keys don't
collide:

- `prompt_affect_expectation_quality`
- `prompt_affect_post_discussion_quality`
- (and the same for liking / connection)

## How to use

### 1. Import

```yaml
imports:
  - ./modules/overall_affect/overall_affect.stagebook.yaml
```

### 2. Invoke the realized template post-discussion

```yaml
gameStages:
  - name: affect_post_discussion
    duration: 90
    elements:
      - template: overall_affect_realized
        fields:
          prefix: affect_post_discussion
```

### 3. Invoke the forecast template pre-discussion (post-intervention)

```yaml
gameStages:
  # ... pre-discussion intervention runs here ...

  - name: affect_expectation
    duration: 90
    elements:
      - template: overall_affect_forecast
        fields:
          prefix: affect_expectation
```

The templates emit `contentType: elements` — a flat list of three
prompts plus a submitButton with `exists` gates on every item. The
call site wraps with the stage-level fields it needs (`name`,
`duration`, optional `conditions`, etc.). This shape is intentional:
the same template can be invoked inside a gameStage (where `duration:`
is required) AND inside an exit/intro step (where it isn't). You do
NOT add your own submitButton at the call site — it's already in the
template.

The `name:` of the wrapping stage and the `prefix:` field are
typically the same string so the stage and the storage keys line up
(`affect_post_discussion` → `prompt_affect_post_discussion_quality`).

## Scoring

Composite affect = `mean(quality, liking, connection)` on the 1–7
scale, no inversions.

For the cross-arm analysis (post-T1-drop), the two contrasts of
interest are:

- **Effect on pre-discussion confidence**:
  `affect_expectation[arm] − affect_expectation[control]`
- **Effect on realized affect**:
  `affect_post_discussion[arm] − affect_post_discussion[control]`

The interesting compound is the joint pattern across arms: an
intervention may shift confidence (T2) without shifting realized
affect (T3), or shift realized affect without obviously shifting
confidence. Practitioners often select interventions based on what
participants report liking; this design lets us test whether the
"feels-good" interventions actually deliver on conversation outcomes.

NOTE: do NOT compute `T3 − T2` as a within-subjects forecast-accuracy
diff. T2 and T3 are now treated as different constructs (confidence
vs. realized affect), measured between-subjects. See ADR
[2026-05-11-drop-t1-reframe-t2-as-confidence.md](../../../../docs/decisions/2026-05-11-drop-t1-reframe-t2-as-confidence.md).

## Construct distinction from `perception_of_others/common`

The connection item ("how socially connected do you feel") is
intentionally separate from `perception_of_others/common` ("how much
do you have in common with them"). Two different constructs:

- **Connection** (Aron, Aron & Smollan 1992 / IOS scale tradition) —
  felt closeness / inclusion of other in self. A warmth-of-bond
  measure.
- **Common** (Robinson & Keltner 1995 false-consensus tradition;
  similarity-attraction literature) — perceived overlap of identity,
  views, or experience. A perceived-similarity measure.

You can feel highly connected to someone you have little in common
with (e.g., a deep conversation about a shared values frame across
demographic differences), and you can perceive lots in common with
someone you don't feel close to (e.g., demographic-twin who you
didn't click with). Keeping both items lets us separate these.

## Status: exploratory

This module is **exploratory** for pilot_3. The composite is a
candidate for a third primary DV but not committed to. T1 was
already dropped before pilot launch (per the drop-T1 ADR). Likely
outcomes after pilot data on the surviving T2 + T3 design:

- Both T2 (confidence) and T3 (realized affect) are informative AND
  show distinct cross-arm rankings → keep both for the main study;
  the joint pattern is the headline analytic finding.
- Both informative but rank the same arms → either is fine as the
  primary; consider dropping one for parsimony.
- Only T3 is informative → drop T2; T3 becomes the third primary DV.
- Only T2 is informative → unlikely (confidence without correlated
  realized affect would be a striking finding worth a separate paper).
- The 3-item composite doesn't cohere (low alpha) → treat as three
  separate exploratory secondaries.

The original ADR committed us to running all three time points in
the pilot. The drop-T1 ADR revised that to two (T2 + T3) before
pilot launch. Further drop decisions for the main study happen after
pilot analysis.
