# VarInt

**Variable integer format**, or **varInt**, is a way to represent an unsigned number in a variable number of bytes.

VarInts are mostly used in serialized data that is transmitted on the network, in order to reduce the number of transmitted bytes. It typically appears as the length of an upcoming list of transactions.

This allows us to only use one byte in case there are few transactions, but up to 9 bytes in case they are numerous.

When reading a field of type VarInt, the first byte tells us how long the integer is.

If the first byte is at most `0xfc`, then the value of the field is the value stored in this byte. Otherwise, the value tells us how many of the following bytes are to be taken as the value, according to the following rules:

| First Byte | Value Bytes | Range \(Hex\) | VarInt Example | Numerical value Example |
| :--- | :--- | :--- | :--- | :--- |
| &lt;= 0xfc | 1 | 0x0 - 0xfc \(![](https://latexmath.bolo-app.com/render/1.0/img/1210ad5e8271a8c79512d8b3100f37b1)\) | 0xb3 | 0xb3 |
| 0xfd | 2-3 | 0x0 - 0xffff \(![](https://latexmath.bolo-app.com/render/1.0/img/e537dce9151cc9628ecf21c22c0403ae)\) | 0x**fd**fa25 | 0xfa25 |
| 0xfe | 2-5 | 0x0 - 0xffffffff \(![](https://latexmath.bolo-app.com/render/1.0/img/5df4b6a1443f489b2106b28d519305cb)\) | 0x**fe**3f5a95 | 0x3f5a95 |
| 0xff | 2-9 | 0x0 - 0xffffffffffffffff \(![](https://latexmath.bolo-app.com/render/1.0/img/fd16610706a3023c40efbf13aca9886b)\) | 0x**ff**a4820f9b039f8c0d | 0xa4820f9b039f8c0d |

This format has the manifest advantage that you do not have to look back when evaluating a stream.

