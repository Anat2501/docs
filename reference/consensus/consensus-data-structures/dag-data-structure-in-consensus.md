# DAG Data Structure in Consensus

The DAG structure is at the heart of the consensus layer. It is used to represent the public ledger upon which all nodes attempt to agree.

The DAG data structure has a field which holds the set of all tips, and methods for retrieving any block \(or blocks\) by their ID.

Blocks should **only** be added to the DAG using the AddBlock method. Any block that was added in any other way is not considered a part of the DAG, is not counted in the Size field and is not recoverable via the Block and Blocks methods. Adding blocks in such a circumventive manner creates a discrepancy between the actual traversable graph and its representation in the DAG structure and **must** be avoided.

## Fields <a id="DAGDataStructureInConsensus-Fields"></a>

### DAG.Size <a id="DAGDataStructureInConsensus-DAG.Size"></a>

> Size int

The number of blocks in the DAG \(including the genesis block, excluding the virtual block\).

### DAG.Tips <a id="DAGDataStructureInConsensus-DAG.Tips"></a>

> Tips BlockSet

The set of all Tips in the DAG, i.e., the set of all blocks which have no non-virtual children.

### DAG.UTXOSet <a id="DAGDataStructureInConsensus-DAG.UTXOSet"></a>

> UTXOSet \*FullUTXOSet

The set of all unspent transaction outputs \(UTXO entries\) in the economy.

### DAG.Virtual <a id="DAGDataStructureInConsensus-DAG.Virtual"></a>

> Virtual \*Block

Points to a special block called the virtual block. The virtual block is just a useful abstraction – a hypothetical block from whose past looks like the entire DAG.

It is important to remember that the virtual block is not considered a real block. It holds no transactions, a new block can not point to it, and it is not counted in the Size field.

The virtual block's Parents field always points to DAG.Tips, its UTXOSet field always points to DAG.UTXOSet \(and is hence a pointer to a FullUTXOSet, unlike all other blocks whose UTXOSet points to a DiffUTXOSet\) and its Children field is always null.

## Methods <a id="DAGDataStructureInConsensus-Methods"></a>

### DAG.AddBlock <a id="DAGDataStructureInConsensus-DAG.AddBlock"></a>

> \(g \*DAG\) AddBlock\(newBlock \*Block\)

Adds a new block to the DAG, 

### DAG.Block <a id="DAGDataStructureInConsensus-DAG.Block"></a>

### DAG.Blocks <a id="DAGDataStructureInConsensus-DAG.Blocks"></a>

