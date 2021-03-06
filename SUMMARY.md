# Table of contents

* [Welcome to Kaspa Docs](README.md)
* [About Kaspa](about-kaspa/README.md)
  * [Kaspa for Bitcoiners](about-kaspa/kaspa-for-bitcoiners.md)
  * [Get Started](about-kaspa/get-started.md)
* [Components](components/README.md)
  * [Kaspad Full Node](components/kaspad-full-node/README.md)
    * [Installation](components/kaspad-full-node/installation/README.md)
      * [Build From Source \(Linux\)](components/kaspad-full-node/installation/build-from-source.md)
      * [Build From Source \(Windows\)](components/kaspad-full-node/installation/windows.md)
      * [Build From Source \(Mac\)](components/kaspad-full-node/installation/mac.md)
    * [Usage](components/kaspad-full-node/usage/README.md)
      * [Kaspad Runtime Args](components/kaspad-full-node/usage/cli.md)
      * [Kaspad JSON-RPC](components/kaspad-full-node/usage/rpc-api/README.md)
        * [Selected Parent Chain Related Calls](components/kaspad-full-node/usage/rpc-api/selected-parent-chain-related-api-calls.md)
      * [KaspaCTL](components/kaspad-full-node/usage/kaspactl.md)
      * [GenCerts](components/kaspad-full-node/usage/gencerts.md)
    * [Reference](components/kaspad-full-node/reference/README.md)
      * [Network](components/kaspad-full-node/reference/network/README.md)
        * [Manual Node List](components/kaspad-full-node/reference/network/manual-node-list.md)
      * [Subnetworks](components/kaspad-full-node/reference/subnetworks-1.md)
      * [Garbage Collector](components/kaspad-full-node/reference/garbage-collector.md)
  * [Kasparov API Server](components/kasparov-api-server/README.md)
    * [Architecture](components/kasparov-api-server/architecture.md)
    * [Installation](components/kasparov-api-server/installing/README.md)
      * [Docker Quick Start](components/kasparov-api-server/installing/docker-quick-start.md)
      * [Build from Source](components/kasparov-api-server/installing/build-from-source.md)
    * [API](components/kasparov-api-server/api/README.md)
      * [API Methods](components/kasparov-api-server/api/methods.md)
      * [MQTT Topics](components/kasparov-api-server/api/mqtt-topics.md)
      * [Object Types](components/kasparov-api-server/api/object-types.md)
    * [Tutorials](components/kasparov-api-server/tutorials.md)
    * [Reference](components/kasparov-api-server/reference/README.md)
      * [Database Schema](components/kasparov-api-server/reference/db-schema.md)
      * [Internal Procedures](components/kasparov-api-server/reference/internal-procedures.md)
  * [Kaspa Miner](components/kaspa-miner.md)
  * [Transaction Generator](components/transaction-generator.md)
  * [CLI Wallet](components/cli-wallet.md)
  * [Faucet](components/faucet.md)
  * [DAG Visualizer & Explorer](components/dagviz/README.md)
    * [DAG Explorer](components/dagviz/dag-explorer.md)
  * [Smart Contract Layer](components/smart-contract-layer.md)
