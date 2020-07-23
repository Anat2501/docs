---
description: >-
  This section contains documentation, installation guides, and user manuals for
  Kaspa components. The following gives an overview of these components.
---

# Components

## [The Kaspa Network](kaspad-full-node/reference/network/)

The Kaspa network is a [peer-to-peer](https://en.wikipedia.org/wiki/Peer-to-peer) network of nodes implementing the Kaspa protocol, such as the [kaspad full node](kaspad-full-node/). Each kaspad node syncs and transacts with other nodes to maintain agreement over the distributed ledger.

## [Kaspad Full Node](kaspad-full-node/)

A full node is the basic participant in the Kaspa network. [Kaspad](kaspad-full-node/) is the reference full node Kaspa implementation written in Go. It connects to peer nodes, receives [blocks](../reference/blocks/) and [transactions](../reference/transactions/) from them, validates them, and propagates them across the network.

Currently, kaspad implements a [full archival node](../glossary.md#archival-node). The high throughput capacity of Kaspa could shift the bottleneck to data growth; the Kaspa projects aims to support a network of [pruned full nodes](../glossary.md#pruned-full-node) and auxilliary archival nodes. This is being worked on in v0.6.0.

## [Kasparov API Server](kasparov-api-server/)

[Kasparov](kasparov-api-server/) is an open source API server that communicates with a [kaspad](kaspad-full-node/) node to serve data from the Kaspa network and relay new [transactions](../reference/transactions/) to the network.

## Clients

### [CLI Wallet](cli-wallet.md)

The [CLI wallet](cli-wallet.md) is a basic local wallet, bundled with [Kasparov](kasparov-api-server/). It uses Kasparov API to query address balance and send [transactions](../reference/transactions/).

## [Faucet](faucet.md)

[Faucet](faucet.md) is an application that mines Kaspa and drops Kaspa coins to whomever requests. It is intended to ease the onboarding of interested parties to Kaspa, letting them transact without mining themselves.

## DAG Visualizer & Explorer

[DAGviz](https://dagviz.aspectron.com/) is a community built visualizer of the realtime, ever-growing Kaspa [blockDAG](../glossary.md#blockdag). It also serves as a block explorer for querying block, transaction and address information from the blockDAG.

## [Smart Contract Layer](smart-contract-layer.md)

Kaspa uses [subnetworks](kaspad-full-node/reference/subnetworks-1.md) to enable a decoupled [smart contracts layer](smart-contract-layer.md), while keeping the base money layer light and efficient.

\[page word count: ~268\]

