# Team

- Burint Bevis
- Mark Kennedy
- James Houghton
- Emily Hu

# Demo config for lost at sea

```json
{
  "batchName": "demo",
  "treatmentFile": "projects/constructive_disagreement/treatments.yaml",
  "launchDate": "",
  "dispatchWait": 1,
  "introSequence": "baseline",
  "consentAddendum": "projects/constructive_disagreement/00_consentAddendum.md",
  "cdn": "local",
  "treatments": ["baseline"],
  "videoStorageLocation": "deliberation-lab-recordings-test",
  "preregister": false,
  "dataRepos": [
    {
      "owner": "Watts-Lab",
      "repo": "deliberation-data-test",
      "branch": "main",
      "directory": "cypress_test_exports"
    }
  ]
}
```

# Demo config for hidden profile baker

```json
{
  "batchName": "demo",
  "treatmentFile": "projects/constructive_disagreement/hidden_profile/treatments_baker_2010.yaml",
  "launchDate": "",
  "dispatchWait": 1,
  "introSequence": "baseline",
  "consentAddendum": "projects/constructive_disagreement/00_consentAddendum.md",
  "cdn": "local",
  "treatments": ["baseline"],
  "videoStorageLocation": "deliberation-lab-recordings-test",
  "preregister": false,
  "dataRepos": [
    {
      "owner": "Watts-Lab",
      "repo": "deliberation-data-test",
      "branch": "main",
      "directory": "cypress_test_exports"
    }
  ]
}
```

```json
{
  "batchName": "demo",
  "treatmentFile": "projects/constructive_disagreement/hidden_profile/treatments_baker_2010.yaml",
  "launchDate": "",
  "dispatchWait": 1,
  "exitCodeStem" : "none",
  "cdn": "local",
  "introSequence": "baseline",
  "consentAddendum": "projects/constructive_disagreement/00_consentAddendum.md",
  "treatments": ["baseline"],
  "videoStorageLocation": "deliberation-lab-recordings-test",
  "preregister": false,
  "dataRepos": [
    {
      "owner": "JamesPHoughton",
      "repo": "constructive-disagreement",
      "branch": "main",
      "directory": "test"
    }
  ]
}
```

# Demo config for super sabbatical
```json
{
  "batchName": "super_sabbatical",
  "introSequence": "baseline",
  "consentAddendum": "projects/constructive_disagreement/00_consentAddendum.md",
  "treatmentFile": "projects/constructive_disagreement/super_sabbatical/super_sabbatical.treatments.yaml",
  "dispatchWait": 10,
  "exitCodeStem" : "none",
  "treatments": ["negotiation"],
    "payoffs": [1],
  "cdn": "local",
  "videoStorageLocation": "deliberation-lab-recordings-test",
  "dataRepos": [
    {
      "owner": "JamesPHoughton",
      "repo": "constructive-disagreement",
      "branch": "main",
      "directory": "test"
    }
  ]
}
```

# Demo config for super sabbatical (WBL)
```json
{
  "batchName": "super_sabbatical_wbl",
  "introSequence": "baseline",
  "consentAddendum": "projects/constructive_disagreement/00_consentAddendum.md",
  "treatmentFile": "projects/constructive_disagreement/super_sabbatical_wbl/super_sabbatical_wbl.treatments.yaml",
  "dispatchWait": 10,
  "exitCodeStem": "none",
  "treatments": [
    "negotiation"
  ],
  "cdn": "local",
  "videoStorageLocation": "deliberation-lab-recordings-test",
  "dataRepos": [
    {
      "owner": "JamesPHoughton",
      "repo": "constructive-disagreement",
      "branch": "main",
      "directory": "test"
    }
  ]
}
```

Prod configuration for super sabbatical (WBL)
```
{
  "batchName": "super_sabbatical_wbl",
  "introSequence": "baseline",
  "consentAddendum": "projects/constructive_disagreement/00_consentAddendum.md",
  "treatmentFile": "projects/constructive_disagreement/super_sabbatical_wbl/super_sabbatical_wbl.treatments.yaml",
  "dispatchWait": 10,
  "exitCodeStem": "none",
  "treatments": [
    "negotiation"
  ],
  "cdn": "prod",
  "videoStorageLocation": "deliberation-lab-recordings-icbs",
  "dataRepos": [
    {
      "owner": "JamesPHoughton",
      "repo": "constructive-disagreement",
      "branch": "main",
      "directory": "super_sabbatical_wbl"
    }
  ],
  "preregRepos": [
    {
      "owner": "JamesPHoughton",
      "repo": "constructive-disagreement",
      "branch": "main",
      "directory": "super_sabbatical_wbl"
    }
  ]
}
```