# UTXO Set

The UTXO set is the set of all [unspent transaction outputs](../utxo.md).

It consists of all accepted transaction outputs \([TXO](../) set\) minus the set of all spent transaction outputs \([STXO](../stxo.md) set\).

`UTXO Set = TXO Set - STXO Set`

It is implemented as a [UTXO collection](utxo-collection.md).

Note: when referring to "the UTXO set of a [block](../../blocks/) B", what is actually meant is the UTXO set of [Past](../../blockdag/past.md)\(B\), i.e. the UTXO set known to the miner of B before B was created, and not including the transactions in B itself.

There are two implementations of the UTXO Set:

* [Full UTXO Set](full-utxo-set.md)
* [Diff UTXO Set](diff-utxo-set.md)

