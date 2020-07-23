# Transaction Malleability

T‌ransaction malleability is the ability to change a transaction's unique identifier by changing its signature, before it is [confirmed](../consensus/confirmations/).

‌The transaction still gets processed, but with a different identifier. This exposes the payer to an attack, where the payee can deny receiving payment, asking the payer to repay.

‌In Kaspa, transactions are referenced by their non-malleable transaction ID rather than their transaction hash.‌

## Transaction Non-Malleability <a id="Transaction-Non-Malleability"></a>

‌A non-malleable transaction is a transaction whose unique identifier cannot be changed by changing its internal elements, such as the signature scripts.

‌In Kaspa, the transaction ID does not hash the signature scripts, so it is not malleable, and does not expose the payer to an attack where the payee denies receiving a payment.‌

