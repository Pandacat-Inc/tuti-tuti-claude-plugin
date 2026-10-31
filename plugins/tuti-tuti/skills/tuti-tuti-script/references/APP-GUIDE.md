# Making an app Tuti Tuti ready

Tuti Tuti drives a web app the way a careful person would: it reads what's on screen, finds the thing a sentence names, and clicks, types, or points. Everything it can see, it can use. This guide says what to build into an app so a script written in plain English works first time. It is also plain accessibility, so screen-reader users benefit from the same work.

Give this file to whoever builds the app, human or AI. The last section is a block to paste into a project's instructions for an AI coding assistant.

## 1. Every control has a name a person would say

Tuti Tuti reads, in this order: the accessibility label (`aria-label` or `aria-labelledby`), the visible text, the tooltip (`title`), and an image's alt text. A control with none of these can only be found by colour and position, which is fragile.

| | |
|---|---|
| **Do** | `<button>New warehouse</button>` |
| **Do** | `<button aria-label="Open menu"><ChevronIcon /></button>` |
| **Do** | `<button title="Notifications"><BellIcon /></button>` |
| **Don't** | `<button><BellIcon /></button>` with nothing else |
| **Don't** | text baked into an image or a canvas |

Name things the way the script will say them. If the sidebar item reads "Purchasing", the script says "go to purchasing". A label that only exists in a design file isn't a label.

## 2. Labels are unique on a screen, or carry context

Two buttons that both say "Delete" make Tuti Tuti stop and ask which one. Prefer "Delete item" and "Delete warehouse", or give the second one an `aria-label` that says what it deletes. The same goes for "Edit", "Save", "More", and icon buttons repeated in every row of a list: `aria-label="Edit Blue widget"`.

## 3. Fields say what they are

For every input:

- A real `<label for>` or a wrapping `<label>`, or `aria-label`. A placeholder alone works but disappears when typing starts.
- The right `type`: `tel`, `email`, `number`, `date`, `password`, `url`. Tuti Tuti reads the type as a word, so "for phone number, write …" finds an unlabelled `type="tel"` field.
- `autocomplete` where it applies (`name`, `tel`, `email`, `organization`). It's read too.
- Fields in the order a person fills them. When a script types into "the next field", Tuti Tuti goes to the next empty field in document order within the same form.

```html
<label for="phone">Phone</label>
<input id="phone" type="tel" autocomplete="tel" />
```

Typing is done one character at a time with real `input` events, so controlled inputs in React, Vue, and Angular update normally. Fields that only accept pasted text, or that reformat on every keystroke in ways that move the caret, can break this; test them.

## 4. Dialogs, menus, and drawers announce themselves

While a dialog is open, Tuti Tuti prefers things inside it and discounts the page behind. It recognises a dialog by `<dialog open>`, `role="dialog"`, `aria-modal="true"`, or a class containing *modal*, *dialog*, *drawer*, *sheet*, or *popover*. Use one of the first three.

- **Closed means hidden.** A drawer that's slid off-screen is correctly ignored, but a drawer that's merely `opacity: 0` on top of the page is not. Use `hidden`, `display: none`, `visibility: hidden`, or `inert` on closed overlays.
- **Menus have text.** Menu items are links or buttons with visible text, not icons alone.
- **Tabs are tabs.** `role="tab"` with `aria-selected`, or at least buttons with the tab's name.
- **Modes are buttons.** If drawing or editing needs a mode, make it a labelled button ("Add or draw aisle"), so the script can press it in its own sentence.

## 5. Surfaces you draw on are labelled

A map or canvas the user draws on should say so: `<canvas aria-label="Warehouse floor plan">` or `role="application"` with an `aria-label`. Shapes already drawn, if they're SVG or absolutely positioned elements, are detected and avoided when Tuti Tuti places a new one; a bitmap canvas is opaque, which is fine, just expect new shapes to be placed from the centre.

## 6. Keep the core UI in the page

- **No iframes for the app itself.** Tuti Tuti doesn't reach inside iframes. Embedded videos are fine; the dialog with the Save button is not.
- **Shadow DOM is fine.** Open shadow roots are searched. Closed ones are not.
- **Don't virtualise short lists.** Items that don't exist in the DOM until scrolled can't be found. Long lists inside a scrolling container are fine as long as the items are present; Tuti Tuti scrolls the container to them.
- **No text in images.** Button labels, menu names, and headings are real text.

