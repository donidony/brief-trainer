# Learn your Brief Trainer game

Open **learning.html** in a browser to play. Open the same file in a plain-text code editor to read or change it. This copy uses your current address list and corrected table layout. Its code contains explanatory comments.

Make a copy before experimenting. Save your changes, then refresh the browser to see the result. You do not need a server, an installation step or an internet connection to run the game.

## Three languages in one file

| Part | What it does | Where to look |
| --- | --- | --- |
| HTML | Creates the heading, buttons, dropdowns, letter and empty shelf container | Inside `<body>`, before `<script>` |
| CSS | Controls colors, sizes, spacing, dividers and the shelf layout | Between `<style>` and `</style>` |
| JavaScript | Chooses letters, builds the shelves, checks answers and saves edits | Between `<script>` and `</script>` |

The file extension is `.html`, but the file contains all three languages. There are no external libraries or image downloads.

Comments are notes the browser does not display:

```html
<!-- An HTML comment -->
```

```css
/* A CSS comment */
```

```js
// A JavaScript comment on one line
/* A JavaScript comment that can span several lines */
```

## Start with these small changes

### Change the heading

Search for `Know every letter’s place.` in the HTML. Replace only the words between the opening and closing `<h1>` tags. Save and refresh.

The browser tab title is separate: change the words inside `<title>` near the top of the file.

### Change a color

Near the start of the CSS, find `:root`. It defines reusable colors:

```css
--green: #254f3e;
--yellow: #f4c54c;
```

Try changing `--green` to `#315f85`. This changes controls that use `var(--green)`. A value such as `#315f85` is a hexadecimal color code.

The divider colors come from a separate array inside `renderShelf()`:

```js
['#e8b83c', '#28725c', '#284b72', '#e8b83c'][r - 1]
```

These four entries are the colors for the four levels, from top to bottom. `r - 1` converts level numbers 1–4 into array positions 0–3.

### Session length follows the addresses

`newRound()` shuffles all slots in the selected area and assigns them to `queue`. The session length is `queue.length`: 399 letters for the full table, or the number of slots in a selected section. Each slot appears once. Progress, final scores and completion all use that length. Best scores are stored separately for each area and practice mode.

### Make the dividers taller

Find `.cabinet` in the CSS and change:

```css
--pocket-height: 83px;
```

Try `95px`. The shelf grid calculates each level's height from this value, keeping levels aligned. A later media rule uses `105px` on screens at least 1700 pixels wide; change that rule too if you want the same height on very wide screens.

## Your address data

Search for **ADDRESS DATA APPENDIX** near the end of the file. `originalAddresses()` returns the built-in list. Each object describes one slot:

```js
{
  "id": "s1r1p1",
  "section": 1,
  "row": 1,
  "position": 1,
  "street": "Wallrißstraße",
  "number": "43"
}
```

| Field | Meaning |
| --- | --- |
| `section` | Your vertical level: 1 is top, 4 is bottom |
| `row` | Your horizontal column: 1 is far left, 4 is far right |
| `position` | The slot number from left to right inside that level and column |
| `id` | A unique identity combining section, row and position |
| `street` | The street label |
| `number` | The building label, stored as text; obscured suffixes are kept as visible in the photo |
| `labelGroup` | Identifies the separate printed block on the real shelf |
| `labelText` | Street text displayed underneath that printed block |

`"33 St 1"` is one `number` value and one slot. It stays distinct from `"33 St 2"`. Labels such as `"6 Pa"`, `"103b"` and `"9-11"` also remain intact. The game does not expand a hyphenated label into multiple slots.

Objects are separated with commas. Keep quotation marks around street names and building labels. Keep the records for each intersection in slot order: the renderer uses their order in the list.

For small label corrections, the game's **Edit addresses** button is easier than editing the source. To add a physical slot in code, add a complete record with a unique ID and the next position in that intersection, in the correct order. The editor itself changes labels only; it does not add or move slots. Backups must match the current built-in layout to pass validation.

## How the game runs

1. The browser creates the HTML controls and applies the CSS.
2. JavaScript loads the address list and restores compatible saved edits.
3. `newRound()` filters the training pool, shuffles it and selects the round's letters.
4. `nextQuestion()` displays a letter and calls `renderShelf()`.
5. Clicking a slot calls `choose()`. It compares both street and building label.
6. Next advances to another letter. After the last letter, `finish()` shows the result.

`originalAddresses()` is declared at the end, but JavaScript allows a function declaration to be called earlier in the same script. This is called **hoisting**.

## Find your way around the functions

