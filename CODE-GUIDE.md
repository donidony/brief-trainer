# Brief Trainer: code guide

The game is one self-contained HTML file. It works locally without a server. Open `learning.html` in a text editor to read the commented version.

## HTML, CSS and JavaScript

HTML provides the letter, buttons, table container and address editor. CSS controls colors, spacing and responsive layout. JavaScript builds the address buttons, checks answers and saves edits in the browser.

## Readable numbers

Each address has its own button. The main building number uses 18px type; long ranges use 15px. St/Ha/Pa details use a separate 12px line. Buttons remain at least 68px wide and 54px tall. Street labels are below their number groups.

The main table uses four columns above 1199px, two columns at tablet widths, and one column at 620px or below. On smaller devices, scroll vertically through the table. There is no magnifying viewer or forced horizontal panning.

## Address records

Every object in `ORIGINAL` represents one slot:

```js
{ id: 's1r1p1', section: 1, row: 1, position: 1,
  street: 'Wallrißstraße', number: '43' }
```

Sections 1–4 run top to bottom. Rows 1–4 identify the physical table columns from left to right. `position` preserves the original order within that intersection. Numbers are strings so `33 St 1`, `114-116` and parcel suffixes remain intact.

The original photo grouping and measurements remain in the data for reference. They do not determine the current responsive widths.

## Street labels

`printedGroups(members)` combines consecutive addresses with the same street within one section and row. It never combines non-neighbouring addresses or removes number buttons.

`streetLabel(street)` shortens display text, for example `Naaffgasse` becomes `Naaffg.`. Full names stay in the letter, editor, saved data and accessible labels. To change an abbreviation, edit the `special` mapping in that function. Do not shorten `number`.

## Game functions

| Function | Purpose |
| --- | --- |
| `renderShelf()` | Builds four table columns with four sections each, street groups and number buttons. |
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

## Small changes to try

Change `.building-number` to adjust the main number size. Change `.building-suffix` to adjust St/Pa details. Change the two media-query breakpoints to alter when columns stack. Keep the minimum button width large enough for the longest number range.

Use Edit addresses for corrections. Export a backup before trying larger changes. The learning copy uses its own browser-storage entry, so it does not replace saved edits in the normal game.
