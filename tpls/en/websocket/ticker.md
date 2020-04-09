# Ticker更新


Subscribe to this topic to receive notifications about ticker updates for specific trading pairs.

## Rules

- Topic name: `ticker`
- ApiKey requred: No


## Parameters

|  Parameter |  Required |              Note                |
| :---- | :--- |:--------------------------------- |
| market | Y | [Trading pair](../dex_apis/getMarkets.md)|



## Status code

| Value |                 Note                |
| :---- | :---------------------------------- |
| 104111 | Invalid topic or parameters|

## Notification example

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

## Data Model

#### Notification

|  Field   |          Type           | Required |       Note       |    
| :----- | :--------------------- | :------ | :-------------- | 
| topic |       JSON        |    Y    | Topic and parameters |  
| ts |         integer         |    Y    |     Push timestamp (milliseconds)     |  
|  data   | [List[string]](#ticker) |    Y    |     Ticker array        |

#### <span id="ticker">Ticker</span>

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
