---
name: tuti-tuti-script
description: Write, rewrite, check and save a Tuti Tuti tutorial script. Use whenever the person asks for a tutorial, demo, walkthrough or onboarding script for a web app, mentions Tuti Tuti, or asks to fix a script that missed in rehearsal. Reads the script guide first, checks every draft with the Tuti Tuti MCP checker, and saves to the person's library only when asked.
---

# Tuti Tuti scripts

Tuti Tuti is a Chrome extension that performs a narrated tutorial on a live web app from plain sentences: each sentence becomes a click, a spotlight, or narration. The script is the whole job, so its shape matters.

## Before writing a word

Ask which kind of script the person wants, unless they already said:

- **Product demo**: about sixty seconds, or about three minutes. The one outcome a buyer cares about; no settings, no edge cases.
- **Step-by-step tutorial**: one task start to finish, or one feature end to end. Every click and typed value in order, each result confirmed.
- **Bit-by-bit walkthrough**: one screen, or the whole app. Every control named by its exact label and explained, nothing skipped, not even a small button.

The MCP server offers each as a prompt (`tuti_demo`, `tuti_tutorial`, `tuti_walkthrough`, with a `length` argument).

Then:

1. Read `references/SCRIPT-GUIDE.md` in this skill, or call the MCP tool `tuti_script_guide` for the current edition. Follow it exactly.
2. Read `references/quick-tour.txt`: a complete tour in the exact format. Imitate its shape. For French, call `tuti_example_script` with `lang: "fr"`.
3. Ask the person for the exact on-screen labels and their demo values rather than guessing them.

## The loop

1. Write the script: one sentence per beat; a blank line and a short unspoken title line between chapters; every chapter opens with a transition such as "Now let's take a look at Purchasing." followed by the path; every screen change is its own sentence starting with an action verb, the exact on-screen label, and a location clause such as "at the top right"; the first typed value on a form starts "For example," and every typed value sits in quotes; acronyms capitalised as spoken; no `&` or `/` in narration; open menus, dialogs and modes before referring to what they reveal; no pronouns or place words as targets; nothing in brackets.
2. Call `tuti_check_script` on the whole script and fix every warning. Repeat until it returns none.
3. Call `tuti_parse_script` and confirm each sentence has the kind and action intended.
4. Hand the person the script as plain text to paste into the panel, unless they asked you to save it.

## With the account server (Pro)

When the `tuti-tuti-account` server is connected (run `/mcp` to sign in once):

- `tuti_my_tutorials` and `tuti_get_tutorial` show what is in the library and what rehearsal could not settle.
- `tuti_rework` returns the rework file of a tutorial that missed: which sentence hit what instead. Reword those sentences, or tell the person which label to fix in the app.
- `tuti_save_script` puts a script into the library as a tutorial, new or replacing one by id. Ask before replacing. The person still rehearses it in the panel; nothing here plays, records or exports.

## Never

- Never invent labels. If a label is unknown, ask.
- Never put stage directions, bullets, numbering or markdown in the script: everything is spoken.
- Never save to the library without being asked.
