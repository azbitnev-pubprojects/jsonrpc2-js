JSONRPC2-js API Documentation
============

## Include library to HTML page 

```html
<script src="/path/too/your/copy/JSONRPC2.js"></script>
```

## For Node.js users

```javascript
const JSONRPC2 = require('/path/too/your/copy/JSONRPC2.js')
```

## Basic example

```javascript
;;;

(async () => {

	let obj = new JSONRPC2.RemoteProxyObject(
	    new JSONRPC2.Transports.HTTP(
	        'https://some.where/api/endpoint', // http:// available too
	         {
	            mode: 'cors'
	         }
	    )
	)

	console.log(await obj.substract(50, 23))
		// 27
	console.log(await obj.substract(12, 3))
		// 9

})()
```

## Class JSONRPC2.Protocol.Client

Contains client-side logic. Prepares a correct requests.

### Method .call(method, params = undefined, id = undefined)

Prepares a correct request. Returns an instance of `JSONRPC2.Protocol.Request`. 

Parameters:
`method` - A string containing the name of the method to be invoked.
`params` - A structured value that holds the parameter values to be used during the invocation of the method. Optional parameter.
`id` - An identifier of request. Will auto-generated if omit this.

Returns:
`JSONRPC2.Protocol.Request` - A correct request.

### Method .notify(method, params = undefined)

Prepares a correct request. Returns instance of `JSONRPC2.Protocol.Request`.

Parameters:
`method` - A string containing the name of the method to be invoked.
`params` - A structured value that holds the parameter values to be used during the invocation of the method. Optional parameter.

Returns:
`JSONRPC2.Protocol.Request` - A correct request.

### Method .batch()

Prepares a batch request.

Returns:
`JSONRPC2.Protocol.Batch` - An object which contains all requests of the batch.

## Class JSONRPC2.Protocol.Batch

Represents a container for all requests of the batch.

### Method .call(method, params = undefined, id = undefined)

Add invocation-request into the batch. Returns this batch. 

Parameters:
`method` - A string containing the name of the method to be invoked.
`params` - A structured value that holds the parameter values to be used during the invocation of the method. Optional parameter.
`id` - An identifier of request. Will auto-generated if omit this.

Returns:
`JSONRPC2.Protocol.Batch` - Returns this batch.

### Method .notify(method, params = undefined)

Add notification-request into the batch. Returns this batch. 

Parameters:
`method` - A string containing the name of the method to be invoked.
`params` - A structured value that holds the parameter values to be used during the invocation of the method. Optional parameter.

Returns:
`JSONRPC2.Protocol.Batch` - Returns this batch.

### Method .end()

Complete the batch. Returns array of requests and clean the list of requests.

Returns:
`[ JSONRPC2.Protocol.Request ]` - An array of all requests of the batch.

## Class JSONRPC2.Protocol.Request

Represents a request to remote server.

### Property .method

Public property. A string containing the name of the method to be invoked. See specification for more details.

### Property .params

Public property. A structured value that holds the parameter values to be used during the invocation of the method. This member may be omitted. See specification for more details.

### Property .id

Public property, represents an identifier of request. See specification for more details.

## Class JSONRPC2.Protocol.Response

Represents a response of remote server.

### Property .error

Public property, describes error, if there was an error invoking the method. This member is not exist, if there was no error triggered during invocation.

### Property .result

Public property, contains result of remote method. This member is not exist, if there was an error invoking the method.

### Property .id

Public property, represents an identifier of request. See specification for more details.

## Class JSONRPC2.Protocol.Server

Contains server-side logic.

### Method .on(method, parameters, fn)

Define a handler function for specified name of method.

Parameters:
`method` - A string containing the name of the method to be defined.
`parameters` - Array of names of parameters.
`fn` - Handler function.

Returns:
`JSONRPC2.Protocol.Server` - this server's instance to use chain calls.

