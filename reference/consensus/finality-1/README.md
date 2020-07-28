# Finality

Finality is a consensus-defined time window, after which no [reorganization of the blockDAG](../reorganization-of-the-blockdag-reorg.md) is allowed. It is sometimes referred to as finality window.

Generally, it is the guarantee that a certain decision will not later change, e.g., that the identity of the leader at time t will not be reversed, or that the set of transactions accepted anytime before step t will not change.

## Block’s Finality Score

A [block](../../blocks/)'s finality score helps determine if a block should be considered final. E.g., the finality score of a block could be the number of blocks in its future which are in virtual's blue set. We can then say that blocks whose finality score is above some threshold are final.

## Probabilistic Finality

Probabilistic finality is a guarantee of the form “the decision will not be reversed with a probability of at least 1 - ε”.

## Static Finality

Static finality is a finality guarantee that happens at fixed points in "time". E.g., every midnight.

That is, in static finality we say "every block created before the next to last finality point is considered finalized".

In contrast to rolling window, where we say, e.g., "any block which is at least 24 hours old is considered finalized".

