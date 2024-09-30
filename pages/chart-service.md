**http api protocol (BETA)**

**requests**:

* [EXCHANGES](#exchanges) 
* [SYMBOLS](#symbols)
* [CANDLES](#candles)  


#### EXCHANGES 
get all exchanges    

request

```
http://localhost:8888/exchanges
```

response

``` json
{
	"success": true,
	"result": ["BITTREX", "LMAX", "T4B"]
}
```

#### SYMBOLS 
get all symbols for particular exchange    

request

```
http://localhost:8888/symbols?exchange=BITTREX
```

response

``` json
{
	"success": true,
	"result": ["XRP/USDT", "FOL/USDT", "USDT/USD", "ETH/USDC", "UNI/BTC"]
}
```

#### CANDLES
get chart data <br>
all datetimes in GMT timezone

parameters:
exchange - [string] mandatory<br>
symbol - [string] mandatory<br>
period - [string] [1m,5m,15m,30m,1h,2h,4h,8h,1d,1w,1month,1y]<br>
from - [string] <br>
till - [string] <br>
count - [int] mandatory if from is empty<br>


headers:<br>
time: 17:00 (default 00:00)<br>
timezone: EST5EDT (default GMT)

```
http://localhost:8888/candles?exchange=BITTREX&symbol=USDT/USD&period=1d&from=202201100000&till=202201101020

```

