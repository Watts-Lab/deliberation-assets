# political_party_us module

Standard ANES party-identification protocol: a root party-ID item with
three branching follow-ups (strength-of-identification for partisans,
direction-of-lean for Independents, free-text for "Other"). Adapted
from the
[Watts-Lab `politicalPartyUS` survey](https://github.com/Watts-Lab/surveys/tree/main/surveys/politicalPartyUS)
— the Watts-Lab README is preserved verbatim below.

## What was kept and what was dropped

This module implements **only page 1** of the upstream survey: the
categorical party-ID item and its conditional strength/lean
follow-ups. **The page-2 identity-importance sliders** (`republican_
importance`, `democrat_importance`, `independent_importance`,
`other_importance` — all 0–100 sliders) **are intentionally omitted**;
the pilot_3 analysis plan only uses the categorical party-ID for
matching/blocking and the strength/lean items to recover the 7-point
scale. To add the importance items back, port the `page2` elements
from the upstream JSON.

## Files

```
political_party_us/
├── political_party_us.stagebook.yaml   ← module entry point (one template)
├── party.prompt.md                      always shown
├── party_other.prompt.md                shown if party = "Other"
├── republican_strength.prompt.md        shown if party = "Republican"
├── democrat_strength.prompt.md          shown if party = "Democrat"
├── independent_lean.prompt.md           shown if party = "Independent"
├── _preview_.stagebook.yaml             standalone tester
└── README.md (this file)
```

## What's in the battery

| # | Storage key suffix     | Question                                                                                              | Shown when             |
|---|------------------------|-------------------------------------------------------------------------------------------------------|------------------------|
| 1 | `party`                | Generally speaking, do you usually think of yourself as a Republican, a Democrat, an Independent, …?  | Always                 |
| 2 | `party_other`          | Which political party do you identify with? (free text, 1 row)                                        | `party = "Other"`      |
| 3 | `republican_strength`  | Strong Republican / Not very strong Republican                                                        | `party = "Republican"` |
| 4 | `democrat_strength`    | Strong Democrat / Not very strong Democrat                                                            | `party = "Democrat"`   |
| 5 | `independent_lean`     | Closer to R Party / Neither / Closer to D Party                                                       | `party = "Independent"`|

In any given session exactly one of items 2–5 fires, depending on the
root choice. Item 1 plus the unlocked follow-up fully determines the
participant's position on the canonical 7-point scale (see Scoring).

## How to use

### 1. Import the module from your treatment file

```yaml
imports:
  - ./modules/political_party_us/political_party_us.stagebook.yaml
```

### 2. Invoke `political_party_us_questions` as an introStep with a `stageName:` field

```yaml
introSequences:
  - name: default
    introSteps:
      - template: political_party_us_questions
        fields:
          stageName: party_affiliation
```

The module's template emits `contentType: introExitStep` — a
self-contained step that includes the root prompt, the four
conditional follow-ups, AND a submitButton that enforces the
upstream's required-field logic (root always required; whichever
follow-up is unlocked by the branch is required to advance). You do
NOT add your own submitButton at the call site. Same shape as the
demographics module.

If you need to invoke this as an exitStep (post-game) instead of an
introStep, the template works identically — `introExitStep` is the
shared content type for both pre- and post-game steps.

### 3. Storage keys

With `stageName: party_affiliation`, the module produces:

- `prompt_party_affiliation_party` (always)
- `prompt_party_affiliation_party_other` (if Other)
- `prompt_party_affiliation_republican_strength` (if Republican)
- `prompt_party_affiliation_democrat_strength` (if Democrat)
- `prompt_party_affiliation_independent_lean` (if Independent)

## Scoring: the canonical 7-point party-ID scale

Items 1 + 3/4/5 collapse into a unidimensional scale of partisanship
intensity, which is the most common way the literature uses these
questions (Van Holsteyn & Irwin 2017; ANES). One reasonable mapping:

| Scale position | Code | Combined response                                            |
|----------------|------|--------------------------------------------------------------|
| Strong Republican       | 1 | `party = Republican`  + `republican_strength = Strong`        |
| Weak Republican         | 2 | `party = Republican`  + `republican_strength = Not very strong` |
| Lean Republican         | 3 | `party = Independent` + `independent_lean = Closer to R`     |
| Independent             | 4 | `party = Independent` + `independent_lean = Neither`         |
| Lean Democrat           | 5 | `party = Independent` + `independent_lean = Closer to D`     |
| Weak Democrat           | 6 | `party = Democrat`    + `democrat_strength = Not very strong`  |
| Strong Democrat         | 7 | `party = Democrat`    + `democrat_strength = Strong`           |

