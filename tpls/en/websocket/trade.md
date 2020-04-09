# 最新成交更新

Subscribe to this topic to receive notifications about bew trades for specific trading pairs.

## Rules

- Topic name: `trade`
- ApiKey requred: No


## Parameters

|  Parameter |   Required |             Note                |
| :---- | :---|:--------------------------------- |
| market |  Y |[Trading pair](../dex_apis/getMarkets.md)|


## Status code

| Value |                Note                |
| :---- | :--------------------------------- |
| 104109 | Invalid topic or parameters|

## Notification example

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

## Data Model

#### Notification

|  Field   |          Type           | Required |       Note       |    
| :----- | :--------------------- | :------ | :-------------- |
| topic |       JSON        |    Y    | Topic and parameters |  
| ts |         integer         |    Y    |     Push timestamp (milliseconds)     | 
|  data   | [List[List\[string]](#trade)] |    Y    |    Trade array list     |  

#### <span id="trade">Trade</span>

| Index  |  Type   | Required |         Note         |  
| :------ | :----- | :------ | :------------------ | 
|    1     | integer |    Y    |       成交时间       | 
|    2     | integer |    Y    |       交易编号       |   
|    3     | string  |    Y    |  买或者卖, 指taker   |    
|    4     | string  |    Y    | base token的成交数量 |  
|    5     | string  |    Y    |       成交价格       |   
|    6     | string  |    Y    |   base token的收费   |    

