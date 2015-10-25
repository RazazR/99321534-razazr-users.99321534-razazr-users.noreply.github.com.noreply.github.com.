protobuf.js includes a few advanced options to modify its behaviour.

Convert fields to camelCase
---------------------------
ProtoBuf.js 1.5.1 introduces an option to convert field names from underscore notation (`some_field`) to camel case (`someField`). Afterwards, fields on a message will be accessed through its camel case notation instead, like it's common in JavaScript. Getters (`get_some_field`, `getSomeField`) and setters (...) will still be present for both notations as long as there are no collisions.

To activate the option, set `ProtoBuf.convertFieldsToCamelCase = true;` prior to parsing / building.

When there is a collision, let's say there are two fields named `some_field` *and* `someField` ProtoBuf.js will prefer to keep the original names.

Skipping population of accessors
--------------------------------
If you do not require getters and setters for each individual field, you may turn this feature off by setting.
`ProtoBuf.populateAccessors = false`, which defaults to `true`. Available since ProtoBuf.js 3.2.2.

**Next:** [Feel enlightened and go back to start](https://github.com/dcodeIO/protobuf.js/wiki)