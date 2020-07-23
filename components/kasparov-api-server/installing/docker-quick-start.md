---
description: >-
  This is a quick-start guide to install and configure a Kaspa full node, an API
  server and a basic CLI wallet.
---

# Docker Quick Start

Follow the instructions below to create and run docker instances of the following services.

* [Kaspad](https://github.com/kaspanet/kaspad) \(Kaspa full node\)
* [Kasparov](https://github.com/kaspanet/kasparov) \(API server for Kaspa\)
* RabbitMQ
* MYSQL

Note that Kaspad will connect to the Kaspa Testnet using the current configuration.

### Getting started

#### Prerequisites

* [Docker](https://docs.docker.com/install/)
* [Docker Compose](https://docs.docker.com/compose/install/)

#### Cloning The GitHub Repository

Use git to clone the quick-start repository:

```text
$ git clone --recurse-submodules https://github.com/kaspanet/quick-start.git
```

#### Run

\(build docker images and run\)

```text
$ cd quick-start
$ ./run.sh
```

See `./run.sh --help` for more options.

