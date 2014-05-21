ProtoBuf.js also allows you to define asynchronous RPC [services](https://developers.google.com/protocol-buffers/docs/proto#services) while it does not limit you to a specific RPC implementation. You are free to implement it on your own using your favourite framework for the job, e.g. jQuery's AJAX functionality or WebSockets.

**Requirements:** ProtoBuf.js >= 1.2.0

Let's look at the following example:

```protobuf
message RequestType {}
message ResponseType {}

service MyService {
   rpc MyMethod(RequestType) returns(ResponseType);
}
```

```js
var builder = ProtoBuf.loadProtoFile(...the file from above...),
    root = builder.build(),
    MyService = root.MyService,
    RequestType = root.RequestType,
    ResponseType = root.ResponseType;

// Craft your RPC implementation (this is probably the simplest example possible)
var rpcImpl = function(method, req, callback) {
   if (method == ".MyService.MyMethod") { // Fully qualified name
      doSomeFancyNetworking(req.toArrayBuffer(), function(errorIfAny, response) {
         callback(errorIfAny, response);
         // Response may be either a pre-parsed protobuf message or the raw response data
      });
      // or short: doSomeFancNetworking(req.toArrayBuffer(), callback); ... you get it
   } else {
      throw(new Error("Method not supported: "+method));
   }
}

// Create a new service and provide it with your RPC implementation.
var myService = new MyService(rpcImpl);

// Call a service method. This will also type check the actual request and the response types.
myService.MyMethod(new RequestType(), function(err, res) {
   if (err) {
      // handle error...
      return;
   }
   // handle response...
});
```

It's also possible to skip the instantiation and to hand the `rpcImpl` directly to the alternative static method:

```js
// Note the uppercase "MyService" to access the static method
MyService.MyMethod(rpcImpl, new RequestType(), function(err, res) {
   if (err) {
      // handle error...
      return;
   }
   // handle response...
});
```

**The flow here is:**

1. You call the service method with a request message and provide a callback that accepts the error if any as the first argument and, if it's not an error and thus error is `null`, the response message as the second (just like in node).
2. ProtoBuf.js validates the request message type. If it's an error, the callback is called (goto 7).
3. Your RPC implementation is called with the method name, the request message and the callback.
4. Your RPC implementation serializes/encodes and sends out the request and gathers a response.
5. Your RPC implementation calls the callback provided to it with the error if any or null and the response message or raw response bytes (ideally as a ByteBuffer, an ArrayBuffer or a node Buffer).
6. ProtoBuf.js validates the response message type. If it's an error, the callback is called with it.
7. The callback provided when calling the service method is called with the error if any or null and the response message.

Additionally, all defined options are available as the `$options` property on the service class and instance as well as the static and instance methods.

**Troubleshooting:** Keep in mind that protobuf is a binary protocol and thus its data is likely to be corrupted if it is carelessly converted to a string or vice-versa. If possible, use features like binaryType="arraybuffer" as available with WebSockets or, if nothing like that is available, make sure to encode the binary data to base64 prior to transmission and properly decode it on receival. To achieve this in a convenient way you can use `Message#toBase64():string` and `Message.decode64(string):Message` respectively (requires ByteBuffer >= 1.5.0). See also: [How to read binary data in the browser or under node.js?](https://github.com/dcodeIO/ProtoBuf.js/wiki/How-to-read-binary-data-in-the-browser-or-under-node.js%3F)

**Next:** [Learn more about using reflection](https://github.com/dcodeIO/ProtoBuf.js/wiki/Reflection)