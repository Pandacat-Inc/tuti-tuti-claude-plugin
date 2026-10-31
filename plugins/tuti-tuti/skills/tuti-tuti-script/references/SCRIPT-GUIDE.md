# Writing a Tuti Tuti script

This document is for whoever writes the narration of a tutorial, person or AI. Read all of it before writing. Everything in it is checked against the parser by automated tests, so it describes what Tuti Tuti actually does, not what it should do. Where the parser does not support something, this guide says so rather than promising it.

## 1. What Tuti Tuti does with your words

Tuti Tuti takes a plain-text script and turns it into a self-driving tutorial on a live web app. It reads every sentence and files it as one of three kinds:

| Kind | Meaning | What happens on screen |
|---|---|---|
| **Act** | The presenter does something | A click, a typed value, a drag, or a drawn box, on a named element |
| **Show** | The presenter points at something | A spotlight on a named element, no touching |
| **Say** | The presenter is just talking | The page holds still |

Then it finds each named element on the real page, performs the actions while a voice reads the words, and records the result as a video. There is no hidden layer that understands intent beyond what is written. If the sentence does not say what to click, nothing gets clicked.

Two consequences follow, and everything below serves them:

1. **Every change on screen needs its own sentence, with a clear verb and the thing's on-screen name.**
2. **Everything you write is spoken aloud.** Write as a presenter talks. No stage directions, no brackets, no bullets.

## 2. Spoken-word rules

The script is read by a text-to-speech voice and heard by people who may not be looking at the screen. Write for the ear.

**Acronyms are written the way they are spoken.** "AI", "SMS", "API", "URL", "CSV" in capitals, even when the on-screen label is styled differently. Matching ignores case and small differences, so this is safe: the words "AI Plan Maker" find a button labelled "Ai Plan Maker" (checked by a test).

```
Bad:   Click Ai Plan Maker.          (the voice may say "eye plan maker")
Good:  Click AI Plan Maker.
```

**Numbers meant to be read aloud are written as words or spaced digits.** "twenty four hours", "778 608 0369". Numbers meant to be **typed** stay inside quotes exactly as they must be entered; the voice reads them, but the typing is exact.

```
Bad:   You'll hear back within 24h.
Good:  You'll hear back within twenty four hours.
Good:  For example, type "2024-07-31" into the Date field.     (typed exactly)
```

**No symbols the voice reads badly in narration.** Write "and" not "&", "or" not "/", "twenty four seven" not "24/7". The matcher reads "&" as "and", so "Terms and Conditions" still finds a link labelled "Terms & Conditions" (checked by a test). The one exception is text inside quotes, labels and typed values, which must stay exactly as it appears or must be entered.

```
Bad:   Open Terms & Conditions and read the 24/7 support line.
Good:  Open Terms and Conditions and read the twenty four seven support line.
Good:  Type "A&B Ltd" into the Company field.     (typed exactly, including the &)
```

## 3. Narrate for someone who cannot see the screen

Assume the listener hears the voice but not every pixel. Say where things are and what appears.

**Every chapter opens with a transition, then the exact path.** The transition is plain talk; the path is the clicks. Never write just "Click Voicemail" as the first thing in a chapter.

```
Bad:   Click Voicemail.

Good:  Now let's take a look at Voicemail. Open the menu. Click on Voicemail from the left panel.
```

"Now let's take a look at …", "Next, let's look at …", "Let's talk about …", "Let's walk through …" are Say sentences: they never click or spotlight anything (checked by a test). The sentences after them do the work.

**Every button gets a location clause.** "Click Add Contact at the top right." "Click Schedule Maker in the bottom right corner." "Click on Voicemail from the left panel." The clause is for the listener; the parser cuts the target before it. Words that end a target: *to, so, which, where, that, and, or, because, if, when, once, while, in order, in, inside, within, under, near, beside, next to, above, below, for, from, with, at, on, by, then, after, before, using, via*, plus commas, semicolons, colons, and dashes (checked by tests for *at*, *in*, *from*, *with*). One caution: *by*, *with*, *via*, *using* followed by a comma list are read as a list of things to spotlight, as in "you can add items by photo, voice, or typing".