| Name | Responsibility |
| --- | --- |
| `valid(data)` | Checks saved/imported addresses against this table's identities and layout |
| `persist()` | Saves labels and the best score in browser storage |
| `key(s)` | Produces a comparable street-and-number string, ignoring case and extra spaces |
| `el(tag, text, cls)` | Creates a page element, with optional text and CSS class |
| `allMatches()` | Finds all slots matching the current letter, including duplicates |
| `printedGroups()` | Keeps the original printed street-label blocks separate, including repeated street names |
| `renderShelf()` | Draws all four columns and four levels, with labels underneath |
| `paint()` | Adds correct/hint outlines and disables answered slots |
| `setZoom(value)` | Scales the entire shelf bank |
| `feedback(text, kind)` | Displays feedback, optionally using success/error colors |
| `stats()` | Updates score, accuracy, streak, best score and progress |
| `newRound()` | Chooses a fresh set of letters and resets round counters |
| `nextQuestion()` | Prepares and displays one letter |
| `choose(s, b)` | Checks the selected slot and updates scoring |
| `hint()` | Shows a matching slot without awarding a point |
| `finish()` | Ends the round and saves the best score |
| `renderEditor()` | Displays the searchable draft address list |

Inside `renderShelf()`, `n` means the left-to-right column, your **Row**. `r` means the top-to-bottom level, your **Section**. This filter connects each displayed shelf to the right records:

```js
slots.filter(s => s.section === r && s.row === n)
```

Label groups are explicit data. Two neighboring blocks both labeled Sonnenbadweg stay separate when they have different `labelGroup` IDs. Do not merge them by street name. The source list currently defines 147 printed blocks across the 16 shelf bands.

The `.slot` button contains the colored `.pocket` first, then the house-number `.label`. The separate `.street-rail` follows the entire line of buttons. Both use the same compact, equal-slot CSS grid to keep numbers under their pockets and street blocks under their intended numbers. Full names wrap across lines rather than being shortened with CSS ellipses. All four columns fit the available screen width at 100%. Zoom increases the table width for closer inspection; Fit table restores the full view. Photo measurements remain in the data for reference and do not force the table to become wider.

## Important state variables

| Variable | Meaning |
| --- | --- |
| `slots` | Current address data, including applied edits |
| `scope` | Which section supplies letters, or `all` |
| `mode` | `practice` shows labels; `memory` hides them |
| `queue` | The selected letters for this round |
| `current` | The letter being sorted now |
| `completed` | Number of letters finished; also the next position in the queue |
| `score` | Letters answered correctly on the first try without help |
| `attempts` | Letters attempted, including hints and skips; repeated wrong clicks do not keep increasing it |
| `streak` | Consecutive first-try successes without help |
| `helped` / `hadError` | Whether the current letter used help or had an incorrect choice |
| `answered` / `done` | Whether this letter / the whole round is finished |
| `draft` | Temporary editor data; Cancel discards it |

The older `section` variable remains as a marker used by hints. It does not select which cabinets are drawn. All cabinets are always rendered.

## Connect a button to an action

An HTML `id` identifies one element. In this game, `$()` is shorthand for looking up that element:

```js
const $ = id => document.getElementById(id);
```

This assigns a function to a button's click event:

```js
$('restart').onclick = () => {
  newRound();
};
```

`=>` creates an arrow function. The statements inside it run when the player clicks, rather than immediately when the page loads.

CSS uses `.slot` to select elements with `class="slot"`, and `#score` would select the element with `id="score"`. Multiple elements can share a class; IDs should be unique.

## A small feature to try yourself

Add a button that returns to the full-table view. First, put this inside the existing `header-actions` area in the HTML:

```html
<button id="my-fit-button">Full table</button>
```

Then add this near the other button handlers inside the JavaScript block:

```js
$('my-fit-button').onclick = () => {
  setZoom(100);
  $('workbench').scrollLeft = 0;
};
```

Zoom in and move sideways, then click your new button. It should restore the full-table view and move back to the left edge. This repeats the existing Reset zoom behavior, making it a small exercise in connecting HTML and JavaScript.

## Saved edits and experiments

The learning copy uses the storage name `brief-trainer-learning-photo-v1`. The normal game uses `brief-trainer-photo-layout-v1`, so this copy does not deliberately overwrite the normal game's saved entry. Browsers differ in how they handle storage for local files; export address backups before relying on it.

Saved address edits can override changes you make to the built-in labels. To start a fresh experiment, change `STORE` to a new name such as `brief-trainer-learning-test2`, then refresh. This leaves the earlier entry intact. To reuse corrected labels from the regular game, export its address backup and import it into the learning copy, then Save changes.

For a different physical table, use its own complete address list and its own storage name. A multi-table selector is not implemented yet. The current renderer expects four levels and four columns; another shape would also need changes to its loops, controls and CSS grid.

## If a change does not work

Save first, then refresh the browser. Check that you edited the same file the browser has open. If necessary, open the browser's developer tools and inspect the Console for the first error and its line number.

Common mistakes are missing quotes, missing commas between address records, mismatched braces, or a button ID that differs between HTML and JavaScript. Undo your last change and try a smaller edit. Avoid renaming IDs or dataset identities while you are first learning the layout.

## Photos for the next table

Send a full-table overview, then readable close-ups taken straight on. Label each image with its location using your numbering: **Section = level from top to bottom; Row = column from left to right**. For example, Section 1 Row 2 is the top level in the second column from the left.

If one close-up cannot cover the whole shelf, include a few overlapping slots in the next photo. Keep house numbers, St / Ha / Pa suffixes, street names and slot boundaries visible. I can transcribe those labels, so you do not need to type the address list yourself. Any unclear labels will still need checking.