Example:
```javascript
server.on(
    'substract',
    [ 'minuend', 'subtrahend' ],
    function (minuend, subtrahend) {
        return minuend - subtrahend
    }
)
```

### Method .off(method)

Remove definition of method with a specified name.

Parameters:
`method` - A string containing the name of the method to be removed.

Returns:
`JSONRPC2.Protocol.Server` - this server's instance to use chain calls.

Example:
```javascript
server.off('substract')
```

### Method .reply(request)

Execute a method specified in the request and returns a response.

Parameters:
`request` - An instance of `JSONRPC2.Protocol.Request` or an array of instances of `JSONRPC2.Protocol.Request`

Returns:
J`SONRPC2.Protocol.Response` - A response.

### Method .reply(request[])

Execute a methods specified in the requests and returns a responses.

Parameters:
`request` - An array of instances of `JSONRPC2.Protocol.Request`

Returns:
`[ JSONRPC2.Protocol.Response ]` - An array of responses.

## Class JSONRPC2.Errors.GenericError (extends Error)

A class which represents generic JSON RPC 2.0 error.

### constructor(code, message, data = null)

Parameters:
`code` - An integer code of error. See specification.
`message` - A message text.
`data` - A primitive or structured value which contains additional information about the error. Optional.

## Class JSONRPC2.Errors.ParseError (extends JSONRPC2.Errors.GenericError)

An error occurred on the server while parsing the JSON text.

### constructor(data = null)

Parameters:
`data` - A primitive or structured value which contains additional information about the error. Optional.

## Class JSONRPC2.Errors.InvalidRequestError (extends JSONRPC2.Errors.GenericError)

An error occurred on the server when the JSON sent is not a valid request object.

### constructor(data = null)

Parameters:
`data` - A primitive or structured value which contains additional information about the error. Optional.

## Class JSONRPC2.Errors.MethodNotFoundError (extends JSONRPC2.Errors.GenericError)

An error occurred on the server when the method does not exist or is not available.

### constructor(data = null)

Parameters:
`data` - A primitive or structured value which contains additional information about the error. Optional.

## Class JSONRPC2.Errors.InvalidParametersError (extends JSONRPC2.Errors.GenericError)

An error occurred on the server when method's parameters are is invalid.

### constructor(data = null)

Parameters:
`data` - A primitive or structured value which contains additional information about the error. Optional.

## Class JSONRPC2.Errors.InternalError (extends JSONRPC2.Errors.GenericError)

An error occurred on the server when internal JSON-RPC error happened.

### constructor(data = null)

Parameters:
`data` - A primitive or structured value which contains additional information about the error. Optional.

## Class JSONRPC2.Errors.ServerError (extends JSONRPC2.Errors.GenericError)

Reserved for implementation-defined server-errors.

### constructor(code, data = null)

Parameters:
`code` - An integer code of error. See specification.
`data` - A primitive or structured value which contains additional information about the error. Optional.

## Constants which defines error codes

`JSONRPC2.Errors.CODE_PARSE_ERROR = -32700`
`JSONRPC2.Errors.CODE_INVALID_REQUEST = -32600`
`JSONRPC2.Errors.CODE_METHOD_NOT_FOUND = -32601`
`JSONRPC2.Errors.CODE_INVALID_PARAMETERS = -32602`
`JSONRPC2.Errors.CODE_INTERNAL_ERROR = -32603`
`JSONRPC2.Errors.MIN_CODE_SERVER_ERROR = -32099`
`JSONRPC2.Errors.MAX_CODE_SERVER_ERROR = -32000`

## Class JSONRPC2.Transports.AbstractTransport

Transport is an abstract code which helps to send a requests to remote server and receive a responses using different transport-protocols.

### Method .encodeRequest(request)

Encode request object to JSON-encoded string.

Parameters:
`request` - An instance of `JSONRPC2.Protocol.Request` or an array of instances of `JSONRPC2.Protocol.Request`

