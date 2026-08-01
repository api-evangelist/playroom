---
name: Initialize a Playroom multiplayer session
description: Boot a Playroom Kit game, wait for players, and read/write synced state.
api: https://docs.joinplayroom.com/api-reference/js
operations: [insertCoin, onPlayerJoin, myPlayer, setState, getState]
source: https://docs.joinplayroom.com/api-reference/js
---

# Initialize a Playroom multiplayer session

Use the Playroom Kit JavaScript SDK (`playroomkit`). There is no server API key —
each app is identified by a read-only `gameId` from the Dev Portal
(https://dev.joinplayroom.com).

## Steps

1. **Start the session.** Call `insertCoin({ gameId: "<your_game_id>" })` and
   `await` it. Omitting `gameId` throws the `no-game-id` error — copy the id from
   the project's Configuration > General tab.
2. **Handle joins.** Register `onPlayerJoin((player) => { ... })` to react to each
   arriving player. Use the `player` (`PlayerState`) to read/set that player's
   own state and to attach an `onQuit` handler.
3. **Identify the local player.** Call `myPlayer()` for the current client's
   `PlayerState`.
4. **Sync shared state.** Write global state with `setState(key, value, reliable)`
   and read it with `getState(key)`. Use `reliable = true` for state that must
   not be dropped; leave it off for high-frequency values (e.g. positions).

## Rules
- All clients share the same key/value store; writes are last-write-wins per key
  (no idempotency key — see `conventions/`).
- Errors surface as thrown JS errors, not problem+json (see `errors/`).