```
Bad:   Click Add Contact.
Good:  Click Add Contact at the top right of the list.
```

**The first typed value on a form is introduced with "For example, …"; the fields that follow on the same form go without it.** The typing rule accepts both (checked by a test).

```
For example, type "Jane Hamilton" into the Name field.
Type "778 608 0369" into the Phone field.
```

**Say what the viewer should now see after each action.** A Show sentence right after a click tells the listener the click worked and gives the page a moment.

```
Click Add Contact at the top right. Here you'll see the new contact form with the name and phone fields.
```

## 4. The shape of a script

- Plain text. One sentence per beat. One action per sentence.
- **A blank line starts a new chapter.** Chapters are the units people play and export separately, so one chapter per page or feature.
- An optional **short title line** right before a chapter's paragraph names the chapter. It is not spoken. Keep it to seven words or fewer with no full stop. `# Title` and `1:47 Title` forms also work.
- Write beats in the exact order the screen changes.
- Sentences end with a full stop, question mark, or exclamation mark. A new sentence may start lowercase; it is still split.
- **A quotation is never split**, however many full stops it holds. "You can type here: "We're in Vancouver. We open at nine. Leave a message and we'll call back."" is one beat, and the whole message is typed exactly as written (checked by a test). Use double quotes for anything typed; apostrophes inside are fine.

```
Warehouses
Now let's take a look at the warehouse map. Click Warehouse map in the left menu. Here you'll see every aisle and shelf, laid out like your real space. Click New Warehouse at the top of the map. For example, type "Main floor" into the Name field. Then click Create.
```

## 5. Act sentences

An Act sentence starts with, or contains, a recognized verb followed by the name of the thing as it appears on screen.

### 5.1 Clicking

Recognized verbs: **click (on), tap (on), press, hit, select, choose, pick, open, expand, collapse, toggle, launch, submit, dismiss, activate**. Also **go to, head to, switch to, jump to, move on to, navigate to**, and **turn on, turn off, enable, disable, check, uncheck**, and **hover over**.

```
Click New Warehouse at the top of the map.
Now let's go to purchasing.
Switch to the warehouse map.
Open the Suppliers tab at the top of the list.
Turn on dark mode.
```

Some words are both the verb and the button: **sign in, log in, sign up, register, sign out, log out, check out, save, cancel, continue, next, back, close, finish, done, refresh**. Use them after *let's*, *we'll*, *now*, *then*, *just*, or at the start of the sentence, on their own. "Next, let's look at billing" is a transition, not a press of a Next button (checked by a test).

```
Let's sign in.
Then save.
```

**Confirming verbs** click the button that carries the verb. *Create, add, save, submit, confirm, finish, apply, send, publish, update, delete, remove, generate, start, import, export, invite, connect, upload, download, print*, followed by what is being confirmed. Tuti Tuti looks for "Send message" first and then for a button that just says "Send".

```
Then create your warehouse.
Then send the message.
```

Use these only as instructions, at the start of a sentence or after *then*, *now*, *next*, *first*, *finally*, *let's*, *we'll*. "You can add items by photo" is a capability, and becomes a Show (see section 6).

### 5.2 Typing

Put the value in quotes. Name the field after *into* or *in*. Lead the first value on a form with "For example," so the listener knows it is sample data; the next fields on the same form skip it.

```
For example, type "Blue widget" into the Search box.
For example, enter "42" in the Quantity field.
Name it something like "Main Warehouse".
```

The third form goes into the field called Name. A bare `Type "Main floor".` with no field named goes into the field that is waiting for input: the one the app already focused, or the only one in the dialog that just opened.

