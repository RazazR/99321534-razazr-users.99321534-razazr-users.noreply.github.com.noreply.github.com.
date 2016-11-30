**NOTE:** The wiki currently contains information related to protobuf.js v5 and earlier. [protobuf.js v6](https://github.com/dcodeIO/protobuf.js) describes its API within its README and new API documentation instead.

[protobuf.js](https://github.com/dcodeIO/protobuf.js) is a [Protocol Buffers](https://developers.google.com/protocol-buffers/docs/overview) implementation on top of [bytebuffer.js](https://github.com/dcodeIO/bytebuffer.js) including a .proto parser, reflection, message class building and simple encoding and decoding in plain JavaScript. There is no compilation step required, it's super easy to use and it works out of the box on .proto files.

**Latest API changes:**
* [protobuf.js 5.0](https://github.com/dcodeIO/ProtoBuf.js/wiki/Changes-in-protobuf.js-5.0) - webpack compatibility
* [ProtoBuf.js 4.0](https://github.com/dcodeIO/ProtoBuf.js/wiki/Changes-in-ProtoBuf.js-4.0) - proto3 and pbjs
* [ProtoBuf.js 3.8](https://github.com/dcodeIO/ProtoBuf.js/wiki/Changes-in-ProtoBuf.js-3.8) - Binary in JSON
* [ProtoBuf.js 3.7](https://github.com/dcodeIO/ProtoBuf.js/wiki/Changes-in-ProtoBuf.js-3.7) - OneOfs
* [ProtoBuf.js 3.5](https://github.com/dcodeIO/ProtoBuf.js/wiki/Changes-in-ProtoBuf.js-3.5) - Extension fields
* [ProtoBuf.js 2.2](https://github.com/dcodeIO/ProtoBuf.js/wiki/Changes-in-ProtoBuf.js-2.2) - Delimited encode/decode
* [ProtoBuf.js 2.0](https://github.com/dcodeIO/ProtoBuf.js/wiki/Changes-in-ProtoBuf.js-2) - General API

**Frequently asked questions:**
* [Why protobuf.js instead of, let's say, JSON?](https://github.com/dcodeIO/ProtoBuf.js/wiki/ProtoBuf.js-vs-JSON)
* [How to validate / decode / reverse engineer a protobuf buffer by hand?](https://github.com/dcodeIO/protobuf.js/issues/55)
* [How to read binary data in the browser / under node.js?](https://github.com/dcodeIO/protobuf.js/wiki/How-to-read-binary-data-in-the-browser-or-under-node.js%3F)
* [field_name looks ugly, can it be converted to fieldName?](https://github.com/dcodeIO/protobuf.js/wiki/Advanced-options#convert-fields-to-camelcase)
* [What about google/protobuf/descriptor.proto?](https://github.com/dcodeIO/protobuf.js/tree/master/src/google/protobuf)

Installation
------------
* See: [wiki/Installation](https://github.com/dcodeIO/protobuf.js/wiki/Installation)

Getting started
---------------
* See: [wiki/Getting-started](https://github.com/dcodeIO/protobuf.js/wiki/Getting-started)

Examples
--------
* See: [examples/](https://github.com/dcodeIO/protobuf.js/tree/master/examples) and start with [protoify](https://github.com/dcodeIO/protobuf.js/tree/master/examples/protoify)

**Next:** [Learn more about using the API](https://github.com/dcodeIO/protobuf.js/wiki/Builder)