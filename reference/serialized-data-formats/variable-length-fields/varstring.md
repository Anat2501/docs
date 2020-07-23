# VarString

**Variable string format**, or **varString**, is a way to represent a string of variable size by first prepending it with a [VarInt](varint.md) indicating what that size is.

This approach is favorable to null terminated strings in the context of byte streams as it allows the receiver to know the length in advance.

Since the size of the string itself can be of any length \(the largest VarInt value allows for a message of length one byte shy of 16 exabytes\), different messages might require different preparations.

Since the range of possible sizes of a VarString is 2-18446744073709551624, we just denote the length of such fields as "?".

The structure of a VarString is:

| Size in bytes | Name | Data type | Description |
| :--- | :--- | :--- | :--- |
| 1-9 | length | varInt | length of message |
| length | string | char\[\] | The \(possibly empty\) string itself |

