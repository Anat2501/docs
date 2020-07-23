---
description: This page lists the calls that can be requested from the Kasparov API.
---

# API Methods

All API calls return `200 OK` if no error was encountered.  
If an error occurs, an error code is returned with the following body:

```go
{
    errorCode       // int       HTTP error code
    errorMessage    // string    Human-readable error description
}
```

Example:

```javascript
{
    "errorCode":404,
    "errorMessage":"No transaction was found for the given address."
}
```

In addition, some calls return objects. The object types are listed [here](object-types.md).

## List of API Request Calls

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/transaction/id/:txid" %}
{% api-method-summary %}
/transaction/id/:txid
{% endapi-method-summary %}

{% api-method-description %}
Searches for a transaction by its non-malleable ID.  
Response: Transaction
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="txid" type="string" required=true %}
64bit integer for requested transaction ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is a single transaction object.
{% endapi-method-response-example-description %}

```javascript
{
    "transactionId":"string",
    "transactionHash":"string",
    "acceptingBlockHash":"string",
    "acceptingBlockBlueScore":int,
    "subnetworkId":"string",
    "lockTime":int,
    "gas":int,
    "payloadHash":"string",
    "payload":"string",
    "inputs":
    [
        {
            "previousTransactionId":"string",
            "previousTransactionOutputIndex":"string",
            "scriptSig":"string",
            "sequence":"string",
        },
        {
            "previousTransactionId":"string",
            "previousTransactionOutputIndex":"string",
            "scriptSig":"string",
            "sequence":"string",
        }
    ],
    "outputs":
    [
        {
            "value":"string",
            "scriptPubKey":"string",
            "address":"string",
        },
        {
            "value":"string",
            "scriptPubKey":"string",
            "address":"string",
        }
    ],
    "mass":int
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":404,
    "errorMessage":"No trasnaction with the given txid was found."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422,
    "errorMessage":"The given txid is not a hex-encoded 32-byte hash."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500,
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/transaction/hash/:txhash" %}
{% api-method-summary %}
/transaction/hash/:txhash
{% endapi-method-summary %}

{% api-method-description %}
Searches for transaction by its hash.  
Response: Transaction
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="txhash" type="string" required=true %}
Hex-encoded 32 bytes, representing the 64 character transaction ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is a single transaction object.
{% endapi-method-response-example-description %}

```javascript
{
    "transactionId":"string",
    "transactionHash":"string",
    "acceptingBlockHash":"string",
    "acceptingBlockBlueScore":int,
    "subnetworkId":"string",
    "lockTime":int,
    "gas":int,
    "payloadHash":"string",
    "payload":"string",
    "inputs":
    [
        {
            "previousTransactionId":"string",
            "previousTransactionOutputIndex":"string",
            "scriptSig":"string",
            "sequence":"string"
        },
        {
            "previousTransactionId":"string",
            "previousTransactionOutputIndex":"string",
            "scriptSig":"string",
            "sequence":"string"
        }
    ],
    "outputs":
    [
        {
            "value":"string",
            "scriptPubKey":"string",
            "address":"string"
        },
        {
            "value":"string",
            "scriptPubKey":"string",
            "address":"string"
        }
    ],
    "mass":
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":404,
    "errorMessage":"No transactions for the given txhash was found."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422,
    "errorMessage":"The given txhash is not a hex-encoded 32-byte hash."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500,
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/transactions/address/:address/count" %}
{% api-method-summary %}
/transactions/address/:address/count
{% endapi-method-summary %}

{% api-method-description %}
Searches for all transactions where the given address is either an input or an output.  
Returns an integer representing the total number of transactions for the given address.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="address" type="string" required=true %}
A Bech32-encoded address
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
12345
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/transactions/address/:address\[&skip=5000&limit=1000\]" %}
{% api-method-summary %}
/transactions/address/:address
{% endapi-method-summary %}

