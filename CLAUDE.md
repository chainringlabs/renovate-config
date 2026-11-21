# renovate-config

## Overview
Shared Renovate preset (`default.json`) extended by every tgrecojr repo. It sets
the dependency update policy: cooldown, pinning, automerge rules.

## Tech Stack
- Language: JSON (Renovate configuration)
- Framework: Renovate, hosted Mend app
- Database: none

## Commands
- `npx --yes --ignore-scripts --package renovate@44.236.3 -- renovate-config-validator --strict --no-global default.json renovate.json` — validate the preset

## Architecture
- `default.json` — the preset; other repos extend `github>chainringlabs/renovate-config`
- `renovate.json` — this repo's own Renovate config; extends the preset, automerge off
- `.github/workflows/validate.yml` — runs the validator on every PR and push to main

A change to `default.json` changes update behaviour in every repo at once.
Never automerge here, and validate before pushing. Repo-specific rules belong
in that repo's own config, not in the preset.

## Environment Variables
None.
