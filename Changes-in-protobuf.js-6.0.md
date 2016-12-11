protobuf.js 6 is a complete rewrite of the previous codebase, hence a lot has changed (to the better).

### Dependencies

* Instead of relying on bytebuffer.js, there is now a use case specific and reusable wire format reader / writer interface that abstract away any differences between node and modern to ancient browsers.
* Overall minified size is now ~58kb instead of ~96kb with bytebuffer. ~18kb gziped.
* Basic long support has been integrated into the library to fall back to (possibly unsafe) JavaScript numbers if no long library is present.
* The pbjs CLI now sets up itself only when required. Its dependencies are not pulled on `npm install protobufjs`.

### API

* Loading .proto or .json files is now all asynchronous and loads all files in parallel.
* Builder functionality has been integrated into reflection.
  Instead of calling `Builder#build`, there is now `Namespace#lookup` which returns the reflected type for a path.
* Creating message instances from reflected types is done through calling `MyType#create` instead of building classes and calling `new`.
  Additionally, plain objects can now be encoded just like runtime messages and you can now inherit from the internal prototype to create your own classes with custom functionality.
* Runtime messages don't have `encode` and `decode` methods anymore by default.
  These are now static methods on the reflected type respectively custom constructor and can be used with both actual runtime messages and plain objects.
* There are no more `null` values marking unset fields. Instead, default values are on the prototype now and existence on the wire can be checked with `Object#hasOwnProperty`.
* API documentation has received a major overhaul.

### Performance

* Reflection has built-in code generation which makes encoding and decoding of messages blazingly fast.
* The new Reader/Writer interface sets up itself according to the environment to reduce redundant conditionals

### Compatibility

* Common Google types within `any.proto`, `duration.proto`, `timestamp.proto`, `empty.proto` and `struct.proto` are now integrated.
* The build-process has been changed to gulp/browserify for seamless integration into your own project.
* TypeScript definitions are now automatically generated and the API is built in a way that makes it easier to work with typed dialects (i.e. working with typed reflection objects instead of just magically built messages).

### Other

* It's now easier to generate static code. There is actually a `static` target for `pbjs` now as a starting point.