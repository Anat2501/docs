# Coinbase Transaction

## Overview <a id="Overview"></a>

A coinbase transaction is a [transaction](./) that pays block rewards to miners. Block rewards include the minting of new coins as a reward for mining the [block](../blocks/), and fees from [accepted transactions](../consensus/accepted-transactions.md) in the block. As in Bitcoin, a transaction's fee is the implied difference of the sum of the transaction's input values and the sum of its output values.

‌In Kaspa, instead of each block paying the coinbase to itself, only [chain blocks](../consensus/selected-parent-chain.md#Chain-Blocks) pay block rewards, via their coinbase transactions, to all their [merged blocks](../consensus/merged-blocks.md). This approach is taken because in Kaspa, upon a block’s creation, it is not certain that its miner should get fees for all the transactions included in it, since some of those transactions might be included in other blocks, and a transaction fee can be only paid once.

Upon creation, every block assumes it is a chain block and references its merged blocks and accepted transactions. Accordingly, it pays the blocks, containing the transactions it accepts, their due fees in its coinbase transaction. In the event of chain blocks [reorging](../consensus/reorganization-of-the-blockdag-reorg.md) out of the [selected parent chain](../consensus/selected-parent-chain.md), the coinbase transaction they paid to their accepted blocks will get annulled \(unaccepted\). Since coinbase transactions cannot be rolled back easily, as a preventative measure, their outputs are not spendable for 100 [confirmations](../consensus/confirmations/) from the block that accepted them.‌

The miner of a block must include a coinbase transaction that has one output per merged block. For each output, the value must equal the block reward for that merged block + the total fees for all accepted transactions from that merged block, and the scriptPubKey must be taken from that merged block’s coinbase transaction’s payload. This is enforced by the consensus protocol.

Likewise, the coinbase transaction must include a redeem script as instructions for the future block that accepts it on where to pay it the due block rewards.

## Transaction Inputs <a id="Transaction-Inputs"></a>

In a coinbase transaction, the numTxIns is set to 0 and there are no inputs.

## Transaction Outputs <a id="Transaction-Outputs"></a>

### Serialization <a id="Serialization.1"></a>

| **Field Name** | **Type** | **Size in bytes** | **Description** |
| :--- | :--- | :--- | :--- |
| value | uint64 **\(was int64 in Bitcoin\)** | 8 | Coinbase amount in Sompolinskys |
| scriptPubkeyLen | VarInt | 1-9 | Length of _scriptPubkey_ field in bytes |
| scriptPubKey | DAGScript | scriptPubkeyLen | Script specifying the conditions under which the transaction output can be claimed |

## Serialized Format <a id="Serialized-Format"></a>

Coinbase transactions are marked by having the hash value set to 0, and the index set to -1 \(or actually 2^{32}-1, as this is an unsigned value\).

A coinbase struct is the same as a [regular transaction](./) struct. However, in coinbase transactions, numTxIns is set to 0 and there are no txIns.

A serialized coinbase transaction is comprised of the fields of the following table, in the order they appear in the table, and with the types described in the table.

| **Field Name** | **Type** | **Size in bytes** | **Description** |
| :--- | :--- | :--- | :--- |
| version | int32 | 4 | Transaction format version \(currently 1\) |
| numTxIns | VarInt | 1-9 | Number of transaction input entries in `TxIns`. Set to 0 for coinbase transactions. |
| txIns | txIn array | varies | Empty |
| numTxOuts | VarInt | 1-9 | Number of transaction output entries in _txOuts. numTxOuts_ must equal _numTxIns_ |
| txOuts | txOut array | varies | [Transaction outputs](./#Transaction-Outputs). |
| lockTime | uint64 | 8 | Timestamp that indicates the earliest time a transaction can be added to the blockDAG |
| subnetworkID | Byte array | 20 | ID of subnetwork this transaction belongs to \(coinbase transactions belong to subnetwork 1\) |
| gas | uint64 | 8 | A measure of computation cost of this transaction |
| payloadHash | Byte array | 32 | Hash of data in _payload_ field |
| payloadLength | VarInt | 1-9 | Length of _payload_ field in bytes |
| payload | Byte array | payloadLength | Data that specifies the destination for block rewards to be paid by future blocks' coinbase transactions. The format is: `BlueScore` \(uint64\), `scriptPubKeyLen` \(VarInt\), `scriptPubKey`, `ExtraData` |

## Example <a id="Example"></a>

01000000000300f2052a010000001976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088ac00f2052a010000001976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088ac00f2052a010000001976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088ac000000000000000001000000000000000000000000000000000000000000000000000000ae812bdaaed78fafecf97d2c72899212d8e56b94287f76de8fed49e53f42b66c2a1976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088ac123ce465187f8ec82f6b61737061642f

### Breakdown <a id="Breakdown"></a>

```text
01000000                                                         --   version 1
00                                                               --   numTxIns (coinbase transactions have no inputs)
03                                                               --   numTxOuts (this transaction has 3 outputs)
                                                                 --   txOuts[0]
                                                                 --   ---------
00f2052a01000000                                                 --     value
19                                                               --     scriptPubKeyLen
                                                                 --     scriptPubKey
                                                                 --     ------------
76                                                               --       opDup
a9                                                               --       opHash160
                                                                 --       opData_20
                                                                 --       ---------
14                                                               --         code
1234e3080bbd4135ca3b330fdc42699f2997cdc0                         --         data[20]
88                                                               --       opEqualverify
ac                                                               --       opChecksig
                                                                 --   txOuts[1]
                                                                 --   ---------
00f2052a01000000                                                 --     value
19                                                               --     scriptPubKeyLen
                                                                 --     scriptPubKey
                                                                 --     ------------
76                                                               --       opDup
a9                                                               --       opHash160
                                                                 --       opData_20
                                                                 --       ---------
14                                                               --         code
1234e3080bbd4135ca3b330fdc42699f2997cdc0                         --         data[20]
88                                                               --       opEqualverify
ac                                                               --       opChecksig
                                                                 --   txOuts[2]
                                                                 --   ---------
00f2052a01000000                                                 --     value
19                                                               --     scriptPubKeyLen
                                                                 --     scriptPubKey
                                                                 --     ------------
76                                                               --       opDup
a9                                                               --       opHash160
                                                                 --       opData_20
                                                                 --       ---------
14                                                               --         code
1234e3080bbd4135ca3b330fdc42699f2997cdc0                         --         data[20]
88                                                               --       opEqualverify
ac                                                               --       opChecksig
0000000000000000                                                 --   lockTime
0100000000000000000000000000000000000000                         --   subnetworkID[20]
0000000000000000                                                 --   gas
ae812bdaaed78fafecf97d2c72899212d8e56b94287f76de8fed49e53f42b66c --   payloadHash[32]
2a                                                               --   payloadLength (VARINT)
1976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088ac123ce465187f8ec82f6b61737061642f -- payload[42] (payload is 0x2a = 42 in this case)
```



