---
description: >-
  This page lists and details the JSON-RPC calls related to the selected parent
  chain.
---

# Selected Parent Chain Related Calls

Consumer-facing applications of Kaspa need to understand the state of the network, i.e the [UTXO set](../../../../glossary.md#utxo-set). They should be able to update it in real time and immediately react to [reorgs](../../../../glossary.md#reorg) and other events that cause drastic changes to the state.

On the other hand, it is not desirable to require client applications to understand advanced topics such as the entire [PHANTOM](../../../../glossary.md#phantom) protocol.

Therefore, the [blockDAG](../../../../glossary.md#blockdag) is simplified into a blockchain-like structure called the selected parent block chain. [This page](../../../../reference/consensus/selected-parent-chain.md) defines the selected parent block chain and related terms, such as chain blocks and accepted blocks.

## Structures <a id="Structures"></a>

The following structures are used in selected parent chain related calls.

### ChainBlock <a id="ChainBlock"></a>

```text
{
  Hash:            string,          Block hash
  AcceptedBlocks:  []AcceptedBlock, List of AcceptBlocks
}
```

### AcceptedBlock <a id="AcceptedBlock"></a>

```text
{
  Hash:           string,    Block hash
  AcceptedTxIds:  []string   List of IDs of the accepted transactions
}
```

## API Calls <a id="API-Calls"></a>

### getChainFromBlock <a id="GetChainFromBlock"></a>

Accepts a [block](../../../../glossary.md#block) as input, and returns the [selected parent chain](../../../../glossary.md#selected-parent-block-chain-selected-parent-chain) from that block until the [selected tip of the blockDAG](../../../../glossary.md#selected-tip-of-the-blockdag).

Specifically, if the input block is a [chain block](../../../../reference/consensus/selected-parent-chain.md#Chain-Blocks), it will return a list of chain blocks from that block to the selected tip of the DAG in ascending [blue score](../../../../glossary.md#blue-score) order.

If the input block is not a chain block, it will returns two lists:

* **Removed Chain Blocks** - a list of blocks that are no longer chain blocks \(from the input block to the block in the input block’s [past](../../../../glossary.md#past) from whence its [locally observed selected parent chain](../../../../reference/consensus/selected-parent-chain.md#selected-parent-chain-of-a-block) diverged from the DAG’s [global selected parent chain](../../../../reference/consensus/selected-parent-chain.md#selected-parent-chain-of-the-blockdag)\).
* **Added Chain Blocks** - a list of chain blocks from whence its locally observed selected parent chain diverged from the DAG’s global selected parent chain, to the selected tip of the DAG in ascending blue score order.

If requested, it can also return the chain blocks' contents.

#### Parameters <a id="JSON-RPCAPIcalls-Parameters.1"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| StartHash | boolean | N | hash of [genesis](../../../../glossary.md#genesis-block) | Hash of the bottom of the requested chain. If this hash is unknown or is not a chain block, returns an error |
| IncludeBlocks | boolean | N | false | If set to true, the block contents would be also included |

#### Output <a id="Output"></a>

```text
{ (JSON object)
  RemovedChainBlockHashes:  []string             List chain-block hashes that were removed from the selected parent chain in top-to-bottom order 
  AddedChainBlocks:         []ChainBlock         List of chain-blocks in bottom-to-top order
  Blocks:                   []verboseblocks      If IncludeBlocks=true, contains the verbose contents of all chain and accepted blocks in the AddedChainBlocks. Otherwise, omitted.
}
```

### getSelectedTip <a id="NotifyChainUpdates"></a>

Returns information about the [selected tip of the DAG](../../../../glossary.md#selected-tip-of-the-blockdag) as a hex-encoded block hash or, if Verbose is true, a JSON object as in the listed output.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Verbose | boolean | N | true | Specifies that the block should be returned as a JSON object instead of hex-encoded string |
| VerboseTx | boolean | N | false | Specifies that each transaction should be returned as a JSON object and only applies if the verbose flag is true |

#### Output

```text
{ (JSON object)
	Hash:                 string,        The hash of the block
	Version:              int32,         The block version
	VersionHex:           string,        The block version in hexadecimal
	HashMerkleRoot:       string,        Merkle tree reference to hashes of all transactions in the block
	AcceptedIDMerkleRoot: string,        Merkle tree reference to hashes of all transactions accepted from the block's blues
	UTXOCommitment:       string,        An ECMH UTXO commitment of this block
	ParentHashes:         []string,      The hashes of the parent blocks
	SelectedParentHash:   string,        The selected parent hash
	Nonce:                uint64,        The block nonce
	Time:                 int64,         The block time in seconds since 1 Jan 1970 GMT
	Confirmations:        uint64,        The number of confirmations
	BlueScore:            uint64,        The block's blue score
	IsChainBlock:         bool,          Whether the block is in the selected parent chain
	Size:                 int32,         The size of the block
	Bits:                 string,        The bits which represent the block difficulty
	Difficulty:           float64,       The proof-of-work difficulty as a multiple of the minimum difficulty
	ChildHashes:          []string,      The hashes of the child blocks (only if there are any)
	AcceptedBlockHashes:  []string      The hashes of the blocks accepted by this block
}
```

### getSelectedTipHash

Returns the hash of the [selected tip of the DAG](../../../../glossary.md#selected-tip-of-the-blockdag).

#### Parameters

This call takes no parameters.

#### Output

The hex-encoded block hash.

### notifyChainChanges <a id="NotifyChainUpdates"></a>

Subscribes the caller to any updates to the selected parent chain, whether be it a simple extension or a [reorg](../../../../glossary.md#reorg).

#### Notification Structure <a id="NotificationStructure"></a>

```text
{
  RemovedChainBlockHashes: []string,      List of chain-block hashes that were re-orged out in top-to-bottom order
  AddedChainBlocks:        []ChainBlock   List of chain-blocks in bottom-to-top order
}
```

#### Comments

1. `NotifyChainUpdates` should notify of block _B_ being added as a [chain block](../../../../reference/consensus/selected-parent-chain.md#Chain-Blocks), only after [`NotifyBlocks`](./#JSON-RPCAPIcalls-submitblock%28empty%29) has already notified of _B_'s existence and details.
2. `RemovedChainBlockHashes` might contain some block hashes that are also included in `AddedChainBlocks`.
3. `RemovedChainBlockHashes` would be empty for simple chain extensions.

### stopNotifyChangeChanges

Unregisters chain change updates. This call takes no parameters and outputs `null`.

## Modus Operandi of Clients <a id="Modus-Operandi-of-Clients"></a>

A typical client application should operate in the following way:

1. Obtain a [UTXO set](../../../../glossary.md#utxo-set) of the prune point. \[Q: what is the prune point?\]
2. Call `NotifyBlocks` to start receiving new [blocks](../../../../glossary.md#block).
   1. If any notification received - add new block to database.
3. Call `NotifyChainUpdates` to start receiving updates.
   1. If any notification received before step 4 is done - defer handling to step 5 \(Only after step 4 is complete\).
4. Call `GetChainFromBlock(prunePoint, true)` to receive the [selected parent chain](../../../../glossary.md#selected-parent-block-chain-selected-parent-chain) above their current state \(starting from prunePoint excluding\).
   1. Populate the database with the information inside the `Blocks` list.
   2. Use the `SelectedParentChain` to update the UTXO set to the most recent state.
5. On notification from `NotifyChainUpdates`:
   1. Undo the effect of any blocks in `RemovedChainBlockHashes`
   2. Apply all blocks in `AddedChainBlocks`

