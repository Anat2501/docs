# Transaction Lifecycle

### Transaction Creation

A [transaction](../../glossary.md#transaction) is created locally in a wallet application by specifying the following:

* **Version**
* **Number of** [**Inputs**](../../glossary.md#transaction-inputs)\*\*\*\*
* **Inputs:** which input UTXO to use as the source of payment; the transaction creator must possess the private key that complies with the redeem script in the source inputs.
* **Number of** [**Outputs**](../../glossary.md#transaction-outputs)**:** the transaction is signed locally.
* **Outputs:** which output addresses to pay to; where to send the money \(how much to each address, including change\) and what the redeem condition is.
* **Lock Time**
* \*\*\*\*[**Subnetwork**](../../glossary.md#subnetworks-silos) **ID**
* **Gas** \(only in non-native transactions\)
* **Payload Hash** \(only in non-native transactions\)

At this point, the transaction is in the form of a serialized raw transaction and has a [transaction hash](../../glossary.md#transaction-hash) and a [non-malleable](../../glossary.md#transaction-non-malleability) [transaction ID](../../glossary.md#transaction-id).

### Transaction Sending

After a user [creates](../../glossary.md#transaction-creation) a [transaction](../../glossary.md#transaction), in order to add it to the [blockDAG](../../glossary.md#blockdag), he needs to send the transaction to a _node_ in the Kaspa network.

The user can do this directly, by running a node on the permissionless Kaspa network. Alternatively, he can send the raw transaction to any node on the Kaspa network that he can communicate with using [JSON-RPC](../../glossary.md#json-rpc-api). Alternatively, he can relay the transaction to the Kaspa network via an API server such as [Kasparov](../../glossary.md#kasparov).

At this point the transaction is in the mempool of one of the network nodes.

### Transaction Verification



### Transaction Propagation



### Transaction Mining

After a [transaction](../../glossary.md#transaction) is propagated, it needs to be included in one of the future [blocks](../../glossary.md#block) that are _mined_ by any miner on the Kaspa network and added to the Kaspa [blockDAG](../../glossary.md#blockdag).

The transaction is included in a block and the block is propagated to other nodes.

* [ ] needs more info or link to more info about mining

### First Confirmation

