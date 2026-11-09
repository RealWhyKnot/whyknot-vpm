# Notice:

I probably won't be publishing to this much anymore. As much as I would like to make my tools open source, it makes more sense to keep them private. I'm working on an avatar and gating these tools behind a license check would help prevent leaking of avatars while not preventing most editing. I have some seriously cool tools and if you ever want access to them, just purchasing one of my avatars will grant access. I hope to make it easy to transfer between projects outside of just what I create but this is why this repo is getting sort of abandoned.

# WhyKnot VPM listing

A [VRChat Package Manager](https://vcc.docs.vrchat.com/vpm/) listing for [WhyKnot](https://whyknot.dev)'s VRChat editor tools. Once it's added to the VRChat Creator Companion (VCC), the packages below appear in the Add Package dialog of every Unity project VCC manages.

Add to VCC: <https://vpm.whyknot.dev/>

Or paste `https://vpm.whyknot.dev/index.json` into VCC's Add Repository dialog.

## Packages

| Package                                                      | ID                           | Source repo                                                                 |
| ------------------------------------------------------------ | ---------------------------- | --------------------------------------------------------------------------- |
| [VRCFury QoL](https://github.com/RealWhyKnot/wk-vrcfury-qol) | `dev.whyknot.wk-vrcfury-qol` | [RealWhyKnot/wk-vrcfury-qol](https://github.com/RealWhyKnot/wk-vrcfury-qol) |

## Add to VCC

Open <https://vpm.whyknot.dev/>. It redirects to a `vcc://` URL that opens VCC with the listing filled in. Click "I Understand, Add Repository".

If VCC doesn't open (it isn't registered for `vcc://`, or the browser blocked the redirect):

1. Open the VRChat Creator Companion.
2. Go to Settings > Packages > Add Repository.
3. Paste `https://vpm.whyknot.dev/index.json` and click "I Understand, Add Repository".

The packages are then under Manage Project > Add Package in any Unity project VCC manages.

## How it works

The listing is frozen. `site/` holds `index.json` and every release zip, and GitHub Pages serves that folder as it is. Nothing is fetched from the package repos any more.

The workflow checks each zip against the SHA-256 in `index.json` and publishes the folder when `site/` changes.

## License

GNU General Public License v3.0 or later, like the packages it lists. See [LICENSE](LICENSE).
