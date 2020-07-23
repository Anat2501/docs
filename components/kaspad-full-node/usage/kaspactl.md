# KaspaCTL

KaspaCTL is a tool to talk to a kaspad node over JSON-RPC using CLI.

## Usage

```text
kaspactl [OPTIONS]
```

## Options

<table>
  <thead>
    <tr>
      <th style="text-align:left">Option</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">/V, /version</td>
      <td style="text-align:left">Display version information and exit
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/l, /listcommands</td>
      <td style="text-align:left">List all of the supported commands and exit
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/C, /configfile:</td>
      <td style="text-align:left">Path to configuration file (default:
        <br />$USERHOME/AppData/Local/Kaspactl/kaspactl.conf)
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/u, /rpcuser:</td>
      <td style="text-align:left">RPC username
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/P, /rpcpass:</td>
      <td style="text-align:left">RPC password
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/s, /rpcserver:</td>
      <td style="text-align:left">RPC server to connect to (default: localhost)
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/c, /rpccert:</td>
      <td style="text-align:left">
        <p>RPC server certificate chain for validation (default:
          <br />
        </p>
        <p>$USERHOME/AppData/Local/Kaspad/rpc.cert)
          <br />
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/notls</td>
      <td style="text-align:left">Disable TLS
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/proxy:</td>
      <td style="text-align:left">Connect via SOCKS5 proxy (eg. 127.0.0.1:9050)
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/proxyuser:</td>
      <td style="text-align:left">Username for proxy server
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/proxypass:</td>
      <td style="text-align:left">Password for proxy server
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/skipverify</td>
      <td style="text-align:left">Do not verify tls certificates (not recommended!)
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/testnet</td>
      <td style="text-align:left">Use the test network
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/regtest</td>
      <td style="text-align:left">Use the regression test network
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">/simnet</td>
      <td style="text-align:left">Use the simulation test network</td>
    </tr>
    <tr>
      <td style="text-align:left">/devnet</td>
      <td style="text-align:left">Use the development test network
        <br />
      </td>
    </tr>
  </tbody>
</table>

## Help Options

| Option | Description |
| :--- | :--- |
| /?, /h, /help | Show this help message |

