# Transaction's Accepting Block

A transaction's accepting block is the earliest [chain block](selected-parent-chain.md#Chain-Blocks) that [merges](merged-blocks.md) one of the transaction's [including blocks](../transactions/transactions-including-blocks.md).

* Transactions that are still in the mempool are not accepted yet.
* Transactions that try to spend an unavailable [outpoint](../txo/outpoint.md) \(not in the [UTXO Set](../txo/utxo-set/)\) are not accepted.
* Transactions from [red blocks](red-set.md#red-block) are not accepted.

![](../../.gitbook/assets/image%20%2829%29.png)

Following the illustration above:

* Block B accepts transaction included in block Q.
* Block C accepts transactions included in blocks B, R and P.
* Transactions included in blocks C and S are not accepted by any block yet.

