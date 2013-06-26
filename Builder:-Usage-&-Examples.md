The Builder is probably the core component of ProtoBuf.js. It resolves all type references, performs all the necessary checks and returns ready to use classes. Can be created from a .proto file or from a JSON definition. The later does not even require the .proto parser to be included ([see](https://github.com/dcodeIO/ProtoBuf.js/wiki/Builder:-Usage-&-Examples#using-json-without-the-proto-parser)).

#### Using .proto files ####

Example: [tests/complex.proto](https://github.com/dcodeIO/ProtoBuf.js/tree/master/tests/complex.proto)

```javascript
var ProtoBuf = require("protobufjs");

var builder = ProtoBuf.protoFromFile("tests/complex.proto");
var Game = builder.build("Game");
var Car = Game.Cars.Car;

// Construct with arguments list in field order:
var car = new Car("Rusty", new Car.Vendor("Iron Inc.", new Car.Vendor.Address("US")), Car.Speed.SUPERFAST);

// OR: Construct with values from an object, implicit message creation (address) and enum values as strings:
var car = new Car({
    "model": "Rusty",
    "vendor": {
        "name": "Iron Inc.",
        "address": {
            "country": "US"
        }
    },
    "speed": "SUPERFAST" // also equivalent to "speed": 2
});

// OR: It's also possible to mix all of this!

// Afterwards, just encode your message:
var buffer = car.encode();

// And send it over the wire:
var socket = ...;
socket.send(buffer.toArrayBuffer());

// OR: Short...
socket.send(car.toArrayBuffer());
```

#### Putting multiple .proto files into a common namespace (outdated, use import instead) ####

Example: [tests/example1.proto](https://github.com/dcodeIO/ProtoBuf.js/tree/master/tests/example1.proto),
[tests/example2.proto](https://github.com/dcodeIO/ProtoBuf.js/tree/master/tests/example2.proto)

```javascript
var builder = ProtoBuf.protoFromFile("tests/example1.proto");
ProtoBuf.protoFromFile("tests/example2.proto", builder);
var root = builder.build();
var test1 = new root.Test1(123);
var test2 = new root.Test2("123");
...
```

#### Using JSON without the .proto parser ####

Example: [tests/complex.json](https://github.com/dcodeIO/ProtoBuf.js/tree/master/tests/complex.json)

```javascript
var ProtoBuf = require("protobufjs");

var builder = ProtoBuf.newBuilder();    // Alternatively:
builder.define("Game");                 // var builder = ProtoBuf.newBuilder("Game");
builder.create([
      {
          "name": "Car",
          "fields": [
              {
                  "rule": "required",
                  "type": "string",
                  "name": "model",
                  "id": 1
              },
              ...
          ],
          "messages": [
              {
                  "name": "Vendor",
                  "fields": ...,
              },
              ...
          ],
          "enums": [
              {
                  "name": "Speed",
                  "values": [
                      {
                          "name": "FAST",
                          "id": 1
                      },
                      ...
                  ]
              }
          ]
      }
]);

var Game = builder.build("Game");
var Car = Game.Cars.Car;
... actually the same as above ...
```

When using JSON only, you can use [ProtoBuf.noparse.js](https://github.com/dcodeIO/ProtoBuf.js/blob/master/ProtoBuf.noparse.js)
/ [ProtoBuf.noparse.min.js](https://github.com/dcodeIO/ProtoBuf.js/blob/master/ProtoBuf.noparse.min.js) instead, which
do NOT include the `ProtoBuf.DotProto` package for parsing and are therefore even smaller.

Encoder / Decoder
-----------------
Built into all message classes. Just call `YourMessage#encode([buffer])` respectively `YourMessage.decode(buffer)`.

#### Encoding a message ####

```javascript
...
var YourMessage = builder.build("YourMessage");
var myMessage = new YourMessage(...);
var byteBuffer = myMessage.encode();
var buffer = byteBuffer.toArrayBuffer();

// OR: Short...
var buffer = myMessage.toArrayBuffer();

var socket = ...; // E.g. a WebSocket
socket.send(buffer);
```

#### Decoding from an ArrayBuffer/ByteBuffer/node Buffer ####

```javascript
...
var YourMessage = builder.build("YourMessage");
var buffer = ...; // E.g. a buffer received on a WebSocket
var myMessage = YourMessage.decode(buffer);
```

#### Handling truncated messages
If a message is missing a required field when decoding, the library will throw an `Error` that still contains the rest
of the decoded message as its `msg` property. Example:

```javascript
try {
    var myMessage = YourMessage.decode(bufferWithMisstingRequiredField);
    ...
} catch (e) {
    if (e.msg) { // Truncated
        myMessage = e.msg; // Decoded message with missing required fields
        ...
    } else { // General error
        ...
    }
}
```
Documentation
-------------
* [Read the API docs](http://htmlpreview.github.com/?http://github.com/dcodeIO/ProtoBuf.js/master/docs/ProtoBuf.html)

**Next:** [Learn more about the Parser](https://github.com/dcodeIO/ProtoBuf.js/wiki/Parser:-Usage-&-supported-directives)