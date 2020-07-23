# Block Height

A [block](../blocks/)'s height is the length of the longest path from it to the [genesis block](../blocks/genesis-block.md).

A block B's height can be defined recursively as `B.height = max(B.parents.height) + 1`.

The block height of Genesis equals Zero.

{% hint style="info" %}
A block’s height is to be distinguished from a block’s [blue score](../consensus/blue-score.md), which a  is generalization of block height in a blockchain.
{% endhint %}

