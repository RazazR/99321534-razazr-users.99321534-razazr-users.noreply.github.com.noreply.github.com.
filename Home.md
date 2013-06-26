ProtoBuf.js is a protobuf implementation on top of [ByteBuffer.js](https://github.com/dcodeIO/ByteBuffer.js) including a .proto parser, reflection, message class building and simple encoding and decoding in plain JavaScript. There is no compilation step required and it works out of the box on .proto files.

Features
--------
* [CommonJS](http://www.commonjs.org/) compatible
* [RequireJS](http://requirejs.org/)/AMD compatible
* [node.js](http://nodejs.org) compatible, also available via [npm](https://npmjs.org/package/protobufjs)
* Browser compatible
* [Closure Compiler](https://developers.google.com/closure/compiler/) compatible (fully annotated, [externs](https://github.com/dcodeIO/ProtoBuf.js/tree/master/externs))
* Fully documented using [jsdoc3](https://github.com/jsdoc3/jsdoc)
* Well tested through [test.js](https://github.com/dcodeIO/test.js)
* [ByteBuffer.js](https://github.com/dcodeIO/ByteBuffer.js) is the only production dependency
* Small footprint (even smaller if you use a noparse build)