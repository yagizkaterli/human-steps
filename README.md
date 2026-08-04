# human-steps

A Claude Code skill: **never bury what you need from the human.**

The quiet failure mode of long AI-assisted sessions: the assistant does 95% of the
work, and the 5% only the human can do is mentioned once, mid-paragraph, and lost.
Days later the pipeline is mysteriously stuck — on a click nobody surfaced properly.

When completion, continuation, approval, safety, or delivery depends on a human-only
action, this skill surfaces it as an explicitly flagged, numbered, concrete step list —
each step to the level of a command, click, or path, with the evidence to expect —
instead of leaving it in prose. It does **not** fire on optional suggestions or on work
the assistant can do itself.

See `SKILL.md` for the full rule. Generic by design.

## Install

Copy `SKILL.md` into `.claude/skills/human-steps/`.

## License

MIT.
