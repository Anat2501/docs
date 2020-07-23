---
description: This page describes how to install and run kaspad on Windows.
---

# Build From Source \(Windows\)

{% hint style="warning" %}
Windows is not officially supported yet, but it should work.
{% endhint %}

## Prerequisites

* Git \(e.g. [git for windows](https://gitforwindows.org/)\)
* Latest version of [Go](http://golang.org/) \(currently 1.14.2\).
* A gcc compiler \(e.g. [TDM-gcc](https://jmeubank.github.io/tdm-gcc/download/)\)

{% hint style="success" %}
Run the commands below in Windows PowerShell or Git Bash.
{% endhint %}

### Installing Git

* Install git for windows by following the official install

### Installing Go

* Install Go according to the official [installation instructions](http://golang.org/doc/install). 
* Ensure Go was installed properly and is a supported version \(see prerequisites\).

```bash
go version
```

Check that `$GOROOT` and `$GOPATH` were set properly

```bash
go env GOROOT GOPATH
```

{% hint style="info" %}
The `GOROOT` and `GOPATH` above must not be the same path. By default, `GOROOT` will be `c:\go` and `GOPATH` will be `C:\users\your_user\go`. `$GOPATH\bin` should be added to your system `$PATH`.
{% endhint %}

### Installing gcc

Install gcc by visiting [TDM-gcc](https://jmeubank.github.io/tdm-gcc/download/) and following the provided minimal online installer.

## Building from Source

Clone kaspad locally:

```bash
git clone https://github.com/kaspanet/kaspad $GOPATH/src/github.com/kaspanet/kaspad
```

Go to the local directory where the code was cloned to:

```bash
cd $GOPATH/src/github.com/kaspanet/kaspad
```

Run the tests:

```bash
./test.sh
```

{% hint style="info" %}
This can be skipped, but some things might not run correctly on your system if tests fail.
{% endhint %}

{% hint style="warning" %}
Known issue: some tests may fail on Windows because slashes `/`are expected and Windows uses backward slashes `\`. These issues should be solved in a future version.
{% endhint %}

Install kaspad:

```bash
go install . ./cmd/...
```

Kaspad \(and utilities\) should now be installed in `$GOPATH\bin`. \(Default: `C:\users\your_user\go\bin`\).

## Running

Kaspad has several configuration options available to tweak how it runs, but all of the basic operations work with zero configuration.

Run the following in Windows PowerShell or Git Bash

```bash
kaspad.exe --testnet
```

## What Next?

Get a wallet. Currently there is only a basic [CLI Wallet](../../cli-wallet.md).

\[page word count: ~237\]

