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
* Trades info
* Experation
* Devitation

## Installation

1. Install ZeroMQ for MQL5 [https://github.com/dingmaotu/mql-zmq](https://github.com/dingmaotu/mql-zmq)
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

action     | actionType           | Description                |
-----------|----------------------|----------------------------|
CONFIG     | null            	    | Set script configuration   |
ACCOUNT    | null            	    | Get account settings       |
BALANCE    | null                 | Get current balance        |
HISTORY    | null                 | Get symbol history         |
POSITIONS  | null                 | Get current open positions |
ORDERS     | null                 | Get current open orders    |
-----------|----------------------|----------------------------|
TRADE      | ORDER_TYPE_BUY       | Buy market                 |
TRADE      | ORDER_TYPE_SELL      | Sell market                |
TRADE      | ORDER_TYPE_BUY_LIMIT | Buy limit                  |
TRADE      | ORDER_TYPE_SELL_LIMIT| Sell limit                 |
TRADE      | ORDER_TYPE_BUY_STOP  | Buy stop                   |
TRADE      | ORDER_TYPE_SELL_STOP | Sell stop                  |
TRADE      | POSITION_MODIFY      | Position modify            |
TRADE      | POSITION_PARTIAL     | Position close partial     |
TRADE      | POSITION_ID          | Position close by id       |
TRADE      | POSITION_CLOSE       | Position close             |
TRADE      | ORDER_MODIFY         | Order modify               |
TRADE      | ORDER_CANCEL         | Order cancel               |


``` python
import zmq

class MTraderAPI:

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
