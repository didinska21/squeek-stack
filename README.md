# 🐹 SQUEEK STACK: Build the Block

A fan-made, single-file browser game built for the **Squeekchain Genesis "Buildooooor"** community badge submission. It is a Tetris-like falling-block stacker (10×20 grid, 7 tetrominoes, line clears, score & levels) whose falling speed is driven **live** by Squeektoshi the hamster's real wheel, via Squeekchain's public, read-only API. No wallet, no keys, no signing — display only.

## How to play

- Open `index.html` in any modern browser (desktop or phone) and press **Start** (or Enter).
- **Keyboard:** ← → move · ↓ soft drop · ↑ rotate · **Space** hard drop · **P** pause
- **Touch:** on-screen buttons for left / rotate / soft drop / right / hard drop.
- Clear lines to score (100/300/500/800 × level); level rises every 10 lines.

### The Squeekchain twist

- The game polls the chain every 15 seconds:
  - Hamster **RUNNING** 🏃 — warm theme; fall speed scales with the live **difficulty** (`workPerThousandBlocks`).
  - Hamster **IDLE** 💤 — **NAP MODE**: slow fall and a dimmed blue theme.
- A **REAL BLOCK** progress bar mirrors the chain's real current block progress (`blockProgress`). If the real on-chain block height increments while you play, you get **+500 bonus points** and a celebration banner.
- The HUD shows live hamster status, wheel RPM, session distance, current round, settled round, block height, difficulty, and SQK minted vs the 2.1B cap.
- If the API is unreachable, the game keeps the last good values, shows an "API unreachable — playing offline values" note, and keeps working.
- Top-10 scores are kept in a session leaderboard in `localStorage` (with date), and a **Share score on X** button posts a prefilled tweet mentioning @squeekchain.

## Live data sources (public API, read-only, CORS-open)

- `GET https://api.squeekchain.com/v1/status` — `measurement.coreStatus`, `measurement.rpmMilli`, `measurement.speedMmPerSecond`, `measurement.distanceMm`, `builder.anchor.blockHeight`, `builder.anchor.blockProgress`, `builder.anchor.workPerThousandBlocks`, `builder.anchor.settledThroughRound`
- `GET https://api.squeekchain.com/v1/economy/state` — `supply.totalMintedBaseUnits`, `supply.capBaseUnits`, `capabilities.core.currentRound`

## Tech

- One self-contained `index.html` — vanilla HTML/CSS/JS + Canvas. No build step, no dependencies, no backend.
- Just open the file, or serve the folder with any static file server.

## Disclaimer

Fan-made community game for the Squeekchain Sepolia testnet. Not affiliated with the Squeekchain team. Testnet assets have no monetary value.

## License

MIT — free to use, copy, modify, and share; attribution appreciated.

## Builder

Built by Didinska (@Didinska0610) for the Squeekchain Genesis Buildooooor / Early Buildooooor badges.

<!-- SQUEEKCHAIN-VERIFY:REPO-LINE -->
