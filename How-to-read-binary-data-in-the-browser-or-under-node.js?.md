When reading/writing binary data in the browser or under node.js, it is mandatory to understand that just reading it (as a string) is not enough. When doing this, the data will probably become corrupted when it is converted between character sets and protobuf.js will not be able to decode it or will return random stuff. What's actually required here is to properly handle the data as **binary**.

To make it easier for you to get this right, here is some insight on the topic:

* [XMLHttpRequest: responseType="arraybuffer"](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Sending_and_Receiving_Binary_Data)
* [WebSockets: binaryType="arraybuffer"](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
* [node.js: Buffer](http://nodejs.org/api/buffer.html)

#### Browser XMLHttpRequest snippet

Simple case, when you do request with response encoded only with protobuf bytes

```js
var xhr = new XMLHttpRequest();
xhr.open(
    /* method */ "GET",
    /* file */ "/path/to/encodedSomeMessageData.bin",
    /* async */ true
);
xhr.responseType = "arraybuffer";
xhr.onload = function(evt) {
    var msg = SomeMessage.decode(new Uint8Array(xhr.response));
    alert(JSON.stringify(msg, null, 4)); // Correctly decoded
}
xhr.send(null);
```

Multipart case, when you make request to endpoint that produce response encoded in "multipart" format, where different part of message could be encoded in different types, for example one part is encoded with XML and second part is encoded with protobuf. (For example, you can meet this case when you make requests against [MTOM2](https://en.wikipedia.org/wiki/Message_Transmission_Optimization_Mechanism) endpoint):

```js
var xhr = new XMLHttpRequest();
xhr.open(
    /* method */ "GET",
    /* file */ "/path/to/encodedSomeMessageData.mtom2",
    /* async */ true
);
xhr.responseType = "arraybuffer";
xhr.onload = function(evt) {
    // reading whole body
    var bodyEncodedInString = String.fromCharCode.apply(String, new Uint8Array(xhr.response));
    console.log(bodyEncodedInString);
    /*
        for example it would be something like that in console:
        `RESPONSE_BODY:
            --protobuf
            [...someprotobytes]
            --protobuf--
        `
    */
   
   var protoStart = bodyEncodedInString.indexOf('--protobuf');
   var protoEnd = bodyEncodedInString.indexOf('--protobuf--');

   /*
        "Offset start" and "offset end" are required for line endings. Don't forget, that on various operating systems there is different line endings, and it is not so uncommon when your backend is running on Windows and generates "\r\n" in place of line endings.
    */
   var offsetStart = 2;
   var offsetEnd = 2;

   var bufferSliceWhereProtobufBytesIs = xhr.response.slice(protoStart + offsetStart, protoEnd - offsetEnd);

   var msg = SomeMessage.decode(bufferSliceWhereProtobufBytesIs);
   alert(JSON.stringify(msg, null, 4)); // Correctly decoded
}

xhr.send(null);
```

__NOTE:__ In multipart case you need to use `xhr.responseType = "arraybuffer";` and slice buffers via offsets because browsers, when read binary encoded parts, creates some corrupted text from it, and you can't convert it to buffer and get correct ArrayBuffer for protobuf, so you need to slice original buffer (i did not found any solution, correct me if you find, please. Tested in Chrome, with native Typed arrays and Array Buffers).

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
