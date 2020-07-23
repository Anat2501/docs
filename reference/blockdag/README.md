# BlockDAG

A blockDAG is a [DAG](../../glossary.md#dag) of [blocks](../blocks/).

Instead of pointing to one previous block as in a blockchain, in a blockDAG each block points to all previous recent blocks; formally, these are the tips of the blockDAG that the mining node observed locally at the time of the new block’s creation.

![Directional Acyclic Graph of Blocks - blockDAG](https://lh6.googleusercontent.com/W-v03qdqQp_1rQsHFz00A5p14z3Bklo3Ag09-a16aJNlXXpbOOEzhCdpTtnhROEO_A9e1TDghXRhTD21wVt4oO9lUhfezsGt6F8NQXSwzmWL-bvwvuMPEp4iPX5zn1U1CwFjHhwT)

Unlike a blockchain which preserves consistency by construction \(every block in the chain adds transactions that are consistent with its predecessors in the chain\), a blockDAG incorporates blocks from different “branches” and so may contain conflicting transactions. We therefore need a method to recover consistency; in other words, a blockDAG system requires replacing Satoshi’s longest chain rule with a new consensus protocol. In Kaspa, the consensus protocol is [PHANTOM](../consensus/).

A blockDAG is denoted _G_ = \(_C_, _E_\), where _C_ represents blocks and _E_ represents unidirectional hash references from each block to previous blocks.

A blockchain is technically a special case of a blockDAG, where [k](../consensus/parameters.md#k) = 1.

In Kaspa, "blockDAG" refers to both the public ledger of Kaspa [transactions](../transactions/) and its data structure. It is sometimes referred to as simply a "DAG".

