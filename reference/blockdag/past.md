# Past

In a [blockDAG](./), the past of [block](../blocks/) _B_ is the subset of blocks [reachable](reachability.md) from _B_.

![](../../.gitbook/assets/dag-concepts-past-future-anticone-tips-.png)

## Previous Blocks \(Parents\)

A parent block is a block directly referenced by another block.

Block _A_ is a parent of block _B_ if block _B_ directly references block _A_. This implies that block _A_ provably existed before block _B_ was created_._

A block references its parent blocks by their hashes in the array field prevBlockHashes in its [block header](../blocks/block-header/).

The [PHANTOM](../consensus/) protocol prohibits parent blocks of block _B_ from being [ancestors](past.md#ancestors) of each other. In other words, a block’s parents can only be previous blocks that are not reachable one from another \(not in each other’s past\).

The PHANTOM protocol, which favors “well connected” blocks, dictates that it is in a miner's best interest to reference in his newly created block exactly all the blocks in the DAG that currently do not yet have descendants \(all the [tips](tips.md) of the blockDAG\).

In the Kaspa blockDAG, a block may reference several parent blocks.

The [genesis block](../blocks/genesis-block.md) has no parent blocks.

## Ancestors

A block _A_ is an ancestor of another block _B_ if _A_ in the past of _B_.

In other words, _A_ is an ancestor _B_ if _A_ is reachable from _B_.

This implies that _A_ provably preceded _B_, because a block can only reference blocks which had already existed when it referenced them.

