<p align="center">
  <img src="https://raw.githubusercontent.com/Prisma-Chain/assets/main/prismachain.png" alt="Prisma Chain"/>
</p>

<h3 align="center">A validator built on light.</h3>

<p align="center">
  Hardware-signed validators for proof-of-stake networks.<br/>
  Six chains, one signing substrate, one named engineer per pager.
</p>

<p align="center">
  <a href="https://prismachain.it.com/"><b>prismachain.it.com</b></a> &nbsp;│&nbsp;
  <a href="https://prismachain.it.com/explorer/">Blobspace Explorer</a> &nbsp;│&nbsp;
  <a href="https://x.com/prismablocks">X</a> &nbsp;│&nbsp;
  <a href="https://medium.com/@PrismaCo">Medium</a>
</p>

## Refraction

A prism doesn't make light. It takes one beam and splits it into separate colours. Prisma works the same way: one signing setup feeds six networks, and each network has its own operator who reads its upgrades and carries its pager.

We keep the list short on purpose. Every chain has its own consensus, rekey cadence and upgrade path. Before we sign on mainnet, a chain sits through a **90-day testnet observation window** with an engineer assigned to it full-time. If nobody on the team can read a chain closely, we don't run it.

## Channels

| | Channel | Network | What it is | Validator |
|:-:|:-:|---|---|---|
| 🔴 | `CH 01` | **Dymension** | RollApp hub: modular rollups settled on the Dymension Hub | [ping.pub ↗](https://ping.pub/dymension/staking/dymvaloper1s6vgnk3hnr8aj52q9kjzy2qg4hh0jz8pfe3gdd) |
| 🟠 | `CH 02` | **Nibiru** | Smart-contract chain for DeFi | [site ↗](https://prismachain.it.com/#networks) |
| 🟡 | `CH 03` | **KYVE** | Data validation and archival for other blockchains | [site ↗](https://prismachain.it.com/#networks) |
| 🟢 | `CH 04` | **Starknet** | Permissionless ZK-rollup on Ethereum, STARK-proven execution | [site ↗](https://prismachain.it.com/#networks) |
| 🔵 | `CH 05` | **Fetch.ai** | Infrastructure for autonomous agents and AI services | [ping.pub ↗](https://ping.pub/fetchhub/staking/fetchvaloper1cfgswfuzmdz4f8n2anct5qzj4crvt3cj4hfxm9) |
| 🟣 | `CH 06` | **Akash** | Open marketplace for decentralized compute | [mintscan ↗](https://www.mintscan.io/akash/validators/akashvaloper1m6mzzsrgag22n9dtdwsmnrtjqzpudfz6gdhc9j) |

Celestia runs on testnet for now; it has its own section below.

## From wallet to signed block

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Prisma-Chain/assets/main/signing-flow.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Prisma-Chain/assets/main/signing-flow-light.svg">
  <img src="https://raw.githubusercontent.com/Prisma-Chain/assets/main/signing-flow-light.svg" width="100%" alt="Your wallet delegates to the Prisma validator; hardware modules in three datacenters reach a 3-of-5 quorum; the signed block's protocol rewards, minus commission, go straight back to your wallet"/>
</picture>

- **Delegate.** Stake from any wallet the network supports. Your tokens never leave your custody, and you aren't locked into any platform.
- **Sign.** Hardware modules in three datacenters have to reach a 3-of-5 quorum before a block is signed. Keys stay inside the hardware, and no key is reused across chains.
- **Earn.** Rewards come straight from the protocol to your wallet. Our commission is the only cut, and there's no minimum delegation on our side.

## Celestia

Most of what we build in public is about Celestia's data layer: what gets written there, by whom, and what it costs. We run a validator on the `mocha-5` testnet and publish two tools, one for the browser and one for the terminal.

| | |
|---|---|
| **Validator** | `mocha-5` testnet · [`celestiavaloper1egp3…j9jc4u`](https://celestia-dashboard.krews.xyz/celestia-mocha/validators/celestiavaloper1egp30ktgq9805l6es7vs9skl7yt2q7wrj9jc4u) on the Krews dashboard |
| **[Blobspace Explorer](https://prismachain.it.com/explorer/)** | Live view of Celestia blobspace and what rollups save by posting there |
| **[blobspace](https://github.com/Prisma-Chain/blobspace)** | Command-line tool that reads the same data straight from a node |

### Blobspace Explorer

<a href="https://prismachain.it.com/explorer/"><b>prismachain.it.com/explorer</b></a> answers one question: **how much do rollups save by posting data to Celestia instead of Ethereum?**

It reads live from Celenium and public Celestia REST, prices each blob against Ethereum calldata at about 30 gwei, and refreshes every 12 seconds.

| If you want to know… | Open |
|---|---|
| How much data Celestia holds right now, and what it would have cost on Ethereum | [Dashboard](https://prismachain.it.com/explorer/celestia) |
| Which rollups write the most data, and how many dollars each one has saved | [DA League](https://prismachain.it.com/explorer/celestia/savings) |
| What each namespace's data looks like: millions of micro-blobs or a few MB-scale ones | [Namespace Workbench](https://prismachain.it.com/explorer/celestia/namespace-workbench) |
| Which namespaces are still writing and which went quiet months ago | [Namespaces](https://prismachain.it.com/explorer/celestia/namespaces) |
| What landed in the last few blocks, blob by blob | [Blobs](https://prismachain.it.com/explorer/celestia/blobs) · [Blocks](https://prismachain.it.com/explorer/celestia/blocks) · [Transactions](https://prismachain.it.com/explorer/celestia/transactions) |
| The same picture on Ethereum's side: base fee, gas use, EIP-4844 blob space | [Ethereum](https://prismachain.it.com/explorer/ethereum) |

> [!NOTE]
> Savings are an estimate. They compare what a namespace actually paid on Celestia with what the same bytes would cost as Ethereum calldata at a fixed gas price. Blob-space pricing on Ethereum would give a different, lower number.

### blobspace CLI

The explorer shows the whole network. **[blobspace](https://github.com/Prisma-Chain/blobspace)** is for one question at a time, from your own shell. It needs no API key, only a Celestia RPC, and works on mainnet and `mocha-5`.

```bash
blobspace quote 1MB        # Celestia vs Ethereum blobs vs calldata, at today's gas
blobspace shape            # namespaces in the last 200 blocks: volume, blob size, wasted share space, utia/KB
blobspace tail             # every PayForBlobs as it lands
```

`quote` compares against Ethereum **blobs** as well as calldata, using live fees, so it shows the comparison the explorer's headline number leaves out.

## Why there are no uptime badges here

A badge in a README is a number someone typed. We'd rather you check the chain itself: every validator above links to an independent explorer that shows its signatures, missed blocks and jail history.

> *A validator is judged on the block it did not miss, not the one it announced.*
> We publish every signature, every outage and every post-mortem.

<br/>

<p align="center">
  <sub>Prisma Chain · built on light · <a href="https://prismachain.it.com/">prismachain.it.com</a></sub>
</p>


