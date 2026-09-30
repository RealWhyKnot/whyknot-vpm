# Notice:
I probably won't be publishing to this much anymore. As much as I would like to make my tools open source, it makes more sense to keep them private. I'm working on an avatar and gating these tools behind a license check would help prevent leaking of avatars while not preventing most editing. I have some seriously cool tools and if you ever want access to them, just purchasing one of my avatars will grant access. I hope to make it easy to transfer between projects outside of just what I create but this is why this repo is getting sort of abandoned.

# WhyKnot VPM listing

A [VRChat Package Manager](https://vcc.docs.vrchat.com/vpm/) listing for [WhyKnot](https://whyknot.dev)'s VRChat editor tools. Add it to the VRChat Creator Companion (VCC) and the packages below show up in the Add Package dialog of every Unity project VCC manages.

Add to VCC: <https://vpm.whyknot.dev/>

Or paste `https://vpm.whyknot.dev/index.json` into VCC's Add Repository dialog.

## Packages

| Package | ID | Source repo |
|---|---|---|
| [VRCFury QoL](https://github.com/RealWhyKnot/wk-vrcfury-qol) | `dev.whyknot.wk-vrcfury-qol` | [RealWhyKnot/wk-vrcfury-qol](https://github.com/RealWhyKnot/wk-vrcfury-qol) |

## Add to VCC

Open <https://vpm.whyknot.dev/>. It redirects to a `vcc://` URL, VCC opens with the listing filled in, and you click **I Understand, Add Repository**.

If that doesn't work (VCC isn't registered as the `vcc://` handler, or the browser blocks the redirect):

1. Open the VRChat Creator Companion.
2. Go to **Settings -> Packages -> Add Repository**.
3. Paste `https://vpm.whyknot.dev/index.json` and click **I Understand, Add Repository**.

Then open a Unity project managed by VCC. The packages above are under **Manage Project -> Add Package**.

## How it works

This repo is only a listing. It hosts no zips of its own:

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
- `repository_dispatch: rebuild-listing`, sent by the source repos right after they cut a release.
- `workflow_dispatch`, for a manual rebuild from the Actions tab.
- `schedule: cron '0 6 * * *'`, a daily fallback.
- A push to `main` that touches `source.json` or the workflow, for adding a new package.

## Adding a new package

1. The source repo needs a `package.json` at root (VPM manifest) and `release.yml` matching the pattern in [wk-vrcfury-qol/.github/workflows/release.yml](https://github.com/RealWhyKnot/wk-vrcfury-qol/blob/main/.github/workflows/release.yml).
2. Append `{ "repo": "<owner>/<repo>", "packageId": "<dev.whyknot.foo>" }` to `source.json`'s `sources` array. The `packageId` is the expected `name` field in the source repo's `package.json`. Releases whose `package.json` declares a different name are skipped, which is how a renamed package keeps its old zips on GitHub without bringing the old id back into VCC. Push to `main` and the `paths:` filter on the build workflow triggers a rebuild.
3. Tag a release in the source repo. Its `release.yml` posts `repository_dispatch` here, which runs the build again, and the new release is in the listing within about a minute.

## License

GNU General Public License v3.0 or later, the same as the packages it lists. See [LICENSE](LICENSE).
