# Difficulty

Difficulty is the probability of a block hash being less than or equal to the [target bits](../block-header/#target-bits).

The smaller the target bits, the less likely the [block header](../block-header/) hash will be less than or equal to it, and the more difficult proof of work mining is.

### Difficulty Adjustment

Difficulty adjustment is the practice of changing the target bits so that it is either less difficult or more difficult to mine the next block.

{% hint style="info" %}
In Bitcoin, difficulty is adjusted every 10 minutes.

In Kaspa, difficulty is adjusted for each block \(approximately every second\).
{% endhint %}

Kaspa uses the simple moving average \(SMA\) difficulty adjustment approach. For more information, see [SMA Algorithm](sma-algorithm.md).

