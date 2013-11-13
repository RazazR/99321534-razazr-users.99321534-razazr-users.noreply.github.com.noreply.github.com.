When reading binary data in the browser it is mandatory to understand that just reading it (as a string) is not enough. When doing this, the data will probably become corrupted when it is converted between character sets and ProtoBuf.js will not be able to decode it or will return random stuff.

To make it easier for you to get this right, here is some insight on the topic:

* [XMLHttpRequest: responseType="arraybuffer"](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Sending_and_Receiving_Binary_Data)
* [WebSockets: binaryType="arraybuffer"](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

#### XMLHttpRequest snippet
```js
var xhr = ProtoBuf.Util.XHR();
xhr.open('GET', "person.bin", true);
xhr.responseType = "arraybuffer";
xhr.onload = function(evt) {
	var msg = test.decode(xhr.response);
	alert(JSON.stringify(msg.person[0])); // Correctly decoded
}
xhr.send(null);
```

Depending on the actual browser you are using, this may differ.

*Note:* Do not use `ProtoBuf.Util.fetch(...)` for reading binary data. This is used exclusively to fetch .proto files. 

**Next:** [Feel enlightened and go back to start](https://github.com/dcodeIO/ProtoBuf.js/wiki)