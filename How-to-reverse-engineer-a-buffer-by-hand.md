Let's say you have the following buffer and that you want to know what's wrong with it:

`0a 0a 32 08 08 07 10 00 18 1a 20 00 0a 08 22 06 08 02 10 19 18 03 0a 09 22 07 08 02 10 a2 03 18 20 0a 09 22 07 08 02 10 8d 02`

First you need to know whether the message is length delimited or not. If it is length delimited, the first byte(s) would specify the message's total length as a **varint**. Otherwise, which is the case for our buffer above, the message starts with the first field's **tag**.

### How to decode a varint?
Varints occupy 1 to 10 bytes (var = variable) to encode integer values. The most significant bit of each byte indicates whether more bytes are following (msb=1, | 0x80) or not (msb=0, & 0x7f). Most of the time the actual value isn't of importance when just reverse engineering a standard buffer. In these cases you can just skip all bytes with the msb set plus one. Otherwise you can calculate the value by taking the other 7 bits (all except the msb) and add each byte's value by shifting it by 0, 7, 14, 21 etc. first. In code: `var i = 0; while (buffer[pos] & 0x80) value |= (buffer[pos++] & 0x7f) << i++*7;`

Back to our buffer: In our case the message isn't length delimited, hence the message starts with the first field's tag, which are encoded as a varint. The last 3 bits of the value represent the wire type, all other bits except the last 3 represent the field id.

Let's start decoding:

```
0a	convert to binary: 1010, split into id and wireType: 1 | 010
        convert to decimal: id = 1, wireType = 2
        note that the msb isn't set here, hence it is a varint of just 1 byte
```

### So, what are those wire types?

Value     | Wire type   | Size
----------|-------------|------
0         | varint      | 1 to 10 bytes
1         | fixed64     | 8 bytes
2         | ldelim      | varint length + length * bytes
3         | start_group | N/A
4         | end_group   | N/A
5         | fixed32     | 4 bytes

Going back to our buffer, we now know that the field uses the **ldelim** wire type, which indicates a varint length followed by this exact amount of bytes.

```
0a	id = 1, wireType = 2
0a	convert to binary: 1010 (msb not set, last byte) => length 10

32 08 08 07 10 00 18 1a 20 00

0a	id = 1, wireType = 2
08	length = 8

22 06 08 02 10 19 18 03

0a	id = 1, wireType = 2 
        we clearly have a pattern here. this is most likely a repeated field.
09      length = 9

22 07 08 02 10 a2 03 18 20

0a	id = 1, wireType = 2
09      length = 9

22 07 08 02 10 8d 02 ...
```