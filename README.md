<div align="center">

![ManifestHub3 — the data backbone of steamtools.games](og-home.png)

# ManifestHub3

**Community-maintained Steam manifest file cache that powers [steamtools.games](https://steamtools.games/).**

🌐 **[Open steamtools.games →](https://steamtools.games/)**
· 📖 [API Docs](https://steamtools.games/developers)
· 💬 [Discord](https://discord.com/invite/FDKpJu5zgT)
· 📝 [Blog](https://steamtools.games/blog)
· 🌍 Available in 7 languages

[![Website](https://img.shields.io/badge/Website-steamtools.games-0b1220?style=for-the-badge)](https://steamtools.games/) [![API](https://img.shields.io/badge/API-Public%20%26%20Free-22c55e?style=for-the-badge)](https://steamtools.games/developers) [![Discord](https://img.shields.io/badge/Discord-Join-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.com/invite/FDKpJu5zgT) [![License](https://img.shields.io/badge/License-MIT-3b82f6?style=for-the-badge)](LICENSE)

</div>

---

> [!IMPORTANT]
> **You almost certainly don't need to clone this repo.**
> [**steamtools.games**](https://steamtools.games/) is a free, no-signup web
> tool that returns the `.lua` and `key.vdf` for any Steam App ID in under a
> second. It is the user-friendly front-end for the data in this repo.
>
> 👉 **[Try it at steamtools.games](https://steamtools.games/)** — type a game
> name or paste an App ID, then press <kbd>Ctrl</kbd>+<kbd>↵</kbd>.

---

## What is ManifestHub3?

A community-maintained cache of Steam manifest files (Lua scripts + key
files), structured the way **SteamTools** (Watt Toolkit / Steam++)
expects on disk. It is a preservation mirror of the original ManifestHub
database that was taken down on GitHub.

- **This GitHub repo** stores one Git branch per Steam App ID, each
  containing `<AppID>.lua` and `key.vdf`.
- **[steamtools.games](https://steamtools.games/)** is the user-facing
  front-end that reads from these branches via the GitHub API and serves
  ready-to-use ZIP packages.

## How the data flows

```
steamtools.games (Cloudflare Workers)
  │
  ├── GitHub Branch API
  │     GET /repos/steamtools-games/ManifestHub3/branches/{appId}
  │     → Does this App ID exist in the cache?
  │
  └── GitHub Raw Content
        GET raw.githubusercontent.com/.../ManifestHub3/{appId}/{appId}.lua
        GET raw.githubusercontent.com/.../ManifestHub3/{appId}/key.vdf
        → Fetch the actual manifest files
```

The website caches results in Cloudflare D1 (24 h for positive hits,
7 d for negatives) and in-memory LRU (5 min for built ZIP packages).

## Why use steamtools.games?

- ⚡ **Fast lookup** — type a game name or paste an App ID; results appear
  instantly when the cache is warm.
- 🔎 **Live Steam search** — the same engine Steam uses on the storefront,
  100 k+ apps including DLCs and soundtracks.
- 📦 **Manifest + Lua pair** — canonical filenames SteamTools already
  expects, drop-in ready for your installation root.
- ✅ **Storefront-verified** — every App ID is checked against the Steam
  store before files are generated.
- 🌐 **7 languages** — English, 中文, Español, Français, Русский, Türkçe,
  Português (Brasil).
- 🚫 **No signup required** — direct CDN URLs only.
- 🧰 **Free public API** — `GET /api/search` and `POST /api/generate`, no
  auth, no SDK.

## From App ID to a working manifest in 30 seconds

1. Open **[steamtools.games](https://steamtools.games/)**.
2. Type a game name (e.g. *Portal*) or paste an App ID (e.g. `400`).
3. Press <kbd>Ctrl</kbd>+<kbd>↵</kbd>. You get a `.zip` with the `.lua`
   and `key.vdf`.
4. Drop the files into your SteamTools installation root (same folder as
   the executable) and restart SteamTools. The game appears in your
   library.

> 🌀 *Portal* — `400` &nbsp;·&nbsp; 🌃 *Cyberpunk 2077* — `1091500`
> &nbsp;·&nbsp; ⚔️ *Elden Ring* — `1245620` &nbsp;·&nbsp; 🌱 *Stardew Valley* — `413150`

## Public API

The same endpoints the homepage uses are exposed as plain HTTP at
`steamtools.games`. No API key, no signup — just IP throttling
(search: one request per 300 ms, generate: one per 1.5 s).

### Search the Steam catalog

```bash
curl -s "https://steamtools.games/api/search?query=portal" | jq
```

### Generate the manifest + Lua pair

```bash
curl -s -X POST "https://steamtools.games/api/generate" \
  -H "Content-Type: application/json" \
  -d '{"appId": "400", "branch": "public"}' | jq
```

Example response:

```json
{
  "code": 0,
  "message": "ok",
  "data": {
    "appId": "400",
    "branch": "public",
    "gameName": "Portal",
    "downloadUrl": "https://steamtools.games/api/files/400/zip",
    "luaUrl":      "https://steamtools.games/api/files/400/lua",
    "keyVdfUrl":   "https://steamtools.games/api/files/400/key"
  }
}
```

Full schema, rate-limit details, and an `openapi.json` download →
[steamtools.games/developers](https://steamtools.games/developers).

## What's in this repo

| Path | What it is | Size |
| --- | --- | --- |
| `origin/<AppID>` branches | One branch per Steam App ID, holding `<AppID>.lua` and `key.vdf`. | ~62 k branches |
| `og-home.png` | The brand image used as this README's hero. | ~360 KB |
| `archive/` | **Deprecated.** Legacy JSON snapshots from the original ManifestHub. Not consumed by the website. | ~15 MB |
| `README.md` | You are here. | — |

> **⚠️ About `archive/`:** The `depotkeys.json` and `appaccesstokens.json`
> files in `archive/` are **historical artifacts** from the original
> ManifestHub project. The website at steamtools.games **does not read
> them** — it reads manifest files directly from per-App-ID Git branches.
> These files are kept for preservation only and may be removed in a
> future release.

Clone this repo only if you are integrating against the raw branch data
or running your own mirror. For day-to-day use,
[the site](https://steamtools.games/) is faster, easier, and avoids pulling
21 GB of git history.

## Contributing

We welcome contributions! See **[CONTRIBUTING.md](CONTRIBUTING.md)** for
details on how to add new App IDs, report broken files, or improve the
project.

**Quick start:**

- **Request a new App ID** → [Open an issue](https://github.com/steamtools-games/ManifestHub3/issues/new?template=request-app-id.yml)
- **Report a broken file** → [Open an issue](https://github.com/steamtools-games/ManifestHub3/issues/new?template=report-broken-file.yml)
- **Add data directly** → Create a branch named `<AppID>`, add `<AppID>.lua` and `key.vdf`, open a PR.

## Credits

- Original project: the **ManifestHub** database, preserved here after its takedown.
- Front-end & API: **[steamtools.games](https://steamtools.games/)** — the
  official user-facing tool for this data.
- Desktop client: the **SteamTools** project, which consumes these files.

---

<div align="center">

🌐 **[steamtools.games](https://steamtools.games/)** — the fastest way to get a
working SteamTools manifest.

</div>
