Upon [request](https://github.com/dcodeIO/ProtoBuf.js/issues/115) the convenience methods `Message#encodeDelimited([bb])` and `Message.decodeDelimited(bb)` have been added, which encode respectively decode a varint32 length-delimited message.

Because this is usually used to encode multiple messages into one ByteBuffer, `Message.encode` no longer implicitly flips the destination ByteBuffer if it is explicitly specified:

```js
var MyMessage = builder.build("MyMessage");
var myMessage = new MyMessage(...);

// If omitted, nothing has changed:
var bb = myMessage.encode(); // 2.0: flips, 2.2: flips

// If explicitly specified, it does not flip anymore so that
// multiple messages can easily be written into one ByteBuffer:
var bb = new ByteBuffer();
myMessage.encode(bb); // 2.0: flips: 2.2: does not flip
```