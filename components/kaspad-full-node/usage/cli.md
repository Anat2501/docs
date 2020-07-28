---
description: >-
  The following is a list of command line arguments that may be passed to kaspad
  when starting it up.
---

# Kaspad Runtime Args

{% hint style="info" %}
Some arguments, if not specified, have defaults.
{% endhint %}

## Usage

```text
kaspad [OPTIONS]
```

Example:

```text
kaspad --outpeers=10 --maxinpeers=120 --testnet
```

## Options <a id="Version"></a>

### Version

| **Argument** | **Description** |
| :--- | :--- |
| `-V` or `--version` | Show version information and exit. |

### Help

| **Argument** | **Description** |
| :--- | :--- |
| `-h` or  `--help` | Show help message. |

### Configuration File

| **Argument** | **Description** |
| :--- | :--- |
| `-C` or  `--configfile=` | Path to configuration file. Default: `$userHome/.kaspad/kaspad.conf` |

### Data

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Argument</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>-b</code> or
        <br /><code>--datadir=</code>
      </td>
      <td style="text-align:left">Directory to store data.
        <br />Default: <code>$userHome/.kaspad/data</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--reset-db</code>
      </td>
      <td style="text-align:left">
        <p>Reset database before starting the node.</p>
        <p><b>Required</b> when switching between <a href="../../../glossary.md#subnetwork">subnetworks</a>.</p>
        <p>Switching subnetworks without specifying this
          <br />will produce an error.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--acceptanceindex</code>
      </td>
      <td style="text-align:left">
        <p>Maintain a full index of
          <br />chain-block-hash/accepted-tx-ids/from-block.</p>
        <p>Makes the <code>getChainFromBlock</code> RPC available.</p>
        <p><b>Required</b> to be included from the first run.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--dropacceptanceindex</code>
      </td>
      <td style="text-align:left">Deletes the hash-based acceptance index
        <br />from the database on start up and then exits.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--dbtype=</code>
      </td>
      <td style="text-align:left">
        <p>Database backend to use for the Block DAG.
          <br />Default: <code>ffldb</code>
        </p>
        <p>This is currently the only option.</p>
      </td>
    </tr>
  </tbody>
</table>

### Logs

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Argument</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>-d</code> or <code>--debuglevel=</code>
      </td>
      <td style="text-align:left">
        <p>Logging level for all subsystems. Options:
          <br /><code>trace</code>, <code>debug</code>, <code>info</code>, <code>warn</code>, <code>error</code>, <code>critical</code>
          <br
          />Default: <code>info</code>
        </p>
        <p>You may also specify <code>&lt;subsystem&gt;=&lt;level&gt;,</code>
        </p>
        <p><code>&lt;subsystem2&gt;=&lt;level&gt;,...</code> to set the log level
          <br
          />for individual subsystems.
          <br />Use <code>show</code> to list available subsystems.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--logdir=</code>
      </td>
      <td style="text-align:left">Directory to log output. Default:
        <br /><code>$userHome/.kaspad/logs</code>
      </td>
    </tr>
  </tbody>
</table>

