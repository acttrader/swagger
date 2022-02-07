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
* [ENUMS](#enums) 


##### ENUMS

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

status enum:
* NEW
* REJECTED
* FILLED
* PARTIAL_FILL
* CANCELED

reason enum:
* OCO_CONDITION     // order rejected after OCO order was triggered
* LIFETIME_EXPIRED  // order rejected by lifetime expiration
* USER              // order closed by user
  

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

response

``` json
{
    "event": "newordersingle",
    "requestId": "ID",
    "payload": {
        "orderId": 1000001,
        "clientOrderId": "my first order",
        "status": "NEW",
        "type": "LIMIT",
        "login": 123456 
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
        "quantity": 10000,            // [DECIMAL]
        "tillTime": "2022-04-23T18:25:43.511Z" // [STRING, pass "" if you want to reset tillTime]
    }
}
```

response

``` json
{
    "event": "modifyordersingle",
    "requestId": "ID",
    "payload": {
        "orderId": 1000001,
        "clientOrderId": "my order",
        "price": 1.2345,         
        "stopPrice": 1.2345,     
        "quantity": 10000,
        "tillTime": "2022-04-23T18:25:43.511Z"       
    }
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
            "type": "LIMIT",
            "login": 123456 

        },
        {
            "orderId": 1000002,
            "clientOrderId": "stop-limit order",
            "status": "NEW",
            "type": "STOP_LIMIT",
            "login": 123456 

        }
    ]
}
```

#### ORDERS
request to get all pending conditional orders (LIMIT, STOP) plus closed from fromTime 

``` json
{
    "event": "orders",  // [STRING, Mandatory]
    "requestId": "ID",  // [STRING, Mandatory]
    "payload": {
       "fromTime" : "2022-01-01T22:33:44.555Z"
   }
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
        "transactTime": "2022-04-23T18:25:43.511Z", // transaction time in UTC timezone
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
     "payload": {
        "orderId": 1000001,    
        "clientOrderId": "my order",
        "status": "CANCELED",
        "reason": "USER"
    }
}
```

#### EXECUTION
message examples

``` json
{
    "event": "execution",                             // [STRING]
    "payload": {
        "symbol": "EURUSD",
        "type": "MARKET",
        "side": "SELL",
        "orderId": 1000001,
        "clientOrderId": "my order",
        "transactTime":  "2022-04-23T18:25:43.511Z",  // transaction time in UTC timezone
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
    "event": "execution",                            // [STRING]
    "payload": {
        "symbol": "EURUSD",
        "type": "LIMIT",
        "side": "SELL",
        "orderId": 1000001,
        "clientOrderId": "my order",
        "timeInForce": "AON",
        "tillTime":  "2022-04-23T18:25:43.511Z",
        "transactTime":  "2022-04-23T18:25:43.511Z", // transaction time in UTC timezone
        "price": 1.2345,
        "origQty": 10000,
        "executedQty": 0,
        "status": "CANCELED",
        "reason": "lifetime expired",                // [STRING] reject reason
    }
}

```

#### ERROR
message examples
    
``` json
{
    "event": "error",
    "requestId": "ID",
    "payload": {
        "event": "request event name", // request event, could be empty
        "code": 123345,
        "error": "error message"
    }
}
```

``` json
{
    "event": "error",
    "requestId": "ID",
    "payload": {
        "event": "ordercancel", 
        "code": 400,
        "error": "ORDER_NOT_EXISTS"
    }
}
```

``` json
{
    "event": "error",
    "requestId": "ID",
    "payload": {
        "event": "ordercancel", 
        "code": 401,
        "error": "ORDER_ALREADY_CLOSED"
    }
}
```
