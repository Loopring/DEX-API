# Top 10 Bids & Asks  (WIP)

Subscribe to this topic to receive the top 10 bids and asks **every second** for the specific trading pair, even if there is no changes in these orders.


## Subscription

- `topic` must specify a trading pair and the aggretation level。If the traidng pair is `LRC-ETH`，and the aggretation level is `1`, then `topic` should be：`"depth10&LRC-ETH&1"`.
- You DO NOT need to provide your ApiKey.
- You can get the list of supported trading pairs through [api/v2/exchange/markets](../dex_apis/getMarkets.md).



## Status code

| Value |                 Note                  |
| :---- | :----------------------------------- |
| 104108 | Invalid or unsupported `topic`|

##  Push data example

```json
{
    "topic": "depth10&LRC-BTC&1",
    "ts": 1565844208,
    "data": {
        "bids": [
            [
              "295.97",  //price
              "4567810000000000",  //size
              "30150000000000",  //volume
              "2"  //count
            ]
        ],
        "asks": [
            [
              "298.97",
              "456781000000000000",
              "301500000000000",
              "2"
            ]
        ]
    }
}
```

## Model

#### `data` object

| Field  |      Type       | Required |       Note       |        Example         |
| :--- | :------------- | :------ | :-------------- | :----------------- |
| topic |     string      |    Y    | Topic and parameters | "depth10&LRC-ETH&1" |
|  ts   |     integer     |    Y    |     Time of change     |    1584717910000    |
| data  | [Depth](#depth) |    Y    |     Orderbook data     |          /          |

####<span id="depth">`Depth` object</span>

| Field | Type                           | Required | Note     | Example |
| :---- | :------------------------------ | :-------- | :-------- | :---- |
| bids | [List\[List\[string\]]](#slot) | Y       | a list of `DepthItem`s representing bids | /    |
| asks | [List\[List\[string\]]](#slot) | Y       | a list of `DepthItem`s representing adks| /    |

#### <span id = "slot">`DepthItem` array</span>

Each array in `asks` and `bids` is a fix-size array we called *DepthItem* which contains the following items：

| Index  | Type   | Required | Note           | Example       |
| :------ | :------ | :-------- | :-------------- | :---------- |
|    1     | string | Y       | Price           | "0.002"    |
|    2     | string | Y       | Amount (quantity of base token)         | "21000"    |
|    3     | string | Y       | Total (quantity of quote token)    | "33220000" |
|    4     | string | Y       | Number of orders aggregated | "4"        |

Note the amount and total are the new values, not the delta between new and old values.

