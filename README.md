**websocket protocol (BETA)**

**requests**:

*  **NEWORDERSINGLE** - request to place single order
    

``` json
{
    "event": "newordersingle",                  //[STRING, Mandatory]
    "requestId": "ID",                          //[STRING, Mandatory]
    "payload": {
        "symbol": "EURUSD",                     //[STRING, Mandatory] 
        "side": "BUY",                          // [ENUM, Mandatory]
        "type": "LIMIT",                        // [ENUM, Mandatory]
        "price": 1.2345,                        // [DECIMAL, Mandatory for LIMIT and STOP_LIMIT orders]
        "stopPrice": 1.2345,                    // [DECIMAL, Mandatory for STOP and STOP_LIMIT orders]
        "quantity": 10000,                      // [DECIMAL, Mandatory]
        "clientOrderId": "my first order",      // [STRING] 
        "timeInForce": "AON",                   // [ENUM, Mandatory]
        "lifeTime": "2022-03-19T07:22:00.000Z", // [DATE, order lifetime in UTC timezone] 
        "login": "user-login"                   // [STRING, Mandatory]
    }
}
```

side enum:  
* SELL
* BUY
  
type enum:
* MARKET
* LIMIT
* STOP
* STOP_LIMIT
  
timeInForce enum:
* AON
* FOK
* IOC 

**NEWSINGLEORDER** - response on 

``` json
{
    "event": "newordersingle",
    "requestId": "ID",
    "payload": {
        "orderId": 100001,
        "clientOrderId": "my first order",
        "status": "NEW",
        "type": "LIMIT"
    }
}

```

*   request to modify order
    

``` json
{
    "event": "modifyordersingle",
    "requestId": "ID",
    "payload": {
        "orderId": 123123,
        "price": 1.2345,
        "stopPrice": 1.2345,
        "quantity": 10000
    }
}
{
    "event": "modifyordersingle",
    "requestId": "ID",
}

```

*   request to place OCO order
    

``` json
{
 {
    "event": "neworderoco",
    "requestId": "ID",
    "payload": {
        "login": "user-login",
        "lifeTime": "2022-03-19T07:22:00.000Z",
        "symbol": "EURUSD",
        "side": "BUY",
        "timeInForce": "AON",
        "quantity": 10000,
        "orders": [
            {
                "type": "LIMIT",
                "price": 1.2345,
                "clientOrderId": "6gCrw2kRUAF9CvJDGP16IP"
            },
            {
                "type": "STOP_LIMIT",
                "price": 1.2345,
                "stopPrice": 1.2345,
                "clientOrderId": "clientorderid"
            }
        ]
    }
}
{
    "event": "neworderoco",
    "requestId": "ID",
    "payload": [
        {
            "orderId": 12321320,
            "clientOrderId": "6gCrw2kRUAF9CvJDGP16IP",
            "status": "NEW",
            "type": "LIMIT"
        },
        {
            "orderId": 12321321,
            "clientOrderId": "6gCrw2kRUAF9CvJDGP16IP",
            "status": "NEW",
            "type": "STOP_LIMIT"
        }
    ]
}

```

*   request to get all pending conditional orders (LIMIT, STOP)
    

``` json
{
    "event": "orders",
    "requestId": "ID"
}
{
    "event": "orders",
    "payload": [
        {
            "symbol": "BTCUSDT",
            "type": "MARKET",
            "orderId": 28,
            "clientOrderId": "6gCrw2kRUAF9CvJDGP16IP",
            "transactTime": 1507725176595,
            "price": 0.00000000,
            "origQty": 10,
            "executedQty": 10,
            "cummulativeQuoteQty": 1.2344,
            "status": "FILLED",
            "timeInForce": "GTC",
            "type": "MARKET",
            "side": "SELL"
        },
        {
            "symbol": "BTCUSDT",
            "type": "MARKET",
            "orderId": 29,
            "clientOrderId": "6gCrw2kRUAF9CvJDGP16IP",
            "transactTime": 1507725176595,
            "price": 1.234,
            "origQty": 10,
            "executedQty": 10,
            "status": "CANCELED",
            "timeInForce": "GTC",
            "type": "MARKET",
            "side": "SELL"
        }
    ]
}

```

*   request to get particular order
    

``` json
{
    "event": "orderstatus",
    "requestId": "ID",
    "payload": {
        "orderId": 123123
    }
}
{
    "event": "orderstatus",
    "requestId": "ID",
    "payload": {
        "symbol": "BTCUSDT",
        "type": "MARKET",
        "orderId": 28,
        "clientOrderId": "6gCrw2kRUAF9CvJDGP16IP",
        "transactTime": 1507725176595,
        "price": 1.12312,
        "origQty": 10,
        "executedQty": 10,
        "status": "FILLED",
        "timeInForce": "GTC",
        "type": "MARKET",
        "side": "SELL",
        "fills": [
            {
                "price": 4000,
                "qty": 5.00000000
            },
            {
                "price": 3999,
                "qty": 5
            },
        ]
    }
}

```

*   request to cancel particular order
    

``` json
{
    "event": "ordercancel",
    "requestId": "ID",
    "payload": {
        "orderId": 123123
    }
}
{
    "event": "ordercancel",
    "requestId": "ID",
}

```

*   execution message
    

``` json
{
    "event": "execution",
    "payload": {
        "symbol": "BTCUSDT",
        "type": "MARKET",
        "orderId": 28,
        "clientOrderId": "6gCrw2kRUAF9CvJDGP16IP",
        "transactTime": 1507725176595,
        "price": 1.1323,
        "origQty": 10,
        "executedQty": 10,
        "status": "FILLED",
        "timeInForce": "GTC",
        "type": "MARKET",
        "side": "SELL"
    }
}

```

*   error response
    

``` json
{
    "event": "error",
    "requestId": "ID",
    "payload": {
        "code": 123345,
        "error": "error message"
    }
}

```
