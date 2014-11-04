ProtoBuf.js ships with the `pbjs` command line utility. With it it's possible to convert between .proto and JSON descriptors and even to generate the code required to access runtime structures as a class, an AMD module or a CommonJS module.

```
 _ |_ . _
|_)|_)|_)
|     '

CLI utility to convert between .proto and JSON syntax / to generate classes.

Usage: pbjs filename -target=FORMAT [-min] [> outFile]

General options:

  -source=FORMAT          Specifies the source format. Valid formats are:

                             json         Plain JSON descriptor
                             proto        Plain .proto descriptor

                          If omitted, the application will attempt to detect it.

  -target=FORMAT          Specifies the target format. Valid formats are:

                             amd          Runtime structures as an AMD module
                             commonjs     Runtime structures as a CommonJS module
                             js           Runtime structures
                             json         Plain JSON descriptor
                             proto        Plain .proto descriptor

                          If omitted, target format defaults to json.

  -using:NAME[=VALUE]     Specifies an option to apply to the volatile builder
                          loading the source file, e.g. convertFieldsToCamelCase.

  -min                    Minifies the output.

  -path=DIR               Adds a directory to the include path.

  -legacy                 Includes legacy descriptors from google/protobuf/ if
                          explicitly referenced.

  -quiet                  Suppresses any informatory output to stderr.

Specific to classes:

  -use:NAME[=VALUE]       Specifies an option to apply to the emitted builder
                          utilized by your program, e.g. populateAccessors

  -exports=FQN            Specifies the namespace to export. Defaults to export
                          the root namespace.

  -dependency=ProtoBuf    Library dependency to use when generating classes.
                          Defaults to 'protobufjs' for CommonJS, 'ProtoBuf' for
                          AMD modules and 'dcodeIO.ProtoBuf' for classes.
```