# Candlestick更新


通过订阅该主题, 您可以获得特定交易对Candlestick更新的数据推送.


## Rules

- `topic` string:`candlestick`.
- ApiKey **not required**.


## Parameters

|  Parameter | Required |                Note                |
| :---- | :---| :--------------------------------- |
| market |  Y |[Trading pair](../dex_apis/getMarkets.md)| 
| interval |  Y |Time interval|

#### Time intervals

| Value  |  Note  |
| :--- | :---- |
| 1min  | 1 minute  |
| 5min  | 5 minutes  |
| 15min | 15 minutes |
| 30min | 30 minutes |
|  1hr  | 1 hour  |
|  2hr  | 2 hours  |
|  4hr  | 4 hours |
| 12hr  | 12 hours |
|  1d   |  1 day   |
|  1w   |  1 week   |



## Status code

| Value |                   Note                   |
| :---- | :--------------------------------------- |
| 104106 | invalid `topic` or parameters|

## Push data examples

```json
{
    "topic": "candlestick%3Fmarket%3DLRC-ETH%26interval%3D1hr",
    "ts":1584717910000,
    "data": [
        "1584717910000",  //start
        "5000",  //count
        "3997.3",  //open
        "3998.7",  //close
        "4031.9",  //high
        "3982.5",  //low
        "500000000000000000",  //size
        "2617521141385000000",  //volume
    ]
}
```

# Data Model

# Push data structure

| Field  |             Type              | Required |       Note       |    
| :--- | :--------------------------- | :------ | :-------------- | 
| topic |       JSON        |    Y    | Topic and parameters |  
|  ts   |            integer            |    时    | Push timestamp（毫秒） |      
| data  | [List\[string]](#candlestick) （`Candlestick`列表）|    Y    | cCandlestick数组 |      

####<span id= "candlestick">Candlestick array</span>

| Index  |  Type   | Required |               Note                |        
| :------ | :----- | :------ | :------------------------------- | 
|    1     | integer |    Y    |            指开盘时间             |     
|    2     | integer |    Y    |             成交笔数              |         
|    3     | string  |    Y    |             开盘价格              |      
|    4     | string  |    Y    |             收盘价格              |       
|    5     | string  |    Y    |              最高价               |       
|    6     | string  |    Y    |              最低价               |      
|    7     | string  |    Y    | 为wei为单位的base token的成交数量 | 
|    8     | string  |    Y    | 为wei为单位 quote token的成交数量 | 
