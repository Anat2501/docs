# Future

In a [blockDAG](./), the future of [block](../blocks/) _B_ is the subset of blocks from which _B_ is [reachable](reachability.md).

![](../../.gitbook/assets/dag-concepts-past-future-anticone-tips-.png)

## Children

A child block is a block directly referencing another block.

Block _B_ is a child of block _A_ if block _B_ directly references block _A_. This implies that block _A_ provably existed before block _B_ was created_._

This is the inverse definition of a [parent block](past.md#previous-blocks-parents). If B is a child of A, then A is a parent of B.

## Descendants

A block _A_ is a descendant of another block _B_ if _A_ in the future of _B_.

In other words, _A_ is a descendant of _B_ if _B_ is reachable from _A_.

This implies that _B_ provably preceded _A_, because a block can only reference blocks which had already existed when it referenced them.

