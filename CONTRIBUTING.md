# Contributing a theme

Thank you for adding a theme. The steps, and the rules a theme has to follow to be listed, are in
the [README](README.md#add-a-theme): one new folder, `<id>/theme.json`, that passes
`conn theme check --gallery`. This page covers what the README does not.

- **One theme per pull request**, and only that theme's folder. Do not touch `index.json`: the
  maintainer rebuilds it after the merge, which is also when the theme becomes installable.
- **It has to be yours to give.** Use only colors, shapes and fonts you made or have the right to
  share. A theme can name the fonts conn already bundles and nothing else, so no font file comes
  with it.
- **License.** The repository is under the [Apache License 2.0](LICENSE). By sending a theme you
  agree it is licensed under the same terms (section 5 of the license). There is nothing else to
  sign.
- **Updating your theme.** Raise `version` in `theme.json`, or people who installed it will not
  be offered the change.
- **Review.** The maintainer checks that the theme passes the checker, reads well in light and
  dark where it has both, and does not copy another theme or a brand. A theme can be unlisted
  later if it is found to break these rules.

Problems with conn itself, or with the checker, go to
[conn's issues](https://github.com/claytoncasey01/conn/issues).
