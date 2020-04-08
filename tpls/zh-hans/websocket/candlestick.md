# Candlestick更新


通过订阅该主题，您可以获得特定交易对Candlestick更新的数据推送。


## 订阅规则

- `topic`字符串：`candlestick`。
- 订阅该主题**不需要**提供ApiKey。


## 参数列表

| 参数名| 必现 |                描述                 |
| :---- | :---| :--------------------------------- |
| market | 是|交易对（支持的交易对可以通过api接口[api/v2/exchange/markets](../dex_apis/getMarkets.md)获取）| 
| interval | 是|时间间隔|

#### `interval`的取值

| 间隔  |  说明  |
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



## 状态码

| 状态码 |                   描述                    |
| :---- | :--------------------------------------- |
| 104106 | `topic`的值或其参数非法|

## 推送示例

```json
{
    "topics": "candlestick%3Fmarket%3DLRC-ETH%26interval%3D1hr",
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

## 模型

#### `data`数据结构

| 字段  |             类型              | 必现 |       说明       |    
| :--- | :--------------------------- | :------ | :-------------- | 
| topics |            string             |    是    | 订阅的主题和条件 |
|  ts   |            integer            |    时    | 推送时间（毫秒） |      
| data  | [List\[string]](#candlestick) （`Candlestick`列表）|    是    | candlestick数据  |      

####<span id= "candlestick">`Candlestick`数据结构</span>

| 序号  |  类型   | 必现 |               说明                |        
| :------ | :----- | :------ | :------------------------------- | 
|    1     | integer |    是    |            指开盘时间             |     
|    2     | integer |    是    |             成交笔数              |         
|    3     | string  |    是    |             开盘价格              |      
|    4     | string  |    是    |             收盘价格              |       
|    5     | string  |    是    |              最高价               |       
|    6     | string  |    是    |              最低价               |      
|    7     | string  |    是    | 为wei为单位的base token的成交数量 | 
|    8     | string  |    是    | 为wei为单位 quote token的成交数量 | 