{% api-method-description %}
Searches for all transactions where the given address is either an input or an output.    
Returns: Array of Transaction, sorted by API Server Transaction ID, which is sorted according to accepting block date in ascending order, which is almost according to transaction date in ascending order, earliest transaction first. \(The reason it is almost but not exactly chronological order stems from the high block creation rate of the DAG. If two blocks are created in parallel in two different parts of the DAG, it is hard to say which one came first, because of the lack of global clock.\)  
Accepts optional limit and skip parameters.  
  -  limit \(default=100\) allows limiting the number of returned transactions between 1 and 1000.  
  -  skip \(default=0\) allows to skip a given number of transactions.  
If there are no transactions for the given address, returns an empty array.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="address" type="string" required=true %}
A Bech32-encoded address
{% endapi-method-parameter %}

{% api-method-parameter name="skip" type="integer" required=false %}
If provided, skips the first given number of transactions, ordered by ID \(default 0\); If less than 0 is specified, returns an error.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="integer" required=false %}
If provided, limits the response to the given number of  transactions \(default 100, max 1000\); If more than 1000 or less than 1 is specified, returns an error
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is an array of transaction objects. In the following example, the array consists of two transaction objects.
{% endapi-method-response-example-description %}

```javascript
{
    "transactions":[
        {
            "transactionHash":"string",
            "transactionId":"string",
            "acceptingBlockHash":"string",
            "acceptingBlockBlueScore":int,
            "subnetworkId":"string",
            "lockTime":integer,
            "gas":integer,
            "payloadHash":"string",
            "payload":"string",
            "inputs":
            [
                {
                    "previousTransactionId":"string",
                    "previousTransactionOutputIndex":int,
                    "signatureScript":"string",
                    "sequence":"string",
                    "address":"string"
                },
                {
                    "previousTransactionId":"string",
                    "previousTransactionOutputIndex":int,
                    "signatureScript":"string",
                    "sequence":"string",
                    "address":"string"
                }
            ],
            "outputs":
            [
                {
                    "value":int,
                    "scriptPubKey":"string",
                    "address":"string",
                    "index":int
                },
                {
                    "value":int,
                    "scriptPubKey":"string",
                    "address":"string",
                    "index":int
                }
            ],
            "mass":int,
            "version":int,
            "raw":"string",
            "confirmations":int
        },
        {
            "transactionHash":"string",
            "transactionId":"string",
            "acceptingBlockHash":"string",
            "acceptingBlockBlueScore":int,
            "subnetworkId":"string",
            "lockTime":integer,
            "gas":integer,
            "payloadHash":"string",
            "payload":"string",
            "inputs":
            [
                {
                    "previousTransactionId":"string",
                    "previousTransactionOutputIndex":int,
                    "signatureScript":"string",
                    "sequence":"string",
                    "address":"string"
                },
                {
                    "previousTransactionId":"string",
                    "previousTransactionOutputIndex":int,
                    "signatureScript":"string",
                    "sequence":"string",
                    "address":"string"
                }
            ],
            "outputs":
            [
                {
                    "value":int,
                    "scriptPubKey":"string",
                    "address":"string",
                    "index":int
                },
                {
                    "value":int,
                    "scriptPubKey":"string",
                    "address":"string",
                    "index":int
                }
            ],
            "mass":int,
            "version":int,
            "raw":"string",
            "confirmations":int
        }
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If specifying a limit, then it must be that 1 &lt;= limit &lt;= 1000.  
If specifying skip, then it must be that skip &gt;=0.
{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":400,
    "errorMessage":"limit<1, limit>1000 or skip<0 was requested."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422,
    "errorMessage":"The given address is not a well-formatted P2PKH or P2SH address."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500,
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/transactions/block/:blockhash" %}
{% api-method-summary %}
/transactions/block/:blockhash
{% endapi-method-summary %}

{% api-method-description %}
Searches for a block by hash and returns all the transactions within the given block, sorted by internal sequence.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="blockhash" type="string" required=true %}
Hex-encoded block hash
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is a "transactions" array containing all Transaction objects within the given block.
{% endapi-method-response-example-description %}

```javascript
{
    "transactions":[
        {
            "transactionHash":"string",
            "transactionId":"string",
            "acceptingBlockHash":"string",
            "acceptingBlockBlueScore":int,
            "subnetworkId":"string",
            "lockTime":integer,
            "gas":integer,
            "payloadHash":"string",
            "payload":"string",
            "inputs":
            [
                {
                    "previousTransactionId":"string",
                    "previousTransactionOutputIndex":int,
                    "signatureScript":"string",
                    "sequence":"string",
                    "address":"string"
                },
                {
                    "previousTransactionId":"string",
                    "previousTransactionOutputIndex":int,
                    "signatureScript":"string",
                    "sequence":"string",
                    "address":"string"
                }
            ],
            "outputs":
            [
                {
                    "value":int,
                    "scriptPubKey":"string",
                    "address":"string",
                    "index":int
                },
                {
                    "value":int,
                    "scriptPubKey":"string",
                    "address":"string",
                    "index":int
                }
            ],
            "mass":int,
            "version":int,
            "raw":"string",
            "confirmations":int
        },
        {
            "transactionHash":"string",
            "transactionId":"string",
            "acceptingBlockHash":"string",
            "acceptingBlockBlueScore":int,
            "subnetworkId":"string",
            "lockTime":integer,
            "gas":integer,
            "payloadHash":"string",
            "payload":"string",
            "inputs":
            [
                {
                    "previousTransactionId":"string",
                    "previousTransactionOutputIndex":int,
                    "signatureScript":"string",
                    "sequence":"string",
                    "address":"string"
                },
                {
                    "previousTransactionId":"string",
                    "previousTransactionOutputIndex":int,
                    "signatureScript":"string",
                    "sequence":"string",
                    "address":"string"
                }
            ],
            "outputs":
            [
                {
                    "value":int,
                    "scriptPubKey":"string",
                    "address":"string",
                    "index":int
                },
                {
                    "value":int,
                    "scriptPubKey":"string",
                    "address":"string",
                    "index":int
                }
            ],
            "mass":int,
            "version":int,
            "raw":"string",
            "confirmations":int
        }
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":404,
    "errorMessage":"No Block with the given hash found."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422,
    "errorMessage":"The given hash is not a hex-encoded 32-byte hash."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500,
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/utxos/address/:address" %}
{% api-method-summary %}
/utxos/address/:address
{% endapi-method-summary %}

{% api-method-description %}
Searches for all unspent transaction outputs for the given address.  
Response: Array of TransactionOutput, omitting the address field.  
If there are no UTXOs for the given address, returns an empty array.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="address" type="string" required=true %}
A Bech32-encoded address
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is an array of unspent transactionOutput objects. In the following example the array consists of two transactionOutput objects. The address field in each transactionOutput object is omitted.
{% endapi-method-response-example-description %}

```javascript
{
    "utxos":[
        {
            "transactionId":"string",
            "index":int,
            "value":"string",
            "scriptPubKey":"string",
            "acceptingBlockHash":"string",
            "acceptingBlockBlueScore":"string",
            "isCoinbase":boolean,
            "confirmations":"string",
            "isSpendable":boolean
        },
        {
            "transactionId":"string",
            "index":int,
            "value":"string",
            "scriptPubKey":"string",
            "acceptingBlockHash":"string",
            "acceptingBlockBlueScore":"string",
            "isCoinbase":boolean,
            "confirmations":"string",
            "isSpendable":boolean
        }
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422,
    "errorMessage":"The given address is not a well-formatted P2PKH or P2SH address."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500,
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/block/:blockhash" %}
{% api-method-summary %}
/block/:blockhash
{% endapi-method-summary %}

{% api-method-description %}
Searches for a block by its hash.  
Response: Block
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="blockhash" type="string" required=true %}
Hex-encoded block hash
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is a single block object.
{% endapi-method-response-example-description %}

```javascript
{
    "blockHash":"string",
    "parentBlockHashes":["string","string"],
    "version":int,
    "hashMerkleRoot":"string",
    "acceptedIdMerkleRoot":"string",
    "utxoCommitment":"string",
    "timestamp":int,
    "bits":int,
    "nonce":int,
    "acceptingBlockHash":"string",
    "blueScore":int,
    "isChainBlock":boolean,
    "mass":int,
}                         
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":404,
    "errorMessage":"No Block with the given hash found."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422,
    "errorMessage":"The given hash is not a hex-encoded 32-byte hash."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500,
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/blocks/count" %}
{% api-method-summary %}
/blocks/count
{% endapi-method-summary %}

{% api-method-description %}
Searches for all blocks.  
Response is an integer with the total number of blocks.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/blocks\[?order=asc/desc&skip=0&limit=25\]" %}
{% api-method-summary %}
/blocks
{% endapi-method-summary %}

{% api-method-description %}
Searches for all blocks.  
Response: Array of Block
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="order" type="string" required=false %}
If provided \(asc or desc\), sorts the blocks by ascending or descending order respectively \(default: desc\). If anything else is specified, returns an error.
{% endapi-method-parameter %}

