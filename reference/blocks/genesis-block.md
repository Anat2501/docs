# Genesis Block

## General <a id="General"></a>

The Genesis Block is the first [block](./) in the [blockDAG](../../glossary.md#blockdag).

It precedes all other blocks and references no other block \(it has no parent blocks\).

Other than that, it is like any other block.

## Contents <a id="Contents"></a>

Besides common block contents, the Genesis Block may contain initial transactions created by DAGlabs.

## Timing <a id="Timing"></a>

The Genesis Block is created by DAGlabs and published upon the network’s launch on GitHub.

## Validation <a id="Validation"></a>

In order to have a single valid Genesis Block, it should be signed with DAGlabs private key, while the public key will be included in the code base.

## Example <a id="Example"></a>

0100000000160ac68b7708f496a30705bc92daee73265ed08578a25d02498a2a22ef41c9c30000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000058e7155e00000000ffff7f1eac820200000000000101000000010000000000000000000000000000000000000000000000000000000000000000ffffffff00ffffffffffffffff0000000000000000000100000000000000000000000000000000000000000000000000000011c70c029eb22eb3ad2410fe2cdb8e1dde815bbb42feb493d6e3be8602e63a652417a914da1745e9b549bd0bfa1a569971c77eba30cd5a4b876b617370612d6465766e6574

### Breakdown <a id="Breakdown"></a>

```text
                                                                 -- header
                                                                 -- ------
01000000                                                         -- version 2
00                                                               -- numPrevBlocks (Only the Genesis block does not point to any previous blocks)
160ac68b7708f496a30705bc92daee73265ed08578a25d02498a2a22ef41c9c3 -- hashMerkleRoot
0000000000000000000000000000000000000000000000000000000000000000 -- acceptedIdMerkleRoot
0000000000000000000000000000000000000000000000000000000000000000 -- utxoCommitment
58e7155e00000000                                                 -- time
ffff7f1e                                                         -- bits
ac82020000000000                                                 -- nonce
                                                                 -- body
                                                                 -- ----
01                                                               --   numTxs (This block has 1 txn - the coinbase transaction)
                                                                 --   txs[1] (coinbase tx)
                                                                 --   -----------
01000000                                                         --   version 1
03                                                               --   numTxIns (this transaction has 1 input)
                                                                 --   txIns[0]
                                                                 --   --------
0000000000000000000000000000000000000000000000000000000000000000 --     prevTxId
ffffffff                                                         --     prevTxIndex
00                                                               --     scriptSigLen
ffffffffffffffff                                                 --     sequence
00                                                               --   numTxOuts (this transaction has no outputs. There are no premined coins in the Genesis block in Kaspa.)
0000000000000000                                                 --   lockTime
0100000000000000000000000000000000000000                         --   subnetworkID[20]
0000000000000000                                                 --   gas
11c70c029eb22eb3ad2410fe2cdb8e1dde815bbb42feb493d6e3be8602e63a65 --   payloadHash[32]
24                                                               --   payloadLength (VARINT)
17a914da1745e9b549bd0bfa1a569971c77eba30cd5a4b876b617370612d6465766e6574 -- payload[36] (payload is 0x24 = 36 in this case)
```

