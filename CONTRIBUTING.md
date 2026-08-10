# Contributing to ManifestHub3

First off, thanks for taking the time to contribute! 🎉

This repository is a **data cache** — not a software project. Most
contributions are either adding new Steam App IDs or reporting broken
files.

## How the data is structured

Each Steam App ID lives in its own Git **branch** (not a folder):

```
Branch name: 400          (the Steam App ID for Portal)
  └── 400.lua             (the Lua script SteamTools expects)
  └── key.vdf            (the decryption key file)
```

The website at [steamtools.games](https://steamtools.games/) reads these
files via the GitHub Raw Content API — it does **not** read any JSON
files from the `main` branch.

## Ways to contribute

### 1. Request a new App ID

Use the **"Request new App ID"** issue template:
[Open a request →](https://github.com/steamtools-games/ManifestHub3/issues/new?template=request-app-id.yml)

Or join our [Discord](https://discord.com/invite/FDKpJu5zgT) — new App
IDs are usually added within a few days.

### 2. Report a broken or missing file

If a file download from steamtools.games is empty or corrupt, use the
**"Report broken file"** issue template:
[Report a problem →](https://github.com/steamtools-games/ManifestHub3/issues/new?template=report-broken-file.yml)

### 3. Add data directly via PR

1. **Create a branch** named exactly after the Steam App ID (e.g. `400`).
2. Add two files to the branch:
   - `<AppID>.lua` — the Lua manifest script.
   - `key.vdf` — the key file in Valve's VDF format.
3. Open a Pull Request targeting `main` with:
   - Title: `Add App <AppID> — <Game Name>`
   - A link to the Steam store page (`store.steampowered.com/app/<AppID>`)
4. A maintainer will review and merge.

### Branch naming rules

| Rule | Example |
| --- | --- |
| Branch name = numeric Steam App ID | `400`, `730`, `1245620` |
| No prefixes, no descriptions | ✅ `400` · ❌ `app-400` · ❌ `add-portal` |
| One App ID per branch | ❌ Don't put multiple apps in one branch |

### File format requirements

**`<AppID>.lua`** — a plain-text Lua script. Non-empty.

**`key.vdf`** — Valve's KeyValues format. Must contain at least a
`depotid` and `decryptionkey` field.

```vdf
"depotid" "400"
"decryptionkey" "a1b2c3d4e5f6..."
```

## Code of conduct

Be respectful. This is a community project run by volunteers.

## Questions?

Join our [Discord](https://discord.com/invite/FDKpJu5zgT) for any
questions not covered here.
