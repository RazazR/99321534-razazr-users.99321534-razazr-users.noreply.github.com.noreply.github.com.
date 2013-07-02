![ProtoBuf.js - protobuf for JavaScript](https://raw.github.com/dcodeIO/ProtoBuf.js/master/ProtoBuf.png)
-----

Why ProtoBuf.js instead of, let's say, JSON?
--------------------------------------------
While JSON is already much better than XML, it still comes with a significant overhead. Imagine you are transfering object data between two nodes: Using JSON, this might look something like `{"type":"ping","time":123456789}` which is, in this example, 32 bytes large, containing 2+3+3+2+1=11 bytes of bounding characters plus 4+4+4=12 bytes of string names, making up 23/32 bytes ~= 71% redundant data for each subsequent message of this type. That's much, especially if there are hundreds of clients sending thousands of messages. ProtoBuf.js, on the other hand, efficiently serializes the data to binary without losing any information, which turns out to be 7 bytes long while still being able to distringuish between two messages of different types: `<0A 05 08 95 9A EF 3A>`

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

What about binary JSON?
-----------------------
While [BSON](http://bsonspec.org) is better than JSON, it still comes with pretty similar overhead. As of [the specification](http://bsonspec.org/#/specification), BSON always includes all the keys as cstrings like known from JSON and it also does not efficiently store integer or long values as there is no varint encoding. Looking on its hello world example: `{"hello": "world"}` -> `<16 00 00 00 02 h e l l o 00 06 00 00 00 w o r l d 00 00>`

**Bottom line:** Not requiring a schema like in JSON/BSON comes at the price of size. Furthermore, Protocol Buffers are capable of storing numeric values more efficiently.

**Next:** [Feel enlightened and go back to start](https://github.com/dcodeIO/ProtoBuf.js/wiki)