---
name: write-like-me
description: Write posts, threads, emails and scripts that sound like the user, not like AI. Builds a voice guide from the user's own past writing, then drafts with a writer agent and checks with a separate editor agent. Use when the user asks to write anything in their voice, set up their voice, or says a draft "sounds like AI".
---

# Write like me

Based on Nat Eliason's method ("How to Build an AI Writing System That Actually Sounds Like You", x.com/nateliason/status/2115803497773904292).

Two jobs, kept apart: one agent writes, a different agent edits. A model is bad at judging its own draft.

## 1. Set up the voice (once)

1. Ask the user for 10–30 things they wrote **before AI**: posts, emails, newsletters, scripts. Plain text is fine. Save them in `voice/samples/`.
2. Clean them: cut signatures, footers, plugs, links and anything someone else wrote.
3. Keep 3 samples aside in `voice/holdout/`. Never learn from these; use them to test the guide later.
4. If the samples span years and the style changed, ask which era sounds like them now. Use only that era.
5. Write `voice/VOICE.md` from the samples only:
   - **10 moves** they actually make, each with a real quoted line as proof (how they open, how they turn an idea with "But" or "So", how they end).
   - **Numbers:** average sentence length, average paragraph length, how often they use contractions, questions, lists.
   - **Ban list:** words that appear 0 times in the samples but show up in AI writing (start with: delve, crucial, moreover, furthermore, additionally, testament, seamless, robust, leverage, tapestry). Only ban a word if the samples never use it.
6. Test it: have the writer draft something on a holdout topic, then compare it with the real holdout text. Fix the guide where they differ.

## 2. Write (every piece)

**Writer agent** (a subagent with `voice/VOICE.md` and the source material):
- Every fact must come from the source the user gave. Nothing made up.
- Where the piece needs the user's opinion or a feeling and the source doesn't say it, write `[YOUR TAKE: what do you think about X?]`. Never guess an opinion.
- Follow the moves and numbers in VOICE.md. Never use a banned word.

**Editor agent** (a separate subagent; it never writes the draft):
- Reads VOICE.md and the draft.
- Sends back only its **top 3 fixes**, each with the line, what's wrong, and which rule it breaks. Not a rewrite.
- Also writes down, sealed, the 3 edits it predicts the user will make. Don't show these to the writer.

Loop writer → editor up to **3 rounds**, or stop early when the editor has no real fixes left.

Then give the user the draft with the `[YOUR TAKE]` blanks marked.

## 3. Learn from the user's edits

1. When the user sends back their edited version, compare it with the draft.
2. Sort each change: **voice** (wording, rhythm), **structure** (order, cuts), **fact** (something wrong) or **banned word**.
3. Check the editor's sealed predictions. Which did it get right? Which did it miss?
4. Add new rules to VOICE.md, marked `(try)`. A rule seen in only 1–2 edits stays `(try)`. Promote it only after it shows up again.

Patterns that tend to hold up:
- "But" and "So" at the turns.
- Cut a last line that just repeats the first.
- Use contractions.
- Keep the small words ("just", "really") the user actually says.

## Files

```
voice/
  samples/    pre-AI writing used to learn the voice
  holdout/    3 samples kept aside for testing
  VOICE.md    moves, numbers, ban list, learned rules
```
