It is possible to access the internal reflection structure used by the builder. Common use cases might be to look up which fields are available inside of a message or for programmatic debugging.

**Requirements:** ProtoBuf.js >= 1.1.0

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

service MySerivce {
   rpc MyMethod(Player) returns(Player);
}
```

The general reflection structure is a tree composed of ProtoBuf.Reflect.Namespace instances and everything inside is an instance of ProtoBuf.Reflect.T. So, in the example above, there is an internal structure like:

```text
Namespace: [root]
└ Namespace: Game
  └ Message: Player
    └ Message.Field: id
    └ Message.Field: name
    └ Message.Field: type
    └ Enum: Type
      └ Enum.Field: USER
      └ Enum.Field: ADMIN 
  └ Service: MyService
    └ Service.RPCMethod: MyMethod
```

This structure is stored inside of the builder's `ns` property. The easiest way to access its elements is by using `Builder#lookup([string])` that works similar to `Builder#build([string])` but returns the reflection types instead.

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

var nameField = builder.lookup("Game.Player.name"); // instance of ProtoBuf.Reflect.Message.Field
if (nameField) {
    ...
}

var TPlayerType = builder.lookup("Game.Player.Type"); // instance of ProtoBuf.Relect.Enum

var TRoot = builder.lookup(); // instance of ProtoBuf.Reflect.Namespace
```

Types
-----
Reflection type instances have a set of shared and custom properties to work with:

* **ProtoBuf.Reflect.T**  
  Base type extended by all reflection classes.
  * `parent: ProtoBuf.Reflect.Namespace`
  * `name: string`

* **ProtoBuf.Reflect.Namespace** extends *ProtoBuf.Reflect.T*  
  A namespace wrapping a block of children.
  * `children: Array.<ProtoBuf.Reflect.T>`
  * `options: Object.<string,*>`
  * `getChildren: function([ProtoBuf.Reflect.T])` Gets children of a specific type

* **ProtoBuf.Reflect.Message** extends *ProtoBuf.Reflect.Namespace*  
  Message namespace containing message fields and enums.
  * `clazz: ProtoBuf.Builder.Message` Runtime class as returned by Builder#build

* **ProtoBuf.Reflect.Message.Field** extends *ProtoBuf.Reflect.T*  
  Message field contained in a message.
  * `required: boolean`
  * `repeated: boolean`
  * `type: {name: string, wireType: number}` Unresolved or native type
  * `id: number`
  * `options: Object.<string,*>`
  * `resolvedType: ProtoBuf.Reflect.T|null` Resolved non-native reference type

* **ProtoBuf.Reflect.Enum** extends *ProtoBuf.Reflect.Namespace*  
  Enum namespace containing enum values.
  * `object: Object.<string,number>` Runtime object as returned by Builder#build

* **ProtoBuf.Reflect.Enum.Value** extends *ProtoBuf.Reflect.T*  
  Enum value contained in an enum.
  * `id: number`

* **ProtoBuf.Reflect.Service** extends *ProtoBuf.Reflect.Namespace*  
  Service namespace containing service methods.

* **ProtoBuf.Reflect.Service.Method** extends *ProtoBuf.Reflect.T*  
  Abstract service method.
  * `options: Object.<string,*>`

* **ProtoBuf.Reflect.Service.RPCMethod** extends *ProtoBuf.Reflect.Service.Method*  
  RPC service method.
  * `requestName: string` Request message name
  * `responseName: string` Response message name
  * `resolvedRequestType: ProtoBuf.Reflect.Message` Resolved request message type
  * `resolvedResponseType: ProtoBuf.Reflect.Message` Resolved response message type

Documentation
-------------
* [ProtoBuf.Reflect API](http://htmlpreview.github.io/?http://raw.github.com/dcodeIO/ProtoBuf.js/master/docs/ProtoBuf.Reflect.html)

Please note: Ensure not to make any changes to the internal structure unless you know exactly what you are doing as this will most likely break your code otherwise.

**Next:** [Discover more pages](https://github.com/dcodeIO/ProtoBuf.js/wiki/_pages) or [Back to the repository](https://github.com/dcodeIO/ProtoBuf.js)