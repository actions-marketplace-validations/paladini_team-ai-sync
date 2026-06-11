# Public demo walkthrough

The public demo shows `team-ai-sync` solving a concrete problem: a team wants to
keep AI collaboration files aligned across an API repository and a web
repository without copying files manually.

## Demo repositories

- [team-ai-sync](https://github.com/paladini/team-ai-sync): the GitHub Action.
- [team-ai-sync-demo-source](https://github.com/paladini/team-ai-sync-demo-source):
  the source repository with shared guidance and workflow configuration.
- [team-ai-sync-demo-api](https://github.com/paladini/team-ai-sync-demo-api):
  a target repository that started with stale API guidance.
- [team-ai-sync-demo-web](https://github.com/paladini/team-ai-sync-demo-web):
  a target repository that started with partial web guidance.

## Shared files

The source repository synchronizes:

- `AGENTS.md`
- `CLAUDE.md`
- `.editorconfig`
- `.github/instructions/code-review.md`
- `.github/instructions/security.md`
- `.github/prompts/review.prompt.md`

## Workflow evidence

The run history demonstrates the full lifecycle:

1. [Dry run with changes detected](https://github.com/paladini/team-ai-sync-demo-source/actions/runs/27315787215)
   validated configuration and detected changes without opening pull requests.
2. [Real sync run](https://github.com/paladini/team-ai-sync-demo-source/actions/runs/27315807029)
   created pull requests in both target repositories.
3. [Update run](https://github.com/paladini/team-ai-sync-demo-source/actions/runs/27315999623)
   updated the existing pull requests instead of opening duplicates.
4. [Final dry run](https://github.com/paladini/team-ai-sync-demo-source/actions/runs/27316035674)
   reported `changed=false` after the generated pull requests were merged.

## Generated pull requests

- [API demo pull request](https://github.com/paladini/team-ai-sync-demo-api/pull/1)
- [Web demo pull request](https://github.com/paladini/team-ai-sync-demo-web/pull/1)

## What to notice

- The source repository owns the shared files and `sync-config.json`.
- Each target repository receives a normal pull request.
- The same branch name is reused for updates.
- Merging the pull requests brings each target into sync.
- A later dry run reports no changes.

Use this demo as a template for presentations, onboarding, and internal rollout
planning.
