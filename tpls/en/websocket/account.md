# Account


通过订阅该主题, 您可以获得用户余额和冻结金额更新的数据推送.

## Rules

- `topic` string:`account`.
- ApiKey **required**.



## Parameters

This topic doesn't support any parameter.


## Push data examples

```json
{
    "topic": {
        "topic:": "account"
    },
	"ts":1584717910000,
	"data": {
	    "accountId":1,
	    "totalAmount": "24439253519655",
	    "tokenId": 2,
	    "aamountLocked": "0"
	}
}
```

# Data Model

# Push data structure

| Field  |        Type         | Required |       Note       |     
| :--- | :----------------- | :------ | :-------------- | 
| topic |       JSON        |    Y    | Topic and parameters |  
|  ts   |       integer       |    Y    |     Push timestamp     | 
| data  | [Balance](#balance) |    Y    |     余额信息     |     

#### <span id= "balance">Balance数据结构</span> 

|     Field     |  Type   | Required |    Note    |     
| :---------- | :----- | :------ | :-------- | 
|  accountId   | integer |    Y    |   用户Id   |     
|   tokenId    | integer |    Y    |   通证Id   |     
| totalAmount  | string  |    Y    |  用户余额  | 
| aamountLocked | string  |    Y    | 冻结的余额 |    

