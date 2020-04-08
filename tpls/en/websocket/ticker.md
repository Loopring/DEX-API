# Ticker更新

通过订阅该主题, 您可以获得特定交易对ticker更新的数据推送.




## Rules

- `topic` string:`ticker`.
- ApiKey **not required**.


## Parameters

|  Parameter |  Required |              Note                |
| :---- | :--- |:--------------------------------- |
| market | Y | [Trading pair](../dex_apis/getMarkets.md)|



## Status code

| Value |                 Note                |
| :---- | :---------------------------------- |
| 104111 | invalid `topic` or parameters|

## Push data examples

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
| topic |       JSON        |    Y    | Topic and parameters |  
| integer |         integer         |    Y    |     Push timestamp     |  
|  data   | [List[string]](#ticker) |    Y    |     Ticker array list          |

#### <span id="ticker">Ticker array</span>

| Index  |  Type   | Required |         Note         |    
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
