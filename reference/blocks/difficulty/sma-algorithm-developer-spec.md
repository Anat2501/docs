# SMA Algorithm Developer Spec

A complete explanation and motivation for the algorithm can be found [here](sma-algorithm.md).

### Constants <a id="SMAAlgorithmDeveloperSpec-Constants"></a>

_TargetBlockDelay_ = 1 \(second\)

_DifficultyAdjustmentWindowSize_ = 2640 \(blocks\)

_TimestampDeviationTolerance = 132 \(block delays\)_

_MaxTimeOffsetSeconds = TimestampDeviationTolerance_ \* _TargetBlockDelay_

### Algorithm <a id="SMAAlgorithmDeveloperSpec-Algorithm"></a>

_blockWindow\(b block, size int\):_

1. return last _size_ blocks in b.Past by [PHANTOM](../../consensus/) order

_calcBlockTarget\(b block\):_

1. _bluestParent = b.parents.bluest\(\)_
2. _DifficultyAdjustmentWindow = blockWindow\(bluestParent, DifficultyAdjustmentWindowSize_\)
3. _AdjustmentFactor_ = \(_DifficultyAdjustmentWindow.MaxTimestamp - DifficultyAdjustmentWindow_.MinTimestamp\) / \(_TargetBlockDelay \* DifficultyAdjustmentWindowSize_\)
4. return _DifficultyAdjustmentWindow_.AverageTarget \* AdjustmentFactor

### Changes to Block Acceptance Rules <a id="SMAAlgorithmDeveloperSpec-Changestoblockacceptancerules"></a>

Do not accept block _b_ if any of the following is true:

1. Block in the future _b.Timestamp_ - _systemClock.Now_ &gt; _MaxTimeOffsetSeconds_
   1. In this case, don't reject, but rather delay until it's time is acceptable
2. Block in the past: _b.Timestamp &lt; blockWindow\(b, 2 \* TimstampDeviationTolerance - 1\)_.MedianTimestamp

