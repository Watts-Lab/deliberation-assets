# Demographics module (short US)

Reusable Stagebook module that emits a short demographic battery for
US-resident participants. Adapted from the
[Watts-Lab `demographicsShortUS` survey](https://github.com/Watts-Lab/surveys/tree/main/surveys/demographicsShortUS).

## Files

```
demographics/
├── demographics.stagebook.yaml        ← module entry point (templates only)
├── birth_year.prompt.md
├── gender.prompt.md
├── gender_other.prompt.md             (conditional)
├── language_primary.prompt.md
├── language_primary_expansion.prompt.md  (conditional)
├── education.prompt.md
├── race.prompt.md
├── latin.prompt.md
└── README.md (this file)
```

## What's in the battery

Eight question elements, in display order:

| # | Field | Type | Required | Notes |
|---|---|---|---|---|
| 1 | `birth_year` | openResponse (1 row, max 4 chars) | yes | 4-digit year |
| 2 | `gender` | multipleChoice (single) | yes | Female / Male / Other |
| 3 | `gender_other` | openResponse (1 row) | conditional | Shown only if `gender` == "Other" |
| 4 | `language_primary` | multipleChoice (single) | yes | ~30 languages + Other |
| 5 | `language_primary_expansion` | openResponse (1 row) | conditional | Shown only if `language_primary` == "Other" |
| 6 | `education` | multipleChoice (single) | yes | 7 US education levels |
| 7 | `race` | multipleChoice (multiple) | yes | 6 categories, multi-select |
| 8 | `latin` | multipleChoice (single) | no | Yes / No |

Field names are storage suffixes — each is prefixed with the
`${prefix}` you pass at the call site (see below).

## How to use

### 1. Import the module from your treatment file

```yaml
imports:
  - ./modules/demographics/demographics.stagebook.yaml
```

(Path is relative to the importing file. If your modules live in a
sibling directory, adjust accordingly.)

### 2. Invoke `demographics_questions` with a `prefix:` field

The prefix becomes part of every element's storage key, so you can
invoke the module more than once in the same treatment without
key collisions.

```yaml
introSteps:
  - name: demographics
    elements:
      - template: demographics_questions
        fields:
          prefix: demographics
      - type: submitButton
        buttonText: Continue
        conditions:
          - reference: self.prompt.demographics_birth_year
            comparator: exists
          - reference: self.prompt.demographics_gender
            comparator: exists
          - reference: self.prompt.demographics_language_primary
            comparator: exists
          - reference: self.prompt.demographics_education
            comparator: exists
          - reference: self.prompt.demographics_race
            comparator: exists
```

The submitButton conditions enforce the required fields. The two
conditional fields (`gender_other`, `language_primary_expansion`) and
the optional `latin` field are not required for advancement.

### 3. Resulting storage keys

With `prefix: demographics`, the module produces these keys (each
prefixed with `prompt_`):

- `prompt_demographics_birth_year`
- `prompt_demographics_gender`
- `prompt_demographics_gender_other` (only if gender == "Other")
- `prompt_demographics_language_primary`
- `prompt_demographics_language_primary_expansion` (only if language == "Other")
- `prompt_demographics_education`
- `prompt_demographics_race`
- `prompt_demographics_latin`

## Differences from the Watts-Lab source

The original [`demographicsShortUS.json`](https://github.com/Watts-Lab/surveys/blob/main/surveys/demographicsShortUS/demographicsShortUS.json)
is a SurveyJS schema; this module is the Stagebook equivalent. One
fidelity note:

- **Birth-year numeric validation isn't enforced.** The original uses
  SurveyJS's `numeric` validator with `minValue: 1900, maxValue: 2022`;
  Stagebook's `openResponse` doesn't currently support numeric
  constraints, so the field accepts any 1–4 character string. The
  placeholder hint ("4-digit year") communicates the expectation.

The `language_primary` prompt uses the
[`dropdown` type](https://github.com/deliberation-lab/stagebook/pull/319)
to render the long language list as a `<select>` rather than a stack
of radios. Requires Stagebook ≥ the build that includes PR #319.

The question wording, choice labels, conditional-visibility logic, and
required/optional designations match the source verbatim.
