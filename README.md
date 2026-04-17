# gm1

Multiplayer Joust-style game served at `erd.mmec.ca/gm1/`.

## Architecture

- Static game client: `_site/gm1/index.html` (HTML/JS/Canvas, inline).
- Worker entry: `src/worker.js` — serves static assets and routes WebSocket signaling.
- Durable Object: `src/signaling-do.js` (`SignalingRoom`) — in-memory WebSocket signaling for WebRTC peer discovery and signal relay.
- Route: `erd.mmec.ca/gm1/*` on zone `mmec.ca`.

## Deploy

Pushes to `main` auto-deploy via GitHub Actions (`.github/workflows/deploy.yml`).
Required repo secrets: `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID`.

Manual deploy:
```
npx wrangler@4 deploy
```

## Signaling endpoint

`wss://erd.mmec.ca/gm1/api/signal/ws?peerId=<id>[&room=<code>]`

Peers without a `room` param are auto-grouped by WAN IP (IPv4 full, IPv6 /48 prefix).