One field per sentence. A form with three fields is three sentences, then one sentence for the submit button.

**Several fields in a row, without naming each.** After a typed sentence, a bare "write …" moves to the next empty field in the same form; a sentence that offers another example for the same field, opening with *or*, *alternatively*, or *instead*, rewrites the same field instead. Naming the field before the verb also works: "For the phone number, write "778 608 0369"." finds the phone field, and an unlabelled field is still found by its type (checked by tests).

```
For example, write "Jordan Lee".                      (first field)
Or you can write "Pandacat".                          (same field, rewritten)
For the phone number, write "778 608 0369".           (the phone field)
```

### 5.3 Drawing (click, hold, drag, release)

For apps where you draw shapes on a map or canvas. Recognized cues: **click and drag, click and hold, press and drag, drag out, drag across, drag your cursor, draw, outline, sketch, trace**, and **click anywhere inside X … and drag / let go**.

```
Click and drag on the floor plan to draw the shape of the warehouse.
Click anywhere inside the warehouse box and drag your cursor, then let go.
Draw a large rectangle for the first aisle.
```

Name the surface after *on*, *over*, *inside*, or *within*. If none is named, the biggest canvas or map on screen is used. The box's size comes from what is being drawn: an **aisle**, **corridor**, or **lane** is long and thin; a **shelf**, **rack**, or **bin** is small; a **zone**, **area**, or **section** is a block; a **warehouse**, **room**, or **outline** is large. Add *small*, *large*, or *whole* to scale it. If a drawing mode must be selected first, say so in its own sentence before the drawing sentence.

### 5.4 Dragging one thing onto another

```
Drag the Blue widget onto Shelf A.
Move the Receiving zone into Aisle 2.
```

### 5.5 Two actions in one sentence

"X and then Y" is split into two beats automatically when both halves are actions. Anything else joined by *and* performs only the first action. When in doubt, write two sentences.

## 6. Show sentences

A Show sentence names something without acting on it. Recognized openers:

- **Here you'll see the …**, **you'll see the …**, **notice the …**, **look at the …**, **find the …**, **check out the …**
- **Here's the …**, **here is the …**, **this is the …**, **these are the …**, **there's a …**
- **The X is here**, **the X lives here**, **the X sits at the top**
- **You can …**, **this lets you …**, **this allows you to …** (a capability, pointed at, not done)

```
Here you'll see the activity log.
This is the AI assistant.
The purchase orders live here.
You can add items by photo, voice, or typing.
```

The last form is special: a list after *by*, *with*, *via*, or *using* lights each item up as its word is spoken.

The first two groups of openers only count when a named thing follows: *the*, *your*, *every*, a Title Case name, or a quoted label. "Everything you'll see runs on the free plan" names nothing after *you'll see*, so it is Say. Quantities are not things either: "There are two modes" is Say. And "let's look at" is a transition (section 3), never a Show.

## 7. Say sentences

Everything else. Overviews, benefits, reasons, transitions, what the user will get. The page holds still and the words are spoken with captions.

**Keep action verbs out of Say sentences.** "In this video we click through settings" would try to click "through settings". Describe with *we'll see how*, *this covers*, *it has*, *there are*, *the app keeps*. Note that *we'll see the dashboard* is a Show, because it names a thing; *we'll see how the pieces fit* is Say, because it doesn't.

## 8. Naming the thing: the single most important skill

The target is the words right after the verb. Tuti Tuti matches those words against what is visible: button text, link text, labels, placeholders, icon names. Exact words always win; a few synonyms are understood (home / dashboard, sign in / log in, bell / notifications, settings / preferences, add / new / create, menu / hamburger / sidebar).

In order of reliability:

1. **The exact label, in quotes or Title Case.** `Click "New Warehouse".` `Open Purchase Orders.`
2. **The label in plain words.** `go to purchasing` finds Purchasing.
3. **With a role word.** `the Suppliers tab`, `the bell icon`, `the Search box`, `the Save button`. The role word is dropped from the search and used to prefer the right kind of element.

