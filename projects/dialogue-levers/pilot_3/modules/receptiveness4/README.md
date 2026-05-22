# receptiveness4 module

Four-item adaptation of the Receptiveness to Opposing Views scale
(Minson, Chen, & Tinsley 2020) — one item per factor in the four-factor
oblique-rotation solution from the source paper. Adapted from the
[Watts-Lab `receptiveness4` survey](https://github.com/Watts-Lab/surveys/tree/main/surveys/receptiveness4)
— the Watts-Lab README is preserved verbatim below.

## Files

```
receptiveness4/
├── receptiveness4.stagebook.yaml    ← module entry point (one template)
├── frustrated.prompt.md              (R) · Negative Emotion
├── reading.prompt.md                 (+) · Intellectual Curiosity
├── emotional_arguments.prompt.md     (R) · Derogation of Opponents
├── ideas_dangerous.prompt.md         (R) · Taboo Issues
├── _preview_.stagebook.yaml          standalone tester
└── README.md (this file)
```

## What's in the battery

| # | Storage key suffix    | "..." (1–7 Strongly Disagree → Strongly Agree)                                                                  | Factor                   | Keyed |
|---|-----------------------|------------------------------------------------------------------------------------------------------------------|--------------------------|-------|
| 1 | `frustrated`          | I often feel frustrated when I listen to people with social and political views that oppose mine.                | Negative Emotion         | **R** |
| 2 | `reading`             | I like reading well thought-out information and arguments supporting viewpoints opposite to mine.                | Intellectual Curiosity   | +     |
| 3 | `emotional_arguments` | People who have views that oppose mine often base their arguments on emotion rather than logic.                  | Derogation of Opponents  | **R** |
| 4 | `ideas_dangerous`     | Some ideas are simply too dangerous to be part of public discourse.                                              | Taboo Issues             | **R** |

Display order matches the upstream JSON. The upstream randomizes the
order per-participant (`questionsOrder: "random"`); this module
presents in fixed order — stagebook doesn't expose a shuffle on
template element lists. See "Display order" below.

## How to use

### 1. Import the module from your treatment file

```yaml
imports:
  - ./modules/receptiveness4/receptiveness4.stagebook.yaml
```

### 2. Invoke `receptiveness4_questions` as an introStep / exitStep with a `stageName:` field

```yaml
introSequences:
  - name: default
    introSteps:
      - template: receptiveness4_questions
        fields:
          stageName: receptiveness
```

The module's template emits `contentType: introExitStep` — a
self-contained step that includes all four prompts AND a submitButton
with `exists`-conditions gating advancement on every item. You do
NOT add your own submitButton at the call site. Same shape as the
demographics, political_party_us, and TIPI modules.

The same template works as an `exitStep` in `exitSequence` — `introExitStep`
is the shared content type for both pre- and post-game steps.

### 3. Storage keys

With `stageName: receptiveness`, the module produces:

- `prompt_receptiveness_frustrated`
- `prompt_receptiveness_reading`
- `prompt_receptiveness_emotional_arguments`
- `prompt_receptiveness_ideas_dangerous`

## Two render variants: slider OR Likert radio buttons

The module exports two parallel templates that share the same four
items, same encoding, and same scoring conventions — they differ only
in the response widget:

| Template | Widget | Prompt files | When to use |
|---|---|---|---|
| `receptiveness4_questions` | Slider with three labeled anchors (1/4/7) | `<item>.prompt.md` | Compact UX for a short battery where slider's continuous-feel reads well. |
| `receptiveness4_questions_likert` | Vertical radio row with all 7 options labeled | `<item>_likert.prompt.md` | Default for pilot_3 — the slider renderer has known UX issues (stagebook#326 thumb alignment) and the explicit Likert is clearer to participants. |

Encoding (`raw 1–7`), scoring (`8 − X` for the three (R) items, then
average the four into a composite), and per-item factor assignments
are identical across both variants. Switching templates at a call
site swaps only the widget, not the data.

### Slider rendering notes (slider variant)

Each slider item is `min: 1, max: 7, interval: 1` with anchor labels
at 1 (Strongly Disagree), 4 (Neither agree nor disagree), and 7
(Strongly Agree). Snap is to integers via `interval: 1`. Matches the
TIPI module's slider UX.

### Likert rendering notes (likert variant)

Each Likert item is `type: multipleChoice` (vertical layout, the
default) with all 7 options explicitly listed. Anchor labels at
1 / 4 / 7 with the integer baked into the visible label
("1 - Strongly Disagree", "4 - Neither agree nor disagree", "7 - Strongly Agree");
positions 2, 3, 5, 6 show just the integer as the label. Same anchor
convention as `rate_partner_description` and the TIPI Likert variant.

The upstream Watts-Lab survey renders the same items as a
`displayMode: "buttons"` radio row with all 7 anchor labels and an
HTML intro paragraph that restates the full scale key. We don't
reproduce the intro paragraph — the per-item anchors carry enough
information for participants.

## Scoring

Encoding stored in the data is **raw 1–7** (Strongly Disagree →
Strongly Agree), matching the upstream `receptiveness4.score.js`
function. To produce factor scores and an overall receptiveness
composite, apply `8 − X` to each (R) item and then average:

```
NegativeEmotion         = 8 − frustrated
IntellectualCuriosity   =     reading
DerogationOfOpponents   = 8 − emotional_arguments
TabooIssues             = 8 − ideas_dangerous

Receptiveness = mean(NegativeEmotion,
                     IntellectualCuriosity,
                     DerogationOfOpponents,
                     TabooIssues)
```

Higher = more receptive to opposing views. Both raw 1–7 and 0–1
normalized variants are computed in the upstream score function:
`normalized = (raw - 1) / 6`.

**Why raw and not pre-inverted?** Matches the TIPI module's policy.
Pre-inverting at the prompt level would force the slider to visually
flip for (R) items (Strongly Agree on the left, Strongly Disagree on
the right) because stagebook's slider couples numeric value directly
to physical position. Keeping raw 1–7 across the battery preserves
visual consistency for participants — every item shows SD on the
left and SA on the right. The cost is a small bit of analyst work at
scoring time.

## Display order

The upstream uses `questionsOrder: "random"` on the panel containing
the four items, so the order is randomized per participant. Stagebook
doesn't currently expose a shuffle option on prompt element lists, so
this module presents in fixed display order:

  1. `frustrated`
  2. `reading`
  3. `emotional_arguments`
  4. `ideas_dangerous`

This matches the order in the upstream JSON. With only four items the
position effect is small, but if it turns out to matter you'd need a
stagebook enhancement for a per-element-list `shuffle:` field.

## Required-field enforcement

The template's baked-in submitButton has `comparator: exists`
conditions on each of the four items. The slider's value space
(`min: 1, max: 7, interval: 1`) doesn't have a meaningful "unset"
state for an interacted item — once the participant clicks the bar to
anchor the thumb, every value 1–7 is a legitimate response. The
`exists` gate enforces "the participant interacted with each item,"
which is the most we can verify at the platform level.

## Field-name differences from the Watts-Lab source

Upstream JSON field names use camelCase (`emotionalArguments`,
`ideasDangerous`). This module uses snake_case
(`emotional_arguments`, `ideas_dangerous`) matching the convention in
the other pilot_3 modules. The stagebook `prompt_<stageName>_` namespace
prepends to all of them.

---

# Watts-Lab `receptiveness4` README (preserved below)

The upstream README explains the item-selection rationale (one item
per factor in the four-factor solution from the source paper's oblique
rotation factor analysis; the 4-item set correlates r=0.890 with the
full 18-item scale). Included verbatim below. The screenshot and
correlation plot referenced are not included here — the rendered
Stagebook prompts in this folder are the equivalent for the questions.

---

# Four Item Receptiveness to Opposing Views

This survey adapts (Minson, Julia A., Frances S. Chen, and Catherine H. Tinsley. 2020. "Why Won't You Listen to Me? Measuring Receptiveness to Opposing Views." Management Science 66 (7): 3069–94.) by reducing the number of questions from 18 to 4.

Our goals in selecting these are to choose questions that taken together yield the highest correlation with the full 18-item scale, while also yielding questions that are sensible, and if possible correspond to the different primary factors listed in the paper.

The obvious way to do this is to just use the questions that have the highest factor loading in the paper. There are two forms of factor analysis reported:

#### Varimax rotation factor analysis

This forces the factors to be orthogonal, and suggests the following questions:

> 1. ' I often get annoyed during discussions with people with views that are very different from mine. (R)'
> 2. ' I like reading well thought-out information and arguments supporting viewpoints opposite to mine.',
> 3. ' People who have views that oppose mine often base their arguments on emotion rather than logic. (R) ',
> 4. ' Some ideas are simply too dangerous to be part of public discourse. (R) '

#### Oblique rotation factor analysis

This allows the factors to be correlated, and is probably what we want to pay attention to for the purpos

> 1. ' I often feel frustrated when I listen to people with social and political views that oppose mine. (R) ',
> 2. ' I like reading well thought-out information and arguments supporting viewpoints opposite to mine.',
> 3. ' People who have views that oppose mine often base their arguments on emotion rather than logic. (R) ',
> 4. ' Some ideas are simply too dangerous to be part of public discourse. (R) '

The two methods are produce the same result for 3 of the 4 factors, and the difference in the remaining factor is in both cases less than 0.4%. As a result, we can choose to use the four factors from the oblique rotation analysis.

## Double-checking this choice

We can double-check that this result is reasonable by comparing the correlation of these four items against the correlation with every other four-item question set. Using the combined dataset for all studies reported in the paper (rather than just the study used to design the scale) gives 16698 rows. The best performing four-item question set gives a correlation of 0.902, while the correlation of the most loaded factors is 0.890, about a 1% difference. As such, we safely defer to the paper's listed highest loaded factors.

We also compare the performance of the 4-item scale to the best performing 2-item scale (correlation 0.790) and 8 itme scale (.961). Using a 4-item version seems to be a reasonable compromise between fidelity and brevity, while still allowing us to extract the component factors.
