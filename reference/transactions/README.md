# Transactions

A transaction is a transfer of Kaspa value, similar to a Bitcoin transaction. For an introduction to Bitcoin transactions, see [Mastering Bitcoin Chapter 6](https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch06.asciidoc).

Transactions are grouped together in [blocks](../blocks/), which are mined and added to the public ledger.

Like Bitcoin, Kaspa uses the [UTXO](../txo/utxo.md) model, in which ownership of coins is spread across unspent transaction outputs, which, in order to be spent, are used as inputs to new transactions.

Kaspa has several transaction types:

* [Native transaction](./) - a regular Kaspa transaction on the base layer.
* [Coinbase transaction](coinbase-transaction.md) - a transaction in a [chain block](../consensus/selected-parent-chain.md) that pays block rewards to [merged blocks](../consensus/merged-blocks.md), by minting new coins and paying fees for [accepted transactions](../consensus/accepted-transactions.md).
* [Subnetwork registration transaction](registry-transaction.md) - a transaction that registers a new [subnetwork](../../components/kaspad-full-node/reference/subnetworks-1.md).
* Subnetworks transactions - non-base layer transactions used for smart contracts.

## Transaction Hash <a id="Transaction-Hash"></a>

‌A transaction hash is a [hash](../serialized-data-formats/hash.md) of the following:‌

