# Repository instructions

- Use English for all source code, identifiers, comments, user-facing strings, error messages, documentation, examples, release notes, and commit messages.
- Keep `README.md` entirely in English.
- Never introduce German text into code or documentation.
- Keep the card dependency-free and edit `ha_power-energy-mix-card.js` directly; do not add a build step unless the project requirements materially change.
- Run `npm run check` before committing.
- Keep one option order everywhere: `title`, `collection_key`, `default_mode`, `show_card`, then `tap_action`, `hold_action`, `double_tap_action`. The visual editor schema, the YAML examples and the options table in `README.md` all follow it, and a test compares the editor against that table.

## Where this card lives

- The repository is developed on `https://git.itup.ch/mme/ha_power-energy-mix-card`. `https://github.com/psym88/ha_power-energy-mix-card` is the published copy HACS reads.
- Publishing is a release on Forgejo: `.forgejo/workflows/sync-github.yml` then pushes the branch, every tag, the release and `ha_power-energy-mix-card.js` as that release's asset to GitHub. A manual run of the same workflow without a tag only syncs branch and tags.
- GitHub gets only the file Home Assistant installs and the page a visitor reads: the branch and every new tag are published from the include list `PUBLISH_INCLUDE` in the workflow, `hacs.json README.md LICENSE images`, plus the file `hacs.json` names. A new file here is not on GitHub until it is named on that list, and `AGENTS.md`, `scripts/`, `tests/` and the package manager files are not. The branch tip on GitHub is a commit of its own with the commit here as its parent, so its sha is not the sha here, and that is the design rather than a mistake. Older tags keep whatever they were published with: they never move.
- Never commit to the GitHub repository, and never move or delete a tag or a release asset that is already on GitHub: HACS installs from them, and the download count hangs off the asset, so replacing it starts that count at zero.
- Keep the version in `package.json` and the release tag in step with each other, and keep `filename` in `hacs.json` pointing at the file that is the release asset.
- Creating a release fires the sync right away, draft or not: a Forgejo `release` event arrives when the release is created, so a draft alone does not keep a tag off GitHub. `sync-github.yml` now skips its job while `github.event.release.draft` is set, and publishing a draft is still what mirrors its tag, release and asset to GitHub for good.
