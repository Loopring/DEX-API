# 最新成交

通过订阅该主题，您可以获得特定交易对全部用户新成交记录的数据推送。


## 订阅规则

- `topic`字符串：`trade`。
- 订阅该主题**不需要**提供ApiKey。


## 参数列表

| 参数名|   必现|             描述                 |
| :---- | :---|:--------------------------------- |
| market | 是|交易对（支持的交易对可以通过api接口[api/v2/exchange/markets](../dex_apis/getMarkets.md)获取）|


## 状态码

| 状态码 |                描述                 |
| :---- | :--------------------------------- |
| 104109 | `topic`的值或其参数非法|

## 推送示例

```json
{
    "topics": "trade%3Fmarket%3DLRC-ETH",
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

## 模型

#### `data`数据结构

|  字段   |          类型           | 必现 |       说明       |    
| :----- | :--------------------- | :------ | :-------------- |
|  topic  |         string          |    是    | 订阅的主题和条件 | 
| integer |         integer         |    是    |     推送时间     | 
|  data   | [List[List\[string]](#trade)] （`Trade`列表）|    是    |     深度信息     |  

#### <span id="trade">`Trade`数据结构</span>

| 序号  |  类型   | 必现 |         说明         |  
| :------ | :----- | :------ | :------------------ | 
|    1     | integer |    是    |       成交时间       | 
|    2     | integer |    是    |       交易编号       |   
|    3     | string  |    是    |  买或者卖，指taker   |    
|    4     | string  |    是    | base token的成交数量 |  
|    5     | string  |    是    |       成交价格       |   
|    6     | string  |    是    |   base token的收费   |    

