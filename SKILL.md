---
name: clammerly
description: Grammarly for clankers. Takes a path, a diff, pasted text, or a plain description like "the front page", "onboarding" or "all user-facing copy". Finds the writing tics that make text read as machine-written (negation reveals, "the one thing that matters", aphoristic closers, fragment cadence, dressed metaphors and more) plus vague wording, hidden actions and buried points, then rewrites each line plainly. Use when asked to run /clammerly, de-slop or check copy, review user-facing strings, a prompt, an email, a landing page or a draft for AI-sounding writing.
---

# Clammerly

Check writing for the patterns people notice as "AI wrote this", and fix them. Works on product copy, emails, prompts, docs and drafts.

## What to check

The arguments say what to check: file paths, a directory, a git range, pasted text, or `--diff` for the uncommitted diff. With nothing given, check the user-facing strings added or changed in the current diff. Options:

- `--fix`: apply the rewrites after reporting them. Without it, report only.
- `--strict`: also report the borderline cases.

### Vague targets

The target can be a plain description instead of a path: "the front page", "onboarding", "the paywall", "our emails", "the share page", or "all user-facing copy". Find the files yourself before checking anything:

1. Read the project's CLAUDE.md and docs for where that surface lives. They often name the screen, the content file and the i18n area.
2. Search for it: route and screen names, the entry point for the URL (for "the front page", whatever renders `/`), and the i18n keys or content files those screens read. Follow the imports until you reach the strings themselves. Copy is often kept in a content or i18n file, separate from the component.
3. Include everything the reader sees on that surface: its metadata and link-preview text, buttons, dialogs it opens, and the emails or notifications it sends.
4. Before checking, list the files you will read in one line, so the user can correct the scope.

For **all user-facing copy**, check the English source of every surface: i18n files, content files, email and notification templates, store metadata, and prompts that write what people read. Read their strings straight from those files instead of opening every component. If the work is large and subagents are available, give each one a surface, this skill's file path, and the report format. Merge their findings into a single report, and check that each rewrite follows the rules below. Order the report by how many people read the line: landing page and onboarding first, then emails and notifications, buttons and paywall, then the rest.

Check only text a person reads: UI strings, i18n files, emails, notifications, landing copy, prompts whose wording the model copies, docs. Skip code comments, logs, identifiers, test fixtures, and quotations of other people (customer quotes, testimonials).

Read the project's own voice rules first, if it has any (CLAUDE.md, a style guide, a banned-words list). Apply them on top of this list. Where they conflict, the project wins.

## The patterns

Each line can carry several. Name them by their number.

### The ten tics

1. **Negation-first reveal.** Something is set up as what it is not, then revealed. "It's not a diet. It's a lifestyle." "Less planning, more living." "This isn't just an app." *Fix:* say what it is. A real negation that tells the reader a fact is fine: "We don't charge fees."
2. **Significance designation.** Calling something important instead of showing it. "The one thing that matters." "Here's the part that counts." "What matters most." *Fix:* name the thing. If it matters, the sentence will show it.
3. **Abstraction agency.** Abstractions doing the acting. "Your week is speaking." "Let momentum carry you." "It grows with you." "The board owns the reminders now." *Fix:* give the verb to a person or a thing you can point at. "You walked three times."
4. **Deferred noun.** A colon or a drumroll before the point. "This changes everything: our new card." "It's finally here. The recipe box." "The things that..." *Fix:* lead with the noun.
5. **Coy or thesis header.** A title that teases instead of labelling. "The quiet part." "Before you go." "Why this, and not another tool." *Fix:* say what is under it. "Cancel your subscription." "Refund policy."
6. **Aphoristic closer.** A wise-sounding last line. "Small steps, big results." "Showing up is the whole game." "Let's make it count." *Fix:* end on the useful part: a fact, a step or a question. Often you can just delete it.
7. **Straw-man knockdown.** Knocking down a thing nobody proposed. "Forget willpower." "No more boring spreadsheets." "Instead of a regular planner." "There are no hoops." *Fix:* say what this one does.
8. **Fragment cadence.** Clipped verbless fragments for rhythm. "Fast. Simple. Yours." "One meal. Then another." "Money, finally calm." *Fix:* write full sentences. Short labels, buttons and list items are fine.
9. **Dressed metaphor.** Decoration standing in for the plain word. "Your money's north star." "Finding the threads that matter." "Simmering the logic." "The door is open." *Fix:* the plain word. Keep a metaphor only when it is the clearest way to say it.
10. **Label-first framing.** A label before the sentence. "Quick note:" "The short version:" "Bottom line:" "The first thing to do:" "So important:" *Fix:* drop the label.

### Clarity faults

11. **Hidden action.** The verb is buried in a noun ("needs a payment update before access can resume"), the subject is an abstraction instead of a person, a sentence is passive for no reason, or a button or title doesn't say what happens ("Accept", "Your account needs attention"). *Fix:* the person does the verb. "Update your card to keep using the app." The button names its result: "Add to my week".
12. **Vague wording.** "It", "this" or "both" with nothing to point at. Stacked nouns ("an ongoing picture of your life"). Claims with nothing behind them ("private and protected", "a better way to..."). Phrases nobody says aloud. *Fix:* the concrete thing. What exactly happens, what number, which screen.
13. **Jargon.** Internal or system words reaching an everyday reader: "tokens generated", "billing profile", "context", "trust layer", "standard memory mode", feature names they haven't met yet. *Fix:* the word the reader would use.
14. **Empty or buried.** Filler, a point buried at the end, saying the same thing twice, a subject line or preheader that hides the news ("Your plans are saved" when access is ending). *Fix:* the point goes first. Cut the rest.

## How to judge

- Judge by what the line does to the reader, not by a word match. "Not available on Android" is a fact, not tic 1. "Before you go" is tic 5 on a cancel screen, but fine as an ordinary sentence opener.
- Look at the line's context: the surrounding strings, where it is shown (button, title, email subject, push body, prompt), and who reads it. A prompt's own wording counts double, because the model copies the style it is written in.
- Most functional copy (errors, confirmations, dialog titles) is clean. Expect the tics to cluster in motivational, progress, onboarding and marketing lines.
- Don't report a line just to have findings. If the copy is clean, say so.

## How to rewrite

- Keep the meaning, the facts and every placeholder (`{name}`, `%s`, `{{count}}`) exactly. Keep legal and billing terms accurate. Flag a legal line rather than loosely rewriting it.
- Keep the length about the same, or shorter. Plain, warm, specific, and sentence case.
- Check your own rewrite against all fourteen patterns before reporting it. A common failure is swapping one tic for another, for example replacing an aphorism with a fragment.
- Don't explain the product inside the copy, and don't add promises the product can't keep. If the rewrite needs a fact you don't have, ask, or mark it.
- If a string has translations (i18n files), report the English and note that the translations need the same change. Only rewrite another language when asked.

## Report

Group the findings by file, with the most-read surfaces first (email subjects, push bodies, landing hero, onboarding, buttons). For each finding:

| Where | Now | Pattern | Rewrite |
|---|---|---|---|
| `path/to/file.ts:42` | "Small wins stack." | 6, 8 | "Each one you check off counts toward your streak." |

Then add one line on anything systemic. For example: "the prompt in X writes in tics 6 and 8, so its output will too. Fix the prompt."

With `--fix`, apply the rewrites with exact edits, then run the project's type check or tests if the files are code. List what changed and anything left for a person to decide (legal lines, A/B-tested copy, translations).
