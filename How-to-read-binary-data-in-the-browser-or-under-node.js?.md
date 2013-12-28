When reading binary data in the browser or under node.js, it is mandatory to understand that just reading it (as a string) is not enough. When doing this, the data will probably become corrupted when it is converted between character sets and ProtoBuf.js will not be able to decode it or will return random stuff. What's actually required here is to read the data as binary.

To make it easier for you to get this right, here is some insight on the topic:

* [XMLHttpRequest: responseType="arraybuffer"](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Sending_and_Receiving_Binary_Data)
* [WebSockets: binaryType="arraybuffer"](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
* [node.js: Buffer](http://nodejs.org/api/buffer.html)

#### Browser XMLHttpRequest snippet
```js
...
var SomeMessage = ...; // Obtained via ProtoBuf.protoFromFile(...).build("SomeMessage") for example

var xhr = ProtoBuf.Util.XHR();
xhr.open(
    /* method */ "GET",
    /* file */ "/path/to/encodedSomeMessageData.bin",
    /* async */ true
);
xhr.responseType = "arraybuffer";
xhr.onload = function(evt) {
    var msg = SomeMessage.decode(xhr.response);
    alert(JSON.stringify(msg, null, 4)); // Correctly decoded
}
xhr.send(null);
```

Depending on the actual browser you are using, this may differ.

#### node.js: Properly using Buffers
```js
var http = require("http");

http.get("http://some/url/to/binary.pb", function(res) {
	var data = []; // List of Buffer objects
	res.on("data", function(chunk) {
		data.push(chunk); // Append Buffer object
	});
	res.on("end", function() {
		data = Buffer.concat(data); // Make one large Buffer of it
		var myMessage = MyMessage.decode(data);
		...
	});
	...
});
...
```

*Note:* Do not use `ProtoBuf.Util.fetch(...)` for reading binary data. This is used exclusively to fetch .proto files. 

**Next:** [Feel enlightened and go back to start](https://github.com/dcodeIO/ProtoBuf.js/wiki)