[ProtoBuf.js](https://github.com/dcodeIO/ProtoBuf.js) is a protobuf implementation on top of [ByteBuffer.js](https://github.com/dcodeIO/ByteBuffer.js) including a .proto parser, reflection, message class building and simple encoding and decoding in plain JavaScript. There is no compilation step required and it works out of the box on .proto files.

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

API documentation
-----------------
* [Read the wiki](https://github.com/dcodeIO/ProtoBuf.js/wiki/_pages) ([start here](https://github.com/dcodeIO/ProtoBuf.js/wiki/Builder:-Usage-&-Examples))
* [Read the API docs](http://htmlpreview.github.com/?http://github.com/dcodeIO/ProtoBuf.js/master/docs/ProtoBuf.html)

Tests [![Build Status](https://travis-ci.org/dcodeIO/ProtoBuf.js.png?branch=master)](https://travis-ci.org/dcodeIO/ProtoBuf.js)
------------------
* [View source](https://github.com/dcodeIO/ProtoBuf.js/blob/master/tests/suite.js)
* [View report](https://travis-ci.org/dcodeIO/ProtoBuf.js)

Downloads
---------
* [ZIP-Archive](https://github.com/dcodeIO/ProtoBuf.js/archive/master.zip)
* [Tarball](https://github.com/dcodeIO/ProtoBuf.js/tarball/master)

**License:** [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html)