# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) preset for my repos. One file,
`default.json`, holds the dependency update policy so it is not copied into
every repo.

## Use

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>chainringlabs/renovate-config"]
}
```

Keep only repo-specific rules (grouping of paired base images, disabled
packages, manager settings) in the repo's own config.

## Policy

| Area | Rule |
|------|------|
| Cooldown | A release must be 7 days old before Renovate raises it. |
| Transitive dependencies | Renovate passes the same 7 days to npm, pnpm and Cargo when it regenerates lockfiles. uv is not supported; set `exclude-newer` in `pyproject.toml`. |
| Docker images | Only Docker Hub reports a release date. Images on GHCR, Quay and `cgr.dev` are raised without an age check. |
| Security fixes | PR opens immediately; automerges once the fix is 7 days old. Merge by hand to take it sooner. |
| Automerge | Minor, patch and digest updates, once aged and green. Never majors. |
| Lockfile maintenance | Weekly, not automerged. |
| Pinning | GitHub Actions and Docker images are pinned by digest. Actions carry their full version (`# v7.0.1`), so later updates have a release date and wait 7 days. The pin itself does not wait. |

## Changing the policy

A change here applies to every repo that extends the preset on its next
Renovate run. Changes go through a PR; CI runs `renovate-config-validator`.
Nothing in this repo is automerged.

To validate locally:

```sh
npx --yes --ignore-scripts --package renovate@44.236.3 -- \
  renovate-config-validator --strict --no-global default.json renovate.json
```
