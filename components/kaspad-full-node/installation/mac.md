---
description: This page describes how to install and run kaspad on Mac.
---

# Build From Source \(Mac\)

{% hint style="warning" %}
Mac is not officially supported yet, but it should work.
{% endhint %}

## Prerequisites

* Git \(e.g. follow the instructions on [git-scm](https://git-scm.com/download/mac)\)
* Latest version of [Go](http://golang.org/) \(currently 1.14.2\).
* A gcc compiler \(e.g. follow these instructions by [mkyong](https://mkyong.com/mac/how-to-install-gcc-compiler-on-mac-os-x/)\)

{% hint style="success" %}
Run the commands below in Terminal \(⌘ + Space -&gt; Terminal\).
{% endhint %}

### Installing Git

* Install git for windows by following the installation instructions on [git-scm](https://git-scm.com/download/mac). 

### Installing Go

* Install Go according to the official [installation instructions](http://golang.org/doc/install). 
* Ensure Go was installed properly and is a supported version \(see prerequisites\).

```bash
go version
```

* Check that `$GOROOT` and `$GOPATH` were set properly

```bash
go env GOROOT GOPATH
```

{% hint style="info" %}
The `GOROOT` and `GOPATH` above must not be the same path. By default, `GOROOT` will be `/usr/local/go` and `GOPATH` will be `/Users/your_user/go`.
{% endhint %}

### Installing gcc

* Install gcc by following these instructions by [mkyong](https://mkyong.com/mac/how-to-install-gcc-compiler-on-mac-os-x/). Apple's official Command Line Tools for Xcode come with gcc. You will need to visit the [Apple Developers website](https://developer.apple.com/download/more/) and get the version for your version of OSX.

## Building from Source

* Clone the kaspad source:

```bash
git clone https://github.com/kaspanet/kaspad
```

* Go to the directory where you cloned the code:

```bash
cd kaspad
```

* Run the tests:

```bash
./test.sh
```

{% hint style="info" %}
`./test.sh` tests can be skipped, but some things might not run correctly on your system if tests fail.
{% endhint %}

* Install:

```bash
go install . ./cmd/...
```

Kaspad \(and utilities\) should now be installed in `$GOPATH/bin`.

## Running

Kaspad has several configuration options available to tweak how it runs, but all of the basic operations work with zero configuration.

Run the following:

```bash
./kaspad --testnet
```

## What Next?

Get a wallet. Currently there is only a basic [CLI Wallet](../../cli-wallet.md).

\[page word count: ~226\]

