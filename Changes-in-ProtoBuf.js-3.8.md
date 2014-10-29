It [has been requested](https://github.com/dcodeIO/ProtoBuf.js/issues/191) to change the default representation of binary data when assigning strings to bytes fields respectively converting messages to raw JSON structures.

As of version 3.8.0, ProtoBuf.js uses base64 encoding by default:

* Instead of returning buffers, `Message#toRaw([includeBinaryAsBase64=false])` now returns base64 encoded strings for binary data when `includeBinaryAsBase64` is set to `true` (was: `includeBuffers`).

* String assignments to bytes fields by default parse the string as base64.