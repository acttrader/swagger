**websocket protocol (BETA)**

**requests**:

*   request to place single order
    

``` json
{
    "event": "newordersingle",
    "requestId": "ID",
    "payload": {
        "symbol": "EURUSD",
        "side": "BUY",
        "type": "LIMIT",
        "price": 1.2345,
        "stopPrice": 1.2345, // for STOP, STOP_LIMIT orders
        "quantity": 10000,
        "clientOrderId": "string",
        "timeInForce": "GTT",
        "timeInForceDate": "2022-03-19T07:22:00.000Z", // date for timeInForce
        "login": "user-login"
    }
}
{
    "event": "newordersingle",
    "requestId": "ID",
    "payload": {
        "orderId": 12321321,
        "status": "NEW"
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
