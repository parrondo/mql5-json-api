# Metaquotes MQL5 - Python Backtrader - API

### Development state: Betta (Code is stable, documentation in development)

[Report Bug](https://github.com/khramkov/MQL5-JSON-API/issues) or [Request Feature](https://github.com/khramkov/MQL5-JSON-API/issues)

## Table of Contents
* [About the Project](#about-the-project)
* [Installation and usage](#installation-and-usage)
* [License](#license)

## About The Project

This project was created to work as a broker for Backtrader trading framework. It uses ZeroMQ sockets to communicate. Python side of this project located here: [Python Backtrader - Metaquotes MQL5 ](https://github.com/khramkov/Backtrader-MQL5-API)

Working:
* Change timeframe and instrument symdol
* Account info
* Balance info
* Historical data
* Live data
* Fetching orders/positions
* Order creation
* Cancel order
* Close position

Not working:
* Trades info

## Installation and usage

1. Inastall ZeroMQ for MQL5 [https://github.com/dingmaotu/mql-zmq](https://github.com/dingmaotu/mql-zmq)
2. Put 'include/json.mqh' from this repo to your host 'include' directoty.
3. Compile the code and attach it to chart in Metatrader 5. 
4. This version dosn't support symbol and timeframe changing. You should control it yourself.


## License
Distributed under the GNU License. See `LICENSE` for more information.
