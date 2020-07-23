# Double Spending

An [unspent transaction output](../txo/utxo.md) can be consumed and spent only once. **Double spending** is the practice of attempting to spend an output more than once.

Double spending is usually considered malicious, however there are some cases where it is not malicious.

A **non-malicious** example is when Alice sent Bob a transaction with a low fee, it does not get accepted to any block, so Alice wants to try again with a higher fee. She will have to create a new transaction, however she does not want to pay Bob twice, in case the first transaction also gets accepted to a block. Therefore, Alice can re-use in the second transaction one of the outputs she used in the first transaction. Since an output can only be spent once, only one of those transactions will eventually be accepted by the consensus protocol, and Alice will not be “charged” twice for consuming the same output.

A **malicious** example is when Alice buys a car from Bob. Alice makes a transaction to Bob, but at the same time, makes another transaction that re-uses one of the outputs from the transaction paying to Bob, but pays back to herself. Unaware of this, Bob hands over the car to Alice, and Alice disappears into the sunset. Meanwhile, Alice mines blocks that point to the block containing the transaction that paid herself, and eventually the consensus protocol favors that transaction over the transaction that paid Bob. Bob is left with no car and no money.

