# Blue Set

The blue set is the set of [blocks](../../blocks/) in the [blockDAG](../../blockdag/) that [PHANTOM/GHOSTDAG](../) output as blue. _Intuitively_, it is the set of blocks that were withheld, or mined on an old state of the blockDAG \(blocks that did not point to all existing [tips](../../blockdag/tips.md) of the DAG at the time they were mined\) are unlikely to turn out blue.

‌The PHANTOM/GHOSTDAG algorithm constructs the blue set of the blockDAG by first inheriting the blue set of the best tip _B\_max_, i.e., the tip with the largest blue set in its past, and then adds blocks outside _B\_max_’s past to the blue set, provided that the _k_-cluster property is preserved.

In other words, the blue set of the blockDAG is the blue set of the [virtual block](../../blockdag/virtual-block.md).

## Blue Block

A blue block is a block included in the blue set.

