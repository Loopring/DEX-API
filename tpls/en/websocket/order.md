# Orders

Subscribe to this topic to receive changes on orders for the specific trading pair.



## Subscription
- `topic` must specify a trading pair。If the traidng pair is `LRC-ETH`，then `topic` should be：`"order&LRC-ETH"`。
- You DO NOT need to provide your ApiKey.
- You can get the list of supported trading pairs through [api/v2/exchange/markets](../dex_apis/getMarkets.md).

## Status code

| Value |                Note                 |
| :---- | :--------------------------------- |
| 104110 | Invalid or unsupported `topic`|

## Push data example

```json
{
   "topic": "order&LRC-BTC",
   "ts":1565844328,
   "data": {
        "hash": "11212",
        "clientOrderId": "myOrder",
        "size": "500000000",
        "volume": "210000000",
        "price": "0.000004",
        "filledSize": "30000000",
        "filledVolume": "100000",
        "filledFee": "1000000",
        "status": "processing",
        "createdAt": "1494900087",
        "validSince": "1494900087",
        "validUntil": "1495900087",
        "side": "buy",
        "market": "LRC-BTC"
    }
}
```

## Model

#### `data` object

| Field  |      Type       | Required |       Note       |      Example       |
| :--- | :------------- | :------ | :-------------- | :------------- |
| topic |     string      |    Y    | Topic and parameters | "order&LRC-ETH" |
|  ts   |     integer     |    Y    |     Time of change     |  1584717910000  |
| data  | [Order](#order) |    Y    |     Order data     |        /        |

#### <span id="order">`Order` object</span>

|     Field      |  Type   | Required |            Note            |     Example      |
| :----------- | :----- | :------ | :------------------------ | :----------- |
|     hash      | string  |    Y    |          Order's hash          |    "11212"    |
| clientOrderId | string  |    Y    |        Order's clientOrderId        |   "myOrder"   |
|     size      | string  |    Y    |     Order's base token quantity      |  "500000000"  |
|    volume     | string  |    Y    |     Order's quote token quantity     |  "210000000"  |
|     price     | string  |    Y    |          Order's price          |  "0.000004"   |
|  filledSize   | string  |    Y    | Filled amount of the base token  |  "30000000"   |
| filledVolume  | string  |    Y    | Filled amount of the quote token |   "100000"    |
|   filledFee   | string  |    Y    |      Trading fees paid       |   "1000000"   |
|    status     | string  |    Y    |          Order status         | "processing"  |
|   createdAt   | integer |    Y    |        Order submission timestamp      | 1584717910000 |
|   updateAt    | integer |    Y    |   Order's last update timestamp   | 1584717910000 |
|     side      | string  |    Y    |           Buy or sell          |     "buy"     |
|    market     | string  |    Y    |           the trading pair           |   "LRC-ETH"   |

#### Order status

|    Value    |                    Note                    |
| :-------- | :---------------------------------------- |
| processing | Order is active and not fully filled |
| processed  |                Order has been fully filled                |
| cancelling |                   Order is being cancelled                   |
| cancelled  |                  Order has been cancelled                 |
|  expired   |                  Order has expired                  |
|  waiting   |                Order hasn't becomen active yet                |