`party = Other` doesn't slot onto this axis; treat as missing (or
analyze separately with the `party_other` free-text response).

## Field-name differences from the Watts-Lab source

Upstream JSON uses camelCase (`partyOther`, `republicanStrength`,
`democratStrength`, `independentLean`). This module uses snake_case
(`party_other`, `republican_strength`, …) matching the convention in
the other pilot_3 modules. The stagebook `prompt_<prefix>_` namespace
prepends to all of them.

## Conditional-display mechanics

Stagebook's `conditions:` block on a prompt element gates its
rendering at runtime — if the referenced value doesn't match, the
prompt simply isn't shown. There's no need to put each follow-up in a
separate page; they all live in the same step and only the relevant
one(s) become visible after the participant answers the root.

The upstream's `requiredIf:` semantics (root always required;
follow-up required IFF its branch was chosen) are enforced by the
template's built-in submitButton, using an `any:` boolean tree per
follow-up: either the branch wasn't chosen (so this follow-up isn't
expected), OR the follow-up exists.

## Why introExitStep and not elements?

The first version of this module emitted `contentType: elements` (a
flat list of prompts that the caller embedded in their own step,
with their own submitButton). That fails schema validation, because
the validator runs BEFORE template expansion — so a call-site
submitButton's conditions referencing `self.prompt.${prefix}_party`
can't be resolved (the validator hasn't yet expanded the template to
know which storage keys it produces). See
[stagebook#317/318](https://github.com/deliberation-lab/stagebook/issues/317).

Baking the prompts AND the submit button into a single template, and
having the template emit a complete step (`contentType: introExitStep`)
keeps the references all inside the template definition where the
validator can expand them together. Same pattern as the demographics
module.

---

# Watts-Lab `politicalPartyUS` README (preserved below)

The upstream README is short — primarily the citation context for the
ANES wording and a note that the page-1 questions are implemented
verbatim. Included below. The screenshots referenced are not included
here — the rendered Stagebook prompts in this folder are the
equivalent for page 1; page 2 (importance sliders) is intentionally
not implemented (see "What was kept and what was dropped" above).

---

# Political Party US

# Party Identification

From `Van Holsteyn & Galen A. Irwin, Joop J. M. 2017. "Party Identification." In The SAGE Encyclopedia of Political Behavior, edited by Fathali M. Moghaddam. SAGE Publications, Inc.`

> The operationalization of party identification by Angus Campbell and his colleagues in the 1950s and 1960s has become a staple in the American national election studies. The standard question wording in these studies has been this: _Generally speaking, do you usually think of yourself as a Republican, a Democrat, an Independent, or what?_ The phrase think of yourself emphasizes self-identification, while generally and usually indicate the identification with the party should be an enduring one.

> This standard question primarily taps the direction of party identification and a follow-up question is assumed to tap its intensity. For Republican and Democratic identifiers, the follow-up question is, _Would you call yourself a strong (Republican/Democrat) or a not very strong (Republican/Democrat)?_ For Independents, direction and intensity are combined in one question: Do you think of yourself as closer to the Republican Party or to the Democratic Party? The results of these questions can be combined to form a 7-point unidimensional scale: Strong Republican, Weak Republican, Leaning Republican, Independent, Leaning Democrat, Weak Democrat, and Strong Democrat.

These initial questions are implemented verbatim.

## Used in:

- `Druckman, James N., and Matthew S. Levendusky. 2019. "What Do We Measure When We Measure Affective Polarization?" Public Opinion Quarterly 83 (1): 114–22.`

- `Jan G. Voelkel, Michael N. Stagnaro, James Chu, Sophia Pink, Joseph S. Mernyk, Chrystal Redekopp, Matthew Cashman, James N. Druckman, David G. Rand, and Robb Willer. 2021. "Megastudy Identifying Successful Interventions to Strengthen Americans' Democratic Attitudes,"`

Followup questions are taken from: `Jan G. Voelkel, Michael N. Stagnaro, James Chu, Sophia Pink, Joseph S. Mernyk, Chrystal Redekopp, Matthew Cashman, James N. Druckman, David G. Rand, and Robb Willer. 2021. "Megastudy Identifying Successful Interventions to Strengthen Americans' Democratic Attitudes,"`
