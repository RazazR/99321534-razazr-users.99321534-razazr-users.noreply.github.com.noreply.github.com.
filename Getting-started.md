#### Loading .proto files

You'll need the full build to load .proto files. To load a .proto file, use:

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

Loading .json files created manually or through proto2js [is also possible](https://github.com/dcodeIO/ProtoBuf.js/wiki/Builder#using-json-without-the-proto-parser).

#### Loading JSON files and strings

To load raw JSON files generated through [proto2js](https://github.com/dcodeIO/ProtoBuf.js/wiki/proto2js), use `ProtoBuf.loadJsonFile` respectively `ProtoBuf.loadJson`. It's the same API.

If you generated classes or modules with it, loading is done just by including respectively requiring the resulting file. Loading is handled transparently in this case.

When using JSON only, you can use [ProtoBuf.noparse.js](https://github.com/dcodeIO/ProtoBuf.js/blob/master/ProtoBuf.noparse.js)
/ [ProtoBuf.noparse.min.js](https://github.com/dcodeIO/ProtoBuf.js/blob/master/ProtoBuf.noparse.min.js) instead, which do NOT include the `ProtoBuf.DotProto` package for parsing and are therefore even smaller.

**Next:** Learn more about [using the Builder](https://github.com/dcodeIO/ProtoBuf.js/wiki/Builder)