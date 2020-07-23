---
description: >-
  This section contains installation guides for kasparov. The following is an
  overview.
---

# Installation

Kasparov is an API server for Kaspa written in Go.

This project is currently under active development and is in a pre-alpha state. Some things still don't work and APIs are far from finalized. The code is provided for reference only.

This project contains the following executables:

* [kasparovd](../architecture.md#kasparov-restful-api-kasparovd): the Kasparov server. Handles user requests.
* [kasparovsyncd](../architecture.md#kasparov-sync-daemon-kasparovsyncd): the Kasparov sync daemon. Maintains sync with a full Kaspa node.
* [wallet](../../cli-wallet.md): an example Kaspa wallet. Interfaces with a kasparovd instance.

There are currently two ways to install Kasparov:

* [Quick start using Docker Compose](docker-quick-start.md): this will install Kasparov, [Kaspad](../../kaspad-full-node/), and all dependencies.
* [Building from source](build-from-source.md): with this option, you will have to install a full node too.

## Contribute

### Source code

The source code is available on [GitHub](https://github.com/kaspanet/kasparov).

### Issue Tracker

The [integrated GitHub issue tracker](https://github.com/kaspanet/kasparov/issues) is used for this project.

### Discuss

Join our [Discord server](https://discord.gg/WmGhhzk).

### License

Kasparov is licensed under the [copyfree](http://copyfree.org/) ISC License.

