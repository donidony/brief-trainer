# Brief Trainer

A browser game for practicing the address positions on a letter-sorting table. The current layout contains 395 slots. All four columns stay visible together, with addresses underneath the dividers.

## Play

Open `index.html` locally, or visit the repository's GitHub Pages website after publishing. Choose practice mode to see labels or memory mode to hide them. Each round contains 20 letters.

This table uses the original document's numbering: **Section 1 is the top level; Section 4 is the bottom. Row 1 is the far-left column; Row 4 is the far-right column.** Slots run from left to right inside each intersection.

## Files

- `index.html` — the playable game, including its CSS, JavaScript and addresses.
- `learning.html` — a playable copy with readable formatting and code comments.
- `CODE-GUIDE.md` — a beginner's explanation and small exercises.
- `.nojekyll` — serves these static files without a Jekyll build.

## GitHub Pages setup

In the repository, open **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, select **main** and **/(root)**, then save. The Pages settings will provide the website's address after deployment.

## Address edits and scores

The game stores edits and best scores in the current browser. They do not automatically sync between devices. Use **Edit addresses → Export backup** and import that backup on another device to transfer corrected labels. Local-file storage and website storage are separate; export existing corrections before switching to the hosted site.

## Learn and modify

Read `CODE-GUIDE.md` and open `learning.html` in a text editor. No build tools or external libraries are required. The learning copy has a separate browser-storage entry and a `ROUND_SIZE` setting.
