#### Loading .proto files

You'll need the full build to load .proto files. To load a .proto file, use:

```js
// Synchronously
var builder = ProtoBuf.loadProtoFile("path/to/file.proto");

// Asynchronously
ProtoBuf.loadProtoFile("path/to/file.proto", function(err, builder) {
    ...
});
```

`ProtoBuf.loadProtoFile` also accepts an object specifying the import root directory and the file to load as its first parameter: `{root: string, file: string}`. Additionally, an already created and then reused builder can be specified as the last argument, which is useful if all the definitions shall reside in a single namespace.

Loading .json files created manually or through proto2js [is also possible](https://github.com/dcodeIO/ProtoBuf.js/wiki/Builder#using-json-without-the-proto-parser).