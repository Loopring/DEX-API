# 最新成交更新

通过订阅该主题, 您可以获得特定交易对全部用户新成交记录的数据推送.


## 订阅规则

- `topic` string:`trade`.
- ApiKey **not required**.


## Parameters

|  Parameter |   Required |             Note                |
| :---- | :---|:--------------------------------- |
| market |  Y |交易对（支持的交易对可以通过api接口[api/v2/exchange/markets](../dex_apis/getMarkets.md)获取）|


## Status code

| Value |                Note                |
| :---- | :--------------------------------- |
| 104109 | invalid `topic` or parameters|

## Push Examples

```json
{
    "topic": {
        "topic": "trade",
        "market": "LRC-ETH"
    },
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

# Data Model

# Push data structure

|  Field   |          Type           | Required |       Note       |    
| :----- | :--------------------- | :------ | :-------------- |
| topic |       JSON        |    Y    | 订阅的主题和参数 |  
| integer |         integer         |    Y    |     推送时间     | 
|  data   | [List[List\[string]](#trade)] |    Y    |    Trade数组列表     |  

#### <span id="trade">Trade array</span>

| 序号  |  Type   | Required |         Note         |  
| :------ | :----- | :------ | :------------------ | 
|    1     | integer |    Y    |       成交时间       | 
|    2     | integer |    Y    |       交易编号       |   
|    3     | string  |    Y    |  买或者卖, 指taker   |    
|    4     | string  |    Y    | base token的成交数量 |  
|    5     | string  |    Y    |       成交价格       |   
|    6     | string  |    Y    |   base token的收费   |    

