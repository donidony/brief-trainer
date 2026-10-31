# Brief Trainer: code guide

The game is one self-contained HTML file. It works locally without a server. Open `learning.html` in a text editor to read the commented version.

## HTML, CSS and JavaScript

HTML provides the letter, buttons, table container and address editor. CSS controls colors, spacing and responsive layout. JavaScript builds the address buttons, checks answers and saves edits in the browser.

## Fixed table and zoom

The layout is one fixed scene, 8000 × 2680 logical pixels. It always contains four columns and four shelf levels. Screen size never stacks columns or wraps a sequence of numbers. Slot widths use the measured `photoStart` and `photoEnd` values. Adjacent matching street labels may share an abbreviated label, while every slot stays in its original position.

The scene is larger than a device screen. `Fit table` scales it down to an overview. `Read numbers` zooms to the first number labels. The + and − buttons and two-finger pinch change magnification; dragging moves the camera. At overview size a tap zooms closer, and a second tap on a readable number sorts the letter.

The camera uses a CSS transform on the whole scene. It never rebuilds or reorders the slots. Gesture movement and pinch release cannot submit an answer. Browser page zoom remains available outside the table.

## Address records

Every object in `ORIGINAL` represents one slot:

```js
{ id: 's1r1p1', section: 1, row: 1, position: 1,
  street: 'Wallrißstraße', number: '43' }
```

Sections 1–4 run top to bottom. Rows 1–4 identify the physical table columns from left to right. `position` preserves the original order within that intersection. Numbers are strings so `33 St 1`, `114-116` and parcel suffixes remain intact.

The original photo measurements determine the fixed slot widths. Edited labels and imported backups cannot override the original slot geometry.

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

## Camera functions

| Function | Purpose |
| --- | --- |
| `fitTable()` | Fits all four columns and four levels into the viewport. |
| `readNumbers()` | Starts a readable view at the first number labels. |
| `zoomAt(scale, x, y)` | Zooms around a screen point without moving the world point beneath it. |
| `resizeCamera()` | Adapts the view to a resized screen without rearranging the scene. |
| `slotAnchor(slot)` | Locates the original label position for hints. |
| `activateSlot(id, button)` | Zooms an overview tap or submits a readable tap. |
| `endPointer(event)` | Completes a tap, drag or pinch safely. |

## Small changes to try

Change `.building-number` to adjust the base number size. Change `.building-suffix` to adjust St/Pa details. Keep the scene geometry constants and corresponding CSS dimensions in sync. Do not add responsive column stacking or wrapped number groups if you want to preserve the physical table.

Use Edit addresses for corrections. Export a backup before trying larger changes. The learning copy has a separate storage key.
