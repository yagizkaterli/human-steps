---
name: human-steps
description: >
  A standing rule for AI assistants working with a human operator: trigger only when
  completion, continuation, approval, safety, or delivery of the user's requested outcome
  depends on a human-only action (an approval, a vote, a manual step, a payment decision,
  a send action). When it does, surface it as an explicitly flagged, numbered, concrete
  step list — never buried in prose. Not every reply that mentions the human qualifies:
  see "What does NOT trigger this" below.
---

# human-steps — never bury what you need from the human

The quiet failure mode of long AI-assisted sessions: the assistant does 95% of the work,
and the 5% only the human can do is mentioned once, mid-paragraph, and lost. Days later
the pipeline is mysteriously stuck — on a click nobody surfaced properly.

## The trigger

This skill fires when the requested outcome cannot complete, continue, get approved, stay
safe, or ship *without* a human-only action. It does not fire on every reply that leaves
the human aware of something.

**What does NOT trigger this:**

- An optional suggestion is not a human action — it isn't gating anything.
- Work the assistant can do itself must be done, not delegated into a block.
- A future possibility ("you might eventually want to...") is not a pending action.

## The rule

When the trigger condition is met, end the reply with a **HUMAN-ACTION** block:

1. **Flag it.** A visible header (`HUMAN-ACTION` / `YOUR MOVE`), never inline prose.
2. **Number the steps.** Each step concrete to the level of a command, a click, or a file
   path — "set up the key" is not a step; "run `ssh-keygen -t ed25519 -f ~/.ssh/foo`" is.
3. **State the expected evidence.** For each step, what the human should SEE if it worked
   (output line, green check, file that now exists) — so success needs no interpretation.
4. **Separate the sides.** What the assistant can do itself never appears in the human
   block. If you can run it, run it; only the genuinely human-gated part crosses over.
5. **Say what unblocks you.** One line: "once you paste X / approve Y, I continue with Z."

An assistant request without steps is an incomplete delivery — treat it as a bug in your
own output, not a style choice.

## Hardening rules

- **Never emit an empty HUMAN-ACTION block.** If nothing currently gates on the human,
  don't produce the header just to have one.
- **Optional suggestions are not human actions** unless the requested outcome depends on
  them — see "What does NOT trigger this" above.
- **For irreversible or high-stakes actions, state the consequence before the command.**
  The human should know what a step costs before they run it, not after.

## Anti-patterns this replaces

- "You'll need to configure the API key at some point." (when? where? how?)
- Three human-actions scattered across ten paragraphs.
- Steps the assistant could have executed itself, delegated to the human.
- "Let me know when it's done" with no way for the human to verify "done".

## Configuration

- **Block label**: pick a house style (`HUMAN-ACTION`, `YOUR MOVE`, `OPERATOR STEPS`).
- **Escalation ledger** (optional): append every human-action block to a pending-actions
  file so nothing silently expires between sessions.

## Optional: per-action IDs (multi-stage systems)

For agent systems where a pending-actions ledger tracks state across sessions, tag each
block with an ID instead of a bare header:

```
HUMAN-ACTION / HA-017
STATUS: OPEN
BLOCKS: package publication
```

Not needed for a single-session assistant — v1 simplicity is a feature. Add it only when
something else (a ledger, a dashboard) actually reads `STATUS` and `BLOCKS`.

---
*Origin: sealed as an internal law of the HERAKLES multi-agent system (2026-07-17) after a
session where the human operator asked: "if you expect something from me, explain the
steps." Trigger narrowed and hardening rules added after an external cross-family review
(2026-07-18). Generic by design — it exposes no internal method. License: MIT.*
