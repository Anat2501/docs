# Chained Transactions

Chained transactions are a pair of [transactions](./) such that one spends an output of the other. E.g., transaction B spends an output of transaction A.

In Kaspa, chained transactions within the same block are forbidden, to allow multi-threaded transaction verification.

{% hint style="info" %}
Bitcoin, with its one-block-per-10-minutes block rate, allows chained transactions in order to facilitate “Child Pays for Parent \(CPFP\)”. CPFP is a concept where a child transaction that relies on the confirmation and the outputs of an earlier parent transaction, compensates for its low fee, so that both the parent and child can be confirmed sooner, in the same block.

Kaspa, with its one-block-per-second block rate, obviates the need to chain transactions within the same block. The benefit of disallowing chained transactions is the ability to parallelize verification of transactions within the same block.
{% endhint %}

