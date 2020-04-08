# Ticker更新

通过订阅该主题, 您可以获得特定交易对ticker更新的数据推送.




## 订阅规则

- `topic` string:`ticker`.
- ApiKey **not required**.


## Parameters

|  Parameter |  Required |              Note                |
| :---- | :--- |:--------------------------------- |
| market | Y | 交易对（支持的交易对可以通过api接口[api/v2/exchange/markets](../dex_apis/getMarkets.md)获取）|



## Status code

| Value |                 Note                |
| :---- | :---------------------------------- |
| 104111 | invalid `topic` or parameters|

## Push Examples

```json
{
    "topic": {
        "topic": "ticker",
        "market": "LRC-ETH"
    },
    "ts": 1584717910000,
    "data": [
        "LRC-ETH",  //market
        "1584717910000",  //timestamp
        "5000000",  //size
        "1000",  //volume
        "0.0002",  //open
        "0.00025",  //high
        "0.0002",  //low
        "0.00025",  //close       
        "5000",  //count    
        "0.00026",  //bid
        "0.00027"  //ask
    ]
}
```

# Data Model

# Push data structure

|  Field   |          Type           | Required |       Note       |    
| :----- | :--------------------- | :------ | :-------------- | 
| topic |       JSON        |    Y    | 订阅的主题和参数 |  
| integer |         integer         |    Y    |     推送时间     |  
|  data   | [List[string]](#ticker) |    Y    |     Ticker数组列表          |

#### <span id="ticker">Ticker array</span>

| 序号  |  Type   | Required |         Note         |    
| :------ | :----- | :------ | :------------------ | 
|    1     | string  |    Y    |         交易对         | 
|    2     | integer |    Y    |    ticker生成时间    | 
|    3     | string  |    Y    |  base token的成交量  |  
|    4     | string  |    Y    | quote token 的成交量 |    
|    5     | string  |    Y    |        开盘价        |  
|    6     | string  |    Y    |        最高价        |  
|    7     | string  |    Y    |        最低价        | 
|    8     | string  |    Y    |      最新成交价      |  
|    9     | integer |    Y    |       成交笔数       |    
|    10    | string  |    Y    |      买单最高价      |  
|    11    | string  |    Y    |      卖单最低价      |   
