# Block Body

## General

A block body in Kaspa is the second part of a [block](./), after the [block header](block-header/). A block body contains:

* The number of transactions within the block
* A list of the raw transactions in the block

## Number of Transactions <a id="Number-of-Transactions"></a>

The number of transactions is a special integer of varying length construct called [VarInt](../serialized-data-formats/variable-length-fields/varint.md).

## List of Transactions <a id="List-of-Transactions"></a>

Each block must have exactly one [coinbase transaction](../transactions/coinbase-transaction.md), as the first transaction in the block. The rest of the transactions are optional.

Chained transactions within the same block are forbidden, to allow multi-threaded transaction verification.

{% hint style="info" %}
Chained transactions are are a pair of transactions, such that one spends an output of the other. E.g., transaction B spends an output of transaction A.

Bitcoin, with its block-per-10-minutes block rate, allows chained transactions, in order to facilitate “Child Pays for Parent \(CPFP\)”. CPFP is a concept where a child transaction that relies on the confirmation and the outputs of an earlier parent transaction, compensates for its low fee, so that both the parent and child can be confirmed sooner, in the same block.

Kaspa, with its block-per-second block rate, obviates the need to chain transactions within the same block. The benefit of disallowing chained transactions is the ability to parallelize verification of transactions within the same block.
{% endhint %}

Transaction must be ordered in lexicographical [subnetwork](../../components/kaspad-full-node/reference/subnetworks-1.md) ID order. Other than that they may be ordered however the miner would like.

There is no explicit limit on the number of transactions in a block. The number is implicitly limited by the maximum [block mass](./#Mass).

No single transaction within a block shall surpass half of the block’s mass.

## Contents <a id="Contents"></a>

| **Bytes** | **Field** | **Data Type** | **Description** |
| :--- | :--- | :--- | :--- |
| 1-9 | numTxs | VarInt | Number of transaction entries in _vtx_ |
| Variable | txs | Array of transactions | An array of serialized transactions in the order they appear in the block, including one coinbase transaction as the first one in the array. |