### Peers

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Argument</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>-a</code> or <code>--addpeer=</code>
      </td>
      <td style="text-align:left">
        <p>Add a peer to connect with at startup.</p>
        <p>Expected input: <code>address:port</code>
        </p>
        <p>If port unspecified, default <code>16111</code> is used.</p>
        <p>Multiple <code>-a</code> arguments may be chained.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--connect=</code>
      </td>
      <td style="text-align:left">
        <p>Connect only to the peers specified at startup</p>
        <p>Expected input: <code>address:port</code> or <code>id</code>
        </p>
        <p>If port unspecified, default <code>16111</code> is used.</p>
        <p>Multiple <code>--connect</code> arguments may be chained.</p>
        <p><b>Note</b>: when used, node will not listen to incoming
          <br />connections.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--nolisten</code>
      </td>
      <td style="text-align:left">
        <p>Disable listening for incoming P2P connections.</p>
        <p><b>Note</b>: Listening is automatically disabled if the
          <br /><code>--connect</code> or <code>--proxy</code> options are used without
          <br
          />also specifying listen interfaces via <code>--listen</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--listen=</code>
      </td>
      <td style="text-align:left">Add an interface/port to listen for P2P connections.
        <br />Default all interfaces port: <code>16111</code>, testnet: <code>16211</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--outpeers=</code>
      </td>
      <td style="text-align:left">Target number of outbound peers
        <br />Default: <code>8</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--maxinpeers=</code>
      </td>
      <td style="text-align:left">Max number of inbound peers
        <br />Default: <code>117</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--nobanning</code>
      </td>
      <td style="text-align:left">Disable banning of misbehaving peers</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--banduration=</code>
      </td>
      <td style="text-align:left">How long to ban misbehaving peers for.
        <br />Valid time units are {<code>h</code>, <code>m</code>, <code>s</code>}.
        <br
        />Minimum: <code>1</code> second
        <br />Default: <code>24h0m0s</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--banthreshold=</code>
      </td>
      <td style="text-align:left">
        <p>Maximum allowed ban score before disconnecting
          <br />from and banning misbehaving peers.
          <br />Default: <code>100</code>
        </p>
        <p>See <code>banscores.go</code> for more info.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--whitelist=</code>
      </td>
      <td style="text-align:left">Add an IP network or IP that will not be banned.
        <br />E.g. <code>192.168.1.0/24</code> or <code>::1</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--nopeerbloomfilters</code>
      </td>
      <td style="text-align:left">Disable bloom filtering support</td>
    </tr>
  </tbody>
</table>

### RPC

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Argument</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>-u</code> or <code>--rpcuser=</code>
      </td>
      <td style="text-align:left">Username for RPC connections</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>-P</code> or <code>--rpcpass=</code>
      </td>
      <td style="text-align:left">Password for RPC connections</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--rpclimituser=</code>
      </td>
      <td style="text-align:left">Username for limited RPC connections</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--rpclimitpass=</code>
      </td>
      <td style="text-align:left">Password for limited RPC connections</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--rpclisten=</code>
      </td>
      <td style="text-align:left">Add an interface/port to listen for RPC connections
        <br />Default port: <code>16110</code>, testnet: <code>16210</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--rpccert=</code>
      </td>
      <td style="text-align:left">File containing the certificate file
        <br />Default: <code>$userHome/.kaspad/rpc.cert</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--rpckey=</code>
      </td>
      <td style="text-align:left">File containing the certificate key
        <br />Default: <code>$userHome/.kaspad/rpc.key</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--rpcmaxclients=</code>
      </td>
      <td style="text-align:left">Max number of RPC clients for standard connections
        <br />Default: <code>10</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--rpcmaxwebsockets=</code>
      </td>
      <td style="text-align:left">
        <p>Max number of RPC websocket connections</p>
        <p>Default: <code>25</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--rpcmaxconcurrentreqs=</code>
      </td>
      <td style="text-align:left">
        <p>Max number of concurrent RPC requests that may be
          <br />processed concurrently</p>
        <p>Default: <code>20</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--norpc</code>
      </td>
      <td style="text-align:left">
        <p>Disable built-in RPC server</p>
        <p><b>Note</b>: The RPC server is disabled by default if no
          <br /><code>rpcuser</code>/<code>rpcpass</code> or <code>rpclimituser</code>/<code>rpclimitpass</code>
          <br
          />is specified</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--notls</code>
      </td>
      <td style="text-align:left">
        <p>Disable TLS for the RPC server</p>
        <p><b>Note</b>: This is only allowed if the RPC server is bound
          <br />to localhost</p>
      </td>
    </tr>
  </tbody>
</table>

### DNS Seeder

| **Argument** | **Description** |
| :--- | :--- |
| `--nodnsseed` | Disable DNS seeding for peers |
| `--dnsseed=` | Override DNS seeds with specified hostname. Only 1 hostname allowed |

