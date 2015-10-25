*Note:* You'll need the full build to load .proto data. NOPARSE builds are able to load JSON only.

#### Loading .proto files

To load a .proto file, use:

**API:** `ProtoBuf.loadProtoFile(source[, callback[, builder]]):Builder|undefined`

```js
// Synchronously
var builder = ProtoBuf.loadProtoFile("path/to/file.proto");

// Asynchronously
ProtoBuf.loadProtoFile("path/to/file.proto", function(err, builder) {
    ...
});
```

`ProtoBuf.loadProtoFile` also accepts an object specifying the import root directory and the file to load as its first parameter: `{root: string, file: string}`. Additionally, an already created and then reused builder can be specified as the last argument, which is useful if all the definitions shall reside in a single namespace.

#### Loading .proto strings

**API:** `ProtoBuf.loadProto(source[, builder][, filename]):Builder`

```js
var builder = ProtoBuf.loadProto(...protoString..., "myproto.proto");
```

Loading .json files created manually or through pbjs [is also possible](https://github.com/dcodeIO/protobuf.js/wiki/Builder#using-json-without-the-proto-parser).

#### Loading JSON files and strings

To load the (raw) JSON counterpart generated through [pbjs](https://github.com/dcodeIO/protobuf.js/wiki/pbjs), use `ProtoBuf.loadJsonFile` respectively `ProtoBuf.loadJson`. It's the same API.

If you generated classes or modules with it, loading is done just by including respectively requiring the resulting file. Loading is handled transparently in this case.

When using JSON only, you can use [protobuf-light.js 
or protobuf-light.min.js](https://github.com/dcodeIO/protobuf.js/tree/master/dist) instead, which do NOT include the `ProtoBuf.DotProto` package for parsing and are therefore smaller.

**Next:** Learn more about [using the Builder](https://github.com/dcodeIO/protobuf.js/wiki/Builder)