1. [Version](./#Transaction-Inputs)
2. The number of [transaction inputs](./#Transaction-Inputs)
3. [Transaction inputs](./#Transaction-Inputs), **including** the signature scripts
4. The number of [transaction outputs](./#Transaction-Outputs)
5. [Transaction outputs](./#Transaction-Outputs)
6. [Lock time](./#Locktime)
7. [Subnetwork ID](./#Subnetwork-ID)
8. [Gas](./#Gas) \(only in non-native transactions\)
9. [Payload hash](./#Payload-Hash) \(only in non-native transactions\)

‌Transaction hash is used in Kaspa only when building the [block hash merkle root](../blocks/block-header/#hash-merkle-root).

## Transaction ID <a id="Transaction-ID"></a>

‌A transaction ID is a [hash](../serialized-data-formats/hash.md) of the following:‌

1. [Version](./#Transaction-Inputs)
2. The number of [transaction inputs](./#Transaction-Inputs)
3. [Transaction inputs](./#Transaction-Inputs), **not including** the signature scripts
4. The number of [transaction outputs](./#Transaction-Outputs)
5. [Transaction outputs](./#Transaction-Outputs)
6. [Lock time](./#Locktime)
7. [Subnetwork ID](./#Subnetwork-ID)
8. [Gas](./#Gas) \(only in non-native transactions\)
9. [Payload hash](./#Payload-Hash) \(only in non-native transactions\)

‌In Kaspa, transaction ID is [non-malleable](transaction-malleability.md#Transaction-Non-Malleability) and is used for referring to transactions everywhere, except in the [block hash merkle root](../blocks/block-header/#hash-merkle-root), in which the [transaction hash](./#Transaction-Hash) is used.

> **Note**: Bitcoin went through a journey of solving [transaction malleability](transaction-malleability.md) while having to keep backward compatibility, with [BIP\_0062](https://en.bitcoin.it/wiki/BIP_0062) and then [Segwit](https://en.bitcoin.it/wiki/Segregated_Witness). Kaspa solves transaction malleability simply by using the [non-malleable](transaction-malleability.md#Transaction-Non-Malleability) Transaction ID instead.

## Version <a id="Transaction-Inputs"></a>

TBD:

## Transaction Inputs <a id="Transaction-Inputs"></a>

‌Kaspa transaction inputs are the same as [Bitcoin transaction inputs](https://bitcoin.org/en/glossary/input), except for the following two changes:‌

* Kaspa does not have the witness field because it does not need it to solve [transaction malleability](transaction-malleability.md). In Kaspa, the transaction ID is [non-malleable](transaction-malleability.md#Transaction-Non-Malleability) in the first place.
* In Kaspa the sequence field is `uint64` instead of `uint32`. This allows it to express timestamps that go further than the year 2106.‌

### Transaction Input Serialization <a id="Transaction-Input-Serialization"></a>

| Field Name | Type | **B**ytes | Description |
| :--- | :--- | :--- | :--- |
| prevTxId | uint256 | 32 | [Hash](../serialized-data-formats/hash.md) of a past transaction |
| prevTxIndex | uint32 | 4 | The index of a [transaction output](../txo/) within the past transaction referenced by `prevTxId` |
| scriptSigLen | [VarInt](../serialized-data-formats/variable-length-fields/varint.md) | 1-9 | Length of `scriptSig` field in bytes |
| scriptSig | \[\]byte | scriptSigLen | Script to satisfy spending condition of the previous transaction output, referenced by `prevTxId`, `prevTxIndex` |
| sequence | uint64 | 8 | Transaction input sequence number |

Sequence is the same as nSequence in Bitcoin, except it is `uint64` instead of `uint32`.

## Transaction Outputs <a id="Transaction-Outputs"></a>

‌Kaspa transaction outputs are the same as [Bitcoin transaction outputs](https://bitcoin.org/en/glossary/output), except that they are `uint64` instead of `int64`.‌ Read more about Kaspa transaction outputs [here](../txo/).

### Transaction Output Serialization <a id="Transaction-Output-Serialization"></a>

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| value | uint64 | 8 | Transaction amount in TBD: Sompolinskys |
| scriptPubkeyLen | VarInt | 1-9 | Length of `scriptPubkey` field in bytes |
| scriptPubKey | \[\]byte | scriptPubkeyLen | Script specifying the conditions under which the transaction output can be claimed |

## LockTime <a id="Locktime"></a>

LockTime is a timestamp that indicates the earliest time a transaction can be added to the blockDAG. This is the same as in Bitcoin, except it is `uint64` instead of `uint32`.

* [ ] TBD: The table below is work in progress.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Operation</th>
      <th style="text-align:left">Transaction.LockTime</th>
      <th style="text-align:left">Transaction_Input.Sequence</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Replace By Fee (RBF)</td>
      <td style="text-align:left">A | B | 0</td>
      <td style="text-align:left">&lt; Max</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Mine Not Before</b>
      </td>
      <td style="text-align:left">Block Number</td>
      <td style="text-align:left">!(all(Max)) // not all Max</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Spend not Before<br /></b>Check LockTime Verify (CLTV)</td>
      <td style="text-align:left">
        <p>&#x2265; 500,000,000 sec : Unix epoch;</p>
        <p>&lt; 500,000,000 sec : Block Number</p>
      </td>
      <td style="text-align:left">Max</td>
    </tr>
    <tr>
      <td style="text-align:left">Check Sequence Verify (CSV)</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">&lt; Max
        <br />and 23&#x2B3;&#x1D48; bit = 1</td>
    </tr>
  </tbody>
</table>

## Subnetwork ID <a id="Subnetwork-ID"></a>

Transactions specify which subnetwork they belong to. You can read about subnetworks [here](../../components/kaspad-full-node/reference/subnetworks-1.md).

## Gas <a id="Gas"></a>

Gas is a measure of computation cost for subnetwork transactions. Gas is explained on the [subnetworks page](../../components/kaspad-full-node/reference/subnetworks-1.md).

## Payload <a id="Payload"></a>

‌Payload is a field inside a transaction, used to store arbitrary data and code, that is used by [subnetworks](../../components/kaspad-full-node/reference/subnetworks-1.md) to run logic.

‌[Coinbase transactions](coinbase-transaction.md) \(transaction on the Coinbase Subnetwork\) use the payload field to specify to future blocks where to pay block rewards to.‌

Subnetwork transactions may use the payload field to store data and logic relevant to their subnetwork.

## Payload Hash <a id="Payload-Hash"></a>

‌Payload hash is a [hash](../serialized-data-formats/hash.md) of the arbitrary data field, [payload](./#Payload), in a transaction.

‌While the payload is not part of the Kaspa transaction hash, the payload hash field still is. This allows uninterested nodes to ignore the payload of subnetwork transactions, while the payload still gets validated.‌

## Transaction Mass <a id="Transaction-Mass"></a>

‌Transaction mass is a measure of the [mass](../blocks/#Mass) of a transaction, as follows:

```text
Txn Mass [gram] =      1 * size of txn in bytes 
                +     10 * size of txn's output scripts in bytes 
                + 10,000 * number of signature operations it runs‌
```

## Transaction Confirmations <a id="Transaction-Confirmations"></a>

‌In Kaspa, a transaction receives its first confirmation when a block on the [selected parent chain](../consensus/selected-parent-chain.md) accepts it. More confirmations for the transaction are added as more [blue blocks](../consensus/blue-set/#blue-block) are added on top of the [accepting block](../consensus/transactions-accepting-blocks.md) of the transaction.

‌The number of confirmations a transaction has is the difference in [blue score](../consensus/blue-score.md) between the [selected tip](../consensus/selected-parent.md#selected-tip-of-the-blockdag) of the blockDAG and the transaction's accepting block, incremented by 1:‌

```text
Txn.confirmations = DAG.selected_tip.blue_score - Txn.accepting_block.blue_score + 1
```

Read more about confirmations [here](../consensus/confirmations/).

## Schnorr Signatures <a id="Schnorr-Signatures"></a>

Kaspa uses Schnorr signatures instead of ECDSA signatures. It uses Bitcoin Cash’s version. For motivation and specs, see [the BCH docs](https://github.com/bitcoincashorg/bitcoincash.org/blob/master/spec/2019-05-15-schnorr.md).

### Txsigner <a id="Txsigner"></a>

## Serialized Format <a id="Serialized-Format"></a>

A serialized regular transaction is comprised of the fields of the following table, in the order they appear in the table, and with the types described in the table.

| **Field Name** | **Type** | **Size in bytes** | **Description** |
| :--- | :--- | :--- | :--- |
| version | int32 | 4 | Transaction format version \(currently 1\) |
| numTxIns | VarInt | 1-9 | Number of transaction input entries in `txIns` |
| txIns | \[\]txIn | variable | [Transaction inputs](./#Transaction-Inputs-1) |
| numTxOuts | VarInt | 1-9 | Number of transaction output entries in `txOuts` |
| txOuts | \[\]txOut | variable | [Transaction outputs](./#Transaction-Outputs) |
| lockTime | uint64 | 8 | Timestamp that indicates the earliest time a transaction can be added to the blockDAG |
| subnetworkID | \[\]byte | 20 | ID of [subnetwork](../../components/kaspad-full-node/reference/subnetworks-1.md) this transaction belongs to |
| gas | uint64 | 8 | A measure of the computational cost for the execution of the arbitrary code in the payload of this transaction. See [subnetworks](../../components/kaspad-full-node/reference/subnetworks-1.md) for more information. |
| payloadHash | \[\]byte | 32 | Hash of data in `payload` field |
| payloadLength | VarInt | 1-9 | Length of `payload` field in bytes |
| payload | \[\]byte | payloadLength | Arbitrary data and code relevant to a specific [subnetwork](../../components/kaspad-full-node/reference/subnetworks-1.md) |

### Example <a id="Example"></a>

0100000001360329fa59fd36071820b164645c234728f1edc2f9d462cb68a423b31f050000ffffffff00ffffffffffffffff0100f2052a010000001976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088ac00000000000000000100000000000000000000000000000000000000000000000000000052cd4f97ad5f073e572a1b3e733c12294d6463a2c75dded90a5bb0bdce9a85562a1976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088acbeb7ef74aa9be1222f6b61737061642f

### Breakdown <a id="Breakdown"></a>

```text
01000000                                                         --   version 1
01                                                               --   numTxIns (this transaction has 1 input)
                                                                 --   txIns[0]
                                                                 --   --------
360329fa59fd36071820b164645c234728f1edc2f9d462cb68a423b31f050000 --     prevTxId
ffffffff                                                         --     prevTxIndex
00                                                               --     scriptSigLen
ffffffffffffffff                                                 --     sequence
01                                                               --   numTxOuts (this transaction has 1 output)
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
0000000000000000                                                 --   lockTime
0100000000000000000000000000000000000000                         --   subnetworkID[20]
0000000000000000                                                 --   gas
52cd4f97ad5f073e572a1b3e733c12294d6463a2c75dded90a5bb0bdce9a8556 --   payloadHash[32]
2a                                                               --   payloadLength (VARINT)
1976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088acbeb7ef74aa9be1222f6b61737061642f -- payload[42] (payload is 0x2a = 42 in this case)
```

