[ProtoBuf.js](https://github.com/dcodeIO/ProtoBuf.js) is a [Protocol Buffers](https://developers.google.com/protocol-buffers/docs/overview) implementation on top of [ByteBuffer.js](https://github.com/dcodeIO/ByteBuffer.js) including a .proto parser, reflection, message class building and simple encoding and decoding in plain JavaScript. There is no compilation step required, it's super easy to use and it works out of the box on .proto files.

**FAQ:**
* [Why ProtoBuf.js instead of, let's say, JSON?](https://github.com/dcodeIO/ProtoBuf.js/wiki/ProtoBuf.js-vs-JSON)
* [How to validate / decode / reverse engineer a protobuf buffer by hand?](https://github.com/dcodeIO/ProtoBuf.js/issues/55)
* [How to read binary data in the browser?](https://github.com/dcodeIO/ProtoBuf.js/wiki/How-to-read-binary-data-in-the-browser)

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

#### RequireJS / AMD

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

#### Browser

Requires [ByteBuffer.js](http://github.com/dcodeIO/ByteBuffer.js). Optionally depends on [Long.js](https://github.com/dcodeIO/Long.js)
for long (int64) support. If you do not require long support, you can skip the Long.js include.

```html
<script src="Long.min.js"></script>
<script src="ByteBuffer.min.js"></script>
<script src="ProtoBuf.min.js"></script>
```

```javascript
var ProtoBuf = dcodeIO.ProtoBuf;
...
```

Now, everything is set up and ready to go.

#### Loading .proto files

You'll need the full build to load .proto files and you may either load them from a file or as a string.

```js
// Loading from a file, assuming imports to be relative to that file
// i.e. `import "file2.proto"` will import `path/to/file2.proto`)
var builder = ProtoBuf.protoFromFile("path/to/file.proto");

// Loading from a file, overriding the import root directory for relative imports
// i.e. `import "to/file2.proto"` will import `./path/to/file2.proto`
var builder = ProtoBuf.protoFromFile({ root: "./path", file: "to/file.proto" });

// Loading from a string
var builder = ProtoBuf.protoFromString(protoString[, filename]);
// The optional filename parameter is required only if imports are used and is
// equivalent to the usage of ProtoBuf.protoFromFile
```

#### Loading .json files

For an extra boost in bandwidth utilization and speed, you may use the `proto2js` command line utility to compile your .proto files to JSON. While proto2js also provides some abstraction to create entire classes, using the plain JSON definitions behind this is also possible:

```js
var builder = ProtoBuf.newBuilder();
builder.define("my.package"[, topLevelOptions]); // Moves the pointer to the correct namespace
builder.create(...json definition...); // Processes all definitions on top of that namespace
builder.reset(); // Resets the namespace pointer to root

// Afterwards, everything is as usual
var SomeMessage = builder.build("SomeMessage");
...
```


**Next:** [Learn more about using the API](https://github.com/dcodeIO/ProtoBuf.js/wiki/Builder)