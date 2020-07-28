---
description: This page describes how to install and run kaspad on Linux.
---

# Build From Source \(Linux\)

## Prerequisites

Latest version of [Go](http://golang.org/) \(currently 1.14.2\).

### Installing Go

Install Go according to the official [installation instructions](http://golang.org/doc/install) and ensure it was installed properly and is a supported version:

```bash
$ go version
$ go env GOROOT GOPATH
```

{% hint style="info" %}
The `GOROOT` and `GOPATH` above must not be the same path. It is recommended that `GOPATH` is set to a directory in your home directory such as `~/dev/go` to avoid write permission issues. It is also recommended to add `$GOPATH/bin` to your `PATH` at this point.
{% endhint %}

## Building from Source

Run the following commands to install kaspad including all dependencies:

```bash
$ git clone https://github.com/kaspanet/kaspad $GOPATH/src/github.com/kaspanet/kaspad
$ cd $GOPATH/src/github.com/kaspanet/kaspad
$ ./test.sh
$ go install . ./cmd/...
```

{% hint style="info" %}
`./test.sh` tests can be skipped, but some things might not run correctly on your system if tests fail.
{% endhint %}

Kaspad \(and utilities\) should now be installed in `$GOPATH/bin`. If you did not already add the bin directory to your system path during Go installation, do so now.

## Running

Kaspad has several configuration options available to tweak how it runs, but all of the basic operations work with zero configuration.

```bash
./kaspad --testnet
```

## What Next?

Get a wallet. Currently there is only a basic [CLI Wallet](../../cli-wallet.md).

\[page word count: ~171\]

