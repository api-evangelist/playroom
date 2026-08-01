---
name: Run host-authoritative logic with RPCs
description: Elect a host, run game logic once on the host, and exchange actions via RPCs.
api: https://docs.joinplayroom.com/api-reference/js
operations: [isHost, transferHost, RPC, onPlayerJoin, onDisconnect]
source: https://docs.joinplayroom.com/api-reference/js
---

# Run host-authoritative logic with RPCs

Playroom elects one client as the authoritative **host**. Run game-deciding logic
behind the host check so it executes exactly once, and use RPCs for cross-client
calls.

## Steps

1. **Gate authoritative logic.** Guard server-like logic with `isHost()` (or the
   `useIsHost()` React hook) so only the host advances game rules; other clients
   render from synced state.
2. **Register RPCs.** On every client call `RPC.register("actionName", handler)`
   so incoming calls are handled consistently.
3. **Invoke across clients.** Use `RPC.call("actionName", data, mode)` to send an
   action (e.g. a player move) to the host or broadcast it.
4. **Handle churn.** Use `onPlayerJoin` and `onDisconnect` to keep roster state
   current; call `transferHost()` if you need to hand off authority deliberately.

## Rules
- RPCs are fire-and-forward, not idempotent — design handlers to tolerate repeats
  (see `conventions/`).
- Keep authoritative mutations inside the `isHost()` branch to avoid double-apply.