Rules:

- Six words or fewer.
- Never a pronoun or a place word: not *it*, *this*, *that*, *here*, *there*, *anywhere*.
- Use the words that are on the screen, not a description of the function. "the button that opens the order form" fails; "New order" succeeds.
- A word inside quotes is never read as a command. `Select "add or draw aisle"` is a click on that label even though it contains "draw".
- If you do not know the exact label, use the shortest likely one. Fuzzy matching covers small differences and capitalisation, not paraphrases.

Where the target sits in the sentence sets when the click happens. The click lands on the longest word of the target, inside the phrase, so a repeated word earlier in the sentence does not fire early. Put the target early and let the location and reason trail.

## 9. Navigation patterns

**Menus that slide out, or a dropdown behind an avatar.** The page link is not on screen until the menu is open, so "Open the menu." is its own sentence before every page click. Repeat it in **every chapter**, because chapters are played and exported separately and each one starts from wherever the app is.

```
Bad:   Click Voicemail from the left panel.                          (the panel is closed)

Good:  Open the menu. Click on Voicemail from the left panel.
```

**Icon-only buttons.** Tuti Tuti reads a button's accessibility label and its tooltip as well as visible text, in that order. So a hamburger with no text but labelled "Open menu" answers to "menu". A bare icon with neither a label nor a tooltip gets only its colour and position, as in "the gold icon on the left edge", which is fragile. Ask the app owner for a label rather than guessing.

**Menus, dialogs, tabs, modes, data.** Open the menu in one sentence, choose the item in the next. Opening a dialog is a sentence, each field is a sentence, the confirm button is a sentence, closing is a sentence. Say which tab. Click a drawing or editing mode before using it. Only act on data that exists. Scrolling is automatic; do not write "scroll down to". Keyboard shortcuts are not supported; write the click instead.

## 10. Live-AI screens

When a step talks to a chatbot, an assistant, or anything that answers on its own time:

- Put **everything the assistant needs into one typed message**. It cannot handle a follow-up question. If it asks one, the next click in the script will miss, because the screen is not what the script expects.
- Follow each Send with **two or three narration sentences** so the reply has time to arrive before the next action.
- Tell the reader what to do if the assistant asks something anyway. This is a note for the person reading the script, written in the Say voice.

```
For example, type "Make a weekly plan for a two person cleaning crew covering six homes" into the message box. Then send the message. The assistant reads the request and drafts the plan. This usually takes a few seconds. Here you'll see the proposal card with the week laid out. If the assistant asks a question instead, answer it in the box, send again, and the plan appears.
```

Tuti Tuti cannot wait for a reply by itself. The narration is the waiting.

## 11. Data side-effects

The script runs on a live app. Every Act changes real data. Before writing:

- **List what the tour creates**: records, contacts, plans, messages, warehouses. Include the list with the script.
- **Say how to clean up between takes**, or the second run will find the thing already there and the story will not match the screen.
- **Use the app owner's designated demo values.** For contacts and texts that means a real company number the owner has chosen for demos, never a stranger's number, which could be charged or blocked. Same for emails.

## 12. Writing a full tour of an app

One chapter per page or feature, in the order a new user would meet them. A chapter that works:

1. **Transition.** One Say sentence: "Now let's take a look at purchasing."
2. **Path.** The Act sentences that get there, with locations: "Open the menu. Click on Purchasing from the left panel."
3. **Orient.** One Show sentence: "Here you'll see every purchase order, grouped by supplier."
4. **Explain.** One or two Say sentences on why it matters.
5. **Demonstrate.** One to three Act sentences doing the single most useful thing on that page, each with the exact label and a location.
6. **Confirm.** A Show sentence saying what appeared.

