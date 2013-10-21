ProtoBuf.js also allows you to define RPC services while it does not limit you to a specific RPC implementation. You are free to implement it on your own using your favourite framework for the job.

Let's look at the following example:

```protobuf
message RequestType {}
message ResponseType {}

service MyService {
   rpc MyMethod(RequestType) returns(ResponseType);
}
```

```js
var builder = ProtoBuf.protoFromFile(...the file from above...),
    root = builder.build(),
    MyService = root.MyService,
    RequestType = root.RequestType,
    ResponseType = root.ResponseType;

// Craft your RPC implementation (this is probably the simplest example possible)
var rpcImpl = function(method, req, callback) {
   if (method == "MyMethod") {
      doSomeFancyNetworkingAndCall(function(errorIfAny, response) {
         callback(errorIfAny, response);
         // Response may be either a pre-parsed protobuf message or the raw response data
      });
      // or short: doSomeFancNetworkingAndCall(callback); ... you get it
   } else {
      throw(new Error("Invalid method"));
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