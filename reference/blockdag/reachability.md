# Reachability

Reachability between two [blocks](../blocks/) in the [DAG](./) is whether one block is in the [past](past.md) of the other, i.e., whether there is a [directed path](https://en.wikipedia.org/wiki/Path_%28graph_theory%29) from one block to the other.

In a broader sense, reachability is the ability to maintain a data structure that is query-able over the DAG.

## The Reachability Problem

**Note**: In the article below we use freely the fact that our DAG G = \(V, E\) is connected and has [out degrees](https://en.wikipedia.org/wiki/Directed_graph#Indegree_and_outdegree) bounded by L. This implies that \|V\| ≤ \|E\| ≤ L\|V\| so O\(\|E\|\) = O\(\|V\|\). The block creation time is best described as a Poisson process, which implies that O\(\|V\|\) = O\(T\) is the running time of the system. We hence use all of the above interchangeably.

## Introduction <a id="TheReachabilityProblem-Introduction"></a>

One of the services we need to provide is to efficiently answer _reachability queries_, i.e. we should be able to efficiently tell whether one block is in the [past](past.md) of the other.

The naive approach to that problem is to traverse the past of the newer of the blocks, returning true if we traverse the older block at some point or false if the search is exhausted. This solution is obviously unacceptable, as it has a worst case complexity of O\(IE\|\).

Another naive approach is to store a huge hash set of all the pairs of non-comparable blocks. This allows for a \(amortized, or worst case, or average case, depends on the hashing method used\) O\(1\) query time, but at the expanse of O\(\|V\|^2\) space, which is unreasonably huge.

It seems that a compromising approach is required, balancing between the space and time complexity to reach feasible bounds on both sides. Unfortunately, despite our best efforts it seems that such a data structure is not known to exist.

The next step is to further compromise. The approach finally agreed upon is to find a solution which:

* requires at most O\(T\) space
* efficiently answers reachability queries _between_ [_blue blocks_](../consensus/blue-set/)\_\_
* answers reachability queries of [red blocks](../consensus/red-set.md) in linear time
* is incremental, and
* can be "cleaned out" easily when pushing the [finality](../consensus/finality-1/) window

The basis of this compromise is that the [PHANTOM](../consensus/) algorithm provides us with a [blue subset](../consensus/blue-set/) with very strong combinatorial properties which could be useful.

## The Blue Leaf Graph method <a id="TheReachabilityProblem-theBlueLeafGraphmethod"></a>

Anything below will be properly defined in following sections.

The following algorithm exploits the fact that PHANTOM affords us a big \(at times of peace\) subset of the graph with nice combinatorial properties, namely the blue set from the point of view of the virtual node.

**Henceforth, when we call a node blue, it is to be understood that it is blue from the context of the virtual node**.

As we know, this set has the property that any blue block has at most _k_ blue blocks in its anticone. By adding some edges \(namely from any blue edge to its so called _blue leaves_**\)**, one can construct a graph whose vertices are the blue blocks which is even nicer. This graph is called the _blue leaves graph_, and it has the following nice properties:

* It is connected
* It is _k_-connected
* Its only sink is _Genesis_, so it affords a notion of height which turns out to be useful, we will define it as the _blue height_
* Two blue blocks are reachable in the blue leaves graph if _and only if_ the are reachable in the DAG

As we will see, two blocks whose height difference is larger than _k_ are definitely reachable. If the height difference is smaller than that then one can look for one block in the past of the other, by performing a DFS of bounded depth \(and hence, bounded time\). This means that any query between blue blocks could be answered in time constant in _T_.

This algorithm can be half extended to reachability queries from a red block to a blue block: the half that works is that having a large blue height difference still immediately implies reachability, the half that does not work is that in the other case we can no longer bound the depth required to determine reachability \(and the search itself would have 

As we detail below, a major advantage of this method is that with slight modification, it can also allow to query whether a blue block is reachable from _any_ block \(be it blue or red\). The constant bound on red to blue queries is larger by a factor of _k_, but it is still constant in _T_. However, this approach still does not imply efficient reachability queries to red blocks.

Note that in some cases the algorithm FAILS. How to handle these fails \(e.g. checking reachability naively in linear time, or not supporting these cases\) is to be determined independently.

### A Toy Example - The _k_-connected Case <a id="TheReachabilityProblem-AToyExample-The2353d8fa-3e87-36c6-89e7-288814f9ed62k-connectedCase"></a>

In this inductory section we solve the problem for the case where the entire DAG is blue.

Recall that a DAG is _k_-connected if it has no anticones of size &gt; _k_. Also recall that _k_-connected graph _need not be singe sink or even connected_.

**Proposition**: Suppose _D_ is a _k_-connected DAG, with a unique sink node _Gen_. Define the _height_ of a node _d_\(_v_\) to be its distance from _Gen_. Let _u_, _v_ ∈ _D_ such that _v_ ≰ _u_, then _d_\(_v_\) - _d_\(_u_\) &gt; _k_, then _u_ ≤ _v._

**Proof**: Assume _u_ ≰ _v_.  Consider the chain _v_ = _v\__0 ≥ _v_\_1 ≥ ... ≥ _v_\__k_ with _d_\(_v\_j_\) = _d_\(_v\__{_j_-1}\) - 1, _j_ = 1, ..., _k_ \(such chain could always be constructed by the definition of length\). Then _d_\(_v\_k_\) = _d_\(_v_\) - _k_ &gt; _d_\(_u_\) and hence _v\_k_ ≰ _u_.

It follows that {_v\__0, ..., _v\_k_} ⊂ _Anticone_\(_u_\) contradicting _k_-connectivity □

This solves the problem on the spot for blocks that are far away. The only remaining case is if their distance is bound by _k_, which means that we can verify it in constant time.

This all comes down to a simple algorithm in the case of  _k_-connectedness. The algorithm assumes that we maintain a linear ordering of the blocks by order inclusion \(which is just an arbitrary topological sort which is easy to maintain, note that this order might be different for different nodes\), as well as the height of the nodes.

The algorithm is as follows:

```text
Input: two (hashes of) blocks u, v
Output: TRUE iff there's a path from v to u (i.e. if u is in Past(v))
 
If v appears before u in the topological order return FALSE
If d(v) - d(u) > k return TRUE
Traverse from u down the blue leafs, terminating at nodes with Height <= u.Height. Return TRUE if u is found or FALSE if the search is exhausted
```

Or to put it verbally: If they are far away they must be reachable, if they are close then you have to check, but you get a bound on the depth of the check.

**Proposition**: This algorithm runs in time _O_\(_k_^2 ⋅ ****min{_L_, _k_}\)

**Proof**: The only case whose run time depends on _k_ is the case _d_\(_v_\) - _d_\(_u_\) ≤ _k_.

We note that the set of all blocks of a given fixed blue height is an antichain, so for any possible blue height there are at most _k_ + 1 blocks of that height.

Since we are starting with a block of blue height _d_\(_v_\) and only traversing downwards, terminating at blocks of blue height _d_\(_u_\) it turns out we only traverse _k_ + 1 different height, so that we are only traversing _O_\(_k_^2\) blocks.

From any given block we traverse to its immediate children. There are at most _L_ of them by definition, but also note that the set of immediate children of a block is an antichain, so one can also say there are at most _k_ + 1 of them.

This gives the required bound. □

### The Blue Leaf Graph <a id="TheReachabilityProblem-TheBlueLeafGraph"></a>

Of course in the general case we may not assume that our DAG is _k_-connected.

The PHANTOM algorithm affords a blue subset which allows us to construct one, namely the blue set of the virtual block.

It is tempting to say that we can treat this blue set as a _k_-connected subgraph, but that is not the case. Indeed, the _k_-connectedness means that there are no large anticones in the case of the entire graph. That is, two blue blocks are not considered in each other's anticone if one is reachable from the other in the original graph.

Consider the following simple example:

![](https://daglabs.atlassian.net/wiki/download/attachments/246612073/BLG1.png?version=1&modificationDate=1553509789952&cacheVersion=1&api=v2)

In this example A and B are not considered in each other's anticone, even though they are when passing to the blue subgraph.

The solution to this is to add edges which retain the properties. More specifically, for _any_ block _B_ we maintain a set of blocks _B.BlueLeaves_ which contains all blue blocks which are:

* in _B.Past_
* are not reachable from any other blue in _B.Past_

As an example consider the following graph:

![](https://daglabs.atlassian.net/wiki/download/attachments/246612073/BLG2.png?version=1&modificationDate=1553511157058&cacheVersion=1&api=v2)

Note that B is a blue leaf of A while D is not. Even though D is reachable from A, it is not included because it is reachable from B.

**Corollary**: If a blue block _B_ is reachable from a block _A_, then it is also reachable from one of its blue leafs

We maintain these sets for all blocks rather than just for blue blocks so that the complexity of calculating the blue leafs of a new block will remain independent of _n_, as we shall detail below.

This allows the notion of the _blue leaf graph_:

* The vertices are the blue blocks
* There is an edge from any block to all of its blue leaves

The blue leaf graph of the graph above is:

![](https://daglabs.atlassian.net/wiki/download/attachments/246612073/BLG3.png?version=1&modificationDate=1553511689070&cacheVersion=1&api=v2)

**Corollary**: The blue leaf set of a node is a blue antichain, and hence of size at most _k_ + 1

**Corollary**: The only sink node in this graph is the genesis node

The last corollary allows us to define the _blue height_ of each _blue_ node to be the length of a _maximal_ path from the node to genesis in this graph, more accurately:

* _Genesis.BlueHeight_  = 0
* ![](https://latexmath.bolo-app.com/render/1.0/img/372c9faff2d92de7025cbf7b764a2e42)

**Corollary**: If _A_ and _B_ are blue, _B_ ≤ _A_ and _A.BlueHeight_  = _B.BlueHeight_ - 1 then _B_ ∈ _A.BlueLeaves_

**Proposition**: Two blue blocks are reachable in the blue leaf graph if and only if they are reachable in the original graph

**Proof**: If two blocks are reachable in the blue leaf graph they are obviously reachable in the original graph.

For the other direction we note that if there is a path from _A_ to _B_ in the original, then there is also a path from _A_ to _B_ such that each blue block along the path is a blue leaf of the previous blue in the path.

To do so assume that _B'_ and _B''_ are two consecutive blue blocks along the path. If _B''_ then we are done, else we have that is reachable from some _B'''_ which is a blue leaf of _B'_. So we can replace the original stretch of path from _B'_ to _B''_ by a path from _B'_  to _B'''_ to _B''_. We now repeat the process for the path from _B'''_ to _B''_. This process terminates after at most _B'.blueHeight_ - _B''.blueHeight_ steps. □

**Corollary**: The blue leaf graph is _k_-connected

This implies that we can apply the method described in the previous section to answer blue-blue queries in the general case, by traversing the blue leaf graph rather than the original graph.

**Proposition**: If _B_, _B'_ are blue, _B'_ is earlier than _B_ and _B'.BlueHeight_ + _k_ &lt; _B.BlueHeight_ then _B'_ ≤ _B_

**Proof**: Take a path of length _k_ + 1 from _B_ towards _Genesis_ where each step decreases the height by 1.

All of the nodes along this path have blue height at least _B'.BlueHeight_ so they can not be in _B'.Past_.

If one of them is in _B'.Future_ then _B'_ ≤ _B_. Else, they are all in _B'.Anticone_ contradicting _k_-connectedness □

Hence, as before, we have the following algorithm:

```text
Input: two (hashes of) blue blocks u, v
Output: TRUE iff there's a path from v to u (i.e. if u is in Past(v))
 
If v appears before u in the topological order return FALSE
If v.BlueHeight - u.BlueHeight > k return TRUE
Traverse from u down the blue leafs, terminating at nodes with BlueHeight <= u.BlueHeight. Return TRUE if u is found or FALSE if the search is exhausted
```

### Red-Blue Reachability Queries <a id="TheReachabilityProblem-Red-BlueReachabilityQueries"></a>

As stated above, this method can be extended to red-blue queries, albeit with larger constants.

Let us redefine the blue height for all blocks:

* _Genesis.BlueHeight_ = 0
* If _B_ is blue then 

![](../../.gitbook/assets/image%20%2832%29.png)

* If _B_ is red then 

![](../../.gitbook/assets/image%20%2834%29.png)

Note that we are traversing _B.Parents_ rather than _B.BlueLeafs_. However, read blocks do not increase the blue height so the blue height of blue nodes is the same in both definitions.

**Proposition**: If _B_ is red, _B'_ is blue, _B'_ is earlier than _B_ and _B'.BlueHeight_ + _k_ &lt; _B.BlueHeight_ then _B'_ ≤ _B_

**Proof**: Let _B\*_ be the highest blue reachable from _B_, then _B\*.BlueHeight_  = _B.BlueHeight_ and the proposition from the blue-blue case shows that _B'_ ≤ _B\*_, hence _B'_ ≤ _B_ □

This means that, yet again, a large blue height can immediately imply reachability.

What about the other case? This could also be solved, albeit slower than in the blue-blue case, but still in complexity constant in _n_. This is because any blue block reachable from a read block is also reachable from one of its blue leaves. So we have to repeat the blue-blue method for each blue leaf independently.

This has a worst case complexity of _O_\(_k_^4\).

### The Final Query Algorithm <a id="TheReachabilityProblem-TheFinalQueryAlgorithm"></a>

Assuming we maintain blue height for all blocks as described above

```text
Input: two (hashes of) blocks u, v
Output: TRUE if u>=v, FALSE if u||v, FAIL if the algorithm can not determine
 
If v == u return TRUE
If u is a tip return FALSE
If v appears before u in the topological order return FALSE
If v is red FAIL
If v.BlueHeight - u.BlueHeight > k return TRUE
If u is red:
    For any w in u.BlueLeaves:
        Traverse from u down the blue leafs, terminating at nodes with Height <= u.Height. Return TRUE if u is found or FALSE if the search is exhausted        
Traverse from u down the blue leafs, terminating at nodes with Height <= u.Height. Return TRUE if u is found or FALSE if the search is exhausted
```

Checking that u is a tip might be a very obvious and seemingly insignificant optimization, but it is justified below.

### Maintaining the Data <a id="TheReachabilityProblem-MaintainingtheData"></a>

Upon placing a new block _B_, assuming by induction that all current blocks have correct blue leafs, we can follow this procedure to calculate the blue leaves of the new block \(we use u &gt;= w \[u &lt;= w\] to denote that we've performed a reachability check from u to w \[from w to u\] which returned TRUE\):

```text
For v in B.Parents:
ParentBlueLeavesLoop: //label
    For u in v.BlueLeaves:
        For w in B.BlueLeaves:
            If u >= w:
                B.BlueLeaves.remove(w)
                B.BlueLeaves.insert(u)
                Continue ParentBlueLeafsLoop
            If u <= w:
                Continue ParentBlueLeafsLoop
        B.BlueLeaves.insert(u) //adding u keeps B.BlueLeaves an antichain
```

Let us analyze the complexity:

* _B_ has _O_\(_L_\) parents
* Each such parent has _O_\(_k_\) blue leaves
* Each of them is compared against all blocks currently in _B.BlueLeaves_ which is always an antichain so of size _O_\(_k_\)

All in all there are _O_\(_Lk_^2\) reachability queries, since each such query is _O_\(_k_^3\) we get a total complexity of _O_\(_Lk_^5\), yikes.

Further contemplation reveales that in a _k_-connected graph there could be at most \(_k_ + 1\)^2 nodes whose height is at most _B.BlueHeight_ but at least _B.BlueHeight_ - _k_ \(hint: the set of all blue blocks of a given blue height is an antichain\), so this slightly reduces the bound to _O_\(min{_L_ + _k_}_k_^5\).

However, some measures can be taken to reduce the constants significantly. The key is to avoid checks which we know will turn out FALSE, because these are the heaviest, hence:

* Never check if a blue leaf which is also a parent is reachable from the current node
* Never check reachability between two leafs taken from the same blue leaf set
* Skip nodes which were already traversed \(from a previous blue leaf set\) to avoid making the same query twice

To do so easily, we use two arrays, one to store the blue leafs themselves and one to store the parents they came from. We also keep a map of all vertices we already tried to include.

We use an array because we know in advance the size of _B.BlueLeaves_ is at most _k_ + 1 so hardcoding this saves us the overhead of using resizable random access data structures such as a vector.

Once we finish we dump the array into _B.BlueLeaves_.

```text
Let BlueLeafs and BlueSources be arrays of size k+1
Let UsedLeafs be a hashmap of blocks
leafCount = 0
For v in B.Parents:
ParentBlueLeavesLoop: //label
    For u in v.BlueLeaves:
        if UsedLeafs.contains(u) continue
        UsedLeafs.insert(u)
        For i = 0; i < leafCount ; i++:
            If v == BlueSources[i] or BlueLeafs[i] == BlueSources[i] Continue //the line which makes the difference
            If u >= BlueLeaves[i]:
                BlueLeaves[i] = u
                BlueSources[i] = v
                Continue ParentBlueLeavesLoop
            If u <= BlueLeafs[i] Continue ParentBlueLeavesLoop
        BlueLeaves[leafCount] = u
        BlueSources[leafCount] = v
        leafCount++
                 
         
B.BlueLeaves.insert(BlueLeafs)
```

After the blue leaves are constructed we may calculate _B.BlueHeight_ by any of the methods above.

### Redistributing Data Maintenance <a id="TheReachabilityProblem-RedistributingDataMaintenance"></a>

Even with all the optimization, the maintenance procedure might be too involved.

The good news is we do not have to do it all at once.

Imagine that we've simply added all of the blue leafs of _B.Parents_ to _B.BlueLeaves_, without trimming it to an antichain. But that all the rest of the blue leaf sets are properly trimmed.

Then _B.BlueHeight_ remained the same, and the entire approach still works.

The only adverse effect is that now the blue leaf graph has a vertex whose degree might be as high as _Lk_^2.

_B_ is only traversed in reachability queries from _B_ to a block whose blue height is at least _B.BlueHeight_ - _k_ \(and even that only if _B_ is blue\).

However, this does not mean that we can just leave the set like that, because then the adverse effect will accumulate, where each blue set will be larger by an order of magnitude by those below it.

What it _does_ mean, is that we can divide the work into steps:

* As part of the procedure of placing a new block, let _B.BlueLeaves_ be the union of the blue leaf sets of its parents
* In the background, trim _B.BlueLeaves_

The point is that queries can still be answered during the trimming process, and this has no effect on the long term runtime as long as the trimming is done before the next block is placed.

Hopefully this affords enough time to complete the trimming in a leisurely manner.

### Handling Reorgs <a id="TheReachabilityProblem-HandlingReorgs"></a>

Recall that a reorganization takes place when the selected chain has changed. We let _s_ denote the latest node in both the old and the new chains, and call it the _splitting point_.

The blue blocks from the context of the virtual nodes are simply the union of the blue sets along the selected chain, which means that one can calculate the lists of blues turned red and the set of reds turned blue, both in topological order.

This is the only data required from the reorg to update the blue leaves.

The blue leaves are then updated in two steps. First handling all blues turned red and then all reds turned blue.

We use the term _topological order_ to mean the order in which blocks were added to the graph \(this is actually an inverse topological order, but it seems that the name has stuck\).

**Step 1: blues turned red**

Assume that _B_ is a blue turned red. One now has to traverse the blocks which have _B_ as a blue leaf. For each such block _C_, one has to remove _B_ from _C.BlueLeaves_ and then insert into it all elements of _B.BlueLeaves_ which are not below an element of _C.BlueLeaves_.

Note that it is impossible for an element of _C.BlueLeaves_ to be below an element of _B.BlueLeaves_, because then it is reachable from _B_. This means we only have to query in one direction, which is nice.

To efficiently traverse all the nodes which have _B_ as a blue leaf note that:

* They are all in _B.Future_
* If _C_ ∈ _B.Future_ was blue before the reorg, then none of the elements in _C.Future_ have _B_ as a blue leaf
* If _C_ ∈ _B.Future_ and _B_ is not a blue leaf of _C_ then none of the elements in _C.Future_ have _B_ as a blue leaf

In case there are many blues turned red, we repeat the process for each one, making sure we do so in topological order.

**Step 2: reds turned blue**

The red turned blue case is easier, because it requires for less comparisons.

Assume that _B_ is a red turned blue, we traverse its future in topological order and for each block _C_ we check if _B_ is reachable from any blue leaf of _C_.  If it is we terminate. If it isn't, we add _B_ to the blue leaves and remove all blue leaves which are reachable from _B_. If _C_ is blue, we terminate after doing so.

To be more efficient, whenever we traverse the past of a changed node to update its offspring, we keep track of notes we already visited.

```text
Let BlueTurnedRed be the set of all blocks which were blue before the reorg and red after, in topological order
Let RedTurnedBlue be the set of all blocks which were red before the reorg and blue after, in topological order
Let Traversed be an empty hash map
 
func updateRed(v,w):
    If w in Traversed:
        Return
    Traversed.insert(w)
    If v not in w.BlueLeaves:
        Return
    w.BlueLeaves.remove(v):
OuterLoop:  //label
    For v' in v.BlueLeaves:
        For w' in w.BlueLeaves:
            If v' <= w':
                Continue OuterLoop
        w.BlueLeaves.insert(v')
    If (w is Blue and w not in RedTurnedBlue) or w in BlueTurnedRed:
        Return
    For w' in w.Children:
        updateRed(v,w')
 
func updateBlue(v,w):
    If w in Traversed:
        Return
    Traversed.insert(w)
    For w' in w.BlueLeaves:
        If v <= w':
            Return
        If w' <= v:
            w.BlueLeaves.remove(w')
    w.BlueLeaves.add(v)
    If w is not blue:
        For w' in w.Children:
            updateBlue(v,w')
 
 
For v in BlueTurnedRed:
    For v' in v.Children:
        Traversed.Clear()
        updateRed(v,v')
 
For v in RedTurnedBlue:
    For v' in v.Children:
        Traversed.Clear()
        updateBlue(v,v')
```

## The Chain Decomposition Method <a id="TheReachabilityProblem-theChainDecompositionMethod"></a>

This is a proposed improvement over the depth pruning approach. It does not offer any improvement for not entirely blue queries, but it might give a considerable \(constant\) speedup in the blue case.

It does not require that the set be _k_-connected, only that it has bounded width. If _A_ is an antichain and _x_ ∈ _A_ then obviously _A_ ∖ {_x_} ⊂ _Anticone_\(_x_\), so bounded anticones implies bounded width. However, in practice the width itself might be considerably smaller than _k_. Since the constants of this approach actually depend on width, this might reduce computations noticeably.

The proposed solution follows the lines of [the following article](https://arxiv.org/abs/0707.1532). Their context, however, is not suited for an online environment. In order to incrementally maintain the best chain we use the following construction, which is based on the \(true, in our case\) assumption that any block in the DAG was maximal at the time of its inclusion. [The following work](http://users.ece.utexas.edu/~garg/dist/icdcs06.pdf) proposes a highly efficient incrementally maintained optimal chain decomposition under these assumptions.

It is **not yet clear** whether this approach is suited for our needs.

### The incremental ChainMerge data structure <a id="TheReachabilityProblem-TheincrementalChainMergedatastructure"></a>

### Chain Decomposition <a id="TheReachabilityProblem-ChainDecomposition"></a>

### Answering Reachability Queries <a id="TheReachabilityProblem-AnsweringReachabilityQueries"></a>

### Maintaining a Minimal Chain Decomposition <a id="TheReachabilityProblem-MaintainingaMinimalChainDecomposition"></a>

### Handling Reorgs <a id="TheReachabilityProblem-HandlingReorgs.1"></a>

### Handling Finality <a id="TheReachabilityProblem-HandlingFinality"></a>

