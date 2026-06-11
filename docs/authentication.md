# Authentication and permissions

`team-ai-sync` needs a token that can read and write target repositories. The
workflow's default `GITHUB_TOKEN` is usually not enough for cross-repository
sync because it is scoped to the source repository.

## Recommended token types

Use one of these token types:

- Fine-grained personal access token for small teams, demos, or repository
  groups with a clear owner.
- GitHub App installation token for organization-wide or long-lived
  automation.
- Classic personal access token only when a fine-grained token or GitHub App
  does not support the organization policy or workflow-file use case.

## Minimum useful permissions

Grant the token access to every target repository in `targetRepositories`.

For each target repository, grant:

- `Contents: Read and write`
- `Pull requests: Read and write`
- `Metadata: Read`

Grant this additional permission when needed:

- `Issues: Read and write` if `prOptions.labels` contains labels, because pull
  request labels use the issues API.

If you sync files under `.github/workflows/**`, your organization may require a
token that can modify workflow files. Depending on your setup, this may require
a classic PAT with the `workflow` scope.

## Source repository workflow permissions

The workflow in the source repository can keep `GITHUB_TOKEN` permissions small:

```yaml
permissions:
  contents: read
```

The source checkout uses `GITHUB_TOKEN`. Cross-repository clone, branch push,
and pull request creation use the token passed through `github-token`.

## Store the token

Store the token as an Actions secret in the source repository:

```text
TEAM_SYNC_ADMIN_PAT
```

Reference it from the workflow:

```yaml
- uses: paladini/team-ai-sync@v1
  with:
    github-token: ${{ secrets.TEAM_SYNC_ADMIN_PAT }}
    config-path: sync-config.json
```

## Rotation

Use expiration dates for personal access tokens and rotate them regularly. When
rotating a token, update the source repository secret before the old token
expires.

## Security boundaries

The token can push branches and create pull requests in every configured target
repository. Treat write access as sensitive:

- Limit token repository access to the exact targets that need sync.
- Avoid organization-wide access unless you need it.
- Use a dedicated automation identity when possible.
- Keep `targetRepositories` reviewed like production configuration.
