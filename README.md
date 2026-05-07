# WhyKnot VPM Listing

A [VRChat Package Manager](https://vcc.docs.vrchat.com/vpm/) listing for [WhyKnot](https://whyknot.dev)'s VRChat editor tools. Add this listing to the VRChat Creator Companion (VCC) and the packages below appear in the Add-Package dialog of every Unity project you manage with VCC.

**Click to add to VCC:** <https://vpm.whyknot.dev/>
**Manual entry URL** (paste into VCC's Add Repository dialog): `https://vpm.whyknot.dev/index.json`

## Packages

| Package | ID | Source repo |
|---|---|---|
| [Avatar QoL](https://github.com/RealWhyKnot/vrc-avatar-qol) | `dev.whyknot.avatar-qol` | [RealWhyKnot/vrc-avatar-qol](https://github.com/RealWhyKnot/vrc-avatar-qol) |
| [VRCFury QoL](https://github.com/RealWhyKnot/vrcfury-qol) | `dev.whyknot.vrcfury-qol` | [RealWhyKnot/vrcfury-qol](https://github.com/RealWhyKnot/vrcfury-qol) |

## Add to VCC

The fast path: click <https://vpm.whyknot.dev/>. The page redirects to a `vcc://` handler URL, VCC opens with the listing pre-filled, click **I Understand, Add Repository**.

If the click path doesn't work (VCC not registered as the `vcc://` handler, browser blocks the redirect, etc.):

1. Open the VRChat Creator Companion.
2. Go to **Settings -> Packages -> Add Repository**.
3. Paste `https://vpm.whyknot.dev/index.json` and click **I Understand, Add Repository**.

After either path, open any Unity project managed by VCC. The packages above are now available under **Manage Project -> Add Package**.

## How it works

This repo is purely a **listing**. It hosts no zips of its own:

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
- `repository_dispatch: rebuild-listing` -- fired by the source repos right after they cut a release.
- `workflow_dispatch` -- manual rebuild from the Actions tab.
- `schedule: cron '0 6 * * *'` -- daily safety net.
- `push to main` whose paths touch `source.json` or the workflow itself -- for adding a new package.

## Adding a new package

1. The source repo needs a `package.json` at root (VPM manifest) and `release.yml` matching the pattern in [vrc-avatar-qol/.github/workflows/release.yml](https://github.com/RealWhyKnot/vrc-avatar-qol/blob/main/.github/workflows/release.yml).
2. Append `"<owner>/<repo>"` to `source.json`'s `sources` array. Push to `main` -- the `paths:` filter on the build workflow will trigger a rebuild.
3. Tag a release in the source repo. Its `release.yml` posts `repository_dispatch` here, which kicks the build a second time so the new release is in the listing within ~1 minute.

## License

Licensed under the GNU General Public License v3.0 or later. See [LICENSE](LICENSE) for the full text. Same license as the packages it lists.
