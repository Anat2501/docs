# Network

Like Bitcoin, Kaspa uses a broadcast network to propagate [transactions](../../../../glossary.md#transaction) and [blocks](../../../../glossary.md#block). All network communications are done over [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol).

## Peer Discovery

The first time a [node](../../../../glossary.md#full-node) is started, it only has the hardcoded [genesis block](../../../../glossary.md#genesis-block). The node needs to connect to the Kaspa network in order to sync with other peers.

To know where to connect, a Kaspa node will typically have a list of trusted DNS servers, called “DNS seeders”, known to be active and highly available at the time the code of the node was released. Connecting to a DNS seeder allows a Kaspa node to acquire an initial set of other active nodes to connect to. After connecting to some other nodes, a Kaspa node should maintain an address book of peers that will allow it to stay well connected. If the node plans to mine, acceptance of its mined blocks depends on the promptness with which they are propagated throughout the network.

### DNS Seeder

DNS seeders are standalone DNS servers run by volunteers, serving a list of highly available, active nodes on the Kaspa network that are likely to accept new connection requests.

Kaspa nodes use DNS seeders to identify an initial set of other nodes to connect to when starting up for the first time - or when none of the peers they have are online or accepting new connections, following a reboot or in any case when all connections were dropped.

### Address Discovery

Address discovery is the process of exchanging addresses between Kaspa nodes on the peer-to-peer network. Whenever two nodes connect, they exchange address books with each other. This allows them to better situate themselves in the network, and allows the network as a whole to better propagate data.

### Bucketing

Bucketing is a method of saving IP addresses to reduce the chance of an [eclipse attack](https://www.usenix.org/conference/usenixsecurity15/technical-sessions/presentation/heilman).

Each node has 2 tables for IP addresses:

* New - IP addresses the node has heard about, but has not yet connected to
* Tried - IP addresses the node has previously connected to

Each of these tables has buckets:

* The New table has 1024 buckets; each may contain up to 64 IP addresses
* The Tried table has 64 buckets; each may contain up to 256 IP addresses

When an IP address is added to a table, it is first broken into 2 parts, i.e. `198.51.100.0` is broken into two parts: `198.51` and `100.0`. The first part is hashed to choose 8 out of the Tried buckets or 64 out of the New buckets, and the second part chooses one of those chosen buckets to store the IP address in. If the chosen bucket is full, the node chooses a random IP addresses from the bucket and discards it if it is offline.

When the node wants to connect to a new peer, a randomly selected IP address from the New table is chosen.

## Connecting to Peers

### Address Manager

The address manager is responsible for storing all peer addresses known to the [kaspad node](../../../../glossary.md#kaspad), providing an address when the node wants a new outgoing connection and providing all addresses during address exchange. Read more on [GoDoc](https://godoc.org/github.com/kaspanet/kaspad/addrmgr).

### Connection Manager

The connection manager responsible for managing connections. This includes creating new connections, handling disconnections, handling the dynamic ban score, and more. Read more on [GoDoc](https://godoc.org/github.com/kaspanet/kaspad/connmgr).

### Ban Scores

Nodes may be banned from the network if they attempt a flooding attack; a single message every N seconds will not do any harm, but a continuous barraging of messages will result in a ban.

Banning policy is not a direct "you do this --&gt; you are banned" rule. Instead, it's a more flexible "you do this --&gt; you get X points"; when you reach some threshold, you are banned. Ban scores decay with time. 

\[Qs: Is the entire score decaying, or part of the score is persistent and some transient? Are the scores configurable, allowing nodes to be more lax or strict as their operator wishes?\]

### Outgoing Connections

Kaspad maintains 8 outgoing connections by default. This is configurable by the user.

### Incoming Connections

Kaspad limits incoming connections to 117 by default. This is configurable by user.

## Synchronizing Between Peers

### Sync Manager

Sync Manager is the software component within a [node](../../../../glossary.md#kaspad) that manages [Netsync](./#netsync).

### Sync Peer

From the perspective of a syncing node, the sync peer is the connected peer from which the [Initial Netsync](./#initial-netsync-ibd) is done.

### Netsync

Netsync is the process of acquiring [block](../../../../glossary.md#block) data from other nodes.

### Handshake

Handshake is a process that happens when two nodes connect to each other. They exchange [version messages](./#version) with an intent to understand each other’s state. If they do not have the same group of [blockDAG tips](../../../../glossary.md#tips), they initiate [Initial Netsync](./#initial-netsync-ibd).

### Initial Netsync \(IBD\)

Called Initial Block Download \(IBD\) in Bitcoin, Initial Netsync is the process of acquiring block data from connected peers upon a new connection, where blocks that are synced are ones that were mined before the connection.

### Continuous Netsync

Continuous Netsync is the process of acquiring block data from connected peers as the blocks are being mined. Continuous Netsync is suspended during [Initial Netsync](./#initial-netsync-ibd).

### Current \(Synced\) Node

“Current” is a self-defined state of a mining node, when the [selected tip](../../../../glossary.md#selected-tip-of-the-blockdag) it sees is not older than 12 hours.

Non-current nodes should not mine, because the cost of mining will be wasteful compared to the reward.

### Current Non-Syncing Node

A current non-syncing node is a [current node](./#current-synced-node) that also does not have any [sync peers](./#sync-peer).

Current non-syncing nodes should not propagate blocks to their peers.

### Net Split

A net split is a lack of connectivity between two or more groups of connected nodes.

A net split can cause a [DAG split](./#dag-split) if it extends longer than the [finality window](../../../../glossary.md#finality).

### DAG Split

A DAG split is a divergence in the [blockDAG](../../../../glossary.md#blockdag) as a result of a lack of agreement on one or more blocks between two or more groups of nodes.

## Synchronous Network

A synchronous network is a model in distributed system networks wherein a specific upper bound on the communication delay diameter of nodes is assumed inside the protocol. This is in contrast to a partially synchronous network, where the existence of such a bound is assumed, but not assigned a specific value.

\[Q: what's the context of this? Is Kaspa synchronous or partially synchronous?\]

## Network Messages

### Format

Network messages between nodes follow a strict format. Each network message includes a header with a constant length and an optional binary payload:

* Header
  * Network Magic
  * Command name \(message type\)
  * Length \(of the binary payload\)
  * Checksum \(of the binary payload\)
* Binary Payload

### Message Header

| Bytes | Field Name | Data Type | Description |
| :--- | :--- | :--- | :--- |
| 4 | network magic | char | A string to help identify the network and where one message ends and another starts. |
| 12 | message name | char | An ASCII string identifying the message type. Message names shorter than the [constant 12 bytes](https://github.com/kaspanet/kaspad/blob/5374d95416c1f036e5453c7d250427bb54fbee7b/wire/message.go#L24) are padded by 0x00’s, e.g. version\0\0\0\0\0. |
| 4 | payload size | int32 | The number of bytes in the message payload. Messages with payload size surpassing the max size of [32 MiB](https://github.com/kaspanet/kaspad/blob/5374d95416c1f036e5453c7d250427bb54fbee7b/wire/message.go#L28) \(same as Bitcoin\) are dropped or rejected. |
| 4 | payload checksum | char | First 4 bytes of [hash](../../../../reference/serialized-data-formats/)\(message payload\) in internal byte order. If payload is empty, as in [verack](./#verack) and [getaddr](./#getaddr), the checksum is 0x5df6e0e2 \(hash of empty string\). |

### Message Payload

The message payload is binary data that accompanies some network messages. The size of the payload is specified in the message header in the field payload size and can range from Zero \(no payload\) to 32MiB.

### Network Magic

Network Magic is a 4 byte string that every message starts with.

It serves two purposes:

1. It helps identify where messages start in a dump of network messages.
2. It allows network peers to ensure their connected peers are on the same network as them.

Network Magic per Kaspa Network:

| Kaspa Network | Network Description | Network Magic |
| :--- | :--- | :--- |
| MainNet | The main Kaspa network | 0x3ddcf71d |
| TestNet | The test network | 0xddb8af8f |
| RegTest | The regression test network | 0xf396cdd6 |
| SimNet | The simulation test network | 0x374dcf1c |
| DevNet | The development test network | 0x732d87e1 |

## Messages

### version

The version message is used by a peer to advertise itself as soon as an outbound connection is made. The remote peer then uses this information to negotiate parameters. The remote peer must respond with a version message of its own containing the negotiated values followed by a [verack](./#verack) message. This exchange must take place before any further communication is done.

The payload of a version message includes the following fields:

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| Protocol Version | int32 | 4 | The version of the protocol the node is using. |
| Services | uint64 | 8 | 64 bits representing 64 supported service flags \(see below\). |
| Timestamp | int64 | 8 | The time the message was generated, used in Bitcoin to sync clocks between nodes. Kaspa nodes do not use this to sync clocks. |
| AddrYou | uint32 | 4 | NetAddress object containing the address of the remote peer. |
| AddrMe | uint32 | 4 | NetAddress object containing the address of the local peer. |
| Nonce | uint64 | 8 | Unique value associated with message that is used to detect self connections. |
| UserAgent | [varString](../../../../reference/serialized-data-formats/variable-length-fields/varstring.md) | variable | The user agent that generated message. It is encoded as a varString on the wire. It has a max length of MaxUserAgentLen. |
| SelectedTipHash | [Hash](../../../../reference/serialized-data-formats/hash.md) | 4 | The hash of the selected tip of the generator of the version message. |
| DisableRelayTx | Bool | 1 | Don't announce transactions to peer. |
| SubnetworkID | string | 20 | The [subnetwork](../subnetworks-1.md) of the generator of the version message. Should be nil in full nodes. Otherwise it is ripmed160\(sha256\(data\)\) |

#### Service Flags

Service flags identify services supported by a Kaspa node.

| Flag Name | Description |
| :--- | :--- |
| SFNodeNetwork | A flag used to indicate a peer is a full node. |
| SFNodeGetUTXO | A flag used to indicate a peer supports the getutxos and utxos commands \(BIP0064\). |
| SFNodeBloom | A flag used to indicate a peer supports bloom filtering. |
| SFNodeXthin | A flag used to indicate a peer supports xthin blocks. |
| SFNodeBit5 | A flag used to indicate a peer supports a service defined by bit 5. |
| SFNodeCF | A flag used to indicate a peer supports committed filters \(CFs\). |

### verack

The verack message is used by a peer to acknowledge a [version](./#version) message after it has used the information to negotiate parameters.

This message has no payload.

### getaddr

The getaddr message is used to request a list of known active peers from a peer on the network, to help identify potential nodes. The list is returned via one or more addr messages.

This message has no payload.

### addr

The addr message is used to provide a list of known active peers on the network. An active peer is considered one that has transmitted a message within the last 3 hours. Nodes which have not transmitted in that time frame should be forgotten. Each message is limited to a maximum number of addresses, which is currently 1000. As a result, multiple messages must be used to relay the full list. The AddAddress function is used to build up the list of known addresses when sending an addr message to another peer.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| IncludeAllSubnetworks | bool | 1 | The version of the protocol the node is using. |
| SubnetworkID | string | 20 | The [subnetwork](../subnetworks-1.md) of the generator of the version message. Should be nil in full nodes. Otherwise it is ripmed160\(sha256\(data\)\) |
| AddrList | \[\]uint32 | variable | An array of NetAddress objects, containing the addresses of known peers. |

### getblockinvs

The getblockinvs message is used to request a list of blocks starting after the given low hash \(not including it\) and until the given high hash \(including it\).

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| LowHash | [Hash](../../../../reference/serialized-data-formats/hash.md) | 4 | Start with blocks with hash higher than this one |
| HighHash | [Hash](../../../../reference/serialized-data-formats/hash.md) | 4 | End with the block with this hash |

### inv

The inv message is used to advertise a peer's known data such as [blocks](../../../../glossary.md#block) and [transactions](../../../../glossary.md#transaction) through inventory vectors. It may be sent unsolicited to inform other peers of the data, or in response to a [getblockinvs](./#getblockinvs) message. Each message is limited to a maximum number of inventory vectors, which is currently 50,000. The AddInvVect function is used to build up the list of inventory vectors when sending an inv message to another peer.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| InvList | \[\]InvVect | variable | An array of [InvVect](./#invvect) objects, referencing the available data. |

#### InvVect

InvVect is a structure made of:

* [InvType](./#invtypes) \(uint32\)
* a [hash](../../../../reference/serialized-data-formats/hash.md) of data.

#### InvTypes

```text
InvType = 0    "ERROR"                 InvTypeError
InvType = 1    "MSG_TX"                InvTypeTx
InvType = 2    "MSG_BLOCK"             InvTypeBlock
InvType = 3    "MSG_FILTERED_BLOCK"    InvTypeFilteredBlock
InvType = 4    "MSG_SYNC_BLOCK"        InvTypeSyncBlock
```

### getdata

The getdata message is used to request data such as [blocks](../../../../glossary.md#block) and [transactions](../../../../glossary.md#transaction) from another peer. It should be used in response to the [inv](./#inv) message to request the actual data referenced by each inventory vector the receiving peer doesn't already have. Each message is limited to a maximum number of inventory vectors, which is currently 50,000. As a result, multiple messages must be used to request larger amounts of data. The AddInvVect function is used to build up the list of inventory vectors when sending a getdata message to another peer.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| InvList | \[\]InvVect | variable | An array of [InvVect](./#invvect) objects, referencing the requested data. |

### notfound

The notfound message is sent in response to a [getdata](./#getdata) message, if any of the requested data in not available on the peer. Each message is limited to a maximum number of inventory vectors, which is currently 50,000. The AddInvVect function is used to build up the list of inventory vectors when sending a notfound message to another peer.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| InvList | \[\]InvVect | variable | An array of [InvVect](./#invvect) objects, referencing the data not found. |

### block

The block message is used to deliver [block](../../../../glossary.md#block) and [transaction](../../../../glossary.md#transaction) information in response to a [getdata](./#getdata) message for a given block hash.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| Header | blockHeader | variable | A [block header](../../../../glossary.md#block-header) |
| Transactions | \[\]tx | variable | An array of transactions within the block |

### tx

The tx message is used to deliver [transaction](../../../../glossary.md#transaction) information in response to a [getdata](./#getdata) message for a given transaction. The AddTxIn and AddTxOut functions are used to build up the list of transaction inputs and outputs.

#### Payload

See [transaction serialized format](../../../../reference/transactions/#Serialized-Format).

### ping

The ping message is used primarily to confirm that a connection is still valid. A transmission error is typically interpreted as a closed connection and that the peer should be removed. The message payload contains an identifying nonce, which can be returned in the pong message to determine network timing.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| Nonce | uint64 | 8 | Used to identify this specific ping message |

### pong

The pong message is used in response to a [ping](./#ping) message to confirm that a connection is still valid.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| Nonce | uint64 | 8 | Used to identify a specific ping message, which this pong message replies to |

### filteradd

The filteradd message is used to add a data element to an existing bloom filter.

{% hint style="info" %}
Not in use. Should be removed, and reintroduced when support for SPV is added.
{% endhint %}

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| Data | \[\]byte | variable | Bloom filter |

### filterclear

The filterclear message is used to reset an existing bloom filter.

{% hint style="info" %}
Not in use. Should be removed, and reintroduced when support for SPV is added.
{% endhint %}

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| Data | \[\]byte | variable | Bloom filter |

### merkleblock

The merkleblock message is used to reset a bloom filter.

{% hint style="info" %}
Not in use. Should be removed, and reintroduced when support for SPV is added.
{% endhint %}

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| Header | \[\]byte | variable |  |
| Transactions | uint32 | 4 |  |
| Hashes | \[\][Hash](../../../../reference/serialized-data-formats/hash.md) |  |  |
| Flags | \[\]byte |  |  |

### reject

The reject message.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| Cmd | string |  | The command for the message which was rejected such as CmdBlock or CmdTx, obtained from the Command function of a Message. |
| Code | uint8 | 1 | A code indicating why the command was rejected |
| Reason | string | variable | A human-readable string with specific details \(over and above the reject code\) about why the command was rejected. |
| Hash | [Hash](../../../../reference/serialized-data-formats/hash.md) | 32 | Identifies a specific block or transaction that was rejected and therefore only applies the MsgBlock and MsgTx messages. |

#### Codes

| Reject Code | Name |
| :--- | :--- |
| 0x01 | RejectMalformed |
| 0x10 | RejectInvalid |
| 0x11 | RejectObsolete |
| 0x12 | RejectDuplicate |
| 0x40 | RejectNonstandard |
| 0x41 | RejectDust |
| 0x42 | RejectInsufficientFee |
| 0x43 | RejectFinality |
| 0x44 | RejectDifficulty |

### feefilter

The feefilter message is used to request that the receiving peer does not announce any [transactions](../../../../glossary.md#transaction) below the specified minimum fee rate.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| MinFee | int64 | 8 |  |

### getlocator

The getlocator message is used to request a block locator between high and low hash. The locator is returned via a [locator message](./#locator).

\[Q: what exactly is a block locator?\]

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| LowHash | [Hash](../../../../reference/serialized-data-formats/hash.md) | 4 | Start with blocks with hash higher than this one |
| HighHash | [Hash](../../../../reference/serialized-data-formats/hash.md) | 4 | End with the block with this hash |

### locator

The locator message is used to find the block locator of a peer that one is syncing with.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| BlockLocatorHashes | \[\][Hash](../../../../reference/serialized-data-formats/hash.md) | variable | Start with blocks with hash higher than this one |

### getseltip

The getseltip message is used to request the [selected tip](../../../../glossary.md#selected-tip-of-the-blockdag) of another peer.

This message has no payload.

### selectedtip

The selectedtip message is used by a node to reply to a [getseltip](./#getseltip) message of a peer node, and providing it with the [selected tip](../../../../glossary.md#selected-tip-of-the-blockdag) it sees.

#### Payload

| Field Name | Type | Bytes | Description |
| :--- | :--- | :--- | :--- |
| SelectedTipHash | [Hash](../../../../reference/serialized-data-formats/hash.md) | 32 | The hash of the selected tip of the node sending the message |



