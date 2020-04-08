# WebSocket APIs

## 接入URL

```
wss://ws.loopring.io/v2/ws
```

## 订阅和订退


1. 用户可以一次订阅或订退多个主题。如果`topics`中任何一个主题需要ApiKey，那么本次操作就必须包含ApiKey。
1. 用户在一次订阅或订退时，相同的主题如果在`topics`参数中出现多次，排在后面的主题参数会覆盖排在前面的同一主题参数。
1. 用户一次订阅或订退多个主题时，如果其中有参数错误，则全部订阅或订退都会失败。
1. 用户可以通过多次操作重复订阅相同的主题，最新的成功订阅参数会覆盖之前的订阅参数。
1. 订退主题不能提供任何主题参数。


#### 心跳

WebSocket链接建立后，中继会每30秒会发送“ping”消息给客户端做心跳检测。如果客户端在最近2分钟内都没有任何“pong”消息，中继会断开WebSocket链接。如果客户端的“pong”消息数量超过服务端发送的“ping”消息数量，中继也会断开WebSocket链接。


## 请求
订阅和订退需要发送JSON数据给中继。JSON的字段包括：

|  字段  |     类型     | 必须 |               说明                       |
| :---- | :---------- | :------ | :------------------------------ | 
|   op   |    string    |    是    |         订阅（"sub"）或订退（unSub"）   |  
| apiKey |    string    |    否    | 订阅要求传ApiKey的主题才是必须的 | 
|  topics  | string |    是    |         URL编码后的订阅的主题及参数，（见[主题和参数](#Topics)） |


#### <span id="Topics">主题和参数</span>

每个topic都采用URL风格：`topic?param1=value1&param2=value2&...`。参数采用`key=value`格式，多个参数用`&`连接，topic和参数列表间用`?`连接。每个topic字符串需要采用URL编码；多个topic用`&`连接。

#### 示例

比如订阅连个主题，第一个主题是`topic1`，参数是k1=v1, k2=v2；第一个主题是`topic1`，参数是k3=v4, k3=v4。那么`topics`字符串为：

订阅请求：

```json
{
    "op": "sub",
    "topics": "topic1%3Fk1%3Dv1%26k2%3Dv2&topic2%3Fk3%3Dv3%26k4%3Dv4"
}
```

订退请求：

```json
{
    "op": "unSub",
    "topics": "topic1&topic2"
}
```

## 返回值

|  字段  |     类型     | 必现 |               说明               |      
| :---- | :---------- | :------ | :------------------------------ |
|   op   |    string    |    是    |         订阅（"sub"）或订退（unSub"）         |      
| apiKey |    string    |    否    | 订阅要求传ApiKey的主题才是必须的 | 
|  topics  | string |    是    |         URL编码后的订阅的主题及参数，（见[主题和参数](#Topics)） |
| result |    [Result](#result)   |    是    |             订阅结果             |            


####  <span id="result">Result结构</span>

|  字段  |      类型       | 必现 |         说明         | 
| :---- | :------------- | :------ | :------------------ |
| status |     string      |    是    |     订阅是否成功     | 
| error  | [Error](#error) |    否    | 订阅失败时的错误信息 | 

####   <span id="error">Error结构</span>

|  字段   |  类型   | 必现 |   说明   |     
| :----- | :----- | :------ | :------ | 
|  code   | integer |    是    |  状态码  |  
| message | string  |    是    | 错误信息 | 

#### 状态码

| **状态码** |                         描述                         |
| :-------- | :-------------------------------------------------- |
|   104100   |                     空的订阅信息                     |
|   104101   | 不支持的操作（路印中继服务器仅支持sub 和 unsub操作） |
|   104102   |                     不支持的主题                     |
|   104103   |                    重复的订阅主题                    |
|   104104   |                    缺少ApiKey信息                    |
|   104105   |              与之前订阅使用的ApiKey不符              |
|   104112   |                    不合法的ApiKey                    |
|   104113   |               订退未曾订阅过的主题               |
|   104114   |             无法通过APiKey找到对应的用户             |
|   104115   |                  无法识别的订阅消息                  |

#### 示例

订阅成功示例：

```json
{
    "op": "sub",
    "apiKey": "",
    "topics": "topic1%3Fk1%3Dv1%26k2%3Dv2&topic2%3Fk3%3Dv3%26k4%3Dv4",
    "result": {
        "status": "ok"
    }
}
```

订阅参数不合法的失败示例：

```json
{
    "op": "sub",
    "apiKey": "",
    "topics": "topic1%3Fk1%3Dv1%26k2%3Dv2&topic2%3Fk3%3Dv3%26k4%3Dv4",
    "result": {
        "status": "failed",
        "error": {
            "code": 104106,
            "message": "receive illegal arg for candlestick:lrc-eth"
        }
    }
}
```

订阅参数无法解析的失败示例：

```json
{
    "op": "",
    "apiKey": "",
    "topics": "topic1%3Fk1%3Dv1%26k2%3Dv2&topic2%3Fk3%3Dv3%26k4%3Dv4",
    "result": {
        "status": "failed",
        "error": {
            "code": 104115,
            "message": "unexpected msg:xxx"
        }
    }
}
```
