ProtoBuf.js 2 uses lots of functionality from [ByteBuffer.js 2](https://github.com/dcodeIO/ByteBuffer.js), like an easier way to encode/decode to/from base64 and hex:

* `ProtoBuf.Builder.Message#encodeAB` Encodes to an ArrayBuffer  
* `ProtoBuf.Builder.Message#toArrayBuffer` Alias for encodeAB
* `ProtoBuf.Builder.Message#encodeNB` Encodes to a node Buffer  
* `ProtoBuf.Builder.Message#toBuffer` Alias for #encodeNB  
* `ProtoBuf.Builder.Message#encode64` Encodes to a base64 encoded string  
* `ProtoBuf.Builder.Message#encodeHex` Encodes to a hex encoded string  
* `ProtoBuf.Builder.Message#toString(enc)` Encodes to base64, hex, utf8 (not recommended), debug  
* `ProtoBuf.Builder.Message#decode(buffer, enc)` Decodes with the given encoding if buffer is a string  
* `ProtoBuf.Builder.Message#decode64` Decodes from base64  
* `ProtoBuf.Builder.Message#decodeHex` Decodes from hex

#### Debug output on ByteBuffer instances

ByteBuffer 1.x provided a means of debugability through its `#toHex` method that returned the hex representation of the underlying buffer *including* marked offsets. This led to some confusion and has changed. `#toHex` now returns a plain hex encoded representation while `#toString("debug")` returns what ByteBuffer 1.x previously returned for `#toHex` but without any line breaks.

Unified reporting of missing fields
-----------------------------------
Encoding of messages with missing required fields now reports this through an error containing the rest of the encoded message in the same manner as decoding does. Property names on the thrown `err` have changed to `err.encoded` (still encoded message in the specified encoding) respectively `err.decoded` (decoded message with missing required fields).

Named function shenanigans
--------------------------
Message constructors no longer are named functions as the eval() logic regularily caused trouble.

Conversion to plain JSON with proto2js
--------------------------------------
proto2js now pulls imports into the resulting JSON output instead of just returning the file names for further processing.

Services support
----------------
[see](https://github.com/dcodeIO/ProtoBuf.js/wiki/Services)

**Next:** [Feel enlightened and go back to start](https://github.com/dcodeIO/ProtoBuf.js/wiki)