Two to eight sentences per chapter. Eight to eighteen words per sentence. Spoken pace is about two and a half words per second. Open with an Introduction chapter of two or three Say sentences with no actions. Close with a short Say chapter. Vary the openers. Sound like a person showing a colleague. Never narrate the cursor.

### Before writing, you need

- The pages or features, in order, with the **exact navigation labels** and how the navigation opens.
- The **exact labels** of the buttons, tabs, and fields you will use, and roughly where they sit.
- The owner's **demo values** for anything typed.

If you do not have the exact labels, ask. Guessing labels is the main reason a script fails.

## 13. Self-check before handing over

- [ ] Every screen change has its own sentence with a recognized verb and the on-screen name.
- [ ] Every chapter opens with a transition, then "Open the menu." if the app needs it, then the clicks.
- [ ] Every button has a location clause. The first typed value on each form starts "For example,"; every typed value is in quotes.
- [ ] Acronyms are capitalised as spoken; spoken numbers are words or spaced digits; no & or / in narration.
- [ ] Nothing is clicked before it is visible. Menus, dialogs, and modes are opened first.
- [ ] No *it*, *this*, *that*, *here*, *there*, *anywhere* as a target.
- [ ] Say sentences contain no action verbs.
- [ ] Live-AI steps use one complete message, then two or three sentences of narration.
- [ ] The list of what the tour creates, and the cleanup, comes with the script.
- [ ] Blank lines between chapters, each with a short unspoken title line. No brackets, bullets, emojis, or markdown.
- [ ] Read aloud, it sounds like a person.

## 14. Output format

Return plain text only. Chapter title line, newline, the chapter's sentences as one paragraph, blank line, next chapter. No other formatting of any kind.

## 15. The prompt to give an AI

Three kinds of script exist, and the first thing an AI should settle is which one is wanted: a **product demo** (about sixty seconds, or about three minutes: the one outcome a buyer cares about, no settings), a **step-by-step tutorial** (one task start to finish, or one feature end to end: every click and typed value in order, each result confirmed), or a **bit-by-bit walkthrough** (one screen, or the whole app: every control named by its exact label and explained, nothing skipped, not even a small button). Ask if the person has not said. The ready-made prompts for each are on tutituti.ca and in the MCP server's prompts. Then the prompt below applies to all three.

> You are writing the narration for a Tuti Tuti tutorial, which will be spoken aloud by a text-to-speech voice, performed on the live app, and heard by people who cannot see the screen. Write plain prose, one sentence per beat, one action per sentence.
>
> Spoken-word rules: write acronyms as they are spoken, in capitals ("AI", "SMS", "API"), even if the on-screen label is styled differently; write numbers meant to be heard as words or spaced digits ("twenty four hours", "778 608 0369"), and keep numbers meant to be typed inside quotes exactly as entered; use no symbols the voice reads badly in narration ("and" not "&", "or" not "/", "twenty four seven" not "24/7"), except inside quoted labels and typed values, which stay exact.
>
> For a listener who cannot see the screen: open every chapter with a transition ("Now let's take a look at Voicemail."), then the exact path ("Open the menu. Click on Voicemail from the left panel."), never just "Click Voicemail"; give every button a location clause ("Click Add Contact at the top right"); introduce the first typed value on a form with "For example, type "…" into the Field field." and type the form's remaining fields without repeating "For example"; and after each action say what the viewer now sees ("Here you'll see the proposal card …").
>
> Each time the screen must change, write a sentence that uses an action verb (click, open, go to, switch to, select, type … into …, click and drag on …) followed by the exact on-screen label, in quotes or Title Case. Open menus, dialogs, and modes in their own sentence before referring to what they reveal, in every chapter. Keep overviews free of action verbs. For a chatbot step, put everything into one typed message, then two or three sentences of narration. List what the tour creates and how to clean up. Put a blank line between features, with a short unspoken title line before each. No brackets, bullets, or stage directions. Use only labels you have been given; ask if you are missing one.

