# 用户订单更新


Subscribe to this topic to receive notifications about order updates for specific trading pairs.

## Rules

- Topic name: `order`
ApiKey requred: Yes


## Parameters

|  Parameter |   Required |              Note                |
| :---- | :--- | :--------------------------------- |
| market | Y | [Trading pair](../dex_apis/getMarkets.md)|

## Status code

| Value |                Note                |
| :---- | :--------------------------------- |
| 104110 | Invalid topic or parameters|

## Notification example

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

## Data Model

#### Notification

| Field  |      Type       | Required |       Note       |     
| :--- | :------------- | :------ | :-------------- | 
| topic |       JSON        |    Y    | Topic and parameters |  
|  ts   |     integer     |    Y    |     Push timestamp (milliseconds)     |  
| data  | [Order](#order) |    Y    |     订单数据     |    

#### <span id="order">Order</span>

|     Field      |  Type   | Required |            Note            |    
| :----------- | :----- | :------ | :------------------------ | 
|     hash      | string  |    Y    |          订单哈希          |    
| clientOrderId | string  |    Y    |        用户自定义id        |  
|     size      | string  |    Y    |     base token 的数量      | 
|    volume     | string  |    Y    |     quote token 的数量     | 
|     price     | string  |    Y    |          订单价格          |  
|  filledSize   | string  |    Y    | 已经成交的basetoken的数量  |  
| filledVolume  | string  |    Y    | 已经成交的quotetoken的数量 |   
|   filledFee   | string  |    Y    |       已支付的手续费       | 
|    status     | string  |    Y    |          订单状态          | 
|   createdAt   | integer |    Y    |        订单创建时间        | 
|   updateAt    | integer |    Y    |   订单最后一次的更新时间   | 
|     side      | string  |    Y    |           买或卖           |    
|    market     | string  |    Y    |            交易对            |  

#### Order status

|    状态    |                    Note                    |
| :-------- | :---------------------------------------- |
| processing | 订单进行中, 订单等待成交或者已经成交一部分 |
| processed  |                订单完全成交                |
| cancelling |                   取消中                   |
| cancelled  |                  订单取消                  |
|  expired   |                  订单过期                  |
|  waiting   |                订单还未生效                |
