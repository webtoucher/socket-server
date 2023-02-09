# Simple TCP socket server with select

![License](https://img.shields.io/badge/License-BSD%203--Clause-green)
[![Downloads](https://img.shields.io/pypi/dm/simple-socket-server.svg?color=orange)](https://pypi.python.org/pypi/simple-socket-server)
[![Latest Version](https://img.shields.io/pypi/v/simple-socket-server.svg)](https://pypi.python.org/pypi/simple-socket-server)
[![Supported Python versions](https://img.shields.io/pypi/pyversions/simple-socket-server.svg)](https://pypi.python.org/pypi/simple-socket-server)

## Installation

Install it with pip:

```shell
$ pip install simple-socket-server
```

Or you can add it as dependency in requirements.txt file of your python application:

```
simple-socket-server~=1.8
```

## Usage

Easy way to understand how it works is testing socket server via telnet terminal:

```python
from socket import socket
from simple_socket_server import SimpleSocketServer

socket_server = SimpleSocketServer()


@socket_server.on('connect')
def on_connect(sock: socket, peer):
    print('New connection from %s:%s' % peer)
    socket_server.send(sock, bytes('What is your name?\r\n', 'utf-8'))


@socket_server.on('disconnect')
def on_disconnect(_sock, peer):
    print('Connection from %s:%s is closed' % peer)


@socket_server.on('message')
def on_message(sock: socket, peer, message: bytes):
    print('Incoming data from %s:%s' % peer, message)
    socket_server.send(sock, bytes('Hi, ', 'utf-8') + message)


socket_server.run(host='0.0.0.0', port=5000)

```

Then you can connect to server:

```shell
telnet 127.0.0.1 5000
```
