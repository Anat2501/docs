---
description: >-
  This section contains installation and usage guides for kaspad. The following
  is an overview.
---

# Kaspad Full Node

A full node is the basic participant in the Kaspa network. **Kaspad** is the reference full node Kaspa implementation written in [Go](https://golang.org/). It connects to peer nodes, receives [blocks](../../glossary.md#block) and [transactions](../../glossary.md#transaction) from them, validates them, and spreads them across the network.

### KaspaCTL CLI

[KaspaCTL](usage/cli.md) is a command line interface \(CLI\) for interacting with the full node.

### JSON-RPC API

[JSON-RPC API](usage/rpc-api/) contains calls for interacting with the full node, used by mining applications to verify and publish transactions.

## Who Needs to Run a Full Node?

Users interested in mining Kaspa will need to connect their miner to a full node. The full node does contain a CPU miner, but you will also need to connect an external dedicated miner.

Users interested in developing a wallet will need to run a local development network \(DevNet\) consisting of at least two interconnected full nodes. 

An [API server](../kasparov-api-server/) is available for connecting to a full node and serving wallet and block explorer requests.

## Setup Instructions

There are two ways to setup a Kaspa full node:

1. Quick way, using a [Docker image]().
2. Manually building it [from source code](installation/build-from-source.md).

## Working With a Full Node

After completing setup you can use either the [CLI commands](usage/cli.md) or [JSON-RPC](usage/rpc-api/) to interact with your node.

\[page word count: ~205\]





