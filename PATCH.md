# Peekaping patched fork

This fork carries the following patches on top of the upstream `0.0.46` tag:

1. Upstream PR [0xfurai/peekaping#260](https://github.com/0xfurai/peekaping/pull/260), which fixes empty stats/points data when Peekaping runs against PostgreSQL. Upstream `0.0.46` uses an UPDATE then RowsAffected check before INSERT; with PostgreSQL that can report `-1`, so the `stats` table is never populated. PR #260 replaces that path with an atomic `INSERT ... ON CONFLICT` upsert.
2. Upstream PR [0xfurai/peekaping#270](https://github.com/0xfurai/peekaping/pull/270), which fixes the monitor detail header so TCP/DNS monitors show the configured target instead of an empty target after the monitor type.

The combined image tag is `0.0.46-patch260-ui-target1`.

## Rebase on a new upstream tag

1. Fetch upstream tags: `git fetch upstream --tags`.
2. Create a new patch branch from the new tag: `git checkout -b patch/pg-stats-260-ui-target1-<tag> <tag>`.
3. Cherry-pick PR #260 again if it has not landed upstream: `git cherry-pick 0.0.46..pr-260-source` or cherry-pick the PR commit SHA.
4. Cherry-pick PR #270 again if it has not landed upstream: `git cherry-pick <monitor-header-fix-commit>`.
5. Push the patch branch and let `.github/workflows/patched-build.yml` publish GHCR images with a new immutable tag.
6. Update the cluster image tags, then revert each patch back to upstream images once its PR ships in an upstream release.
