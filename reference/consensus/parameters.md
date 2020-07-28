# Parameters

[PHANTOM](./) defines and uses several parameters:

## lambda λ

λ is the [block](../blocks/) creation rate \(blocks per second\).

## k

k is a parameter set by the [PHANTOM protocol](./). It is a function of [D\_max](parameters.md#d_max) and [lambda](parameters.md#lambda-l). It represents an upper bound over the number of blocks created in parallel.

## D

D is the propagation delay - the time it takes a block to propagate to all miners in the network. It is a function of the block size.

## D\_max

D\_max is an _a priori_ upper bound over [D](parameters.md#d), set in the protocol either explicitly \(hard-coded\) or implicitly through another parameter.

