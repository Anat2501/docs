# Block Diff Data

### Block Diff Data

|  | Size in bytes | Name | Data type | Description |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 1 | hasDiffChild | boolean | Indicates if a [diff child](diff-child.md) exist |
| 2 | 32 | diffChild | [hash](../../serialized-data-formats/hash.md) | The diffChild's hash. Empty if hasDiffChild is true. |
| 3 | ? | diff | UTXODiff | The diff data's [diff](./) |

### UTXODiff <a id="BlockDiffData-UTXODiff"></a>

|  | Size in bytes | Name | Data type | Description |
| :--- | :--- | :--- | :--- | :--- |
| 1 | ? | toAdd | UTXOCollection | The [UTXO](../utxo.md) entries that should be added to the diffChild |
| 2 | ? | toRemove | UTXOCollection | The UTXO entries that should be removed from the diffChild |

### UTXOCollection <a id="BlockDiffData-UTXOCollection"></a>

|  | Size in bytes | Name | Data type | Description |
| :--- | :--- | :--- | :--- | :--- |
| 1 | ? | number\_of\_entries | [varInt](../../serialized-data-formats/variable-length-fields/varint.md) | The number of entries in the UTXOCollection |
| 2 | ? | entries | \[\]UTXOOutpointEntryPair | A list of UTXO entry and [outpoint](../outpoint.md) pairs  |

### UTXOOutpointEntryPair <a id="BlockDiffData-UTXOOutpointEntryPair"></a>

|  | Size in bytes | Name | Data type | Description |
| :--- | :--- | :--- | :--- | :--- |
| 1 | ? | outpoint\_size | varInt | The size of the outpoint |
| 2 | ? | outpoint | outpoint | The outpoint key of the UTXO entry |
| 3 | ? | utxo\_entry\_size | varInt | The size of the UTXO entry |
| 4 | ? | utxo\_entry | utxoentry | The UTXO entry |

