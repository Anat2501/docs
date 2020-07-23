# Address Format

Kaspa uses the [cashaddr](https://github.com/bitcoincashorg/bitcoincash.org/blob/master/spec/cashaddr.md) format for addresses, derived from Bitcoin Cash.

The address is comprised of:

1. A [prefix](keys-and-addresses.md#prefix)
2. A [separator](keys-and-addresses.md#separator)
3. A [payload](keys-and-addresses.md#payload)

```text
prefix   separator   base-32 encoded payload
------   ---------   ------------------------------------------------------
                     version  (1 byte)   hash                      checksum
                     -----------------   -----------------------   --------
                     0   type   length  
                     -   ----   ------  
```

## Prefix

The prefix indicates on which network the address is valid. Below is a list of the prefixes for each network.

| Network | Prefix |
| :--- | :--- |
| MainNet | kaspa |
| TestNet | kaspatest |
| DevNet | kaspadev |
| RegNet | kaspareg |
| SimNet | kaspasim |

## Separator

The separator is a semicolon `:`

## Payload

The payload is base32 encoded:

|  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| +0 | q | p | z | r | y | 9 | x | 8 |
| +8 | g | f | 2 | t | v | d | w | 0 |
| +16 | s | 3 | j | n | 5 | 4 | k | h |
| +24 | c | e | 6 | m | u | a | 7 | l |

Lowercase is preferred. Uppercase is accepted. A mix of lower and uppercase should not be accepted.

The payload is comprised of:

* A [version](keys-and-addresses.md#version)
* A [hash](keys-and-addresses.md#hash)
* A [checksum](keys-and-addresses.md#checksum)

### Version

The version is 1 byte comprised of the following:

| Field | Bits | Description |
| :--- | :--- | :--- |
| Reserved bit | 1 | Reserved bit equaling Zero \(0\) |
| Address type | 4 | P2KH or P2SH |
| Hash length | 3 | The length of the hash part of the payload |

#### Address Type

Possible address types:

| Address type bits | Address Type |
| :--- | :--- |
| 0 | P2KH |
| 1 | P2SH |

#### Hash Length

Possible hash lengths:

| Hash Length bits | Hash size in bits |
| :--- | :--- |
| 0 | 160 |
| 1 | 192 |
| 2 | 224 |
| 3 | 256 |
| 4 | 320 |
| 5 | 384 |
| 6 | 448 |
| 7 | 512 |

### Hash

The hash is a base32-encoded hash of the public key for P2KH addresses, or the hash of the scriptPubKey for P2SH addresses.

### Checksum

See the calculation on the [original spec](https://github.com/bitcoincashorg/bitcoincash.org/blob/master/spec/cashaddr.md#checksum).

### Examples

* `kaspa:pr6m7j9njldwwzlg9v7v53unlr4jkmx6ey65nvtks5`
* `kaspatest:pr6m7j9njldwwzlg9v7v53unlr4jkmx6eyvwc0uz5t`
* `kaspadev:qr6m7j9njldwwzlg9v7v53unlr4jkmx6eylep8ekg2`

