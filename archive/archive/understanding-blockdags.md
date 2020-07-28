---
description: A tale of blues and reds
---

# Understanding blockDAGs

{% hint style="warning" %}
While this page looks nice, this is very redundant reference material \(I already had to remove my own explanation of phantom and ghostdag to archive\). I will probably archive this page and incorporate some of the diagrams to the consensus/blockDAG reference material, as I edit and refine those.
{% endhint %}

## PHANTOM - The New Consensus Kid on the Block

The natural topology of a DAG already induces a partial ordering over the blocks: if there is a path from block Y to block X in the DAG, then block X was provably created before block Y, and so should precede Y in the global order. Thus, we only need to define an order over blocks created in parallel, i.e. any set of blocks such that there is no path that connects them.

PHANTOM takes as input a blockDAG and outputs an ordering of its blocks. It does so by identifying a cluster of well-connected \(blue\) blocks and ordering the DAG in a way that favors them over the other \(red\) blocks.

![Depiction of Blue vs Red Blocks in the DAG](https://lh4.googleusercontent.com/ryec3BWdfGLVasVyG569W7DvvmV5ItBRkv91rLyCK7Ao9m6AutzGcijHdZEmHc5UanV5kp-vPKmV3S_zUdw1kB5bsdnOtpOjJ1vJqZAkqhNd--rEN69bqqK3pAIOLHbpW_t5ec58)

In PHANTOM, each node extracts transaction consistency from its locally observed blockDAG by running the PHANTOM algorithm on it, which assigns a linear ordering to the blocks, and thus, a linear ordering to the transactions, from which double spends can be eliminated. Currently, the red blocks are not part of the consensus.

### Blue Blocks

Blue blocks are blocks, which the PHANTOM consensus algorithm marked as well connected. They were with high probability produced by honest miners who did not withhold them from the network. A block B's blues \(B.Blues\) are all the blocks in B's past that are blue.

### Red Blocks

Red blocks are blocks, which the PHANTOM consensus algorithm marked as not well connected. They were with high probability produced by dishonest miners who withhold them from the network.

### Blue Score of a Block

The Blue Score of a block is how many blue blocks it has in its past \(not including itself\). In the blockDAG example above, block 2 has a blue score of 1 because there is one blue block in its past \(block 1\), and block 10 has a blue score of 4 because it has four blue blocks in its past \(blocks 4, 3, 2 and 1\).

### Selected Parent Block

The Selected Parent Block or simply the Selected Parent of a block is its parent block with the highest blue score. In the blockDAG example above, the selected parent of block 11 is block 7, because block 11 has two parent blocks \(blocks 7 and 10\), block 7 has a blue score of 5 \(blocks 5, 4, 6, 2 and 1\) while block 10 has a smaller blue score of 4 \(blocks 4, 3, 2 and 1\).

### The Selected Parent Chain

The Selected Parent Chain is the chain with the most blue blocks in its past. Without getting into specifics about the algorithm that finds the Selected Parent Chain, we will just say that it allows to create a view of the blockDAG, where the blocks are “reordered” as a chain and each block has branches coming out of it. Every block in the Selected Parent Chain "accepts" all transactions in its branches, forming a structure that is very similar to a linear blockchain, but contains much more information that would otherwise be discarded in a traditional blockchain. By including this information, the blockDAG is able to increase and transparency into the full history of transactions, orphaned blocks, double-spends and more; and also increase security by allowing the blocks branching off the selected parent chain to add consensus-mass under it.

![Selected Parent Chain \(DAG reordered as a &quot;blockchain&quot; with branches\)](../../.gitbook/assets/image%20%2810%29.png)

### Chain Blocks

A Chain Block \(CB\) is a block that is part of the Selected Parent Chain. In the example above, the selected parent chain is the chain formed by chain blocks 13, 11, 7, 3, 1 and 0.

### Accepted Blocks

Any block included in a chain block \(CB\)'s Blues is "accepted by CB".

### Accepted Transactions

Any transaction included in an accepted block is an accepted transaction.

{% hint style="info" %}
Transactions in a chain block \(CB\) would not be "accepted by CB", but rather would be accepted by one of CB's child blocks in the Selected-Parent-Chain.
{% endhint %}

