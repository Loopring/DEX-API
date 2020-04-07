# 订阅用户账号金额更新


通过订阅该主题，您可以获得用户余额和冻结金额更新的数据推送。

## Subscription rules

- `topic`需要设为`account`。
- 订阅该主题**需要提供ApiKey**。

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

## Data Model

#### 推送Structure

| Field  |        Type         | Required |       Note       |     Example      |
| :--- | :----------------- | :------ | :-------------- | :----------- |
| topic |       string        |    Y    | Topic and parameters |   "account"   |
|  ts   |       integer       |    Y    |     推送时间     | 1584717910000 |
| data  | [Balance](#balance) |    Y    |     余额信息     |       /       |

#### <span id= "balance">BalanceStructure</span> 

|     Field     |  Type   | Required |    Note    |       Example       |
| :---------- | :----- | :------ | :-------- | :-------------- |
|  accountId   | integer |    Y    |   用户Id   |        1         |
|   tokenId    | integer |    Y    |   通证Id   |        2         |
| totalAmount  | string  |    Y    |  用户余额  | "24439253519655" |
| frezeeAmount | string  |    Y    | 冻结的余额 |       "0"        |

