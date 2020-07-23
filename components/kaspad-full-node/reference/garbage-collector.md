# Garbage Collector

Right now we store [blocks](../../../glossary.md#block)' [UTXO](../../../glossary.md#utxo) data in memory as well as in the database, mostly for fast verification of blocks. When the server restarts, UTXO data is loaded from the database on-demand, but it is never cleared from memory while the node is running. However, blocks that are beyond the [finality](../../../glossary.md#finality) point can never serve as [parents](../../../glossary.md#previous-blocks-parents) for newer blocks, and therefore their data shouldn't be kept in memory.

Note: this is not [pruning](../../../glossary.md#pruning). Pruning deletes block data from the disk, making it impossible to sync new nodes, and therefore should be done much more carefully. Here we are clearing data from memory \(and potentially from the database\) that is provably redundant.

## Specification <a id="GarbageCollector-Specification"></a>

Add new field to blockNode: `isFinalized bool`

\[Q: what is "blockNode"?\]

### Algorithm <a id="GarbageCollector-Algorithm"></a>

Whenever `updateFinalityPoint` runs and finds that the finality point should be updated, it should trigger the garbage collector, which runs on a separate goroutine:

_**CollectGarbage\(newFinalityPoint\)**_

1. Iterate over newFinalityPoint's past in any order: For every block:
   1. Set isFinalized = true
   2. Set diff = nil
   3. Set diffChild = nil
2. Do not continue iterating on blocks which are already finalized.
3. Let golang's garbage collector do the magic of cleaning the memory.

### Changes to block validation <a id="GarbageCollector-Changestoblockvalidation"></a>

A block can be automatically marked as invalid the moment we see that any of its parents is finalized.

This does not mean we should get rid of the old routine that validates finality, since the garbage collector might be running with delay.

## Stage 2 <a id="GarbageCollector-Stage2"></a>

Once a block has been finalized, delete its UTXO data from the database.