* [Reference](reference/README.md)
  * [Transactions](reference/transactions/README.md)
    * [Coinbase Transaction](reference/transactions/coinbase-transaction.md)
    * [Registry Transaction](reference/transactions/registry-transaction.md)
    * [Transaction's Including Blocks](reference/transactions/transactions-including-blocks.md)
    * [Script](reference/transactions/script.md)
    * [Transaction Lifecycle](reference/transactions/transaction-lifecycle.md)
    * [Transaction Malleability](reference/transactions/transaction-malleability.md)
    * [Chained Transactions](reference/transactions/chained-transactions.md)
    * [Transaction Selection and Replacement](reference/transactions/transaction-selection-and-replacement.md)
    * [Double Spending](reference/transactions/double-spending.md)
  * [Transaction Output \(TXO\)](reference/txo/README.md)
    * [UTXO](reference/txo/utxo.md)
    * [STXO](reference/txo/stxo.md)
    * [Outpoint](reference/txo/outpoint.md)
    * [UTXO Set](reference/txo/utxo-set/README.md)
      * [UTXO Collection](reference/txo/utxo-set/utxo-collection.md)
      * [Full UTXO Set](reference/txo/utxo-set/full-utxo-set.md)
      * [Diff UTXO Set](reference/txo/utxo-set/diff-utxo-set.md)
      * [UTXO Set Data Structures and Algorithms](reference/txo/utxo-set/the-utxo-set.md)
    * [UTXO Diffs](reference/txo/utxo-diffs/README.md)
      * [Diff Child](reference/txo/utxo-diffs/diff-child.md)
      * [Block Diff Data](reference/txo/utxo-diffs/block-diff-data.md)
  * [Blocks](reference/blocks/README.md)
    * [Block Header](reference/blocks/block-header/README.md)
      * [Merkle Trees](reference/blocks/block-header/merkle-trees.md)
    * [Block Body](reference/blocks/block-body.md)
    * [Genesis Block](reference/blocks/genesis-block.md)
    * [Difficulty](reference/blocks/difficulty/README.md)
      * [Simple Moving Average \(SMA\) Algorithm](reference/blocks/difficulty/sma-algorithm.md)
      * [SMA Algorithm Developer Spec](reference/blocks/difficulty/sma-algorithm-developer-spec.md)
    * [Block Validation](reference/blocks/block-validation.md)
  * [BlockDAG](reference/blockdag/README.md)
    * [Past](reference/blockdag/past.md)
    * [Future](reference/blockdag/future.md)
    * [Anticone](reference/blockdag/anticone.md)
    * [Tips](reference/blockdag/tips.md)
    * [Reachability](reference/blockdag/reachability.md)
    * [Virtual Block](reference/blockdag/virtual-block.md)
    * [Block Height](reference/blockdag/block-height.md)
  * [PHANTOM Consensus](reference/consensus/README.md)
    * [Blue Set](reference/consensus/blue-set/README.md)
      * [Block Blue Window](reference/consensus/blue-set/block-blue-window.md)
    * [Red Set](reference/consensus/red-set.md)
    * [Blue Score](reference/consensus/blue-score.md)
    * [Selected Parent](reference/consensus/selected-parent.md)
    * [Selected Parent Block Chain](reference/consensus/selected-parent-chain.md)
    * [Merged Blocks](reference/consensus/merged-blocks.md)
    * [Accepted Transactions](reference/consensus/accepted-transactions.md)
    * [Transaction's Accepting Block](reference/consensus/transactions-accepting-blocks.md)
    * [Parameters](reference/consensus/parameters.md)
    * [Reorganization of the BlockDAG \(Reorg\)](reference/consensus/reorganization-of-the-blockdag-reorg.md)
    * [Confirmations](reference/consensus/confirmations/README.md)
      * [Confirming Block Examples](reference/consensus/confirmations/confirming-block-examples.md)
    * [Finality](reference/consensus/finality-1/README.md)
      * [Finality Algorithm](reference/consensus/finality-1/finality.md)
    * [Consensus Data Structures](reference/consensus/consensus-data-structures/README.md)
      * [Block Data Structures in Consensus](reference/consensus/consensus-data-structures/block-data-structures-in-consensus.md)
      * [DAG Data Structure in Consensus](reference/consensus/consensus-data-structures/dag-data-structure-in-consensus.md)
      * [Transaction Data Structures in Consensus](reference/consensus/consensus-data-structures/transaction-data-structures-in-consensus.md)
  * [Pruning](reference/block-acceptance.md)
  * [Keys and Addresses](reference/keys/README.md)
    * [Address Format](reference/keys/keys-and-addresses.md)
  * [Serialized Data Formats](reference/serialized-data-formats/README.md)
    * [Variable Length Fields](reference/serialized-data-formats/variable-length-fields/README.md)
      * [VarString](reference/serialized-data-formats/variable-length-fields/varstring.md)
      * [VarInt](reference/serialized-data-formats/variable-length-fields/varint.md)
    * [Hash](reference/serialized-data-formats/hash.md)
* [Glossary](glossary.md)
* [Contribute](contribute.md)

## Archive

* [Archive](archive/archive/README.md)
  * [Change Log](archive/archive/change-log.md)
  * [Kaspa Social Contract](archive/archive/kaspa-open-source.md)
  * [Understanding blockDAGs](archive/archive/understanding-blockdags.md)
  * [Phantom Finality](archive/archive/phantom-finality.md)
  * [Concepts](archive/archive/concepts.md)
  * [Coinbase archive](archive/archive/coinbase-archive.md)
  * [Differences from Bitcoin](archive/archive/differences-from-bitcoin.md)
  * [PHANTOM/GHOSTDAG](archive/archive/phantom-ghostdag.md)

---

* [Kaspa GitHub](https://github.com/kaspanet)

