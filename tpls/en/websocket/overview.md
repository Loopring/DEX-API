# WebSocket APIs

## 接入URL

```
wss://ws.loopring.io/v2/ws
```

## Subscription
Clients can send JSON to subscribe to multiple topics:

```JSON
 {
    "op":"sub",
    "sequence": 10000,
    "apiKey": ".....",
    "unsubscribeAll": true,
    "topics": [
        {
            "topic": "account"
        },
        {
            "topic": "order",
            "market": "LRC-ETH"
        },
        {
            "topic": "order",
            "market": "LRC-USDT"
        },
        {
            "topic:": "depth",
            "market": "LRC-ETH",
            "count": 10
        },
        {
            "topic:": "depth",
            "market": "LRC-USDT",
            "count": 20,
            "snapshot": true
        }
    ]
  },
```


1. In one subscription request, if at least one topic requires the ApiKey, then the `apiKey` filed is required;
1. In one subscription request, the same topic configuration can only occur once;
1. In one subscription request, if there are any configuration errors, the entire subscription request fails;
1. If `unsubscribeAll` is `true`, all previous subscriptions will be canceled;
1. If `sequence` is provided, the relayer will use the same sequence number in its response.



## Unsubscription
Clients can send JSON to unsubscribe from multiple topics:

```JSON
 {
    "op":"unSub",
    "sequence": 10000,
    "apiKey": ".....",
    "unsubscribeAll": false,
    "topics": [
        {
            "topic": "account",
        },
        {
            "topic": "order",
            "market": "LRC-ETH"
        },
        {
            "topic": "order",
            "market": "LRC-USDT"
        },
        {
            "topic:": "depth",
            "unsubscribeAll":true
        }
    ]
  },
```


1. In one unsubscription request, if at least one topic requires the ApiKey, then the `apiKey` filed is required;
1. In one unsubscription request, the same topic configuration can only occur once;
1. In one unsubscription request, if there are any configuration errors, the entire unsubscription request fails;
1. If the top-level `unsubscribeAll` is `true`, all previous subscriptions will be canceled; if the per-topic `unsubscribeAll` is `true`, then all subscriptions to that topic will be canceled;
1. If `sequence` is provided, the relayer will use the same sequence number in its response.

#### 心跳

WebSocket链接建立后, 中继会每30秒会发送“ping”消息给客户端做心跳检测.如果客户端在最近2分钟内都没有任何“pong”消息, 中继会断开WebSocket链接.如果客户端的“pong”消息数量超过服务端发送的“ping”消息数量, 中继也会断开WebSocket链接.


## 返回值

|  Field  |     Type     | Required |               Note               |      
| :---- | :---------- | :------ | :------------------------------ |
|   op   |    string    |    Y    |         订阅（"sub"）或订退（unSub"）         |    
|   sequence   |    integer    |    N    |        操作序列号        |   
| topics |   JSON  |    Y    |             Topics and their configurations            | 
| result |    [Result](#result)   |    Y    |             Subscription result             |            


####  <span id="result">Result结构</span>

|  Field  |      Type       | Required |         Note         | 
| :---- | :------------- | :------ | :------------------ |
| status |     string      |    Y    |     Status code     | 
| error  | [Error](#error) |    N    | Error | 

####   <span id="error">Error结构</span>

|  Field   |  Type   | Required |   Note   |     
| :----- | :----- | :------ | :------ | 
|  code   | integer |    Y    |  Value  |  
| message | string  |    Y    | Error message | 

#### Status code

| **Value** |                         Note                        |
| :-------- | :-------------------------------------------------- |
|   104100   |                     空的订阅信息                     |
|   104101   | 不支持的操作（路印中继服务器仅支持sub 和 unsub操作） |
|   104102   |                     不支持的主题                     |
|   104103   |                    重复的订阅主题                    |
|   104104   |                    缺少ApiKey信息                    |
|   104105   |              与之前订阅使用的ApiKey不符              |
|   104112   |                    不合法的ApiKey                    |
|   104113   |               订退未曾订阅过的主题               |
|   104114   |             无法通过APiKey找到对应的用户             |
|   104115   |                  无法识别的订阅消息                  |

#### Examples

A successful subscription：

```json
{
    "op": "sub",
    "sequence": 10000,
    "topic": {
        "topic:": "depth",
        "market": "LRC-ETH",
        "count": 10
    },
    "result": {
        "status": "ok"
    }
}
```

A failed subscription：

```json
{
    "op": "sub",
    "sequence": 10000,
    "topic": {
        "topic:": "depth",
        "market": "LRC-ETH",
        "count": 10
    },
    "result": {
        "status": "failed",
        "error": {
            "code": 104106,
            "message": "receive illegal arg for candlestick:lrc-eth"
        }
    }
}
```

Another failed subscription：

```json
{
    "op": "",
    "sequence": 10000,
    "topic": {
        "topic:": "depth",
        "market": "LRC-ETH",
        "count": 10
    },
    "result": {
        "status": "failed",
        "error": {
            "code": 104115,
            "message": "unexpected msg:xxx"
        }
    }
}
```
