# Ai Sales Agent (ASA) – WordPress.org SVN Release Guide

This guide documents only the steps required to publish and manage releases on the WordPress.org Plugin Directory via Subversion (SVN).

## Repository
- Public page: https://wordpress.org/plugins/asa-ai-sales-agent
- SVN URL: https://plugins.svn.wordpress.org/asa-ai-sales-agent
- Your SVN username is your WordPress.org username (case-sensitive).
- Generate/manage your SVN password in your WordPress.org profile (Account & Security → SVN credentials). Never store credentials in the repo.

## SVN Directory Layout
- `trunk/` – The current development/working copy shown to users when Stable tag points to a specific tag. Keep it always releasable.
- `tags/<version>/` – Frozen copy of each release (e.g., `tags/1.0.8`). Users get this when your readme Stable tag matches the tag.
- `assets/` – Plugin Directory listing assets (banner, icons, screenshots) at the repository root (not inside trunk). These are not shipped with the plugin zip.

## Before You Release
1) Bump versions
- `asa-ai-sales-agent.php` → `Version: X.Y.Z`
- `trunk/readme.txt` → `Stable tag: X.Y.Z`

2) Validate readme
- Use the Readme Validator: https://wordpress.org/plugins/developers/readme-validator/

3) Clean the tree
- Remove build artifacts and large files from `trunk/` (e.g., `*.zip`, temporary reports). Only ship what the plugin needs to run.

4) Prepare listing assets (optional but recommended)
- Place these in the repository root `assets/` (not in `trunk/`):
  - Banners: `banner-772x250.(png|jpg)` and `banner-1544x500.(png|jpg)`
  - Icons: `icon-128x128.(png|jpg|gif)`, `icon-256x256.(png|jpg|gif)` (SVG + PNG fallback optional)
  - Screenshots: `screenshot-1.png`, `screenshot-2.png`, ...
- Docs: https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/

## First-Time Checkout
```bash
svn co https://plugins.svn.wordpress.org/asa-ai-sales-agent asa-svn
cd asa-svn
```

## Release Workflow (for every new version X.Y.Z)
1) Copy plugin files into `trunk/` (copy the CONTENTS of your plugin folder, not the parent folder itself)
```bash
# from your local workspace (adjust the path)
rsync -av --delete /path/to/asa-ai-sales-agent/ ./trunk/

# remove files you do not want to ship
find ./trunk -maxdepth 1 -type f \( -name "*.zip" -o -name "report.md" \) -delete
```

2) Update listing assets in repository root (if changed)
```bash
mkdir -p ./assets
cp -f ./trunk/assets/banner-772x250.png ./assets/ 2>/dev/null || true
cp -f ./trunk/assets/banner-1544x500.png ./assets/ 2>/dev/null || true
cp -f ./trunk/assets/icon-128x128.png ./assets/ 2>/dev/null || true
cp -f ./trunk/assets/icon-256x256.png ./assets/ 2>/dev/null || true
cp -f ./trunk/assets/screenshot-*.png ./assets/ 2>/dev/null || true
```

3) Add and commit `trunk/` (and `assets/` if changed)
```bash
svn add --force trunk assets
svn status
svn commit -m "Release X.Y.Z to trunk"
```

4) Create the tag from `trunk` and commit
```bash
svn copy trunk tags/X.Y.Z
svn commit -m "Tag X.Y.Z"
```

Notes
- The `Stable tag` in `readme.txt` must match the tag you created (e.g., `1.0.9`).
- Avoid using `Stable tag: trunk`. Always tag releases.
- Directory listing updates (banners/screenshots) may take time due to CDN caching.

## Verifications After Commit
- Check public page displays the correct version/changelog.
- Download the plugin from WordPress.org and smoke test the install.
- If assets were updated, allow a few hours for cache refresh.

## Troubleshooting
- Authentication: Use your WordPress.org username (case-sensitive) and your generated SVN password (not your site login password).
- Accidental empty tag: remove it and re-tag correctly:
```bash
svn delete tags/X.Y.Z
svn commit -m "Remove incorrect tag X.Y.Z"
svn copy trunk tags/X.Y.Z
svn commit -m "Tag X.Y.Z"
```
- Readme not updating: validate readme and ensure it is in `trunk/` and in the tag.
- Large files rejected: do not commit build zips or unnecessary binaries.

## References
- Using Subversion with the Plugin Directory: https://developer.wordpress.org/plugins/wordpress-org/how-to-use-subversion/
- Readme standard: https://wordpress.org/plugins/developers/#readme
- Readme validator: https://wordpress.org/plugins/developers/readme-validator/
- Plugin assets: https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/
- Detailed guidelines: https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/