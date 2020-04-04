# 请求签名

路印API涉及到两种不同类别的签名。一种是通用的API签名，用来验证API调用已经被账号拥有者授权；另一种是对API POST到服务端的特定的数据结构做签名，用来向路印协议的电路证明该数据已经被账号拥有者授权。我们分别对这两者做个说明。


## 通用API请求的签名



## 路印协议链下请求的签名

路印协议3.1.1支持“订单”，和“链下提现”两种**链下请求**。由于这两种链下请求都会造成对交易所默克尔树的修改，通过中继API提交这是两种数据时，必须附带特殊的签名。


{% hint style='info' %}
路印协议3.1.1还支持**取消订单**这个链下请求，但会在后续的3.5版本中去掉。因此路印交易所没有计划支持链下取消订单。
{% endhint %}

链下请求签名包括以下步骤：

1. 对请求`r`进行规整，使其变成一个字符串`s`。
2. 计算`s`的**Poseidon**哈希值`h`（见下面章节）。
3. 对`h`用账号的私钥`privateKey`做签名，得到三个值：`Rx`,`Ry`, 和`s`（见下面章节）。
4。将`h`, `Rx`,`Ry`, 和`S`合并到`r`当中。

```json
"hash": ...,
"signatureRx": "16367919966553849834214288740952929086694704883595501207054796240908626703398",
"signatureRy": "5706650945525714138019517276433581394702490352313697178959212750249847059862",
"signatureS": "410675649229327911665390972834008845981102813589085982164606483611508480748"
```

#### 对订单做签名

订单中一些数据项需要按照特定序列化成一个整数数组，对这个数组计算Poseidon哈希，然后对该哈希做EdDSA签名。

{% hint style='info' %}
订单的序列化规则，哈希，签名方式必须严格遵循[路印协议规范](https://github.com/Loopring/protocols/blob/master/packages/loopring_v3/DESIGN.md)。
{% endhint %}

下面我们用Python代码做示范：

```python
def sign_int_array(privateKey, serialized, p):
    PoseidonHashParams = poseidon_params(
        SNARK_SCALAR_FIELD,
        p,
        6,
        53,
        b'poseidon',
        5,
        security_target=128
    )
    
    hash = poseidon(serialized, PoseidonHashParams)
    signedMessage = PoseidonEdDSA.sign(hash, FQ(int(privateKey)))
    return ({
        "hash": str(hash),
        "signatureRx": str(signedMessage.sig.R.x),
        "signatureRy": str(signedMessage.sig.R.y),
        "signatureS": str(signedMessage.sig.s),
    })

def serialize_order(order):
    return [
        int(order["exchangeId"]),
        int(order["orderId"]),
        int(order["accountId"]),
        int(order["tokenSId"]),
        int(order["tokenBId"]),
        int(order["amountS"]),
        int(order["amountB"]),
        int(order["allOrNone"]=="true"),
        int(order["validSince"]),
        int(order["validUntil"]),
        int(order["maxFeeBips"]),
        int(order["buy"]=="true"),
        int(order["label"])
    ]

def sign_order(privateKey, order):
	serialized = serialize_order(order)
	signed = sign_int_array(serialized, 14 /* 注意这个值 */)
    order.update(signed)
```
{% hint style='info' %}
如果您不使用ethsnarks代码仓库计算poseidon哈希，请一定配置好poseidon的参数，保证其与路印协议使用的参数完全一致。否则验证签名会失败。
{% endhint %}



#### 对链下提现做签名
{% hint style='danger' %}
目前的中继API还不支持客户端提交链下提现请求。不过我们会很快增加这个API。
{% endhint %}

下面链下提现的一个例子：
```json
{
    "exchangeId": 2,
    "accountId":100,
    "tokenId": 0,
    "amount": 1000000000000000000,
    "feeTokenId": "2",
    "amountFee": 20000000000000000000,
    "label": 0,
    "nonce": 10
}
```

用Python对其签名的代码：
```python
def serialize_offchain_withdrawal(withdrawal):
    return [
        int(withdrawal['exchangeId']),
        int(withdrawal['accountId']),
        int(withdrawal['tokenId']),
        int(withdrawal['amount']),
        int(withdrawal['feeTokenId']),
        int(withdrawal['amountFee']),
        int(withdrawal['label']),
        int(withdrawal['nonce'])
    ]

def sign_offchain_withdrawal(privateKey, offchainWithdrawal):
    serialized = serialize_offchain_withdrawal(offchainWithdrawal)
    signed = sign_int_array(serialized, 9 /* 注意这个值 */)
    offchainWithdrawal.update(signed)
```

## 参考资料
您可以通过下列文献和代码仓库了解更多关于Poseidon哈希和EdDSA签名的细节。

1. `ethsnarks`代码仓库：https://github.com/HarryR/ethsnarks.git
2. `SHA256 Hash`算法：<https://en.wikipedia.org/wiki/SHA-2>
3. `EdDSA`算法：<https://en.wikipedia.org/wiki/EdDSA>
4. `Poseidon Hash`算法：<https://www.poseidon-hash.info/>


您也可以参考我们的[示范代码](./examples.md)了解更多应用细节。