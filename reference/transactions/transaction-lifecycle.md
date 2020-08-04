# Transaction Lifecycle

## Transaction Creation

A [transaction](./) is created locally in a wallet application by specifying the following:

* **Version**
* **Number of** [**Inputs**](../../glossary.md#transaction-inputs)
* **Inputs:** which input UTXO to use as the source of payment; the transaction creator must possess the private key that complies with the redeem script in the source inputs.
* **Number of** [**Outputs**](../../glossary.md#transaction-outputs)**:** the transaction is signed locally.
* **Outputs:** which output addresses to pay to; where to send the money \(how much to each address, including change\) and what the redeem condition is.
* **Lock Time**
* [**Subnetwork**](../../glossary.md#subnetworks-silos) **ID**
* **Gas** \(only in non-native transactions\)
* **Payload Hash** \(only in non-native transactions\)

At this point, the transaction is in the form of a serialized raw transaction and has a [transaction hash](../../glossary.md#transaction-hash) and a [non-malleable](../../glossary.md#transaction-non-malleability) [transaction ID](../../glossary.md#transaction-id).

## Transaction Sending

After a user creates a transaction, he needs to send the transaction to a node in the Kaspa network.

The user can do this directly, by running a node on the permissionless Kaspa network. Alternatively, he can send the raw transaction to any node on the Kaspa network that he can communicate with using [JSON-RPC](../../components/kaspad-full-node/usage/rpc-api/). Alternatively, he can relay the transaction to the Kaspa network via an API server such as [Kasparov](../../glossary.md#kasparov).

At this point the transaction is in the mempool of one of the network nodes.

## Transaction Verification and Propagation

After a transaction makes it into the mempool, a node will verify the transaction: it will check the signature, whether there are any errors, and whether it is trying to double spend. If the transaction fails any of the criteria, it is ignored entirely. Otherwise the node keeps a note of the transaction in temporary memory. It then propagates the transaction to other nodes in its network.

## Transaction Mining

As the transaction is propagated through the network, it will reach mining nodes that will include it in [blocks](../blocks/) they attempt to create through proof of work. Once the transaction is included in a successful block, the block is part of the [blockDAG](../blockdag/) and propagated to other nodes so they can add it to their view of the Kaspa blockDAG.

