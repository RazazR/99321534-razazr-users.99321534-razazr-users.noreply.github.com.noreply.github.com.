Since ProtoBuf.js 4.0.0 the library ships with the `pbjs` command line utility. With it it's possible to convert between .proto and JSON descriptors and even to generate the code required to access runtime structures as pure JS (classes), an AMD module or a CommonJS module.

```

 _ |_ . _
|_)|_)|_)           ProtoBuf.js v4.0.0-b3 https://github.com/dcodeIO/ProtoBuf.js
|     '

CLI utility to convert between .proto and JSON syntax / to generate classes.

Usage: pbjs <filename> [options] [> outFile]

Options:
  --help, -h        Show help  [boolean]
  --version, -v     Show version number  [boolean]
  --source, -s      Specifies the source format. Valid formats are:

                       json       Plain JSON descriptor
                       proto      Plain .proto descriptor

  --target, -t      Specifies the target format. Valid formats are:

                       amd        Runtime structures as AMD module
                       commonjs   Runtime structures as CommonJS module
                       js         Runtime structures
                       json       Plain JSON descriptor
                       proto      Plain .proto descriptor

  --using, -u       Specifies an option to apply to the volatile builder
                    loading the source, e.g. convertFieldsToCamelCase.
  --min, -m         Minifies the output.  [default: false]
  --path, -p        Adds a directory to the include path.
  --legacy, -l      Includes legacy descriptors from google/protobuf/ if
                    explicitly referenced.  [default: false]
  --quiet, -q       Suppresses any informatory output to stderr.  [default: false]
  --use, -i         Specifies an option to apply to the emitted builder
                    utilized by your program, e.g. populateAccessors.
  --exports, -e     Specifies the namespace to export. Defaults to export
                    the root namespace.
  --dependency, -d  Library dependency to use when generating classes.
                    Defaults to 'protobufjs' for CommonJS, 'ProtoBuf' for
                    AMD modules and 'dcodeIO.ProtoBuf' for classes.
```

**Next:** [Learn more about long (int64) support](https://github.com/dcodeIO/ProtoBuf.js/wiki/Long)