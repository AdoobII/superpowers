# The Commit Gate

Every commit made through a Superpowers workflow passes these steps, in order. Never run `git commit` directly.

1. **Review.** Follow `superpowers:review-comments` (`skills/review-comments/SKILL.md`) on the staged and unstaged changes (target: staged and unstaged together). Wait for its report.
2. **Re-check.** If the reviewer edited files, run the test suite again and read the output. The edits are comment-only, so the result should not change. If it does, stop and find out why before committing.
3. **Commit.** Follow `superpowers:commit-all` (`skills/commit-all/SKILL.md`), which runs the `commit-organizer` subagent. Pass it any files that must stay out of the commit (for example a plan file or workspace that is not part of this change).

Both skills use the `subagent` tool. If it is missing, follow the same two skills inline in this session, with the same rules. Do not skip the gate.

The agents ship in this package's `agents/` directory. `package.json` lists it for `pi-subagents`. If Superpowers is loaded as a plain extension and the agents are not listed, link them once: `mkdir -p ~/.pi/agent/agents && ln -s <superpowers clone>/agents/*.md ~/.pi/agent/agents/`.
