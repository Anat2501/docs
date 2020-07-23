# Block Validation

The following is a list of actions involved in Kaspa [block](./) validation:

* Check if block already exists in the [DAG](../blockdag/) or as an orphan \(a temporary state in which a block's parents are not yet found\) - if so, no further action required
* Check block sanity:
  * Check block header sanity - check proof of work, check that block parents order is based on hashes, check that timestamp is in allowed range
  * Confirm there is at least one transaction in the block
  * Confirm that it is under the byte size limit of a block 
  * Confirm that the first transaction is a coinbase transaction
* Validate each transaction in the block:
  * Determine whether or not a transaction is finalized
  * Determine whether or not a previous transaction [outpoint](../txo/outpoint.md) is set
  * Perform a series of checks on the inputs to a transaction to ensure they are valid, e.g. verifying all inputs exist, ensuring the block reward seasoning requirements are met, detecting double spends, validating all values and fees are in the legal range and the total output amount doesn't exceed the input amount. While checking the inputs, calculate total transaction fees
  * Determine if a transaction's sequence locks have been met, meaning that all the inputs of the transaction have reached a blue score or time sufficient for their relative locktime maturity
  * Calculate transaction mass
  * Validate that transaction mass does not exceed limit
* Confirm block's parents exist
* Calculate block subsidy
* Calculate block mass
* Check block context - perform several validation checks on the block which depend on its position within the block DAG
  * Validate that no parent is an ancestor of another parent, and no parent is finalized
  * Check block header context - check difficulty value, validate median time
* Store block in database
* Run block through PHANTOM, yielding the block's selected parent, blue merged blocks, and blue score
* Add to DAG Index
* Connect block to DAG and confirm connecting the block doesn't violate any rules \(such as ensuring this causes no duplicate transactions\)
* Send notifications

