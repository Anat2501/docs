---
description: >-
  This page contains a short description of Kaspa aimed at those familiar with
  Bitcoin and general crypto concepts.
---

# Kaspa for Bitcoiners

Kaspa is a premine-less, scalable cryptocurrency with deflationary properties that does not have explicit central governance. Kaspa also supports fully validated financial products that do not have any counterparty risks, via Turing complete smart contracts. ****To support fast, decentralized payments and decentralized finance, Kaspa is split into two layers: a base consensus layer based on Bitcoin's proof of work consensus, and a secondary computation layer, based on Layer 2 smart contract scaling solutions \(i.e., the family of "[rollup](../glossary.md#optimistic-rollup)" solutions for Ethereum\).

## Layer 1 - Fast Payments Without Compromising Security

Traditional cryptocurrencies suffer from a security-scalability-decentralization tradeoff: decentralized cryptocurrencies must limit their block creation rate in order to limit "orphans", off-chain blocks created during the time it takes for a latent block to be propagated across the network. A high orphan rate decreases the effectiveness of the PoW network, thus decreasing its defense against attacks from malicious actors joining the open network.

To solve this tradeoff, Kaspa's consensus layer uses [PHANTOM](https://eprint.iacr.org/2018/104.pdf), a proof-of-work consensus protocol that generalizes Nakamoto’s chain into a directed acyclic graph of blocks \([blockDAG](../glossary.md#blockdag)\). PHANTOM incorporates "orphan" blocks into the chain to form a blockDAG, and then uses a novel greedy algorithm to order the blocks such that well-connected, honest blocks are favored, quickly and with high probability. PHANTOM allows Kaspa to circumvent the traditional tradeoff of blockchains, improving on block rate by orders of magnitude while maintaining the security guarantees of Bitcoin.

This results in a cryptocurrency that is supported by 51% security, has a high number of mining validator nodes, and has throughput on the order of one block per second. This is unlike existing cryptocurrencies, which inevitably trade off on having small numbers of validator nodes or lower BFT security \(33% threshold for malicious actors\).

### Fast Confirmations

Traditional cryptocurrencies' slow block rates indicate slow confirmations, i.e., the time it takes for a transaction to be published on the blockchain. Kaspa's consensus layer supports fast, subsecond confirmations—importantly, a fast first confirmation, which enables use cases that need immediate [proof of publication](../glossary.md#proof-of-publication-aka-data-availability) \(but not immediate irreversibility\), such as e-commerce.

### High Throughput

Traditional cryptocurrencies' slow block rate also indicates low transaction throughput. Using PHANTOM, Kaspa's consensus layer removes security as a bottleneck for high throughput, allowing block rate and block size increase up to what the network can handle. Kaspa also optimizes bandwidth cost and network infrastructure for high throughput.

### Mining Decentralization

Traditional cryptocurrencies' slow block rate also indicates high variance of mining income \(i.e. irregular mining rewards due to the difficulty of finding a block\), incentivizing miners to join larger and larger mining pools—which combine computing power and distribute smaller, more regular mining incomes to participants—as the network grows and the block difficulty increases. This centralizes the consensus power into the hands of a few pool managers. Kaspa's consensus layer's fast block rate decreases the variance of mining income, which decreases the incentive to join mining pools, contributing to mining decentralization.

## Layer 2 - DeFi \(Decentralized Finance\)

Bitcoin is not technologically optimized for financial applications. Any stable token on Bitcoin cannot be fully validated and therefore the crypto dollars \(stable tokens\) being built on Bitcoin can only act as an IOU \(e.g., Tether\). There are other attempts at smart contracts on Bitcoin, but because Bitcoin full nodes do not fully validate complex state, most DeFi is built on top of Ethereum.

Ethereum is built for financial applications, but it does not have the decentralization norms; the thin, proof-of-work-dedicated layer 1; and thus, the conviction as "money" that Bitcoin has. In Ethereum's current state, computation is coupled with layer 1, which necessitates running each step of the EVM inside the base consensus, causing issues around state bloat, gas pricing, and hard-to-manage dependencies, limiting its ability to scale.

To establish a needed new paradigm for expressive money, Kaspa has [a computation layer](../components/smart-contract-layer.md) that runs on top of but is decoupled from the base consensus layer; thus, the base layer remains thin, fast, and decentralized, while this layer provides smart contract support for decentralized finance applications. This layer supports various rollup constructions borrowed from scalability solutions for Ethereum: smart contracts are computed and stored off-chain, and their data are “rolled up” into commitments that are posted on-chain. This layer uses the base layer only for proof of publication and dispute resolution. Sets of smart contracts are separated into "[silos](../glossary.md#silo)" for different use cases, and are governed by sets of permissionless stakers.

\[page word count: ~731\]



