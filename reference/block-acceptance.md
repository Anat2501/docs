# Pruning

* [ ] This is a stub. This page should explain about pruning on Kaspa

Pruning is deleting data after you've validated it. What you lose when you prune, and how to overcome it, varies by type of pruning.

### Block Headers Pruning

Pruning old block headers. You lose the ability of new nodes to validate historical state transitions and the ability to support deep reorgs. This can be overcome by formal or informal finality rules or by relying on full non-pruning nodes.

### UTXO Set Pruning

Pruning the UTXO set. You lose the ability to validate state transitions. This is overcome via accumulators. Stateless clients use UTXO set pruning.



