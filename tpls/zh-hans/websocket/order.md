# 用户订单更新


通过订阅该主题，您可以获得用户在指定交易对的订单状态提送。

## 订阅规则

- `topic`字符串：`order`。
- 订阅该主题**需要**提供ApiKey。


## 参数列表

| 参数名|  必现|              描述                 |
| :---- | :--- | :--------------------------------- |
| market | 是 | 交易对（支持的交易对可以通过api接口[api/v2/exchange/markets](../dex_apis/getMarkets.md)获取）|

## 状态码

| 状态码 |                描述                 |
| :---- | :--------------------------------- |
| 104110 | `topic`的值或其参数非法|

## 推送示例

```json
{
   "topic": {
        "topic": "order",
        "market": "LRC-ETH"
   },
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
        "market": "LRC-ETH"
    }
}
```

## 模型

#### 推送消息数据结构

| 字段  |      类型       | 必现 |       说明       |     
| :--- | :------------- | :------ | :-------------- | 
| topic |       JSON        |    是    | 订阅的主题和参数 |  
|  ts   |     integer     |    是    |     推送时间     |  
| data  | [Order](#order) |    是    |     订单数据     |    

#### <span id="order">Order数据结构</span>

|     字段      |  类型   | 必现 |            说明            |    
| :----------- | :----- | :------ | :------------------------ | 
|     hash      | string  |    是    |          订单哈希          |    
| clientOrderId | string  |    是    |        用户自定义id        |  
|     size      | string  |    是    |     base token 的数量      | 
|    volume     | string  |    是    |     quote token 的数量     | 
|     price     | string  |    是    |          订单价格          |  
|  filledSize   | string  |    是    | 已经成交的basetoken的数量  |  
| filledVolume  | string  |    是    | 已经成交的quotetoken的数量 |   
|   filledFee   | string  |    是    |       已支付的手续费       | 
|    status     | string  |    是    |          订单状态          | 
|   createdAt   | integer |    是    |        订单创建时间        | 
|   updateAt    | integer |    是    |   订单最后一次的更新时间   | 
|     side      | string  |    是    |           买或卖           |    
|    market     | string  |    是    |            交易对            |  

#### 订单状态取值范围

|    状态    |                    说明                    |
| :-------- | :---------------------------------------- |
| processing | 订单进行中，订单等待成交或者已经成交一部分 |
| processed  |                订单完全成交                |
| cancelling |                   取消中                   |
| cancelled  |                  订单取消                  |
|  expired   |                  订单过期                  |
|  waiting   |                订单还未生效                |
