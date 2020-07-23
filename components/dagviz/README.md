# DAG Visualizer & Explorer

## DAG Visualizer‌

The purpose of a BlockDAG Visualizer is to draw a visualization of the ever-growing DAG of blocks.

The visualizer should be able to perform two tasks:

* Receive a list of blocks in the DAG and draw them with the parent relations between them.
* Receive a stream of blocks, and upon receiving each block, draw it.

![](../../.gitbook/assets/image%20%2828%29.png)

The visualizer will be useful for seeing the blockDAG, for testing, simulations and for understand the speed of Kaspa's underlying architecture.‌

## Getting Started <a id="Getting-Started"></a>

This is an independent project. If you are interested in doing this project, you can come talk to the community on Discord who can help get you started.

### Getting a list of blocks <a id="Getting-a-list-of-blocks"></a>

‌To get a list of blocks, you may use Kasparov's RESTful API [GET /blocks](https://daglabs.atlassian.net/@kaspa/s/kaspa/~/drafts/-M7Ii2jeeZ1k1ozz3XXP/try-kaspa/kasparov-api-server/api/methods#blocks) method. Specifically, you can ask for the latest 100 blocks for example.‌

### Drawing blocks as a DAG <a id="Drawing-blocks-as-a-DAG"></a>

A usable package for the visualization is [d3js.org](https://d3js.org/) specifically something like Mike Bostock's [Build Your Own Graph!](https://bl.ocks.org/mbostock/929623)

To draw blocks as a DAG, you need to draw each block as a node \(a square for example\) and link blocks to their parents. As opposed to a blockchain, where each block has one parent, in the blockDAG each block has one or more parents \(in the blockDAG below block 7's parents are block 3 and block 4\). The parents of a block are listed in the `parentBlockHashes` field in the `block` object.

As a general rule, no arrow between two blocks should go forward or perpendicular. Arrows only go backwards.

![](../../.gitbook/assets/image%20%2835%29.png)

Let's say you called `GET /blocks` and got the list of blocks between t1 and t2. This is what you are able to draw:

![](../../.gitbook/assets/image%20%2823%29.png)

You should use block-parentBlock links as the primary cue for drawing the topology. Block 7's parents are blocks 3 and 4. Therefore, blocks 3 and 4 should precede block 7 in at least one "step" \(offset\) in the drawing. Block 8's parents are blocks 4 and 6. Therefore, blocks 4 and 6 should precede block 8. The same goes for block 9 and its parents, blocks 5 and 6. Imagine two imaginary columns for drawing the blocks. A first column for drawing blocks 3, 4, 5 & 6 and a second column for drawing blocks 7, 8 & 9.

![](../../.gitbook/assets/image%20%2822%29.png)

Let's assume that block 9's parents were blocks 5 and 8 \(instead of 5 and 6\). Under that assumption, a third imaginary column would be needed to host drawing block 9, because block 9 could not be in the same imaginary column as block 8, which it points to. In this situation, block 9 would be drawn as follows:

![](../../.gitbook/assets/image%20%2830%29.png)

#### Using timestamps to add jitter \(low priority\)‌ <a id="Using-timestamps-to-add-jitter-(low-priority)&#x200C;"></a>

The timestamp inside each block can be used as a secondary visualization cue, to add jitter \(a small offsets\) between blocks in the same imaginary column. If each imaginary column has some padding,

![](../../.gitbook/assets/image%20%2818%29.png)

To simply do this, here's a grid of the imaginary columns mentioned above, each one with some left and right padding. You could take only the blocks needed to be drawn within each column, and draw them such that the block with the earliest timestamp is closest to the left padding and the block with the latest timestamp is closest to the right padding. See blocks 0 through 8 in the illustration below.

![](../../.gitbook/assets/image%20%2817%29.png)

Timestamps inside blocks are reported by the miner. Time might not be synchronized between different miners, and block timestamps cannot be relied on completely. It is possible for example, that a child block's timestamp is earlier than its parent block.‌

### Getting older blocks <a id="Getting-older-blocks"></a>

‌In the [previous step](https://daglabs.atlassian.net/@kaspa/s/kaspa/~/drafts/-M7Ii2jeeZ1k1ozz3XXP/community/contribute/community-plan/dag-visualizer#getting-a-list-of-blocks), you got a list of blocks using Kasparov's RESTful API [GET /blocks](https://daglabs.atlassian.net/@kaspa/s/kaspa/~/drafts/-M7Ii2jeeZ1k1ozz3XXP/try-kaspa/kasparov-api-server/api/methods#blocks) method. Specifically, you asked for the latest 100 blocks.

In the reply to each `GET /blocks` request, you get a field called `total` which is the total number of blocks that answered the request call query criteria. Using the `total` from the previous call, you can keep requesting older blocks, by calling `GET /blocks` again, and skipping a number of blocks equal to the `total` from the previous call, minus 100 \(the previous "page" size\).

When needing to scroll the blockDAG drawing to view older blocks, you can make the request described above, and add the blocks in the new response to the blocks already drawn, linking them. Similarly, when needing to scroll to newer blocks, you can make a request for blocks in the opposite direction.

### Getting blocks as a stream <a id="Getting-blocks-as-a-stream"></a>

To get a stream of blocks, as they are created, you may use the [Kasparov API Server](https://daglabs.atlassian.net/@kaspa/s/kaspa/~/drafts/-M7Ii2jeeZ1k1ozz3XXP/try-kaspa/kasparov-api-server) and subscribe to [block notifications MQTT topic](https://daglabs.atlassian.net/@kaspa/s/kaspa/~/drafts/-M7Ii2jeeZ1k1ozz3XXP/try-kaspa/kasparov-api-server/api/mqtt-topics#dag-blocks). This should give a notification of type Block whenever Kasparov becomes aware of a new block added to the blockDAG.

You can then draw each block as it becomes available, and place it on the canvas first according to its parent relations. It can be drawn when it was received, but it should be drawn at least after the imaginary column of its latest parent block. The rule is that no arrow between two blocks should go forward or perpendicular.

## Next Steps <a id="Next-Steps"></a>

Blocks have additional attributes that are worth visualizing, such as confirmations, being a blue or a red block, being part of the selected parent chain, block byte-size, mass, fee to transaction amount ratio, etc.

Blocks can be clicked to show included transactions.

The DAG visualizer backend can be used as a base for creating a block explorer.

## Challenges <a id="Challenges"></a>

### Performance <a id="Performance"></a>

With many shapes to draw, it is easy to overburden the user interface with drawing too many shapes \(blocks and links\), that can eventually cause a performance drop.

This can be overcome with a hack of recycling shapes. To do this, the visualizer could have a maximum number of shape containers. Container are used to draw blocks and links to parent blocks. When all the containers have been used, containers used by old blocks gone out of the viewport are recycled in favor of new blocks coming into the viewport\).

### Deciding the Correct Position on the Time Axis <a id="Deciding-the-Correct-Position-on-the-Time-Axis"></a>

Blocks have the following cues that can help decide how to position them on the time axis:

* The block's parents - the arrows to previous blocks it points to
* The block's timestamp - a time given to the block by its miner

Timestamp is an inaccurate thing to rely on when deciding where to position blocks because there is no globally synced clock between all mining entities. A block may even sometimes have a timestamp that is earlier than its parents.

Just to be clear on that, a block is still valid from the perspective of a validating node's if its reported timestamp is:

* 2\*132 seconds in the past
* 132 seconds in the future
* Get correct past timing
* Insert an illustration

The main visualization cue should come from parent-child block relations, or in other words from the topology of the blockDAG. A block's direct parent should always appear at least one "step" earlier on the time axis than its child blocks, regardless of the block's reported timestamp.

Blocks' reported timestamps can still be used as a secondary visualization cue, to add jitter from the exact position on the time axis, and to visualize the variable jitter between blocks mined simultaneously on parallel branches.

* Illustrate the jitter

### Disconnected SubDAGs <a id="Disconnected-SubDAGs"></a>

When getting a chunk of blocks and drawing them, it is possible to have two or more separate branches \(sub sections of the DAG\), with a common ancestor and/or a common descendant that are outside of the current viewport. The result is what seems like two separate disconnected blockDAGs. It can happen when the visualizer obtains a list of blocks and either the common ancestor block and/or the common descendant block are outside of the obtained list of blocks or the blocks currently viewable in the UI.

‌This causes two potential problems:

‌Suboptimal branch ordering

* Varying length parallel branches

#### Sub-Optimal Branch Ordering <a id="Sub-Optimal-Branch-Ordering"></a>

It is not clear in which order to draw these parallel sub-sections of the DAG that are disconnected from each other. The arbitrary order that they were drawn in might need to be changed, when the blocks that eventually connects them come into view, or the links will cross one over the other, which might be avoided if the order between the branches were different to begin with.

There are several ways to address this problem:

‌Quickly rearrange branches, when needed, to avoid tangles or to reduce tangles to a minimum, and have a neatly drawn DAG.

* Keep the branches in their original drawn position, and have links cross each other \(i.e. not doing anything\).

‌**Parallel Branches of Varying Length**

‌If the branches themselves are of varying length of blocks, then when when the blocks that eventually connects them come into view, the links might be longer than normal and the drawing might look weird.

‌Illustrate parallel branches of varying length before having a common descendant, and then after having a common descendant.

There are several ways to address this problem:

‌Use physics on the links to rearrange the blocks' position so that the links within each branch are ultimately of similar length.

* Keep the blocks in their original drawn position, and have longer links \(i.e. not doing anything\).

