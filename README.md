# EIP-7702 Revoke Tool

A web-based tool to remove EIP-7702 drainer delegations from your wallet across all chains at once.

**Live:** [https://yowanda.github.io/revoke-eip7702/](https://yowanda.github.io/revoke-eip7702/)

## Features

- **100% Client-Side** — Private keys never leave your browser. Everything runs locally, no backend server.
- **Multi-Chain** — Supports 7 chains: Ethereum, Base, Arbitrum, Optimism, Polygon, BNB Chain, Avalanche
- **Flashbots Protect** — Ethereum mainnet transactions are sent via Flashbots to prevent frontrunning
- **Sponsored Mode** — A separate wallet can pay gas fees (for drained wallets with 0 balance)
- **Real-time Gas Estimation** — Fetches live gas prices from each chain's RPC, not hardcoded thresholds. Works optimally on L2 chains (Optimism, Base, Arbitrum) where gas fees are extremely low
- **Gas Sponsor Panel** — Real-time gas fee breakdown per chain + total amount to send to sponsor, with copy buttons
- **Quick Actions** — One-click delegation check and revoke per chain
- **Real-time Status** — Watch revoke progress per chain as it happens

## How to Use

1. Open [https://yowanda.github.io/revoke-eip7702/](https://yowanda.github.io/revoke-eip7702/)
2. Enter the **Private Key** of the wallet that has been drained/delegated
3. Enter the **Sponsor Wallet Private Key** (a separate wallet with ETH/gas tokens to pay gas fees)
4. Click **"Check All Chain Delegations"** to scan which chains have active delegations
5. Click **"Estimate Gas Fee for All Chains"** to see exactly how much gas is needed per chain
6. Send the exact amount shown to the sponsor wallet
7. Select chains to revoke, or click **"Revoke All Chains"**
8. Wait for confirmation — each chain will display its status and a link to the block explorer

## What is EIP-7702?

EIP-7702 allows an EOA (Externally Owned Account) to delegate code execution to a smart contract. If someone signs an authorization that delegates their wallet to a drainer contract, the drainer can execute transactions on behalf of that wallet — including automatically transferring all assets out.

### How can a drainer add delegation on all chains?

If the authorization is signed with `chainId = 0`, the delegation applies to **all EVM chains** simultaneously. With just one signature, a drainer can apply delegation on Ethereum, Optimism, Base, Arbitrum, and any other chain.

### How revoke works

This tool sends a new EIP-7702 transaction that delegates the wallet to `address(0)` (zero address). This removes the delegation code from the wallet and restores it to a regular EOA.

## Security

- **Never share your private key with anyone**
- Run this tool on a **secure, private device** only
- Do not use on public computers or untrusted WiFi networks
- Open **Developer Tools (F12)** to verify no requests are made to servers other than blockchain RPCs
- If your private key is also compromised (not just the EIP-7702 delegation), **create a new wallet** — the attacker could re-delegate
- JavaScript source is obfuscated to prevent easy code tampering
- Anti-clickjacking protection prevents embedding in iframes
- Content Security Policy (CSP) blocks XSS/injection attacks
- Subresource Integrity (SRI) ensures the ethers.js CDN file hasn't been tampered with

## Supported Chains

| Chain | Chain ID | Gas Token |
|-------|----------|-----------|
| Ethereum Mainnet | 1 | ETH |
| Base | 8453 | ETH |
| Arbitrum One | 42161 | ETH |
| Optimism | 10 | ETH |
| Polygon | 137 | POL |
| BNB Smart Chain | 56 | BNB |
| Avalanche C-Chain | 43114 | AVAX |

## Gas Estimation

This tool uses real-time gas cost estimation from the network:

1. Fetches `feeData` (current gas price) from each chain's RPC
2. Calculates estimated cost: `gasLimit (60000) × maxFeePerGas`
3. Adds a 30% buffer for safety margin
4. Compares sponsor balance against the actual estimated cost

This allows revocations on L2 chains like Optimism, Base, and Arbitrum even with very small sponsor balances (e.g., 0.00002 ETH), as long as it's sufficient for the gas fee.

## Tech Stack

- HTML/CSS/JavaScript (vanilla, no frameworks)
- [ethers.js v6](https://docs.ethers.org/v6/) — Ethereum library
- GitHub Pages — hosting

## License

MIT
