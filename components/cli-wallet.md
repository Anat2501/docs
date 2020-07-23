---
description: >-
  This page describes the CLI Wallet, a very basic command line interface wallet
  for Kaspa.
---

# CLI Wallet

The purpose of the Command Line Interface \(CLI\) Wallet is to allow basic user interaction with the Kaspa network. Users can create a wallet, view its address and balance, and send [transactions](../glossary.md#transaction).

The CLI Wallet creates a wallet \(private-public key pair\) on the local machine. It uses the [Kasparov API server](kasparov-api-server/) to send transactions and get the address balance.

## Installing the CLI Wallet

The CLI wallet comes as an example package within the Kasparov API Server. To install the CLI wallet, [install Kasparov](kasparov-api-server/).

## Creating a New Wallet

To create a new wallet, enter the command `wallet create`, with no input arguments:

```
$ wallet create
```

{% hint style="success" %}
A Bech32 cashaddr private key \(PK\), and a set of public addresses \(PA\), one for each of the networks DevNet, TestNet and MainNet, will be created locally on the machine and output to console. **Keep the private key safe**, as it allows access to all funds in all three addresses.
{% endhint %}

```bash
$ wallet new
Private Key:              tb1qw508d6qejxtdg4y5r3zarvary0c5xw7kxpjzsxawgt
Public Address (DevNet):  kaspadev:qr6m7j9njldwwzlg9v7v53unlr4jkmx6eylep8ekg2
Public Address (TestNet): kaspatest:pr6m7j9njldwwzlg9v7v53unlr4jkmx6eyvwc0uz5t
Public Address (MainNet): kaspa:pr6m7j9njldwwzlg9v7v53unlr4jkmx6ey65nvtks5
```

## Checking the Balance

To check the balance of an address, enter the command `wallet balance` with the following input arguments:

* `address` - the public address to return the balance of.
* `api_address` - the address of the Kasparov API Server to use for relaying the balance request to the network. Must include https:// \(e.g. [https://api.kas.pa](http://api.kas.pa)\).

```text
$ wallet balance --address=kaspatest:pr6m7j9njldwwzlg9v7v53unlr4jkmx6eyvwc0uz5t
                 --kasparov_address=https://kasparov.kas.pa
```

{% hint style="success" %}
The CLI Wallet will send a balance request on the input address, relayed through the input Kasparov API Server, using the REST API method [GET /utxos/{address}](kasparov-api-server/api/methods.md#utxos-address-address). The method returns a list of [TransactionOutput](kasparov-api-server/api/object-types.md#transactionoutput)s.
{% endhint %}

```text
$ wallet balance --address=kaspatest:pr6m7j9njldwwzlg9v7v53unlr4jkmx6eyvwc0uz5t
                 --kasparov_address=https://kasparov.kas.pa
Balance:                  KAS 1234.12345678
Pending Balance:          KAS   12.12345678
```

The balance is the sum of the [UTXOs](../glossary.md#utxo) belonging to the input address. If the [UTXO set](../glossary.md#utxo-set) does not contain any outputs belonging to the input address, then balance = 0.

The pending balance is comprised of non-mature [coinbase](../glossary.md#coinbase-transaction) and time locked outputs.

{% hint style="warning" %}
In case of an error, an error code and message will be output:
{% endhint %}

```text
$ wallet balance --address=kaspatest:pr6m7j9njldwwzlg9v7v53unlr4jkmx6eyvwc0uz5t
                 --kasparov_address=https://kasparov.kas.pa
Error 403: Access denied.
```

Possible errors:

* Error 422: Unprocessable Entity - &lt;forwarded error from the Kaspa node&gt; E.g. when the given address is not well-formatted.
* Error 500: Internal Server Error - &lt;forwarded error from the Kasparov API Server&gt;

## Receiving Funds

If you have just created your wallet, your wallet address will have a balance of Zero. In order to send a transaction, you need a positive balance. To get some Kaspa in your wallet, you may:

* [ ] Set up a [miner](kaspa-miner.md), and let it mine Kaspa into your wallet address.
* [ ] Ask someone on [Discord](https://discord.gg/WmGhhzk) to send you some Kaspa.
* [ ] Use the [Faucet](faucet.md) to drop some Kaspa.

## Sending a Transaction

To send a transaction, enter the command `wallet send` with the following input arguments:

* `private_key` - the private key to access the unspent Kaspa on the source address.
* `to_address` - the destination public address to send Kaspa to.
* `send_amount` - the amount of Kaspa to send \(decimal number with up to 8 digits after the decimal point, e.g. 1,234.123456789\).
* `kasparov_address` - the address of the Kasparov API Server, to relay the transaction to the network through. Must include https:// \(e.g. [https://kasparov.kas.pa](https://kasparov.kas.pa)\).

{% hint style="info" %}
Fee input is currently not supported. The fee will be a constant 1,000 Sompis \(equals to KAS 0.00001\).
{% endhint %}

```text
$ wallet send --private_key=tb1qw508d6qejxtdg4y5r3zarvary0c5xw7kxpjzsxawgt
              --to_address=kaspatest:qr6m7j9njldwwzlg9v7v53unlr4jkmx6eylep8ekg2
              --send_amount=1,234.12345678
              --kasparov_address=https://kasparov.kas.pa
```

{% hint style="success" %}
The CLI Wallet will check validity of the private key, the destination public address and the amount. It will then get an updated balance, that will allow it to build a transaction locally. It will then relay the transaction, through the input Kasparov API Server, to the Kaspa network using the REST API method [POST /transaction](kasparov-api-server/api/methods.md#transaction).
{% endhint %}

Once the synchronous part of sending the transaction completes successfully \(i.e. a Kaspa node accepts the transaction to its mempool\), the CLI Wallet will output the transaction id to the console:

```text
$ wallet send --private_key=tb1qw508d6qejxtdg4y5r3zarvary0c5xw7kxpjzsxawgt
              --to_address=kaspatest:qr6m7j9njldwwzlg9v7v53unlr4jkmx6eylep8ekg2
              --send_amount=1,234.12345678
              --kasparov_address=https://kasparov.kas.pa
Transaction was sent successfuly.
Transaction ID: feee746c516ee9e97ef71c913186425efa8ff273ec6a1d2c58a3f0a449adf683
```

{% hint style="info" %}
This marks the synchronous part of sending a transaction. It means that the transaction was accepted into the mempool - the waiting list of the network. It does not mean that the transaction was accepted into a block yet. The CLI Wallet does not yet support following up on the transaction's remaining lifecycle.
{% endhint %}

In case of an error in the synchronous part of sending the transaction, the CLI Wallet will output an error to console:

```text
$ wallet send --private_key=tb1qw508d6qejxtdg4y5r3zarvary0c5xw7kxpjzsxawgt
              --to_address=kaspadev:qr6m7j9njldwwzlg9v7v53unlr4jkmx6eylep8ekg2
              --send_amount=1,234.12345678
              --kasparov_address=https://kasparov.kas.pa
Error 403: Access denied.
```

Possible errors:

* Client side errors:
  * Error: private\_key is invalid.
  * Error: send\_amount is invalid. send\_amount must be in Kaspa.
  * Error: send\_amount is invalid. send\_amount must be in the valid range.
* API errors:
  * Error 422: Unprocessable Entity - &lt;forwarded error from the Kaspa node&gt;
  * Error 500: Internal Server Error - &lt;forwarded error from the API Server&gt;

## Displaying Version

To display the CLI wallet version, enter the command `wallet --version`:

```text
$ wallet --version
```

{% hint style="success" %}
The CLI Wallet will display the version.
{% endhint %}

```text
$ wallet --version
Kaspa CLI Wallet Version 0.0.1
```

## Displaying Help

To display the list of acceptable CLI Wallet commands, enter the command `wallet --help`:

```text
$ wallet --help
```

{% hint style="success" %}
The wallet will display a list of accepted commands.
{% endhint %}

