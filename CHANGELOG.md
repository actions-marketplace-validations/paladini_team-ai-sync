# Changelog

## v1.0.0

Initial stable release of `team-ai-sync`.

### Added

- JavaScript GitHub Action running on Node 24.
- `sync-config.json` contract for target repositories, files, directories,
  exclusions, sync mode, orphan deletion, and pull request options.
- File sync and recursive directory sync.
- `overwrite` and `skip` sync modes.
- Optional orphan deletion inside configured synced directories.
- Safe repository-relative path validation.
- Per-target processing with independent failure reporting.
- Pull request creation and update per target repository.
- Label and reviewer support for generated pull requests.
- `dry-run` mode for validation without branch pushes or pull requests.
- JSON outputs for pull request URLs, synced targets, failed targets, and
  change detection.
- Public demo repositories and run evidence.
- Versioned documentation under `docs/`.
