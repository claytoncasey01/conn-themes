# conn-themes

Community themes for [conn](https://github.com/claytoncasey01/conn), the dashboard for Claude Code sessions. conn's Themes page reads this repo's `index.json` and installs a theme from the commit the index pins it at.

## What is here

```
index.json          the list conn reads: made by a script, never edited by hand
<id>/theme.json     one folder per theme; the folder name is the theme's id
.gitattributes      `*.json -text`: see below
README.md
```

A theme is `theme.json` alone: colors, type, shape and named effects. No CSS, no images, no fonts (it can name the fonts conn already bundles). conn installs the file only if it hashes to the value in `index.json` and passes the same checks as a theme in `~/.config/conn/themes/`, contrast included.

## Add a theme

1. Fork this repo and add `<id>/theme.json`. Start from `themes/_template/` in the conn repo; [docs/themes.md](https://github.com/claytoncasey01/conn/blob/main/docs/themes.md) lists every token.
2. From a conn checkout run `conn theme check --gallery ./path/to/<id>` until it prints `<id>: ok`.
3. Open a pull request with the one new folder. Do not touch `index.json`; the maintainer rebuilds it. [CONTRIBUTING.md](CONTRIBUTING.md) has the license and review terms.

Rules the build enforces, so a pull request that breaks one will not be listed:

- the folder name is the `id` in the file (`a-z`, `0-9` and `-`, up to 40 characters), and it is not the id of a conn built-in (today `default`, `neon`, `matrix` and `crt`)
- `theme.json` is the only file in the folder: no `theme.css`, no dotfiles
- every folder at the root is a theme folder (only dot folders such as `.github` are skipped); loose files like this README are not themes
- UTF-8 without a byte order mark, at most 64 KB
- it passes `conn theme check --gallery`, which applies all of the above and every contrast pair, no waiver
- `version` goes up when a change should reach people who installed it as an update

## For the maintainer

`.gitattributes` marks `*.json` as `-text` on purpose: the index pins the sha256 of each file's exact bytes, so git must never convert its line endings (autocrlf on a contributor's machine would otherwise make an installed file hash differently from the index). Keep it.

Merging a theme rebuilds the index: [`.github/workflows/index.yml`](.github/workflows/index.yml) runs on every push to `main` that changes a `theme.json`, builds `conn theme index` from conn's `main`, and commits `index.json` to `main` as `github-actions[bot]`. It pushes with the deploy key in the `INDEX_DEPLOY_KEY` secret: deploy keys are the one bypass the `main` ruleset allows besides the owner, so keep that key the repo's only one with write access. If the run fails, a theme failed conn's checks (the log names it) or `main` moved during the run; fix the theme, or re-run it from the Actions tab (it also has a manual trigger).

To build the index by hand, from a conn checkout, with this repo checked out at `main` and its files exactly as committed (no line-ending conversion):

```sh
conn theme index --src ../conn-themes --out ../conn-themes-out --commit "$(git -C ../conn-themes rev-parse HEAD)" --repo claytoncasey01/conn-themes
```

Then copy `../conn-themes-out/index.json` over this repo's `index.json`. conn reads `https://raw.githubusercontent.com/claytoncasey01/conn-themes/main/index.json` and, for an install, `.../<commit>/<id>/theme.json`.

## License

[Apache-2.0](LICENSE), the same as conn.