## 16. After rehearsal: the rework file

Rehearsal is the test of a script. When it is done, the panel's **05 Rework** step offers a JSON file (`Title - rework.json`) written for whoever will rewrite the script, usually the AI that is building the app. It lists every beat with a status and, for anything that was not a clean match, a finding, a `scriptFix`, and an `appFix`:

| status | what happened | what to do |
|---|---|---|
| `ok` | the words matched one element | leave the sentence alone |
| `asked` | several things matched; a person or model chose one | use the `pinnedTo` label and add a location clause or role |
| `pointed` | nothing matched; the owner clicked the element | say the `pinnedTo` label, which is what they meant |
| `recorded` | the owner did it by hand as several actions | one sentence per screen change, each with its control's label |
| `skipped` | the owner skipped the beat; it now only speaks | if a screen change was meant, name its control |
| `missing` | nothing on the page matched; now narration | exact label, location clause, and open what reveals it first |
| `failed` | pinned, but something went wrong when played | read the finding; often the app needs a readiness fix (section 9 of the app guide) |

The file also carries the whole script as it stands, so the rewrite can be pasted straight back into Tuti Tuti and rehearsed again. Give it to the AI with one line: *"Rewrite this script following docs/SCRIPT-GUIDE.md; the instructions inside the file say how to read it."*

## 17. A complete example

See `docs/examples/quick-tour.txt`. It is parsed by the test suite, and every sentence in it comes out as the kind and action described in this guide.

## 18. Writing in French

The same rules, in French. The parser reads both languages sentence by sentence, so a French script needs nothing switched on, and a fr-CA voice reads it.

- **Transitions** are Say sentences: "Maintenant, jetons un coup d'œil à …", "Ensuite, …", "Enfin, …", "Passons à …", "Regardons …", "Voyons …".
- **Clicking**: *cliquez sur, appuyez sur, touchez, sélectionnez, choisissez, ouvrez, allez à, allez dans, rendez-vous à, activez, désactivez, cochez, décochez*; hover with *survolez*. Location clauses as in English: "en haut à droite", "dans le menu de gauche", "au bas de la page". The target ends at *à, au, aux, en, dans, sous, sur, pour, avec, par, puis, et, ou* and at the English stops.
- **The role word comes first, in lowercase**: "le bouton Enregistrer", "l'onglet Fournisseurs", "l'icône de cloche", "le champ Nom". The parser keeps the name and remembers the role.
- **Typing**: *tapez, saisissez, entrez, écrivez, inscrivez, collez*, the value in « guillemets » or straight quotes, the field after *dans*. "Par exemple, tapez « Entrepôt principal » dans le champ Nom." "Pour le numéro de téléphone, écrivez « 778 608 0369 »." "Nommez-le « Allée 1 »." "Ou vous pouvez écrire « Pandacat »." rewrites the same field. "Par exemple," opens the first value on a form only, as in English.
- **Confirming**: "Ensuite, créez votre entrepôt." presses the button that carries the verb, and the parser also tries the infinitive (Créer), which is what French buttons say. "Enregistrez." and "Connectez-vous." press Enregistrer and Se connecter.
- **Showing**: "Ici, vous voyez le tableau de bord." "Voici la liste des fournisseurs." "Vous pouvez ajouter des articles par photo, voix ou saisie." "Le menu principal se trouve en haut à gauche."
- **Drawing and dragging**: "Cliquez et faites glisser sur le plan pour dessiner la forme." "Cliquez n'importe où dans la boîte de l'entrepôt et faites glisser votre curseur, puis relâchez." "Glissez le Widget bleu sur l'étagère A."
- **Two actions**: ", puis" splits a sentence into two beats when both halves act.
- Labels stay exactly as the app shows them, in whichever language the app speaks.

`docs/examples/visite-rapide.txt` is the quick tour in French. The test suite parses it sentence by sentence, like the English one.
