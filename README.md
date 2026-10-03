# sligh-games Kiro Crew App Registry

A federated [Kiro Crew](https://github.com/kirodotdev/KiroCrew) app registry for
the `sligh-games` organization. Users opt in by adding this registry to their
Kiro Crew config; apps listed here are then installable from the dashboard App
Store under this source.

## Add this registry

Add it to your Kiro Crew `config.json` (or via **Apps -> Sources** in the
dashboard):

```json
{
  "registries": [
    {
      "name": "sligh-games",
      "repo": "https://github.com/sligh-games/kiro-app-registry",
      "branch": "main"
    }
  ]
}
```

Then refresh sources in the App Store. Apps in this registry appear in Discover.

## List an app

The registry index is `app-registry.json` at the repo root. Each entry is a row:

```json
[
  {
    "name": "lore-forge",
    "gitUrl": "https://github.com/arkaeon/lore-forge",
    "branch": "main"
  }
]
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Must match the app's `app.json` `name` exactly (kebab-case). |
| `gitUrl` | yes | Any git-cloneable URL. |
| `branch` | no | Branch to read/clone. Defaults to `main`. |
| `repo` | no | Repo identifier the blob proxy uses to serve committed images. |
| `subdirectory` | no | Path within the repo holding `app.json`, for a monorepo layout. |
| `resources` | no | `"gateway"` (default) or `"app"`: who registers agents/skills/MCP/crons. |
| `lifecycle` | no | `"gateway"` (default), `"app"`, or `"locked"`: who owns updates/uninstall. |

If no `app-registry.json` exists, Kiro Crew falls back to discovering
`apps/*/app.json` in this repo (monorepo layout -- put each app under `apps/<name>/`).

## Trust & credential notes

- Apps listed here are cloned **credential-free by default** (a confused-deputy
  defense). The **same-repo carve-out** uses owner credentials only when an
  entry's clone URL is byte-identical to this registry repo's URL (monorepo
  layout).
- Adding a registry is itself the trust decision -- there is no per-app Kiro Crew
  review on a federated registry. Only list apps this org vouches for.
- An app with any executable surface (backend, Python hooks, lifecycle scripts)
  additionally requires the installing user to enable
  **Settings -> Security -> Trusted apps**.

## Monorepo layout (optional)

To host app sources in this same repo instead of linking out, put each under
`apps/<name>/` with its own `app.json`, and either list it in
`app-registry.json` with a `subdirectory`, or rely on auto-discovery.
