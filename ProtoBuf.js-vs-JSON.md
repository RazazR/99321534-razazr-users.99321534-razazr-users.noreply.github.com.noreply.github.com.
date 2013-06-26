![ProtoBuf.js - protobuf for JavaScript](https://raw.github.com/dcodeIO/ProtoBuf.js/master/ProtoBuf.png)
-----

Why ProtoBuf.js instead of, let's say, JSON?
--------------------------------------------
While JSON is already much better than XML, it still comes with a significant overhead. Imagine you are transfering object data between two nodes: Using JSON, this might look something like `{"type":"ping","time":123456789}` which is, in this example, 32 bytes large, containing 2+3+3+2+1=11 bytes of bounding characters plus 4+4+4=12 bytes of string names, making up 23/32 bytes ~= 71% redundant data for each subsequent message of this type. That's much. ProtoBuf.js, on the other hand, efficiently serializes the data to binary without losing any information, which turns out to be 7 bytes long while still being able to distringuish between two messages of different types: `<0A 05 08 95 9A EF 3A>`

What's the difference to C structs?
-----------------------------------
When transfering C structs, there will always be a fixed amount of bytes, even if, let's say a 32 bit integer value, does not require the full 4 bytes to represent its information (e.g. a value of 1). As opposed to this, ProtoBuf.js uses base 128 variable-length integers known as "varints", making the smalles values just one byte large. Additionally, there are fixed length data types available. Furthermore, a protobuf encoded message can easily be extended with more values without breaking existing code while any change to a C struct will usually render old code incompatible.

Example
-------
```protobuf
// PingExample.proto
message Message {
    optional Ping ping = 1;
    optional Pong pong = 2;
    
    message Ping {
        required uint32 time = 1;
    }
    
    message Pong {
        required uint32 time = 1;
    }
}
```

```js
var builder = ProtoBuf.protoFromFile("PingExample.proto"); // Load .proto file
var Message = builder.build("Message"); // Build the Message namespace
var msg = new Message(); // Create a new Message
msg.ping = new Message.Ping(123456789); // Set the ping property
var bb = msg.encode(); // Encode
// bb.printDebug(); // Print debug info: <0A 05 08 95 9A EF 3A>

... // Send it over the wire, e.g. a WebSocket, WebRTC, etc.

var msg = Message.decode(bb); // Decode
if (msg.ping) { // Handle Ping if present
   console.log("Ping: "+msg.ping.time);
   ...
}
if (msg.pong) { // Handle Pong if present (even allows multiple message types at once in this example)
   console.log("Pong: "+msg.pong.time);
   ...
}
```

Convinced already?

**Next:** [Feel enlightened and go back to start](https://github.com/dcodeIO/ProtoBuf.js/wiki)