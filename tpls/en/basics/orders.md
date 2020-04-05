# Orders


##Uni-Directional Order Model
Unlike the order models of most centralized exchanges, Loopring uses the **Uni-Directional Order Model** (UDOM). UDOM represents buy orders and sell orders uniformly with one single data structure. Let's start with a simplified UDOM model to give you a few examples of Loopring's limit price orders (Loopring doesn't support market price orders).

In the LRC-ETH trading pair, a **sell** order that sells 500 LRC at the price of 0.03ETH/LRC can be expressed as:
```JSON
{   // LRC-ETH: sell 500 LRC at 0.03ETH/LRC
    "tokenS": "LRC",
    "tokenB": "ETH",
    "amountS": 500,
    "amountB": 15 // = 500 * 0.03
}
```

{% hint style='info' %}
The letter S stands for *Sell* and letter B stands for *Buy*.
{% endhint %}


a **buy** order that buys 500 LRC at the price of 0.03ETH/LRC can be expressed as:
```JSON
{   // LRC-ETH: buy 500 LRC at 0.03ETH/LRC
    "tokenS": "ETH",
    "tokenB": "LRC",
    "amountS": 15, // = 500 * 0.03
    "amountB": 500 
}
```

As you may have noticed, UDOM does not specify trading pairs or prices explicitly.



However, there is a problem with this simplified model: the match-engine doesn't know when an order should be considered as **fully filled**. We need to introduce another parameter called `buy` for this purpose. If `buy == true`, the match-engine shall check the total fill amount of `tokenB` against `amountB` to determine if an order has been fully filled; otherwise, it shall use the total fill amount of `tokenS` against `amountS`. With this new field, the above orders will look like the following: 

```JSON
{   // LRC-ETH: sell 500 LRC at 0.03ETH/LRC
    "tokenS": "LRC",
    "tokenB": "ETH",
    "amountS": 500,
    "amountB": 15 // = 500 * 0.03，
    "buy": false  // check tokenS's fill amount against amountS
}
```

```JSON
{   // LRC-ETH: buy 500 LRC at 0.03ETH/LRC
    "tokenS": "ETH",
    "tokenB": "LRC",
    "amountS": 15, // = 500 * 0.03
    "amountB": 500,
    "buy": true // check tokenB's fill amount against amountB
}
```

Note: If the above sell order is fully filled, the amount of ETH bought may be larger than 15ETH; and if the buy order is fully filled, the ETH paid may be less than 15ETH, which is the impact of the `buy` parameter on the match engine's behaviors.


What is the effect of reversing the `buy` value in the two orders above? The sell order for the LRC-ETH trading pair becomes a buy order for the ETH-LRC trading pair, and the buy order for the LRC-ETH trading pair becomes a sell order for the ETH-LRC trading pair. It means one Loopring trading pair, such as LRC-ETH, is equivalent to two trading pairs in many centralized exchanges, i.e.,  LRC-ETH and ETH-LRC. Besides its elegancy and simplicity, Loopring's UDOM also makes it possible to implement much simpler settlement logic in ZKP circuits.


## 订单数据
Loopring's actual order format is a bit more complex. You can use the following JSON to express a limit price order. For details of specific parameters, see [Submit Order](../dex_apis/submitOrder.md).

```JSON
newOrder = {
    "tokenSId": 2,  // LRC
    "tokenBId": 0,  // ETH
    "amountS": "500000000000000000000",
    "amountB": "15000000000000000000",
    "buy": "false",
    "exchangeId": 2,
    "accountId": 1234,
    "allOrNone": "false", // Must be "false" for now
    "maxFeeBips": 50,
    "label": 211,
    "validSince": 1582094327,
    "validUntil": 1587278341,
    "orderId": 5,
    "hash": "14504358714580556901944011952143357684927684879578923674101657902115012783290",
    "signatureRx": "15179969700843231746888635151106024191752286977677731880613780154804077177446",
    "signatureRy": "8103765835373541952843207933665617916816772340145691265012430975846006955894",
    "signatureS" : "4462707474665244243174020779004308974607763640730341744048308145656189589982",
    "clientOrderId": "Test01"
}
```

Next, we will further explain some of these data fields for you.

#### Tokens and Amounts
In an actual order,  tokens are not expressed by their names or ERC20 addresses, but by their **token ID**, the index at which the tokens have been registered in the Loopring Exchange's smart contract.  Note that the same ERC20 token may have different IDs on different exchanges built on top of the same Loopring protocol.

In the above example, we assume that the IDs of LRC and ETH are 2 and 0, respectively.
You can query token's information using [Token Information Supported by the Exchange](../dex_apis/getTokens.md).

The amounts of tokens are in their smallest unit as strings. Taking LRC as an example, its `decimals` is 18, so 1.0LRC should be expressed as `" 1000000000000000000 "` (1 followed by 18 0s). Each token's `decimals` is coded in its smart contract; the decimals of ETH is 18.

{% hint style='info' %}
The types of `buy` and` allOrNone` in the order are strings rather than boolean.
{% endhint %}

#### Trading Fee
`maxFeeBips=50`代表该订单愿意支付给交易所的**最高手续费比例**是0.5%(`maxFeeBips`的单位是0.01%)。路印的交易手续费都是用成交获得的`tokenB`支付的。假设上面订单某次成交买入了`"10000000000000000000"`ETH(10ETH)，那么实际支付的手续费**不会超过0.05ETH**(`"10000000000000000000" * 0.5%`)。

实际支付的手续费比例是由路印中继决定的。中继会根据不同的VIP等级，给不同的用户相应的交易手续费折扣。路印协议不允许实际手续费比例大于用户订单中指定的最高手续费比例。

用户在特定交易对的交易手续费可以通过`/api/v2/user/feeRates`查询获得。

#### Timestamps

`validSince`代表订单生效时间，`validUntil`代表订单过期时间，其单位均为秒。

中继服务器收到订单时会验证订单中的这两个时间戳；路印协议的零知识证明电路代码在清算时候也需要判断这两个时间戳。由于zkRollup批处理延迟，以及以太坊上时间与服务器时间可能存在的偏差，我们强烈建议`validSince`设置得比当前时间至少早15分钟，即：

```python
order["validSince"] = int(time.time() - 15 * 60)
```

我们同样建议`validSince`和`validUntil`之间的时间窗口不小于1小时，否则您的订单可能不会被撮合。

{% hint style='tip' %}
您可以通过使用`validUntil`时间戳来让订单自动过期，避免不必要的主动取消订单操作。
{% endhint %}


#### Fill Status and Order ID


路印协议3.1.1为支持的每个通证预留了16384($$2^{14}$$)个槽位来记录**卖出该通证的**订单的成交量。如果订单ID是`N`，那么使用的槽位编号就是`N % 16384`。换言之，如果槽位编号是`m`，该槽位就可以被用来记录具有下列ID的订单：`m`，`m + 16384`，`m + 16384 * 2`，... 以此类推。

每个槽位都记录了当前在追踪的订单的ID(初始值就是槽位编号)，并且后续不接受订单ID比当前订单ID更小的订单。假设槽位1记录的是ID为`32769`( `1 + 16384 * 2`)的订单的订单状态，当用户下一个订单ID为`1`或`16385`的订单的时候，下单就会失败。


订单ID的最大值是`1048576`，即$$2^{20}$$。到达这个ID上限后，对应的通证就无法再下任何卖单。对于普通用户，这不是大问题；但对于程序化交易，您可能需要注册一个新账号继续交易。

{% hint style='info' %}
路印协议3.5会去除订单ID最大值的限制，但依然保留槽位的设计和数量。
{% endhint %}

值得注意的是，同一用户在基础通证相同的多个交易对(如LRC-ETH和LRC-USDT)的所有**卖单**共享上面的16384槽位的。如果您不想在客户端维护交易对间订单ID和槽位的分配，您可以注册多个账号：一个账号参与LRC-ETH市场的交易，另一个账号参与LRC-USDT市场的交易。


综合上述信息，我们建议：
1. 使用从0开始逐渐递增的ID作为新订单的ID；
2. 对于特定的卖出通证，如果某个槽位已经被某个订单占用，您需要先取消该订单，才能继续使用这个槽位追踪新订单的成交量 - 除非之前的订单已经完全成交。
3. 对于做市程序，您最好在客户端维护槽位分配情况并计算新订单的ID。如果您通过路印API获取下一个订单的ID，有可能因为调用次数过多而被限制。

{% hint style='info' %}
我们知道这种设计带来的不便利。不过这是路印协议设计时候做的取舍。希望后续技术的进步可以将这个限制去除。
{% endhint %}


#### Other Fields

- `exchangeId`：路印交易所在路印协议体系中的交易所序号。后续路印交易所升级智能合约后，这个`exchangeId`的值会变化。路印交易所beta1对应的`exchangeId`是2。
- `accountId`：用户的账号ID。
- `allOrNone`：如果是`"true"`，要求订单要么不成交，要么就要完全成交。目前这个参数还不被撮合引擎支持，因此请先设置为`"false"`。
- `label`: 用于在协议层标记订单。该项的值对于交易清算没有任何影响。用户会对这个值做签名，因此该值对于不同实体间的分润根据可信度。
- `clientOrderId`: 用户客户端在协议层外标记订单，可以是任意长度小于66的字符串。该项的值对于交易清算没有任何影响。用户不会对该项做签名。

更多细节请参考[提交订单](../dex_apis/submitOrder.md)。




