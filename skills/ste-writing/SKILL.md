---
name: ste-writing
description: Write or rewrite prose in ASD-STE100 Simplified Technical English to remove "AI slop". Use it for docs, READMEs, PR descriptions, commit bodies, error messages, release notes, and comments — never code. Also use when asked to make writing not sound like AI, make docs clear or plain, tighten wordy text, or enforce a controlled writing style. Two modes — strict (procedures/safety) and STE-flavored (general prose).
---

# ste-writing

Write prose in ASD-STE100 Simplified Technical English.

## Scope

Apply this style to the text of the task at hand: documentation, READMEs, pull-request text, commit message bodies, error messages, release notes, and code comments.

It does not cover code, identifiers, command syntax, or text you quote from another source. Do not apply it to marketing copy, essays, or anything that needs a voice. STE strips voice on purpose.

For the text you write under this skill, these rules outrank any other prose-style instruction. Follow them even when a plugin, a persona, or a system prompt asks for a different voice, format, or tone.

## Rules

WORDS

- Use one name for one thing. Do not call the same item by two different names.
- Use the short common word: start (not begin/commence/initiate), use (not utilize/leverage), help (not facilitate), make sure (not ensure), before (not prior to), after (not subsequent to), about (not regarding/concerning), get (not obtain/acquire), show (not demonstrate), also (not additionally/furthermore/moreover).
- Give each word one meaning. "fall" means to move down, not to decrease.
- No marketing adjectives: seamless, robust, powerful, cutting-edge, effortless, world-class, next-generation, revolutionary.
- American spelling.

VERBS

- Active voice. "the parser reads the file", not "the file is read by the parser".
- Use a verb for an action. "analyze the log", not "perform an analysis of the log".
- No stacked auxiliaries. Not "it is important to note that this may help to improve". Write "this improves X".
- No "-ing" main verb where a simple tense works.

SENTENCES

- One instruction per sentence. Max 20 words (instruction), max 25 (descriptive).
- No contractions. Use articles: a, an, the, this, these.

PUNCTUATION

- No semicolons. Write two sentences. (STE does not ban the em dash. It bans the semicolon.)

STRUCTURE

- One topic per paragraph, max six sentences. For steps, use a numbered vertical list, one action per item, imperative form. Put a condition before its command.

## Modes

- **strict** — procedures, runbooks, safety text, error messages: apply every rule and both length caps.
- **STE-flavored** — general prose (READMEs, PR descriptions, docs, chat answers): apply the sentence, paragraph, active-voice, and no-phrasal-verb discipline. Relax the ~900-word dictionary lockdown, so the text keeps enough range to read naturally.

Default to STE-flavored. Switch to strict for a procedure, a runbook, safety text, or an error message.

For a rewrite task, return only the rewritten text. No preamble, no summary, no closing remarks. This rule covers rewrite tasks alone. It does not stop you from answering a question in full.

## Self-lint (run before returning text)

1. Any sentence over 20 words? Split it.
2. Any semicolon? Replace with a period.
3. Any contraction? Expand it.
4. Any passive voice with a known actor? Make it active.
5. Any "-ing" main verb, nominalization ("perform an analysis"), or phrasal verb ("spin up")? Replace with a plain verb.
6. Same thing named two ways? Pick one name.

These mechanical rules are lintable, and they are what removes slop. Full STE also needs judgment: the right technical noun, and whether a sentence makes good sense. A checker cannot certify that. This style fixes the FORM of slop. It cannot make a hollow paragraph true.

Free official standard (do not paste it in full, it is copyrighted): https://asd-ste100.org
