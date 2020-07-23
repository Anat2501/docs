# Block Data Structures in Consensus

## Block <a id="BlockDataStructuresinConsensus-Block"></a>

The **block** data structure represents a [block](../../blocks/) in the [blockDAG](../../blockdag/).

This data structure contains two types of data: network data received with the block and required to transmit it, and metadata used for our implementation of the consensus layer.

### Block.Blues <a id="BlockDataStructuresinConsensus-Block.Blues"></a>

> Pointer to a list of blocks

We need to be able to calculate the blue blocks in the past of any block B. Keeping a list of all these blocks is impossible, as this list can be as large as the entire DAG. To avoid this, every block only stores a list of the blue blocks which are not in the past of its designated parent.

These are exactly the blocks which are both in Past\(B\) and Anticone\(B.designatedparent\) \(see figure\).

![](https://daglabs.atlassian.net/wiki/download/thumbnails/143949828/image2018-10-8_16-53-13.png?version=1&modificationDate=1539006796101&cacheVersion=1&api=v2&effects=border-simple%2Cblur-border&width=474&height=250)

Block.Blues is a list of pointers to these blocks, sorted topologically, and internally sorted by Block.ID. This sorting criteria is strong enough for the order to be to uniquely defined.

### Block.Children <a id="BlockDataStructuresinConsensus-Block.Children"></a>

### Block.DiffChild <a id="BlockDataStructuresinConsensus-Block.DiffChild"></a>

> \*block

This field is used for the implementation of the [UTXO set](../../txo/utxo-set/) along with Block.UTXODiff.

Block.UTXODiff contains the differences between `this` and `this.DiffChild`.

### Block.ID <a id="BlockDataStructuresinConsensus-Block.ID"></a>

### Block.Parents <a id="BlockDataStructuresinConsensus-Block.Parents"></a>

### Block.SelectedParent <a id="BlockDataStructuresinConsensus-Block.SelectedParent"></a>

### Block.UTXODiff <a id="BlockDataStructuresinConsensus-Block.UTXODiff"></a>

> \*UTXODiff

This field is used for the implementation of the UTXO list along with Block.DiffChild.

Contains the differences between `this` and `this.DiffChild`.

## BlockSet <a id="BlockDataStructuresinConsensus-BlockSet"></a>

## BlockHeap <a id="BlockDataStructuresinConsensus-BlockHeap"></a>

