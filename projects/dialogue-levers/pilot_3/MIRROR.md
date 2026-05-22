# Mirror provenance

This subtree is a snapshot copy from a sibling private repo — kept here so the
public S3 mirror of `deliberation-assets` can serve it as a treatment + assets
bundle.

| Field | Value |
| --- | --- |
| Source repo | [`Watts-Lab/dialogue-levers`](https://github.com/Watts-Lab/dialogue-levers) (private) |
| Source path | `stagebook/pilot_3/` |
| Source SHA at mirror | `06e33499ed88651f45e5daf9dd6d88c67a81ad2f` |
| Source commit date | 2026-05-20 |
| Mirrored on | 2026-05-22 |

## Why a copy lives here

`dialogue-levers` is private, so the Deliberation Lab runtime (which fetches
treatments and prompt files over plain unauthenticated HTTPS) can't pull
directly from `raw.githubusercontent.com`. Mirroring into this public-read repo
sends the tree through `.github/workflows/s3_mirror.yml` to
`https://s3.amazonaws.com/assets.deliberation-lab.org/`, where the runtime can
reach it.

The snapshot was taken to drive a no-manager Railway deploy used to reproduce
the early-lifecycle memory regression we've been chasing on production —
pilot_3 is the workload under which that regression has been observed. The
other configs already in `deliberation-assets/projects/` are on the legacy
`*.treatments.yaml` syntax and don't run on the current stagebook bundle.

## Refreshing

If `pilot_3` evolves upstream and you want the live deploy to track it:

```bash
cd /path/to/Watts-Lab/dialogue-levers && git pull
rsync -a --delete \
  /path/to/Watts-Lab/dialogue-levers/stagebook/pilot_3/ \
  /path/to/Watts-Lab/deliberation-assets/projects/dialogue-levers/pilot_3/
# preserve this MIRROR.md when rsync --delete'ing — re-stamp the SHA + date
```

Commit and push from `deliberation-assets`; the `Mirror repo to S3` workflow
will publish to the bucket within ~1–2 minutes.

## Footprint at the bucket

```
https://s3.amazonaws.com/assets.deliberation-lab.org/projects/dialogue-levers/pilot_3/
```

So a batch config that wants this treatment would set:

```json
{
  "assetBaseUrl": "https://s3.amazonaws.com/assets.deliberation-lab.org",
  "treatmentFile": "projects/dialogue-levers/pilot_3/pilot_3.stagebook.yaml"
}
```
