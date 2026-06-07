# Effit community manifest

The remotely-tunable config for [Effit](https://github.com/ThomPetrus/adventure-app)'s
community-funded free tier. The app fetches `community-manifest.json` at launch
(network → cached copy → bundled defaults — a failed fetch never blocks anything).

## Fields (`schemaVersion: 1`)

| Field | Meaning |
|---|---|
| `subscriberCount` | Current supporter count, shown in the community strip |
| `freeDailySpins` | Free tier's daily spin allowance (`0` = floor state: Travel only) |
| `subscriberDailySpins` | Supporter tier's daily allowance (never lowered, only raised) |
| `goal` | `null`, or `{ "target": n, "unlocks": { "freeDailySpins": n, "subscriberDailySpins": n } }` |

Allowances are positive integers (`freeDailySpins` may be `0`); unknown
`schemaVersion`s are ignored by the app (it falls back to its bundled defaults),
so breaking shape changes must bump the version.

## Editing rules

An allowance/milestone change is **one atomic action** — edit this JSON,
recompute the cap stage table, and apply the matching GCP quota caps together
(see the app repo's `docs/ops/quota-runbook.md`). Git history is the audit log.
