# Finality Algorithm

{% hint style="info" %}
Extracted from posts by @Shai Wyborski and others at Kaspa Discord [https://discordapp.com/channels/599153230659846165/610815395527393301/676116635698069533](https://discordapp.com/channels/599153230659846165/610815395527393301/676116635698069533)

This is non-final discussion of finality.
{% endhint %}

A finality algorithm is used to externally prevent the [PHANTOM](../) ordering to change below a certain point, by rejecting [blocks](../../blocks/) which directly points to blocks which are “too old” \(this is a vague term, and designing a good [finality](./) approach mostly amounts to pinning down an appropriate precise interpretation\). The main purpose of finality is to allow [pruning](../../block-acceptance.md): since we know that some of the blocks will never be pointed, they could be discarded.

It should be noted that finality is consensus breaking. Testing whether a block is “too old” will always depend on local data \(such as the currently available graph, or the local time\) which could vary across nodes, and so it is always possible that the same block will be validated by one node but rejected by the other. Such a state is called a [net split](../../../components/kaspad-full-node/reference/network/#net-split), and every proposed finality algorithm should address this scenario.

### What we expect of a finality algorithm? <a id="What-we-expect-of-a-finality-algorithm?"></a>

We expect the following:

* Deterministic: the conditions which make us reject a block should be irreversible, regardless of any future blocks \(even in the presence of a 60% attacker\).
* Net split resilient: a less-than-half adversary should not be able to cause “meaningful” net splits. Below we precisely define what “meaningful” means, but the gist is that any differences between the nodes should not incur differences between their UTXO sets.
* Prunable: the finality should imply that there is a block in the selected chain of the virtual node such that the parents of any validated block will be in its future. This allows us to prune everything but the future of that block. Furthermore, the depth of this prunable block needs to remain constant with time.

### Parameters <a id="Parameters"></a>

* `windowSize`: the size of the finality window
* `tipDiff`: the maximum allowed difference between the scores of two parents which are blue from the block’s point of view

The parameters should be chosen such that:

* `windowSize >> tipDiff + k`
* The finality window results in a time window in the order of magnitude of approximately 24 hours or longer, so people have sufficient time to manually resolve issues.
* Changing the parameters after MainNet: TBD.

## Algorithm <a id="Algorithm"></a>

Given two blocks B, B', let split\(B,B'\) be the most recent block which is in the selected chain of both B and B'.

Whenever a new block B arrives, reject it if any of the following is true:

1. split\(B,T\).BlueScore &lt; T.BlueScore - windowSize \(where T is the selected tip\).
2. B points to two blocks B' and B'' such that B'.BlueScore - B''.BlueScore &gt; tipDiff

where T is the selected tip when B was discovered.

### Determinicity <a id="Determinicity"></a>

The determinicity of the algorithm is “true by design”. If F is in the selected chain of T such that F.BlueScore &lt; T.BlueScore - windowSize then the selected chain of _any_ validated block will always include F

### Net Split Resilience <a id="Net-Split-Resilience"></a>

In order for an attacker to create a net split, they must post a block C such that split\(C,T1\).BlueScore &gt;= T1.BlueScore - windowSize while split\(C,T2\).BlueScore &lt; T2.BlueScore - windowSize \(where T1 and T2 are the selected tips of two different nodes, at he time they heard of C\). After the adversary reported C to one of the nodes, the rest of the network will learn of C within a period of time which is orders of magnitude smaller than the length of a finality window \(seconds compared to days\). We may thus assume that \|T1.BlueScore - T2.BlueScore\| &lt;&lt; windowSize. It follows that split\(C,T1\).BlueScore &lt; T1.BlueScore - windowSize - x for some x &lt;&lt; windowSize.

Thus, C had to maintain a competing chain to the selected chain of T1 for the duration of a window. Now, there are two options:

* C.BlueScore &lt; T.BlueScore - tipDiff, and then no honest block will ever point to C
* C.BlueScore &gt;= T.BlueScore - tipDiff, which could only be true if the attacker maintained a separate chain _of comparable score_ for an entire finality window. The PHANTOM analysis proves that this could only be accomplished by a 51% attack

### Prunability <a id="Prunability"></a>

We wish to isolate a block G on the selected chain of T with the property that all parents of any newly validated block will be in the future of G. This implies that everything but the future of G may be discarded.

I argue that any G such that G.BlueScore &lt; T.BlueScore - 2\*windowSize - tipDiff - k has this property.

Indeed, assume that a block B was validated which does not have this property. Let F be the most recent block in the selected chain of T such that F.BlueScore &lt; T.BlueScore - windowSize, maximality implies that F.BlueScore &gt; T.BlueScore - windowSize - k, from which follows that G.BlueScore &lt; F.BlueScore - windowSize - tipDiff. Let C be the selected parent of B, that B was validated implies that F is in the selected chain of C. However, by hypothesis there is another parent C' of B such that G is not in its past. Since C and C' are both parents of the same valid block, we also have that C'.BlueScore &gt;= C.BlueScore - tipDiff.

Since G is in the selected chain of exactly one of C and C' it follows that Split\(C,C'\) is in the past of G, so Split\(C,C'\).BlueScore &lt; G.BlueScore &lt; F.BlueScore - tipDiff - windowSize &lt; C'.BlueScore - windowSize &lt;= C.BlueScore - windowSize.

Assume C was validated before C', and let T be the selected tip at the time C was validated, then we had that G was in the selected chain of T, while G.BlueScore &lt; C.BlueScore - windowSize &lt;= T.BlueScore - windowSize. So any block validated after C must have G in their selected chain, contradicting the hypothesis that C' was validated.

Assuming C' was validated earlier the same argument works to show that after C' was validated no block which _does_ have G in their selected chain would be validated.

### Comments <a id="Comments"></a>

* C doesn't affect the selected tip UTXO, but it does affect the virtual UTXO. In the fixed window approach, the virtual UTXO will have to add C's transactions for windowSize blocks \(unless we'll do some kind of ugly workaround\) and in the sliding window approach C will be finalized after a few blocks. C won't affect the "actual" UTXO. It will affect only the virtual UTXO.
* If we include red blocks, you can double spend any transaction in C, because C is not in the past of any block.
* Red inclusion doesn't matter here because C will remain in the anticone of the selected tip.
* It prevents a deep reorg by _any_ attacker. You know for sure that a transaction below the finality point is irreversible, but it doesn't prevent human intervention in case of deep reorg. The attacker can force you to intervene to decide the state of non finalized transactions. You can no longer choose to ignore deep reorgs because of the netsplit.
* The attacker can manipulate timestamps to decrease difficulty, and they can use this to create very long chain for a small amount of work. HOWEVER, at any point of time, the amount of blocks they have created _whose timestamp will be accepted by honest nodes_ is still proportional to their hashrate \(up to a small constant, depending on the grace we have to give to compensate for drift between clocks of honest nodes\).
* We want to select a parent by blue work rather than blue score. Need to consider the consequences, but on the surface I think this makes the proposed approach even tighter.

