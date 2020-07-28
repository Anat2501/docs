---
description: >-
  This section lists and details all of the calls the clients expect the server
  to support.
---

# Kaspad JSON-RPC

Most JSON-RPC calls are borrowed from the [Bitcoin Core developer reference](https://bitcoin.org/en/developer-reference) \(either verbatim or with slight changes\), though some have been borrowed from other sources \(e.g. `nodes`, `getheaders` which were borrowed from the btcd API\).

Calls related to the selected parent chain are listed separately in [Selected Parent Chain Related Calls](selected-parent-chain-related-api-calls.md).

Some global conventions:

* All hashes are represented in [RPC Byte order](https://bitcoin.org/en/glossary/rpc-byte-order)
* All serialized blocks are serialized as defined in [Serialized Block](./)
* All serialized transactions are serialized as defined in [Serialized Transaction](../../../../reference/transactions/#Serialized-Format)
* All of the above are **hex encoded**

## Nodes <a id="JSON-RPCAPIcalls-Nodes"></a>

### addManualNode <a id="JSON-RPCAPIcalls-addManualNode"></a>

Connects to a node. Adds the node to a [manual node list](../../reference/network/manual-node-list.md) unless the OneTry flag is set to true.

#### Parameters <a id="JSON-RPCAPIcalls-Parameters"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Required</th>
      <th style="text-align:left">Default Value</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Addr</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">N/A</td>
      <td style="text-align:left">
        <p>Node to add as a string of the form <code>&lt;address&gt;:&lt;port&gt;</code>
        </p>
        <p>The address may be:</p>
        <ul>
          <li>an IPv4 address,</li>
          <li>an <a href="https://en.wikipedia.org/wiki/IPv6#IPv4-mapped_IPv6_addresses">IPv4-as-IPV6 address</a>,
            or</li>
          <li>a resolvable hostname</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">OneTry</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">false</td>
      <td style="text-align:left">
        <p>If true:</p>
        <ul>
          <li>Immediately attempt connection, regardless of the state of the outgoing
            connection slots</li>
          <li>Only attempt once</li>
          <li>In no case this node will be added to the manual node list</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

#### Output <a id="JSON-RPCAPIcalls-Output"></a>

```text
null
```

#### Comments <a id="JSON-RPCAPIcalls-Comments"></a>

This returns null as to not block the client while waiting for response from the other node.

The design of this call is borrowed from the parallel addNode call in the Bitcoin Core API, but has been split into the current call and the [removeManualNode](./#JSON-RPCAPIcalls-removeManualNode). call \(which does not exist in Bitcoin Core\).

### getAllManualNodesInfo <a id="JSON-RPCAPIcalls-getAllManualNodesInfo"></a>

Return the information one would receive from [getManualNodeInfo](./#JSON-RPCAPIcalls-getManualNodeInfo) for all nodes in the [manual node list](../../reference/network/manual-node-list.md).

#### Parameters <a id="JSON-RPCAPIcalls-Parameters.1"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Details | boolean | N | true | If set to false, display only the `<address>:<port>` string, otherwise, print full details as described in the output |

#### Output <a id="JSON-RPCAPIcalls-Output.1"></a>

```text
[ (array of JSON objects)
    // For each node in the manual node list, the output of getManualNodeInfo for that node
]
```

### getConnectedPeerInfo <a id="JSON-RPCAPIcalls-getPeerInfo"></a>

Returns information about the node's currently connected peers as an array of JSON objects.

#### Parameters <a id="JSON-RPCAPIcalls-Parameters.3"></a>

This call takes no parameters.

#### Output <a id="JSON-RPCAPIcalls-Output.3"></a>

```text
[ (array of JSON objects) Representations of the node's peers
    {
        ID:           int32,     Index number for this remote peer in the local node address database
        Addr:         string,    IP address and port number used to connect to this peer
        Services:     string,    Services supported by the remote peer, currently set to 0x01
        RelayTxes:    bool,      Peer has requested transactions be relayed to it
        LastSend:     int64,     UNIX epoch time of when we last successfully sent TCP data to this peer
        LastRecv:     int64,     UNIX epoch time of when we last successfully received data from this peer
        BytesSent:    uint64,    Total bytes sent to this peer
        BytesRecv:    uint64,    Total bytes received from this peer
        ConnTime:     int64,     UNIX epoch time when we connected to this peer
        TimeOffset:   int64,     Time offset in seconds
        // if the peer was ever pinged:
            PingTime:     float64,      Microseconds the last ping took
        // if there is an outstanding ping to this peer:
            PingWait:     float64,      Microseconds a queued ping has been waiting for a response
        Version:      uint32,    Version of the protocol used by this peer, currently set to 1
        SubVer:       string,    User agent this peer advertises itself with
        Inbound:      bool,      True iff the remote peer initiated the connection
        SelectedTip:  string,    Selected tip of the peer
          BanScore:     int32,     Ban score assigned to the peer based on a yet-to-be-specified enforcement mechanism
          FeeFilter:    int64,     Requested minimum fee a transaction must have to be announced to the peer
          SyncNode:     bool,      Whether or not the peer is the sync peer
    }, ... 
]
```

#### Comments

The original Bitcoin Core call also included a field called AddrLocal where it would state the IP the remote node uses to connect to the current node. This was done with the forsaken notion of [Pay-to-IP](https://en.bitcoin.it/wiki/IP_transaction). We have removed this altogether.

### getManualNodeInfo <a id="JSON-RPCAPIcalls-getManualNodeInfo"></a>

Returns information about a node in the [manual node list](../../reference/network/manual-node-list.md).

#### Parameters <a id="JSON-RPCAPIcalls-Parameters.2"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Node | string | Y | N/A | The node to get information about, as a string of the form `<address>:<port>` such as in [addManualNode ](./#JSON-RPCAPIcalls-addManualNode) |
| Details | boolean | N | true | If set to false, display only the `<address>:<port>` string, otherwise, print full details as described in the output |

#### Output <a id="JSON-RPCAPIcalls-Output.2"></a>

```text
{ (JSON object)
    ManualNode:    string,    Requested node as a string of the form <address>:<port> such as in addManualNode, or empty string if the node was not found
    // If Details is true
        Connected:    boolean,    True iff the node is currently connected
        Addresses:    [ (array of JSON objects) Addresses belonging to the node
            {
                Address:    string,    IP address (resolved) and port number of the node
                Connected:  bool,      True iff we are connected to the node using this address
                // If Connected is true
                    Outbound:    bool,    True iff we established the connection
            }, ...
        ]
}
```

#### Comments

In Bitcoin Core the node field is optional; when omitted the node would return a list of all nodes. We deferred this behavior to the [getAllManualNodesInfo](./#JSON-RPCAPIcalls-getAllManualNodesInfo) call. Was originally called getAddedNodeInfo in Bitcoin Core.

### getPeerAddresses <a id="JSON-RPCAPIcalls-node(empty)"></a>

Returns the peers state.

#### Parameters

This call takes no parameters.

#### Output

```text
{
        Version:                         int,                                          Peers state serialization version
        Key:                                  [32]byte,                              Address manager's key for randomness purposes                  
        Addresses:                   [ (array of JSON objects) The node's known addresses
                                                  {
                                                          Addr:         string,        Address
                                                            Src:          string,        Address of the peer that handed the address
                                                            SubnetworkID: string,        Address subnetwork ID
                                                            Attempts:     int,            Number of attempts to connect to the address
                                                            TimeStamp:    int64,        Time the address was added
                                                            LastAttempt:  int64,        Last attempt to connect to the address
                                                            LastSuccess:  int64,        Last successful attempt to connect to the address
                                                  }, ...
                                                 ],            
        NewBuckets:                  map[string]*newbucket,      Peers state subnetwork new buckets keyed by subnetwork ID           
        NewBucketFullNodes:  [][]string,                             Peers state full nodes new bucket
        TriedBuckets:        map[string]*triedbucket, Peers state subnetwork tried buckets keyed by subnetwork ID
        TriedBucketFullNodes:[][]string,                             Peers state tried full nodes bucket
    }
```

### node <a id="JSON-RPCAPIcalls-node(empty)"></a>

Attempts to perform the passed node subcommand on the target. For example, it can be used to add or a remove a persistent peer, or to connect or disconnect a non-persistent one.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| SubCmd | string | Y | N/A | "disconnect" to remove all matching non-persistent peers, "remove" to remove a persistent peer, or "connect" to connect to a peer |
| Target | string | Y | N/A | The node to remove, as a string of the form `<address>:<port>` such as in [addManualNode](./#JSON-RPCAPIcalls-addManualNode) or as a valid peer ID |
| ConnectSubCmd | string | N | N/A | Either "perm" or "temp", if you are targeting a persistent or non-persistent peer, respectively |

#### Output

```text
null
```

### removeManualNode <a id="JSON-RPCAPIcalls-removeManualNode"></a>

Removes a node from the [manual node list](../../reference/network/manual-node-list.md).

#### Parameters <a id="JSON-RPCAPIcalls-Parameters.4"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Addr | string | Y | N/A | The node to remove, as a string of the form `<address>:<port>` such as in [addManualNode ](./#JSON-RPCAPIcalls-addManualNode) |

#### Output <a id="JSON-RPCAPIcalls-Output.4"></a>

```text
null
```

## Transactions <a id="JSON-RPCAPIcalls-Transactions"></a>

### createRawTransaction <a id="JSON-RPCAPIcalls-createrawtransaction(empty)"></a>

Create a [transaction](../../../../glossary.md#transaction) spending the given inputs and creating new outputs.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Inputs | JSON array | Y | N/A | An array of JSON objects: [transaction inputs](../../../../glossary.md#transaction-inputs) each containing a [transaction ID](../../../../glossary.md#transaction-id), number of [outputs](../../../../glossary.md#transaction-outputs), and optional sequence number |
| Outputs | JSON array | Y | N/A | An array of JSON objects: transaction outputs \(key-value pairs\), where keys are addresses and values are amounts |
| LockTime | numeric | N | 0 | Raw [locktime](../../../../reference/transactions/#Locktime) |

#### Output

Hex-encoded bytes of the serialized transaction.

### decodeRawTransaction <a id="JSON-RPCAPIcalls-decoderawtransaction(empty)"></a>

Returns a JSON object representing the provided serialized, hex-encoded transaction.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| HexTx | string | Y | N/A | Serialized, hex-encoded transaction |

#### Output

```text
{
  TxID:     string,    Transaction ID
    Version:  int32,     Transaction version
    LockTime: uint64,    Transaction locktime
    Vin:      [ (array of JSON objects) Transaction inputs
                {
                TxID:      string,     Transaction ID
                Vout:      uint32,     Output number
                ScriptSig: { (JSON object) Unlocking script
                             ...
                           },
                Sequence:  uint64,     Script sequence number
              }, ...
              ],
    Vout:     [ (array of JSON objects) Transaction outputs
                  {
                Value:        uint64,     Value in Kaspa
                N:            uint32,     Index
                ScriptPubKey: { (JSON object) Locking script
                                ...
                              },
              }, ...
              ]
}
```

### decodeScript <a id="JSON-RPCAPIcalls-decodescript(empty)"></a>

Returns a JSON object with information about the provided hex-encoded script.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| HexScript | string | Y | N/A | The hex-encoded script |

#### Output

```text
{ (JSON object)
  Asm:     string,   Disassembly of the script
  Type:    string,   Type of the script (e.g. 'pubkeyhash')
  Address: string,   Kaspa address (if any) associated with this script
  P2sh:    string,   Address of the P2SH script wrapping this redeem script (not returned if the script is already P2SH).
}
```

### getTxOut <a id="JSON-RPCAPIcalls-getTxOut"></a>

Fetches details about a [UTXO](../../../../glossary.md#utxo).

#### Parameters <a id="JSON-RPCAPIcalls-Parameters.5"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| TxID | string | Y | N/A | The ID of the transaction containing the desired output |
| OutputIndex | int | Y | N/A | The index of the output within the transaction, the index of the first output being 0 |
| Unconfirmed | boolean | N | false | If set to true, unconfirmed outputs \(from mempool\) will also be displayed |

#### Output <a id="JSON-RPCAPIcalls-Output.5"></a>

```text
{   (JSON object)
    SelectedTip:   string,    Hex-encoded hash of the block that contains the transaction output
      Confirmations: uint64,    Number of confirmations received for this transaction
      IsInMempool:   bool,      Whether the transaction is in the mempool
      Value:         float64,   Amount spent to this output in KAS
      ScriptPubKey:  { (JSON object) Unlocking script
                        ...
                     },
      Coinbase:      bool       Whether or not the transaction is a coinbase
}
```

### loadTxFilter <a id="JSON-RPCAPIcalls-ping(empty)"></a>

Loads, reloads, or adds data to a websocket client's transaction filter. The filter is consistently updated based on inspected transactions during mempool acceptance, block acceptance, and for all rescanned blocks.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Reload | boolean | Y | N/A | Load a new filter instead of adding data to an existing one |
| Addresses | string array | Y | N/A | List of Kaspa addresses to add to the transaction filter |
| Outpoints | JSON array | Y | N/A | List of [transaction outpoints](../../../../reference/txo/outpoint.md) to add to the transaction filter as follows: \[{TxID:"value", Index: n}, ...\] |

#### Output

```text
null
```

### notifyNewTransactions <a id="JSON-RPCAPIcalls-sendRawTransaction"></a>

Registers the client to receive notifications every time a new transaction is accepted to the memory pool. The notifications are delivered to the notification handlers associated with the client. Calling this function has no effect if there are no notification handlers and will result in an error if the client is configured to run in HTTP POST mode.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Verbose | boolean | N | false | Specifies which type of notification to receive. If verbose is true, then the caller receives txacceptedverbose, otherwise the caller receives txaccepted |
| Subnetwork | string | N | N/A | If Verbose=true, [subnetwork](../../../../glossary.md#subnetwork) needs to be identified and must equal node's subnetwork if node is partial |

#### Output

```text
null
```

### sendRawTransaction <a id="JSON-RPCAPIcalls-sendRawTransaction"></a>

Validates a transaction and broadcasts it to the P2P network.

#### Parameters <a id="JSON-RPCAPIcalls-Parameters.7"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| HexTx | string | Y | N/A | The hex-encoded serialized transaction to broadcast |

#### Output <a id="JSON-RPCAPIcalls-ReturnValue.1"></a>

The hash of the transaction.

#### Comments

The Bitcoin Core version of this call has a second parameter allowing for "insanely high fees" \(whatever that means\). As of now, this parameter doesn't actually do anything, and since it is also wallet-oriented we decided to remove it altogether.

### stopNotifyNewTransactions

Unregisters new mempool transaction updates. This call takes no parameters and outputs `null`.

### validateAddress <a id="JSON-RPCAPIcalls-validateaddress(empty)"></a>

Return information about the given Kaspa address.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Address | string | Y | N/A | The Kaspa address to validate |

#### Output

```text
{
  IsValid: bool,      If the address is valid or not. If not, this is the only property returned.
  Address: string     The Kaspa address (only when IsValid is true)
}
```

## Blocks and DAG <a id="JSON-RPCAPIcalls-BlocksandDAG"></a>

### getBlock <a id="JSON-RPCAPIcalls-getBlock"></a>

Given a block hash, return the block info, either as the hex-encoded serialized raw block, or if Verbose is true, as a JSON object as specified in the output.

#### Parameters <a id="JSON-RPCAPIcalls-Parameters.8"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Hash | string | Y | N/A | The hash of the header of the desired block |
| Verbose | boolean | N | true | Specifies the block is returned as a JSON object instead of hex-encoded string |
| VerboseTx | boolean | N | false | Specifies that each transaction is returned as a JSON object and only applies if the Verbose flag is true |
| Subnetwork | string | N | N/A | If passed, the returned block will be a partial block of the specified subnetwork |

#### Output <a id="JSON-RPCAPIcalls-Output.6"></a>

```text
{ (JSON object)
    Hash:                 string        Hash of the block (same as provided)
    Confirmations:        uint64        Number of confirmations for this block
    Size:                 int32         Size of the block
    BlueScore:            uint64        Block blue score
    IsChainBlock:         bool          Whether the block is in the selected parent chain
    Version:              int32         Block version
    VersionHex:           string        Block version in hexadecimal
    HashMerkleRoot:       string        Merkle tree reference to the hashes of all transactions in the block
    AcceptedIDMerkleRoot: string        Merkle tree reference to the hashes of all transactions accepted from the block's merged blocks
    UTXOCommitment:       string        ECMH UTXO commitment of this block
    Tx:                   []string      Transaction hashes (only when VerboseTx=false)
    RawTx:                []TxRawResult Transactions as JSON objects (only when VerboseTx=true)
    Time:                 int64         Block time in seconds since 1 Jan 1970 GMT
    Nonce:                uint64        Block nonce
    Bits:                 string        Bits which represent the block difficulty
    Difficulty:           float64       Proof-of-work difficulty as a multiple of the minimum difficulty
    ParentHashes:         []string      Hashes of the parent blocks
    SelectedParentHash:   string        Selected parent hash
    ChildHashes:          []string      Hashes of the child blocks (if there are any)
    AcceptedBlockHashes:  []string      Hashes of the blocks accepted by this block
}
```

#### Comments <a id="JSON-RPCAPIcalls-Comments.6"></a>

The transactions in "Tx" must appear **in the same order** as in the serialized block.

### getBlockCount <a id="JSON-RPCAPIcalls-getblockcount(empty)"></a>

Returns the number of blocks in the [blockDAG](../../../../glossary.md#blockdag).

#### Parameters

This call takes no parameters.

### getBlockDAGInfo <a id="JSON-RPCAPIcalls-getblockchaininfo(empty)"></a>

Returns an object containing various state info regarding [blockDAG](../../../../glossary.md#blockdag) processing.

#### Parameters

This call takes no parameters.

#### Output

```text
{ (JSON object)
    DAG:                  string                              Name of the DAG the daemon is on (testnet, mainnet, etc)
    Blocks:               uint64                              Number of blocks in the DAG
    Headers:              uint64                              Number of headers we have validated
    TipHashes:            []string                            Block hashes for the tips in the DAG
    Difficulty:           float64                             Current difficulty
    MedianTime:           int64                               Median time from the PoV of the selected tip in the DAG
    VerificationProgress: float64                             Estimate for how much of the DAG we've verified
    Pruned:               bool                                Indicates if the node is pruned or not
    PruneHeight:          uint64                              Lowest block retained in the current pruned DAG
    DAGWork:              string                              Total cumulative work in the DAG
    SoftForks:            []*SoftForkDescription              Status of the super-majority soft-forks
    Bip9SoftForks:        map[string]*Bip9SoftForkDescription Map describing active BIP9 deployments, keyed by BIP9 soft fork
}
```

### getBlockHeader <a id="JSON-RPCAPIcalls-getBlockHeader(empty)"></a>

Translate a block hash to a [block header](../../../../glossary.md#block-header), either as the hex-encoded serialized header, or if Verbose is true, as a JSON object as specified in the output.

#### Parameters <a id="JSON-RPCAPIcalls-Parameters.9"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Hash | string | Y | N/A | The hash of the header of the desired block |
| Verbose | boolean | N | true | Specifies that the block header should be returned as a JSON object instead of hex-encoded string |

#### Output <a id="JSON-RPCAPIcalls-Output.7"></a>

```text
{ (JSON object)
  Hash:                 string             Hash of the block (same as provided)
    Confirmations:        uint64             Number of confirmations
    Version:              int32         Block version
    VersionHex:           string        Block version in hexadecimal
    HashMerkleRoot:       string        Merkle tree reference to the hashes of all transactions in the block
    AcceptedIDMerkleRoot: string        Merkle tree reference to the hashes of all transactions accepted from the block's blues
    Time:                 int64         Block time in seconds since 1 Jan 1970 GMT
    Nonce:                uint64        Block nonce
    Bits:                 string        Bits which represent the block difficulty
    Difficulty:           float64       Proof-of-work difficulty as a multiple of the minimum difficulty
    ParentHashes:         []string      Hashes of the parent blocks
    SelectedParentHash:   string        Selected parent hash
    ChildHashes:          []string      Hashes of the child blocks (if there are any)
}
```

### getBlockTemplate <a id="JSON-RPCAPIcalls-getblocktemplate(empty)"></a>

Returns a JSON object with information necessary to construct a block to mine or accepts a proposal to validate.

If the request parameters include a ‘mode’ key, that is used to explicitly select between the default ‘template’ request or a ‘proposal’.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Template\_request | JSON object | Y | N/A | Request object which controls the mode and several parameters, in the spec of the following table: |

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Mode | string | N | N/A | This must be set to "template", "proposal", or omitted |
| Capabilities | list of strings | N | N/A | Client-side supported features |
| PayAddress | string | N | N/A | The address the coinbase pays to |
| LongPollID | string | N | N/A | The long poll ID of a job to monitor for expiration; required and valid only for long poll requests |
| SigOpLimit | numeric | N | N/A | Number of signature operations allowed in blocks \(this parameter is ignored\) |
| MassLimit | numeric | N | N/A | Max transaction mass allowed in blocks \(this parameter is ignored\) |
| MaxVersion | numeric | N | N/A | Highest supported block version number \(this parameter is ignored\) |
| Target | string | N | N/A | The desired target for the block template \(this parameter is ignored\) |
| Data | string | N | N/A | If mode is "proposal", data contains the hex-encoded serialized block that is being proposed |
| WorkID | string | N | N/A | The server provided work ID if provided in block template \(not applicable\) |

#### Output

```text
{ (JSON object)
        Bits:                 string,                        Hex-encoded compressed difficulty
        CurTime:              int64,                        Current timestamp in seconds since epoch (Jan 1 1970 GMT)
        Height:               uint64,                        Height of the next block
        ParentHashes:         []string,                    List of the block header hashes of the next block's parents
        MassLimit:            int64,                         Maximum total transaction mass a block may have.
        Transactions:         []transactions,        Contents of non-coinbase transactions that should be included in the next block
        AcceptedIDMerkleRoot: string,                     Accepted ID merkle root of the next block
        UTXOCommitment:       string,                     UTXO commitment of the next block
        Version:              int32,                        Preferred block version
        LongPollID:           string,                        Identifier for long poll request which allows monitoring for expiration
        LongPollURI:                  string,                        Alternate URI to use for long poll requests if provided (not provided)
        Target:               string,                        Hash target
        MinTime:              int64,                        Minimum timestamp appropriate for next block time in seconds since epoch (Jan 1 1970 GMT)
        MaxTime:              int64,                        Maximum allowed timestamp for a block
        Mutable:              []string,                    List of ways the block template may be changed
        NonceRange:           string,                        Range of valid nonces
        Capabilities:         []string,                    List of additional client-side supported capabilities    
        RejectReasion:                 string,                        Reason the proposal was invalid as-is (only applies to proposal responses)
       WorkID:               string,           This value must be returned with result if provided (not provided)
        IsSynced:             bool                            Whether this node is synced with the rest of of the network
    }
```

### getBlocks <a id="JSON-RPCAPIcalls-getheaders(empty)"></a>

Return the blocks starting from lowHash up to the [virtual](../../../../glossary.md#virtual-block) ordered by [blue score](../../../../glossary.md#blue-score).

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| LowHash | string | N | N/A | Hash of the block with the bottom blue score. If this hash is unknown, returns an error |
| IncludeRawBlockData | boolean | Y | N/A | If set to true, the raw block data would be also included |
| IncludeVerboseBlockData | boolean | Y | N/A | If set to true, the verbose block data would also be included |

#### Output

Returns blocks starting from lowHash. The result may contains up to 1000 blocks. For the remainder, call the command again with the bluest block's hash.

```text
{ (JSON object)
        Hashes:        []string,                  List of hashes from lowHash (excluding lowHash) ordered by smallest blue score to greatest
        RawBlocks:     []string,                  If includeRawBlockData=true, contains the block contents. Otherwise omitted
        VerboseBlocks: []verboseblock,        If includeRawBlockData=true and includeVerboseBlockData=true, each block is returned as a JSON object. Otherwise, hex-encoded string
}
```

### getHeaders <a id="JSON-RPCAPIcalls-getheaders(empty)"></a>

Returns block headers starting with the first known block hash from the request.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| LowHash | string | Y | N/A | Block hash to start including headers from; if not found, it'll start from the genesis block |
| HighHash | string | Y | N/A | Block hash to stop including block headers for; if not found, all headers to the latest known block are returned |

#### Output

A list of serialized block headers of all located blocks, limited to some arbitrary maximum number of hashes \(currently 2000, which matches the wire protocol headers message, but this is not guaranteed\).

### getInfo <a id="JSON-RPCAPIcalls-getinfo(empty)"></a>

Returns a JSON object containing various state info.

#### Parameters

This call takes no parameters.

#### Output

```text
{ (JSON object)
    Version:         string  Version of the server
    ProtocolVersion: int32   Latest supported protocol version
    Blocks:          uint64  Number of blocks processed
    Connections:     int32   Number of connected peers
    Proxy:           string  Proxy used by the server
    Difficulty:      float64 Current target difficulty
    Testnet:         bool    Whether or not server is using testnet
    Devnet:          bool    Whether or not server is using devnet
    RelayFee:        float64 Minimum relay fee for non-free transactions in KAS/KB
    Errors:          string  Any current errors
}
```

#### Comments

This call is deprecated in Bitcoin Core, retained in btcd minus fields related to wallet functionality.

### getTopHeaders <a id="JSON-RPCAPIcalls-invalidateblock(empty)"></a>

Returns the top block headers starting with the provided high hash \(not inclusive\).

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| HighHash | string | N | N/A | Block hash to start including block headers from; if not found, it'll start from the virtual |

#### Output

A list of the serialized block headers of all located blocks, limited to some arbitrary maximum number of hashes \(currently 2000, which matches the wire protocol headers message, but this is not guaranteed\).

### notifyBlocks <a id="JSON-RPCAPIcalls-submitblock(empty)"></a>

Registers the client to receive notifications when blocks are connected and disconnected from the main chain. The notifications are delivered to the notification handlers associated with the client. Calling this function has no effect if there are no notification handlers and will result in an error if the client is configured to run in HTTP POST mode.

This call takes no parameters and outputs `null`.

### rescanBlockFilter <a id="JSON-RPCAPIcalls-submitblock(empty)"></a>

Rescans a block for any relevant transactions for the passed lookup keys. Any discovered transactions are returned hex-encoded as a string slice.

#### Parameters

This call takes no parameters.

#### Output

A string of hex-encoded transactions.

### rescanBlocks <a id="JSON-RPCAPIcalls-submitblock(empty)"></a>

Rescans the blocks identified by BlockHashes, in order, using the client's loaded transaction filter.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| BlockHashes | string array | Y | N/A | List of hashes to rescan. Each next block must be a child of the previous |

#### Output

```text
[ (JSON array) List of blocks that contain relevant transactions
 {
    Hash:         string,         Block hash
    Transactions: []transactions  Transactions in the block
 },
 ...
]
```

### stopNotifyBlocks <a id="JSON-RPCAPIcalls-submitblock(empty)"></a>

Unregisters block updates. This call takes no parameters and returns `null`.

### submitBlock <a id="JSON-RPCAPIcalls-submitblock(empty)"></a>

Attempts to submit a new block to network.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| HexBlock | string | Y | N/A | The hex-encoded block to submit |

#### Output

```text
null
```

## Mining <a id="JSON-RPCAPIcalls-Mining"></a>

### getDifficulty <a id="JSON-RPCAPIcalls-getdifficulty(empty)"></a>

Returns the proof-of-work difficulty as a multiple of the [minimum difficulty](../../../../glossary.md#bits-target).

#### Parameters

This call takes no parameters.

#### Output

The proof-of-work difficulty as a multiple of the minimum difficulty.

### getMempoolEntry <a id="JSON-RPCAPIcalls-getmempoolinfo(empty)"></a>

Returns mempool data for the given transaction.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| TxID | string | Y | N/A | The [transaction ID](../../../../glossary.md#transaction-id) \(must be in mempool\) |

#### Output

```text
{ (JSON object)
    Fee:    uint64,    Transaction fee in KAS
    Time:   int64,     Local time transaction entered pool in seconds since 1 Jan 1970 GMT
    RawTx:  string,    Raw transaction
}
```

### getMempoolInfo <a id="JSON-RPCAPIcalls-getmempoolinfo(empty)"></a>

Returns details on the active state of the transaction memory pool.

#### Parameters

This call takes no parameters.

#### Output

```text
{ (JSON object)
  Size:  int64,    Current tx count
  Bytes: int64,    Sum of all serialized transaction sizes
}
```

### getRawMempool <a id="JSON-RPCAPIcalls-getrawmempool(empty)"></a>

Returns information about all of the transactions currently in the memory pool.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Verbose | boolean | N | false | Returns JSON object when true or an array of transaction hashes when false |

#### Output

If Verbose = false:

```text
[ (string array)
  TransactionID: string,     Transaction ID
  ...
]
```

If Verbose = true:

```text
{ (JSON object)
  TransactionID: { (JSON object)
    Size: int32,   Virtual transaction size in bytes
    Fee: float64,  Transaction fee in KAS
    Time: int64,   Local time transaction entered pool in seconds since 1 Jan 1970 GMT
    Depends:       [ (string array) Unconfirmed transactions used as inputs for this transaction
                     TransactionID: string,    Parent transaction id
                     ...
                   ]
  }, ...
}
```

## Network <a id="JSON-RPCAPIcalls-Network"></a>

### getConnectionCount <a id="JSON-RPCAPIcalls-getconnectioncount(empty)"></a>

Returns the number of connections to other nodes.

#### Parameters

This call takes no parameters.

#### Output

The number of connections to other nodes.

### getCurrentNet <a id="JSON-RPCAPIcalls-getcurrentnet(empty)"></a>

Gets the Kaspa network the server is running on.

#### Parameters

This call takes no parameters.

#### Output

The [magic bytes](../../reference/network/#network-messages) used to identify the network.

### getNetTotals <a id="JSON-RPCAPIcalls-getnettotals(empty)"></a>

Returns information about network traffic, including bytes in, bytes out, and current time.

#### Parameters

This call takes no parameters.

#### Output

```text
{
  TotalBytesRecv:   uint64,   Total bytes received
  TotalBytesSent:   uint64,   Total bytes sent
  TimeMillis:       int64,    Current UNIX time in milliseconds
}
```

### getSubnetwork <a id="JSON-RPCAPIcalls-ping(empty)"></a>

Returns the [gas limit](../../../../glossary.md#gas) of the specified [subnetwork](../../../../glossary.md#subnetwork).

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| SubnetworkID | string | Y | N/A | The subnetwork ID |

#### Output

The gas limit \(uint64\).

### ping <a id="JSON-RPCAPIcalls-ping(empty)"></a>

Requests that a ping be sent to all other nodes, to measure ping time. This call takes no parameters and outputs `null`.

### stop <a id="JSON-RPCAPIcalls-stop(empty)"></a>

Stops Kaspa server. This call takes no parameters and outputs "kaspad stopping."

## Server <a id="JSON-RPCAPIcalls-Server"></a>

### debugLevel <a id="JSON-RPCAPIcalls-debuglevel(empty)"></a>

Dynamically sets the debug logging level to the passed level specification.

The LevelSpec can be either a debug level or of the form:

```text
<subsystem>=<level>,<subsystem2>=<level2>,...
```

The valid debug levels are trace, debug, info, warn, error, and critical. The valid subsystems are AMGR, ADXR, KSDB, BMGR, KASD, BDAG, DISC, PEER, RPCS, SCRP, SRVR, and TXMP. Finally the keyword "show" will return a list of the available subsystems.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| LevelSpec | string | Y | N/A | The debug level\(s\) to use or the keyword "show" |

#### Output

"Done." Or, if LevelSpec="show", a list of available subsystems.

### help <a id="JSON-RPCAPIcalls-help(empty)"></a>

List all commands, or get help for a specified command.

#### Parameters

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Command | string | N | all commands | The command to get help on |

#### Output

The help text.

### session

Returns details regarding a websocket client's current connection.

#### Parameters

This call takes no parameters.

#### Output

The unique session ID for a client's websocket connection.

### uptime <a id="JSON-RPCAPIcalls-uptime(empty)"></a>

Returns the total uptime of the server.

#### Parameters

This call takes no parameters.

#### Output

The number of seconds that the server has been running.

### version <a id="JSON-RPCAPIcalls-version(empty)"></a>

Returns information about the server's JSON-RPC API versions.

#### Parameters

This call takes no parameters.

#### Output

```text
{ (JSON object) Version objects keyed by the program or API name
        KaspadJSONRPCAPI: {
            VersionString: string,        JSON-RPC API version
            Major:         uint32         Major component of the JSON-RPC API version
            Minor:         uint32         Minor component of the JSON-RPC API version
            Patch:         uint32         Patch component of the JSON-RPC API version
            Prerelease:    string         Prerelease info about the current build
            BuildMetadata: string         Metadata about the current build
        }, ...
}
```

## Irrelevant to Non-Wallet <a id="JSON-RPCAPIcalls-Irrelevanttonon-wallet"></a>

The original Bitcoin Core full node doubled as a wallet manager. This makes little sense, as it is rarely true that you want to keep your node on the same machine where you want to keep your wallet.

We decided to remove all wallet related RPC calls, which we list for completeness.

The calls are:

`addmultisigaddress, addnode, createmultisig, dumpprivkey, encryptwallet, getaccount, getaccountaddress, getaddressesbyaccount, getbalance,`

`getnewaddress, getrawchangeaddress, getreceivedbyaccount, getreceivedbyaddress, gettxoutsetinfo, importprivkey, keypoolrefill, listaccounts,`

`listaddressgroupings, listlockunspent, listreceivedbyaccount, listreceivedbyaddress, listsinceblock, listtransactions, listunspent, lockunspent,`

`move, sendfrom, sendmany, sendtoaddress, setaccount, settxfee, signmessage, signrawtransaction, walletlock, walletpassphrase, walletpassphrasechange.`

## Template for future API calls

{% hint style="warning" %}
This API call has not yet been approved
{% endhint %}

#### Parameters <a id="JSON-RPCAPIcalls-Parameters.13"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
|  |  |  |  |  |

#### Output <a id="JSON-RPCAPIcalls-Output.11"></a>

| `...` |
| :--- |


