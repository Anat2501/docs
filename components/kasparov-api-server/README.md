---
description: >-
  This section describes the API server used for interacting with the Kaspa
  network.
---

# Kasparov API Server

Inheriting its name from the chess grandmaster, **Kasparov** is an open source programmatic interface for interacting with the Kaspa network, distributed on [GitHub](https://github.com/kaspanet/kasparov) with the copyfree [ISC license](https://choosealicense.com/licenses/isc/).

The Kaspa network is an inter-connected web of [Kaspa full nodes](../../glossary.md#full-node), working together to maintain the Kaspa distributed ledger. Kasparov connects to one of the full nodes, and provides a programmer-friendly interface for applications to interact with the network. Different applications have different needs, and each app should ideally construct its own API server tailored to its needs. Kasparov was built as a reference API server for applications such as wallets and block explorers. It allows things like sending [transactions](../../glossary.md#transaction), getting an address balance, and querying for and listening to specific [block](../../glossary.md#block) and transaction data.

## Develop using Kasparov

If you are a developer building an application on Kaspa, you may use Kasparov as is, modify it to your needs, or have your application connect to a Kasparov server run by the community. You may also contribute to the Kasparov code base.

### Get Kasparov

* Run your own Kasparov \(quick start with [Docker Compose](installing/docker-quick-start.md) or [build from source](installing/build-from-source.md)\)
* Connect to a community maintained Kasparov server
  * [ ] TBD: list of community run mirrors
* Visit [Discord](https://discord.gg/WmGhhzk) for more info

### Use Kasparov

If you already have access to a Kasparov, read the API documentation here:

* [RESTful API calls](api/methods.md)
* [MQTT pub/sub topics](api/mqtt-topics.md)
* [Object types](api/object-types.md)

### Projects Using Kasparov

#### [CLI Wallet](../cli-wallet.md)

Kasparov is bundled with a basic command line interface wallet. You may use it to create a wallet, check a balance and send simple transactions.

#### [DAGviz Explorer](../dagviz/)

The DAG visualizer and explorer use Kasparov to display the realtime blockDAG and block data.

### Contribute to Kasparov

For code contributions and bug reports, visit [GitHub](https://github.com/daglabs).

To discuss Kasparov and reach other developers and community members, join [Discord](https://discord.gg/WmGhhzk).

