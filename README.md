# Bomberman DOM

A browser game for 2–4 players. Each player starts with three lives in a corner of a 15 × 13 arena. Blast through randomly placed blocks to reach opponents or uncover power-ups for bomb capacity, blast range, and movement speed. The last player standing wins.

## Run locally

With Node.js and npm installed, run these commands from the repository root:

```sh
cd backend
npm ci
npm start
```

Open [http://localhost:3000](http://localhost:3000) in two browser tabs and join with a different nickname in each. The second player starts a 20-second wait for more players, followed by a 10-second countdown. If all four spots fill, the countdown starts immediately. Players can chat in the lobby while they wait.

## Controls and gameplay

- **Arrow keys:** Move one tile at a time, or hold a key to keep moving.
- **Space:** Place a bomb on your current tile. You have three seconds to get clear.
- Blasts travel horizontally and vertically. A solid wall stops a blast; a breakable block absorbs it and is destroyed. A hit costs one life. If you have lives left, you return to your starting corner.

## How it works

- The [Node.js server](backend/server.js) serves the HTML, CSS, JavaScript modules, and images. The same server accepts WebSocket connections using `ws`; no separate frontend process or build step is needed.
- The [game rules](backend/game/logic.js) manage a single match in memory. Clients send directions rather than coordinates: the server checks the destination tile, then broadcasts accepted moves. It also resolves bomb timers, blast paths, damage, and power-ups for all players.
- The [browser client](frontend/js/main.js) applies [incoming events](frontend/js/services/socket.js) to a local store. A [bundled virtual DOM framework](frontend/js/new-framework/README.md) renders the [board](frontend/js/components/GameScreen.js) as positioned HTML elements rather than a canvas.
