# 订阅前10买卖单全量推送

通过订阅该主题，您可以每1秒获得指定交易对前10条卖单和买单的**全量**数据推送 - 即使数据没有任何变化。

## Subscription

- `topic`需要指定交易对和深度聚合等级。如果交易对是`LRC-ETH`，深度聚合等级是`1`，那么`topic`应该拼写为：`depth10&LRC-ETH&1`。
- 订阅该主题不需要提供ApiKey。
- 交易对和深度聚合等级可以通过[api/v2/exchange/markets接口](../dex_apis/getMarkets.md)获取。


## Status code

| Value |                 Note                  |
| :---- | :----------------------------------- |
| 104108 | Invalid or unsupported `topic`|

##  Push data example

```json
{
    "topic": "depth10&LRC-BTC&1",
    "ts": 1565844208,
    "data": {
        "bids": [
            [
              "295.97",  //price
              "4567810000000000",  //size
              "30150000000000",  //volume
              "2"  //count
            ]
        ],
        "asks": [
            [
              "298.97",
              "456781000000000000",
              "301500000000000",
              "2"
            ]
        ]
    }
}
```

## Model

#### Data object

| Field  |      Type       | Required |       Note       |        Example         |
| :--- | :------------- | :------ | :-------------- | :----------------- |
| topic |     string      |    Y    | Topic and parameters | "depth10&LRC-ETH&1" |
|  ts   |     integer     |    Y    |     Time of change     |    1584717910000    |
| data  | [Depth](#depth) |    Y    |     Orderbook data     |          /          |

####<span id="depth">Depthobject</span>

| Field | Type                           | Required | Note     | Example |
| :---- | :------------------------------ | :-------- | :-------- | :---- |
| bids | [List\[List\[string\]]](#slot) (深度条目列表)| Y       | Changed bids | /    |
| asks | [List\[List\[string\]]](#slot) (深度条目列表)| Y       | Changed asks | /    |

### <span id = "slot">深度条目</span>

`asks`和`bids`数组中的每个子数组都是定长数组，我们称之为*深度条目*，其规范如下：

| Index  | Type   | Required | Note           | Example       |
| :------ | :------ | :-------- | :-------------- | :---------- |
|    1     | string | Y       | Price           | "0.002"    |
|    2     | string | Y       | Quantity of base token         | "21000"    |
|    3     | string | Y       | Quantity of quote token       | "33220000" |
|    4     | string | Y       | Number of orders aggregated | "4"        |
