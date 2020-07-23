---
description: This page describes the internal procedures of the API Server.
---

# Internal Procedures

## Bootstrapping

1. Kasparov starts with an empty database.
2. Kasparov connects to a full node via [JSON-RPC](../../kaspad-full-node/usage/rpc-api/) WebSocket.
3. Kasparov subscribes to [notifyBlocks](../../kaspad-full-node/usage/rpc-api/#JSON-RPCAPIcalls-submitblock%28empty%29) and [notifyChainChanges](../../kaspad-full-node/usage/rpc-api/selected-parent-chain-related-api-calls.md#NotifyChainUpdates-1). For now, it stores the notifications without acting upon them until step 4 is complete.
4. The API Server calls [getChainFromBlock](../../kaspad-full-node/usage/rpc-api/selected-parent-chain-related-api-calls.md#GetChainFromBlock)\(nil, true\).
   1. The API Server populates [blocks](db-schema.md#blocks) and related tables using the Blocks part of the response, similar to when receiving a [Block Notification](internal-procedures.md#on-block-notification).
   2. The API Server converts SelectedParentChain to a ChainUpdateNotification with all the SelectedParentChain blocks as AddedChainBlocks, and then acts upon it as standard [Chain Update Notification](internal-procedures.md#on-chain-update-notification).
5. The API Server acts upon all notifications stored in step 3, and resumes normal operation.

## Restarting After Downtime

* Same as [Bootstrapping](internal-procedures.md#bootstrapping), except that in step 4 - call getChainFromBlock\(B, true\) where B is the last finalized block known to the API Server.
* Since this is a rare event, there is no need for the API Server to track what is finalized and what is not. Instead, whenever this is needed, find the finality point using a similar algorithm to that of the full node.

## On Block Notification

1. Add the new block to the [blocks](db-schema.md#blocks) table.
2. Add the new block transactions to the [transactions](db-schema.md#transactions) and relevant tables.

## On Chain Update Notification

1. Iterate over RemovedChainBlockHashes, and for each block hash in it:
   1. Find all Transactions with AcceptingBlockID = ID of corresponding block.
   2. Find all related TransactionInputs.
   3. For each TransactionInput, find the TransactionOutput with TransactionOutputID = ID.
   4. For each TransactionOutput, set the IsSpent to False.
2. Iterate over AddedChainBlocks, and for each chainBlock in it:
   1. For each acceptedBlock:
      1. Find all accepted Transactions by the AcceptedTxID.
      2. For each Transaction, set AcceptingBlockID to the acceptedBlock's ID.
      3. Find all related TransactionInputs.
      4. For each TransactionInput, find the TransactionOutput with TransactionOutputID = ID.
      5. For each TransactionOutput, set isSpent to True.