{% api-method-parameter name="skip" type="integer" required=false %}
If provided, skips the first given number of blocks ordered by ID \(default: 0\). If less than 0 is specified, returns an error.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="integer" required=false %}
If provided, limits the response to the given number of blocks \(default: 25, max 100\). If less than 1 or more than 100 is specified, returns an error.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is an array of block objects.  
Example:  
Let's assume the total number of blocks is 20, but the request was limited to 2 blocks per response by asking for:  
/blocks\[?order=asc&skip=0&limit=25\]  
The response would then be:
{% endapi-method-response-example-description %}

```javascript
{
    [
        {
            "blockHash":"string",
            "version":int,
            "hashMerkleRoot":"string",
            "acceptedIdMerkleRoot":"string",
            "utxoCommitment":"string",
            "timestamp":int,
            "bits":int,
            "nonce":int,
            "parentBlockHashes":["string","string"],
            "acceptingBlockHash":"string",
            "blueScore":int,
            "isChainBlock":boolean,
            "confirmations":int,
            "mass":int
        },
        {
            "blockHash":"string",
            "parentBlockHashes":["string","string"],
            "version":int,
            "hashMerkleRoot":"string",
            "acceptedIdMerkleRoot":"string",
            "utxoCommitment":"string",
            "timestamp":int,
            "bits":int,
            "nonce":int,
            "acceptingBlockHash":"string",
            "blueScore":int,
            "isChainBlock":boolean,
            "mass":int
        }
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
limit should be &gt;=1 and &lt;=100.  
skip cannot be less than 0.  
order can be either "asc" or "desc".
{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":400,
    "errorMessage":"limit>100, limit<1, skip<0 or order not in (asc,desc) was requested."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500,
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/fee-estimates" %}
{% api-method-summary %}
/fee-estimates
{% endapi-method-summary %}

{% api-method-description %}
Returns updated fee estimates.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is a feeEstimates object.
{% endapi-method-response-example-description %}

```javascript
{
    "highPriority":900
    "normalPriority":300
    "lowPriority":100
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.kas.pa" path="/dev/v1/transaction" %}
{% api-method-summary %}
/transaction
{% endapi-method-summary %}

{% api-method-description %}
This method takes a Hex-encoded rawTransaction data as input, and sends it to a node for execution.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="rawTransaction" type="string" required=true %}
Hex-encoded raw transaction data
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422,
    "errorMessage":"<Forwarded error from the node>"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500,
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

