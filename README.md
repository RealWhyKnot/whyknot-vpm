# Notice:
I probably won't be publishing to this much anymore. As much as I would like to make my tools open source, it makes more sense to keep them private. I'm working on an avatar and gating these tools behind a license check would help prevent leaking of avatars while not preventing most editing. I have some seriously cool tools and if you ever want access to them, just purchasing one of my avatars will grant access. I hope to make it easy to transfer between projects outside of just what I create but this is why this repo is getting sort of abandoned.

# WhyKnot VPM listing

A [VRChat Package Manager](https://vcc.docs.vrchat.com/vpm/) listing for [WhyKnot](https://whyknot.dev)'s VRChat editor tools. Once it's added to the VRChat Creator Companion (VCC), the packages below appear in the Add Package dialog of every Unity project VCC manages.

Add to VCC: <https://vpm.whyknot.dev/>

Or paste `https://vpm.whyknot.dev/index.json` into VCC's Add Repository dialog.

## Packages

| Package | ID | Source repo |
|---|---|---|
| [VRCFury QoL](https://github.com/RealWhyKnot/wk-vrcfury-qol) | `dev.whyknot.wk-vrcfury-qol` | [RealWhyKnot/wk-vrcfury-qol](https://github.com/RealWhyKnot/wk-vrcfury-qol) |

## Add to VCC

Open <https://vpm.whyknot.dev/>. It redirects to a `vcc://` URL that opens VCC with the listing filled in. Click "I Understand, Add Repository".

If VCC doesn't open (it isn't registered for `vcc://`, or the browser blocked the redirect):

1. Open the VRChat Creator Companion.
2. Go to Settings > Packages > Add Repository.
3. Paste `https://vpm.whyknot.dev/index.json` and click "I Understand, Add Repository".

The packages are then under Manage Project > Add Package in any Unity project VCC manages.

## How it works

This repo builds `index.json` and nothing else. Each package's zips stay on its source repo's GitHub Releases.

```
release.yml in source repo               this repo's build.yml             GitHub Pages
--------------------------                ---------------------             ------------
  on tag v*:
    build zip + package.json
    create GitHub Release  -----------+
    POST repository_dispatch          |
        rebuild-listing  ----------------+
                                      |  |
                                      |  v
                                      |  fetch all releases of every
                                      |  repo in source.json,
                                      |  download each release's
                                      |  package.json, rewrite url --> public/index.json
                                      |                                      |
                                      |                                      v
                                      |                            vpm.whyknot.dev/index.json
                                      |
                                      v
                              GitHub Releases keep
                              the zips on GH's CDN
```

The build runs on:
- `repository_dispatch: rebuild-listing`, which a source repo sends right after a release
- `workflow_dispatch`, for a manual rebuild from the Actions tab
- `schedule: cron '0 6 * * *'`, once a day in case a dispatch got lost
- any push to `main` that changes `source.json` or the workflow

## Adding a new package

1. The source repo needs a VPM `package.json` at its root and a `release.yml` like [wk-vrcfury-qol's](https://github.com/RealWhyKnot/wk-vrcfury-qol/blob/main/.github/workflows/release.yml).
2. Add `{ "repo": "<owner>/<repo>", "packageId": "<dev.whyknot.foo>" }` to the `sources` array in `source.json`. The `packageId` has to match the `name` in the source repo's `package.json`, and releases with any other name are skipped. A renamed package keeps its old zips on GitHub, and its old id stays out of VCC. Pushing to `main` rebuilds the listing.
3. Tag a release in the source repo. Its `release.yml` sends `repository_dispatch` here and the new release is in the listing about a minute later.

## License

GNU General Public License v3.0 or later, like the packages it lists. See [LICENSE](LICENSE).
