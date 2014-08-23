#### Loading .proto files

You'll need the full build to load .proto files and you may either load them from a file or as a string.

```js
// Loading from a file, assuming imports to be relative to that file
// i.e. `import "file2.proto"` will import `path/to/file2.proto`)
var builder = ProtoBuf.loadProtoFile("path/to/file.proto");

// Asynchronous usage:
ProtoBuf.loadProtoFile("path/to/file.proto", function(err, builder) {
    ...
});

// Loading from a file, overriding the import root directory for relative imports
// i.e. `import "to/file2.proto"` will import `./path/to/file2.proto`
var builder = ProtoBuf.loadProtoFile({ root: "./path", file: "to/file.proto" });

// Loading from a string
var builder = ProtoBuf.loadProto(protoString[, filename]);
// The optional filename parameter is required only if imports are used and is
// equivalent to the usage of ProtoBuf.loadProtoFile
```

Loading .json files created manually or through proto2js [is also possible](https://github.com/dcodeIO/ProtoBuf.js/wiki/Builder#using-json-without-the-proto-parser).