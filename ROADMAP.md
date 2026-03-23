# Roadmap

Current state of groundwork and where it's headed. If you're picking something up, claim it in a PR or thread so we don't duplicate effort.

## Known issues

### Pairing-buddy gets spoiled by the architect's plan

The architect was writing full implementation details into the plan, and since pairing-buddy loads that plan as its primary context, it would just hand the developer the answer instead of guiding them toward it.

**Current workaround:** The architect writes two plans -- one with developer-facing guidance (hints, questions, pseudocode) and another with machine-readable implementation detail.

**Better solution:** Rethink what the architect writes to disk vs. what pairing-buddy is allowed to see. Possibly a single plan with clearly separated sections and instructions in pairing-buddy's prompt to skip certain parts.

### No seamless handoffs between skills

Each skill works standalone, but the intended workflow (ticket-analysis -> architect -> pairing-buddy -> local-review) requires manually invoking each step. Hooks should allow one skill to pass off to the next once the developer confirms they're ready.

**What's needed:** Learn and implement Claude Code hooks to chain skills together. The handoff should still be opt-in (the developer confirms before moving on), not automatic.

## Ideas

### Codebase-aware defaults for the architect

Since this plugin is primarily used in the Boundless codebase, the architect could benefit from some opinionated guidance baked in (e.g. preference for service objects, lean controllers, specific backend patterns). These should eventually live in the project's CLAUDE.md rather than in the plugin, but hardcoding a few temporarily could improve plan quality while we figure out the right boundary.

### Hooks for natural skill transitions

Rather than telling the developer "now run /architect", the brainstorm session in ticket-analysis could offer to hand off to architect once all A/C is confirmed. Same for architect -> pairing-buddy once a plan is written.

## Contributing

Pick something from above, or open an issue if you hit friction using any of the skills. The goal is a workflow that helps developers think through problems without being spoon-fed -- keep that in mind when tuning prompts.
