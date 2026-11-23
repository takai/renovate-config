# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) preset for my repositories.

## Usage

Add this to the `renovate.json` of a repository:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>takai/renovate-config"]
}
```

Renovate loads `default.json` from this repository when no preset name is given.

## What the preset does

All times are in `Asia/Tokyo`.

| Setting | Value | Effect |
| --- | --- | --- |
| `extends` | `config:recommended` | Starts from Renovate's recommended defaults. |
| `schedule` | `* * * * 1-5` | Renovate opens and updates PRs Monday to Friday. |
| `minimumReleaseAge` | `7 days` | A release must be 7 days old before Renovate proposes it. |
| `internalChecksFilter` | `strict` | No PR is created until the release age check passes. |
| `automergeSchedule` | `* 6-9 * * 6` | Automerge only runs on Saturday, 06:00–09:59. |
| `automergeType` | `pr` | Renovate merges the PR itself. |
| `platformAutomerge` | `false` | GitHub's native automerge is not used, so merges only happen inside the automerge window. |
| `prConcurrentLimit` | `10` | At most 10 Renovate PRs are open at once. |
| `dependencyDashboard` | `true` | A Dependency Dashboard issue tracks pending updates. |

### Package rules

- **Minor and patch updates** are automerged during the Saturday window, provided status checks pass.
- **Major updates** are never automerged and need a manual review and merge.

## Overriding

Settings in a repository's own `renovate.json` take precedence over this preset. For example, to opt a repository out of automerge:

```json
{
  "extends": ["github>takai/renovate-config"],
  "automerge": false,
  "packageRules": [
    { "matchUpdateTypes": ["minor", "patch"], "automerge": false }
  ]
}
```

## Validating changes

```sh
npx --yes --package renovate -- renovate-config-validator default.json
```
