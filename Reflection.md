![ProtoBuf.js - protobuf for JavaScript](https://raw.github.com/dcodeIO/ProtoBuf.js/master/ProtoBuf.png)
-----

It is possible to access the internal reflection structure used by the builder. Common use cases might be to look up which fields are available inside of a message or for programmatic debugging.

Let's look at the following example:

```protobuf
package Game;

message Player {
   required int32 id = 1;
   optional string name = 2;
   required Type type = 3 [default=USER];

   enum Type {
      USER = 1;
      ADMIN = 2;
   }
}
```

The general reflection structure is a tree composed of namespaces. So, in the example above, there is an internal structure like:

```text
Namespace: Game
  Message: Player
    Message.Field: id
    Message.Field: name
    Message.Field: type
    Enum: Type
      Enum.Field: USER
      Enum.Field: ADMIN  
```

This structure is stored inside of the builder's `ns` property and all types are instances of ProtoBuf.Reflect.T, ProtoBuf.Reflect.Message, etc.

The easiest way to access its elements is by using `Builder#lookup([string])` that works like `Builder#build([string])` but returns the reflection type instead (available since 1.0.4).

#### Usage examples:

```js
...
var builder = ProtoBuf.protoFromFile("example.proto"); // from above

var TPlayer = builder.lookup("Game.Player"); // instance of ProtoBuf.Reflect.Message

var fields = TPlayer.getChildren(ProtoBuf.Reflect.Message.Field); // instances of ProtoBuf.Reflect.Message.Field
fields.forEach(function(field) {
    if (!field.required) {
        ...
    }
});

var TPlayerType = builder.lookup("Game.Player.Type"); // instance of ProtoBuf.Relect.Enum

var TRoot = builder.lookup(); // instance of ProtoBuf.Reflect.Namespace
```

For information on properties and methods available on the reflected objects, see:
* [ProtoBuf.Reflect API docs](http://htmlpreview.github.io/?http://raw.github.com/dcodeIO/ProtoBuf.js/master/docs/ProtoBuf.Reflect.html)

Please note: Ensure not to make any changes to the internal structure unless you know exactly what you are doing as this will most likely break your code otherwise.

**Next:** [Discover more pages](https://github.com/dcodeIO/ProtoBuf.js/wiki/_pages) or [Back to the repository](https://github.com/dcodeIO/ProtoBuf.js)