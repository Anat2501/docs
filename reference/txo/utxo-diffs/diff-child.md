# Diff Child

A **diff child** of a [block](../../blocks/) B is the first block that points to B as a [parent](../../blockdag/past.md#previous-blocks-parents).

Each block needs to maintain only one diff child.

When the diff child is the [virtual block](../../../glossary.md#virtual-block), it is implemented as `null`.

Note that while a block may have several parents, each block has only one diff child.

