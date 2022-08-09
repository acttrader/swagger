**http api protocol (BETA)**

**requests**:

* [EXCHANGES](#exchanges) 
* [SYMBOLS](#symbols)
  


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
get all symbols for particular exhange    

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