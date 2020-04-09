# Orderbook

Subscribe to this topic to receive notifications about orderbook updates for specific trading pairs.

## Rules

- Topic name: `orderbook`
- ApiKey requred: No


## Parameters

|  Parameter |  Required |             Note                |
| :---- | :------ |:--------------------------------- |
| market | Y | [Trading pair](../dex_apis/getMarkets.md)|
| level | Y | 深度聚合级别 |
| count | Y | 买卖深度条目数量, 值不可以超过50. |
| snapshot |否 | 默认为false. 如果该值为true, count不可以大于20, 并且当深度条目有任何一条变化, 那么指定数量的深度条目会被全量推送给客户端. |

## Status code

| Value |                Note                |
| :---- | :--------------------------------- |
| 104107 | Invalid topic or parameters|

## Notification example

```json
{
    "topic": {
        "topic:": "orderbook",
        "market": "LRC-USDT",
        "count": 20,
        "snapshot": true
    },
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

## Data Model

#### Notification

|     Field     |      Type       | Required |         Note         |    
| :---------- | :------------- | :------ | :------------------ | 
| topic |       JSON        |    Y    | Topic and parameters |  
|      ts      |     integer     |    Y    |       Push timestamp (milliseconds)       |  
| startVersion |     integer     |    Y    | 该次推送的起始版本号 |     
|  endVersion  |     integer     |    Y    | 该次推送的终结版本号 |     
|     data     | [Depth](#depth) |    Y    |       深度信息       |     

####<span id="depth">Depth</span>

| Field | Type                           | Required | Note     | 
| :---- | :------------------------------ | :-------- | :-------- |
| bids | [List\[List\[string\]]](#slot) | Y       | 代表买单深度的DepthItem array list |
| asks | [List\[List\[string\]]](#slot)| Y       | 代表卖单深度的DepthItem array list | 

#### <span id = "slot">DepthItem</span>

`asks`和`bids`数组中的每个子数组都是定长数组, 我们称之为*深度条目*, 其规范如下：

| Index  | Type   | Required | Note           | 
| :------ | :------ | :-------- | :-------------- | :
|    1     | string | Y       | 价格           | 
|    2     | string | Y       | 数量（基础通证的数量）         | 
|    3     | string | Y       | 成交额（ 计价通证的数量）  |
|    4     | string | Y       | 聚合的订单数目 | 


需要注意的是, 每一个推送中的数量和成交额代表这个价格目前的数量和成交额的绝对值, 而不是相对变化.

## 构建本地订单簿

您可以通过下列步骤构建本地订单簿：

1. 订阅 depth主题.
2. 开始缓存收到的更新.同一个价位, 后收到的更新覆盖前面的.
3. 访问接口 [api/v1/depth](../dex_apis/getDepth.md) 获得一个全量的深度快照.
4. 3中获取的快照如果`version`大于本地`version`（`endVersion`）, 则直接覆盖, 如果小于本地version, 则相同的价格不覆盖, 不同的价格则覆盖.
5. 将深度快照中的内容更新到本地订单簿副本中, 并从WebSocket接收到的第一个`startVersion` <=本地 `version + 1` 且 endVersion >= 本地version 的event开始继续更新本地副本.
6. 每一个新推送的`startVersion`应该恰好等于上一个event的`endVersion + 1`, 否则可能出现了丢包, 请从step3重新进行初始化.
7. 如果某个价格对应的挂单量为0, 表示该价位的挂单已经撤单或者被吃, 应该移除这个价位.