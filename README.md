jsonrpc2-js
============

A simple JSON-RPC 2.0 implementation

## Features
- Correct: fully compliant with the [JSON-RPC 2.0 specifications](http://www.jsonrpc.org/specification)
- Has no depeddencies from other libraries and packages.
- Simple JSON-RPC 2.0 client code.

## Tasks to be completed
- Write more examples
- Write some unit tests
- Write better API docs

Read [API] documentation for more details.

## Examples

### Typical client (works in browser or Node.js)

```ecmascript

(async () => {

	let remoteMath = new JSONRPC2.ProxyObject(
	    // using JSON RPC over HTTP
	    new JSONRPC2.Transports.HTTP(
	        'http://anywhere.host/some/endpoint',
	         {
	            mode: 'cors'
	         }
	    )
	)

	console.log(await remoteMath.substract(50, 23)) // substract is a remote method
		// 27
	console.log(await remoteMath.substract(12, 3))
		// 9

})()

```

### More flexible client (works in browser or Node.js)

```ecmascript

(async () => {

	let remoteMath = new JSONRPC2.RemoteObject(
	    // using JSON RPC over HTTP
	    new JSONRPC2.Transports.HTTP(
	        'http://anywhere.host/some/endpoint',
	         {
	            mode: 'cors'
	         }
	    )
	)

	console.log(await remoteMath.call('substract', { subtrahend: 23, minuend: 50 }))
		// Response { ... result: 27 ... }

	console.log(await remoteMath.call('substract', [ 12, 3 ]))
		// Response { ... result: 9 ... }

	console.log(
		await remoteMath.batch()
			.call('substract', { subtrahend: 23, minuend: 50 })
			.call('substract', [ 12, 3 ])
			.end()
	)
	// [
	//	  Response { ... result: 27 ... },
	//	  Response { ... result: 9 ... }
	// ]

})()

```

### Server (Node.js)

```ecmascript

let server = new JSONRPC2.LocalServer()

server.on(
    'substract',
    [ 'minuend', 'subtrahend' ],
    function (minuend, subtrahend) {
        return minuend - subtrahend
    }
)

http.createServer(function (request, response) {

	let headers = {
		'Access-Control-Allow-Origin': '*',
		'Access-Control-Allow-Methods': 'POST, GET, DELETE, PUT, PATCH, OPTIONS',
		'Access-Control-Allow-Headers': 'Authorization, Origin, Content-Type, Accept',
		'Access-Control-Max-Age': '5',
	}

    let requestText = ''

    if (request.method === 'OPTIONS') {

        request.on('end', function () {

			response.writeHead(200, headers)
			response.end('')
		})

	} else if (request.method === 'POST') {

        request.on('data', function (data) {

            requestText += data
        })

        request.on('end', function () {

			headers['Content-Type'] = 'application/json'

			response.writeHead(200, headers)
            response.end(server.reply(requestText))
        })

    } else if (request.method === 'GET') {

        request.on('end', function () {

			headers['Content-Type'] = 'text/plain; charset=utf-8'

			response.writeHead(200, headers)
    	    response.end('Use requests with method POST.\nИспользуйте запросы методом POST.\n')
        })

	} else {

		headers['Content-Type'] = 'text/plain; charset=utf-8'

		response.writeHead(405, headers)
	    response.end(`Method ${request.method} is not allowed.`)
	}
    
}).listen(port)

```
