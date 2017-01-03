Let's say you have the following buffer and that you want to know what's wrong with it:

`Buffer <0a 0a 32 08 08 07 10 00 18 1a 20 00 0a 08 22 06 08 02 10 19 18 03 0a 09 22 07 08 02 10 a2 03 18 20 0a 09 22 07 08 02 10 8d 02 ...>`

First you need to know whether the message is length delimited or not. If it is length delimited, the first byte(s) would specify the message's total length as a **varint**. Otherwise the message starts with the first field's **tag** directly.

### How to decode a varint?
Varints occupy 1 to 10 bytes to efficiently encode integer values. The most significant bit of each byte indicates whether more bytes are following (msb=1, | 0x80) or not (msb=0, & 0x7f). Most of the time the actual value isn't of importance when just reverse engineering a standard buffer because the varint is either just 1 byte long or the value isn't of importance. If not important you can just skip all bytes with the msb set plus one. If it's just 1 byte, you can just take the byte as the value (msb isn't set anyway). Otherwise you can calculate the value by taking the other 7 bits and add each byte's value by shifting it by 0, 7, 14, 21 etc. first. In code:
```js
var i = 0;
while (buffer[pos] & 0x80)
    value |= (buffer[pos++] & 0x7f) << i++ * 7;
```

Back to our buffer: In our case the message isn't length delimited, hence the message starts with the first field's tag.

### Tags
Tags are encoded as a varint. The last 3 bits of the tag's value represent the wire type, all other bits except the last 3 represent the field id.

Let's start decoding:

```
0a	convert to binary: 1010
  	note that the msb isn't set here, hence it is a varint of just 1 byte
  	split into id and wireType: 1 | 010
  	convert to decimal: id = 1, wireType = 2
```

### So, what are those wire types?

Value     | Wire type   | Size                           | Possible types
----------|-------------|--------------------------------|---------------
0         | varint      | 1 to 10 bytes                  | int32, int64, uint32 etc.
1         | fixed64     | 8 bytes (little endian)        | fixed64, sfixed64, double
2         | ldelim      | varint length + length * bytes | string, bytes, (inner) messages
3         | start_group | N/A                            | N/A
4         | end_group   | N/A                            | N/A
5         | fixed32     | 4 bytes (little endian)        | fixed32, sfixed32, float

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
09	length = 9

22 07 08 02 10 a2 03 18 20

0a	id = 1, wireType = 2
09	length = 9

22 07 08 02 10 8d 02 ...
```

Unfortunately, that's all the bytes printed to console, so the next step would be to get the complete buffer and continue here OR to make a guess what actual type the length delimited chunks above could be. We already know it must be a repeated field because the same id is used multiple times. We also learned from the table above that the ldelim wire type is either encoding a string, raw bytes or an inner message. When looking at the chunks above, these do not appear to be strings, hence it makes sense to check for inner messages.

Let's start with the first:

```
32	110 | 010 = id 6, wireType 2
08	length = 8

08 07 10 00 18 1a 20 00
```

Just as we assumed, it appears that the chunks are inner messages. Again, the inner inner chunks do not appear to be strings, so let's assume another level of nesting:

```
08	1 | 000 = id 1, wireType 0
  	remember: wire type 0 is a varint
07	value 7

10	10 | 000 = id 2, wireType 0 (varint)
00	value 0

18	11 | 000 = id 3, wireType 0 (varint)
1a	value 26

20	100 | 000 = id 4, wireType 0 (varint)
00	value 0
```

So far the buffer looks ok and appears to match our assumptions on inner messages. Something else must be wrong. Either other inner messages are invalid or there is an error even farther down the road.

Next steps: Either obtain more data or validate the other inner chunks. You got the drill. Happy reverse engineering!

P.S.: This technique can also be used to build a protobuf definition for unknown data. In our case, this is what we have learned so far:

```protobuf
message Outer {
   repeated Inner1 inner1 = 1;
}

message Inner1 {
   Inner2 inner2 = 6;
}

message Inner2 {
   uint32 value1 = 1; // could also be int32, sint32, int64 etc.
   uint32 value2 = 2;
   uint32 value3 = 3;
   uint32 value4 = 4;
}
```

As you see, the exact type of each field must be guessed because a single wire type can refer to multiple value types.

A few additional examples are available starting with and referenced within [this issue](https://github.com/dcodeIO/protobuf.js/issues/55).