Command line utility
--------------------
It's also possible to transform .proto files into their JSON counterparts or to transform entire namespaces into ready-to-use message classes and enum objects by using the `proto2js` command line utility.

```
  Usage: proto2js protoFile [-class[=My.Package]|-commonjs[=My.Package]|-amd[=My.Package]] [-min] [> outFile]

  Options:

    -class[=My.Package]     Creates the classes instead of just a JSON definition.
                            If you do not specify a package, the package
                            declaration from the .proto file is used instead.

    -commonjs[=My.Package]  Creates a CommonJS export instead of just a JSON def.
                            If you do not specify a package, the package
                            declaration from the .proto file is used instead.

    -amd[=My.Package]       Creates an AMD define instead of just a JSON def.
                            If you do not specify a package, the package
                            declaration from the .proto file is used instead.

    -min                    Minifies the output.
```

So, to create a JSON definition from the [tests/complex.proto](https://github.com/dcodeIO/ProtoBuf.js/blob/master/tests/complex.proto)
file, run:

```bash
proto2js tests/complex.proto > tests/complex.json
```

Or to create classes/a CommonJS export/an AMD define for the entire `Game` namespace, run:

```bash
proto2js tests/complex.proto -class=Game > tests/complex.js
```

```bash
proto2js tests/complex.proto -commonjs=Game > tests/complex.js
# ^ The resulting code will implicitly call require("protobufjs");
```

```bash
proto2js tests/complex.proto -amd=Game.Cars > tests/complex.js
# ^ The resulting code will implicitly use ["ProtoBuf"] as its
#   declared dependencies and "Game/Cars" as the module name
```