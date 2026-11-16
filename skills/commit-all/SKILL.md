---
name: commit-all
description: Use to commit all uncommitted work as logically separated commits with short messages, authored by the local git identity, through a dedicated commit-organizer subagent. Never pushes.
---

Commit the uncommitted work through a dedicated `commit-organizer` subagent. Do not make the commits yourself, unless the `subagent` tool is missing (see step 5).

The `commit-organizer` agent is defined in `agents/commit-organizer.md` in this package. If `subagent({ action: "list" })` does not show it, install it with `ln -s <superpowers clone>/agents/*.md ~/.pi/agent/agents/`.

1. The target is every repository path the user named, or the current project's repository if none is named.
2. Launch exactly one `commit-organizer` with the `subagent` tool, and wait for it.
3. Its task contains only: the repository path or paths, and any instruction the user or the calling skill explicitly gave, such as files to leave uncommitted. Add other context only if explicitly asked to pass it on; then write it as a short, self-contained brief covering just that.
4. Do not include the conversation history, a summary of the work done in this session, your opinions about the changes, or proposed commit messages. The subagent reads the diff and decides the grouping and messages itself.
5. If the `subagent` tool is missing, read `agents/commit-organizer.md` and follow it yourself, in this session, with the same rules. Do not skip it, and do not commit by any other route.
6. When it returns, give the user its table of commits, the author and trailer check, and the list of what was left uncommitted. Do not push. If a correction is needed, resume the same subagent and send only the correction.

The author is the local git identity, read from `git config` in the target repository at run time. Never set it by hand.
