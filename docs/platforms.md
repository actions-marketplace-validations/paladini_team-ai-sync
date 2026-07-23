# Platform packages

`team-ai-sync` ships in the native CI/CD packaging format for each supported
platform:

- GitHub Actions for GitHub repositories.
- GitLab CI/CD Components for GitLab projects.
- Bitbucket Pipes for Bitbucket repositories.

Cross-platform sync is not the supported operating model. The GitHub package is
documented and tested for GitHub repositories, the GitLab package for GitLab
projects, and the Bitbucket package for Bitbucket repositories.

## GitHub Actions

Use the stable GitHub Action tag:

```yaml
- uses: paladini/team-ai-sync@v1
  with:
    github-token: ${{ secrets.TEAM_SYNC_ADMIN_PAT }}
    config-path: sync-config.json
```

GitHub target repositories use `owner/repo` values in `targetRepositories`.

## GitLab CI/CD Component

Use the component from a GitLab source project:

```yaml
include:
  - component: gitlab.com/paladini/team-ai-sync/team-ai-sync@1.0.0
    inputs:
      config-path: sync-config.json
```

By default the component reads the token from a CI/CD variable named
`GITLAB_TOKEN`. To use a different variable name:

```yaml
include:
  - component: gitlab.com/paladini/team-ai-sync/team-ai-sync@1.0.0
    inputs:
      config-path: sync-config.json
      token-variable-name: TEAM_SYNC_GITLAB_TOKEN
```

GitLab target projects may use `group/project` or `group/subgroup/project`
values in `targetRepositories`. The component creates or updates merge requests.

To publish the component in the GitLab CI/CD Catalog, mirror this repository to
GitLab, enable the **CI/CD Catalog project** setting, and create a SemVer tag.
The tag pipeline uses the `release:` keyword to publish the catalog release.

## Bitbucket Pipe

Use the pipe from `bitbucket-pipelines.yml`:

```yaml
pipelines:
  default:
    - step:
        name: Sync AI Assets
        script:
          - pipe: paladini/team-ai-sync:1.0.0
            variables:
              BITBUCKET_USERNAME: $BITBUCKET_USERNAME
              BITBUCKET_TOKEN: $BITBUCKET_TOKEN
              CONFIG_PATH: 'sync-config.json'
```

Bitbucket target repositories use `workspace/repo` values in
`targetRepositories`. The pipe creates or updates pull requests.

Define both `BITBUCKET_USERNAME` and `BITBUCKET_TOKEN`. `BITBUCKET_TOKEN`
should be a Bitbucket API token with repository read/write and pull request
access. Bitbucket app passwords are not recommended for new setups.

The pipe can be used from its public Bitbucket repository and tag. Appearance in
the public Bitbucket integrations list depends on Atlassian's pipe review and
curation process.

## OCI image

GitLab and Bitbucket wrappers run the same versioned OCI image. The image is
published to both registries:

```text
paladini/team-ai-sync:<semver>
ghcr.io/paladini/team-ai-sync:<semver>
```

- **Docker Hub** — published from Bitbucket Pipelines on git tags (existing flow).
- **GHCR** — published from GitHub Actions (`.github/workflows/docker-publish.yml`)
  on `v*` tags and via `workflow_dispatch`. When `DOCKERHUB_USERNAME` /
  `DOCKERHUB_PASSWORD` secrets are set on GitHub, the same workflow also pushes
  to Docker Hub; otherwise Docker Hub remains Bitbucket-only.

Pin production usage to a SemVer tag. Avoid relying on mutable image tags for
team-wide automation.

### GHCR visibility

The first GHCR package may be created as private. To make it public:

1. Open [github.com/users/paladini/packages](https://github.com/users/paladini/packages)
   (or **Packages** on the repository page).
2. Select `team-ai-sync`.
3. **Package settings** → **Change visibility** → **Public**.
