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

## Delegation policy — primary agents (plan, build)

Applies to the built-in primary agents `plan` and `build`. Subagents
(engineer, developer, reviewer, general, explore) ignore this section; their
shared contract governs them instead.

### plan delegates
- Drafting or restructuring an implementation plan → `engineer`.
- Reviewing a plan or a diff → `reviewer` (state which mode, pass the artifact
  verbatim).
- Multi-file research, exhaustive search, long-document reading → `explore`
  (or `general` when broader tools are needed).
- Inline only for quick lookups: single-file reads, ≤2 tool-call questions. If
  one task runs past ~4 tool calls, hand the remainder to a subagent.
- plan never implements and never launches `developer`; implementation waits
  for the build agent.

### build delegates
- Self-contained implementation (>~5 lines across files, new functions,
  components) → `developer`, passing the plan/brief verbatim with exact file
  paths and verification commands.
- build-direct only for trivial units: one-line fixes, mechanical renames,
  config tweaks, staging/committing.
- Run `reviewer` on non-trivial diffs before reporting done; act on
  CRITICAL/HIGH findings.

### Brief contract (both agents)
- The brief is self-contained: the child sees none of the parent conversation.
- Pass plans/briefs verbatim; never summarize away constraints or paths.
- The child's final message is the only return value; read it before acting.

### Failure handling
- On a failed subagent call, retry once with a sharpened brief. If it fails
  again, fall back inline only if the work is small — and say so explicitly.