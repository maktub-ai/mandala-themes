# Contributing themes

## Create and preview

Export a theme from **Settings → General → Theme & appearance**, or copy [`packages/paper.mandala-theme.json`](packages/paper.mandala-theme.json). Import your file through the same page's **Import** button to preview it before submission. Save/export your previous theme first, or use a separate test profile.

Keep the top-level `format: "mandala-theme"`, `version: 1`, `theme`, `appearance`, and `background` fields. Use a descriptive theme ID and name. `theme.variant` is `light` or `dark`. Colors use six-digit hex values (`#RRGGBB`). Exporting from the App is the simplest way to produce supported font choices and valid numeric ranges.

A background must be empty or an embedded PNG, JPEG, or WebP data URL. The App's image limit is 2 MiB and the whole package limit is 4 MiB. Background images are included in exports: only use images you have permission to distribute. Do not include private pictures or personal data.

Check text readability, buttons, chat/code content, and a narrow window. Attach screenshots with synthetic content to the PR if possible. Do not claim a dark variant when only a light palette has been tested.

## Submit a package

1. Fork this repository on GitHub and create a branch.
2. Add your JSON file under `packages/` (or update the existing file for a new version).
3. Follow the validation and preview steps below.
4. Open a pull request to this repository's `main` branch. The PR template asks for the package ID, display name, English description, publisher, release version, minimum App version, license, and validation results.
5. A maintainer reviews the file, merges it, and publishes a separate catalog change. **A merged package PR alone does not make an item appear in Market.**

For a first contribution, you only need to submit the package and fill in the PR template. Leave `catalog.json` to the maintainer: the final package commit does not exist until your PR is merged. Squash and rebase merges can change commit IDs.

An alternative is to keep the package in your own public GitHub repository. Submit a catalog-only PR here with the existing full commit SHA, file path, SHA-256, and a link to its license. The package must remain publicly downloadable at that commit. Market discovers only entries in this repository's catalog; it does not scan arbitrary GitHub repositories.

Use English for documentation, PR titles and descriptions, commit messages, and catalog descriptions. Localized language names, translation keys, and translated values are intentional exceptions.

## Validate with Mandala's parser

Use a local checkout of [open-mana](https://github.com/MaktubCN/open-mana), with its dependencies installed (`npm ci`) and the Node version required by its `package.json`. Run the following **from the open-mana checkout**, replacing the absolute path:

```sh
PACK_theme="theme" PACK_FILE="/absolute/path/to/paper.mandala-theme.json" node --import tsx --input-type=module <<'JS'
import { readFileSync } from 'node:fs';
import { parseExtensionData } from './shared/ui-extensions.ts';
parseExtensionData(readFileSync(process.env.PACK_FILE, 'utf8'), process.env.PACK_theme);
console.log('Package format is valid.');
JS
```

This checks the actual App format. It does not establish translation accuracy, visual quality, licensing, or successful installation. Include your validation result and the App build used in the PR.

## Maintainer publication

Publication uses two commits: the package commit first, then a catalog commit that references it. No release archive, hosted website, or cloud workflow is needed.

1. Review the package, license, and PR evidence. Check for duplicate IDs; for languages, prefer improving an existing locale instead of introducing a second pack for the same locale (both cannot be installed together).
2. Merge the package PR. Fetch the resulting `main` and select the actual commit containing the reviewed bytes, rather than the contributor's pre-merge SHA.
3. Derive the hash from those committed bytes. Example, run in this repository:

   ```sh
   git fetch origin main
   PACKAGE_REVISION=$(git rev-parse origin/main)
   PACKAGE_PATH="packages/paper.mandala-theme.json"
   git show "$PACKAGE_REVISION:$PACKAGE_PATH" | shasum -a 256
   ```

4. Add or update an item in `catalog.json`. Copy the existing item as a structural example and replace these fields:

   | Field | Meaning |
   | --- | --- |
   | `id` | Stable unique ID, 1–60 lowercase letters, digits, or hyphens; start with a letter or digit. |
   | `name` | Display name, at most 100 characters. |
   | `description` | English description, at most 2,000 characters; disclose incomplete translations. |
   | `publisher` | Author or organization, at most 100 characters. |
   | `version` | Package release version, such as `1.0.0`; increase for a new release. |
   | `minAppVersion` | Minimum supported App version in `x.y.z` form. |
   | `package.repository` | Public `owner/repository` containing the file. |
   | `package.revision` | Existing full 40-character lowercase commit SHA; never `main`, a tag, or a future SHA. |
   | `package.path` | Repository-relative JSON path, such as `packages/paper.mandala-theme.json`. |
   | `package.sha256` | 64-character lowercase SHA-256 of the exact file bytes at that commit. |

   Keep the catalog's `format`, schema `version`, and `kind` unchanged. The catalog entry's release `version` is separate from the package's schema `version: 1`.

5. Validate the candidate catalog with `parseExtensionCatalog` from `shared/ui-extensions.ts`; fetch the pinned public file and check its digest and `parseExtensionData` result. Inspect the diff and publish the catalog change to `main`.
6. In a separate test profile, browse the published item in Market, install it, explicitly apply it, restart Mandala, and confirm it still works. Record the result; investigate or revert a faulty catalog entry. Never use personal screenshots or private backgrounds as public PR evidence.

The App rejects malformed packages, checksum mismatches, and unsupported minimum versions. Each package is limited to 4 MiB; each catalog to 500 entries. Admission to the catalog is not an additional guarantee about a third-party author's identity or work.

## Updates and discovery

Keep the same catalog ID for updates. Commit the new package first, then raise its release version and update the pinned revision and hash in a catalog PR. Do not change the bytes behind a release by reusing its version.

Once the catalog change reaches `main`, users can reopen Market, enable `Mandala Themes` in **Manage sources** if needed, and find the item under **Themes**. Open its details, select **Install**, then **Apply theme**. Installation alone does not change preferences. Existing users can reopen the item from **Browse** to check for an **Update**; updates are not installed automatically. Network access to GitHub is required for discovery and installation. Installed packages remain usable without GitHub access while the local Mandala service is running.
