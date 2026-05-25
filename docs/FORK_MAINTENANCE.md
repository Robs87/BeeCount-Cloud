# Fork Maintenance

This fork is the production source for the family cashflow workflow. Upstream
BeeCount Cloud remains the base product, while this fork keeps local
customizations that are required by Family Cashflow Radar.

## Branch Model

- `Robs87/BeeCount-Cloud:main` is the custom production branch.
- `TNT-Likely/BeeCount-Cloud:main` is the upstream source.
- Local custom features should land in the fork first, then optionally be
  proposed upstream as pull requests.
- Do not rebase or force-push `main`. Merge upstream into the fork so custom
  commits stay visible and recoverable.

## Sync Upstream

Use the `sync-upstream` GitHub Actions workflow in this fork.

What it does:

- checks out fork `main`
- fetches `TNT-Likely/BeeCount-Cloud:main`
- merges upstream into fork `main`
- pushes the result back to `Robs87/BeeCount-Cloud:main`

If upstream changes conflict with fork-only customizations, the workflow fails
instead of overwriting local behavior. Resolve the conflict in a normal branch,
test it, and merge it into `main`.

## Docker Image

The NAS should run an image built from this fork, not directly from upstream,
when fork-only behavior is required.

Use the `fork-ghcr-image` workflow to publish:

```text
ghcr.io/robs87/beecount-cloud:<tag>
```

For the current read API PAT build:

```text
ghcr.io/robs87/beecount-cloud:read-api-pat
```

NAS upgrades should keep the existing data volume and only replace the image.

## Current Fork-Only Customizations

- `read:api` PAT scope for long-lived read-only `/api/v1/read/*` clients.
- GHCR image publishing workflow for fork builds.
- Upstream sync workflow for preserving fork customizations while receiving
  upstream features.
