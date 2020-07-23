# UTXO Set Data Structures and Algorithms

## Data Structures <a id="TheUTXOSet-DataStructures"></a>

### UTXOSet <a id="TheUTXOSet-UTXOSet"></a>

The UTXOSet interface is an abstraction of of a collection of UTXOs in a state.

Implementations: [DiffUTXOSet](diff-utxo-set.md), [FullUTXOSet](full-utxo-set.md)

#### UTXOSet.AddTx <a id="TheUTXOSet-UTXOSet.AddTx"></a>

> AddTx\(tx \*Tx\) \(ok bool\)

Given a pointer to a Tx, go over all the transactions, and try to apply them.

For a transaction input \(TxIn\) to be applicable to a UTXOSet, it is required that txIn.OutPoint corresponds to a UTXO \(i.e. all the coin that this Tx is trying to spend is indeed spendable\).

In this case, all the UTXOs which correspond to the OutPoints in txIn are removed, and all the outputs in tx.txOut are added to the UTXO.

This only happens if **all** TxIns are applicable. If **any** of the inputs is invalid, **none** of them are applied, and ok will return as false.

**Note**: AddTx does not verify that the transactions in the block make sense \(e.g. that the expenditure does not exceed the redeemed transactions\), it should only get legal transactions.

#### UTXOSet.Clone <a id="TheUTXOSet-UTXOSet.Clone"></a>

> Clone\(\) UTXOSet

Returns a perfect deep copy of `this`.

#### UTXOSet.Diff <a id="TheUTXOSet-UTXOSet.Diff"></a>

> Diff\(other UTXOSet\) \*UTXODiff

Returns a list of differences that, if applied to `this`, will result in a perfect copy of `other`.

**Warning**: The implementations of this method in both DiffUTXOSet and FullUTXOSet have non-trivial assumptions, please read the reference before using them.

#### UTXOSet.DiffFromTx <a id="TheUTXOSet-UTXOSet.DiffFromTx"></a>

> DiffFromTx\(tx \*Tx\) \(\*UTXODiff, error\)

Given a transaction, this method calculates the difference between `this` before and after applying the transaction.

If one or more of the inputs in the transactions do not appear as an output from the worldview of `this`,  an error is returned.

This has a default implementation in the function called `utxo_set.diffFromTx`. Implementations of UTXOSet should decorate this function by default, choosing not to do so must be explicitly justified in the documentation.

#### UTXOSet.Get <a id="TheUTXOSet-UTXOSet.Get"></a>

> Get\(outPoint [OutPoint](../outpoint.md)\) \(\*TxOut, bool\)

Given an OutPoint, return the corresponding transaction output \(TxOut\) in the UTXOSet.

If the TxOut is not in the UTXOSet return false in the second argument.

#### UTXOSet.Iterate <a id="TheUTXOSet-UTXOSet.Iterate"></a>

> Iterate\(\) UTXOIterator

Returns a UTXOIterator which iterates over the unspent transactions in the UTXO

#### UTXOSet.WithDiff <a id="TheUTXOSet-UTXOSet.WithDiff"></a>

> WithDiff\(diff \*UTXODiff\) \*UTXODiff

Given a [UTXO diff](../utxo-diffs/) `diff`, return a copy of `this` with the changes in `diff` applied.

### DiffUTXOSet <a id="TheUTXOSet-DiffUTXOSet"></a>

An implementation of UTXOSet, representing the data as some base set and a list of differences from the base set.

#### DiffUTXOSet.base <a id="TheUTXOSet-DiffUTXOSet.base"></a>

> \*[FullUTXOSet](full-utxo-set.md).

Pointer to the base relative to which the diff list describes our worldview.

#### DiffUTXOSet.diff <a id="TheUTXOSet-DiffUTXOSet.diff"></a>

> \*UTXODiff.

Points to the list of differences between the base and the described worldview.

#### DiffUTXOSet.Diff <a id="TheUTXOSet-DiffUTXOSet.Diff"></a>

> \(u \*DiffUTXOSet\) Diff\(other UTXOSet\) \*UTXODiff

An implementation of UTXOSet.Diff.

DiffUTXOSet.Diff has some assumptions on the input:

* other is an instance of DiffUTXOSet, and
* `u.base == other.base`

If the assumptions are not met a panic is raised.

After verifying these assumptions, the actual construction of the output is made with a call to UTXODiff.Diff.

Up to checking the assumptions and raising the required panic, this method is extremely simple:

```text
Diff(other UTXOSet)
    o = (DiffUTXOSet) other
    return u.diff.Diff(o.diff)
```

An implementation of UTXOSet simply holding a list of all UTXOs.

FullUTXOSet is a struct with a single field of type UTXOCollection.

#### FullUTXOSet.Diff <a id="TheUTXOSet-FullUTXOSet.Diff"></a>

> \(u \*FullUTXOSet\) Diff\(other UTXOSet\) \*UTXODiff

An implementation of UTXOSet.Diff.

FullUTXOSet.Diff has some assumptions on the input:

* other is an instance of DiffUTXOSet, and
* `u.base == other.base`

If the assumptions are not met a panic is raised.

If the assumptions are met then the list of differences is actually just `other.diff`.

### UTXODiff <a id="TheUTXOSet-UTXODiff"></a>

Represents the difference between two UTXOSets.

Note that UTXODiff is **antisymmetric** on UTXOSets – UTXODiff describes how to get from one set to the other, and to go the other way around one must _invert_ the UTXODiff, i.e. switch toAdd and toRemove.

Below we refer to the set from which changes are made as the _base_, and to the resulting set as the _result_. Note however that these sets do not exist as far as UTXODiff is concerned. It is up to the user that UTXODiff is used in a meaningful context.

