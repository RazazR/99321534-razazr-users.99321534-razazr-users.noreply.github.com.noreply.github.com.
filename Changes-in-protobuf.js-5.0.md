For optimal compatibility with [webpack](https://github.com/webpack/webpack), distributed files for protobuf.js, bytebuffer.js (&gt;=5.0.0) and long.js (&gt;=3.0.0) have been renamed to lower case.

* If you are using the npm package, it will just keep working.

* If you are using AMD, the respective module names should be changed to lower case.

* If you are using neither of those, the &lt;script&gt;-include should be changed to reference the new lower case file. The script namespace remains `dcodeIO.ProtoBuf` etc.