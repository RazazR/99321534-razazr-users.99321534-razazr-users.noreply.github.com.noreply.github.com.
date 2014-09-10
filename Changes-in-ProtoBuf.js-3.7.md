It [has been requested](https://github.com/dcodeIO/ProtoBuf.js/issues/183) to add [oneof](https://developers.google.com/protocol-buffers/docs/proto#oneof) support to ProtoBuf.js. This feature has been introducted with Protocol Buffers 2.6.0.

As of version 3.7.0, ProtoBuf.js is capable of using the oneof syntax:

```js
message MyMessage {
    oneof request {
        CreateRequest create = 1;
        UpdateRequest update = 2;
        DeleteRequest delete = 3;
    }
    ...
}
```

For such messages, ProtoBuf.js adds a virtual property named like the oneof to message instances (here: `MyMessage#request`), holding the name of the field that is present, or evaluates to `null` if none is set. If `MyMessage#create` is set, `MyMessage#request` evaluates to `"create"`.

**Note:** When working with oneof enclosed fields, it is highly recommended to always use `Message#set(key, value[, noAssert])` when assigning values, as this guarantees that the virtual property is adjusted and a possibly previously assigned value to another enclosed field is unset. It is possible though to assign values directly to multiple oneof enclosed fields, but this will result in all of these fields being present on the wire. The decoding side, however, will most likely discard any values but the latest, which is in the case of ProtoBuf.js the latest declared and present field.

**Example:**

```js
// Encoding side
var myMessage = new MyMessage();
myMessage.set("create", new CreateRequest(...));
// myMessage.create = CreateRequest instance
// myMessage.update = null
// myMessage.delete = null;
// myMessage.request = "create"
var bb = myMessage.encode();
```

```js
// Decoding side
var myMessage = MyMessage.decode(bb),
    which = myMessage.request;
var request = which !== null ? myMessage[which] : null;
// request = CreateRequest instance
```

Assigning a value to the virtual field has no effect on encoding, it's basically just a volatile marker.