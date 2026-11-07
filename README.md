# write-like-me

A Claude skill that writes posts, emails and scripts that sound like you, not like AI.

It learns your voice from things you wrote before AI, then one agent writes and a separate agent edits (top 3 fixes, up to 3 rounds). Where it needs your opinion, it leaves a `[YOUR TAKE]` blank instead of guessing.

Method from Nat Eliason: https://x.com/nateliason/status/2115803497773904292

## Install

**Claude Code** (one line in your terminal):

```bash
git clone https://github.com/sullyo/write-like-me ~/.claude/skills/write-like-me
```

**Claude app (claude.ai / desktop):** download [SKILL.md](SKILL.md), zip it, then go to Settings → Capabilities → Skills → Upload skill.

## Use it

1. Gather 10–30 things you wrote before AI (posts, emails, newsletters).
2. Tell Claude: "set up my voice" and paste them in.
3. Then: "write a post about ___ in my voice".
4. Fill in the `[YOUR TAKE]` blanks yourself, and send Claude your final edit so it keeps learning.
