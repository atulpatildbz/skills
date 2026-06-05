---
name: learning-detour
description: >-
  Spin off a focused sub-conversation to learn a prerequisite before continuing
  with a larger topic. Use when the user is learning something and hits a
  sub-topic, term, or concept they don't yet understand and want to go learn it
  first — the classic "I need to understand X before this topic makes sense"
  situation. Trigger on phrases like "I don't actually know what X is", "this
  keeps referencing X and I'm lost", "I need to learn X first", "let me branch
  off on X", "spin off a detour on X", "make me a prerequisite briefing for X",
  or any time the user wants to pause the current topic to solidify a base
  concept it depends on. Produces a self-contained context block the user pastes
  into a NEW conversation, plus a return path back to the parent topic. This is
  NOT the handoff skill — it carries minimal focused context, not a full dump.
---

# Learning Detour

When you're learning a topic, you often hit a sub-topic it quietly assumes you
already know. You can't really understand the parent until those base pieces are
solid. This skill spins off a **focused detour**: a fresh conversation dedicated to
the one prerequisite, carrying just enough context to learn it well — and a way
back to the parent topic afterward.

This is deliberately different from the `handoff` skill. Handoff dumps the whole
conversation forward so another agent continues the *same* work. A detour goes
*sideways and back*: it carries the **minimum** context needed to learn one base
concept, so the sub-topic isn't buried under everything else. If the user actually
wants to continue the same work elsewhere, point them to `handoff` instead.

## What this skill produces

The core deliverable is a **prerequisite briefing** — a self-contained block the
user copies into a new conversation. Because a fresh conversation has zero memory
of the parent topic, the briefing must stand entirely on its own.

Produce it as a single copy-pasteable code block with these parts:

```markdown
# Learning detour: <sub-topic>

I'm learning **<parent topic>** and hit **<sub-topic>**, which I don't understand
well enough yet. I want to solidify this base concept first.

**Where it showed up:** <the specific place/claim in the parent topic that
references this — quote or paraphrase it>

**Why it matters to the parent:** <its role — what the parent topic uses it for>

**Where I'm starting from:** <what the user already knows / their current guess,
or "basically from scratch" — helps pitch the explanation at the right level>

**What I need to walk away understanding:** <the concrete success criterion — the
question that, once answered, lets me return to the parent topic>

**How to teach me:** Use the `feynman-learning` skill. Explain it clearly, then
let me explain it back so we can catch gaps. Don't assume the parent topic — teach
this as its own thing.

(When I understand this, I'll return to: **<parent topic>**.)
```

Fill every angle-bracket field from the conversation. The two fields that make a
detour work — and that a naive dump gets wrong — are **why it matters to the
parent** and **what I need to walk away understanding**. They keep the detour
scoped: the user learns enough to return, not an unbounded deep-dive that becomes
its own rabbit hole.

## How to respond when this skill activates

1. **Identify the prerequisite and its context.** From the conversation, pin down:
   the sub-topic, where it appeared in the parent topic, and what role it plays.
   If any of these is unclear, ask one quick question rather than guessing — a
   vague briefing produces a vague detour.
2. **Scope it.** A good detour is one base concept, not five. If the user names
   several unknowns at once, surface them and ask which to branch on first (or
   suggest the most foundational one — the one the others depend on).
3. **Emit the briefing block** above, ready to copy into a new conversation.
4. **Record the return path** (see below) so the parent topic isn't lost.
5. **Tell the user the move:** open a new conversation, paste the block, learn the
   sub-topic there, then come back here (or a new parent session) to resume.

## The return path: don't lose the parent topic

A detour is only useful if the user can get back. Track it so resuming is painless.

**If a `.feynman-learning-log.md` exists in the current directory** (the
`feynman-learning` skill's log), record the detour there:
- Add or update a section for the **sub-topic**, marked as a prerequisite, noting
  which parent topic it serves.
- Mark the **parent topic** as blocked-on / waiting for that prerequisite, with a
  one-line note of where to pick back up.

```markdown
## <parent topic>
Status: paused — learning prerequisite "<sub-topic>" first
Resume at: <the specific point/claim to return to>

## <sub-topic>
Status: in progress (prerequisite for "<parent topic>")
```

**If there's no log**, just state the return path inline ("when you're done, resume
the parent topic at <point>") so it's at least captured in this conversation. Don't
silently create a file if the user is working somewhere they wouldn't want one.

## Nesting: detours within detours

Prerequisites have prerequisites. While learning a sub-topic, the user may hit an
even more basic concept and want to branch again. That's fine and expected — each
detour is independent and self-contained, so just generate another briefing. If a
log is in use, the chain of "prerequisite for…" notes forms the trail back up:
learn the deepest piece first, then unwind toward the original parent topic.

## Tone

This is an encouraging, momentum-preserving move, not an admission of failure —
recognizing that you're missing a base piece *is* the skill of a good learner.
Frame it that way. Keep the briefing tight and confidence-building, and make the
path back obvious so stepping away from the parent topic never feels like losing
your place.
