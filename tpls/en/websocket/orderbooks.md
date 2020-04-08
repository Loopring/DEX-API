# Orderbooks

Subscribe to this topic to receive changes on the orderbook for the specific trading pair.




## Subscription

- `topic` must specify a trading pair and the aggretation level。If the traidng pair is `LRC-ETH`，and the aggretation level is `1`, then `topic` should be：`"depth&LRC-ETH&1"`.
- You need to provide your ApiKey.
- You can get the list of supported trading pairs through [api/v2/exchange/markets](../dex_apis/getMarkets.md).



## Status code

| Value |                Note                 |
| :---- | :--------------------------------- |
| 104107 | Invalid or unsupported `topic`|

## Push data example

```json
{
    "topic": "depth&LRC-ETH&1",
    "ts": 1584717910000,
    "startVersion": 1212121,
    "endVersion": "1212123",
    "data": {
        "bids": [
            [
                "295.97",  //price
                "456781000000000",  //size
                "3015000000000",  //volume
                "4"  //count
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

|     Field     |      Type       | Required |         Note         |       Example        |
| :---------- | :------------- | :------ | :------------------ | :--------------- |
|    topic     |     string      |    Y    |   Topic and parameters   | "depth&LRC-ETH&1" |
|      ts      |     integer     |    Y    |       Time of change       |   1584717910000   |
| startVersion |     integer     |    Y    | Previous verion |      1212121      |
|  endVersion  |     integer     |    Y    | Current version |      1212123      |
|     data     | [Depth](#depth) |    Y    |        Orderbook changes     |         /         |

####<span id="depth">`Depth` object</span>

| Field | Type                           | Required | Note     | Example |
| :---- | :------------------------------ | :-------- | :-------- | :---- |
| bids | [List\[List\[string\]]](#slot) (a list of `DepthItem`)| Y       | 买单深度 | /    |
| asks | [List\[List\[string\]]](#slot) (a list of `DepthItem`)| Y       | 卖单深度 | /    |

#### <span id = "slot">`DepthItem` array</span>

`asks`和`bids`数组中的每个子数组都是定长数组，我们称之为*DepthItem*，其规范如下：

| Index  | Type   | Required | Note           | Example       |
| :------ | :------ | :-------- | :-------------- | :---------- |
|    1     | string | Y       | Price           | "0.002"    |
|    2     | string | Y       | Amount (Quantity of base token)         | "21000"    |
|    3     | string | Y       | Total (Quantity of quote token)    | "33220000" |
|    4     | string | Y       | 聚合的订单数目 | "4"        |


需要注意的是，每一个推送中的数量和成交额代表这个价格目前的数量和成交额的绝对值，而不是相对变化。


## 构建本地订单簿

您可以通过下列步骤构建本地订单簿：

1. 订阅 depth主题。
2. 开始缓存收到的更新。同一个价位，后收到的更新覆盖前面的。
3. 访问接口 [api/v1/depth](../dex_apis/getDepth.md) 获得一个全量的深度快照。
4. 3中获取的快照如果`version`大于本地`version`(`endVersion`)，则直接覆盖，如果小于本地version，则相同的价格不覆盖，不同的价格则覆盖。
5. 将深度快照中的内容更新到本地订单簿副本中，并从WebSocket接收到的第一个`startVersion` <=本地 `version + 1` 且 endVersion >= 本地version 的event开始继续更新本地副本。
6. 每一个新推送的`startVersion`应该恰好等于上一个event的`endVersion + 1`，否则可能出现了丢包，请从step3重新进行初始化。
7. 如果某个价格对应的挂单量为0，表示该价位的挂单已经撤单或者被吃，应该移除这个价位。