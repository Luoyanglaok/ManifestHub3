<div align="center">

![ManifestHub3 — the data backbone of steamtools.games](og-home.png)

# ManifestHub3

**The community-maintained Steam depot-key cache that powers [steamtools.games](https://steamtools.games/).**

🌐 **[Open steamtools.games →](https://steamtools.games/)**
· 📖 [API Docs](https://steamtools.games/developers)
· 💬 [Discord](https://discord.com/invite/FDKpJu5zgT)
· 📝 [Blog](https://steamtools.games/blog)

</div>

---

> [!IMPORTANT]
> **You almost certainly don't need to clone this repo.**
> [**steamtools.games**](https://steamtools.games/) is a free, no-signup, no-ads web
> tool that returns the `.manifest` + `.lua` (and `key.vdf`) for any Steam App ID
> in under a second. It is the user-friendly front-end for the data in this repo.
>
> 👉 **[Try it at steamtools.games](https://steamtools.games/)** — type a game
> name or paste an App ID, then press <kbd>Ctrl</kbd>+<kbd>↵</kbd>.

---

## What is ManifestHub3?

A complete, community-maintained cache of Steam depot decryption keys and app
access tokens, structured the way **SteamTools** expects on disk. It is a
preservation mirror of the original ManifestHub database that was taken down on
GitHub.

- **This GitHub repo** is the raw data layer.
- **[steamtools.games](https://steamtools.games/)** is the user-facing front-end
  built on top of it.

Pick whichever interface fits what you are doing.

## Why use steamtools.games?

- ⚡ **Sub-second lookup** — type a game name or paste an App ID; results in under 300 ms.
- 🔎 **Live Steam search** — the same engine Steam uses on the storefront, 100k+ apps including DLCs and soundtracks.
- 📦 **Manifest + Lua pair** — canonical filenames SteamTools already expects, drop-in ready for `depotcache/`.
- ✅ **Storefront-verified** — every App ID is checked against the Steam store before files are generated.
- 🌐 **English & 中文** — both locales supported, paths and dates auto-localize.
- 🚫 **No signup, no ads, no shady redirects** — direct CDN URLs only.
- 🧰 **Free public API** — `GET /api/search` and `POST /api/generate`, no auth, no SDK.

## From App ID to a working manifest in 30 seconds

1. Open **[steamtools.games](https://steamtools.games/)**.
2. Type a game name (e.g. *Portal*) or paste an App ID (e.g. `400`).
3. Press <kbd>Ctrl</kbd>+<kbd>↵</kbd>. You get a `.zip` with the `.manifest`,
   `.lua`, and `key.vdf`.
4. Drop the files into your SteamTools `depotcache/` (or installation root) and
   restart SteamTools. The game appears in your library.

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
| `depotkeys.json` | Depot decryption keys for every App ID tracked by the project. | ~16 MB |
| `appaccesstokens.json` | App access tokens required to request protected depot manifests. | ~182 KB |
| `origin/<AppID>` branches | One branch per App ID, holding that app's manifest files. | ~62k branches |
| `og-home.png` | The brand image used as this README's hero. | ~360 KB |
| `README.md` | You are here. | — |

Clone this repo only if you are integrating against the raw dataset or running
your own mirror. For day-to-day use,
[the site](https://steamtools.games/) is faster, easier, and avoids pulling
21 GB of git history.

## Credits

- Original project: the **ManifestHub** database, preserved here after its takedown.
- Front-end & API: **[steamtools.games](https://steamtools.games/)** — the
  official user-facing tool for this data.
- Desktop client: the **SteamTools** project, which consumes these files in `depotcache/`.

---

<div align="center">

🌐 **[steamtools.games](https://steamtools.games/)** — the fastest way to get a
working SteamTools manifest.

</div>
