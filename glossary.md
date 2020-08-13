---
description: >-
  This is a glossary of terms used in Kaspa. Each term links to a documentation
  page or section that provides more information on it, if such a page or
  section exists.
---

# Glossary

## A

### [Accepted ID Merkle Root](reference/blocks/block-header/#accepted-id-merkle-root)

The [merkle root](reference/blocks/block-header/merkle-trees.md) derived from all [transactions accepted by the block](glossary.md#accepted-transactions)_,_ used as a short commitment to these transactions.

### [Accepted Transactions](reference/consensus/accepted-transactions.md)

When a [chain block](reference/consensus/selected-parent-chain.md#Chain-Blocks) B is added to the [DAG](glossary.md#blockdag), [transactions](glossary.md#transaction) from [blue blocks](glossary.md#blue-block) that B [merges](glossary.md#merged-block), which spend outputs available in the [UTXO set](glossary.md#utxo-set), are accepted, while transactions which do not are rejected.

### Accumulator

A primitive that allows for a commitment \(a short digest that represents a set S\) and membership proof \(the ability to prove that item x belongs to S, by revealing x and a witness/proof that x is in S\). Accumulators enable stateless clients, as they reduce the state size to the constant-size digest.

### [Address Manager](components/kaspad-full-node/reference/network/#address-manager)

A component in [kaspad](glossary.md#kaspad) responsible for storing all peer addresses known to the kaspad node, providing an address when the node wants a new outgoing connection and providing all addresses during address exchange.

### [Ancestors](reference/blockdag/past.md#ancestors)

A [block](glossary.md#block) _A_ is an ancestor of another block _B_ if _A_ in the [past](glossary.md#past) of _B_.

### [Antichain](reference/blockdag/anticone.md#antichain)

A set _A_ ⊆ _G_ is an antichain if ∀_u_, _v_ ∈ _A_ : _u_ \|\| _v_. In other words, a set of [blocks](glossary.md#block) is an antichain if none of them are [reachable](glossary.md#reachability) from any other of them.

### [Anticone](reference/blockdag/anticone.md)

In a [blockDAG](glossary.md#blockdag), the anticone of [block](glossary.md#block) _B_ is the subset of blocks excluding _B_'s [past](glossary.md#past), _B_'s [future](glossary.md#future), and _B_ itself. I.e., it is the set of blocks in the DAG which did not reference _B_ \(directly or indirectly via their predecessors\) and were not referenced by _B_ \(directly or indirectly via _B_’s predecessors\).

### Archival Node

A [full node](glossary.md#full-node) that stores all data it has validated.

## B

### [Bits \(Target\)](reference/blocks/block-header/#target-bits)

A compact representation of the target that the [block header](glossary.md#block-header) hash must be less than or equal to in _proof of work_.

### [Block](reference/blocks/)

A structure that contains [transaction](glossary.md#transaction) data, to be included in the [public ledger](glossary.md#blockdag), similar to a Bitcoin block. It consists of a [block header](glossary.md#block-header) and a [block body](glossary.md#block-body).

### [Block Body](reference/blocks/block-body.md)

The second part of a [block](glossary.md#block), after the [block header](glossary.md#block-header). A block body contains:

* The number of [transactions](glossary.md#transaction) in the block
* A list of the raw transactions in the block

### [Block Header](reference/blocks/block-header/)

The first part of a [block](glossary.md#block). The _miner_ creating the block repeatedly hashes the header with a varying [nonce](glossary.md#nonce) until the hash output is below or equal to the [target bits](glossary.md#bits-target) in a process called _proof of work_; then, if the rest of the block is valid, it is added to the [blockDAG](glossary.md#blockdag).

### [Block Headers Pruning](reference/block-acceptance.md#block-headers-pruning)

Pruning old [block headers](glossary.md#block-header). You lose the ability of new nodes to validate historical state transitions and the ability to support deep [reorgs](glossary.md#reorg). This can be overcome by formal or informal [finality](glossary.md#finality) rules or by relying on [full non-pruning nodes](glossary.md#full-node).

### [Block Height](reference/blockdag/block-height.md)

A [block](glossary.md#block)'s height is the length of the longest path from it to the [genesis block](glossary.md#genesis-block).

### [Block Validity](reference/blocks/block-validation.md)

* [ ] TBD

### [Block Version](reference/blocks/block-header/#version)

The block version is the same as Bitcoin’s block version at the time of forking the btcd code. The version will be reverted to 1 when the network is launched.

### [Block’s Finality Score](reference/consensus/finality-1/#blocks-finality-score)

Helps determine if a [block](glossary.md#block) should be considered final. E.g., the finality score of a block could be the number of blocks in its future which are in [virtual](glossary.md#virtual-block)'s [blue set](glossary.md#blue-set). We can then say that blocks whose finality score is above some threshold are final.

### [BlockDAG](reference/blockdag/)

A [DAG](glossary.md#dag) of [blocks](glossary.md#block). Denoted _G_ = \(_C_, _E_\), where _C_ represents blocks and _E_ represents unidirectional hash references from each block to previous blocks.

### [Blue Block](reference/consensus/blue-set/#blue-block)

A [block](glossary.md#block) included in the [blue set](glossary.md#blue-set).

### [Blue Score](reference/consensus/blue-score.md)

The blue score of a [block](glossary.md#block) is the number of [blue blocks](glossary.md#blue-block) in its [past](glossary.md#past-1). In the Kaspa [blockDAG](glossary.md#blockdag), blue score, rather than [block height](glossary.md#block-height), is used to convey the "age" of a block since [genesis](glossary.md#genesis-block).

### [Blue Set](reference/consensus/blue-set/)

The set of [blocks](glossary.md#block) in the [blockDAG](glossary.md#blockdag) that [PHANTOM](glossary.md#phantom) and [GHOSTDAG](glossary.md#ghostdag) output as blue. Intuitively, blocks that were withheld, or mined on an old state are unlikely to turn out blue.

### [Blue Window](reference/consensus/blue-set/block-blue-window.md)

A function that accepts a [block](glossary.md#block) B and a window size N and returns a group of N [blue blocks](glossary.md#blue-block) in the [past](glossary.md#past) of B in [PHANTOM](glossary.md#phantom) order.

## C

### [Cashaddr](reference/keys/keys-and-addresses.md)

The address format used in Kaspa [transactions](glossary.md#transaction). The format is bech32-based, and is derived from [bitcoincash](https://github.com/bitcoincashorg/bitcoincash.org/blob/master/spec/cashaddr.md).

### [Chain Block](reference/consensus/selected-parent-chain.md#Chain-Blocks)

A [block](glossary.md#block) on the [selected parent chain](glossary.md#selected-parent-block-chain-selected-parent-chain).

### [Chained Transactions](reference/transactions/chained-transactions.md)

A pair of [transactions](glossary.md#transaction) such that one spends an output of the other. In Kaspa, chained transactions within the same [block](glossary.md#block) are forbidden, to allow multi-threaded transaction verification.

### [Children](reference/blockdag/future.md#children)

A [block](glossary.md#block)'s children are one or more succeeding blocks that reference it in their [headers](glossary.md#block-header).

### [Coinbase Transaction](reference/transactions/coinbase-transaction.md)

A [transaction](glossary.md#transaction) that includes the minting of new coins for discovering a [block](glossary.md#block) and [transaction fee](glossary.md#transaction-fees) collection.

### [Coinbase Subnetwork](components/kaspad-full-node/reference/subnetworks-1.md#kaspa-coinbase-subnetwork)

The [subnetwork](glossary.md#subnetworks-silos) used for [coinbase transactions](glossary.md#coinbase-transaction).

### Composability

The ability to compose together different smart contracts. There is a tension between sharding and composability, to be explained elsewhere.

### Confirmation Time

The time it takes for a [transaction](glossary.md#transaction) to become effectively irreversible.

### [Confirmations](reference/consensus/confirmations/)

A [block](glossary.md#block)'s number of confirmations is the difference in [blue score](reference/consensus/blue-score.md) between the [selected tip](glossary.md#selected-tip-of-the-blockdag) and the block's [accepting block](glossary.md#blocks-accepting-block) + 1.

`Block.confirmations = DAG.selected_tip.blue_score - Block.accepting_block.blue_score + 1`

### [Connection Manager](components/kaspad-full-node/reference/network/#connection-manager)

A component in [kaspad](glossary.md#kaspad) responsible for managing connections. That includes creating a new connection, handling disconnections, and handling the dynamic ban score.

### [Continuous Netsync](components/kaspad-full-node/reference/network/#continuous-netsync)

The process of acquiring [block](glossary.md#block) data from connected peers as the blocks are being mined. The process is suspended during [Initial Netsync](glossary.md#initial-netsync-ibd).

### Current Node

A self-defined state of a mining node of seeing the [selected tip](glossary.md#selected-tip-of-the-blockdag) as not being older than 12 hours. Non-current nodes should not mine, because the cost of mining will be wasteful compared to the reward.

### Current Non-Syncing Node

A current node that also does not have any sync peers. Current Non-Syncing nodes should not propagate [blocks](glossary.md#block) to its peers.

## D

### [_D_](reference/consensus/parameters.md#d)

The propagation delay -- the time it takes a [block](glossary.md#block) to propagate to all miners in the network. It is a function of the block size.

### [_D_\_max](reference/consensus/parameters.md#d_max)

An _a priori_ upper bound over [_D_](glossary.md#d), set in the protocol either explicitly \(hard-coded\) or implicitly through another parameter.

### DAG

A [Directed Acyclic Graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph).

### [DAG Split](components/kaspad-full-node/reference/network/#dag-split)

Divergence in the [blockDAG](glossary.md#blockdag) as a result of no consensus between two or more groups of nodes on one or more [blocks](glossary.md#block).

### [Descendants](reference/blockdag/future.md#descendants)

A [block](glossary.md#block) _A_ is a descendant of another block _B_ if _A_ in the [future](glossary.md#future) of _B_.

### [Diff Child](reference/txo/utxo-diffs/diff-child.md)

A diff child of a [block](glossary.md#block) B is the chronologically-first [parent](glossary.md#previous-blocks-parents) of B.

### [Diff UTXO Set](reference/txo/utxo-set/diff-utxo-set.md)

An object consisting of a pointer to the [full UTXO set](glossary.md#full-utxo-set) object and a [UTXO diff](glossary.md#utxo-diffs).

### [Difficulty](reference/blocks/difficulty/)

The probability of a block hash being less than or equal to the [target bits](glossary.md#bits-target).

### [Difficulty Adjustment](reference/blocks/difficulty/#difficulty-adjustment)

The practice of changing the [target bits](glossary.md#bits-target), so that it is either less [difficult](glossary.md#difficulty) or more difficult to mine the next block.

### [DNS Seeder](components/kaspad-full-node/reference/network/#dns-seeder)

A piece of software that uses the [P2P protocol](glossary.md#p-2-p-protocol-wire-protocol) to connect to all known hosts and ask them for more hosts.

## F

### [Finality](reference/consensus/finality-1/)

A consensus-defined time window, after which no [reorg](glossary.md#reorg) of the [blockDAG](glossary.md#blockdag) is allowed. Sometimes referred to as finality window.

### FlyClient

A protocol that allows a light client to verify the ledger's accumulated proof of work while receiving only a logarithmic number of blocks. For more information, see [the paper](https://eprint.iacr.org/2019/226.pdf).

### Full Node

A node that validates all [blocks](glossary.md#block)/[transactions](glossary.md#transaction) since the point where it plugged into the system; note that in Bitcoin jargon a node is considered a full node only if it validates historical data as well. Kaspa's full node client is [kaspad](glossary.md#kaspad).

### [Full UTXO Set](reference/txo/utxo-set/full-utxo-set.md)

### [Future](reference/blockdag/future.md)

In a [blockDAG](glossary.md#blockdag), the future of [block](glossary.md#block) _B_ is the subset of blocks from which _B_ is [reachable](glossary.md#reachability).

* [ ] TBD: context dependent

## G

### [Gas](components/kaspad-full-node/reference/subnetworks-1.md#gas)

A measure of computation cost for [subnetwork transactions](components/kaspad-full-node/reference/subnetworks-1.md).

### [Genesis Block](reference/blocks/genesis-block.md)

The genesis block of a [blockDAG](glossary.md#blockdag) is its initial [block](glossary.md#block), not referencing any previous blocks.

### [GHOSTDAG](reference/consensus/)

A greedy, recursive algorithm that approximates the largest [_k_](glossary.md#k-1)-cluster in a [DAG](glossary.md#blockdag). \(A greedy version of [PHANTOM](glossary.md#phantom)\).

## H

### [Hash](reference/serialized-data-formats/hash.md)

The output of a double [SHA-256](https://en.bitcoinwiki.org/wiki/SHA-256) on some input data.

### [Hash Merkle Root](reference/blocks/block-header/#hash-merkle-root)

The [merkle root](https://bitcoin.org/en/glossary/merkle-root) derived from all [transactions](glossary.md#transaction) within the [block body](glossary.md#block-body), used to verify a transaction’s inclusion in the [block](glossary.md#block) with a short proof.

## I

### [Initial Netsync \(IBD\)](components/kaspad-full-node/reference/network/#initial-netsync-ibd)

Called Initial Block Download \(IBD\) in Bitcoin, this is the process of acquiring [block](glossary.md#block) data from connected peers upon a new connection between peers, where blocks that are synced are ones that were mined before the connection.

## K

### [_k_](reference/consensus/parameters.md#k)

A parameter set by the [GHOSTDAG](glossary.md#ghostdag) and [PHANTOM](glossary.md#phantom) protocols. It is a function of [D\_max](glossary.md#d_max) and [lambda](glossary.md#lambda). It represents an upper bound over the number of [blocks](glossary.md#block) created in parallel.

### [Kaspad](components/kaspad-full-node/)

Kaspa's [full node](glossary.md#full-node) client.

### [Kasparovd](components/kasparov-api-server/)

### [Kaspaminer](components/kaspa-miner.md)

### KasparovSyncd

## L

### [lambda _λ_](reference/consensus/parameters.md#lambda-l)

The [block](glossary.md#block) creation rate.

### Layer 1

The base [blockDAG](glossary.md#blockdag) layer that achieves global consensus on Kaspa [transactions](glossary.md#transaction) using [PHANTOM](glossary.md#phantom).

### [Layer 2](components/smart-contract-layer.md)

The smart contract layer that runs on top of [Layer 1](glossary.md#layer-1). Compatible with a variety of [optimistic rollup](glossary.md#optimistic-rollup) constructions.

### Layer 3

An environment for [webs of trust ](glossary.md#web-of-trust)on top of [Layer 2](glossary.md#layer-2).

## M

### [Mass](reference/blocks/#Mass)

A measure used to approximate the storage and computational costs of [transactions](glossary.md#transaction) and [blocks](glossary.md#block). Greater mass signifies greater byte size and greater computational complexity.

### [Merged Block](reference/consensus/merged-blocks.md)

Every newly mined [block](glossary.md#block) merges \("adds'' to the [blockDAG](glossary.md#blockdag)\) up to [k](glossary.md#k-1) [blue blocks](glossary.md#blue-block). These blocks are referred to as the block's merged blocks.

### [Merging Chain Block](reference/consensus/merged-blocks.md#a-blocks-merging-chain-block)

A [block](glossary.md#block) B's merging chain block is the earliest [chain block](reference/consensus/selected-parent-chain.md#Chain-Blocks) that references B directly or indirectly.

### Median Time

The [timestamp](glossary.md#time-timestamp) of the median [block](glossary.md#block) of a group of blocks ordered by block time.

### Mempool

A node's memory storage containing [transactions](glossary.md#transaction) it has verified against the [Full UTXO Set](glossary.md#full-utxo-set), but which have not yet been [included](glossary.md#transaction-including-blocks) in a [block](glossary.md#block) known to the node.

### [MQTT](components/kasparov-api-server/api/mqtt-topics.md)

An open [standard](https://www.iso.org/standard/69466.html) lightweight [pub/sub](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) network protocol to transport messaged between devices.

## N

### [Native Subnetwork](components/kaspad-full-node/reference/subnetworks-1.md#kaspa-native-subnetwork)

The [subnetwork](glossary.md#subnetworks-silos) used to send all native Kaspa [transactions](glossary.md#transaction). Native Kaspa transactions do not use the [payload](glossary.md#payload) field; this field must be empty.

### [Net Split](components/kaspad-full-node/reference/network/#net-split)

Lack of connectivity between two or more groups of connected nodes. A net split can cause a [DAG split](glossary.md#dag-split) if the split was longer than the [finality window](glossary.md#finality).

### [Netsync](components/kaspad-full-node/reference/network/#net-sync)

The process of acquiring [block](glossary.md#block) data from other nodes.

### [Nonce](reference/blocks/block-header/#nonce)

An arbitrary 64-bit number that a miner may change to alter the [block header](glossary.md#block-header) hash between hash attempts, in order to get a valid hash \(once it is lower or equal to the [target bits](glossary.md#bits-target) threshold\).

## O

### Optimistic Rollup

A [layer 2](glossary.md#layer-2) scalability concept in which the [base layer](glossary.md#layer-1) consensus is not required to validate all [transactions](glossary.md#transaction) but is just required to provide [proof of publication](glossary.md#proof-of-publication-aka-data-availability). Application-layer nodes will validate only transactions they are interested in and provide fraud proofs to the base layer, and the base layer will help resolve any disputes that arise between these nodes. The types of fraud proof and dispute resolution method depend on the type of optimistic rollup \(ORU\).

## P

### [P2P Protocol \(Wire Protocol\)](components/kaspad-full-node/reference/network/)

### [Partial Blocks](components/kaspad-full-node/reference/subnetworks-1.md#partial-block)

A [block](glossary.md#block) that contains regular [transactions](glossary.md#transaction) for the given [subnetwork](glossary.md#subnetworks-silos) and [partial transactions](glossary.md#partial-transactions) for any subnetwork other than the given subnetwork.

### [Partial Nodes](components/kaspad-full-node/reference/subnetworks-1.md#partial-nodes)

A [full node](glossary.md#full) that validates all [base layer](glossary.md#layer-1) [blocks](glossary.md#block)/[transactions](glossary.md#transaction) but does not listen to transactions’ [payload](glossary.md#payload) data, except for those of the [subnetwork\(s\)](glossary.md#subnetworks-silos) it is interested in. Such a node essentially validates all base layer logic apart from [proof of publication](glossary.md#proof-of-publication-aka-data-availability).

### [Partial Transactions](components/kaspad-full-node/reference/subnetworks-1.md#partial-transaction)

A transaction with the [payload](glossary.md#payload) field zeroed.

### [Past](reference/blockdag/past.md)

In a [blockDAG](glossary.md#blockdag), the past of [block](glossary.md#block) _B_ is the subset of blocks [reachable](glossary.md#reachability) from _B_.

### [Payload](reference/transactions/#Payload)

A field inside a [transaction](glossary.md#transaction), used to store arbitrary data and code \(i.e., [subnetwork](glossary.md#subnetworks-silos)-specific logic\).

### [Payload Hash](reference/transactions/#Payload-Hash)

The [hash](glossary.md#hash) of the arbitrary data field, [payload](glossary.md#payload), in a [transaction](glossary.md#transaction).

### [PHANTOM](reference/consensus/)

A [blockDAG](glossary.md#blockdag) consensus protocol that returns an ordered list of [blocks](glossary.md#block) and a set of - intuitively - "well-connected" blocks to be inherited by the future blockDAG.

### [Previous Blocks \(Parents\)](reference/blockdag/past.md#previous-blocks-parents)

A [block](glossary.md#block)'s parents are one or more preceding blocks that the block references in its [header](glossary.md#block-header).

### [Probabilistic Finality](reference/consensus/finality-1/#probabilistic-finality)

A guarantee of the form “the decision will not be reversed with a probability of at least 1 - ε”.

### Proof of Publication \(aka Data Availability\)

A [proof of publication](https://petertodd.org/2014/setting-the-record-proof-of-publication) \(PoB\) layer decouples data ordering from data logic. In other words, it is an agnostic blockchain/[blockDAG](glossary.md#blockdag) without any formal native token or native application. In Ethereum community jargon, it is known as a [data availability](https://github.com/ethereum/research/wiki/A-note-on-data-availability-and-erasure-coding) \(DA\) layer.

### Pruned Full Node

A [full node](glossary.md#full-node) that [prunes](glossary.md#pruning) historical data.

### [Pruning](reference/block-acceptance.md)

Deleting data after validating it. What you lose when you prune, and how to overcome it, varies by type of pruning.

## R

### [Reachability](reference/blockdag/reachability.md)

Reachability between two [blocks](glossary.md#block) in the [DAG](glossary.md#blockdag) is whether one block is in the [past](glossary.md#past) of the other, i.e., whether there is a [directed path](https://en.wikipedia.org/wiki/Path_%28graph_theory%29) from one block to the other.

### [Red Block](reference/consensus/red-set.md#red-block)

A [block](glossary.md#block) included in the [red set](glossary.md#red-set).

### [Red Set](reference/consensus/red-set.md)

The set of [blocks](glossary.md#block) in the [blockDAG](glossary.md#blockdag) that [PHANTOM](glossary.md#phantom) and [GHOSTDAG](glossary.md#ghostdag) output as red. Intuitively, blocks that were withheld, or mined on an old state are likely to turn out red.

### [Registry Subnetwork](components/kaspad-full-node/reference/subnetworks-1.md#kaspa-registry-subnetwork)

The [subnetwork](glossary.md#subnetworks-silos) used to create new subnetworks. In order to create and announce a new subnetwork, one needs to make a [transaction](glossary.md#transaction) on `subnetwork 2`; the resulting [hash](glossary.md#hash) of this transaction is the new subnetwork ID.

### [Registry Transaction](reference/transactions/registry-transaction.md)

A [transaction](glossary.md#transaction) created on the [registry subnetwork](glossary.md#registry-subnetwork) in order to create a new [subnetwork](glossary.md#subnetworks-silos). Also called a "subnetwork registration transaction".

### [Reorg](reference/consensus/reorganization-of-the-blockdag-reorg.md)

An event in which an observation of new [blocks](glossary.md#block) in the [blockDAG](glossary.md#blockdag) changes a node's observed [PHANTOM](glossary.md#phantom) order of the blocks, and thus the blocks in its [selected parent chain](glossary.md#selected-parent-block-chain-selected-parent-chain).

### Responsiveness

A protocol is responsive if its performance is based on [_D_](glossary.md#d) and not on [_D_\_max](glossary.md#d_max). Read more here: [https://eprint.iacr.org/2016/917.pdf](https://eprint.iacr.org/2016/917.pdf)

* [ ] TBD: Better definition

## S

### [Schnorr Signature](reference/transactions/#Schnorr-Signatures)

### [SCRIPT](reference/transactions/script.md)

### [Selected Parent Block Chain \(Selected Parent Chain\)](reference/consensus/selected-parent-chain.md)

A chain of [blocks](glossary.md#block) formed from a block to the [genesis block](glossary.md#genesis-block) by recursively adding to the chain the [selected parent block](glossary.md#selected-parent-of-a-block).

### [Selected Parent of a Block](reference/consensus/selected-parent.md)

The [block](glossary.md#block)'s [parent](glossary.md#previous-blocks-parents) with the highest [blue score](glossary.md#blue-score). If there is a tie, then the selected parent block is the one with the lowest [hash](glossary.md#hash).

### [Selected Tip of the BlockDAG](reference/consensus/selected-parent.md#selected-tip-of-the-blockdag)

The [blockDAG](glossary.md#blockdag)'s [tip](glossary.md#tips) with the highest [blue score](glossary.md#blue-score).

### **Silo**

### **Standard Transactions**

### Stateless **C**lient

A node that after validating all data prunes not only historical data \(i.e. the [STXO](glossary.md#stxo) set\) but also the [UTXO set](glossary.md#utxo-set), and locally keeps only a commitment to the UTXO set. This type of node usually requires support from stateful nodes, which feed it with the data needed to calculate the new commitment.

### [Static Finality](reference/consensus/finality-1/#static-finality)

A [finality](glossary.md#finality) guarantee that happens at fixed points in "time". E.g., every midnight.

### [STXO](reference/txo/stxo.md)

Spent Transaction Output.

### \*\*\*\*[**Subnetwork**](components/kaspad-full-node/reference/subnetworks-1.md)\*\*\*\*

The subnetwork mechanism in Kaspa's consensus layer allows nodes with a common interest to form nested networks with their own rules within the greater Kaspa network. These subnetworks allow parties to store data and run custom logic but do not force them to spend network and computational resources on sending and verifying subnetwork-specific data.

### [Sync Manager](components/kaspad-full-node/reference/network/#sync-manager)

The software component within a node that manages [Netsync](glossary.md#netsync).

### [Sync Peer](components/kaspad-full-node/reference/network/#sync-peer)

From the perspective of a syncing node, this is the connected peer from which the Initial [Netsync](glossary.md#netsync) is done.

### [Synchronous Network](components/kaspad-full-node/reference/network/#synchronous-network)

A model in distributed system networks wherein a specific upper bound on the communication delay diameter of nodes is assumed inside the protocol. This in contrast to partially synchronous where the existence of such a bound is assumed, but not assigned a specific value.

## T

### TestNet

A public testing network for Kaspa in which coins are valueless and separate from real Kaspa coins.

### [Time \(Timestamp\)](reference/blocks/block-header/#time)

A [block](glossary.md#block)'s timestamp is the time the block was mined as reported by the miner.

### [Tips](reference/blockdag/tips.md)

In a [blockDAG](glossary.md#blockdag), tips are the set of [blocks](glossary.md#block) with [in-degree](https://en.wiktionary.org/wiki/indegree) 0 \(usually, the most recent blocks\).

### [Transaction](reference/transactions/)

A transfer of Kaspa value, similar to a Bitcoin transaction.

### [Transaction Accepting Block](reference/consensus/transactions-accepting-blocks.md)

A [transaction](glossary.md#transaction)'s accepting block is the earliest [chain block](reference/consensus/selected-parent-chain.md#Chain-Blocks) that [merges](glossary.md#merging-block) one of the transaction's [including blocks](glossary.md#transaction-including-blocks).

### Transaction Batching

tbd

### [Transaction Confirmations](reference/consensus/confirmations/#Transaction-Confirmations)

In Kaspa, a [transaction](glossary.md#transaction) receives its first confirmation when a [block](glossary.md#block) on the [selected parent chain](glossary.md#selected-parent-block-chain-selected-parent-chain) accepts it. More confirmations for the transaction are added as more [blue blocks](glossary.md#blue-block) are added on top of the [accepting block](glossary.md#transaction-accepting-block) of the transaction.

The number of confirmations a transaction has is the difference in [blue score](glossary.md#blue-score) between the [selected tip](glossary.md#selected-tip-of-the-blockdag) of the [blockDAG](glossary.md#blockdag) and the transaction's [accepting block](glossary.md#transaction-accepting-block), incremented by 1:

`Txn.confirmations = DAG.selected_tip.blue_score - Txn.accepting_block.blue_score + 1`

### [Transaction Fees](glossary.md)

### [Transaction Hash](reference/transactions/#Transaction-Hash)

A [transaction](glossary.md#transaction) identifier used only when building the [block hash merkle root](glossary.md#hash-merkle-root).

### [Transaction ID](reference/transactions/#Transaction-ID)

A [non-malleable](glossary.md#transaction-non-malleability) transaction identifier used for referring to [transactions](glossary.md#transaction) everywhere, except in the [block hash merkle root](glossary.md#hash-merkle-root).

### [Transaction Including Blocks](reference/transactions/transactions-including-blocks.md)

A transaction's including blocks are all the [blocks](glossary.md#block) containing that [transaction](glossary.md#transaction).

### [Transaction Inputs](reference/transactions/#Transaction-Inputs-1)

The same as [Bitcoin transaction inputs](https://bitcoin.org/en/glossary/input) except for the following two changes:

* Kaspa does not have the witness field because it does not need it to solve [transaction malleability](glossary.md#transaction-malleability). In Kaspa, the [transaction ID](glossary.md#transaction-id) is [non-malleable](glossary.md#transaction-non-malleability).
* In Kaspa the field sequence is `uint64` instead of `uint32`. This allows it to express [timestamps](glossary.md#time-timestamp) that go further than the year 2106.

### [Transaction Lifecycle](reference/transactions/transaction-lifecycle.md)

tbd

### [Transaction Malleability](reference/transactions/transaction-malleability.md)

The ability to change a [transaction](glossary.md#transaction)'s unique identifier by changing its signature, before it is [confirmed](glossary.md#transaction-confirmations). The transaction still gets processed, but with a different identifier. This exposes the payer to an attack, where the payee can deny receiving payment, asking the payer to repay.

### Transaction Mixing

tbd

### [Transaction Non-Malleability](reference/transactions/transaction-malleability.md#Transaction-Non-Malleability)

A non-malleable [transaction](glossary.md#transaction) is a transaction whose unique identifier cannot be changed by changing its internal elements, such as the signature scripts.

### [Transaction Outputs](reference/transactions/#Transaction-Outputs)

The same as [Bitcoin transaction outputs](https://bitcoin.org/en/glossary/output), except that they are `uint64` instead of `int64`.

### [Transaction Validity](glossary.md)

### Txsigner

## U

### [UTXO](reference/txo/utxo.md)

Unspent Transaction Output.

### [UTXO Commitment](reference/blocks/block-header/#utxo-commitment)

A hash of the [ECMH](https://cseweb.ucsd.edu/~mihir/papers/inc1.pdf) of the [UTXO](glossary.md#utxo) up to and including the [block](glossary.md#block).

### [UTXO Diff](reference/txo/utxo-diffs/)

A UTXO diff of a [block](glossary.md#block) is the information needed to obtain the [UTXO set](glossary.md#utxo-set) of that block from the UTXO set of its [diff child](glossary.md#diff-child).

### [UTXO _\*\*_Set](reference/txo/utxo-set/)

The set of all [UTXOs](glossary.md#utxo) \(from the perspective of some block\).

### [UTXO Set Pruning](reference/block-acceptance.md#utxo-set-pruning)

Pruning the [UTXO set](glossary.md#utxo-set). You lose the ability to validate state transitions. This is overcome via [accumulators](glossary.md#accumulator). [Stateless clients](glossary.md#stateless-client) use UTXO set pruning.

## V

### [Virtual Block](reference/blockdag/virtual-block.md)

The virtual block of a [blockDAG](glossary.md#blockdag) G is a _hypothetical_ [block](glossary.md#block), V, whose [past](glossary.md#past) equals the entire blockDAG.

## W

### Web of Trust

A trust graph whose topology is digitally signed by the participating nodes, and which enables users to authenticate and verify data and events without relying on trusted third parties. This concept originated from PGP.

