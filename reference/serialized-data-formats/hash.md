# Hash

A hash in Kaspa always refers to the output of a double [SHA256](https://en.bitcoin.it/wiki/SHA-256) on some input data.

```text
SHA256(SHA256(input))
```

It is serialized as a 32 long array of ubytes.

| **Bytes** | **Field** | **Data Type** | **Description** |
| :--- | :--- | :--- | :--- |
| 32 | Hash | Array of ubyte \(uint8\) | Double SHA256 hash. |

## Example <a id="Example"></a>

16 0A C6 8B 77 08 F4 96 A3 07 05 BC 92 DA EE 73  
26 5E D0 85 78 A2 5D 02 49 8A 2A 22 EF 41 C9 C3

ubyte hash\[0\] = 0x16 = 22

ubyte hash\[1\] = 0x0A = 10

…

ubyte hash\[31\] = 0xC3 = 195

