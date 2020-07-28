# PHANTOM/GHOSTDAG

\[I like this explanation, but there is already a page. Leaving here for when I go through and refine that page.\]

## PHANTOM

{% hint style="info" %}
TL;DR: A blockDAG consensus protocol that returns an ordered list of blocks and a set of -- intuitively -- "well-connected" blocks to be inherited by the future blockDAG.
{% endhint %}

A consensus protocol in which miners are expected to reference, in their new blocks, all observed tips of the blockDAG, and publish their blocks as quickly as possible. Intuitively, blocks created by honest miners \(presumed to make up &gt;50% of the network\) are more "well-connected" than blocks created by miners who do not follow the default policy.

The PHANTOM authors expand on this as follows:

> Indeed, let _D_ be an upper bound on the network’s propagation delay. If block _B_ was mined by an honest miner at time _t_, then any block published before time _t_ - _D_ necessarily arrived at its miner before time _t_, and is therefore included in _past_\(_B_\). Similarly, the honest miner will publish B immediately, and so _B_ will be included in the past set of any block mined after time _t_ + _D_. As a result, the set of honest blocks in B’s anticone is typically small, and consists only of blocks created in the interval \[_t_ − _D_, _t_ + _D_\]. The proof-of-work mechanism guarantees that the number of blocks created in an interval of length 2 · _D_ is typically below some _k_ &gt; 0.

In other words, if B is an honest block, then the honest blocks in B's anticone are few and they are there due to the network propagation delay, which is parameterized by a constant _k_. _k_ is at the core of the PHANTOM protocol; PHANTOM captures the well-connected set of blocks in a DAG by defining a "_k_-cluster", as follows:

> Given a DAG _G_ = \(_C_, _E_\), a subset _S_ ⊆ _C_ is called a _**k**_**-cluster**, if ∀_B_ ∈ _S_: \|_anticone_\(_B_\) ∩ _S_\| ≤ _k_.

In other words, a _k_-cluster is a subset of blocks in a DAG such that each block in the subset has _k_ or fewer of the other blocks in the subset in its anticone.

Therefore, to find the most well-connected group of blocks, the PHANTOM protocol solves the optimization problem of finding the largest possible _k_-cluster in a blockDAG. This problem is formalized as follows:

> **Maximum** _**k**_**-cluster SubDAG \(**_**MCS\_k**_**\)  
> Input:** DAG _G_ = \(_C_, _E_\)  
> **Output:** A subset _S\*\* ⊂ C of maximum size, s.t. \|anticone \(\_B_\) ∩ _S\*_\| ≤ _k \_for all_ B _∈_ S\*\*.

PHANTOM "colors" all blocks in the _k_-cluster blue \(probably honest\) and all other blocks red \(probably malicious\).

We use an example of a 3-cluster from the paper to illustrate PHANTOM:

![](../../.gitbook/assets/image%20%283%29.png)

> It is easy to verify that each of these blue blocks has at most 3 blue blocks in its anticone, and \(a bit less easy\) that this is the largest set with this property.

After solving _MCS\_k_, PHANTOM uses any simple topological sort to order the DAG, favoring blue blocks over red blocks \(e.g., when ordering, add red blocks right before a blue block _B_ only if it is in _past_\(_B_\)\), resulting in a total block order from which a consistent transaction set can be extracted.

_MCS\_k_, however, is [NP-hard](https://en.wikipedia.org/wiki/NP-hardness), and thus is not suitable for a growing blockDAG. Kaspa uses a greedy version of PHANTOM, called GHOSTDAG, that is possible to implement.

For a full explanation of PHANTOM, see [the original paper](https://eprint.iacr.org/2018/104.pdf).

## GHOSTDAG

* [ ] TBD: Yoni proofread

{% hint style="info" %}
TL;DR: A greedy version of PHANTOM.
{% endhint %}

A _greedy, recursive algorithm_ that approximates the largest _k_-cluster in a DAG. A _greedy algorithm_ for an optimization problem is one that uses a heuristic to find a local optimum at each step of the process, in order to approximate a global optimum in a short amount of time. A _recursive algorithm_ for a problem is one that solves the problem by first solving smaller instances of the same problem; thus the algorithm contains at least one call to itself.

The authors introduce GHOSTDAG as follows:

> The algorithm constructs the Blue set of the DAG by first inheriting the Blue set of the best tip _B_\_max, i.e., the tip with the largest Blue set in its past, and then adds to the Blue set blocks outside _B_\_max’s past, provided that the _k_-cluster property is preserved.

In other words, the problem to be solved by _recursion_ is finding the blue set \(maximal _k_-cluster\) of the entire current DAG, which relies on the incrementally smaller problems of finding the blue set of each of the DAG’s tips. Solving a problem with recursion is possible with a _base case_—in this case, the _genesis_ block—where the solution is trivial—in this case, the blue set consists of itself. The _greedy heuristic_ is to construct the current blue set with the largest blue set of these tips, then add to it all _k_-cluster preserving blocks outside the selected tip’s past.

> Observe that this greedy inheritance rule induces a chain: The last block of the chain is the selected tip of _G_, _B_\_max; the next block in the chain is the selected tip of the DAG _past_\(_B_\_max\); and so on down to the _genesis_. We denote this chain by _Chn_\(_G_\) = \(_genesis_ = _Chn_\_0\(_G_\), _Chn_\_1\(_G_\), . . . , _Chn_\_h\(_G_\)\).

Henceforth, we refer to this chain as the _selected chain_ of the DAG.

> The final order over all blocks, in GHOSTDAG, follows a similar path as the colouring procedure: We order the blockDAG by first inheriting the order of _B_\_max on blocks in _past_\(_B_\_max\), then adding _B_\_max itself to the order, and finally adding blocks outside _past_\(_B_\_max\) according to some topological ordering. Thus, essentially, the order over blocks becomes robust as the colouring procedure.

In GHOSTDAG, finding the current blue set is needed to find _B_\_max in the future DAG, and future _B_\_max \(and its blue set and order of blocks\) is needed to find the blue set and order of blocks in the future DAG. Like coloring, ordering the blocks is recursive and greedy. Coloring and ordering of blocks happen in the same iterations, and the recursive call is to the entire GHOSTDAG algorithm. Note that the ordering favors the blue set, by nature of it favoring _B_\_max. Once a total block order is reached, a consistent transaction set can be extracted. GHOSTDAG returns an ordered list of blocks and a blue set to be inherited by the future DAG.

For the formal GHOSTDAG algorithm and a walk-through of it, see section 2.4.1 of [the PHANTOM paper](https://eprint.iacr.org/2018/104.pdf).

Note that in both PHANTOM and GHOSTDAG, setting _k_ = 0 corresponds to selecting the single longest chain, as in Bitcoin.

