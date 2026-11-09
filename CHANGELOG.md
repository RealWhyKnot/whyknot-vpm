# Changelog

All notable user-visible changes to whyknot-vpm are tracked here.

## Unreleased

### Changed

- **listing:** Froze the listing. `site/` now holds `index.json` and every release zip, and nothing is fetched from the package repos.

### Removed

- **listing:** Dropped `dev.whyknot.wk-vrc-qol` from the listing; the package is unmaintained. Existing installs keep resolving their zips from the source repo's GitHub releases.
