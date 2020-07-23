# Registry Transaction

A registry transaction, or a subnetwork registration transaction, is a transaction created to register a new [subnetwork](../../glossary.md#subnetwork). A registry transaction struct is the same as a [regular \(native\) transaction](./) struct.

A registry transaction identifies the [Kaspa Registry Subnetwork](../../components/kaspad-full-node/reference/subnetworks-1.md#kaspa-registry-subnetwork) \(subnetwork 2\) in its `subnetwork ID` field. Its `payload` field should include special instructions for the new subnetwork, such as gas limit per block. The resulting hash of this transaction is the subnetwork ID of the newly created subnetwork.

