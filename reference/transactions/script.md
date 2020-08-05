# Script

Kaspa uses a scripting system in its transactions almost identical to Bitcoin's. For a gentle introduction to Bitcoin Script, see [this section](https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch06.asciidoc#transaction-scripts-and-script-language) of Mastering Bitcoin Chapter 6, as well as [Chapter 7](https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch07.asciidoc) of the book. For a more detailed specification of Bitcoin Script, see the [Bitcoin wiki article](https://en.bitcoin.it/wiki/Script). Another good walkthrough is available on the [Bitcoin.org developer site](https://developer.bitcoin.org/devguide/transactions.html#introduction).

Transaction scripts describe how outputs can be accessed and allow flexibility in transaction type. A "locking" script "encumbers" the transaction outputs, requiring the "unlocking" script to meet certain criteria before the outputs can be released and spent in a new transaction.

Script uses commands called "opcodes".

### P2PKH

Most transactions use pay-to-pub-key-hash \(P2PKH\) scripts: outputs of a P2PKH transaction contain a locking script that locks the output to a public key hash, i.e., a Kaspa [address](../keys/). An output locked by a P2PKH script can be unlocked \(spent\) by presenting a public key and a digital signature created by the corresponding private key.

The locking script \(referred to as the "scriptPubKey"\) appears in the transaction output:

```text
OP_DUP OP_HASH160 <PubkeyHash> OP_EQUALVERIFY OP_CHECKSIG
```

A new transaction unlocks the transaction output for spending by including a "scriptSig" in its corresponding input:

```text
<Sig> <PubKey>
```

### P2SH

Pay-to-script-hash \(P2SH\) scripts allow versatility in transactions and simplify the use of complex transaction scripts. Instead of sending a transaction to an address \(or a complex string of addresses\), the script sends the transaction to a script hash.

The locking scriptPubKey is as follows:

```text
OP_HASH160 <Hash160(redeemScript)> OP_EQUAL
```

The unlocking scriptSig is as follows:

```text
<sig> [sig] [sig...] <redeemScript>
```

### Multisig <a id="Multisig"></a>

Multi-signature \(multisig\) scripts set a condition where N public keys are recorded in the script and at least M of those must provide signatures to unlock the funds. This is also known as an M-of-N scheme, where N is the total number of keys and M is the threshold of signatures required for validation.

The locking scriptPubKey is as follows:

```text
<m> <A pubkey> [B pubkey] [C pubkey...] <n> OP_CHECKMULTISIG
```

The unlocking scriptSig is as follows:

```text
OP_0 <A sig> [B sig] [C sig...]
```

### Changes in Script from Bitcoin

For completeness, the following lists changes in Script from Bitcoin to Kaspa.

1. No multisig dummy: in Bitcoin, the multisig opcode expects an additional input that it doesn’t use. In Kaspa this was removed\)
2. Remove OP\_CODESEPARATOR: in Bitcoin, OP\_CODESEPARATOR is used to make OP\_CHECKSIG check only _part_ of the scriptPubKey. Essentially, only the script that goes after the last OP\_CODESEPARATOR is used to sign the transaction and hence evaluated by OP\_CHECKSIG.
3. Remove FindAndDelete functionality
4. [SigHashSingle](https://godoc.org/github.com/btcsuite/btcd/txscript#SigHashType) errors on wrong index
5. Minimum-if-policy as consensus rule: in Bitcoin, this is only for segwit transactions
6. Applied[ BIP 62](https://github.com/bitcoin/bips/blob/master/bip-0062.mediawiki) as consensus rule
7. Changed opcodeCheckLockTimeVerify and opcodeCheckSequenceVerify to use Pop instead of Peek: because these opcodes override NOPs in Bitcoin, they need to peek for the input and then call OP\_DROP manually.

## Standard Transactions

As in Bitcoin, Kaspa has [standard transactions](https://developer.bitcoin.org/devguide/transactions.html#standard-transactions) that pass the IsStandard\(\) test in the Kaspad client. In Kaspad, only P2SH and P2PKH are standard.

