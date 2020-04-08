# Account


通过订阅该主题, 您可以获得用户余额和冻结金额更新的数据推送.

## 订阅规则

- `topic` string:`account`.
- ApiKey **required**.



## Parameters

该主题不支持任何参数.


## Push Examples

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
| topic |       JSON        |    Y    | 订阅的主题和参数 |  
|  ts   |       integer       |    Y    |     推送时间     | 
| data  | [Balance](#balance) |    Y    |     余额信息     |     

#### <span id= "balance">Balance数据结构</span> 

|     Field     |  Type   | Required |    Note    |     
| :---------- | :----- | :------ | :-------- | 
|  accountId   | integer |    Y    |   用户Id   |     
|   tokenId    | integer |    Y    |   通证Id   |     
| totalAmount  | string  |    Y    |  用户余额  | 
| aamountLocked | string  |    Y    | 冻结的余额 |    

