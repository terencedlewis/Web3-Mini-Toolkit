# Web3 Mini Toolkit

A minimal single-page dApp that lets users connect their MetaMask wallet, send ETH, check ERC-20 token-gated access, and view their NFT collection — no framework, no build step, no backend.

---

## UI Preview

![Web3 Mini Toolkit UI](assets/ui-preview.svg)

---

## User Story

> **As a crypto user,**
> I want to open a webpage, connect my MetaMask wallet, enter a recipient address and ETH amount, and send a transaction —
> **so that** I can transfer ETH without installing any app or setting up a development environment.

> **As a token-gated content creator,**
> I want to check whether a connected wallet holds enough of a specific ERC-20 token —
> **so that** I can reveal exclusive content only to token holders.

> **As an NFT collector,**
> I want to see all NFTs owned by my connected wallet displayed as images —
> **so that** I can browse my collection without leaving the page.

### Acceptance Criteria

| # | Given | When | Then |
|---|-------|------|------|
| 1 | User opens the page with MetaMask installed | Clicks **Connect Wallet** | MetaMask pops up and requests account access |
| 2 | Wallet is connected | Page loads account info | Account address and active network are shown |
| 3 | User enters a valid recipient and amount | Clicks **Send ETH** | MetaMask opens the transaction confirmation dialog |
| 4 | User confirms the transaction in MetaMask | Transaction is mined | Tx hash appears with a block explorer link |
| 5 | User rejects the transaction in MetaMask | Rejection is caught | A clear error message is shown, no crash |
| 6 | User enters an invalid address or zero amount | Clicks **Send ETH** | Validation error is shown before MetaMask is invoked |
| 7 | Connected chain is Ethereum mainnet | Wallet connects | A prominent safety warning is displayed |
| 8 | User switches wallet account or network | MetaMask fires the change event | UI resets or reloads to reflect new state |
| 9 | Wallet is connected | User enters a token address and minimum balance, clicks **Check Access** | App reads `balanceOf` on the ERC-20 contract and shows balance vs threshold |
| 10 | User holds enough tokens | Check passes | Gated content is revealed with a success message |
| 11 | User does not hold enough tokens | Check fails | Access denied message shown, gated content stays hidden |
| 12 | User enters an invalid token address | Clicks **Check Access** | Validation error shown before any contract call |
| 13 | Wallet is connected | User pastes an Alchemy API key and clicks **Load My NFTs** | App fetches NFTs for the wallet and renders image cards in a grid |
| 14 | Wallet holds NFTs | NFTs load successfully | Each card shows the token image and name; broken images show a placeholder |
| 15 | Wallet holds no NFTs on the selected network | Fetch returns empty | Friendly "no NFTs found" message is shown |
| 16 | User provides an invalid API key | Alchemy returns an error | Error message shown, no crash |

---

## Features

- **One-click wallet connect** — uses `eth_requestAccounts` via `window.ethereum`
- **Live network detection** — identifies Mainnet, Sepolia, Polygon, Optimism, Arbitrum, and custom chains
- **Mainnet safety warning** — highlights risk when sending on Ethereum mainnet
- **Recipient + amount inputs** — user-controlled; no hardcoded addresses
- **Client-side validation** — checks `ethers.isAddress()` and positive numeric amount before sending
- **Transaction lifecycle** — tracks pending → confirmed with block number
- **Block explorer links** — auto-generates Etherscan/Polygonscan/etc links for confirmed txs
- **Wallet event handling** — responds to `accountsChanged` and `chainChanged` events from MetaMask
- **Token gate** — checks ERC-20 `balanceOf` against a user-defined threshold; reveals gated content on pass
- **Auto token metadata** — reads `decimals()` and `symbol()` for accurate balance display on any token
- **NFT viewer** — fetches up to 24 NFTs via Alchemy `getNFTsForOwner` and renders them as a responsive image grid
- **NFT image fallback** — shows a placeholder for NFTs with missing or broken images
- **Zero dependencies** — ethers.js loaded via CDN; no npm install required
- **Single file** — everything in `index.html`; open it directly in any browser

---

## Stack

| Tool | Purpose |
|------|---------|
| [MetaMask](https://metamask.io) | Wallet provider (`window.ethereum`) |
| [ethers.js v6](https://docs.ethers.org/v6/) | `BrowserProvider`, `getSigner`, `sendTransaction`, `Contract` |
| [Alchemy NFT API v3](https://docs.alchemy.com/reference/nft-api-quickstart) | `getNFTsForOwner` — fetch wallet NFTs with metadata |
| Vanilla HTML/CSS/JS | UI, styling, and event logic |

---

## Usage

```bash
# No install needed — just open the file
open index.html
```

Or serve it locally:

```bash
npx serve .
# then open http://localhost:3000
```

### Send ETH

1. Open `index.html` in a browser with MetaMask installed
2. Click **Connect Wallet** — approve in MetaMask
3. Enter a recipient `0x...` address and ETH amount
4. Click **Send ETH** — confirm in MetaMask
5. Tx hash with block explorer link appears on success

### Token Gate

1. Connect wallet (step 1–2 above)
2. Paste an ERC-20 token contract address into **Token Contract Address**
3. Set the **Minimum Balance Required** (e.g. `1`)
4. Click **Check Access**
5. If you hold enough tokens, the secret content is revealed

### NFT Viewer

1. Connect wallet (step 1–2 above)
2. Get a free API key at [dashboard.alchemy.com](https://dashboard.alchemy.com) → Create App
3. Paste the key into **Alchemy API Key**
4. Click **Load My NFTs**
5. Your NFTs render as an image grid with names below each card

---

## Safety

- Always test on **Sepolia testnet** before sending real value
- The app warns explicitly when connected to Ethereum Mainnet
- No private keys, secrets, or sensitive data ever touch this codebase
- Validate recipient address independently before confirming large transactions

---

## Development Workflow

This repo follows a standard branch → PR → merge flow:

```
main                    ← production-ready default branch
feature/<name>          ← short-lived feature branches
```

```bash
git checkout -b feature/my-change
# make changes
git add .
git commit -m "feat: describe the change"
git push -u origin feature/my-change
# open PR → review → merge → delete branch
```

---

## License

MIT
