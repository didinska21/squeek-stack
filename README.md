# SQUEEK TOOLS — Faucet + Staking for Squeekchain (Sepolia)

squeekchain-project: bbcde132-b8e9-4c39-8ae8-1c14cc9f44ff

A fan-made community tool for [Squeekchain](https://squeekchain.com), the
hamster-powered toychain anchored on **Ethereum Sepolia testnet**. One
single-page app with two tools the community needs every day:

- **💧 Faucet** — free Sepolia ETH for gas: **0.01 ETH per wallet per
  24 hours** (plus per-IP limit and a daily global cap), paid from a
  dedicated faucet wallet through a small Cloudflare Worker.
- **🥩 Staking** — stake Squeek NFTs into the official staking contract so
  they earn SQK every block, check claimable SQK and power, claim rewards,
  and withdraw — all signed by the user's own wallet in their browser.

A live strip at the top shows the hamster's status (IDLE/RUNNING, RPM,
current round, block height, SQK minted vs cap) from the official
Squeekchain API.

**Live site:** https://squeek.didinska.my.id/ · **Faucet wallet:**
`0x71105206c0ab218878afb4e6d5334ffaa9bc7862` (fund it with Sepolia ETH to
keep the faucet running)

## How it works

### Faucet (`app/` + `worker/`)
The static page reads the faucet balance directly from the chain and posts
claim requests to the Worker (`worker/src/index.js`). The Worker verifies
an optional Cloudflare Turnstile token, checks Cloudflare KV for the
24-hour cooldowns (per address and per IP) and the daily cap, then sends
exactly 0.01 ETH from the dedicated faucet wallet via ethers.js. It
refuses to send when the balance would fall to the 0.02 ETH gas reserve.

### Staking (`app/`)
Pure front-end — no backend, no keys. Connect a wallet
(`window.ethereum`), the app forces Sepolia (chainId 11155111), then reads
your position from the official API (`/v1/accounts/<address>`) and directly
from the staking contract (`claimable`, `accountPower`). Actions:

- `setApprovalForAll(staking, true)` on the NFT contract (one-time)
- `deposit(collection, tokenIds)` — stake selected Squeeks
- `withdraw(collection, tokenIds)` — unstake selected Squeeks
- `claim(address)` — collect claimable SQK

A read-only lookup lets anyone view an address's staking position without
connecting a wallet.

## Contracts (Sepolia, deployment `core-sepolia-20261103`)

| Contract | Address |
|---|---|
| Staking (SqueekStaking proxy) | `0x50fd1780526c3186a0c037c7622203bc4fb611ca` |
| SqueekNFT collection | `0xc4c7982657f00509548f3a3a41db7fa439a6856b` |
| SQK token (18 decimals) | `0xd78a3956cf63164d1fa1c622ad3c66623162fa1e` |
| Faucet wallet (dedicated) | `0x71105206c0ab218878afb4e6d5334ffaa9bc7862` |

RPC: `https://ethereum-sepolia-rpc.publicnode.com` · API:
`https://api.squeekchain.com/v1`

## Safety

- **Sepolia testnet only.** Testnet assets have no real-world value.
- Users always sign with **their own wallet**; the site never asks for a
  seed phrase or private key and cannot move user funds.
- The faucet uses a **dedicated wallet** holding only a small testnet
  float; its key exists solely as a Cloudflare Worker secret.
- The staking actions call only the official Squeekchain contracts listed
  above, with contract addresses hardcoded in the app.

## Files

- `app/index.html` — the whole dapp (ethers.js v6 via CDN, themed to match
  web-chatbot: cyan `#60DAFB`, Space Mono). Set `WORKER_URL` and
  `TURNSTILE_SITEKEY` at the top of its script after deploying the Worker.
- `worker/` — Cloudflare Worker (`wrangler`), KV binding `CLAIMS`, see
  `worker/README.md` for deploy steps.

Fan-made community tool. Not affiliated with the Squeekchain team.
Built by Didinska (@Didinska0610).