Returns:
`String` - A request as JSON-encoded string.

### Method .decodeResponse(encodedResponse)

Encode request object to JSON-encoded string.

Parameters:
`encodedResponse` - JSON-encoded response to parse and validate.

Returns:
`JSONRPC2.Protocol.Response` - Decoded response.
`[ JSONRPC2.Protocol.Response ]` - Array of decoded responses, if there are several responses.

### Method .verifyMatching(request, response)

Verify and match identifier(s) inside request(s) and response(s).

Parameters:
`request` - An instance of `JSONRPC2.Protocol.Request` or an array of instances of `JSONRPC2.Protocol.Request`
`response` - An instance of `JSONRPC2.Protocol.Response` or an array of instances of `JSONRPC2.Protocol.Response`

Throws:
`Error` - If identifier(s) does not match.

Returns:
Method does not return any value.

### Method .reply(request)

Abstract method which must be implemented by any extension.

Parameters:
`request` - An instance of `JSONRPC2.Protocol.Request` or an array of instances of `JSONRPC2.Protocol.Request`

Returns:
`Promise` - Method must returns an instance of `Promise`.

## Class JSONRPC2.Transports.HTTP (extends JSONRPC2.Transports.AbstractTransport)

Transport implementation for JSON RPC 2.0 over HTTP/HTTPS using Fetch API implementation.

### constructor(endpoint, options, fetchImpl = undefined)

Parameters:
`endpoint` - An string with URL of remote point.
`options` - A structured value which contains additional options for Fetch API implementation. Optional. Default value is `{ cache: 'no-cache' }`
`fetchImpl` - A function that implements Fetch API. Optional.

Example for browsers:
```javascript
// create transport
let transport = new JSONRPC2.Transports.HTTP(
	'https://some.where/point', // http:// available too
	{
		mode: 'cors'
	}
)
```

Example for Node.js users:
```javascript
// use external implementation of Fetch API
// npm install node-fetch
const nodeFetch = require('node-fetch')

// create transport
let transport = new JSONRPC2.Transports.HTTP(
	'https://some.where/point', // http:// available too
	{
		mode: 'cors'
	},
	nodeFetch
)
```

### Method .setEndpoint(request)

Setup new endpoint.

Parameters:
`endpoint` - An string with URL of remote point.

### Method .setOptions(options)

Setup new options.

Parameters:
`options` - A structured value which contains additional options for Fetch API implementation. Optional. Default value is `{ cache: 'no-cache' }`

### Method .reply(request)

Transport implementation. See same method of class `JSONRPC2.Transports.AbstractTransport`.

## Class JSONRPC2.Transports.WebSockets (extends JSONRPC2.Transports.AbstractTransport)

Transport implementation for JSON RPC 2.0 over WebSockets using WebSocket API implementation.

### constructor(endpoint, options, fetchImpl = undefined)

Parameters:
`endpoint` - An string with URL of remote point.
`options` - A structured value which contains additional options (see below). Optional. Default value is `{ coldStart: false, reconnect: false, reconnectTimeout: 2000 }`.
`fetchImpl` - A function that imlements Fetch API. Optional.

Options:
`coldStart` - `true` - connect to remote server at first request, `false` - connect to remote server when instance of transport object is created.
`reconnect` - `true` - auto-reconnect when connection is broken, `false` - do not reconnect.
`reconnectTimeout` - timeout in milliseconds before reconnection.

Example for browsers:
```javascript
// create transport
let transport = new JSONRPC2.Transports.WebSockets(
	'wss://some.where:3000/point', // ws:// available too
	{
		reconnect: true
	}
)
```

Example for Node.js users:
```javascript
// use external implementation of WebSocket API
// npm install ws
const WebSocket = require('ws');

// create transport
let transport = new JSONRPC2.Transports.HTTP(
	'https://some.where/point', // http:// available too
	{
		mode: 'cors'
	},
	WebSocket
)
```

