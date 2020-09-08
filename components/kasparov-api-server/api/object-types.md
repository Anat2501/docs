---
description: This page lists types of objects appearing in API call responses.
---

# Object Types

Each object includes the JSON object structure, an explanation of each returned value and an example. Some fields may be omitted from some objects, depending on the API call response.

Unless specified otherwise, all hashes and IDs are hex-encoded [SHA-256 hashes](../../../reference/serialized-data-formats/hash.md).

## Transaction

The transaction object contains information about the [transaction](../../../glossary.md#transaction) and a list of sources and destinations for the transferred money \(inputs and outputs\). Some of the fields are optional and appear only if the transaction is accepted by a [block](../../../glossary.md#block), and some only if the transaction is non-native \(used by a [subnetwork](../../../glossary.md#subnetwork) other than '1'\).

```go
{
    transactionHash         // string   Hex-encoded hash
    transactionId           // string   Hex-encoded hash
    acceptingBlockHash      // string   Present only if transaction is accepted
    acceptingBlockBlueScore // int              Present only if transaction is accepted
    subnetworkId            // string   Hex-encoded RIPEMD160 hash
    lockTime                // int      The earliest time or blue score the 
                            //          transaction may be included in a block
    gas                     // int      Present only if SubnetworkID is not the 
                            //          native subnetwork
    payloadHash             // string   Present only if SubnetworkID is not the 
                            //          native subnetwork
    payload                 // string   Base64-encoded data. Present only if 
                            //          SubnetworkID is not native
    inputs                  // Array of TransactionInput
    outputs                 // Array of TransactionOutput
    mass                    // int      The transaction's mass
    version                 // int      The transaction's version
    raw                     // String   Hex-encoded raw transaction
    confirmations           // int      The number of confirmations the transaction has,
                            //          at the time of the response.
}
```

### Fields and Data types

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">transactionHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded <a href="../../../glossary.md#transaction-hash">transaction hash</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">transactionId</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded <a href="../../../glossary.md#transaction-id">transaction ID</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">acceptingBlockHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>The hash of the <a href="../../../glossary.md#transaction-accepting-block">accepting block</a>
        </p>
        <p><em>Present only if the transaction is accepted</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">acceptingBlockBlueScore</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">
        <p>The <a href="../../../glossary.md#blue-score">blue score</a> of the accepting
          block</p>
        <p><em>Present only if the transaction is accepted</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">subnetworkId</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded RIPEMD160 hash</td>
    </tr>
    <tr>
      <td style="text-align:left">lockTime</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The earliest time or blue score the transaction may be included in a block.
        <br
        />If less than &#x2049;, lockTime is parsed as blue score. Otherwise, lockTime
        is parsed as Unix epoch time.</td>
    </tr>
    <tr>
      <td style="text-align:left">gas</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The <a href="../../../glossary.md#gas">gas</a> cost of running the transaction
        <br
        /><em>Present only if subnetworkId is not 1 - the native subnetwork</em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">payloadHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">The hash of the <a href="../../../glossary.md#payload">payload</a>
        <br /><em>Present only if subnetworkId is not</em>  <em>1 - the native subnetwork</em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">payload</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Base64-encoded data.
        <br /><em>Present only if subnetworkId is not 1 -- the native subnetwork</em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">inputs</td>
      <td style="text-align:left">array of transactionInput</td>
      <td style="text-align:left">An array of inputs used as money sources by the transaction</td>
    </tr>
    <tr>
      <td style="text-align:left">outputs</td>
      <td style="text-align:left">array of transactionOutput</td>
      <td style="text-align:left">An array of outputs, used as money destinations by the transaction</td>
    </tr>
    <tr>
      <td style="text-align:left">mass</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The transaction&apos;s mass</td>
    </tr>
    <tr>
      <td style="text-align:left">version</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The transaction&apos;s version</td>
    </tr>
    <tr>
      <td style="text-align:left">raw</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded raw transaction</td>
    </tr>
    <tr>
      <td style="text-align:left">confirmations</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The number of confirmations the transaction has, at the time of the response</td>
    </tr>
  </tbody>
</table>

### Example

```javascript
{
    "transactionHash":"9c6837e1bb65e4d88a23499132a4d45892a6558bdebe3366f3b519d307910364",
    "transactionId":"353419629397723233af387c4fbe45c3546fa04b405db2d2263d703676f94396",
    "acceptingBlockHash":"0000208e9cb15a95b32a0576db5ff18591329cf89c3d92f2fedc3a8fb270d4de",
    "acceptingBlockBlueScore":24062,
    "subnetworkId":"0000000000000000000000000000000000000002",
    "lockTime":0,
    "gas":,
    "payloadHash":"ebd7ca4362298cf2805041ab87a17b03775509de609fb90467f41b174be00931",
    "payload":"1976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088acda2e915ff92971a82f6b61737061642f",
    "inputs":
    [
        {
            "previousTransactionId":"589f1ba1b0603f0403a25a7a383040c57451f0a141bd35a8871e9b26fecc3c2f",
            "previousTransactionOutputIndex":0,
            "signatureScript":"41a7bc12015a39d72e73cc5fb50727a1cdb66ed71c987cace4a31ca812fe6607ea12927f2489c7b41716fccfd5ad6d69401165107961f095b120d6cef44361b61c012103a04b5afba5f23b97845451c02753e282a94340c4f81d866efd50f04742ff1a3c",
            "sequence":"18446744073709551615",
            "address":"kaspadev:qqfrfccgpw75zdw28veslhzzdx0jn97dcqrat4tmuh"
        }
    ],
    "outputs":
    [
        {
            "value":"5018250734",
            "scriptPubKey":"76a9141234e3080bbd4135ca3b330fdc42699f2997cdc088ac",
            "address":"kaspadev:qqfrfccgpw75zdw28veslhzzdx0jn97dcqrat4tmuh",
            "index":0
        }
    ],
    "mass":196,
    "version":1,
    "raw":"0100000001654ae3c60654d00059024f09e11cb5573f76ca6908558f731fb344fb9c0d0000ffffffff00ffffffffffffffff01ee6d1c2b010000001976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088ac0000000000000000010000000000000000000000000000000000000000000000000000003109e04b171bf46704b99f60de095577037ba187ab415080f28c296243cad7eb2a1976a9141234e3080bbd4135ca3b330fdc42699f2997cdc088acda2e915ff92971a82f6b61737061642f",
    "confirmations":41152
}
```

## TransactionInput

The transactionInput object contains information about one source of money used in a transaction \(an [input](../../../glossary.md#transaction-inputs)\). The field transactionId is omitted when the transactionInput is part of the Transaction.

```go
{
    transactionId                   // string   Hex-encoded hash. Omitted when 
                                    //          part of a Transaction object
    previousTransactionId           // string
    previousTransactionOutputIndex  // int
    signatureScript                 // string   Base64-encoded data
    sequence                        // int
    address                         // string   A Bech32-encoded address
}
```

### Fields and Data types

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">transactionId</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>Hex-encoded transaction ID</p>
        <p>Omitted, when returned as part of Transaction object</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">previousTransactionId</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded transaction ID</td>
    </tr>
    <tr>
      <td style="text-align:left">previousTransactionOutputIndex</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The index of this output among outputs within the previous transaction</td>
    </tr>
    <tr>
      <td style="text-align:left">signatureScript</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Base64-encoded data</td>
    </tr>
    <tr>
      <td style="text-align:left">sequence</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Same as in bitcoin but <code>uint64</code> instead of <code>uint32</code>.
        This allows it to express timestamps that go further than the year 2106</td>
    </tr>
    <tr>
      <td style="text-align:left">address</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">A Bech32-encoded address</td>
    </tr>
  </tbody>
</table>

### Example

```javascript
{
    "previousTransactionId":"589f1ba1b0603f0403a25a7a383040c57451f0a141bd35a8871e9b26fecc3c2f",
    "previousTransactionOutputIndex":0,
    "signatureScript":"41a7bc12015a39d72e73cc5fb50727a1cdb66ed71c987cace4a31ca812fe6607ea12927f2489c7b41716fccfd5ad6d69401165107961f095b120d6cef44361b61c012103a04b5afba5f23b97845451c02753e282a94340c4f81d866efd50f04742ff1a3c",
    "sequence":"18446744073709551615",
    "address":"kaspadev:qqfrfccgpw75zdw28veslhzzdx0jn97dcqrat4tmuh"
}
```

## TransactionOutput

The transactionOutput object contains information about one destination for the money in a transaction \(an [output](../../../glossary.md#transaction-outputs)\). Some fields are omitted depending on the request.

```go
{
    transactionId           // string   Hex-encoded hash. Omitted when part of a 
                            //          Transaction object
    index                   // int      Index of this output
    value                   // int
    scriptPubKey            // string   Base64-encoded data
    address                 // string   Bech32-encoded address. Present only if 
                            //          ScriptPubKey is P2PKH or P2SH
                            //          Omitted in the get UTXOs by address response
    acceptingBlockHash      // string   Omitted except in the get UTXOs by address response 
    acceptingBlockBlueScore //          Omitted except in the get UTXOs by address response.
                            //          Equals maxuint64 for Outputs of transactions in the Block
    isCoinbase              // int      Omitted except in the get UTXOs by address response
    confirmations           // int      Omitted except in the get UTXOs by address response
    isSpendable             // string   Omitted except in the get UTXOs by address response
                            //          This can also be calculated by the requesting client.
                            //          True for coinbase if #Confirmations >= 100, and for
                            //          non-coinbase if #Confirmations >=1, where #Confs =
                            //          SelectedTip.BlueScore - AcceptingBlock.BlueScore + 1
    isSpent                 // string   Omitted in get UTXOs by address response 
}
```

### Fields and Data types

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">transactionId</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>Hex-encoded transaction ID</p>
        <p>Omitted when returned as part of Transaction object</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">index</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Index of this output</td>
    </tr>
    <tr>
      <td style="text-align:left">value</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Amount of money to move to this output</td>
    </tr>
    <tr>
      <td style="text-align:left">scriptPubKey</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Base64-encoded data</td>
    </tr>
    <tr>
      <td style="text-align:left">address</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>Bech32-encoded address</p>
        <p>Omitted if scriptPubKey is neither P2PKH nor P2SH</p>
        <p>Omitted in the get UTXOs by address response</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">acceptingBlockHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Omitted except in the get UTXOs by address response</td>
    </tr>
    <tr>
      <td style="text-align:left">acceptingBlockBlueScore</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">
        <p>Omitted except in the get UTXOs by address response.</p>
        <p>Equals maxuint64 for Outputs of transactions in the Block</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">isCoinbase</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Omitted except in the get UTXOs by address response</td>
    </tr>
    <tr>
      <td style="text-align:left">confirmations</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Omitted except in the get UTXOs by address response</td>
    </tr>
    <tr>
      <td style="text-align:left">isSpendable</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">
        <p>Omitted except in the get UTXOs by address response</p>
        <p>This can also be calculated by the requesting client. True for <a href="../../../glossary.md#coinbase-transaction">coinbase</a> if
          #Confirmations &gt;= 100, and for non-coinbase if #Confirmations &gt;=1,
          where #Confirmations = SelectedTip.BlueScore - AcceptingBlock.BlueScore
          + 1</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">isSpent</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">Omitted in the get UTXOs by address response</td>
    </tr>
  </tbody>
</table>

### Example

```javascript
{
    "transactionId":"589f1ba1b0603f0403a25a7a383040c57451f0a141bd35a8871e9b26fecc3c2f",
    "value":5018250734,
    "scriptPubKey":"76a9141234e3080bbd4135ca3b330fdc42699f2997cdc088ac",
    "address":"kaspadev:qqfrfccgpw75zdw28veslhzzdx0jn97dcqrat4tmuh",
    "acceptingBlockHash":"0000208e9cb15a95b32a0576db5ff18591329cf89c3d92f2fedc3a8fb270d4de",
    "acceptingBlockBlueScore":24062,
    "isCoinbase":False,
    "confirmations":41152,
    "isSpendable":True
}
```

## Block

The block object contains information about a [block](../../../glossary.md#block) in the [DAG](../../../glossary.md#dag). The field acceptingBlockHash is present only if the block is accepted.

```go
{
    blockHash               // string    Hex-encoded hash
    version                 // int        
    hashMerkleRoot          // string    Hex-encoded hash
    acceptedIdMerkleRoot    // string    Hex-encoded hash
    utxoCommitment          // string    Hex-encoded hash
    timestamp               // int       Unix epoch timestamp. Seconds since 
                            //           01/01/1970    
    bits                    // int        
    nonce                   // int        
    parentBlockHashes       // array of  Hex-encoded hashes of parent blocks
    acceptingBlockHash      // string    Hex-encoded hash. Present only if 
                            //           this block is accepted
    acceptedBlockHashes     // array of  Hex-encoded hashes of accepted blocks, at the time of the response.
    blueScore               // int        
    isChainBlock            // bool      True if the block is part of the selected 
                            //           parent chain, at the time of the response.
    confirmations           // int       This is the number of confirmations at the
                            //           time of the response.
    mass                    // int         
}
```

### Fields and Data types

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">blockHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded hash</td>
    </tr>
    <tr>
      <td style="text-align:left">version</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">block version</td>
    </tr>
    <tr>
      <td style="text-align:left">hashMerkleRoot</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded hash</td>
    </tr>
    <tr>
      <td style="text-align:left">acceptedIdMerkleRoot</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded hash</td>
    </tr>
    <tr>
      <td style="text-align:left">utxoCommitment</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded hash</td>
    </tr>
    <tr>
      <td style="text-align:left">timestamp</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Unix epoch <a href="../../../glossary.md#time-timestamp">timestamp</a> (seconds
        since 01/01/1970)</td>
    </tr>
    <tr>
      <td style="text-align:left">bits</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">A 4 bytes &#x201C;compact&#x201D; format that translate to an 8 digit
        hexadecimal number, representing the target threshold for proof of work</td>
    </tr>
    <tr>
      <td style="text-align:left">nonce</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">An arbitrary 64-bit number used by the miner when creating the block,
        to reach a header hash lower than the target bits</td>
    </tr>
    <tr>
      <td style="text-align:left">parentBlockHashes</td>
      <td style="text-align:left">strings array</td>
      <td style="text-align:left">Array of Hex-encoded hashes of the parent blocks</td>
    </tr>
    <tr>
      <td style="text-align:left">acceptingBlockHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>Hex-encoded hash</p>
        <p>Omitted unless the block is <a href="../../../glossary.md#accepted-blocks-blues">accepted</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">acceptedBlockHashes</td>
      <td style="text-align:left">strings array</td>
      <td style="text-align:left">Array of Hex-encoded hashes of the blocks accepted by this block at the
        time of the response</td>
    </tr>
    <tr>
      <td style="text-align:left">blueScore</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The number of <a href="../../../glossary.md#blue-block">blue blocks</a> this
        block has in its <a href="../../../glossary.md#past">past</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">isChainBlock</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">True if this block is part of the <a href="../../../glossary.md#selected-parent-block-chain-selected-parent-chain">selected parent chain</a>,
        at the time of the response.</td>
    </tr>
    <tr>
      <td style="text-align:left">confirmations</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The number of <a href="../../../glossary.md#confirmations">confirmations</a> the
        block has, at the time of the response.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../../glossary.md#mass">mass</a>
      </td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">1 gram per byte of block header + sum of all Tx mass in the block</td>
    </tr>
  </tbody>
</table>

### Example

```javascript
{
    "blockHash":""
    "version":
    "hashMerkleRoot":""
    "acceptedIdMerkleRoot":""
    "utxoCommitment":""
    "timestamp":
    "bits":
    "nonce":
    "parentBlockHashes":[]
    "acceptingBlockHash":""
    "acceptedBlockHashes":[]
    "blueScore":
    "isChainBlock":
    "confirmations":
    "mass":
}
```

## FeeEstimate

The feeEstimate object contains information about the estimated [fee](../../../glossary.md#transaction-fees) costs to include a transaction in a block. The values are in Satoshi per gram of transaction [mass](../../../glossary.md#mass).

```go
{
    highPriority      // int    estimated fee for near-immediate confirmation
    normalPriority    // int    estimated fee for confirmation in about 1 minute
    lowPriority       // int    estimated fee for confirmation within 5 minutes
}
```

### Fields and Data types

| Name | Type | Description |
| :--- | :--- | :--- |
| highPriority | int | The estimated fee in Satoshi per gram of transaction mass, for the transaction to be included within the next 10 blocks, with a probability of 90%. With 1 block per second, this translates to near immediate confirmation. |
| normalPriority | int | The estimated fee in Satoshi per gram of transaction mass, for the transaction to be included within the next 100 blocks, with a probability of 90%. With 1 block per second, this translates to confirmation in about a minute. |
| lowPriority | int | The estimated fee in Satoshi per gram of transaction mass, for the transaction to be included within the next 300 blocks, with a probability of 90%. With 1 block per second, this translates to confirmation within 5 minutes. |

{% hint style="warning" %}
Numbers are bound to change during TestNet.

The fee estimates is currently implemented as a stub.
{% endhint %}

### Example

```javascript
{
    "highPriority":900
    "normalPriority":300
    "lowPriority":100
}
```

