# Change Log

## **Components**

Unless specified otherwise, all components versions are the main release’s version

1. Kaspad
2. Kasparov:
   1. KasparovSyncD
   2. KasparovD
3. DNSSeeder
4. KaspaMiner
5. Faucet
6. TxGen

## v0.6.8 - 2020-08-31

### Bug

* \[[NOD-1305](https://daglabs.atlassian.net/browse/NOD-1305)\] - Too many file descriptors on dnsseeder kaspad

## v0.6.7 - 2020-08-30

### Feature

* \[[NOD-1322](https://daglabs.atlassian.net/browse/NOD-1322)\] - Fix compilation on windows

### Bug

* \[[NOD-1323](https://daglabs.atlassian.net/browse/NOD-1323)\] - New block reachability data is not always saved

## v0.6.6 - 2020-08-26

### Feature

* \[[NOD-557](https://daglabs.atlassian.net/browse/NOD-557)\] - Remove RegTest network
* \[[NOD-592](https://daglabs.atlassian.net/browse/NOD-592)\] - Go over all of TODO and XXX and fix where possible/open tickets
* \[[NOD-1279](https://daglabs.atlassian.net/browse/NOD-1279)\] - ruleError from IBD block is not being converted to protocolError
* \[[NOD-1295](https://daglabs.atlassian.net/browse/NOD-1295)\] - Better logs for netsync stability test
* \[[NOD-1301](https://daglabs.atlassian.net/browse/NOD-1301)\] - Add MsgReject to protowire mapping
* \[[NOD-1302](https://daglabs.atlassian.net/browse/NOD-1302)\] - Limit rothschild outputs to 100, and use default addresses file if not found
* \[[NOD-1308](https://daglabs.atlassian.net/browse/NOD-1308)\] - Don't call wg.done\(\) on handshake if flow failed
* \[[NOD-1317](https://daglabs.atlassian.net/browse/NOD-1317)\] - Shuffle UTXOs on rothschild
* \[[NOD-1318](https://daglabs.atlassian.net/browse/NOD-1318)\] - Check if relay block is known before requesting it

### Bug

* \[[NOD-1112](https://daglabs.atlassian.net/browse/NOD-1112)\] - Investigate reachability crash
* \[[NOD-1293](https://daglabs.atlassian.net/browse/NOD-1293)\] - Kaspads send 127.0.0.1 in their msgVersions
* \[[NOD-1303](https://daglabs.atlassian.net/browse/NOD-1303)\] - Concurent access to UTXO set from RPC
* \[[NOD-1304](https://daglabs.atlassian.net/browse/NOD-1304)\] - Nil dereference on handshake
* \[[NOD-1307](https://daglabs.atlassian.net/browse/NOD-1307)\] - Kaspad creates double connection to the same node

## v0.6.5 - 2020-08-23

### Feature

* \[[NOD-1028](https://daglabs.atlassian.net/browse/NOD-1028)\] - clear sync block request queue when calling RemoveFromSyncCandidates
* \[[NOD-1061](https://daglabs.atlassian.net/browse/NOD-1061)\] - merge restartSyncIfNeeded and startSync
* \[[NOD-1062](https://daglabs.atlassian.net/browse/NOD-1062)\] - Rename variables in addrmanager.go
* \[[NOD-1065](https://daglabs.atlassian.net/browse/NOD-1065)\] - Consider getting rid of all MsgFilter\* messages and related functionality
* \[[NOD-1068](https://daglabs.atlassian.net/browse/NOD-1068)\] - Fix probabilistic test TestDuplicateOutboundConnections
* \[[NOD-1069](https://daglabs.atlassian.net/browse/NOD-1069)\] - Ignore sync block invs from non sync peers and consider adding ban score
* \[[NOD-1121](https://daglabs.atlassian.net/browse/NOD-1121)\] - Implement ping-pong flow in libp2p
* \[[NOD-1277](https://daglabs.atlassian.net/browse/NOD-1277)\] - Update "automation" repository to contain all non-core components
* \[[NOD-1286](https://daglabs.atlassian.net/browse/NOD-1286)\] - Call router.Close from netconnection.Disconnect
* \[[NOD-1290](https://daglabs.atlassian.net/browse/NOD-1290)\] - Add blocklogger.LogBlock to IBD.

### Bug

* \[[NOD-1114](https://daglabs.atlassian.net/browse/NOD-1114)\] - Bad log: "No peers state was found in the database. Created a new one 0"
* \[[NOD-1289](https://daglabs.atlassian.net/browse/NOD-1289)\] - Connection manager doesn't check for double connections
* \[[NOD-1294](https://daglabs.atlassian.net/browse/NOD-1294)\] - TestTxRelay can close channel twice

## v0.6.4 - 2020-08-19

### Feature

* \[[NOD-1281](https://daglabs.atlassian.net/browse/NOD-1281)\] - stability test for 100k rpc clients
* \[[NOD-1282](https://daglabs.atlassian.net/browse/NOD-1282)\] - Remove peer after disconnect

### Bug

* \[[NOD-684](https://daglabs.atlassian.net/browse/NOD-684)\] - When the blockrate is subsecond, the difficulty adjustment algorithm crashes with a divide-by-zero error
* \[[NOD-1275](https://daglabs.atlassian.net/browse/NOD-1275)\] - OnNewBlock is not called after RPC submitBlock.
* \[[NOD-1285](https://daglabs.atlassian.net/browse/NOD-1285)\] - Deadlock on connectionmanager

## v0.6.3 - 2020-08-16

### Feature

* \[[NOD-1101](https://daglabs.atlassian.net/browse/NOD-1101)\] - Hash data without serializing first
* \[[NOD-1190](https://daglabs.atlassian.net/browse/NOD-1190)\] - Refactor blockDAG
* \[[NOD-1201](https://daglabs.atlassian.net/browse/NOD-1201)\] - Whenever there's an interface with a setter to a callback, add a validation that the interface cannot be used before the callback is set
* \[[NOD-1207](https://daglabs.atlassian.net/browse/NOD-1207)\] - Send Reject messages for txs and blocks that were rejected
* \[[NOD-1223](https://daglabs.atlassian.net/browse/NOD-1223)\] - Re-organize kaspad in a multi-level folder structure
* \[[NOD-1233](https://daglabs.atlassian.net/browse/NOD-1233)\] - Go over all TODO\(libp2p\), fix, or open a ticket
* \[[NOD-1239](https://daglabs.atlassian.net/browse/NOD-1239)\] - Implement protocol WaitForShutdown
* \[[NOD-1256](https://daglabs.atlassian.net/browse/NOD-1256)\] - Optimize PrepareBlockForTest
* \[[NOD-1266](https://daglabs.atlassian.net/browse/NOD-1266)\] - Make simple sync stability test run faster
* \[[NOD-1269](https://daglabs.atlassian.net/browse/NOD-1269)\] - Add profiling to stability tests
* \[[NOD-1270](https://daglabs.atlassian.net/browse/NOD-1270)\] - Use tmpfs in stability tests
* \[[NOD-1271](https://daglabs.atlassian.net/browse/NOD-1271)\] - Move version package to the top level
* \[[NOD-1272](https://daglabs.atlassian.net/browse/NOD-1272)\] - Fix wrong path on stability tests
* \[[NOD-1276](https://daglabs.atlassian.net/browse/NOD-1276)\] - Pass NetworkFlags to MinimalNetAdatper in dnsseeder

### Bug

* \[[NOD-1129](https://daglabs.atlassian.net/browse/NOD-1129)\] - getBlockTemplate sometimes creates incestous blocks
* \[[NOD-1267](https://daglabs.atlassian.net/browse/NOD-1267)\] - Kasparov uses time.Unix\(\) instead of mstime.UnixMillisecond
* \[[NOD-1273](https://daglabs.atlassian.net/browse/NOD-1273)\] - PrepareBlockForTest doesn't order parentHashes

## v0.6.2 - 2020-08-13

### Feature

* \[[NOD-1093](https://daglabs.atlassian.net/browse/NOD-1093)\] - Add hashMerkleRoot to GetBlockTemplateResult
* \[[NOD-1098](https://daglabs.atlassian.net/browse/NOD-1098)\] - Change timestamps to be millisecond precision
* \[[NOD-1149](https://daglabs.atlassian.net/browse/NOD-1149)\] - Disable writing kaspad heap dumps to S3 every minute
* \[[NOD-1204](https://daglabs.atlassian.net/browse/NOD-1204)\] - Add timestamp and serialNumber to every message received and forwarded to flows
* \[[NOD-1220](https://daglabs.atlassian.net/browse/NOD-1220)\] - Add network string field to Version message
* \[[NOD-1221](https://daglabs.atlassian.net/browse/NOD-1221)\] - Explicitly add maximum message size in grpc
* \[[NOD-1234](https://daglabs.atlassian.net/browse/NOD-1234)\] - Add set -e to stability tests
* \[[NOD-1236](https://daglabs.atlassian.net/browse/NOD-1236)\] - Write script that updates version of all repos
* \[[NOD-1257](https://daglabs.atlassian.net/browse/NOD-1257)\] - Disable difficulty on simnet
* \[[NOD-1262](https://daglabs.atlassian.net/browse/NOD-1262)\] - Add network name to MinimalNetAdapter handshake
* \[[NOD-1263](https://daglabs.atlassian.net/browse/NOD-1263)\] - Add run-fast.sh to stability tests
* \[[NOD-1265](https://daglabs.atlassian.net/browse/NOD-1265)\] - Fix compilation error in stability test

### Sub-task

* \[[NOD-1034](https://daglabs.atlassian.net/browse/NOD-1034)\] - Consider removing isFinalized
* \[[NOD-1036](https://daglabs.atlassian.net/browse/NOD-1036)\] - Update block validation rules
* \[[NOD-1045](https://daglabs.atlassian.net/browse/NOD-1045)\] - Do remove DAG tips from the diffStore's loaded set in clearOldEntries

### Bug

* \[[NOD-1259](https://daglabs.atlassian.net/browse/NOD-1259)\] - Invalid blocks/transactions via RPC crash kaspad

## v0.6.1 - 2020-08-09

### Feature

* \[[NOD-1102](https://daglabs.atlassian.net/browse/NOD-1102)\] - Don't check txgen in testnet-tools/get\_stats.sh
* \[[NOD-1222](https://daglabs.atlassian.net/browse/NOD-1222)\] - Turn on gzip in grpc
* \[[NOD-1225](https://daglabs.atlassian.net/browse/NOD-1225)\] - Rename \`wire\` to \`domainmessage\`
* \[[NOD-1231](https://daglabs.atlassian.net/browse/NOD-1231)\] - Fix repositories after libp2p

### Bug

* \[[NOD-1229](https://daglabs.atlassian.net/browse/NOD-1229)\] - If AntiPastHashesBetween lowHigh or highHash are not found in the DAG, node crashes

## v0.6.0 - 2020-05-06

### Summary

* Move from bitcoin-like p2p layer to grpc-based p2p layer
* Update of thread model
* Architecture changes

### Feature

* \[[NOD-454](https://daglabs.atlassian.net/browse/NOD-454)\] - Make GetBlock have a similar interface to GetBlocks
* \[[NOD-534](https://daglabs.atlassian.net/browse/NOD-534)\] - Consider getting rid of the Services field in NetAddress
* \[[NOD-577](https://daglabs.atlassian.net/browse/NOD-577)\] - Change BlueScore to BlueWork in phantom, and maybe other places as well
* \[[NOD-677](https://daglabs.atlassian.net/browse/NOD-677)\] - Remove allowHighFees field from SendRawTransaction RPC call + GetTxOutSetInfoCmd type
* \[[NOD-688](https://daglabs.atlassian.net/browse/NOD-688)\] - consider removing alt stack
* \[[NOD-689](https://daglabs.atlassian.net/browse/NOD-689)\] - Get rid of ripemd160 anywhere it's used
* \[[NOD-756](https://daglabs.atlassian.net/browse/NOD-756)\] - utilize lock time and sequence flags to 64 bits
* \[[NOD-797](https://daglabs.atlassian.net/browse/NOD-797)\] - In mempool, HandleNewBlock locks the mempool lock and the daglock in an error-prone way
* \[[NOD-815](https://daglabs.atlassian.net/browse/NOD-815)\] - Refactor all UTXO-diff algebra methods
* \[[NOD-894](https://daglabs.atlassian.net/browse/NOD-894)\] - Make Hash, TxID and Subnetworkid String\(\) return non-reversed hex
* \[[NOD-896](https://daglabs.atlassian.net/browse/NOD-896)\] - Change address format so that the checksum includes the prefix
* \[[NOD-897](https://daglabs.atlassian.net/browse/NOD-897)\] - Support rolling back flat-files in database transactions
* \[[NOD-906](https://daglabs.atlassian.net/browse/NOD-906)\] - Move transaction mass limit from CheckTransactionSanity to checkTransactionStandard
* \[[NOD-926](https://daglabs.atlassian.net/browse/NOD-926)\] - High memory usage during initial netsync
* \[[NOD-948](https://daglabs.atlassian.net/browse/NOD-948)\] - Add boolean flag to specify native subnetwork to wire transactions
* \[[NOD-950](https://daglabs.atlassian.net/browse/NOD-950)\] - When deploying/resetting testnet - update existing stacks
* \[[NOD-966](https://daglabs.atlassian.net/browse/NOD-966)\] - Add something similar to ./manage.py exec to testnet
* \[[NOD-978](https://daglabs.atlassian.net/browse/NOD-978)\] - Tests fail in Windows
* \[[NOD-979](https://daglabs.atlassian.net/browse/NOD-979)\] - Print --help to stdout instead of stderr
* \[[NOD-1000](https://daglabs.atlassian.net/browse/NOD-1000)\] - Consider adding isConnected flag to getBlockTemplate and --mine-when-not-connected flag to kaspaminer
* \[[NOD-1004](https://daglabs.atlassian.net/browse/NOD-1004)\] - Optimize AddrManager.getAddress to use only 1 loop to check all address chances and pick one of them
* \[[NOD-1016](https://daglabs.atlassian.net/browse/NOD-1016)\] - testnet.py should yell at the user if they attempt to build/deploy with local git commits not set to what's written in testnet/config.yaml
* \[[NOD-1024](https://daglabs.atlassian.net/browse/NOD-1024)\] - Remove %+v where not needed
* \[[NOD-1025](https://daglabs.atlassian.net/browse/NOD-1025)\] - Rename any place that refers to "bluestParent" to "selectedParent"
* \[[NOD-1026](https://daglabs.atlassian.net/browse/NOD-1026)\] - Log all orphan blocks with Info level, and remove the message that says "Adding orphan block %s. This is normal part of netsync process"
* \[[NOD-1027](https://daglabs.atlassian.net/browse/NOD-1027)\] - Refactor address manager.
* \[[NOD-1028](https://daglabs.atlassian.net/browse/NOD-1028)\] - clear sync block request queue when calling RemoveFromSyncCandidates
* \[[NOD-1032](https://daglabs.atlassian.net/browse/NOD-1032)\] - Pruning Part 1+2: Block Classification + Non-Validation of Red Blocks
* \[[NOD-1037](https://daglabs.atlassian.net/browse/NOD-1037)\] - Kasparov: show kasparov version and kaspad version in kasparov:8080/version
* \[[NOD-1044](https://daglabs.atlassian.net/browse/NOD-1044)\] - Pruning Part 3: Pruning Proper
* \[[NOD-1047](https://daglabs.atlassian.net/browse/NOD-1047)\] - Add test to add 1 block with POW on each network
* \[[NOD-1054](https://daglabs.atlassian.net/browse/NOD-1054)\] - Use same encoding flags for coinbase when serializing transaction for txID
* \[[NOD-1060](https://daglabs.atlassian.net/browse/NOD-1060)\] - replace sync peer in all places where sync peer is misbehaving as part of the netsync protocol
* \[[NOD-1061](https://daglabs.atlassian.net/browse/NOD-1061)\] - merge restartSyncIfNeeded and startSync
* \[[NOD-1062](https://daglabs.atlassian.net/browse/NOD-1062)\] - Rename variables in addrmanager.go
* \[[NOD-1065](https://daglabs.atlassian.net/browse/NOD-1065)\] - Consider getting rid of all MsgFilter\* messages and related functionality
* \[[NOD-1066](https://daglabs.atlassian.net/browse/NOD-1066)\] - Kasparov: Add acceptedFrom and includingBlocks fields in transactions
* \[[NOD-1068](https://daglabs.atlassian.net/browse/NOD-1068)\] - Fix probabilistic test TestDuplicateOutboundConnections
* \[[NOD-1069](https://daglabs.atlassian.net/browse/NOD-1069)\] - Ignore sync block invs from non sync peers and consider adding ban score
* \[[NOD-1071](https://daglabs.atlassian.net/browse/NOD-1071)\] - Write test for Application-level garbage
* \[[NOD-1072](https://daglabs.atlassian.net/browse/NOD-1072)\] - Write test for Infa-level garbage
* \[[NOD-1073](https://daglabs.atlassian.net/browse/NOD-1073)\] - Write test for JSON-RPC stability
* \[[NOD-1074](https://daglabs.atlassian.net/browse/NOD-1074)\] - Create tool that can build DAG from some json-description
* \[[NOD-1077](https://daglabs.atlassian.net/browse/NOD-1077)\] - Decide the right RelayNonStdTxs for each network
* \[[NOD-1080](https://daglabs.atlassian.net/browse/NOD-1080)\] - Add datadog alert when a node is banned
* \[[NOD-1081](https://daglabs.atlassian.net/browse/NOD-1081)\] - Write test that makes sure node stays on same tip
* \[[NOD-1082](https://daglabs.atlassian.net/browse/NOD-1082)\] - Write script for basic kaspad sanity test
* \[[NOD-1083](https://daglabs.atlassian.net/browse/NOD-1083)\] - Write basic netsync test
* \[[NOD-1085](https://daglabs.atlassian.net/browse/NOD-1085)\] - Fix kaspaminer to use all the nonce space
* \[[NOD-1089](https://daglabs.atlassian.net/browse/NOD-1089)\] - Optimize the reachability tree further
* \[[NOD-1093](https://daglabs.atlassian.net/browse/NOD-1093)\] - Add hashMerkleRoot to GetBlockTemplateResult
* \[[NOD-1094](https://daglabs.atlassian.net/browse/NOD-1094)\] - Replace modifiedNodes with dirty set that will be affected automatically by rtn's setters
* \[[NOD-1098](https://daglabs.atlassian.net/browse/NOD-1098)\] - Change timestamps to be millisecond precision
* \[[NOD-1100](https://daglabs.atlassian.net/browse/NOD-1100)\] - Add something similar to kickstart-mining.py to testnet
* \[[NOD-1106](https://daglabs.atlassian.net/browse/NOD-1106)\] - Add readme to simple-sync
* \[[NOD-1107](https://daglabs.atlassian.net/browse/NOD-1107)\] - Go over all db transactions in kaspad, and make sure they can't grow to unmanageable size
* \[[NOD-1111](https://daglabs.atlassian.net/browse/NOD-1111)\] - Don't use util.block in kaspaminer
* \[[NOD-1117](https://daglabs.atlassian.net/browse/NOD-1117)\] - Write interfaces for the P2P layer
* \[[NOD-1118](https://daglabs.atlassian.net/browse/NOD-1118)\] - Implement basic connectivity through grpc
* \[[NOD-1119](https://daglabs.atlassian.net/browse/NOD-1119)\] - Write alternative main that parses CLI args and starts RPC server + all blockdag structures but not p2p
* \[[NOD-1120](https://daglabs.atlassian.net/browse/NOD-1120)\] - Implement connection manager
* \[[NOD-1121](https://daglabs.atlassian.net/browse/NOD-1121)\] - Implement ping-pong flow in libp2p
* \[[NOD-1122](https://daglabs.atlassian.net/browse/NOD-1122)\] - Implement Stall Control in grpc
* \[[NOD-1123](https://daglabs.atlassian.net/browse/NOD-1123)\] - Implement banning support in grpc
* \[[NOD-1124](https://daglabs.atlassian.net/browse/NOD-1124)\] - Implement the Flow thread model and architecture
* \[[NOD-1125](https://daglabs.atlassian.net/browse/NOD-1125)\] - Implement IBD Flow
* \[[NOD-1126](https://daglabs.atlassian.net/browse/NOD-1126)\] - Implement Day-to-Day sync flow
* \[[NOD-1127](https://daglabs.atlassian.net/browse/NOD-1127)\] - Implement Transaction Propagation and Rebroadcast flow
* \[[NOD-1128](https://daglabs.atlassian.net/browse/NOD-1128)\] - Convert msgType to int-based enum
* \[[NOD-1130](https://daglabs.atlassian.net/browse/NOD-1130)\] - Update all P2P-related RPC calls by integrating to new system
* \[[NOD-1133](https://daglabs.atlassian.net/browse/NOD-1133)\] - Decide how to handle transient ban scores on getdata since IBD and normal block relay have their separate message types
* \[[NOD-1134](https://daglabs.atlassian.net/browse/NOD-1134)\] - Integrate Protocol into main
* \[[NOD-1136](https://daglabs.atlassian.net/browse/NOD-1136)\] - Write a component that stringifies peer ID to IP, so we can use it in Peer.String\(\)
* \[[NOD-1137](https://daglabs.atlassian.net/browse/NOD-1137)\] - Implement handshake flow
* \[[NOD-1142](https://daglabs.atlassian.net/browse/NOD-1142)\] - Implement ping flow
* \[[NOD-1143](https://daglabs.atlassian.net/browse/NOD-1143)\] - Validating user agent \(including comments\) when node is starting
* \[[NOD-1145](https://daglabs.atlassian.net/browse/NOD-1145)\] - Normalize panics in flows
* \[[NOD-1146](https://daglabs.atlassian.net/browse/NOD-1146)\] - Re-implement RPC commands that touch connectionManager
* \[[NOD-1147](https://daglabs.atlassian.net/browse/NOD-1147)\] - Implement address exchange in grpc
* \[[NOD-1148](https://daglabs.atlassian.net/browse/NOD-1148)\] - Stabilize P2P layer
* \[[NOD-1149](https://daglabs.atlassian.net/browse/NOD-1149)\] - Disable writing kaspad heap dumps to S3 every minute
* \[[NOD-1150](https://daglabs.atlassian.net/browse/NOD-1150)\] - Add in netadapter function to disconnect router
* \[[NOD-1151](https://daglabs.atlassian.net/browse/NOD-1151)\] - Update DNSSeeder to use grpc
* \[[NOD-1152](https://daglabs.atlassian.net/browse/NOD-1152)\] - Move banning out of netadapter
* \[[NOD-1153](https://daglabs.atlassian.net/browse/NOD-1153)\] - Pass NetConnection to the protocol package and use where appropriate
* \[[NOD-1155](https://daglabs.atlassian.net/browse/NOD-1155)\] - Put netconnection inside peer and use it to disconnect and for String\(\)
* \[[NOD-1157](https://daglabs.atlassian.net/browse/NOD-1157)\] - Make ProcessTransaction always return the given tx as accepted transaction even if it's not orphan
* \[[NOD-1160](https://daglabs.atlassian.net/browse/NOD-1160)\] - Convert config.ActiveConfig from singleton to an object that is being passed around
* \[[NOD-1161](https://daglabs.atlassian.net/browse/NOD-1161)\] - Spawn should get name + generate a unique ID and print those on start and stop
* \[[NOD-1162](https://daglabs.atlassian.net/browse/NOD-1162)\] - Write basic integration tests for kaspad
* \[[NOD-1163](https://daglabs.atlassian.net/browse/NOD-1163)\] - Combine separated flows into single package
* \[[NOD-1164](https://daglabs.atlassian.net/browse/NOD-1164)\] - Convert dbaccess from singleton to object that is passed around
* \[[NOD-1168](https://daglabs.atlassian.net/browse/NOD-1168)\] - Change the way protocol manager supplies information to flows
* \[[NOD-1170](https://daglabs.atlassian.net/browse/NOD-1170)\] - Return a custom error when a route is closed
* \[[NOD-1171](https://daglabs.atlassian.net/browse/NOD-1171)\] - Figure out what to do with the RPC command getNetTotals
* \[[NOD-1172](https://daglabs.atlassian.net/browse/NOD-1172)\] - Instead of returning isClosed - return ErrIsClosed
* \[[NOD-1175](https://daglabs.atlassian.net/browse/NOD-1175)\] - Implement AddBlock in ProtocolManager
* \[[NOD-1176](https://daglabs.atlassian.net/browse/NOD-1176)\] - Flows that pass around a large number of arguments between function calls should have a dedicated object
* \[[NOD-1177](https://daglabs.atlassian.net/browse/NOD-1177)\] - In handleGetConnectedPeerInfo, populate the missing result fields
* \[[NOD-1180](https://daglabs.atlassian.net/browse/NOD-1180)\] - Limit bans to 24
* \[[NOD-1181](https://daglabs.atlassian.net/browse/NOD-1181)\] - Mark banned peers in address manager and persist bans to disk
* \[[NOD-1183](https://daglabs.atlassian.net/browse/NOD-1183)\] - Re-enable RPC notifications
* \[[NOD-1186](https://daglabs.atlassian.net/browse/NOD-1186)\] - Change Peer ID to public key, and sign on version message on a constant message
* \[[NOD-1187](https://daglabs.atlassian.net/browse/NOD-1187)\] - Don't return banned peers when asking for an address
* \[[NOD-1191](https://daglabs.atlassian.net/browse/NOD-1191)\] - Convert wire protocol to 100% protobuf
* \[[NOD-1194](https://daglabs.atlassian.net/browse/NOD-1194)\] - Handle ErrRouteClosed as a special case
* \[[NOD-1195](https://daglabs.atlassian.net/browse/NOD-1195)\] - In checkOutgoingConnections, make the connection loop not probabilistic
* \[[NOD-1196](https://daglabs.atlassian.net/browse/NOD-1196)\] - Get rid of unnecessary messages
* \[[NOD-1197](https://daglabs.atlassian.net/browse/NOD-1197)\] - Ban incoming peers that send us a bad version message
* \[[NOD-1198](https://daglabs.atlassian.net/browse/NOD-1198)\] - Replace NetAdapter.connectionsToRouters with a reference to Router from NetConnection
* \[[NOD-1201](https://daglabs.atlassian.net/browse/NOD-1201)\] - Whenever there's an interface with a setter to a callback, add a validation that the interface cannot be used before the callback is set
* \[[NOD-1203](https://daglabs.atlassian.net/browse/NOD-1203)\] - Create netadapter outside of protocol manager
* \[[NOD-1204](https://daglabs.atlassian.net/browse/NOD-1204)\] - Add timestamp and serialNumber to every message received and forwarded to flows
* \[[NOD-1206](https://daglabs.atlassian.net/browse/NOD-1206)\] - Call peer.StartIBD in new goroutine
* \[[NOD-1207](https://daglabs.atlassian.net/browse/NOD-1207)\] - Send Reject messages for txs and blocks that were rejected
* \[[NOD-1208](https://daglabs.atlassian.net/browse/NOD-1208)\] - Fix Stability Tests
* \[[NOD-1210](https://daglabs.atlassian.net/browse/NOD-1210)\] - Write IBD integration test
* \[[NOD-1212](https://daglabs.atlassian.net/browse/NOD-1212)\] - Request IBD blocks in batches
* \[[NOD-1213](https://daglabs.atlassian.net/browse/NOD-1213)\] - Consider extracting validation logic out of protowire
* \[[NOD-1214](https://daglabs.atlassian.net/browse/NOD-1214)\] - Create integration tests of as much as possible incoming connections to single node
* \[[NOD-1215](https://daglabs.atlassian.net/browse/NOD-1215)\] - Create integration test for address exchange
* \[[NOD-1217](https://daglabs.atlassian.net/browse/NOD-1217)\] - Probabilistic test in rpc/model
* \[[NOD-1219](https://daglabs.atlassian.net/browse/NOD-1219)\] - Ban a peer if during IBD its sync rate is very low
* \[[NOD-1220](https://daglabs.atlassian.net/browse/NOD-1220)\] - Add network string field to Version message
* \[[NOD-1222](https://daglabs.atlassian.net/browse/NOD-1222)\] - Turn on gzip in grpc
* \[[NOD-1223](https://daglabs.atlassian.net/browse/NOD-1223)\] - Re-organize kaspad in a multi-level folder structure
* \[[NOD-1225](https://daglabs.atlassian.net/browse/NOD-1225)\] - Rename \`wire\` to \`domainmodel\`
* \[[NOD-1227](https://daglabs.atlassian.net/browse/NOD-1227)\] - Prepare infrastructure for automated running of stability tests
* \[[NOD-1228](https://daglabs.atlassian.net/browse/NOD-1228)\] - Prepare DNSSeeder for 0.6.0-libp2p
* \[[NOD-1230](https://daglabs.atlassian.net/browse/NOD-1230)\] - Merge v0.6.0-dev and v0.6.0-libp2p
* \[[NOD-1231](https://daglabs.atlassian.net/browse/NOD-1231)\] - Fix repositories after libp2p

### Epic

* \[[NOD-1116](https://daglabs.atlassian.net/browse/NOD-1116)\] - Move p2p layer to grpc

### Sub-task

* \[[NOD-1034](https://daglabs.atlassian.net/browse/NOD-1034)\] - Consider removing isFinalized
* \[[NOD-1036](https://daglabs.atlassian.net/browse/NOD-1036)\] - Update block validation rules
* \[[NOD-1045](https://daglabs.atlassian.net/browse/NOD-1045)\] - Do remove DAG tips from the diffStore's loaded set in clearOldEntries
* \[[NOD-1056](https://daglabs.atlassian.net/browse/NOD-1056)\] - Don't check isFinalized when validating parents
* \[[NOD-1057](https://daglabs.atlassian.net/browse/NOD-1057)\] - Merge 0.5.0-dev into nod-1032
* \[[NOD-1067](https://daglabs.atlassian.net/browse/NOD-1067)\] - Add support for deletion in ffldb

### Bug

* \[[NOD-681](https://daglabs.atlassian.net/browse/NOD-681)\] - error on setting MaxInvPerMsg to 50 on all nodes on local network
* \[[NOD-684](https://daglabs.atlassian.net/browse/NOD-684)\] - When the blockrate is subsecond, the difficulty adjustment algorithm crashes with a divide-by-zero error
* \[[NOD-908](https://daglabs.atlassian.net/browse/NOD-908)\] - Error submitting block from kaspaminer due to timeout
* \[[NOD-980](https://daglabs.atlassian.net/browse/NOD-980)\] - Unintuitive error message when connecting with --notls to kaspad with tls
* \[[NOD-997](https://daglabs.atlassian.net/browse/NOD-997)\] - Concurrent map read and map write in rpcmodel
* \[[NOD-1029](https://daglabs.atlassian.net/browse/NOD-1029)\] - maybeAcceptBlock doesn't handle well the situation where addNodeToIndexWithInvalidAncestor fails
* \[[NOD-1055](https://daglabs.atlassian.net/browse/NOD-1055)\] - Kasparov shows accepting block IDs for coinbase transactions outside selectedParentChain
* \[[NOD-1075](https://daglabs.atlassian.net/browse/NOD-1075)\] - If mining address is not supplied, kaspaminer crashes with "invalid bech32 string length 0"
* \[[NOD-1076](https://daglabs.atlassian.net/browse/NOD-1076)\] - In testnet deployment, there can only be one mainVpcRoute per region, regardless of prefix
* \[[NOD-1079](https://daglabs.atlassian.net/browse/NOD-1079)\] - Nodes ban each other
* \[[NOD-1095](https://daglabs.atlassian.net/browse/NOD-1095)\] - Data races in kaspad
* \[[NOD-1099](https://daglabs.atlassian.net/browse/NOD-1099)\] - Extreme oscillations in hashrate
* \[[NOD-1110](https://daglabs.atlassian.net/browse/NOD-1110)\] - Faucet@testnet dies because private key is base58 instead of hex
* \[[NOD-1112](https://daglabs.atlassian.net/browse/NOD-1112)\] - Investigate reachability crash
* \[[NOD-1113](https://daglabs.atlassian.net/browse/NOD-1113)\] - BlockHeader.BlockHash\(\), MsgTx.TxHash\(\), MsgTx.TxID\(\) all ignore errors from serializers
* \[[NOD-1114](https://daglabs.atlassian.net/browse/NOD-1114)\] - Bad log: "No peers state was found in the database. Created a new one 0"
* \[[NOD-1129](https://daglabs.atlassian.net/browse/NOD-1129)\] - getBlockTemplate sometimes creates incestous blocks
* \[[NOD-1144](https://daglabs.atlassian.net/browse/NOD-1144)\] - CLI Wallet \`create\` prints some weird decimal string instead of hex private-key
* \[[NOD-1184](https://daglabs.atlassian.net/browse/NOD-1184)\] - Concurrent map read and write in router
* \[[NOD-1185](https://daglabs.atlassian.net/browse/NOD-1185)\] - Blocks are not propagated when submitted through RPC
* \[[NOD-1189](https://daglabs.atlassian.net/browse/NOD-1189)\] - BlockRelay flow: filtering of requested block should be on pendingBlocks
* \[[NOD-1192](https://daglabs.atlassian.net/browse/NOD-1192)\] - Deadlock in txPool.HandleNewBlock
* \[[NOD-1193](https://daglabs.atlassian.net/browse/NOD-1193)\] - Race condition in flowContext.Broadcast
* \[[NOD-1200](https://daglabs.atlassian.net/browse/NOD-1200)\] - Race condition between receiveLoop and startFlows
* \[[NOD-1205](https://daglabs.atlassian.net/browse/NOD-1205)\] - RemoveRoute wouldn't work if some message types map to the same route
* \[[NOD-1218](https://daglabs.atlassian.net/browse/NOD-1218)\] - A lot of ping-pongs sent for no reason
* \[[NOD-1224](https://daglabs.atlassian.net/browse/NOD-1224)\] - Duplicate connections cause same block to be requested twice and eventually a crash
* \[[NOD-1226](https://daglabs.atlassian.net/browse/NOD-1226)\] - Connection closed during handshake causes a crash
* \[[NOD-1229](https://daglabs.atlassian.net/browse/NOD-1229)\] - If AntiPastHashesBetween lowHigh or highHash are not found in the DAG, node crashes

## 0.5.0 - 2020-06-25

### Bug

* \[[NOD-763](https://daglabs.atlassian.net/browse/NOD-763)\] - Block Version endianness appears to be wrong
* \[[NOD-1013](https://daglabs.atlassian.net/browse/NOD-1013)\] - Potential deadlock when shutting down kaspad
* \[[NOD-1019](https://daglabs.atlassian.net/browse/NOD-1019)\] - Testnet suffix is not visible in datadog tags
* \[[NOD-1059](https://daglabs.atlassian.net/browse/NOD-1059)\] - Nodes don't request relay invs if all of them are on the same tip
* \[[NOD-1103](https://daglabs.atlassian.net/browse/NOD-1103)\] - Invalid genesis for testnet
* \[[NOD-1105](https://daglabs.atlassian.net/browse/NOD-1105)\] - Large acceptance index recoveries may cause Kaspad to run out of memory

### Sub-task

* \[[NOD-1033](https://daglabs.atlassian.net/browse/NOD-1033)\] - Update block size check in checkBlockSanity
* \[[NOD-1035](https://daglabs.atlassian.net/browse/NOD-1035)\] - Change checkFinalityRules to isFinalityPointInSelectedParentChain

### Feature

* \[[NOD-530](https://daglabs.atlassian.net/browse/NOD-530)\] - Remove Coinbase inputs. Add blueScore to payload.
* \[[NOD-553](https://daglabs.atlassian.net/browse/NOD-553)\] - Anywhere that anything is base58 - update to hex \(or bech32 if applicable\). Then delete package util/base58
* \[[NOD-614](https://daglabs.atlassian.net/browse/NOD-614)\] - Incrementing ban score for misbehaviors
* \[[NOD-809](https://daglabs.atlassian.net/browse/NOD-809)\] - Change FeePerKB to FeePerMegaGram
* \[[NOD-830](https://daglabs.atlassian.net/browse/NOD-830)\] - Consider getting rid of TransactionResponse.acceptingBlockHash's omitEmpty
* \[[NOD-833](https://daglabs.atlassian.net/browse/NOD-833)\] - Consider moving mining-addr to getBlockTemplate params
* \[[NOD-860](https://daglabs.atlassian.net/browse/NOD-860)\] - Get rid of getBlockTemplate.capabilities + Make it return a simple block
* \[[NOD-892](https://daglabs.atlassian.net/browse/NOD-892)\] - Decide how --dropXXXindex should behave in case of a crash/sudden exit
* \[[NOD-907](https://daglabs.atlassian.net/browse/NOD-907)\] - Set defaults for open-devnet and big
* \[[NOD-932](https://daglabs.atlassian.net/browse/NOD-932)\] - Crash dnsseeder if it's attached node is unreachable
* \[[NOD-954](https://daglabs.atlassian.net/browse/NOD-954)\] - In testnet, redirect all http requests to an ELB to https
* \[[NOD-964](https://daglabs.atlassian.net/browse/NOD-964)\] - Add txgen to testnet
* \[[NOD-965](https://daglabs.atlassian.net/browse/NOD-965)\] - dag.index.LookupNode should also return an ok boolean value
* \[[NOD-984](https://daglabs.atlassian.net/browse/NOD-984)\] - Allow --outpeers to be 0
* \[[NOD-999](https://daglabs.atlassian.net/browse/NOD-999)\] - Fix probabilistic test TestMaxRetryDuration
* \[[NOD-1002](https://daglabs.atlassian.net/browse/NOD-1002)\] - Make kasparov transactions have ordered inputs and outputs
* \[[NOD-1007](https://daglabs.atlassian.net/browse/NOD-1007)\] - Breakdown checkBlockSanity to sub-routines.
* \[[NOD-1009](https://daglabs.atlassian.net/browse/NOD-1009)\] - Add script that kickstarts devnet with single --mine-when-not-synced block
* \[[NOD-1010](https://daglabs.atlassian.net/browse/NOD-1010)\] - In kaspaminer, stop checking getBlockTemplate's isSynced after first time it's true
* \[[NOD-1012](https://daglabs.atlassian.net/browse/NOD-1012)\] - Disable partial nodes + subnetwork txs in mempool and blocks \(including registering new subnetworks
* \[[NOD-1017](https://daglabs.atlassian.net/browse/NOD-1017)\] - Move peers.json into the database
* \[[NOD-1020](https://daglabs.atlassian.net/browse/NOD-1020)\] - Do send addr response to getaddr messages even if there aren't any addresses to send
* \[[NOD-1023](https://daglabs.atlassian.net/browse/NOD-1023)\] - search for all functions which are called isCurrent/current/etc and check if it's needed to be changed to the new isSynced
* \[[NOD-1038](https://daglabs.atlassian.net/browse/NOD-1038)\] - Consider adding orphans missing ancestors to the start of the request queue
* \[[NOD-1042](https://daglabs.atlassian.net/browse/NOD-1042)\] - Ignore orphans that are way higher than your selected tip
* \[[NOD-1046](https://daglabs.atlassian.net/browse/NOD-1046)\] - Make ruleError accept an error instead of string and wrap that error
* \[[NOD-1050](https://daglabs.atlassian.net/browse/NOD-1050)\] - Use hex private key in txgen
* \[[NOD-1053](https://daglabs.atlassian.net/browse/NOD-1053)\] - Use hex private key in faucet
* \[[NOD-1063](https://daglabs.atlassian.net/browse/NOD-1063)\] - Optimize deep reachability tree insertions
* \[[NOD-1064](https://daglabs.atlassian.net/browse/NOD-1064)\] - Fix probabilistic test TestOutboundPeer
* \[[NOD-1078](https://daglabs.atlassian.net/browse/NOD-1078)\] - Add --nobanning to all servers in devnet
* \[[NOD-1086](https://daglabs.atlassian.net/browse/NOD-1086)\] - Make isAncestorOf inclusive and consider renaming
* \[[NOD-1088](https://daglabs.atlassian.net/browse/NOD-1088)\] - Rename RejectReasion to RejectReason

## 0.4.1 - 2020-06-21

### Bug

* \[[NOD-970](https://daglabs.atlassian.net/browse/NOD-970)\] - Orphans during netsync, potentially causing the netsync to be stuck after restart
* \[[NOD-1041](https://daglabs.atlassian.net/browse/NOD-1041)\] - Deadlock between connHandler and peerHandler
* \[[NOD-1052](https://daglabs.atlassian.net/browse/NOD-1052)\] - Concurrent map read/write in utxoDiffStore.clearOldEntries\(\)

### Feature

* \[[NOD-1030](https://daglabs.atlassian.net/browse/NOD-1030)\] - Disconnect from syncPeer that sends orphans
* \[[NOD-1039](https://daglabs.atlassian.net/browse/NOD-1039)\] - Remove the call to SetGCPercent in Kaspad
* \[[NOD-1040](https://daglabs.atlassian.net/browse/NOD-1040)\] - Don't remove DAG tips from the diffStore's loaded set
* \[[NOD-1048](https://daglabs.atlassian.net/browse/NOD-1048)\] - Make leveldb compactions less frequent
* \[[NOD-1049](https://daglabs.atlassian.net/browse/NOD-1049)\] - Allow empty addr messages
* \[[NOD-1051](https://daglabs.atlassian.net/browse/NOD-1051)\] - Don't disconnect from sync peer if it sends an orphan

## 0.4.0 - 2020-05-20

### Bug

* \[[NOD-620](https://daglabs.atlassian.net/browse/NOD-620)\] - CLI WALLET: /utxos returns utxos that were just spent
* \[[NOD-837](https://daglabs.atlassian.net/browse/NOD-837)\] - Kasparov\_REST: Sending a GET "BLOCKS" or GET "Transactions" request with limit = 0 returns blocks/txs instead of only total
* \[[NOD-911](https://daglabs.atlassian.net/browse/NOD-911)\] - BLOCK\_DELAY is not set when doing ./manage.py upgrade
* \[[NOD-913](https://daglabs.atlassian.net/browse/NOD-913)\] - If network was stuck for more then 12 hours, it no longer mines blocks
* \[[NOD-915](https://daglabs.atlassian.net/browse/NOD-915)\] - On graceful shutdown, the node crashes ungracefully if no data was written to the flat file write file
* \[[NOD-930](https://daglabs.atlassian.net/browse/NOD-930)\] - Massive reachability tree reorgs cause a spike in memory consuption
* \[[NOD-934](https://daglabs.atlassian.net/browse/NOD-934)\] - Address manager shows a strong bias towards a single address
* \[[NOD-952](https://daglabs.atlassian.net/browse/NOD-952)\] - Nil dereference bug when GetNewAddress doesn't find an address
* \[[NOD-971](https://daglabs.atlassian.net/browse/NOD-971)\] - Netsync does not restart after sync peer disconnects
* \[[NOD-973](https://daglabs.atlassian.net/browse/NOD-973)\] - Devnet servers run out of disk space because application logs are also written to /var/log/messages-\*
* \[[NOD-981](https://daglabs.atlassian.net/browse/NOD-981)\] - Wrong error message when both --rpccert and --notls are ommited
* \[[NOD-983](https://daglabs.atlassian.net/browse/NOD-983)\] - Devnet blocks have an invalid UTXO commitment
* \[[NOD-987](https://daglabs.atlassian.net/browse/NOD-987)\] - stuff's get\_data.sh doesn't filter logs by service
* \[[NOD-993](https://daglabs.atlassian.net/browse/NOD-993)\] - Not all errors are pkg/errors compliant
* \[[NOD-1001](https://daglabs.atlassian.net/browse/NOD-1001)\] - Node's syncManager stuck after temporary network hiccups
* \[[NOD-1005](https://daglabs.atlassian.net/browse/NOD-1005)\] - A new node in a new network doesn't requests invs
* \[[NOD-1011](https://daglabs.atlassian.net/browse/NOD-1011)\] - Long Poll GetBlockTemplate cache becomes a problem when isSynced=false
* \[[NOD-1014](https://daglabs.atlassian.net/browse/NOD-1014)\] - Deadlock in DNSSeeder between AssociateConnection and QueueMessage

### Sub-task

* \[[NOD-975](https://daglabs.atlassian.net/browse/NOD-975)\] - Implement "Don't include block data inside UTXO commitments" without fixing tests
* \[[NOD-976](https://daglabs.atlassian.net/browse/NOD-976)\] - Fix tests after "Don't include block data inside UTXO commitments"
* \[[NOD-990](https://daglabs.atlassian.net/browse/NOD-990)\] - Optimize UTXO commitment logic

### Feature

* \[[NOD-441](https://daglabs.atlassian.net/browse/NOD-441)\] - Check initial sync time of long and wide dag \(at least 10k blocks\)
* \[[NOD-480](https://daglabs.atlassian.net/browse/NOD-480)\] - GitHub preparation
* \[[NOD-820](https://daglabs.atlassian.net/browse/NOD-820)\] - handleGetBlockTemplate should never return an error unless something unexpected happens, and should return indicators for mining as booleans
* \[[NOD-822](https://daglabs.atlassian.net/browse/NOD-822)\] - Don't return rule error from WithDiff, DiffFrom, etc.
* \[[NOD-847](https://daglabs.atlassian.net/browse/NOD-847)\] - Investigate and fix multiple parrallel connections in kaspad
* \[[NOD-849](https://daglabs.atlassian.net/browse/NOD-849)\] - Write tests for the new database layer
* \[[NOD-850](https://daglabs.atlassian.net/browse/NOD-850)\] - Adding block delay parameter to manage.py
* \[[NOD-863](https://daglabs.atlassian.net/browse/NOD-863)\] - Implement a set of tests that verifies all the assumptions of database/interface.go
* \[[NOD-872](https://daglabs.atlassian.net/browse/NOD-872)\] - In the new database, defer all currently undeferred unlocks
* \[[NOD-873](https://daglabs.atlassian.net/browse/NOD-873)\] - Optimize dbPutUTXODiff to reuse the same bytes and allocate less memory
* \[[NOD-877](https://daglabs.atlassian.net/browse/NOD-877)\] - Separate UTXO header code to two fields in serialization: blue score and packed flags
* \[[NOD-879](https://daglabs.atlassian.net/browse/NOD-879)\] - Add correct version constructs to go-secp and modify go.mod in kaspad + related devops to support it
* \[[NOD-885](https://daglabs.atlassian.net/browse/NOD-885)\] - In database interfaces, replace byte arrays with Bucket, Key, and FlatFileLocation types
* \[[NOD-895](https://daglabs.atlassian.net/browse/NOD-895)\] - Break down initDAGState to sub-routines
* \[[NOD-899](https://daglabs.atlassian.net/browse/NOD-899)\] - Inside the database, in case we're out of disk space, panic without printing the stack trace
* \[[NOD-901](https://daglabs.atlassian.net/browse/NOD-901)\] - Don't subscribe KasparovD to notifications of chainChanged and blockAdded
* \[[NOD-902](https://daglabs.atlassian.net/browse/NOD-902)\] - Allow Etc/UTC to be used in kasparov
* \[[NOD-905](https://daglabs.atlassian.net/browse/NOD-905)\] - Fix the nightly builds
* \[[NOD-909](https://daglabs.atlassian.net/browse/NOD-909)\] - Add tests for double spends
* \[[NOD-912](https://daglabs.atlassian.net/browse/NOD-912)\] - Return error from DataAccesor.Cursor if database/tx is closed
* \[[NOD-914](https://daglabs.atlassian.net/browse/NOD-914)\] - Fix double slash on cursor.Key\(\)
* \[[NOD-916](https://daglabs.atlassian.net/browse/NOD-916)\] - Kasparov: Transactions in Block REST API Call
* \[[NOD-922](https://daglabs.atlassian.net/browse/NOD-922)\] - Decide how to handle errors in database.Cursor
* \[[NOD-924](https://daglabs.atlassian.net/browse/NOD-924)\] - Create script that kills one random server out of the kaspads in long-running devnet and run every night
* \[[NOD-939](https://daglabs.atlassian.net/browse/NOD-939)\] - Optimize Count operations
* \[[NOD-941](https://daglabs.atlassian.net/browse/NOD-941)\] - Split total calls from data calls in kasparov
* \[[NOD-943](https://daglabs.atlassian.net/browse/NOD-943)\] - Kaspad: getBlock should return Accepted Blocks Hashes
* \[[NOD-944](https://daglabs.atlassian.net/browse/NOD-944)\] - Kasparov: add AcceptedBlocksHashes array whenever returning a Block object
* \[[NOD-946](https://daglabs.atlassian.net/browse/NOD-946)\] - Profile heap on devnet servers every N minutes
* \[[NOD-947](https://daglabs.atlassian.net/browse/NOD-947)\] - Evaluate cli-arguments and RCP-calls
* \[[NOD-949](https://daglabs.atlassian.net/browse/NOD-949)\] - Write something similar to get\_stats.sh for testnet
* \[[NOD-951](https://daglabs.atlassian.net/browse/NOD-951)\] - cpulimit doesn't run on host restarts
* \[[NOD-955](https://daglabs.atlassian.net/browse/NOD-955)\] - Figure a way out of faucet calling /utxo on the mining address
* \[[NOD-956](https://daglabs.atlassian.net/browse/NOD-956)\] - Lower minimum difficulty
* \[[NOD-963](https://daglabs.atlassian.net/browse/NOD-963)\] - Add option to run non-mining kaspad nodes in devnet and testnet
* \[[NOD-968](https://daglabs.atlassian.net/browse/NOD-968)\] - Wrap any ldb errors with pkg/errors to get a stack trace
* \[[NOD-972](https://daglabs.atlassian.net/browse/NOD-972)\] - Use S3Path constant directly in cloudformation
* \[[NOD-974](https://daglabs.atlassian.net/browse/NOD-974)\] - Don't include block data inside UTXO commitments
* \[[NOD-982](https://daglabs.atlassian.net/browse/NOD-982)\] - Log message with level WARN when getting MsgReject
* \[[NOD-985](https://daglabs.atlassian.net/browse/NOD-985)\] - Add profiling server to all services in testnet
* \[[NOD-986](https://daglabs.atlassian.net/browse/NOD-986)\] - Write a tool to find any aws instance details \(private/public ip, region, stack name\) by private/public ip
* \[[NOD-988](https://daglabs.atlassian.net/browse/NOD-988)\] - Make datadog read logs from file instead of from dockerd to save memory
* \[[NOD-991](https://daglabs.atlassian.net/browse/NOD-991)\] - kasparov /transactions/address/{address} very slow when there are millions of transactions for given address
* \[[NOD-994](https://daglabs.atlassian.net/browse/NOD-994)\] - Significantly increase the limit of logs kaspad keeps in ~/.kaspad/logs
* \[[NOD-995](https://daglabs.atlassian.net/browse/NOD-995)\] - Add isSpent to kasparov response
* \[[NOD-996](https://daglabs.atlassian.net/browse/NOD-996)\] - Disable kaspad logs in TestScripts
* \[[NOD-1006](https://daglabs.atlassian.net/browse/NOD-1006)\] - Reduce the amount of bigint allocations in kaspad
* \[[NOD-1008](https://daglabs.atlassian.net/browse/NOD-1008)\] - In utxoDiffStore, keep diffData in memory for blocks whose blueScore is at least virtualBlueScore - X
* \[[NOD-1015](https://daglabs.atlassian.net/browse/NOD-1015)\] - Fix cert location in devnet kasparov
* \[[NOD-1018](https://daglabs.atlassian.net/browse/NOD-1018)\] - Exit after 2 minutes if graceful shutdown fails
* \[[NOD-1022](https://daglabs.atlassian.net/browse/NOD-1022)\] - Make txgen check if transaction is unaccepted if by subscribing to unaccepted transactions topic

## 0.3.1 - 2020-04-30 testnet

### Bug

* \[[NOD-858](https://daglabs.atlassian.net/browse/NOD-858)\] - During netsync, blocks get rejected due to "already have block"
* \[[NOD-859](https://daglabs.atlassian.net/browse/NOD-859)\] - During netsync, requested blocks are orphaned
* \[[NOD-925](https://daglabs.atlassian.net/browse/NOD-925)\] - Faucet v0.3.0 fails to build
* \[[NOD-936](https://daglabs.atlassian.net/browse/NOD-936)\] - Kasparov queries are very slow
* \[[NOD-962](https://daglabs.atlassian.net/browse/NOD-962)\] - Testnet: hostname in datadog is public IP. Should be private

### Feature

* \[[NOD-587](https://daglabs.atlassian.net/browse/NOD-587)\] - Create VPC peering between testnet and main VPC + close SSH & JSON-RPC Ports for outside connections.
* \[[NOD-637](https://daglabs.atlassian.net/browse/NOD-637)\] - Create something similar to update\_hosts.py in testnet
* \[[NOD-686](https://daglabs.atlassian.net/browse/NOD-686)\] - Use DataDog to notify developers of rejected blocks in devnet/testnet
* \[[NOD-780](https://daglabs.atlassian.net/browse/NOD-780)\] - Make testnet scripts be aware of all regions
* \[[NOD-834](https://daglabs.atlassian.net/browse/NOD-834)\] - Add --no-build flag in ./manage.py
* \[[NOD-835](https://daglabs.atlassian.net/browse/NOD-835)\] - Add datadog tags to testnet
* \[[NOD-844](https://daglabs.atlassian.net/browse/NOD-844)\] - Add process and container monitoring in datadog for testnet
* \[[NOD-852](https://daglabs.atlassian.net/browse/NOD-852)\] - Add datadog tags to all servers in testnet
* \[[NOD-856](https://daglabs.atlassian.net/browse/NOD-856)\] - go-secp256k1: Replace \`String\(\)\` implementation of SchnorrPublicKey and MultiSet with a better one
* \[[NOD-898](https://daglabs.atlassian.net/browse/NOD-898)\] - Datadog monitor all critical devnets
* \[[NOD-902](https://daglabs.atlassian.net/browse/NOD-902)\] - Allow Etc/UTC to be used in kasparov
* \[[NOD-937](https://daglabs.atlassian.net/browse/NOD-937)\] - Use journald for docker log driver in testnet
* \[[NOD-940](https://daglabs.atlassian.net/browse/NOD-940)\] - Increase the server sizes in testnet

## 0.3.0 - 2020-04-02

### Bug

* \[[NOD-881](https://daglabs.atlassian.net/browse/NOD-881)\] - Deep reachability reindex takes a LOT of time
* \[[NOD-886](https://daglabs.atlassian.net/browse/NOD-886)\] - Faucet and cli-wallet compile breaks because of ecc/libsecp256k1 transition

### Sub-task

* \[[NOD-862](https://daglabs.atlassian.net/browse/NOD-862)\] - Replace calls to Tx.StoreBlock, Tx.HasBlock, Tx.FetchBlock with appropriate calls in dbaccess
* \[[NOD-865](https://daglabs.atlassian.net/browse/NOD-865)\] - Migrate misc database logic in the blockdag package to dbaccess
* \[[NOD-866](https://daglabs.atlassian.net/browse/NOD-866)\] - Migrate database logic in blockdag/indexers package to dbaccess
* \[[NOD-867](https://daglabs.atlassian.net/browse/NOD-867)\] - Migrate database logic in blockdag/dagio.go to dbaccess
* \[[NOD-868](https://daglabs.atlassian.net/browse/NOD-868)\] - Delete the old database package and rename database2 to database, fix tests, and go over all \`== nil\` and check they're not used to check existence in db

### Feature

* \[[NOD-519](https://daglabs.atlassian.net/browse/NOD-519)\] - Implement getMempoolEntry
* \[[NOD-805](https://daglabs.atlassian.net/browse/NOD-805)\] - Database refactoring
* \[[NOD-819](https://daglabs.atlassian.net/browse/NOD-819)\] - Fix error messages in Kasparov
* \[[NOD-828](https://daglabs.atlassian.net/browse/NOD-828)\] - Reimplement FFLDB
* \[[NOD-854](https://daglabs.atlassian.net/browse/NOD-854)\] - Add cronjob in jenkins server to do \`docker system prune\` every friday evening
* \[[NOD-857](https://daglabs.atlassian.net/browse/NOD-857)\] - Add profiler server to all remaining services
* \[[NOD-861](https://daglabs.atlassian.net/browse/NOD-861)\] - Get rid of dbtool/fetchblockregion.go
* \[[NOD-870](https://daglabs.atlassian.net/browse/NOD-870)\] - Update all ECC primitives to use go-libsecp256k1
* \[[NOD-874](https://daglabs.atlassian.net/browse/NOD-874)\] - Node stops requesting blocks when receiving invs for some reason
* \[[NOD-882](https://daglabs.atlassian.net/browse/NOD-882)\] - Remove ecc package + HDKeyIDPair, HDCoinType from dagconfig.Params
* \[[NOD-887](https://daglabs.atlassian.net/browse/NOD-887)\] - Add a couple of QoL features to Cursor
* \[[NOD-888](https://daglabs.atlassian.net/browse/NOD-888)\] - In database2, Add RollbackUnlessClosed to Context
* \[[NOD-889](https://daglabs.atlassian.net/browse/NOD-889)\] - In database2, instead of returning a boolean for not-found, return an error
* \[[NOD-900](https://daglabs.atlassian.net/browse/NOD-900)\] - Fix getBlocks returning weird results

## 0.2.0 - 2020-03-19

### Bug

* \[[NOD-832](https://daglabs.atlassian.net/browse/NOD-832)\] - Change log level for kasparov rpc client

### Sub-task

* \[[NOD-791](https://daglabs.atlassian.net/browse/NOD-791)\] - Define the Mempool component
* \[[NOD-792](https://daglabs.atlassian.net/browse/NOD-792)\] - Define the BlockDAG component

### Feature

* \[[NOD-537](https://daglabs.atlassian.net/browse/NOD-537)\] - Make datadog get a notification if kaspad/kasparov/etc. die
* \[[NOD-796](https://daglabs.atlassian.net/browse/NOD-796)\] - Update all dockerfiles to use go 1.14
* \[[NOD-818](https://daglabs.atlassian.net/browse/NOD-818)\] - Simplify timeSource in kaspad
* \[[NOD-839](https://daglabs.atlassian.net/browse/NOD-839)\] - panic from non-rule error from ProcessBlock
* \[[NOD-841](https://daglabs.atlassian.net/browse/NOD-841)\] - Fix tests to not be dependent on block rate
* \[[NOD-842](https://daglabs.atlassian.net/browse/NOD-842)\] - Use flushToDB with the same transaction as everything else in saveChangesFromBlock and never ignore flushToDB errors
* \[[NOD-843](https://daglabs.atlassian.net/browse/NOD-843)\] - Add monitoring of dockers/processes to datadog
* \[[NOD-846](https://daglabs.atlassian.net/browse/NOD-846)\] - Setup logs in datadog + store log archives in S3
* \[[NOD-848](https://daglabs.atlassian.net/browse/NOD-848)\] - optimize allocations when serializing UTXO diffs
* \[[NOD-853](https://daglabs.atlassian.net/browse/NOD-853)\] - Add profiler to kaspaminer
* \[[NOD-855](https://daglabs.atlassian.net/browse/NOD-855)\] - Remove diffMultiset from UTXODiffs
* \[[NOD-876](https://daglabs.atlassian.net/browse/NOD-876)\] - Integrate go-libsecp256k1 into kaspad
* \[[NOD-880](https://daglabs.atlassian.net/browse/NOD-880)\] - Remove CGO\_ENABLED=0 from Dockerfiles
* \[[NOD-883](https://daglabs.atlassian.net/browse/NOD-883)\] - Fix dockerfile in kaspaminer
* \[[NOD-884](https://daglabs.atlassian.net/browse/NOD-884)\] - Integrate go-libsecp256k1 into cli-wallet and txgen

## 0.1.2 - 2020-02-29

### Bug

* \[[NOD-647](https://daglabs.atlassian.net/browse/NOD-647)\] - Error creating default config file
* \[[NOD-661](https://daglabs.atlassian.net/browse/NOD-661)\] - Change BCDB subsystem tag \(for logs\) to KSDB
* \[[NOD-685](https://daglabs.atlassian.net/browse/NOD-685)\] - Kasparovsyncd: "missing block for hash" in long sync
* \[[NOD-694](https://daglabs.atlassian.net/browse/NOD-694)\] - During netsync, tip blocks from other nodes are still requested
* \[[NOD-717](https://daglabs.atlassian.net/browse/NOD-717)\] - Nodes get stuck in an infinite loop in addrManager.getAddress
* \[[NOD-728](https://daglabs.atlassian.net/browse/NOD-728)\] - Check with github if branch is up to date before running manage.py
* \[[NOD-732](https://daglabs.atlassian.net/browse/NOD-732)\] - ./manage.py upgrade doesn't work
* \[[NOD-746](https://daglabs.atlassian.net/browse/NOD-746)\] - CLI-Wallet: When sending transaction and address contains trailing slashes - fails
* \[[NOD-765](https://daglabs.atlassian.net/browse/NOD-765)\] - Database corruption after restart in reachabilitystore and utxodiffstore
* \[[NOD-769](https://daglabs.atlassian.net/browse/NOD-769)\] - Add a log for when a reachability reindex occurs
* \[[NOD-772](https://daglabs.atlassian.net/browse/NOD-772)\] - Node started mining on genesis instead of syncing
* \[[NOD-773](https://daglabs.atlassian.net/browse/NOD-773)\] - Replace mysql with postgres in kasparov
* \[[NOD-781](https://daglabs.atlassian.net/browse/NOD-781)\] - Kasparov: ParentBlockHashes are all the same
* \[[NOD-782](https://daglabs.atlassian.net/browse/NOD-782)\] - errors.As is expecting a parameter that implements error interface bug
* \[[NOD-787](https://daglabs.atlassian.net/browse/NOD-787)\] - Kasparov: same txs appear multiple times when using the GET:"transactions" request
* \[[NOD-798](https://daglabs.atlassian.net/browse/NOD-798)\] - In Devnet, all nodes stop sending invs to one another on block acceptance
* \[[NOD-814](https://daglabs.atlassian.net/browse/NOD-814)\] - Migrate faucet to postgres sql
* \[[NOD-829](https://daglabs.atlassian.net/browse/NOD-829)\] - Error preparing --no-suffix devnet

### Feature

* \[[NOD-467](https://daglabs.atlassian.net/browse/NOD-467)\] - Refactor Kasparov to have proper data-access layer
* \[[NOD-544](https://daglabs.atlassian.net/browse/NOD-544)\] - request id is not incremented in kasparov
* \[[NOD-545](https://daglabs.atlassian.net/browse/NOD-545)\] - Remove headers-first-related messages from the wire package.
* \[[NOD-552](https://daglabs.atlassian.net/browse/NOD-552)\] - Use default ports active net params for kasparov, txgen and miningsimulator in devnet and testnet devops
* \[[NOD-558](https://daglabs.atlassian.net/browse/NOD-558)\] - Change dnsseeder cli "--defaul seeder" to single word
* \[[NOD-571](https://daglabs.atlassian.net/browse/NOD-571)\] - Reach 100% unit-test coverage on GhostDAG related code
* \[[NOD-576](https://daglabs.atlassian.net/browse/NOD-576)\] - Rename NextHashes to ChildHashes in GetBlock/GetBlockHeaders rpc call + Fix TODO near their generation
* \[[NOD-584](https://daglabs.atlassian.net/browse/NOD-584)\] - In devnet deployment add --no-prepare flag
* \[[NOD-586](https://daglabs.atlassian.net/browse/NOD-586)\] - Replace the subtreeSize member in reachabilityTreeNode
* \[[NOD-596](https://daglabs.atlassian.net/browse/NOD-596)\] - Add Datadog to devnet + Set hostnames in datadog to something meaningful
* \[[NOD-597](https://daglabs.atlassian.net/browse/NOD-597)\] - Make BlockIndex clear its dirty entries only after it successfully written them to disk
* \[[NOD-603](https://daglabs.atlassian.net/browse/NOD-603)\] - Update validateParents to use reachability
* \[[NOD-605](https://daglabs.atlassian.net/browse/NOD-605)\] - Add multiregion ELB to testnet
* \[[NOD-610](https://daglabs.atlassian.net/browse/NOD-610)\] - Rename newSet-&gt;newBlockSet and setFromSlice-&gt;blockSetFromSlice
* \[[NOD-611](https://daglabs.atlassian.net/browse/NOD-611)\] - Make testnet and devnet delete parallel for all stacks
* \[[NOD-615](https://daglabs.atlassian.net/browse/NOD-615)\] - Make bluesAnticoneSizes a map with \*blockNode as a key
* \[[NOD-622](https://daglabs.atlassian.net/browse/NOD-622)\] - Fix NewBlockTemplate to show txFees and txMasses in the right order
* \[[NOD-641](https://daglabs.atlassian.net/browse/NOD-641)\] - Use new error constructs
* \[[NOD-648](https://daglabs.atlassian.net/browse/NOD-648)\] - Write tests for delayed blocks
* \[[NOD-656](https://daglabs.atlassian.net/browse/NOD-656)\] - log hashrate in kaspaminer
* \[[NOD-658](https://daglabs.atlassian.net/browse/NOD-658)\] - Add nightly builds to all master + v\*.\*.\*-dev branches that got any updates during the day
* \[[NOD-664](https://daglabs.atlassian.net/browse/NOD-664)\] - Remove version from everything inside kaspad/cmd - use kaspad version instead
* \[[NOD-670](https://daglabs.atlassian.net/browse/NOD-670)\] - Add -default suffix to devnet without suffix
* \[[NOD-671](https://daglabs.atlassian.net/browse/NOD-671)\] - Make txgen work with kasparov
* \[[NOD-674](https://daglabs.atlassian.net/browse/NOD-674)\] - Add log "Listening on XXX:port" to kasparov on startup
* \[[NOD-679](https://daglabs.atlassian.net/browse/NOD-679)\] - Allow multiple transactions with same ID but different hash in kasparov
* \[[NOD-687](https://daglabs.atlassian.net/browse/NOD-687)\] - Remove -gcflags='-l' from everywhere
* \[[NOD-693](https://daglabs.atlassian.net/browse/NOD-693)\] - Update link to ISC license in all READMEs
* \[[NOD-705](https://daglabs.atlassian.net/browse/NOD-705)\] - Add count to each paginated result in kasparov
* \[[NOD-706](https://daglabs.atlassian.net/browse/NOD-706)\] - Remove periods \(.\) from the end of all error messages in kasparov
* \[[NOD-709](https://daglabs.atlassian.net/browse/NOD-709)\] - Rename api server to kasparovd in faucet
* \[[NOD-710](https://daglabs.atlassian.net/browse/NOD-710)\] - Export Kasparov MQTT topics
* \[[NOD-713](https://daglabs.atlassian.net/browse/NOD-713)\] - Make jenkins get kaspad and kasparov version from go.mod
* \[[NOD-715](https://daglabs.atlassian.net/browse/NOD-715)\] - Replace testDbRoot with os.TempDir\(\)
* \[[NOD-721](https://daglabs.atlassian.net/browse/NOD-721)\] - Go over all places that need a defer - and add it
* \[[NOD-722](https://daglabs.atlassian.net/browse/NOD-722)\] - Cleanup in SyncManager.blockHandler\(\)
* \[[NOD-723](https://daglabs.atlassian.net/browse/NOD-723)\] - Kasparov: If the database is not configured to UTC timezone - fail to start
* \[[NOD-725](https://daglabs.atlassian.net/browse/NOD-725)\] - Enlarge logrotator to 10 x 50MB chunks in testnet and devnet
* \[[NOD-726](https://daglabs.atlassian.net/browse/NOD-726)\] - Do something about nagging message of "no sync peer"
* \[[NOD-727](https://daglabs.atlassian.net/browse/NOD-727)\] - Don't add delayed blocks to delayed pool if sent from RPC
* \[[NOD-731](https://daglabs.atlassian.net/browse/NOD-731)\] - cpulimit from sudo and wait 15 seconds before limiting the cpu
* \[[NOD-733](https://daglabs.atlassian.net/browse/NOD-733)\] - add profiler to kasparov
* \[[NOD-734](https://daglabs.atlassian.net/browse/NOD-734)\] - Add parentBlockHash to all kasparov calls that return blocks
* \[[NOD-737](https://daglabs.atlassian.net/browse/NOD-737)\] - remove "btc" and "bitcoin from file names
* \[[NOD-738](https://daglabs.atlassian.net/browse/NOD-738)\] - Move rpcmodel.Uint64 etc to somewhere in util
* \[[NOD-740](https://daglabs.atlassian.net/browse/NOD-740)\] - Map-out all threads in kaspad
* \[[NOD-742](https://daglabs.atlassian.net/browse/NOD-742)\] - Look into connection and address management
* \[[NOD-744](https://daglabs.atlassian.net/browse/NOD-744)\] - make sure all goroutines are wrapped with spawns
* \[[NOD-748](https://daglabs.atlassian.net/browse/NOD-748)\] - Upgrade devnet and testnet machines that run kaspad to t3a.medium
* \[[NOD-752](https://daglabs.atlassian.net/browse/NOD-752)\] - Add test for kasparov dbmodels field names
* \[[NOD-753](https://daglabs.atlassian.net/browse/NOD-753)\] - Assign Datadog tags to devnet
* \[[NOD-754](https://daglabs.atlassian.net/browse/NOD-754)\] - Fix statickcheck errors
* \[[NOD-757](https://daglabs.atlassian.net/browse/NOD-757)\] - Readd addrmanager tests
* \[[NOD-758](https://daglabs.atlassian.net/browse/NOD-758)\] - Kasparov: Add number of confirmations to block response
* \[[NOD-759](https://daglabs.atlassian.net/browse/NOD-759)\] - Merge 0.1.1-dev into 0.1.2-dev
* \[[NOD-766](https://daglabs.atlassian.net/browse/NOD-766)\] - publish transactions to mqtt in kasparov while initial sync
* \[[NOD-778](https://daglabs.atlassian.net/browse/NOD-778)\] - Optimize RestoreUTXO
* \[[NOD-783](https://daglabs.atlassian.net/browse/NOD-783)\] - Refactor Jenkins jobs: extract inline scripts to files.
* \[[NOD-784](https://daglabs.atlassian.net/browse/NOD-784)\] - devnet: Confirm deletion of stacks before actually deleting them
* \[[NOD-785](https://daglabs.atlassian.net/browse/NOD-785)\] - Add exclude list to nightly builds
* \[[NOD-786](https://daglabs.atlassian.net/browse/NOD-786)\] - add chain changed notifications for kasparov MQTT
* \[[NOD-802](https://daglabs.atlassian.net/browse/NOD-802)\] - Understand why are we running out of memory
* \[[NOD-806](https://daglabs.atlassian.net/browse/NOD-806)\] - after panic, gracefully stop logs, and then exit asap
* \[[NOD-808](https://daglabs.atlassian.net/browse/NOD-808)\] - Use the latest version of leveldb instead of btcsuite/goleveldb.
* \[[NOD-810](https://daglabs.atlassian.net/browse/NOD-810)\] - Fix error text in lookupParentNodes
* \[[NOD-816](https://daglabs.atlassian.net/browse/NOD-816)\] - Remove txindex from kaspad
* \[[NOD-817](https://daglabs.atlassian.net/browse/NOD-817)\] - Remove addrindex from kaspad
* \[[NOD-823](https://daglabs.atlassian.net/browse/NOD-823)\] - Re-implement WithDiff using WithDiffInPlace
* \[[NOD-826](https://daglabs.atlassian.net/browse/NOD-826)\] - Update devnet+testnet with Postgres
* \[[NOD-827](https://daglabs.atlassian.net/browse/NOD-827)\] - Get rid of dbtools insecureimport.go and loadheaders.go
* \[[NOD-838](https://daglabs.atlassian.net/browse/NOD-838)\] - Experiment with simpler reachability algorithm

## 0.1.1 - 2020-01-31

### Bug

* \[[NOD-476](https://daglabs.atlassian.net/browse/NOD-476)\] - When DNSSeeder is suspended and then resumed - it has no more addresses, and never updates
* \[[NOD-629](https://daglabs.atlassian.net/browse/NOD-629)\] - Change GHOSTDAG K to a single byte \(uint8\)
* \[[NOD-634](https://daglabs.atlassian.net/browse/NOD-634)\] - In testnet deply, in case of multiple regions, make sure that all dnsseeders start first, then all kaspads, etc...
* \[[NOD-636](https://daglabs.atlassian.net/browse/NOD-636)\] - Database corruption when failed to start kaspad
* \[[NOD-646](https://daglabs.atlassian.net/browse/NOD-646)\] - Database corruption when starting a node
* \[[NOD-653](https://daglabs.atlassian.net/browse/NOD-653)\] - New node is stuck on requesting orphans
* \[[NOD-660](https://daglabs.atlassian.net/browse/NOD-660)\] - Kasparov doesn't notify all accepted transactions
* \[[NOD-668](https://daglabs.atlassian.net/browse/NOD-668)\] - Fix TxGen to use blueScore in OnFilteredBlockAdded
* \[[NOD-669](https://daglabs.atlassian.net/browse/NOD-669)\] - stopNode in getBlueBlocksBetween arrives to genesis
* \[[NOD-680](https://daglabs.atlassian.net/browse/NOD-680)\] - Duplicate transaction in kasparovsyncd
* \[[NOD-682](https://daglabs.atlassian.net/browse/NOD-682)\] - GORM's Preload causes "Prepared statement contains too many placeholders"
* \[[NOD-691](https://daglabs.atlassian.net/browse/NOD-691)\] - Nodes that disconnect don't try to reconnect to anything
* \[[NOD-697](https://daglabs.atlassian.net/browse/NOD-697)\] - In blockLocator, return an error if lowHash blueScore &gt;= highHash blueScore
* \[[NOD-701](https://daglabs.atlassian.net/browse/NOD-701)\] - Add Kaspad version to docker images on devnet
* \[[NOD-702](https://daglabs.atlassian.net/browse/NOD-702)\] - Netsync slows down significantly due to excessive allocs in serializeUTXO
* \[[NOD-703](https://daglabs.atlassian.net/browse/NOD-703)\] - Create script to download logs and data from devnet servers
* \[[NOD-704](https://daglabs.atlassian.net/browse/NOD-704)\] - GetChainFromBlock fails when passed hash is not a chain block
* \[[NOD-716](https://daglabs.atlassian.net/browse/NOD-716)\] - Crash in GetTopHeaders

### Sub-task

* \[[NOD-561](https://daglabs.atlassian.net/browse/NOD-561)\] - Write a Discord bot that copies build failures from Discord and posts them on Telegram
* \[[NOD-665](https://daglabs.atlassian.net/browse/NOD-665)\] - Initialize blockNode blueScore to be MaxUint64 by default

### Feature

* \[[NOD-407](https://daglabs.atlassian.net/browse/NOD-407)\] - Run devnet with many nodes + network throtteling
* \[[NOD-420](https://daglabs.atlassian.net/browse/NOD-420)\] - Check \(and potentially fix\) what happens if new block with good timestamp points to a delayed block
* \[[NOD-504](https://daglabs.atlassian.net/browse/NOD-504)\] - Test --onion flag
* \[[NOD-528](https://daglabs.atlassian.net/browse/NOD-528)\] - Use bulk-insert whenever inserting multiple rows in kasparovsync
* \[[NOD-542](https://daglabs.atlassian.net/browse/NOD-542)\] - Don't require redundant fields when migrating kasparov
* \[[NOD-550](https://daglabs.atlassian.net/browse/NOD-550)\] - Prepare docker-compose file that runs kaspad+kasparov+mysql+rabbit locally
* \[[NOD-554](https://daglabs.atlassian.net/browse/NOD-554)\] - Update \`./manage.py upgrade\` + \`./testnet deploy \(when testnet already exists\)\`to also pull docker-compose.yaml and all other files
* \[[NOD-555](https://daglabs.atlassian.net/browse/NOD-555)\] - Add Jenkins builds to all side-repos
* \[[NOD-575](https://daglabs.atlassian.net/browse/NOD-575)\] - Change devnet address prefix to kaspadev
* \[[NOD-585](https://daglabs.atlassian.net/browse/NOD-585)\] - Move \`ARG KASPAD\_VERSION\` in all docker as late as possible, for faster partial builds
* \[[NOD-616](https://daglabs.atlassian.net/browse/NOD-616)\] - Go over all usages of ChainHeight, and see if we can do without it
* \[[NOD-623](https://daglabs.atlassian.net/browse/NOD-623)\] - Recreate dnsseeder repository to not be a decred fork
* \[[NOD-633](https://daglabs.atlassian.net/browse/NOD-633)\] - Update compilation of pull requests to inject the correct KASPAD\_VERSION and KASPAROV\_VERSION
* \[[NOD-640](https://daglabs.atlassian.net/browse/NOD-640)\] - Revamp netsync
* \[[NOD-644](https://daglabs.atlassian.net/browse/NOD-644)\] - Jenkins: when kaspad or kasparov "master" changes, trigger build of all dependant repositories
* \[[NOD-650](https://daglabs.atlassian.net/browse/NOD-650)\] - Remove --generate from full node. Move mining\_simulator into /cmd
* \[[NOD-652](https://daglabs.atlassian.net/browse/NOD-652)\] - Properly handle the blocks that are being mined while netsync works
* \[[NOD-659](https://daglabs.atlassian.net/browse/NOD-659)\] - Restore all tests removed because of monkey patching.
* \[[NOD-675](https://daglabs.atlassian.net/browse/NOD-675)\] - Replace "startHash" and "stopHash" with more meaningful name everywhere in the code
* \[[NOD-676](https://daglabs.atlassian.net/browse/NOD-676)\] - In blockLocator, get rid of defaultValues
* \[[NOD-683](https://daglabs.atlassian.net/browse/NOD-683)\] - Readd block delay of 5 seconds to devnet
* \[[NOD-692](https://daglabs.atlassian.net/browse/NOD-692)\] - fix thresholdsState to use blue score
* \[[NOD-696](https://daglabs.atlassian.net/browse/NOD-696)\] - handle panics on time.AfterFunc
* \[[NOD-698](https://daglabs.atlassian.net/browse/NOD-698)\] - Change confirmations to be selectedTip.blueScore-acceptingBlock.blueScore+1
* \[[NOD-700](https://daglabs.atlassian.net/browse/NOD-700)\] - Update blockSet to be map\[\*blockNode\]struct{}
* \[[NOD-719](https://daglabs.atlassian.net/browse/NOD-719)\] - Deadlock somewhere
* \[[NOD-747](https://daglabs.atlassian.net/browse/NOD-747)\] - Change FinalityInterval to 24 hours and IsCurrent to be 12 hours

## 0.1.0 - 2020-01-13

### Bug

* \[[NOD-477](https://daglabs.atlassian.net/browse/NOD-477)\] - API server "cannot de-spend an unspent transaction output"
* \[[NOD-521](https://daglabs.atlassian.net/browse/NOD-521)\] - API Server crashes with "out of memory"
* \[[NOD-556](https://daglabs.atlassian.net/browse/NOD-556)\] - Duplicate entry in idx\_addresses\_address in kasparov
* \[[NOD-566](https://daglabs.atlassian.net/browse/NOD-566)\] - Coinbase in selected parent anticone are marked as accepted transactions
* \[[NOD-568](https://daglabs.atlassian.net/browse/NOD-568)\] - API-Server: incorrect response when using request "transactions/address/{address}" with an invalid address
* \[[NOD-569](https://daglabs.atlassian.net/browse/NOD-569)\] - API-Server: incorrect error message is received when using request "BLOCKS" with "Limit" &lt; 0 or L"Limit" &gt; 100
* \[[NOD-583](https://daglabs.atlassian.net/browse/NOD-583)\] - Processes stuck after panic
* \[[NOD-604](https://daglabs.atlassian.net/browse/NOD-604)\] - Hosts in testnet don't know their local addresses
* \[[NOD-613](https://daglabs.atlassian.net/browse/NOD-613)\] - Kaspad with --generate crashes due to concurrent map access
* \[[NOD-635](https://daglabs.atlassian.net/browse/NOD-635)\] - Testnet genesis difficulty and default difficulty are different
* \[[NOD-638](https://daglabs.atlassian.net/browse/NOD-638)\] - Kasparovsyncd fails to start because of non-standard script
* \[[NOD-639](https://daglabs.atlassian.net/browse/NOD-639)\] - In the Kasparov DB, make it possible to have address-less txs

### Sub-task

* \[[NOD-494](https://daglabs.atlassian.net/browse/NOD-494)\] - Update all READMEs
* \[[NOD-495](https://daglabs.atlassian.net/browse/NOD-495)\] - Separate non-btcd applications to their own repositories
* \[[NOD-522](https://daglabs.atlassian.net/browse/NOD-522)\] - Add mqtt to testnet stack
* \[[NOD-540](https://daglabs.atlassian.net/browse/NOD-540)\] - Implement reachability
* \[[NOD-541](https://daglabs.atlassian.net/browse/NOD-541)\] - Implement GhostDAG
* \[[NOD-564](https://daglabs.atlassian.net/browse/NOD-564)\] - Temporarily disable --onion flag

### Epic

* \[[NOD-281](https://daglabs.atlassian.net/browse/NOD-281)\] - API-Server
* \[[NOD-343](https://daglabs.atlassian.net/browse/NOD-343)\] - Testnet DevOps

### Feature

* \[[NOD-5](https://daglabs.atlassian.net/browse/NOD-5)\] - Remove TestFullBlocks and package fullblocktests
* \[[NOD-390](https://daglabs.atlassian.net/browse/NOD-390)\] - Add faucet to testnet stack
* \[[NOD-401](https://daglabs.atlassian.net/browse/NOD-401)\] - Create CLI wallet
* \[[NOD-451](https://daglabs.atlassian.net/browse/NOD-451)\] - Add log rotator to all docker-composes
* \[[NOD-497](https://daglabs.atlassian.net/browse/NOD-497)\] - Fully implement GhostDAG
* \[[NOD-499](https://daglabs.atlassian.net/browse/NOD-499)\] - Change ports and network magics
* \[[NOD-500](https://daglabs.atlassian.net/browse/NOD-500)\] - Remove any references to checkpoints and cmd/findcheckpoint
* \[[NOD-502](https://daglabs.atlassian.net/browse/NOD-502)\] - Remove --rpcquirks flag and support
* \[[NOD-503](https://daglabs.atlassian.net/browse/NOD-503)\] - Remove --onion and related functionality, make sure --proxy works
* \[[NOD-510](https://daglabs.atlassian.net/browse/NOD-510)\] - Change all references to Bitcoin to Kaspa
* \[[NOD-514](https://daglabs.atlassian.net/browse/NOD-514)\] - Update all dagcoin address prefixes, as well as anything else referring to dagcoin to kaspa
* \[[NOD-516](https://daglabs.atlassian.net/browse/NOD-516)\] - Move to stuff \(or delete\) cmd/genesis
* \[[NOD-517](https://daglabs.atlassian.net/browse/NOD-517)\] - Update doc.go files
* \[[NOD-518](https://daglabs.atlassian.net/browse/NOD-518)\] - Test graceful shutdown on Windows.
* \[[NOD-525](https://daglabs.atlassian.net/browse/NOD-525)\] - Rename kasparov sub-apps to kasparovserver and kasparovsync accordingly
* \[[NOD-527](https://daglabs.atlassian.net/browse/NOD-527)\] - Separate Kasparov syncd and server stacks on devnet
* \[[NOD-529](https://daglabs.atlassian.net/browse/NOD-529)\] - Update license files in all packages to properly credit btcd and kaspa developers
* \[[NOD-532](https://daglabs.atlassian.net/browse/NOD-532)\] - Remove redundant useage of the word "chain"
* \[[NOD-538](https://daglabs.atlassian.net/browse/NOD-538)\] - Use anton's statsd tool to write same stuff to datadog
* \[[NOD-543](https://daglabs.atlassian.net/browse/NOD-543)\] - Update testnet domains to kas.pa
* \[[NOD-546](https://daglabs.atlassian.net/browse/NOD-546)\] - Post build failures to Discord instead of Telegram
* \[[NOD-547](https://daglabs.atlassian.net/browse/NOD-547)\] - Fix testnet upgrade
* \[[NOD-548](https://daglabs.atlassian.net/browse/NOD-548)\] - Remove default dns seed from devnet
* \[[NOD-549](https://daglabs.atlassian.net/browse/NOD-549)\] - Add something similar to version.go to all apps + version command for all cli tools + update version in kaspad
* \[[NOD-551](https://daglabs.atlassian.net/browse/NOD-551)\] - Move httpserverutils to kasparov repository
* \[[NOD-559](https://daglabs.atlassian.net/browse/NOD-559)\] - Move genaddr to stuff
* \[[NOD-567](https://daglabs.atlassian.net/browse/NOD-567)\] - Add more logs to txgen
* \[[NOD-570](https://daglabs.atlassian.net/browse/NOD-570)\] - Separate genesis of devnet, testnet, etc. Write something in all of them, according to whatever you want
* \[[NOD-580](https://daglabs.atlassian.net/browse/NOD-580)\] - Change p2p ports to 16611 in testnet and devops
* \[[NOD-582](https://daglabs.atlassian.net/browse/NOD-582)\] - Update testnet with all recent changes + kstats
* \[[NOD-590](https://daglabs.atlassian.net/browse/NOD-590)\] - Export NewLogClosure
* \[[NOD-591](https://daglabs.atlassian.net/browse/NOD-591)\] - Add selected parent to GeBlockVerboseResult
* \[[NOD-593](https://daglabs.atlassian.net/browse/NOD-593)\] - Add README to kasparov repo
* \[[NOD-594](https://daglabs.atlassian.net/browse/NOD-594)\] - Add --generate to testnet
* \[[NOD-595](https://daglabs.atlassian.net/browse/NOD-595)\] - Update testnet build scripts in Jenkins
* \[[NOD-599](https://daglabs.atlassian.net/browse/NOD-599)\] - Add README to faucet repo
* \[[NOD-600](https://daglabs.atlassian.net/browse/NOD-600)\] - Add some intro explanation to dnsseeder repo's README
* \[[NOD-601](https://daglabs.atlassian.net/browse/NOD-601)\] - omit nil selected parent in GetBlockVerboseResult
* \[[NOD-602](https://daglabs.atlassian.net/browse/NOD-602)\] - Update certificates in testnet
* \[[NOD-608](https://daglabs.atlassian.net/browse/NOD-608)\] - Update useragent to "/kaspad:{version}/"
* \[[NOD-612](https://daglabs.atlassian.net/browse/NOD-612)\] - Make TestDifficulty take less time
* \[[NOD-617](https://daglabs.atlassian.net/browse/NOD-617)\] - Remove message saying that test.sh failed due to incomplete coverage
* \[[NOD-618](https://daglabs.atlassian.net/browse/NOD-618)\] - Delete kaspad/docs folder
* \[[NOD-619](https://daglabs.atlassian.net/browse/NOD-619)\] - Disable any tor-related cli flags
* \[[NOD-628](https://daglabs.atlassian.net/browse/NOD-628)\] - Fix side-repositories after change of DevNet -&gt; Devnet
* \[[NOD-643](https://daglabs.atlassian.net/browse/NOD-643)\] - Remove all tests that use monkey patching

## 0.0.3 - 2019-12-11

### Bug

* \[[NOD-450](https://daglabs.atlassian.net/browse/NOD-450)\] - Troubles in netsync
* \[[NOD-461](https://daglabs.atlassian.net/browse/NOD-461)\] - API-Server: request GET:transactions/address/ - wrong error code and message received when limit set to a value higher than 1000 or lower than 0
* \[[NOD-464](https://daglabs.atlassian.net/browse/NOD-464)\] - API-Server: request GET:blocks - wrong error received when submitting the request with key "order" empty
* \[[NOD-470](https://daglabs.atlassian.net/browse/NOD-470)\] - Fix unaccepted transactions notification SQL error
* \[[NOD-478](https://daglabs.atlassian.net/browse/NOD-478)\] - Deadlock between peerHandler and connHandler
* \[[NOD-481](https://daglabs.atlassian.net/browse/NOD-481)\] - Fix API server get blocks error message
* \[[NOD-483](https://daglabs.atlassian.net/browse/NOD-483)\] - API-Server: incorrect response when using request "UTXOS" with an invalid address \(delete the "dagtest" at the beginning\)
* \[[NOD-484](https://daglabs.atlassian.net/browse/NOD-484)\] - System stuck during graceful-shutdown
* \[[NOD-486](https://daglabs.atlassian.net/browse/NOD-486)\] - API-Server: Request POST: /transaction returns status:500, Internal server error for all requests
* \[[NOD-488](https://daglabs.atlassian.net/browse/NOD-488)\] - Netsync causes crash in case of re-org
* \[[NOD-489](https://daglabs.atlassian.net/browse/NOD-489)\] - TxPool.HandleNewBlock gets stuck if no need to relay block
* \[[NOD-493](https://daglabs.atlassian.net/browse/NOD-493)\] - GetBlock returns known invalid blocks

### Feature

* \[[NOD-340](https://daglabs.atlassian.net/browse/NOD-340)\] - Implement necessary rpc commands and delete unnecessary
* \[[NOD-410](https://daglabs.atlassian.net/browse/NOD-410)\] - Add log level CLI argument to API server
* \[[NOD-412](https://daglabs.atlassian.net/browse/NOD-412)\] - Create rpc.cert for TestNet only
* \[[NOD-417](https://daglabs.atlassian.net/browse/NOD-417)\] - Remove wire.MsgMempool and wire.MsgAlert and CmdMemPool
* \[[NOD-428](https://daglabs.atlassian.net/browse/NOD-428)\] - Check why btcctl needs ~/.btcd/btcd.conf and try to remove the need
* \[[NOD-443](https://daglabs.atlassian.net/browse/NOD-443)\] - API-Server should be able to recover from restart of it's BTCD node
* \[[NOD-444](https://daglabs.atlassian.net/browse/NOD-444)\] - API-Server's btcd node disconnects after around 1 hour
* \[[NOD-472](https://daglabs.atlassian.net/browse/NOD-472)\] - Don't fetch accepting block and tx confirmations for getBlocks
* \[[NOD-474](https://daglabs.atlassian.net/browse/NOD-474)\] - ./manage.py suspend corrupts databases
* \[[NOD-475](https://daglabs.atlassian.net/browse/NOD-475)\] - When requesting 0 as limit in API-Server - return error
* \[[NOD-479](https://daglabs.atlassian.net/browse/NOD-479)\] - Make target outbound connections configurable
* \[[NOD-485](https://daglabs.atlassian.net/browse/NOD-485)\] - Remove first argument from saveChangesFromBlock
* \[[NOD-487](https://daglabs.atlassian.net/browse/NOD-487)\] - Close database properly on panic
* \[[NOD-490](https://daglabs.atlassian.net/browse/NOD-490)\] - Fix unorphaning in API server initial sync
* \[[NOD-491](https://daglabs.atlassian.net/browse/NOD-491)\] - Withold blocks in mining simulator
* \[[NOD-492](https://daglabs.atlassian.net/browse/NOD-492)\] - Split API-Server to syncer and frontend applications
* \[[NOD-498](https://daglabs.atlassian.net/browse/NOD-498)\] - Disable mainnet
* \[[NOD-506](https://daglabs.atlassian.net/browse/NOD-506)\] - Remove blockNode.height and any references to it
* \[[NOD-509](https://daglabs.atlassian.net/browse/NOD-509)\] - Rename github repository to kaspad

## 0.0.2 - 2019-11-27

### Bug

* \[[NOD-434](https://daglabs.atlassian.net/browse/NOD-434)\] - If parents are missing when adding a block - API-Server enters an unrecoverable state
* \[[NOD-437](https://daglabs.atlassian.net/browse/NOD-437)\] - Hardly any logs in API-Server
* \[[NOD-442](https://daglabs.atlassian.net/browse/NOD-442)\] - Cannot getRawTransaction with verbose if it's included in a red tip
* \[[NOD-445](https://daglabs.atlassian.net/browse/NOD-445)\] - AWS occasionally kicks out btcds in devnet for not reason at all
* \[[NOD-448](https://daglabs.atlassian.net/browse/NOD-448)\] - In APIServer, in syncBlocks, blocks rawBlocks and get misaligned
* \[[NOD-455](https://daglabs.atlassian.net/browse/NOD-455)\] - API-Server: ErrorCode: 500 is received when using "GET: fee-estimates"
* \[[NOD-457](https://daglabs.atlassian.net/browse/NOD-457)\] - API-Server: incorrect error message is received when using request "BLOCKS" with "Limit" set to 101
* \[[NOD-460](https://daglabs.atlassian.net/browse/NOD-460)\] - API-Server: request GET:transactions/address/ - using the "limit" key, also affects the "skip" functionality
* \[[NOD-462](https://daglabs.atlassian.net/browse/NOD-462)\] - API-Server: incorrect response when using request "UTXOS" with an invalid address when missing several chars
* \[[NOD-463](https://daglabs.atlassian.net/browse/NOD-463)\] - API-Server: request GET:blocks - Default value to parameter "order" is "asc" instead of "desc"
* \[[NOD-466](https://daglabs.atlassian.net/browse/NOD-466)\] - getTransactionsByAddressHandler skip parameter does not work

### Feature

* \[[NOD-381](https://daglabs.atlassian.net/browse/NOD-381)\] - Send notifications to \`/transactions/{address}\`
* \[[NOD-382](https://daglabs.atlassian.net/browse/NOD-382)\] - Send notification to \`transactions/accepted/{address}\`
* \[[NOD-409](https://daglabs.atlassian.net/browse/NOD-409)\] - Add RabbitMQ to Devnet
* \[[NOD-423](https://daglabs.atlassian.net/browse/NOD-423)\] - Implement GetSelectedTip
* \[[NOD-426](https://daglabs.atlassian.net/browse/NOD-426)\] - Send notifications to \`transactions/unaccepted/{address}\`
* \[[NOD-427](https://daglabs.atlassian.net/browse/NOD-427)\] - Send notifications to \`dag/selected-tip\`
* \[[NOD-438](https://daglabs.atlassian.net/browse/NOD-438)\] - In api server, change block\_data to MEDIUMBLOB
* \[[NOD-446](https://daglabs.atlassian.net/browse/NOD-446)\] - Add endpoint for API-Server in devnet
* \[[NOD-447](https://daglabs.atlassian.net/browse/NOD-447)\] - Fix deadlocks and hanging goroutines

## 0.0.1 2019-11-19 devnet

### Bug

* \[[NOD-356](https://daglabs.atlassian.net/browse/NOD-356)\] - In JSON-RPC: Upon submitting an Orphan block, there is no option of verifying that the block is an Orphan, no indication is received during submit.
* \[[NOD-368](https://daglabs.atlassian.net/browse/NOD-368)\] - When using "getAddr" the wrong name is provided for the 3rd variable
* \[[NOD-376](https://daglabs.atlassian.net/browse/NOD-376)\] - Error received when submitting a block to resolve an orphan block \(see description for more information\)
* \[[NOD-395](https://daglabs.atlassian.net/browse/NOD-395)\] - BTCD crashes with "cannot accept outpoint &lt;outpoint&gt; because it doesn't exist in the given UTXO
* \[[NOD-424](https://daglabs.atlassian.net/browse/NOD-424)\] - API Server doesn't sync new blocks
* \[[NOD-433](https://daglabs.atlassian.net/browse/NOD-433)\] - getBlocks rpc call is still sometimes very slow
* \[[NOD-435](https://daglabs.atlassian.net/browse/NOD-435)\] - API-Server DB connection spews "Too many connections"
* \[[NOD-436](https://daglabs.atlassian.net/browse/NOD-436)\] - API-Server sometimes logs "Chain changed: removed 0 blocks and added 0 block"

### Feature

* \[[NOD-379](https://daglabs.atlassian.net/browse/NOD-379)\] - Check why mempool is not emptied when tx propagation delay is high
* \[[NOD-380](https://daglabs.atlassian.net/browse/NOD-380)\] - Implement MQTT client in api-server
* \[[NOD-402](https://daglabs.atlassian.net/browse/NOD-402)\] - Add API-Server to devnet
* \[[NOD-430](https://daglabs.atlassian.net/browse/NOD-430)\] - Print the missing blocks when a parent block is missing in api-server
* \[[NOD-651](https://daglabs.atlassian.net/browse/NOD-651)\] - Properly handle the blocks that are being mined while netsync works

