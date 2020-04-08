# Candlestick更新


通过订阅该主题, 您可以获得特定交易对Candlestick更新的数据推送.


## 订阅规则

- `topic` string:`candlestick`.
- ApiKey **not required**.


## Parameters

|  Parameter | Required |                Note                |
| :---- | :---| :--------------------------------- |
| market |  Y |交易对（支持的交易对可以通过api接口[api/v2/exchange/markets](../dex_apis/getMarkets.md)获取）| 
| interval |  Y |时间间隔|

#### 时间间隔

| 间隔  |  Note  |
| :--- | :---- |
| 1min  | 1分钟  |
| 5min  | 5分钟  |
| 15min | 15分钟 |
| 30min | 30分钟 |
|  1hr  | 1小时  |
|  2hr  | 2小时  |
|  4hr  | 4小时  |
| 12hr  | 12小时 |
|  1d   |  1天   |
|  1w   |  1周   |



## Status code

| Value |                   Note                   |
| :---- | :--------------------------------------- |
| 104106 | invalid `topic` or parameters|

## Push Examples

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
| topic |       JSON        |    Y    | 订阅的主题和参数 |  
|  ts   |            integer            |    时    | 推送时间（毫秒） |      
| data  | [List\[string]](#candlestick) （`Candlestick`列表）|    Y    | cCandlestick数组 |      

####<span id= "candlestick">Candlestick array</span>

| 序号  |  Type   | Required |               Note                |        
| :------ | :----- | :------ | :------------------------------- | 
|    1     | integer |    Y    |            指开盘时间             |     
|    2     | integer |    Y    |             成交笔数              |         
|    3     | string  |    Y    |             开盘价格              |      
|    4     | string  |    Y    |             收盘价格              |       
|    5     | string  |    Y    |              最高价               |       
|    6     | string  |    Y    |              最低价               |      
|    7     | string  |    Y    | 为wei为单位的base token的成交数量 | 
|    8     | string  |    Y    | 为wei为单位 quote token的成交数量 | 
