# 订阅用户订单更新


通过订阅该主题，您可以获得用户在指定交易对的订单状态提送。

## Subscription rules
- `topic`需要指定交易对。如果交易对是`LRC-ETH`，那么`topic`应该拼写为：`order&LRC-ETH`。
- 订阅该主题**需要提供ApiKey**。
- 支持的交易对可以通过api接口[api/v2/exchange/markets](../dex_apis/getMarkets.md)获取。

## Status code

| Value |                Comment                 |
| :---- | :--------------------------------- |
| 104110 | Invalid or unsupported `topic`|

## Push data example

```json
{
   "topic": "order&LRC-BTC",
   "ts":1565844328,
   "data": {
        "hash": "11212",
        "clientOrderId": "myOrder",
        "size": "500000000",
        "volume": "210000000",
        "price": "0.000004",
        "filledSize": "30000000",
        "filledVolume": "100000",
        "filledFee": "1000000",
        "status": "processing",
        "createdAt": "1494900087",
        "validSince": "1494900087",
        "validUntil": "1495900087",
        "side": "buy",
        "market": "LRC-BTC"
    }
}
```

## Data Model

#### Data Structure

| Field  |      Type       | Required |       Note       |      Example       |
| :--- | :------------- | :------ | :-------------- | :------------- |
| topic |     string      |    Y    | Topic and parameters | "order&LRC-ETH" |
|  ts   |     integer     |    Y    |     推送时间     |  1584717910000  |
| data  | [Order](#order) |    Y    |     订单信息     |        /        |

#### <span id="order">OrderStructure</span>

|     Field      |  Type   | Required |            Note            |     Example      |
| :----------- | :----- | :------ | :------------------------ | :----------- |
|     hash      | string  |    Y    |          订单哈希          |    "11212"    |
| clientOrderId | string  |    Y    |        用户自定义id        |   "myOrder"   |
|     size      | string  |    Y    |     base token 的数量      |  "500000000"  |
|    volume     | string  |    Y    |     quote token 的数量     |  "210000000"  |
|     price     | string  |    Y    |          订单价格          |  "0.000004"   |
|  filledSize   | string  |    Y    | 已经成交的basetoken的数量  |  "30000000"   |
| filledVolume  | string  |    Y    | 已经成交的quotetoken的数量 |   "100000"    |
|   filledFee   | string  |    Y    |       已支付的手续费       |   "1000000"   |
|    status     | string  |    Y    |          订单状态          | "processing"  |
|   createdAt   | integer |    Y    |        订单创建时间        | 1584717910000 |
|   updateAt    | integer |    Y    |   订单最后一次的更新时间   | 1584717910000 |
|     side      | string  |    Y    |           买或卖           |     "buy"     |
|    market     | string  |    Y    |            交易对            |   "LRC-ETH"   |

#### 订单状态取值范围

|    状态    |                    Note                    |
| :-------- | :---------------------------------------- |
| processing | 订单进行中，订单等待成交或者已经成交一部分 |
| processed  |                订单完全成交                |
| cancelling |                   取消中                   |
| cancelled  |                  订单取消                  |
|  expired   |                  订单过期                  |
|  waiting   |                订单还未生效                |
