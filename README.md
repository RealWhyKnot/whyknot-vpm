# WhyKnot VPM Listing

A [VRChat Package Manager](https://vcc.docs.vrchat.com/vpm/) listing for [WhyKnot](https://whyknot.dev)'s VRChat editor tools. Add this listing to the VRChat Creator Companion (VCC) and the packages below appear in the Add-Package dialog of every Unity project you manage with VCC.

**Listing URL:** `https://vpm.whyknot.dev/index.json`

## Packages

| Package | ID | Source repo |
|---|---|---|
| [Avatar QoL](https://github.com/RealWhyKnot/vrc-avatar-qol) | `dev.whyknot.avatar-qol` | [RealWhyKnot/vrc-avatar-qol](https://github.com/RealWhyKnot/vrc-avatar-qol) |
| [VRCFury QoL](https://github.com/RealWhyKnot/vrcfury-qol) | `dev.whyknot.vrcfury-qol` | [RealWhyKnot/vrcfury-qol](https://github.com/RealWhyKnot/vrcfury-qol) |

## Add to VCC

1. Open the VRChat Creator Companion.
2. Go to **Settings → Packages → Add Repository**.
3. Paste `https://vpm.whyknot.dev/index.json` and click **I Understand, Add Repository**.
4. Open any Unity project managed by VCC. The packages above are now available under **Manage Project → Add Package**.

You can also click [this `vcc://` link](vcc://vpm/addRepo?url=https://vpm.whyknot.dev/index.json) (works if VCC is installed and registered as the handler).

## How it works

This repo is purely a **listing**. It hosts no zips of its own:

```
release.yml in source repo                this repo's build.yml                GitHub Pages
─────────────────────────                 ─────────────────────                ─────────────
  on tag v*:
    build zip + package.json
    create GitHub Release  ──────────┐
    POST repository_dispatch         │
        rebuild-listing  ───────────────┐
                                     │  │
                                     │  ▼
                                     │  fetch all releases of every
                                     │  repo in source.json
                                     │  download each release's
                                     │  package.json, rewrite url ──→  public/index.json
                                     │                                     │
                                     │                                     ▼
                                     │                              vpm.whyknot.dev/index.json
                                     │
                                     ▼
                              GitHub Releases keep
                              the zips on GH's CDN
```

The build runs on:
- `repository_dispatch: rebuild-listing` — fired by the source repos right after they cut a release.
- `workflow_dispatch` — manual rebuild from the Actions tab.
- `schedule: cron '0 6 * * *'` — daily safety net.
- `push to main` whose paths touch `source.json` or the workflow itself — for adding a new package.

## Adding a new package

1. The source repo needs a `package.json` at root (VPM manifest) and `release.yml` matching the pattern in [vrc-avatar-qol/.github/workflows/release.yml](https://github.com/RealWhyKnot/vrc-avatar-qol/blob/main/.github/workflows/release.yml).
2. Append `"<owner>/<repo>"` to `source.json`'s `sources` array. Push to `main` — the `paths:` filter on the build workflow will trigger a rebuild.
3. Tag a release in the source repo. Its `release.yml` posts `repository_dispatch` here, which kicks the build a second time so the new release is in the listing within ~1 minute.

## License

MIT — same as the packages themselves.