A UTXODiff `d` is considered:

* _legal_ if d.toAdd ∩ d.toRemove = Ø
* _valid_ relative to a UTXOSet u if d.toRemove ⊆ u and d.toAdd ∩ = Ø

Note that being valid implies being legal, and that being legal implies being valid with respect to u = d.toRemove.

#### UTXODiff.Diff <a id="TheUTXOSet-UTXODiff.Diff"></a>

> \(d \*UTXODiff\) Diff\(other \*UTXODiff\) \*UTXODiff

Given two diffs relative to the same base, returns the differences between the two results, where the result of d is considered as the base of the returned UTXODiff.

Has the following assumptions on the input:

* Both `d` and `other` are legal
* Both `d` and `other` are valid relative to **the same** UTXOSet

`UTXODiff.Diff` iterates over all four collections and handles each UTXO appropriately.

The table below details where a given UTXO should be put in the output according to whether and where it appears in both `d` and `other`.

|  other \ d | toAdd | toRemove | neither |
| :--- | :--- | :--- | :--- |
| **toAdd** | none | impossible | toAdd |
| **toRemove** | impossible | none | toRemove |
| **neither** | toRemove | toAdd | irrelevant |

The cases marked impossible simply can not happen under the assumption that both diffs are valid relative to the same UTXOSet, since one assumes the UTXO is in the set while the other assumes the opposite, by way of contradiction.

Note that the assumptions on the input hold if and only if none of the impossible cases happen, so that raising an error upon such an impossibility constitutes a verification of validity.

The case marked irrelevant just never happens, as we only iterate over UTXOs in the lists.

#### UTXODiff.Inverted <a id="TheUTXOSet-UTXODiff.Inverted"></a>

> \(d \*UTXODiff\) Inverted\(\) \*UTXODiff

Returns a copy of `d` with toAdd and toRemove switched.

#### UTXODiff.NewUTXODiff <a id="TheUTXOSet-UTXODiff.NewUTXODiff"></a>

#### UTXODiff.toAdd <a id="TheUTXOSet-UTXODiff.toAdd"></a>

> UTXOCollection

The UTXOs which appear in the resulting set but not in the base set.

#### UTXODiff.toRemove <a id="TheUTXOSet-UTXODiff.toRemove"></a>

> UTXOCollection

The UTXOs which appear in the base set set but not in the result set.

#### UTXODiff.WithDiff <a id="TheUTXOSet-UTXODiff.WithDiff"></a>

> \(d \*UTXODiff\) WithDiff\(diff \*UTXODiff\) \*UTXODiff

This "concatenates" `d` with `diff`. It assumes that `diff` is valid with respect to the result of `d`.

For this to hold, it suffices to assume:

* d.toAdd ∩ diff.toAdd = Ø \(`diff` does not add any UTXO entries already in the result of `d`\)
* d.toRemove ∩ diff.toRemove = Ø \(`diff` does not remove any UTXO entries which are not in the result of `d`\)

UTXODiff.WithDiff creates a diff whose base is the base of d, and whose result is the result of diff relative to the result of d.

`UTXODiff.Diff` iterates over all four collections and handles each UTXO appropriately.

The table below details where a given UTXO should be put in the output according to whether and where it appears in both `d` and `diff`.

|  diff \ d | toAdd | toRemove | neither |
| :--- | :--- | :--- | :--- |
| **toAdd** | impossible | none | toAdd |
| **toRemove** | none | impossible | toRemove |
| **neither** | toAdd | toRemove | irrelevant |

The cases marked impossible simply can not happen under the assumptions, since it implies that diff adds/removes a UTXO entry which was already added/removed by d, by way of contradiction.

Note that the assumptions on the input hold if and only if none of the impossible cases happen, so that raising an error upon such an impossibility constitutes a verification of validity \(however, this is not done in the PoC\).

The case marked irrelevant just never happens, as we only iterate over UTXOs in the lists.

### UTXOCollection <a id="TheUTXOSet-UTXOCollection"></a>

Represents an arbitrary collection of UTXOs. Unlike UTXOSet, this is an arbitrary collection which needs not be a valid state \(i.e. it may hold conflicting UTXOs\).

### UTXOIteratorOutput <a id="TheUTXOSet-UTXOIteratorOutput"></a>

A struct containing an [OutPoint](../outpoint.md) and \*[TxOut](../).

Used as output to UTXOIterator.

The \*TxOut points to the UTXO associated with the OutPoint, out of the UTXOs in the UTXOSet on which this iterator was instantiated.

## Algorithms <a id="TheUTXOSet-Algorithms"></a>

### restoreUTXO <a id="TheUTXOSet-restoreUTXO"></a>

The `restoreUTXO` algorithm gets a block and restores the UTXO list from its worldview.

This is done as follows:

```text
restoreUTXO(block B)
    stack   = [B]
    cur     = B
     
    while cur.DiffChild is not null
        cur     = cur.DiffChild
        stack   = stack.append(cur)
 
    utxo    = empty UTXOSet
     
    while stack is not empty
        utxo = utxo.WithDiff(stack.pop.UTXODiff)
 
    return utxo
```

Note that this algorithm is agnostic about the type of the UTXOSet in question. In practice this function is always applied to a non-[virtual](../../../glossary.md#virtual-block) block, and in this case the output is the full list of differences between the input block and the current state.

### meldToBase <a id="TheUTXOSet-meldToBase"></a>

Given a list of differences in the form of a DiffUTXOSet `D`, apply the changes to `D.base` and replace `D.diff` with an empty `UTXODiff`.

This simple function should be used **with caution**. Typically D.base will contain a pointer to DAG.UTXOSet, so applying meldToBase will change the global state!

