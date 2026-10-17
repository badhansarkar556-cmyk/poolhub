# 🏛️ PoolHub

Centralized proxy management for the Stremio addon family — one service that harvests, verifies and serves proxies to every client app through isolated, keyed sub-spaces.

```
harvest (public lists) → alive pool (cap 300) → 5 sub-spaces (condition-pass = benched)
   → per-space unique API key → client apps fetch with their own key
```

## Sub-spaces

| Space | Condition | Client |
|---|---|---|
| `toonworld4all` | robots + `wp-admin` signature | resolver bridge |
| `raretoons` | robots + `wp-admin` signature | resolver bridge |
| `vidy` | seed API answers `"seed"` | vidy addon |
| `moviebox` | signed POST → `x-user` JWT (token collection, 83% pass) | vidy addon (MovieBox) |
| `multimovies` | resolve-JSON answers | vidy addon |

## Proxy policy

Proxies are for **IP/geo-flag bypass only** (tokens, API calls, robots, small manifest text — 256KB body cap). **Media playback never goes through a proxy**; players always stream CDN-direct.

## API

| Route | Description |
|---|---|
| `GET /health` | pool + spaces + usage counters |
| `GET /spaces?key=` | key = space selector (only its own space); admin token sees all |
| `POST /register` | app declares its space condition (key or `src`) |
| `GET /fetch?src=&url=&key=` | fetch a URL through the space's best exits |
| `GET /pick?src=&n=&key=` | the space's best proxies (default 12, cap 40) |
| `POST /report` | client proof feed (`px`, `ok`, `ms`, `src`) |
| `POST /token` (admin) | token user → `u:<name>` space + `/my/*` routes |

Admin token: `POOLHUB_ADMIN_TOKEN` env or auto-generated `poolhub_admin.txt`.

## Run

```
PORT=8399 node server.js        # local
```

Scaling knobs (v1.3.7): checker wave = 80 proxies every 8s; enrichment every 90s (600 candidates, 200 batches); `/pick` cap 40.

## License

MIT — see [LICENSE](LICENSE).