## 7. Stable hooks survive redesigns

Tuti Tuti pins each sentence to an element using, in order: `data-testid`, `data-test`, `data-cy`, a hand-written `id`, `aria-label`, `name`, `href`, stable class names, and finally the element's path. Generated ids and hashed class names are ignored on purpose.

Adding `data-testid="new-warehouse"` to the controls a tutorial uses means the tutorial keeps working when the layout changes. It's the single cheapest thing you can do.

## 8. Avoid things that fight automation

- **No `isTrusted` checks on click handlers.** Tuti Tuti's clicks are synthetic events (pointer, mouse, click, in the real order). Apps that require trusted events will ignore them.
- **No hover-only reveals for essential controls.** A button that only exists while the mouse is over a row can still be reached by a "hover over" sentence first, but it's one more thing to go wrong.
- **No keyboard-only actions.** Everything a tutorial needs is clickable.
- **No `confirm()` or `alert()` dialogs** on the paths a tutorial takes. Native dialogs block the page and the recording.
- **Predictable timing.** Show a loading state and then the content; don't swap labels or move buttons after the first paint without reason.

## 9. Say when things are ready

Dialogs that animate open, forms that load their fields, chats that scroll to the bottom once their history arrives: for a moment the control exists but isn't usable. Tuti Tuti waits, up to a few seconds, until the control is enabled, nothing above it is marked busy, and it has stopped moving. Make that wait short and certain:

- **Mark loading honestly.** Put `aria-busy="true"` on the dialog or form while it's filling, and remove it when done. Disable inputs and buttons until they work (`disabled`, not just greyed by CSS).
- **Focus the first field when a dialog opens.** A focused field is the strongest signal of "type here now", and the first thing Tuti Tuti looks for when a script says "type …" without naming a field.
- **Finish moving before you're ready.** If a chat scrolls to the bottom or a panel slides in, let that complete before enabling the composer. Keep animations short, a quarter second or so.
- **Don't shift layout after first paint** without reason. A button that moves as content streams in makes a click land on the wrong thing.

## 10. Give the tutorial a demo state

- A demo account or a reset action that returns the app to a known state, so the script matches the screen every run.
- Designated demo values: a company phone number for texts and calls, a demo email, sample names. The script guide tells authors to use these, never a stranger's number.
- Sample data that's enough to show the feature: a few items, one warehouse, a couple of orders.

## 11. Checklist

- [ ] Every button, link, tab, and icon has visible text, `aria-label`, or `title`.
- [ ] Labels are unique per screen or carry context (`aria-label="Edit Blue widget"`).
- [ ] Every input has a label and the right `type`; `autocomplete` where it applies.
- [ ] Dialogs use `<dialog>`, `role="dialog"`, or `aria-modal`; closed overlays are truly hidden.
- [ ] Menus, tabs, and modes are labelled buttons or links with text.
- [ ] Drawing surfaces have an `aria-label`.
- [ ] No iframes for the app's own UI; no closed shadow roots; no text in images.
- [ ] `data-testid` on the controls a tutorial uses.
- [ ] No `isTrusted` checks, native dialogs, or keyboard-only actions on tutorial paths.
- [ ] Loading states use `aria-busy` and `disabled`; dialogs focus their first field; animations finish before controls enable.
- [ ] A demo state, demo values, and a reset.

## 12. For an AI coding assistant: paste this into the project's instructions

> This app is driven by Tuti Tuti, a tool that performs narrated tutorials on the live UI by finding elements from their visible text and accessibility labels. When adding or changing UI: give every button, link, tab, and icon control a visible text label or an `aria-label` (never an icon alone); make labels unique per screen or add context in `aria-label`; give every input a real label and the correct `type` (`tel`, `email`, `number`, `date`) plus `autocomplete` where it applies; build dialogs with `<dialog>` or `role="dialog"` and hide closed overlays with `hidden` or `display: none`, not opacity; make menus, tabs, and modes labelled buttons or links; label drawing surfaces with `aria-label`; keep the app's own UI out of iframes and closed shadow roots; put `data-testid` on controls a tutorial would use; do not require trusted events on click handlers; do not use `alert()` or `confirm()`; mark loading with `aria-busy="true"` and `disabled` until controls work, focus the first field when a dialog opens, and finish animations and auto-scrolls before enabling controls; and keep a demo state with designated demo values and a reset.
