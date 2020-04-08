# 账号金额更新


通过订阅该主题，您可以获得用户余额和冻结金额更新的数据推送。

## 订阅规则

- `topic`字符串：`account`。
- 订阅该主题**需要**提供ApiKey。



## 参数列表

该主题不支持任何参数。


## 推送示例

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

## 模型

#### 推送消息数据结构

| 字段  |        类型         | 必现 |       说明       |     
| :--- | :----------------- | :------ | :-------------- | 
| topic |       JSON        |    是    | 订阅的主题和参数 |  
|  ts   |       integer       |    是    |     推送时间     | 
| data  | [Balance](#balance) |    是    |     余额信息     |     

#### <span id= "balance">Balance数据结构</span> 

|     字段     |  类型   | 必现 |    说明    |     
| :---------- | :----- | :------ | :-------- | 
|  accountId   | integer |    是    |   用户Id   |     
|   tokenId    | integer |    是    |   通证Id   |     
| totalAmount  | string  |    是    |  用户余额  | 
| aamountLocked | string  |    是    | 冻结的余额 |    

