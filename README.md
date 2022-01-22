**websocket protocol (BETA)**

**requests**:


* [NEWORDERSINGLE](#newordersingle) 
* [MODIFYORDERSINGLE](#modifyordersingle) 
* [NEWORDEROCO](#neworderoco) 
* [ORDERS](#orders) 
* [ORDERSTATUS](#orderstatus) 
* [ORDERCANCEL](#ordercancel) 
* [EXECUTION](#execution) 
* [ERROR](#error) 


#### NEWORDERSINGLE 
request to place single order    

``` json
{
    "event": "newordersingle", #STRING,Mandatory
    "requestId": "ID",         #STRING,Mandatory
    "payload": {
        "symbol": "EURUSD",                     #STRING,Mandatory 
        "side": "BUY",                          #ENUM,Mandatory
        "type": "LIMIT",                        #ENUM,Mandatory
        "price": 1.2345,                        #DECIMAL,Mandatory for LIMIT and STOP_LIMIT
        "stopPrice": 1.2345,                    #DECIMAL,Mandatory for STOP and STOP_LIMIT
        "quantity": 10000,                      #DECIMAL,Mandatory
        "clientOrderId": "my first order",      #STRING 
        "timeInForce": "AON",                   #ENUM,Mandatory
        "tillTime": "2022-04-23T18:25:43.511Z", #STRING, order lifetime in UTC timezone
        "login": 123456                         #LONG, Mandatory
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

response

``` json
{
    "event": "newordersingle",
    "requestId": "ID",
    "payload": {
        "orderId": 1000001,
        "clientOrderId": "my first order",
        "status": "NEW",
        "type": "LIMIT"
    }
}

```

#### MODIFYORDERSINGLE
request to modify order
``` json
{
    "event": "modifyordersingle", // [STRING, Mandatory]
    "requestId": "ID",            // [STRING, Mandatory]
    "payload": {
        "orderId": 1000001,          // [LONG, Mandatory if clientOrderId is empty]
        "clientOrderId": "my order", // [STRING]
        "price": 1.2345,             // [DECIMAL]
        "stopPrice": 1.2345,         // [DECIMAL]
        "quantity": 10000            // [DECIMAL]
    }
}
```

response

``` json
{
    "event": "modifyordersingle",
    "requestId": "ID",
}

```

#### NEWORDEROCO
request to place OCO order
    
``` json
{
 {
    "event": "neworderoco",  // [STRING, Mandatory]
    "requestId": "ID",       // [STRING, Mandatory]
    "payload": {
        "login": 123456,                        // [LONG, Mandatory]
        "tillTime": "2022-03-19T07:22:00.000Z", // [STRING, order lifetime in UTC timezone] 
        "symbol": "EURUSD",                     // [STRING, Mandatory]
        "side": "BUY",                          // [ENUM, Mandatory]
        "timeInForce": "AON",                   // [ENUM, Mandatory]
        "quantity": 10000,                      // [DECIMAL]
        "orders": [
            {
                "type": "LIMIT",               // [ENUM, Mandatory]
                "price": 1.2345,               // [DECIMAL, Mandatory for LIMIT and STOP_LIMIT]
                "clientOrderId": "limit order" // [STRING]
            },
            {
                "type": "STOP_LIMIT",               // [ENUM, Mandatory]
                "price": 1.2345,                    // [DECIMAL, Mandatory for LIMIT and STOP_LIMIT]
                "stopPrice": 1.2345,                // [DECIMAL, Mandatory for STOP and STOP_LIMIT]
                "clientOrderId": "stop-limit order" // [STRING]
            }
        ]
    }
}
```

response

``` json
{
    "event": "neworderoco",
    "requestId": "ID",
    "payload": [
        {
            "orderId": 1000001,
            "clientOrderId": "limit order",
            "status": "NEW",
            "type": "LIMIT"
        },
        {
            "orderId": 1000002,
            "clientOrderId": "stop-limit order",
            "status": "NEW",
            "type": "STOP_LIMIT"
        }
    ]
}
```

#### ORDERS
request to get all pending conditional orders (LIMIT, STOP)

``` json
{
    "event": "orders", // [STRING, Mandatory]
    "requestId": "ID"  // [STRING, Mandatory]
}
```
response

``` json
{
    "event": "orders",
    "payload": [
        {
            "orderId": 1000001,
            "symbol": "EURUSD",
            "type": "LIMIT",
            "side": "SELL",
            "clientOrderId": "my first order",
            "price": 1.2345,
            "origQty": 10000,      // original quantity
            "executedQty": 10000,  // executed quantity 
            "status": "NEW",
            "timeInForce": "AON",
            "tillTime": "2022-04-23T18:25:43.511Z",
        },
        {
            "orderId": 1000002,
            "symbol": "EURUSD",
            "type": "STOP",
            "side": "SELL",
            "clientOrderId": "my second order",
            "stopPrice": 1.2345,
            "origQty": 10,
            "executedQty": 10,
            "status": "NEW",
            "timeInForce": "AON",
            "tillTime": "2022-04-23T18:25:43.511Z",
        },
        {
            "orderId": 1000003,
            "symbol": "EURUSD",
            "type": "STOP_LIMIT",
            "side": "SELL",
            "clientOrderId": "my third order",
            "price": 1.0345,
            "stopPrice": 1.2345,
            "origQty": 10,
            "executedQty": 10,
            "status": "NEW",
            "timeInForce": "AON",
            "tillTime": "2022-04-23T18:25:43.511Z",
        }
    ]
}

```

#### ORDERSTATUS
request to get particular order
    
``` json
{
    "event": "orderstatus", // [STRING, Mandatory]
    "requestId": "ID",      // [STRING, Mandatory]
    "payload": {
        "orderId": 1000001,          // [LONG, Mandatory if clientOrderId is empty]
        "clientOrderId": "my order"  // [STRING]
    }
}
```
response

``` json
{
    "event": "orderstatus",
    "requestId": "ID",
    "payload": {
        "symbol": "EURUSD",
        "type": "LIMIT",
        "side": "BUY",
        "orderId": 1000001,
        "tillTime": "2022-05-23T18:25:43.511Z",
        "clientOrderId": "my first order",
        "transactTime": "2022-04-23T18:25:43.511Z",
        "price": 1.2345,
        "origQty": 10000,
        "executedQty": 10000,
        "status": "FILLED",
        "timeInForce": "AON",
        "fills": [
            {
                "price": 1.2345,
                "qty": 5000
            },
            {
                "price": 1.2344,
                "qty": 5000
            }
        ]
    }
}

```

#### ORDERCANCEL
request to cancel particular order

``` json
{
    "event": "ordercancel", // [STRING, Mandatory]
    "requestId": "ID",      // [STRING, Mandatory]
    "payload": {
        "orderId": 1000001,          // [LONG, Mandatory if clientOrderId is empty]
        "clientOrderId": "my order"  // [STRING]
    }
}
```
response

``` json
{
    "event": "ordercancel",
    "requestId": "ID",
}

```

#### EXECUTION
message examples

``` json
{
    "event": "execution",                               // [STRING]
    "payload": {
        "symbol": "EURUSD",
        "type": "MARKET",
        "side": "SELL",
        "orderId": 1000001,
        "clientOrderId": "my order",
        "transactTime":  "2022-04-23T18:25:43.511Z",
        "origQty": 10000,
        "executedQty": 10000,
        "status": "FILLED",
        "timeInForce": "AON",
        "fills": [
            {
                "price": 1.2345,
                "qty": 5000
            },
            {
                "price": 1.2344,
                "qty": 5000
            }
        ]
    }
}

{
    "event": "execution",                               // [STRING]
    "payload": {
        "symbol": "EURUSD",
        "type": "LIMIT",
        "side": "SELL",
        "orderId": 1000001,
        "clientOrderId": "my order",
        "timeInForce": "AON",
        "tillTime":  "2022-04-23T18:25:43.511Z",
        "transactTime":  "2022-04-23T18:25:43.511Z",
        "price": 1.2345,
        "origQty": 10000,
        "executedQty": 0,
        "status": "REJECTED",
        "reason": "lifetime expired",
    }
}

```

#### ERROR
message example
    
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
