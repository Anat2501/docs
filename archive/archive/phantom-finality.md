# Phantom Finality

\[need to check if relevant\]

The concept:

1. Approximately every N \(for the sake of first implementation, assume N = 100\) blue blocks we mark a checkpoint.
2. A block is final if it has at least 2 checkpoints above it

The Algorithm:

1. Let _block.IsCheckpointCandidate_ = _true_ iff _block.BlueScore_/_N_ &gt; _block.SelectedParent.BlueScore_/_N_ \(Note: this is integer division, dropping the remainder\)
2. Set _currentCheckpoint_ := _genesisBlock_
3. Whenever a block is added to the DAG:
   1. Traverse its selected-parent-chain until you either reach _currentCheckpoint_ or block B where _B.BlueScore_ ≤ _currentCheckpoint.BlueScore_
   2. If said block is not _currentCheckpoint_ - decline the block as invalid.
4. Whenever a block B is added to the DAG where _B.IsCheckpointCandidate_  = _true_ and _newBlock.BlueScore_/_N_ ≥ _currentCheckpoint.BlueScore_/_N_: 
   1. Traverse it's selected-parent-chain until you reach the next block B' where _B'.IsCheckpointCandidate_ = _true_
   2. Set _currentCheckpoint_ := _B'_
   3. Implicitly this means that any blocks in B'.Past are now finalized

