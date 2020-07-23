---
description: >-
  This section describes tools for interacting with a kaspad full node. The
  following is an overview.
---

# Usage

## Interacting with a Node

After launching your node, you can configure it using the [kaspaCTL CLI](cli.md), or interact with it using [JSON-RPC](rpc-api/).

## Querying and Developing Applications

For querying the network or building client applications, it is possible to directly interact with a node via [JSON-RPC](rpc-api/), but it is more convenient to interact via a RESTful API server such as [Kasparov](../../kasparov-api-server/).

[Kasparov](../../kasparov-api-server/) was built as a reference API server with the thought of serving wallet- and explorer-type applications, but we believe that every application has different needs and may therefore need a different type of API. Kasparov is open source and can be modified or reused accordingly.

## Setting up a Local Development Network

You may want to run a local network for development purposes, or for fun. For that, you will need:

* At least two [nodes](../) connected to each other
  * At least one of those nodes should mine blocks
* Something to generate transactions

\[page word count: ~150\]

