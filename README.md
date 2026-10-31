# Brief Trainer

A browser game for practicing the address positions on a letter-sorting table. The current layout contains 399 slots. All four columns stay together, with addresses underneath the dividers. The photo-based layout preserves 147 separate printed street-label blocks, including repeated names. Names wrap in full instead of being shortened with ellipses; narrower screens can scroll across the table.

## Play

Open `index.html` locally, or visit the repository's GitHub Pages website after publishing. Choose practice mode to see labels or memory mode to hide them. Each round contains every slot in the selected area once: 399 letters for the full table.

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

Read `CODE-GUIDE.md` and open `learning.html` in a text editor. No build tools or external libraries are required. The learning copy has a separate browser-storage entry and the same automatic session length.

## Photo label layout

Each address record includes `labelGroup` and `labelText`. These preserve the real printed boundaries instead of merging adjacent matching street names. For example, Sonnenbadweg is repeated over separate blocks in Section 4 Row 1. The photo layout uses a new storage entry so the earlier slot list cannot shift the new labels. Earlier browser data is retained under its previous entry. Incomplete photographed parcel and Stiege suffixes remain as visible; no hidden numbers are invented.

READABLE VIEW
The overview keeps the entire table visible. Move the mouse across it, or drag a finger across it on a phone, to inspect neighbouring slots in the readable strip. Tap a large slot in the strip to sort a letter. The arrow buttons move along the same shelf. Phone touches on the overview inspect only, so they do not accidentally sort letters. Memory mode hides labels in both views.
