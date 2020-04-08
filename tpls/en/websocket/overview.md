# WebSocket APIs

## Base URL

```
wss://ws.loopring.io/v2/ws
```

## Rules

1. The client can subscribe to multiple topics in one operation (batch subscription). If at least one topic requires the ApiKey, the batch subscription will need to provide the ApiKey.
1. Multiple subscriptions to the same topic in a batch subscription are not allowed.
1. The client can subscribe to the same topic with different parameters, but only the latest subscription will become active, all previous subscriptions of the same topic will be canceled automatically.
1. Unsubscription also requires the ApiKey if the corresponding subscription requires the ApiKey. This rule also applies to batch unsubscriptions.

## Heartbeat

After establishing a WebSocket connection with the client, the relayer will perform heartbeat detection by sending a "ping" message every 30 seconds and expecting to receive the client's "pong" message. If the relayer have't received a "Pong" after 4 "ping" messages (2 minutes), the relayer will terminate the connection.


## 请求

|  Field  |     Type     | Required |               Note               |                 Example                 |
| :---- | :---------- | :------ | :------------------------------ | :---------------------------------- |
|   op   |    string    |    Y    |         订阅或订退("sub"或者"unSub")         |                "sub"               |
| apiKey |    string    |    N    | 订阅要求传ApiKey的主题才是必须的 | “16M2hKHw9b5VuP21YBAJQmCd3VhuNtdDqG” |
|  args  | list<string> |    Y    |         订阅的主题及条件         | [ "depth&LRC-ETH&0","trade&LRC-ETH"] |

#### Examples


Subscribtion request:

```json
{
    "op": "sub",
    "args": [
        "candlestick&LRC-ETH&1Hour",
        "depth&LRC-ETH&1",
        "depth10&LRC-ETH&1",
        "trade&LRC-ETH",
        "ticker&LRC-ETH"
    ]
}
```

Unsubscribtion request:

```json
{
    "op": "unSub",
    "args": [
        "candlestick",
        "depth",
        "depth10",
        "trade",
        "ticker"
    ]
}
```

## 返回值

|  Field  |     Type     | Required |               Note               |                 Example                 |
| :---- | :---------- | :------ | :------------------------------ | :---------------------------------- |
|   op   |    string    |    Y    |         用户传送来的操作         |                "sub"                 |
| apiKey |    string    |    N    | 订阅要求传ApiKey的主题才是必须的 | “16M2hKHw9b5VuP21YBAJQmCd3VhuNtdDqG” |
|  args  | list<string> |    Y    |         订阅的主题及条件         | [ "depth&LRC-ETH&0","trade&LRC-ETH"] |
| result |    [Result](#result)   |    Y    |             订阅结果             |                  /                   |


####  <span id="result">Result结构</span>

|  Field  |      Type       | Required |         Note         | Example |
| :---- | :------------- | :------ | :------------------ | :-- |
| status |     string      |    Y    |     订阅是否成功     | "OK" |
| error  | [Error](#error) |    N    | 订阅失败时的错误信息 |  /   |

####   <span id="error">Error结构</span>

|  Field   |  Type   | Required |   Note   |     Example     |
| :----- | :----- | :------ | :------ | :---------- |
|  code   | integer |    Y    |  Status code  |    107500    |
| message | string  |    Y    | 错误信息 | 空的订阅信息 |

#### Status code

| **Status code** |                         Comment                         |
| :-------- | :-------------------------------------------------- |
|   104100   |                     空的订阅信息                     |
|   104101   | 不支持的操作(路印中继服务器仅支持sub 和 unsub操作) |
|   104102   |                     不支持的主题                     |
|   104103   |                    重复的订阅主题                    |
|   104104   |                    缺少ApiKey信息                    |
|   104105   |              与之前订阅使用的ApiKey不符              |
|   104112   |                    不合法的ApiKey                    |
|   104113   |               订退未曾订阅过的主题               |
|   104114   |             无法通过APiKey找到对应的用户             |
|   104115   |                  无法识别的订阅消息                  |

#### 示例

订阅成功示例

```json
{
    "op": "sub",
    "apiKey": "",
    "args": [
        "candlestick&LRC-ETH&1hr",
        "depth&LRC-ETH&1",
        "trade&LRC-ETH",
        "ticker&LRC-ETH"
    ],
    "result": {
        "status": "ok"
    }
}
```

订阅参数不合法的失败示例

```json
{
    "op": "sub",
    "apiKey": "",
    "args": [
        "candlestick&LRC-ETH"
    ],
    "result": {
        "status": "failed",
            "error": {
            "code": 104106,
            "message": "receive illegal arg for candlestick:lrc-eth"
        }
    }
}
```

订阅参数无法解析的失败示例

```json
{
    "op": "",
    "apiKey": "",
    "args": [],
    "result": {
        "status": "failed",
        "error": {
            "code": 104115,
            "message": "unexpected msg:xxx"
        }
    }
}
```
