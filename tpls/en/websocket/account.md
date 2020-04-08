# Account

Subscribe to this topic to receive changes on account balances.



## Subscription

- `topic` should be exactly `"account"`。
- You need to provide your ApiKey.

## Push data example

```json
{
	"topic": "account",
	"ts":1584717910000,
	"data": {
	    "accountId":1,
	    "totalAmount": "24439253519655",
	    "tokenId": 2,
	    "frezeeAmount": "0"
	}
}
```

## Model

#### `data` object

| Field  |        Type         | Required |       Note       |     Example      |
| :--- | :----------------- | :------ | :-------------- | :----------- |
| topic |       string        |    Y    | Topic and parameters |   "account"   |
|  ts   |       integer       |    Y    |     Time of change     | 1584717910000 |
| data  | [Balance](#balance) |    Y    |     Balance data     |       /       |

#### <span id= "balance">`Balance` object</span> 

|     Field     |  Type   | Required |    Note    |       Example       |
| :---------- | :----- | :------ | :-------- | :-------------- |
|  accountId   | integer |    Y    |   Account Id   |        1         |
|   tokenId    | integer |    Y    |   Token Id   |        2         |
| totalAmount  | string  |    Y    |  Token's total balance  | "24439253519655" |
| frezeeAmount | string  |    Y    | Token's balance locked by orders |       "0"        |

