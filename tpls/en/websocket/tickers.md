# 订阅Ticker更新

通过订阅该主题，您可以获得特定交易对ticker更新的数据推送。


## Subscription

- `topic`需要指定交易对。如果交易对是`LRC-ETH`，那么`topic`应该拼写为：`ticker&LRC-ETH`。
- 订阅该主题不需要提供ApiKey。
- You can get the list of supported trading pairs through [api/v2/exchange/markets](../dex_apis/getMarkets.md).

## Status code

| Value |                 Note                 |
| :---- | :---------------------------------- |
| 104111 | Invalid or unsupported `topic`|

## Push data example

```json
{
    "topic": "ticker&LRC-ETH",
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

## Model

#### Data object

|  Field   |          Type           | Required |       Note       |       Example       |
| :----- | :--------------------- | :------ | :-------------- | :-------------- |
|  topic  |         string          |    Y    | Topic and parameters | "ticker&LRC-ETH" |
| integer |         integer         |    Y    |     推送时间     |  1584717910000   |
|  data   | [List[string]](#ticker)  (Ticker)|    Y    |     深度信息     |        /         |

#### <span id="ticker">Tickerobject</span>

| Index  |  Type   | Required |         Note         |     Example      |
| :------ | :----- | :------ | :------------------ | :----------- |
|    1     | string  |    Y    |         交易对         |   "LRC-ETH"   |
|    2     | integer |    Y    |    ticker生成时间    | 1584717910000 |
|    3     | string  |    Y    |  base token的成交量  |   "5000000"   |
|    4     | string  |    Y    | quote token 的成交量 |    "1000"     |
|    5     | string  |    Y    |        开盘价        |   "0.0002"    |
|    6     | string  |    Y    |        最高价        |   0.00025"    |
|    7     | string  |    Y    |        最低价        |   "0.0002"    |
|    8     | string  |    Y    |      最新成交价      |   "0.00025"   |
|    9     | integer |    Y    |       成交笔数       |     5000      |
|    10    | string  |    Y    |      买单最高价      |   "0.00026"   |
|    11    | string  |    Y    |      卖单最低价      |   "0.00027"   |
