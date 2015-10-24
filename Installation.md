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

Requires [bytebuffer.js](http://github.com/dcodeIO/bytebuffer.js). Optionally depends on [long.js](https://github.com/dcodeIO/long.js)
for long (int64) support. If you do not require long support, you can skip the Long.js config. [RequireJS](http://requirejs.org/)
example:

```javascript
require(["protobuf"], function(ProtoBuf) {
    ...
});
```

Or as a module dependency:

```javascript
define("MyModule", ["protobuf"], function(ProtoBuf) {
    ...
});
```

Browser
-------

Requires [bytebuffer.js](http://github.com/dcodeIO/bytebuffer.js). Optionally depends on [long.js](https://github.com/dcodeIO/long.js)
for long (int64) support. If you do not require long support, you can skip the long.js include.

```html
<!-- Order is important -->
<script src="long.min.js"></script>
<script src="bytebuffer.min.js"></script>
<script src="protobuf.min.js"></script>
```

```javascript
var ProtoBuf = dcodeIO.ProtoBuf;
...
```

**Next:** [Getting started](https://github.com/dcodeIO/protobuf.js/wiki/Getting-started)