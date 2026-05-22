# Discussion-general module

Nine-item Likert battery capturing distinct dimensions of a participant's
subjective experience of a just-completed discussion: learning, depth,
disagreement perception, atmosphere, speaking opportunity, comfort
speaking up, anxiety, felt judgment, and self-insight. Adapted from
the
[Watts-Lab `discussionGeneral` survey](https://github.com/Watts-Lab/surveys/tree/main/surveys/discussionGeneral)
— the Watts-Lab README is preserved verbatim below.

**Note:** the upstream's `enjoy` item was pulled from this module on
2026-05-11 and consolidated into the new `overall_affect` module as
the realized-quality component of the affect composite. See ADR
[2026-05-11-overall-affect-and-forecast-design.md](../../../../docs/decisions/2026-05-11-overall-affect-and-forecast-design.md).

## Files

```
discussion_general/
├── discussion_general.stagebook.yaml   ← module entry point (one template)
├── learned.prompt.md
├── depth.prompt.md
├── disagreement.prompt.md
├── tension.prompt.md
├── speak_up.prompt.md
├── voice.prompt.md
├── anxious.prompt.md
├── judged.prompt.md
├── insight.prompt.md
└── README.md (this file)
```

## What's in the battery

| Item | Storage key suffix | Question | Valence |
|---|---|---|---|
| 1 | `learned` | How much did you learn during the discussion? | Positive |
| 2 | `depth` | How thoroughly did your discussion explore different ideas and perspectives? | Positive |
| 3 | `disagreement` | How much did participants disagree on the discussion topic? | Neutral (manipulation check) |
| 4 | `tension` | How would you describe the emotional atmosphere of the discussion? | **Reverse** (1.0 = tense, 0.0 = relaxed in upstream encoding) |
| 5 | `speak_up` | Did you get a chance to speak when you had something to say? | Positive |
| 6 | `voice` | Did you feel comfortable speaking up and sharing your opinions? | Positive |
| 7 | `anxious` | How often did you feel anxious during the discussion? | **Negative** (high = more anxiety) |
| 8 | `judged` | During the discussion, did you feel that you or people like you were being judged? | **Negative** (high = more felt judgment) |
| 9 | `insight` | How much did this discussion help you understand your own thoughts more deeply? | Positive |

All nine items are five-point Likert and required in the source schema.
Display order in the module matches the table above. The upstream's
`enjoy` item is no longer here — see the note above.

## How to use

### 1. Import the module from your treatment file

```yaml
imports:
  - ./modules/discussion_general/discussion_general.stagebook.yaml
```

### 2. Invoke `discussion_general_questions` with a `prefix:` field

```yaml
gameStages:
  - name: perception_of_discussion
    duration: 180
    elements:
      - template: discussion_general_questions
        fields:
          prefix: perception_of_discussion
      # (optional) submitButton with required-field conditions
```

The module emits `contentType: elements` — a flat list of ten prompt
elements. It does **not** include a submitButton; if you want to enforce
all-ten-required advancement, add a submit button at the call site with
`comparator: exists` conditions on each of the ten produced storage
keys (the keys are listed above).

### 3. Storage keys

With `prefix: perception_of_discussion`, the module produces:

- `prompt_perception_of_discussion_learned`
- `prompt_perception_of_discussion_depth`
- … (and 7 more — see table)

## Field-name differences from the Watts-Lab source

The upstream JSON's field names use camelCase with `discussion`/`self`
prefixes (e.g. `discussionEnjoy`, `selfLearned`); this module uses short
snake_case names (e.g. `enjoy`, `learned`) since the stagebook prefix-
extension already namespaces them. The mapping is documented in each
prompt file's `notes:` field. If you want to apply the upstream
`discussionGeneral.score.js` scoring function directly, you'll need to
rename the keys in your data export — or write a small mapping.

**Numeric encoding.** Each item is in stagebook's numeric multipleChoice
mode (`- 0: Not at all` … `- 1: A great deal`), so participants see the
text labels and stagebook stores both the label and the 0–1 normalized
numeric value.

**Numeric direction matches valence.** Items with a clear positive /
negative valence are encoded so that **higher = more positive experience**
across the board — researchers can average the items into a composite
without per-item inversion. This means three items diverge from the
upstream encoding (where direction follows display-order conventions
that don't match valence):

| Item | Direction | Upstream conversion |
|---|---|---|
| `tension` | high = more relaxed | `1 - upstream` |
| `anxious` | high = less anxious | `1 - upstream` |
| `judged` | high = less judged | `1 - upstream` |

The participant sees the same display order as the upstream schema for
all items; only the stored numeric values are flipped on the three
reverse-coded items.

**`disagreement` is a manipulation-check item, not a valence item.**
Higher = more disagreement (matches upstream display order). Whether
that's "good" or "bad" depends on study context — in pilot_3 the
matching algorithm is intended to produce disagreement, so high values
mean the manipulation worked; in other studies the interpretation may
differ. The item is not inverted and should not be averaged into a
positive-valence composite.

---

# Watts-Lab `discussionGeneral` README (preserved below)

The upstream README is brief — primarily design-rationale notes on why
"no opinion" options are excluded, why option ordering puts the
socially-desirable answer at the bottom, and other measurement-quality
notes. Included verbatim below.

The screenshot referenced at the top is not included here — the
rendered Stagebook prompts in this folder are the equivalent.

---

# General Discussion Itmes

### Screenshot

![Screenshot](screenshot.png)

## Design notes

- We choose not to use "NA" / "No opinion" / "I haven't thought about this" answer options. [Krosnick et al 2002] argue that "inclusion of no-opinion options in attitude measures may not enhance data quality and instead may preclude measurement of some meaningful opinions", as does [Krosnick, Judd and Wittenbrink 2014] and [Boudreau and Lupia 2011]. The GSS is moving away from "Don't Know" style questions in favor of letting participants skip questions [Davern et al. 2024], although in their context skipping questions has more to do with preserving participant comfort and privacy.
- Some of these questions expect to see a saturation effect, as some of the things we care about are important when they go wrong, but usually go right. There are several biases that make saturation even more likely - a _primacy effect_ when response choices are listed in order means that participants tend to choose items at the top of the list, a _social desirability effect_ which leads people to be more likely to respond with the more socially acceptable answer, and an _acquiescence bias_, which means that certain groups of people are more likely to choose "agree" regardless of their actual feelings [Pew Research Center 2021]. To deal with these interacting effects, we try to minimize use of agree/disagree scales, and to counterbalance the effect of social desirability (and saturation) by putting the more socially desirable answer at the bottom of the list.
- We also try to reduce the cognitive effort associated with answering the question. This should help us get better responses, as we'll have fewer people just randomly clicking or satisficing, or alternately experiencing cognitive overload. It also means we can ask more questions in the same amount of time. Some ways to do this:
  - by shortening the questions and making them easy to read
  - by minimizing the amount of "translation" people have to do between their views and a numerical scale or agree/disagree scale
  - by using language in the responses that directly ties into the question,
