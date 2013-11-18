Advanced options
================

Convert fields to camelCase
---------------------------
ProtoBuf.js 1.5.1 introduces the `convertFieldsToCamelCase` option on the ProtoBuf namespace. When set to `true` prior to parsing, it will convert field names from underscore notation (`some_field`) to camel case (`someField`). Fields on a message will be accessed through its camel case notation, like it's common in JavaScript, in that case.

When there is a collision, let's say there are two field named `some_field` *and* `someField` ProtoBuf.js will keep the original names.

**Next:** [Feel enlightened and go back to start](https://github.com/dcodeIO/ProtoBuf.js/wiki)