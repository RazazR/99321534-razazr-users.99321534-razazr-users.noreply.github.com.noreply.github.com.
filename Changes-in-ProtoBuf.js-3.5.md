As [pointed out](https://github.com/dcodeIO/ProtoBuf.js/issues/161), multiple extension fields declared in different scopes using the same name resulted in duplicate name errors.

To address this issue, extension fields now use their fully qualified name as their key:

```protobuf
message A {
    required string id = 1;
}

extend A {
    optional string id = 2; // fqn: .id
}

message B {
    extend A {
        optional string id = 3; // fqn: .B.id
    }
    ...
}
```

The recommended way of setting them is:

```js
var A = builder.build("A");
var a = new A();
a.set(".id", 234); // or a['.id'] = 234;
a.set(".B.id", 345); // or a['.B.id'] = 345;
...
```