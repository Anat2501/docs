---
description: This page describes how to install Kasparov from source.
---

# Build from Source

### Installation

#### Prerequisites

* Latest version of [Go](http://golang.org/) \(currently 1.13\)

**Build from Source**

* Install Go according to [the installation instructions](http://golang.org/doc/install).
* Ensure Go was installed properly and is a supported version:

```text
$ go version
$ go env GOROOT GOPATH
```

{% hint style="info" %}
The `GOROOT` and `GOPATH` above must not be the same path. It is recommended that `GOPATH` is set to a directory in your home directory such as `~/dev/go` to avoid write permission issues. It is also recommended to add `$GOPATH/bin` to your `PATH` at this point.
{% endhint %}

* Run the following commands to obtain and install kasparovd, kasparovsyncd, and the wallet including all dependencies:

```text
$ git clone https://github.com/kaspanet/kasparov $GOPATH/src/github.com/kaspanet/kasparov
$ cd $GOPATH/src/github.com/kaspanet/kasparov
$ go install ./...
```

* kasparovd, kasparovsyncd, and the wallet should now be installed in `$GOPATH/bin`. If you did not already add the bin directory to your system path during Go installation, you are encouraged to do so now.

### Getting Started

Kasparov expects to have access to the following systems:

* A Kaspa RPC server \(usually [kaspad](https://github.com/kaspanet/kaspad) with RPC turned on\)
* A MySQL database
* An optional MQTT broker

#### Linux/BSD/POSIX/Source

**kasparovd**

```text
$ ./kasparovd --rpcserver=localhost:16210 --rpccert=path/to/rpc.cert --rpcuser=user --rpcpass=pass --dbuser=user --dbpass=pass --dbaddress=localhost:3306 --dbname=kasparov --testnet
```

**kasparovsyncd**

```text
$ ./kasparovsyncd --rpcserver=localhost:16210 --rpccert=path/to/rpc.cert --rpcuser=user --rpcpass=pass --dbuser=user --dbpass=pass --dbaddress=localhost:3306 --dbname=kasparov --migrate --testnet
$ ./kasparovsyncd --rpcserver=localhost:16210 --rpccert=path/to/rpc.cert --rpcuser=user --rpcpass=pass --dbuser=user --dbpass=pass --dbaddress=localhost:3306 --dbname=kasparov --mqttaddress=localhost:1883 --mqttuser=user --mqttpass=pass --testnet
```

**CLI wallet**

See the [CLI Wallet docs](../../cli-wallet.md).

