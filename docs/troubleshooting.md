# Troubleshooting

Use this guide to diagnose common `team-ai-sync` failures.

## `Resource not accessible by integration`

The token cannot access the target repository or does not have the required
permission.

Check:

- The token is stored in the source repository secret used by the workflow.
- The token can access every repository in `targetRepositories`.
- The token has `Contents: Read and write`.
- The token has `Pull requests: Read and write`.
- The token has `Issues: Read and write` when labels are configured.

## `Configured file does not exist`

A path listed in `files` does not exist in the source repository.

Check:

- The file path is spelled correctly.
- The file is committed to the source repository.
- The path is relative to `source-root`.
- The path points to a file, not a directory.

## `Configured directory does not exist`

A path listed in `directories` does not exist in the source repository.

Check:

- The directory path is spelled correctly.
- The directory is committed to the source repository.
- The path is relative to `source-root`.
- The path points to a directory, not a file.

## Unsafe path errors

The action rejects unsafe paths before target processing.

Do not use:

- absolute paths
- paths containing `..`
- paths containing `.git`
- machine-local paths such as `C:\Users\...`

Use repository-relative paths such as:

```json
{
  "files": ["AGENTS.md"],
  "directories": [".github/prompts"]
}
```

## No pull request was created

This can be the expected result.

Check:

- The workflow output `changed`.
- The logs for each target repository.
- Whether target files already match the source files.
- Whether `dry-run` is set to `true`.

When `changed=false`, the action does not push a branch or create a pull
request.

## Labels were not applied

Labels use the issues API.

Check:

- The token has `Issues: Read and write`.
- Label names in `prOptions.labels` are spelled correctly.
- Organization policies allow the token to manage labels.

GitHub creates missing labels in some contexts, but repository and token policy
can still block label operations.

## Reviewers were not requested

Check:

- User reviewers are valid GitHub usernames.
- Team reviewers are valid team slugs.
- The token can access the organization teams.
- The target repository allows reviewer requests from the token identity.

For team reviewers, both `platform` and `org/platform` are accepted. The action
uses the team slug.

## Workflow files were rejected

If you sync files under `.github/workflows/**`, GitHub may require additional
token permissions.

Options:

- Remove `.github/workflows/**` from the sync config.
- Use a token with workflow-file permission.
- Use a classic PAT with the `workflow` scope when your organization requires
  it.

## A target fails but other targets continue

This is expected. Each target repository is processed independently.

Check the `failed-targets` output for the failing repositories and error
messages. Successful targets are still reported in `synced-targets`.

## The workflow cannot find `sync-config.json`

Check:

- The file is committed to the source repository.
- The `config-path` input points to the correct path.
- The workflow checks out the source repository before running the action.
- The file path is repository-relative.
