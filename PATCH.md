# Peekaping PostgreSQL stats patch

This fork carries upstream PR [0xfurai/peekaping#260](https://github.com/0xfurai/peekaping/pull/260) on top of the upstream `0.0.46` tag.

The patch fixes empty stats/points data when Peekaping runs against PostgreSQL. Upstream `0.0.46` uses an UPDATE then RowsAffected check before INSERT; with PostgreSQL that can report `-1`, so the `stats` table is never populated. PR #260 replaces that path with an atomic `INSERT ... ON CONFLICT` upsert.

## Rebase on a new upstream tag

1. Fetch upstream tags: `git fetch upstream --tags`.
2. Create a new patch branch from the new tag: `git checkout -b patch/pg-stats-260-<tag> <tag>`.
3. Cherry-pick PR #260 again if it has not landed upstream: `git cherry-pick 0.0.46..pr-260-source` or cherry-pick the PR commit SHA.
4. Push the patch branch and let `.github/workflows/patched-build.yml` publish GHCR images.
5. Update the cluster image tags, then revert to upstream images once PR #260 ships in an upstream release.
