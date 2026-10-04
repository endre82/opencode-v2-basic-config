# Global instructions

## Git policy — applies to every agent and subagent

- NEVER run `git push`. Pushing is done by the user. Treat a bare "commit
  this" as commit-only; push only when the user explicitly says "push" in the
  current message.
- Plan mode and the plan agent: never commit. Planning must not create
  commits at all.
- Build mode: staging files and drafting a commit message is fine, but only
  execute `git commit` after the user has explicitly approved it in the
  current conversation.
- Subagents (engineer, developer, reviewer): never run `git commit` or
  `git push`. Finish the task and report; the parent agent and the user
  handle version control.
- Never rewrite or delete history (`--amend`, `reset --hard`, history
  rewrites, force pushes) without explicit approval in the current message.
- If a task seems to require something this policy forbids, stop and explain;
  do not work around it.