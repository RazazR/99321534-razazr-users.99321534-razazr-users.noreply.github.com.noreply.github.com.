node.js / CommonJS
------------------

```bash
npm install protobufjs
```

```javascript
var ProtoBuf = require("protobufjs");
...
```

RequireJS / AMD
---------------

Requires [ByteBuffer.js](http://github.com/dcodeIO/ByteBuffer.js). Optionally depends on [Long.js](https://github.com/dcodeIO/Long.js)
for long (int64) support. If you do not require long support, you can skip the Long.js config. [Require.js](http://requirejs.org/)
example:

```javascript
require.config({
    ...
    "paths": {
        "Long": "/path/to/Long.js",
        "ByteBuffer": "/path/to/ByteBufferAB.js",
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

Browser
-------

Requires [ByteBuffer.js](http://github.com/dcodeIO/ByteBuffer.js). Optionally depends on [Long.js](https://github.com/dcodeIO/Long.js)
for long (int64) support. If you do not require long support, you can skip the Long.js include.

```html
<script src="Long.min.js"></script>
<script src="ByteBufferAB.min.js"></script>
<script src="ProtoBuf.min.js"></script>
```

```javascript
var ProtoBuf = dcodeIO.ProtoBuf;
...
```

**Next:** [Getting started](https://github.com/dcodeIO/ProtoBuf.js/wiki/Getting-started)