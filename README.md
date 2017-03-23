Websocket Server
================

A minimal Websockets library in Python with no external dependencies.
Based on [Python Websocket server by Pithikos](https://github.com/Pithikos/python-websocket-server).

  * Works with Python2 and Python3
  * Clean simple API
  * Multiple clients
  * No dependencies

Notice that this implementation does not support the more advanced features
like SSL etc. The project is focused mainly on making it easy to run a
websocket server for prototyping, testing or for making a GUI for your application.

API
===

The API is simply methods and properties of the `WebsocketServer` class.

## WebsocketServer

The WebsocketServer takes two arguments: a `port` and a `hostname`.
By default the localhost `127.0.0.1` is used. However if you want to be able and connect
to the server from the network you need to pass `0.0.0.0` as hostname e.g. `WebsocketServer(13254, host='0.0.0.0')`.

### Properties

| Property | Description          |
|----------|----------------------|
| clients  | A list of `client`   |


### Methods

| Method                      | Description                                                                           | Takes           | Gives |
|-----------------------------|---------------------------------------------------------------------------------------|-----------------|-------|
| `set_fn_new_client()`       | Sets a callback function that will be called for every new `client` connecting to us  | function        | None  |
| `set_fn_client_left()`      | Sets a callback function that will be called for every `client` disconnecting from us | function        | None  |
| `set_fn_message_received()` | Sets a callback function that will be called when a `client` sends a message          | function        | None  |
| `send_message()`            | Sends a `message` to a specific `client`. The message is a simple string.             | client, message | None  |
| `send_message_to_all()`     | Sends a `message` to **all** connected clients. The message is a simple string.       | message         | None  |


### Callback functions

| Set by                      | Description                                       | Parameters              |
|-----------------------------|---------------------------------------------------|-------------------------|
| `set_fn_new_client()`       | Called for every new `client` connecting to us    | client, server          |
| `set_fn_client_left()`      | Called for every `client` disconnecting from us   | client, server          |
| `set_fn_message_received()` | Called when a `client` sends a `message`          | client, server, message |


The client passed to the callback is the client that left, sent the message, etc. The server might not have any use to use. However it is
passed in case you want to send messages to clients.


### Example

```python
from websocket_server import WebsocketServer

def new_client(client, server):
	server.send_message_to_all("Hey all, a new client has joined us")

server = WebsocketServer(13254, host='127.0.0.1')
server.set_fn_new_client(new_client)
server.run_forever()
```

## Client

Client is just a dictionary passed along methods.

```python
{
	'id'      : client_id,
	'handler' : client_handler,
	'address' : (addr, port)
}
```
