It [has been requested](https://github.com/dcodeIO/ProtoBuf.js/issues/183) to add [oneof](https://developers.google.com/protocol-buffers/docs/proto#oneof) support to ProtoBuf.js. This feature has been introducted with Protocol Puffers 2.6.0.

As of version 3.7.0, ProtoBuf.js is capable of using the oneof syntax:

```js
message MyOneOf {
    oneof my_oneof {
        uint32 id = 1;
        string name = 2;
    }
}
```

For such messages, ProtoBuf.js adds a virtual property named like the oneof to message instances, holding the name of the field that is present, or evaluates to `null` if none is set. In this case it is `MyOneOf#my_oneof`. If `MyOneOf#id` is set, `MyOneOf#my_oneof` evaluates to `"id"`.

**Note:** When working with oneof enclosed fields, it is highly recommended to always use `Message#set(key, value[, noAssert])` when assigning values, as this guarantees that the virtual property is adjusted and a possibly previously assigned value to another enclosed field is unset. This is what happens otherwise: It is possible to assign values directly to multiple oneof enclosed fields, but this will result in all of these fields being present on the wire. The decoding side, however, will discard any values but the latest.

**Example:**

```js
// Encoding side
var myOneOf = new MyOneOf();
myOneOf.set("id", 123);
// myOneOf.id = 123
// myOneOf.name = null
// myOneOf.my_oneof = "id"
var bb = myOneOf.encode();
```

```js
// Decoding side
var myOneOf = MyOneOf.decode(bb),
    which = myOneOf.my_oneof;
var my_oneof = which !== null ? myOneOf[which] : null;
// my_oneof = 123
```

Assigning a value to the virtual field has no effect on encoding, it's basically just a volatile marker.