### Method .setOptions(options)

Setup new options.

Parameters:
`options` - A structured value which contains additional options (see below). Optional. Default value is `{ coldStart: false, reconnect: false, reconnectTimeout: 2000 }`.

Options:
`coldStart` - `true` - connect to remote server at first request, `false` - connect to remote server when instance of transport object is created.
`reconnect` - `true` - auto-reconnect when connection is broken, `false` - do not reconnect.
`reconnectTimeout` - timeout in milliseconds before reconnection.

### Method .reply(request)

Transport implementation. See same method of class JSONRPC2.Transports.AbstractTransport.

## Class JSONRPC2.RemoteObject (extends JSONRPC2.Client)

Contains basic code which interacts with a remote server over specified transport.

### constructor(transport)

Parameters:
`transport` - An instance of implementation of `JSONRPC2.Transports.AbstractTransport`.

### Method .setTransport(transport)

Setup new options.

Parameters:
`transport` - An instance of implementation of `JSONRPC2.Transports.AbstractTransport`.

### Method .call(method, params = undefined, id = undefined)

Call a remote method. Returns instance of `JSONRPC2.Protocol.Response`. 

Parameters:
`method` - A string containing the name of the method to be invoked.
`params` - A structured value that holds the parameter values to be used during the invocation of the method. Optional parameter.
`id` - An identifier of request. Will auto-generated if omit this.

Returns:
`JSONRPC2.Protocol.Response` - A response.

### Method .notify(method, params = undefined)

Notify remote server. Returns instance of `JSONRPC2.Protocol.Response`.

Parameters:
`method` - A string containing the name of the method to be invoked.
`params` - A structured value that holds the parameter values to be used during the invocation of the method. Optional parameter.

Returns:
`JSONRPC2.Protocol.Response` - A response.

### Method .batch()

Prepares a batch request.

Returns:
`JSONRPC2.Protocol.CallableBatch` - An object which contains all requests of the batch.

Example:
```javascript

(async () => {

	let remoteMath = new JSONRPC2.RemoteObject(
	    new JSONRPC2.Transports.HTTP(
	        'https://anywhere.host/some/endpoint' // http:// available too
	    )
	)
	
	let batch = remoteMath.batch()
		.call('substract', { subtrahend: 23, minuend: 50 })
		.call('substract', [ 12, 3 ])
		
	console.log(await batch.end())
	// [
	//	  Response { ... result: 27 ... },
	//	  Response { ... result: 9 ... }
	// ]

})()

```

## Class JSONRPC2.CallableBatch (extends JSONRPC2.Batch)

Extension of batch object which can be sent to remote server when batch has been completed.

### Method .call(method, params = undefined, id = undefined)

Add invocation-request into the batch. Returns this batch. See more in a `JSONRPC2.Protocol.Batch.call()` description.

### Method .notify(method, params = undefined)

Add notification-request into the batch. Returns this batch. See more in a `JSONRPC2.Protocol.Batch.notify()` description.

### Method .end()

Send a batch of requests to remote server and receive a responses.

Returns:
`[ JSONRPC2.Protocol.Response ]` - An array of responses.

## Class JSONRPC2.RemoteProxyObject

Another more easy to use implementation of a JSON RPC 2.0 client. It uses instance of `JSONRPC2.RemoteObject` and Proxy API to redirect method calls to remote server.

### constructor(transport)

Parameters:
`transport` - An instance of implementation of `JSONRPC2.Transports.AbstractTransport`.

Example:
```javascript
(async () => {

	let obj = new JSONRPC2.RemoteProxyObject(
	    new JSONRPC2.Transports.WebSockets(
	        'wss://some.where/endpoint' // ws:// available too
	    )
	)

	console.log(await obj.substract(50, 23)) // calling remote method
		// 27
	console.log(await obj.substract(12, 3)) // calling remote method
		// 9
		
})()
```