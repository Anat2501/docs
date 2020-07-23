# Diff UTXO Set

A diff UTXO set is an object consisting of:

1. `base` - a pointer to the [full UTXO set](full-utxo-set.md) object
2. `diff` - a [UTXO diff](../utxo-diffs/) needed to apply to the full UTXO set in order to get the diff UTXO set.

```text
Diff_UTXO_Set(B) = [base, diff]    -- or...
Diff_UTXO_Set(B) = [*Full_UTXO_Set, UTXO_Diff]
```

A diff UTXO set object has an identical API to the full UTXO set object.

