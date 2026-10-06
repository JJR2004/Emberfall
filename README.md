# Emberfall

A browser-based, cooperative, real-time deckbuilding game. Original game identity and content; inspired by the structure of roguelike deckbuilders.

## Local development

Requirements: Node.js 20+.

```bash
npm install
npm run dev
```

Open http://localhost:5173. Open two tabs, create a room in one, and join with the room code in the other.

## Production: easiest option

This repository is configured as a single web service: Express serves the built browser client and Socket.IO serves multiplayer traffic from the same origin. That means you only need one deployment and players only need one URL.

### Render

1. Push this folder to a GitHub repository.
2. In Render, create a **Web Service** from the repository.
3. Render detects `render.yaml`/`Dockerfile` and builds the image.
4. Deploy.
5. Open the generated `https://...onrender.com` URL.

The server listens on Render's `PORT` automatically. `/health` is the health endpoint.

### Docker

```bash
docker build -t emberfall .
docker run -p 3001:3001 emberfall
```

Then visit http://localhost:3001.

## Important multiplayer note

This version keeps active rooms in server memory. That is appropriate for a prototype or a small private game. For a larger public launch, add Redis adapter/state and a persistent database for accounts, progression, matchmaking, and reconnects.

## Production hardening

Before opening the game to the public: add authentication/rate limiting, validate every Socket.IO payload, cap room lifetime, add reconnect tokens, use Redis for multi-instance Socket.IO, and add persistent player profiles.
