jignore this # Deliberation Assets

This repository contains assets served to participants in deliberation experiments.

## License

The collection presented here is the work of a variety of authors. Unless otherwise specified in `README.md`or`LICENSE` files within subfolders, the original authors retain all rights to the materials presented.

## Production Usage

This repo is synced to a CDN, but also provides version control for files, so that
we can refer to the precise version of a file when tracing the data.

## Dev Usage

During dev, you can start this as a local webserver by running:

```bash
npx http-server --cors -a localhost -p 9090
```

or:

```bash
npx http-server --cors -a 127.0.0.1 -p 9090
```

or:

```bash
npx serve -l 9090 --cors
```

In the deliberation-empirica admin, specify the cdn as:

```json
{
  "batchName": "demo",
  "treatmentFile": "projects/example/treatments.demo.yaml",
  "dispatchWait": 1,
  "introSequence": "demoIntro",
  "cdn": "local",
  "treatments": ["demo_2p"]
}
```

## Defining an experiment

## Supported Config Options

- **batchName** name to use in filepath of saved data
- **treatmentFile** path relative to the root of the repository to the treatment file containing the treatments to be included in the batch. At the moment you can only use one treatment file.
- **introSequence** is the name of the sequence defined in **treatmentFile** to be shown to all participants prior to assignment to treatment condition
- **treatments** list of strings, each string corresponding to a treatment condition defined in the **treatmentFile**
- **useData** [true/false] whether the data collected in this batch should be preregistered and used in data analysis. Use false when testing or developing
- **dispatchWait** window for collecting participants before randomizing to groups, in seconds
- **platformConsent** [US/UK/EU] which of several pre-baked consent forms to show to participants
- **consentAddendum** path to a markdown file containing contents to be appended to the end of the consent form, that can be used to provide particular information about collaborating research teams.
- **launchDate** date at which randomization to groups can begin
- **dataRepos**: list of objects describing the repo, branch, and directory where data should be stored. Need to include deliberaiton-data-private as well.

```json
[
  {
    "owner": "Watts-Lab",
    "repo": "deliberation-data-test",
    "branch": "main",
    "directory": "cypress_test_exports"
  }
  //... other repos that should also get the data
]
```

## Future Config Options:

- **embargoThrough**: a datestring after which data collected in this batch can be automatically made public
- **videoStorageBucke**t\*\*: path and name of an AWS S3 storage bucket for video recordings to be stored in
