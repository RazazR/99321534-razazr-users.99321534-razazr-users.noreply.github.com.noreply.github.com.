![ProtoBuf.js - protobuf for JavaScript](https://raw.github.com/dcodeIO/ProtoBuf.js/master/ProtoBuf.png)
-----
[ProtoBuf.js](https://github.com/dcodeIO/ProtoBuf.js) is a protobuf implementation on top of [ByteBuffer.js](https://github.com/dcodeIO/ByteBuffer.js) including a .proto parser, reflection, message class building and simple encoding and decoding in plain JavaScript. There is no compilation step required, it's super easy to use and it works out of the box on .proto files.

Why ProtoBuf.js instead of, let's say, JSON?
--------------------------------------------
While JSON is already much better than XML, it still comes with a significant overhead. Imagine you are transfering object data between two nodes: Using JSON, this might look something like `{"type":"ping","time":123456789}` which is, in this example, 32 bytes large, containing about 2+3+3+2+1=11 bytes of bounding characters plus 4+4+4=12 bytes of string names, making up 23/32 bytes ~= 71% redundant data for each subsequent message of this type. That's much. ProtoBuf.js, on the other hand, serializes all the data efficiently to binary data, which is 7 bytes long while still being able to distringuish between two messages of different types: `<0A 05 08 95 9A EF 3A>`

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

Usage
-----
#### node.js / CommonJS ####

```bash
npm install protobufjs
```

```javascript
var ProtoBuf = require("protobufjs");
...
```

#### RequireJS / AMD ####

Requires [ByteBuffer.js](http://github.com/dcodeIO/ByteBuffer.js). Optionally depends on [Long.js](https://github.com/dcodeIO/Long.js)
for long (int64) support. If you do not require long support, you can skip the Long.js config. [Require.js](http://requirejs.org/)
example:

```javascript
require.config({
    ...
    "paths": {
        "Long": "/path/to/Long.js",
        "ByteBuffer": "/path/to/ByteBuffer.js",
        "ProtoBuf": "/path/to/ProtoBuf.js"
    },
    ...
});
require(["ProtoBuf"], function(ProtoBuf) {
    ...
});
```

Or as a module dependency:

```javascript
define("MyModule", ["ProtoBuf"], function(ProtoBuf) {
    ...
});
```

#### Browser ####

Requires [ByteBuffer.js](http://github.com/dcodeIO/ByteBuffer.js). Optionally depends on [Long.js](https://github.com/dcodeIO/Long.js)
for long (int64) support. If you do not require long support, you can skip the Long.js include.

```html
<script src="//raw.github.com/dcodeIO/Long.js/master/Long.min.js"></script>
<script src="//raw.github.com/dcodeIO/ByteBuffer.js/master/ByteBuffer.min.js"></script>
<script src="//raw.github.com/dcodeIO/ProtoBuf.js/master/ProtoBuf.min.js"></script>
```

```javascript
var ProtoBuf = dcodeIO.ProtoBuf;
...
```

Now, everything is set up and ready to go.

**Next:** [Learn more about using the API](https://github.com/dcodeIO/ProtoBuf.js/wiki/Builder:-Usage-&-Examples)