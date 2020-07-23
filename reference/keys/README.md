# Keys and Addresses

The [CLI Wallet](../../components/cli-wallet.md) bundled with [Kasparov](../../components/kasparov-api-server/) uses the following process to generate its keys and addresses:

1. [Libsecp256k1](https://github.com/bitcoin-core/secp256k1) \([Bitcoin Wiki](https://en.bitcoin.it/wiki/Secp256k1)\) is used to generate a 32-byte, hex-encoded seed on the curve.
2. The seed is decoded. 
3. The decoded result is deserialized to a private key. 
4. The private key is used to generate a public key. 
5. The public key is used to generate an [address](keys-and-addresses.md).

