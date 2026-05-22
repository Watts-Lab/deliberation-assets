# TIPI module

Ten-Item Personality Inventory (Gosling, Rentfrow, & Swann, 2003) — a
brief Big-5 personality measure. Two items per dimension, one
positively keyed and one negatively keyed, on a 7-point
Strongly-Disagree → Strongly-Agree scale. Adapted from the
[Watts-Lab `TIPI` survey](https://github.com/Watts-Lab/surveys/tree/main/surveys/TIPI)
— the Watts-Lab README is preserved verbatim below.

## Files

```
TIPI/
├── TIPI.stagebook.yaml         ← module entry point (one template)
├── extroversion.prompt.md       (E+)
├── criticalness.prompt.md       (A- · R)
├── dependability.prompt.md      (C+)
├── anxiety.prompt.md            (ES- · R)
├── openness.prompt.md           (O+)
├── quietness.prompt.md          (E- · R)
├── sympathy.prompt.md           (A+)
├── carelessness.prompt.md       (C- · R)
├── calmness.prompt.md           (ES+)
├── conventionality.prompt.md    (O- · R)
└── README.md (this file)
```

## What's in the battery

| # | Storage key suffix | "I see myself as:" | Big-5 dimension | Keyed | Reverse-coded |
|---|---|---|---|---|---|
| 1 | `extroversion`    | Extraverted, enthusiastic.       | Extraversion          | + |   |
| 2 | `criticalness`    | Critical, quarrelsome.           | Agreeableness         | − | **R** |
| 3 | `dependability`   | Dependable, self-disciplined.    | Conscientiousness     | + |   |
| 4 | `anxiety`         | Anxious, easily upset.           | Emotional Stability   | − | **R** |
| 5 | `openness`        | Open to new experiences, complex.| Openness              | + |   |
| 6 | `quietness`       | Reserved, quiet.                 | Extraversion          | − | **R** |
| 7 | `sympathy`        | Sympathetic, warm.               | Agreeableness         | + |   |
| 8 | `carelessness`    | Disorganized, careless.          | Conscientiousness     | − | **R** |
| 9 | `calmness`        | Calm, emotionally stable.        | Emotional Stability   | + |   |
| 10| `conventionality` | Conventional, uncreative.        | Openness              | − | **R** |

Display order matches the published TIPI (Gosling et al. 2003).

## How to use

### 1. Import the module from your treatment file

```yaml
imports:
  - ./modules/TIPI/TIPI.stagebook.yaml
```

### 2. Invoke `TIPI_questions` as an introStep / exitStep with a `stageName:` field

```yaml
introSequences:
  - name: default
    introSteps:
      - template: TIPI_questions
        fields:
          stageName: big_5_personality
```

The module's template emits `contentType: introExitStep` — a
self-contained step that includes all ten prompts AND a submitButton
with `exists`-conditions gating advancement on every item. You do
NOT add your own submitButton at the call site. Same shape as the
demographics, political_party_us, and receptiveness4 modules.

The same template works as an `exitStep` in `exitSequence` — `introExitStep`
is the shared content type for both pre- and post-game steps.

### 3. Storage keys

With `stageName: big_5_personality`, the module produces:

- `prompt_big_5_personality_extroversion`
- `prompt_big_5_personality_criticalness`
- … (and 8 more — see table)

## Two render variants: slider OR Likert radio buttons

The module exports two parallel templates that share the same ten
items, same encoding, and same scoring conventions — they differ only
in the response widget:

| Template | Widget | Prompt files | When to use |
|---|---|---|---|
| `TIPI_questions` | Slider with three labeled anchors (1/4/7) | `<item>.prompt.md` | Compact UX for batteries where slider's continuous-feel reads well. |
| `TIPI_questions_likert` | Vertical radio row with all 7 options labeled | `<item>_likert.prompt.md` | Default for pilot_3 — the slider renderer has known UX issues (stagebook#326 thumb alignment) and the explicit Likert is clearer to participants. |

Encoding (`raw 1–7`), scoring (`8 − X` for reverse-coded items, then
average within each dimension), and per-item dimension assignments are
identical across both variants. Switching templates at a call site
swaps only the widget, not the data.

### Slider rendering notes (slider variant)

Each slider item is `min: 1, max: 7, interval: 1` so it snaps to
integer values. The prompt body lists ONLY the three anchor points
(1: Strongly Disagree, 4: Neutral, 7: Strongly Agree) — the in-between
integers 2, 3, 5, 6 are intentionally omitted. The slider still snaps
to every integer because of `step={interval}` in the renderer;
omitting the entries just hides the visible ticks at those positions.
Listing `- 2`, `- 3`, etc. would cause the slider to render labels
like "2", "3" (the parser defaults the label to the number when no
text is supplied), which clutters the scale.

### Likert rendering notes (likert variant)

Each Likert item is `type: multipleChoice` (vertical layout, the
default) with all 7 options explicitly listed. Anchor labels at
1 / 4 / 7 with the integer baked into the visible label
("1 - Strongly Disagree", "4 - Neutral", "7 - Strongly Agree");
positions 2, 3, 5, 6 show just the integer as the label. Same anchor
convention as `rate_partner_description`.

## Scoring

Encoding stored in the data is **raw 1–7** (Strongly Disagree → Strongly
Agree), matching the published TIPI scoring conventions. To get
dimension scores, reverse-code the four (R) items with `8 − X` and
average the two items in each pair:

```
Extraversion       = (extroversion         + (8 − quietness))      / 2
Agreeableness      = ((8 − criticalness)   + sympathy)             / 2
Conscientiousness  = (dependability        + (8 − carelessness))   / 2
Emotional Stability= ((8 − anxiety)        + calmness)             / 2
Openness           = (openness             + (8 − conventionality))/ 2
```

This is the standard Gosling et al. (2003) scoring; it's also what
`Watts-Lab/surveys/surveys/TIPI/TIPI.score.js` computes.

**Why raw and not pre-inverted?** The other pilot_3 modules
(`discussion_general`, `perception_of_others`) pre-invert items so the
stored numeric value matches valence. TIPI is different: there isn't a
single "positive valence" direction across the ten items — each
dimension is its own axis, and the (R) labelling is a per-dimension
artifact of which adjective happens to be on which pole. Keeping raw
1–7 lets researchers apply the canonical TIPI formula directly without
remembering which items the module already flipped.

## Field-name differences from the Watts-Lab source

The upstream JSON's field names match what we use here
(`extroversion`, `criticalness`, etc.), so the stagebook storage-key
suffixes line up 1-to-1. The only addition is the `prompt_<stageName>_`
namespacing that the stagebook stageName-extension adds.

---

# Watts-Lab `TIPI` README (preserved below)

The upstream README is short — primarily a brief on what the TIPI
measures and a note on the Watts-Lab adaptation. Included verbatim
below. The screenshot referenced at the top is not included here —
the rendered Stagebook prompts in this folder are the equivalent.

---

# Ten Item Personality Inventory

## Survey Purpose

This particular survey aims to use the Ten-Item Personality Inventory (TIPI) to quantify the particpant's Big-Five personality dimensions.

## Theoretical construct

The Big 5 personality inventory includes

- Extroversion
- Conscientiousness
- Agreeableness
- Openness to experience
- Emotional Stability / Neuroticism

Measuring these traits can be expensive, as the traditional big 5 tests have many questions. This scale takes a rough measure of these attributes, and was originally developed in:

> Gosling, Samuel D., Peter J. Rentfrow, and William B. Swann Jr. "A very brief measure of the Big-Five personality domains." Journal of Research in personality 37.6 (2003): 504-528.

> When time is limited, researchers may be faced with the choice of using an extremely brief measure of the Big-Five personality dimensions or using no measure at all. To meet the need for a very brief measure, 5 and 10-item inventories were developed and evaluated. Although somewhat inferior to standard multi-item instruments, the instruments reached adequate levels in terms of: (a) convergence with widely used Big-Five measures in self, observer, and peer reports, (b) test–retest reliability, (c) patterns of predicted external correlates, and (d) convergence between self and observer ratings. On the basis of these tests, a 10-item measure of the Big-Five dimensions is offered for situations where very short measures are needed, personality is not the primary topic of interest, or researchers can tolerate the somewhat diminished psychometric properties associated with very brief measures.

# Adaptations

The original survey asked people to fill in a number in a blank. Here we provide a range of numerical values.
