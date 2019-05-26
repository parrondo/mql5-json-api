# Metaquotes MQL5 - JSON - API

### Development state: stable beta (code is stable)

## Table of Contents
* [About the Project](#about-the-project)
* [Installation](#installation)
* [Documentation](#documentation)
* [Usage](#usage)
* [License](#license)

## About the Project

This project was developed to work as a server for Backtrader Python trading framework. It is based on ZeroMQ sockets and uses JSON format to communicate. But now it has grown to the independent project. You can use it with any language that has [ZeroMQ binding](http://zeromq.org/bindings:_start).


Backtrader Python client located here: [Python Backtrader - Metaquotes MQL5 ](https://github.com/khramkov/MQL5-Backtrader-API)

In development:
* Historical data load speed
<<<<<<< HEAD
* Add error handling to docs
* Trades info
* Experation
* Devitation
* Netting/hedging mode switch
* Stop limit orders
=======
* Trades info
* Experation
* Devitation
>>>>>>> f5db7b13fe223453c6b2950ab4e5cddea7a80e19

## Installation

1. Install ZeroMQ for MQL5 [https://github.com/dingmaotu/mql-zmq](https://github.com/dingmaotu/mql-zmq)
<<<<<<< HEAD
2. Put `include/Json.mqh` from this repo to your MetaEditor `include` directoty.
3. Download and compile `experts/JsonAPI.mq5` script. 
4. Check if Metatrader 5 automatic trading is allowed.
5. Attach the script to a chart in Metatrader 5.
6. Allow DLL import in dialog window.
7. Check if the ports are free to use. (default:`15555`,`15556`, `15557`,`15558`)

Tested on macOS Mojave / Windows 10 in Parallels Desktop container.

## Documentation

The script uses four ZeroMQ sockets:

1. `System socket` - recives requests from client and replies 'OK'
2. `Data socket` - pushes data to client depending on request via System socket.
3. `Live socket` - automatically pushes last candle when it closes.
4. `Streaming socket` - automatically pushes last transaction info every time it happens.

The idea is to send requests via `System socket` and recieve results/errors via `Data socket`. For `Live socket` and `Streaming socket` event handlers should be created because server sends data to theese sockets automatically. See examples in [Usage](#usage) section.

`System socket` request uses default JSON dictionary:
=======
2. Put 'include/Json.mqh' from this repo to your MetaEditor 'include' directoty.
3. Download and compile JsonAPI script. 
4. Check if automatic trading is allowed.
5. Attach the script to a chart in Metatrader 5.
6. Allow DLL import in dialog window.
7. Check if the ports are free to use. (default: 15555, 15556, 15557, 15558)

Tested on macOS Mojave and Windows 10 in Parallels Desktop container.

## Documentation

The script uses 4 ZeroMQ sockets:

1. System socket - recives requests from client and replies 'OK'
2. Data socket - pushes data to client depending on request via System socket.
3. Live socket - pushes last candle when it closes.
4. Streaming socket - pushes last transaction info when it happens.



Configure the script:
>>>>>>> f5db7b13fe223453c6b2950ab4e5cddea7a80e19

```
{
	"action": None,
	"actionType": None,
	"symbol": None,
	"chartTF": None,
	"fromDate": None,
	"toDate": None,
	"id": None,
	"magic": None,
	"volume": None,
	"price": None,
	"stoploss": None,
	"takeprofit": None,
	"expiration": None,
	"deviation": None,
	"comment": None
}
```
<<<<<<< HEAD
Check out the available combinations of `action` and `actionType`:

action     | actionType           | Description                |
-----------|----------------------|----------------------------|
CONFIG     | None            	    | Set script configuration   |
ACCOUNT    | None                 | Get account settings       |
BALANCE    | None                 | Get current balance        |
POSITIONS  | None                 | Get current open positions |
ORDERS     | None                 | Get current open orders    |
HISTORY    | DATA                 | Get data history           |
HISTORY    | TRADES               | Get trades history         |
=======

action     | actionType           | Description                |
-----------|----------------------|----------------------------|
CONFIG     | null            	    | Set script configuration   |
ACCOUNT    | null            	    | Get account settings       |
BALANCE    | null                 | Get current balance        |
HISTORY    | null                 | Get symbol history         |
POSITIONS  | null                 | Get current open positions |
ORDERS     | null                 | Get current open orders    |
-----------|----------------------|----------------------------|
>>>>>>> f5db7b13fe223453c6b2950ab4e5cddea7a80e19
TRADE      | ORDER_TYPE_BUY       | Buy market                 |
TRADE      | ORDER_TYPE_SELL      | Sell market                |
TRADE      | ORDER_TYPE_BUY_LIMIT | Buy limit                  |
TRADE      | ORDER_TYPE_SELL_LIMIT| Sell limit                 |
TRADE      | ORDER_TYPE_BUY_STOP  | Buy stop                   |
TRADE      | ORDER_TYPE_SELL_STOP | Sell stop                  |
TRADE      | POSITION_MODIFY      | Position modify            |
TRADE      | POSITION_PARTIAL     | Position close partial     |
<<<<<<< HEAD
TRADE      | POSITION_CLOSE_ID    | Position close by id       |
TRADE      | POSITION_CLOSE_SYMBOL| Positions close by symbol  |
TRADE      | ORDER_MODIFY         | Order modify               |
TRADE      | ORDER_CANCEL         | Order cancel               |

Example Python API class:
=======
TRADE      | POSITION_ID          | Position close by id       |
TRADE      | POSITION_CLOSE       | Position close             |
TRADE      | ORDER_MODIFY         | Order modify               |
TRADE      | ORDER_CANCEL         | Order cancel               |

>>>>>>> f5db7b13fe223453c6b2950ab4e5cddea7a80e19

``` python
import zmq

class MTraderAPI:
<<<<<<< HEAD
    def __init__(self, host=None):
        self.HOST = host or 'localhost'
        self.SYS_PORT = 15555  # REP/REQ port
        self.DATA_PORT = 15556  # PUSH/PULL port
        self.LIVE_PORT = 15557  # PUSH/PULL port
        self.EVENTS_PORT = 15558  # PUSH/PULL port

        # ZeroMQ timeout in seconds
        sys_timeout = 1
        data_timeout = 10

        # initialise ZMQ context
        context = zmq.Context()

        # connect to server sockets
        try:
            self.sys_socket = context.socket(zmq.REQ)
            self.sys_socket.RCVTIMEO = sys_timeout * 1000
            self.sys_socket.connect('tcp://{}:{}'.format(self.HOST, self.SYS_PORT))

            self.data_socket = context.socket(zmq.PULL)
            self.data_socket.RCVTIMEO = data_timeout * 1000
            self.data_socket.connect('tcp://{}:{}'.format(self.HOST, self.DATA_PORT))
        except zmq.ZMQError:
            raise zmq.ZMQBindError("Binding ports ERROR")
=======

    HOST = 'localhost'
    SYS_PORT = 15555  # REP/REQ port
    DATA_PORT = 15556  # PUSH/PULL port
    LIVE_PORT = 15557  # PUSH/PULL port
    EVENTS_PORT = 15558  # PUSH/PULL port

    # ZeroMQ timeout in seconds
    sys_sock_tmout = 1
    data_sock_tmout = 10

    # initialise ZMQ context
    context = zmq.Context()

    # connect to server sockets
    try:
        sys_socket = context.socket(zmq.REQ)
        sys_socket.RCVTIMEO = sys_sock_tmout * 1000
        sys_socket.connect('tcp://{}:{}'.format(HOST, SYS_PORT))

        data_socket = context.socket(zmq.PULL)
        data_socket.RCVTIMEO = data_sock_tmout * 1000
        data_socket.connect('tcp://{}:{}'.format(HOST, DATA_PORT))

        live_socket = context.socket(zmq.PULL)
        live_socket.connect('tcp://{}:{}'.format(HOST, LIVE_PORT))

        events_socket = context.socket(zmq.PULL)
        events_socket.connect('tcp://{}:{}'.format(HOST, EVENTS_PORT))
    except zmq.ZMQError:
        raise zmq.ZMQBindError("Binding ports ERROR")
>>>>>>> f5db7b13fe223453c6b2950ab4e5cddea7a80e19

    def _send_request(self, data: dict) -> None:
        """ Send request to server via ZeroMQ System socket """
        try:
            self.sys_socket.send_json(data)
            msg = self.sys_socket.recv_string()
            # terminal received the request
            assert msg == 'OK', 'Something wrong on server side'
        except AssertionError as err:
            raise zmq.NotDone(err)
        except zmq.ZMQError:
            raise zmq.NotDone("Sending request ERROR")

    def _pull_reply(self):
        """ Get reply from server via Data socket with timeout """
        try:
            msg = self.data_socket.recv_json()
        except zmq.ZMQError:
            raise zmq.NotDone('Data socket timeout ERROR')
        return msg

<<<<<<< HEAD
    def live_socket(self, context=None):
        try:
            context = context or zmq.Context.instance()
            socket = context.socket(zmq.PULL)
            socket.connect('tcp://{}:{}'.format(self.HOST, self.LIVE_PORT))
        except zmq.ZMQError:
            raise zmq.ZMQBindError("Binding ports ERROR")
        return socket

    def streaming_socket(self, context=None):
        try:
            context = context or zmq.Context.instance()
            socket = context.socket(zmq.PULL)
            socket.connect('tcp://{}:{}'.format(self.HOST, self.EVENTS_PORT))
        except zmq.ZMQError:
            raise zmq.ZMQBindError("Binding ports ERROR")
        return socket

    def construct_and_send(self, **kwargs) -> dict:
        """ Construct request dictionary from default """

        # default dictionary
        request = {
            "action": None,
            "actionType": None,
            "symbol": None,
            "chartTF": None,
            "fromDate": None,
            "toDate": None,
            "id": None,
            "magic": None,
            "volume": None,
            "price": None,
            "stoploss": None,
            "takeprofit": None,
            "expiration": None,
            "deviation": None,
            "comment": None
        }

        # update dict values if exist
        for key, value in kwargs.items():
            if key in request:
                request[key] = value
            else:
                raise KeyError('Unknown key in **kwargs ERROR')

        # send dict to server
        self._send_request(request)

        # return server reply
        return self._pull_reply()
```
## Usage
All examples will be on Python 3. Lets create an instance of MetaTrader API class:

``` python
api = MTraderAPI()
```

First of all we should configure script `symbol` and `timeframe`. Live data stream will be configured to the seme params.

``` python
rep = api.construct_and_send(action="CONFIG", symbol="EURUSD", chartTF="M5")
print(rep)
```

Get information about trading account.

``` python
rep = api.construct_and_send(action="ACCOUNT")
print(rep)
```

Get historical data. `fromDate` should be in timestamp format. There are some issues:

- MetaTrader keeps historical data in cache. But when you make a request for the first time, MetaTrader downloads data from a broker. This operation can exceed `Data socket` timout. It depends on your broker. Second request will be handeled quickly.
- Historical data processing code is not optimal. It takes too much time to process more than `50000` candles. Under refactoring now.

``` python
rep = api.construct_and_send(action="HISTORY", actionType="DATA", symbol="EURUSD", chartTF="M5", fromDate=1555555555)
print(rep)
```
Buy market order.

``` python
rep = api.construct_and_send(action="TRADE", actionType="ORDER_TYPE_BUY", symbol="EURUSD", "volume": 0.1, "stoploss": 1.1, "takeprofit": 1.3)
print(rep)
```

Sell limit order. Remember to switch SL/TP depending on BUY/SELL, or you will get 'invalid stops' error.

- BUY:  	SL < price < TP
- SELL: 	SL > price > TP

``` python
rep = api.construct_and_send(action="TRADE", actionType="ORDER_TYPE_SELL_LIMIT", symbol="EURUSD", "volume": 0.1, "price": 1.2, "stoploss": 1.3, "takeprofit": 1.1)
print(rep)
```

Event handler example for `Live socket` and `Data socket`.
 
``` python
import zmq
import threading

api = MTraderAPI()


def _t_livedata():
    socket = api.live_socket()
    while True:
        try:
            last_candle = socket.recv_json()
        except zmq.ZMQError:
            raise zmq.NotDone("Live data ERROR")
        print(last_candle)


def _t_streaming_events():
    socket = api.streaming_socket()
    while True:
        try:
            trans = socket.recv_json()
            request, reply = trans.values()
        except zmq.ZMQError:
            raise zmq.NotDone("Streaming data ERROR")
        print(request)
        print(reply)


for i in range(3):
    t = threading.Thread(target=_t_livedata, daemon=True)
    t.start()

for i in range(3):
    t = threading.Thread(target=_t_streaming_events, daemon=True)
    t.start()

while True:
    pass
```

## License
This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See `LICENSE` for more information.
=======
    def live_data(self):
        """ Catch live data from server """
        try:
            candle = self.live_socket.recv_json()
        except zmq.ZMQError:
            raise zmq.NotDone("Live data ERROR")
        return candle

    def streaming_events(self):
        """ Catch events from server """
        try:
            candle = self.events_socket.recv_json()
        except zmq.ZMQError:
            raise zmq.NotDone("Streaming events ERROR")
        return candle

    def construct_and_send(self, **kwargs) -> dict:
        """ Construct request dictionary from default """

        # default dictionary
        request = {
            "action": None,
            "actionType": None,
            "symbol": None,
            "chartTF": None,
            "fromDate": None,
            "toDate": None,
            "id": None,
            "magic": None,
            "volume": None,
            "price": None,
            "stoploss": None,
            "takeprofit": None,
            "expiration": None,
            "deviation": None,
            "comment": None
        }

        # update dict values if exist
        for key, value in kwargs.items():
            if key in request:
                request[key] = value
            else:
                raise KeyError('Unknown key in **kwargs ERROR')

        # send dict to server
        self._send_request(request)

        # return server reply
        return self._pull_reply()

```

# License
Distributed under the GNU v3 License. See `LICENSE` for more information.
>>>>>>> f5db7b13fe223453c6b2950ab4e5cddea7a80e19
