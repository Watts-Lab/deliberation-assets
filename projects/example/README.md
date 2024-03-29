# Example Study

This project holds example files used to demonstrate how to construct the various resources needed by the platform, and that are also used in the test suite.

To run a single-player demo, use a variation on the config:

```json
{
  "batchName": "labDemo",
  "treatmentFile": "projects/example/treatments.test.yaml",
  "dispatchWait": 1,
  "useTreatments": ["demo1p"],
  "useIntroSequence": "cypress_standard",
  "platformConsent": "US",
  "consentAddendum": "projects/example/consentAddendum.md",
  "launchDate": "01 Mar 2023 23:30:00 EST"
}
```

```json
{
  "batchName": "test_etherpad",
  "treatmentFile": "projects/example/treatments.demo.yaml",
  "dispatchWait": 1,
  "preregister": false,
  "treatments": ["demo_2p_etherpad"],
  "videoStorageLocation": "none",
  "exitCodeStem": "none",
  "cdn": "prod",
  "checkAudio": false,
  "checkVideo": false,
  "dataRepos": [
    {
      "owner": "Watts-Lab",
      "repo": "deliberation-data-test",
      "branch": "main",
      "directory": "demo"
    }
  ]
}
```

```json
{
  "batchName": "labDemo",
  "treatmentFile": "projects/example/treatments.test.yaml",
  "dispatchWait": 1,
  "useTreatments": ["demo1p"]
}
```

## Demo

```json
{
  "batchName": "labDemo",
  "preregister": "false",
  "treatmentFile": "projects/example/treatments.demo.yaml",
  "dispatchWait": 1,
  "introSequence": "demoIntro",
  "treatments": ["demo_2p"],
  "cdn": "test",
  "platformConsent": "US",
  "consentAddendum": "projects/example/consentAddendum.md",
  "launchDate": "14 Jul 2023 18:45:00 CEST"
}
```

# Local Demo

```json
{
  "batchName": "demo",
  "preregister": "false",
  "treatmentFile": "projects/example/treatments.demo.yaml",
  "dispatchWait": 1,
  "introSequence": "demoIntro",
  "treatments": ["demo_2p"],
  "cdn": "local",
  "platformConsent": "US",
  "consentAddendum": "projects/example/consentAddendum.md",
  "videoStorageLocation": "deliberation-lab-recordings-test",
  "dataRepos": [
    {
      "owner": "Watts-Lab",
      "repo": "deliberation-data-test",
      "branch": "main",
      "directory": "demo"
    }
  ]
}
```

# Test video recording

```json
{
  "batchName": "demo",
  "preregister": "false",
  "treatmentFile": "projects/example/treatments.demo.yaml",
  "dispatchWait": 1,
  "treatments": ["demo_4p_test_video"],
  "cdn": "prod",
  "platformConsent": "US",
  "exitCodeStem": "none",
  "consentAddendum": "projects/example/consentAddendum.md",
  "videoStorageLocation": "deliberation-lab-recordings-public",
  "dataRepos": [
    {
      "owner": "Watts-Lab",
      "repo": "deliberation-data-test",
      "branch": "main",
      "directory": "demo"
    }
  ]
}
```

# Test etherpad

```json
{
  "batchName": "demo",
  "preregister": "false",
  "treatmentFile": "projects/example/treatments.demo.yaml",
  "dispatchWait": 1,
  "treatments": ["demo_2p_etherpad"],
  "cdn": "local",
  "videoStorageLocation": false,
  "checkAudio": false,
  "checkVideo": false,
  "dataRepos": [
    {
      "owner": "Watts-Lab",
      "repo": "deliberation-data-test",
      "branch": "main",
      "directory": "demo"
    }
  ]
}
```

# Prod demo

```json
{
  "batchName": "demo",
  "preregister": "false",
  "treatmentFile": "projects/example/treatments.demo.yaml",
  "dispatchWait": 1,
  "introSequence": "demoIntro",
  "treatments": ["demo_2p"],
  "cdn": "prod",
  "platformConsent": "US",
  "consentAddendum": "projects/example/consentAddendum.md",
  "videoStorageLocation": "deliberation-lab-recordings-test",
  "dataRepos": [
    {
      "owner": "Watts-Lab",
      "repo": "deliberation-data-test",
      "branch": "main",
      "directory": "demo"
    }
  ]
}
```
