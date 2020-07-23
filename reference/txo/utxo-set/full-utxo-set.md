# Full UTXO Set

The full UTXO set is a [UTXO collection](utxo-collection.md) object containing the current state of the [UTXO Set](./) of the entire [blockDAG](../../../glossary.md#blockdag) known to the [virtual block](../../../glossary.md#virtual-block) V, i.e. `UTXO_Set(Past(V))`.

The full UTXO set exists only for the virtual block. It is stored in memory, as well as in sequential order on the disk.

