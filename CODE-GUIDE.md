# Brief Trainer: code guide

The game is one self-contained HTML file. It works locally without a server. Open `learning.html` in a text editor to read the commented version.

## HTML, CSS and JavaScript

HTML provides the letter, buttons, table container and address editor. CSS controls colors, spacing and responsive layout. JavaScript builds the address buttons, checks answers and saves edits in the browser.

## Restored v2 layout

The presentation, CSS and shelf-rendering function match the supplied `Brief-Trainer-v2.html`. Four columns stay side by side, each containing four shelf levels. Numbers appear on the shelf edge, with adjacent street labels underneath.

The original Zoom slider changes the table width; Fit table restores 100%. The later camera, pinch handlers and separate readable viewer have been removed. Current address data, saved edits and full-length sessions remain.

## Address records

Every object in `ORIGINAL` represents one slot:

```js
{ id: 's1r1p1', section: 1, row: 1, position: 1,
  street: 'Wallrißstraße', number: '43' }
```

Sections 1–4 run top to bottom. Rows 1–4 identify the physical table columns from left to right. `position` preserves the original order within that intersection. Numbers are strings so `33 St 1`, `114-116` and parcel suffixes remain intact.

Photo measurements remain in the address records for reference. The restored v2 renderer uses equal-width slots within each shelf band.

## Street labels

`printedGroups(members)` combines consecutive addresses with the same street within one section and row. It never combines non-neighbouring addresses or removes number buttons.

`streetLabel(street)` shortens display text, for example `Naaffgasse` becomes `Naaffg.`. Full names stay in the letter, editor, saved data and accessible labels. To change an abbreviation, edit the `special` mapping in that function. Do not shorten `number`.

## Game functions

| Function | Purpose |
| --- | --- |
| `renderShelf()` | Builds the fixed four columns and four levels, with shared photo-based number and street grids. |
| `numberParts(number)` | Keeps the building number and St/Pa details on separate lines. |
| `numberLabel(slot)` | Creates the large number label, or a question mark in memory mode. |
| `newRound()` | Shuffles every slot in the selected practice area once. |
| `nextQuestion()` | Displays the next letter and clears previous answer outlines. |
| `choose(slot, button)` | Checks the selected address and awards first-try points. |
| `hint()` | Outlines the matching address and scrolls it into view. |
| `finish()` | Shows the final score and saves the best result. |
| `valid(data)` | Validates saved or imported address records. |
| `persist()` | Saves address edits and best scores in local browser storage. |

## Session length

The question count is `queue.length`, rather than a fixed 20. A full-table session has 399 letters. Selecting a section uses its own slot count. Best scores are tracked separately for each section and practice mode.

## Original zoom controls

`setZoom(value)` changes the width of the entire shelf bank. `100` is its normal width; `180` means 180%. The workbench can scroll when zoomed in. Fit table restores 100% and returns to the left edge.

## Changes to try

Edit the CSS `.slot .label` and `.street-group` rules to change number and street-label appearance. The `renderShelf()` loops use Row for horizontal columns and Section for vertical levels.

Use Edit addresses to correct labels, and export a backup before larger changes. The learning copy uses a separate browser-storage key.
