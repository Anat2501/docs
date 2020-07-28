---
description: This page describes the modus operandi for client applications.
---

# Tutorials

## Constructing the BlockDAG

To construct the entire [blockDAG](../../glossary.md#blockdag), the client should:

1. Start getting new [blocks](../../glossary.md#block) as they are created by subscribing to the [dag/blocks](api/mqtt-topics.md#dag-blocks) notification. For now, it will store these blocks in a side queue.
2. Query the total number of blocks in existence.
3. Get all historical blocks from [Genesis](../../glossary.md#genesis-block) till now, \(an amount of blocks equal to the number received in the previous step\). This can be done with [GET /blocks](api/methods.md#blocks) page by page in any order.
4. Append the blocks from the first step.

## Missing Referenced Blocks

The client may occasionally see a block that it is not familiar with, possibly due to some notifications not reaching the client or not completing a full sync. In this case, the client can get unfamiliar block specifically by calling [GET /block/:blockhash](api/methods.md#block-blockhash).

## Maintaining the Selected Parent Chain

While nodes propagate blocks across the network and establish consensus about the [selected parent chain](../../reference/consensus/selected-parent-chain.md), [reorgs](../../glossary.md#reorg) of blocks near the [tips](../../glossary.md#tips) may occur. The selected parent chain blocks cast a total order over the blockDAG, and are thus important for calculating [confirmations](../../glossary.md#confirmations).

#### Modus Operandi for Updating the Selected Parent Chain and the Accepted Blocks

Updates to the selected parent chain obtained by subscribing to the [dag/selected-parent-chain](api/mqtt-topics.md#dag-selected-parent-chain) notification say which blocks were removed from the selected parent chain \(no longer accepted\), and which blocks were added to it \(newly accepted\). Accordingly, transactions that were accepted by newly unaccepted blocks are no longer accepted, while newly accepted blocks contain newly accepted transactions.

From an implementation standpoint, the updating order matters: first, remove the blocks that are no longer part of the selected parent chain and mark the blocks that they accepted \(and the transactions that they accepted\) as no longer accepted. Then, add the blocks that are now part of the selected parent chain and mark the blocks that they accept \(and the transactions that they accept\) as accepted.

**Step 1: Handling Blocks Removed from the Selected Parent Chain**

```text
for each block R in removedBlockHashes:
  R.isChainBlock = false
  for each block B having B.acceptingBlockHash = R.hash:
    B.acceptingBlockHash = null
```

**Step 2: Handling Blocks Added to the Selected Parent Chain**

```text
for each block A in addedBlockHashes:
  A.isChainBlock = true
  for each block B in A.acceptedBlockHashes: 
    B.acceptingBlockHash = A.hash
```

## Confirmations

Bitcoin relies on the concept of [confirmations](https://en.bitcoin.it/wiki/Confirmation) to determine a transaction's probability of being reversed. A standard Bitcoin client accepts six confirmations as adequate for irreversibility. This number reflects the block creation rate, the assumed attacker size potential, and the users' risk tolerance. Some users may accept fewer or more confirmations, depending on subjective parameters such as the transaction's importance, how well the paying party is known and trusted, etc.

As in Bitcoin, in Kaspa, the assurance that a transaction will not be reversed is probabilistic, subjective, and relies on confirmations, which are described on [this page](../../reference/consensus/confirmations/).

Since double spends are included in the Kaspa blockDAG \(but not accepted\), they provide additional information for a transaction's receiving party, in judging whether to accept the transaction. If two transactions that spend the same [UTXO](../../reference/txo/utxo.md) are included in the [blockDAG](../../reference/blockdag/), only one of them will be accepted. For one to take precedence over the other, it must be accepted by a [chain block](../../reference/consensus/selected-parent-chain.md#Chain-Blocks) with a higher [blue score](../../reference/consensus/blue-score.md). The later a double spending transaction is included in the blockDAG, the less likely it will be accepted over the original transaction.

Double spends can serve as a warning flag for the receiving end of a transaction. Client applications, such as wallets, check for confirmations; when the satisfactory amount of confirmations is reached, they can query for any seen double spends in the blockDAG, and in case there are, re-assess the situation and decide whether to reject the transaction from the paying party, or to wait for more confirmations and check again. It is advised to either wait a very large number of confirmations \(e.g. 1000\) or to reject the transaction.

A transaction is only 100% final after the [finality window](../../reference/consensus/finality-1/) has passed over it.

### Transaction Confirmations

#### Single Request

Whenever a [Transaction](api/object-types.md#transaction) object is returned by Kasparov, it will include the number of [transaction confirmations](../../reference/consensus/confirmations/#Transaction-Confirmations) at the time of the response, and an indication whether there are any seen double spending transactions.

When the number of confirmations is satisfactory, the client should request to reacquire the transaction re-check that there are no double spends in sight.

#### **First Confirmation**

To be notified of the first confirmation, listen to updates from [transactions/accepted/{address}](api/mqtt-topics.md#transactions-accepted-address). Whenever a transaction paying to or from the given address is accepted, this transaction will be returned.

Kaspa, with its one-block-per-second block rate, has very fast first confirmations.

{% hint style="info" %}
To continue to be updated on succeeding confirmations - and reorgs that may cause the transaction to be unconfirmed - listen to the topics [dag/selected-tip](api/mqtt-topics.md#dag-selected-tip) and [transactions/unaccepted/{address}](api/mqtt-topics.md#transactions-unaccepted-address), discussed below.
{% endhint %}

#### Live Updates

To show live updates for the number of transaction confirmations, listen to updates from [dag/selected-tip](api/mqtt-topics.md#dag-selected-tip). Whenever the length of the selected parent chain increases, due to a new chain block extending the current chain or a reorg, the [selected tip](../../reference/consensus/selected-parent.md#selected-tip-of-the-blockdag) [Block](api/object-types.md#block) will be returned.

Given updated selected tips, [this page](../../reference/consensus/confirmations/#Transaction-Confirmations) describes how to calculate transaction confirmations.

{% hint style="info" %}
A transaction may become unaccepted due to a [reorg](../../glossary.md#reorg). Any client monitoring confirmations should subscribe the [transactions/unaccepted/{address}](api/mqtt-topics.md#transactions-unaccepted-address) topic to be notified if and when a transaction becomes unaccepted. This is discussed below.
{% endhint %}

#### Dis-Accepting a Transaction \(Annulling\)

A reorg could annul a transaction. This happens when the [chain block](../../reference/consensus/selected-parent-chain.md#Chain-Blocks) that accepted it is no longer considered a chain block by the consensus algorithm.

To be notified when a transaction is no longer accepted, listen to updates from [transactions/unaccepted/{address}](api/mqtt-topics.md#transactions-unaccepted-address). Whenever a transaction paying to or from the given address is dis-accepted, this transaction will be returned.

{% hint style="info" %}
Even after being dis-accepted, a transaction may and probably will become accepted again, only by a different chain block, rewarding the new accepting block's miner with the transaction fees.
{% endhint %}

### Block Confirmations

#### Single Request

Whenever a [Block](api/object-types.md#block) object is returned by Kasparov, it will include the number of [block confirmations](../../reference/consensus/confirmations/#Block-Confirmations) at the time of the response.

#### Live Updates

To show live updates for the number of confirmations for a block, the client should :

1. Maintain the selected parent chain by listening to [dag/selected-parent-chain](api/mqtt-topics.md#dag-selected-parent-chain).
2. If the block is dis-accepted, update its number of confirmations to 0.
3. As long as it is not unaccepted, and more chain blocks are added to the selected parent chain after its accepting block, calculate the block confirmations using the formula on [this page](../../reference/consensus/confirmations/#Block-Confirmations).

## Checking for Double Spends

Whenever a [Transaction](api/object-types.md#transaction) object is returned by Kasparov, it will include an indication of any seen double spending transactions - that is, transactions in the [anticone](../../reference/blockdag/anticone.md) of the given transaction that spend the same [UTXO](../../reference/txo/utxo.md) as the given transaction.

Double spends should raise a red flag for the receiving side of the transaction.

## Getting Balance

To get the balance of an address, a client would need to [GET /utxos/address/:address](api/methods.md#utxos-address-address). This will return a list of the [transaction outputs](../../glossary.md#transaction-outputs) for the given address.

It is up to the client to accumulate the outputs in a way that lets it understand what can be spent when.

### Live Updates

When listening to [transactions/accepted/{address}](api/mqtt-topics.md#transactions-accepted-address), whenever a transaction paying to or from the given address is accepted, this transaction will be returned.

If the transaction was paid from the address, it is recommended to immediately slash it off the balance, to prevent attempts to double spend the outputs used by it.

If the transaction was paid to the address, it is recommended to immediately show it in a "pending" balance until the satisfactory number of confirmations has been reached. Assuming there are no double spends, the outputs of a transaction with even only one confirmation may be spent as early as in the next block.

{% hint style="info" %}
To continue to be updated on succeeding confirmations - and reorgs that may cause the transaction to be unconfirmed - listen to topics [dag/selected-tip](api/mqtt-topics.md#dag-selected-tip) and [transactions/unaccepted/{address}](api/mqtt-topics.md#transactions-unaccepted-address), discussed above in [Transaction Confirmations](tutorials.md#transaction-confirmations).
{% endhint %}

