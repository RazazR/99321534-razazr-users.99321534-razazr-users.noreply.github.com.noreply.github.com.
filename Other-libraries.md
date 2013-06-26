There are lots of other protobuf libraries for JavaScript around. Before starting ProtoBuf.js, I tried out the most promising ones and later extended this list with a few more libraries. Please note that some libraries may not have been examined in detail, so consider this my personal experience only - for what it's worth.

ThirdPartyAddOns ([see](http://code.google.com/p/protobuf/wiki/ThirdPartyAddOns))
---

### [protobuf-js](http://code.google.com/p/protobuf-js/)
Cons: Depends on ruby and prototype.js. Incomplete. 

### [protojs](https://github.com/sirikata/protojs)
Cons: Depends on ANTLR. Incomplete. 

npm ([see](https://npmjs.org/search?q=protobuf))
---

### [protobuf](https://github.com/chrisdew/protobuf)
Pros: Based on the official protoc  
Cons: Requires compilation and a compilation step. Node only.

### [node-protobuf](https://npmjs.org/package/node-protobuf)
Pros: Based on the official protoc  
Cons: Requires compilation and a compilation step. Hard to compile on Windows. Node only.

### [pomelo-protobuf](https://npmjs.org/package/pomelo-protobuf)
Pros: Seems to be somewhat popular. Split into server and client code.  
Cons: Not compatible to protoc in some cases. At least no package, long, default and enum support (incomplete).

### [protobuf.js](https://github.com/nlf/protobuf.js)
Pros: Plain JavaScript.  
Cons: Incomplete. Does not make breakfast.  

### [pbjs-compiler](https://npmjs.org/package/pbjs-compiler)
Pros: Uses Google's Closure Library (goog.proto2.Message)  
Cons: Requires a compilation step. Depends on the Closure Library (goog.proto2.Message).  

### [gpb](https://github.com/Sannis/node-gpb)
Cons: Few documentation. Incomplete.  

Found on Google
---------------
### [protobuf-for-node](http://code.google.com/p/protobuf-for-node/)
Pros: Based on the official protoc.  
Cons: Compilation step and C-knowledge required. No type checking and long support. Node only.