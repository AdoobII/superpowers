---
name: review-comments
description: Use before committing, or when asked to review comments and docs. Reviews comments, docstrings and docs in a diff with dedicated comment-reviewer subagents and rewrites them to be short, plain, direct and about the code only.
---

Run a comment and documentation review through dedicated `comment-reviewer` subagents. Do not do the review yourself, unless the `subagent` tool is missing (see the last step).

The `comment-reviewer` agent is defined in `agents/comment-reviewer.md` in this package. If `subagent({ action: "list" })` does not show it, install it with `ln -s <superpowers clone>/agents/*.md ~/.pi/agent/agents/`.

1. Work out the review target from the user's request: unstaged and untracked changes, staged changes, staged and unstaged together, a branch diff against a base branch, the last N commits, or named files. If the request names no target, ask the user which one, and for the base branch or N if needed. Use the current project's repository unless other paths are given.
2. Get the list of files in the target and their diff sizes (`git diff --name-only`, `git diff --stat` and the untracked files). Do not read file contents. Leave out generated files, lock files and vendored code.
3. If there are 6 files or fewer, launch exactly one `comment-reviewer` with the `subagent` tool, and wait for it.
4. If there are more than 6 files, split them into groups and launch one `comment-reviewer` per group, with at most 4 running at the same time:
   - one docs group with every Markdown and documentation file;
   - code groups of at most 4 files each, balanced by diff size.
5. Each task contains only: the target, the repository path or paths, "edit in place" unless the user asked for proposals only, and for a split run the group name ("docs group" or "code group") with its exact file list and the instruction to review only those files. Add extra context only if the user explicitly asks you to pass something on; then write it as a short, self-contained brief covering just that.
6. Do not include the conversation history, a summary of the work done in this session, your opinions about the changes, or file contents. The subagents discover everything themselves from the repository.
7. When they return, give the user one short merged summary: the files edited, the kept, rewritten and deleted counts, the sections cut or moved, and the statements flagged as inaccurate. Do not redo or second-guess their work. If a correction is needed, resume the same subagent and send only the correction.
8. If the `subagent` tool is missing, read `agents/comment-reviewer.md` and follow it yourself, in this session, with the same rules and the same report. Do not skip the review.