### Proxy

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Argument</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>--proxy=</code>
      </td>
      <td style="text-align:left">
        <p>Connect via SOCKS5 proxy, e.g. <code>127.0.0.1:9050</code>
        </p>
        <p><b>Note</b>: when used, node will not listen to incoming</p>
        <p>connections.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--proxyuser=</code>
      </td>
      <td style="text-align:left">Username for proxy server</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--proxypass=</code>
      </td>
      <td style="text-align:left">Password for proxy server</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--profile=</code>
      </td>
      <td style="text-align:left">Enable HTTP profiling on given port.
        <br /><b>Note</b>: port must be between <code>1024</code> and <code>65536</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--cpuprofile=</code>
      </td>
      <td style="text-align:left">Write CPU profile to the specified file</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--externalip=</code>
      </td>
      <td style="text-align:left">Add an IP to the list of local addresses we claim
        <br />to listen top peers on</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--upnp</code>
      </td>
      <td style="text-align:left">Use UPnP to map our listening port outside of NAT</td>
    </tr>
  </tbody>
</table>

### Mempool

<table>
  <thead>
    <tr>
      <th style="text-align:left">Argument</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>--minrelaytxfee=</code>
      </td>
      <td style="text-align:left">The minimum transaction fee in Sompis/gram
        <br />to be considered a non-zero fee.
        <br />Default: <code>KAS 1^-05</code> or <code>Sompis 0.00001000</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--maxorphantx=</code>
      </td>
      <td style="text-align:left">Max number of orphan transactions to keep in
        <br />memory.
        <br />Default: <code>100</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--rejectnonstd</code> or
        <br /><code>--relaynonstd</code>
      </td>
      <td style="text-align:left">
        <p>Reject or relay non-standard transactions regardless
          <br />of the default settings for the active network.</p>
        <p>Default:</p>
        <p>reject: mainnet</p>
        <p>relay: testnet, devnet, simnet, regtest</p>
      </td>
    </tr>
  </tbody>
</table>

### Mining

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Argument</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>--uacomment</code>
      </td>
      <td style="text-align:left">
        <p>Comment to add to the user agent</p>
        <p>See <a href="https://github.com/bitcoin/bips/blob/master/bip-0014.mediawiki">BIP-0014</a> for
          more information.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--sigcachemaxsize=</code>
      </td>
      <td style="text-align:left">The maximum number of entries in the signature
        <br />verification cache
        <br />Default: <code>100000</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--blocksonly</code>
      </td>
      <td style="text-align:left">Do not accept transactions from remote peers.</td>
    </tr>
  </tbody>
</table>

{% hint style="warning" %}
The following arguments have been moved to kaspaminer
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Argument</b>
      </th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>--miningaddr=</code>
      </td>
      <td style="text-align:left">
        <p>Add the specified payment address to the
          <br />list of addresses to use for generated blocks.</p>
        <p>At least one address is required if the
          <br />generate option is set.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--blockmaxmass=</code>
      </td>
      <td style="text-align:left">Maximum transaction mass to be used when
        <br />creating a block
        <br />Default: <code>1000000</code>
      </td>
    </tr>
  </tbody>
</table>

### Subnetworks

| **Argument** | **Description** |
| :--- | :--- |
| `--subnetwork=` | If a subnetwork ID is specified, then node will  request and process only payloads from  specified subnetwork. And if a subnetwork ID  is omitted, then payloads of all subnetworks  are processed. [Subnetworks](../reference/subnetworks-1.md) with IDs 0 through  255 are reserved for future use and are currently  not allowed. |

### Network Selection

{% hint style="info" %}
MainNet is the default network
{% endhint %}

| **Argument** | **Description** |
| :--- | :--- |
| `--testnet` | Use the test network |
| `--regtest` | Use the regression test network |
| `--simnet` | Use the simulation test network |
| `--devnet` | Use the development test network |

\[page word count: ~700\]

