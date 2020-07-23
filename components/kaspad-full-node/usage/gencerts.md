# GenCerts

When connecting to JSON-RPC, an SSL certificate needs to be presented, and one can be generated using gencerts.

## Usage

```text
gencerts [OPTIONS]
```

## Options

| Option | Description |
| :--- | :--- |
| /d, /directory: | Directory to write certificate pair |
| /y, /years: | How many years a certificate is valid for \(default: 10\) |
| /o, /org: | Organization in certificate \(default: gencerts\) |
| /H, /host: | Additional hosts/IPs to create certificate for |
| /f, /force | Force overwriting of any old certs and keys |

## Help Options

| Option | Description |
| :--- | :--- |
| /?, /h, /help | Show this help message |

