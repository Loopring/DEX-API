# 订阅最新成交

通过订阅该主题，您可以获得特定交易对全部用户新成交记录的数据推送。


## Subscription rules

- `topic`需要指定交易对。如果交易对是`LRC-ETH`，那么`topic`应该拼写为：`trade&LRC-ETH`。
- 订阅该主题不需要提供ApiKey。
- 支持的交易对可以通过api接口[api/v2/exchange/markets](../dex_apis/getMarkets.md)获取。




## Status Code

| Status Code |                Comment                 |
| :---- | :--------------------------------- |
| 104109 | Invalid or unsupported `topic`|

## Push data example

```json
{
    "topic": "trade&LRC-ETH",
    "ts": 1584717910000,
    "data": [
        [
            "1584717910000",  //timestamp
            "123456789",  //tradeId
            "buy",  //side
            "500000",  //size 
            "0.0008",  //price
            "100"  //fee
        ]
    ]
}
```

## Data Model

#### 推送Structure

|  Field   |          Type           | Required |       Note       |      Example       |
| :----- | :--------------------- | :------ | :-------------- | :------------- |
|  topic  |         string          |    Y    | Topic and parameters | "trade&LRC-ETH" |
| integer |         integer         |    Y    |     推送时间     |  1584717910000  |
|  data   | [List[List\[string]](#trade)] （Trade列表）|    Y    |     深度信息     |        /        |

#### <span id="trade">Trade Structure</span>

| Index  |  Type   | Required |         Note         |     Example      |
| :------ | :----- | :------ | :------------------ | :----------- |
|    1     | integer |    Y    |       成交时间       | 1584717910000 |
|    2     | integer |    Y    |       交易编号       |   123456789   |
|    3     | string  |    Y    |  买或者卖，指taker   |     "buy"     |
|    4     | string  |    Y    | base token的成交数量 |   "500000"    |
|    5     | string  |    Y    |       成交价格       |   "0.0008"    |
|    6     | string  |    Y    |   base token的收费   |     "100"     |

