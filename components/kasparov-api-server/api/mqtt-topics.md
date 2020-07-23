---
description: This page lists subscribe topics and their notification response types.
---

# MQTT Topics

Message Queuing Telemetry Transport \([MQTT](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html)\) is a lightweight publish/subscribe \(pub/sub\) messaging protocol designed for machine to machine \(M2M\) telemetry in low bandwidth environments.

Kasparov is bundled with [RabbitMQ](https://en.wikipedia.org/wiki/RabbitMQ), but it supports any MQTT broker with MQTT version 3.

## List of MQTT Topics

### **transactions/{address}**

Subscribing to this topic will send a notification of type [Transaction](object-types.md#transaction), whenever a [transaction](../../../glossary.md#transaction) for the given address is seen in a new [block](../../../glossary.md#block).

### **transactions/accepted/{address}**

Subscribing to this topic will send a notification of type [Transaction](object-types.md#transaction), whenever a transaction for the given address is accepted.

### **transactions/unaccepted/{address}**

Subscribing to this topic will send a notification of type [Transaction](object-types.md#transaction) whenever a transaction for the given address is unaccepted. This can happen when there is a [blockDAG reorg](../../../glossary.md#reorg). In this case, a transaction that was previously by a specific block may become unaccepted from that block, and may or may not be accepted in a different block \(receivable in the _transaction/accepted_ topic\).

### **dag/selected-tip**

{% hint style="success" %}
This should be used for calculating confirmations.
{% endhint %}

Subscribing to this topic will send a notification of type [Block](object-types.md#block) whenever the [selected parent chain](../../../glossary.md#selected-parent-block-chain-selected-parent-chain) becomes longer as a result of a new block extending it or a reorg.

### dag/selected-parent-chain

{% hint style="success" %}
This should be used for updating the selected parent chain
{% endhint %}

Subscribing to this topic will send a notification whenever the selected parent chain changes as a result of a new block extending it or a reorg.

The notification contains two arrays:

1. The first array contains hashes of blocks removed form the selected parent chain.
2. The second array contains hashes of blocks added to the selected parent chain, and for each block a list of hashes of blocks that it accepts.

```text
{
    "removedBlockHashes":
    [
        "string",
        "string"
    ],
    "addedChainBlocks":
    [
        {
            "hash":"string",
            "acceptedBlockHashes":
            [
                "string",
                "string",
                // ... up to k blocks total
            ]
        }
    ]
}
```

### dag/blocks

Subscribing to this topic will send a notification of type [Block](object-types.md#block) whenever any new block is added to [Kasparov](../).

## Usage

### Block Explorers

Block explorers would need:

* [dag/blocks](mqtt-topics.md#dag-blocks) to draw each new block.
* [dag/selected-parent-chain](mqtt-topics.md#dag-selected-parent-chain) to update the selected parent chain and to calculate block [confirmations](../../../glossary.md#confirmations).

#### Modus Operandi for Updating the Selected Parent Chain and the DAG

To update the selected parent chain and, consequently, the accepted and unaccepted blocks, one must first remove the blocks that are no longer part of the selected parent chain and mark the blocks that they accepted as blocks that were not accepted; then, add the blocks that are now part of the selected parent chain and mark the blocks that they accept as accepted blocks.

**Handling blocks removed from the selected parent chain:**

```text
for each block R in removedBlockHashes:
  R.isChainBlock = false
  for each block B having B.acceptingBlockHash = R.hash:
    B.acceptingBlockHash = null
```

**Handling blocks added to the selected parent chain:**

```text
for each block A in addedBlockHashes:
  A.isChainBlock = true
  for each block B in A.acceptedBlockHashes: 
    B.acceptingBlockHash = A.hash
```

### Wallets

Wallets need:

* [transactions/{address}](mqtt-topics.md#transactions-address) to show new transactions for each of the wallet's addresses.
* [dag/selected-tip](mqtt-topics.md#dag-selected-tip), [transactions/accepted/{address}](mqtt-topics.md#transactions-accepted-address) and [transactions/unaccepted/{address}](mqtt-topics.md#transactions-unaccepted-address) to calculate [transaction confirmations](../../../glossary.md#transaction-confirmations).

