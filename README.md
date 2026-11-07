# Mandala Themes

The community theme catalog for [Mandala](https://github.com/MaktubCN/open-mana). Market reads this repository's [`catalog.json`](catalog.json) and downloads reviewed JSON packages from pinned GitHub commits.

## Create and share a theme

1. Open **Settings → General → Theme & appearance** in Mandala.
2. Select a light or dark palette, then use **Customize appearance** to adjust colors, fonts, spacing, and an optional background.
3. Select **Export** to save a `.mandala-theme.json` file. A package contains one light **or** dark palette; publish separate entries if you want to offer both.
4. Fork this repository, add the exported file to `packages/`, and open a PR using the supplied template. Maintainers review the package and publish its catalog entry.
5. Once listed, other users select **Market → Themes → Install → Apply theme**.

See [CONTRIBUTING.md](CONTRIBUTING.md) for validation commands, the two-step publication process, and updates. The [Community Paper package](packages/paper.mandala-theme.json) is a complete example.

Themes are data, not executable plugins: supported colors, built-in font choices, appearance options, and an optional embedded image. Arbitrary CSS, scripts, remote font downloads, and remote image URLs are not supported. Installation does not switch appearance; applying imports an editable local copy. Uninstalling the package leaves that applied copy in place.

Use a Mandala build that includes theme and language-pack support in Market. An older build labeled `0.1.1` may not include this feature. The example packages and catalog use the [MIT license](LICENSE); externally hosted packages must declare their own redistribution license.

Before launch, this repository is private. Market downloads require the catalog and pinned packages to be public. The linked Mandala source documentation requires access until Mandala is open-sourced.
