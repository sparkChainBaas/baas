#### <a href="./index.md#top">返回上一级目录</a>      

## 目录
===========================  

<a href="./chapter04.md#4. 用户账户接口">4. 用户账户接口</a>  <br> 
* <a href="./chapter04.md#4.1. 创建账户">4.1. 创建账户</a>  <br>
* <a href="./chapter04.md#4.2. 账户充值">4.2. 账户充值</a>  <br>
* <a href="./chapter04.md#4.3. 账户的支付请求">4.3. 账户的支付请求</a>  <br>
* <a href="./chapter04.md#4.4. 获取账户余额">4.4. 获取账户余额</a>  <br>
* <a href="./chapter04.md#4.5. 获取账户支付信息">4.5. 获取账户支付信息</a>  <br>
* <a href="./chapter04.md#4.6. 同步流水">4.6. 同步流水</a>  <br>
* <a href="./chapter04.md#4.7. 获取账户支付历史">4.7. 获取账户支付历史</a>  <br>
* <a href="./chapter04.md#4.8. 账户的文本上链">4.8. 账户的文本上链（新加）</a>  <br>

## <a name="4. 用户账户接口">4. 用户账户接口</a>  
用户账户与用户钱包对应，一个用户钱包包括多个用户账户，在一个公链上针对同一个用户，在火花接入平台创建一个账户，用来管理其在公链上的操作。这里提供了更底层的操作，接入应用可以直接使用这些账户接口对公链进行直接操作。

### <a name="4.1. 创建账户">4.1. 创建账户</a>  
[回到顶部](#目录)

在指定的公链上创建账户，返回其公钥和私钥。  
- 接口地址：/v1/account/createAccount

- 请求方式：GET/POST  

- 接口参数：  

| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|chainCode|String|区块链编码| 
- 请求示例图：
--- 
![image](./pics/account_createAccount.jpg?raw=true)
- 请求示例代码：
--- 
```
/v1/account/createAccount?chainCode=jingtum
```

- 结果返回参数：  

| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|code|String|请求结果|
|message|String|返回信息|
|success|boolean|是否成功（true：成功）|
|data|Object|返回数据|
|data.privateKey|String|账户的私钥|
|data.addr|String|账户地址|

- 返回示例图：
--- 
![image](./pics/account_createAccount_result.jpg?raw=true)

- 返回示例代码：
--- 
```json
{
    "code": "200",
    "data": {
        "privateKey": "snSNydkdvutwfRvxds6HGotkNRHDP",
        "publicKey": "jwxmFu5MxAPF9wkFZVCxoDU1E7Wpwp5oTo",
        "addr": "jwxmFu5MxAPF9wkFZVCxoDU1E7Wpwp5oTo"
    },
    "message": "",
    "success": true
}
```


### <a name="4.2. 账户充值">4.2. 账户充值</a>   
[回到顶部](#目录)

外部账户向本平台的账户或平台的钱包进行充值，比如创建app钱包之后，就需要外部账户向其app钱包中充值，然后app钱包向用户钱包转账。  
- 接口地址：/v1/account/charge 

- 请求方式：GET/POST  

- 接口参数:  
 
| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|chainCode|String|区块链编码|
|tokenCode|String|通证编码|
|srcAccount|String|充值方的账户|
|srcPrivateKey|String|充值方的账户私钥|
|destAccount|String|【三选一】接受方的账户 --- 当为空时，（1）可传入destWalletAddr（2）可传入appid,destUserId|
|destWalletAddr|String|【三选一】接收方的钱包地址|
|appid|Long|应用Id，和destUserId结合在一起使用|
|destUserId|String|【三选一】接受方的用户Id|
|bizId|String|业务ID（每次操作都不能重复，保证事务）|
|amount|String|充值金额|  
|gasLimit|Long|【可选】Gas数的上限值,该gas设置对jingtum公链不起效|
|gasPrice|Long|【可选】Gas单位价格,该gas设置对jingtum公链不起效|  

- 请求示例图：
---
![image](./pics/account_charge.jpg?raw=true)

- 请求示例代码：
---
```
/v1/account/charge?chainCode=moac&tokenCode=MOAC&srcAccount=XXX&srcPrivateKey=XXX&destWalletAddr=4ed93afd-47bf-43cb-8fd9-ed8ed3c7affe&bizId=f0cefc3b-b936-4da6-afe1-94ef9014c500&amount=0.001
```
- 结果返回参数：  
  
| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|code|String|请求结果|
|message|String|返回信息|
|success|boolean|是否成功（true：成功）---链返回正确的结果，但交易可能还未写入区块，所以仍在执行中|
|data|Object|返回数据|
|data.hash|String|交易返回的hash值|  
- 返回示例图：
---
![image](./pics/account_charge_result.jpg?raw=true)

- 返回示例代码：
---
```json
{
    "code": "200",
    "data": {
        "hash": "0x79d851b23ea2507b541c71751fec1911a7393706db84a214eeec608a45d36ae0"
    },
    "message": "",
    "success": true
}
```



### <a name="4.3. 账户的支付请求">4.3. 账户的支付请求</a>  
[回到顶部](#目录)

不同账户间充值。两个外部账户可以通过该接口进行转账，本平台中会保存它们转账的流水记录。  
- 接口地址：/v1/account/transfer  

- 请求方式：GET/POST  

- 接口参数：  
  
| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|srcAccount|String|支付方的账户|
|privateKey|String|支付方的账户私钥|
|destAccount|String|接受方的账户|
|amount|String|支付金额|
|chainCode|String|区块链编码|
|tokenCode|String|通证编码|
|memo|String|【可选】记录内容（提供了敏感词过滤功能，上链时敏感词会转换为*）|
|bizId|String|业务Id（每次操作都不能重复，保证事务）|
|gasLimit|Long|【可选】Gas数的上限值,该gas设置对jingtum公链不起效|
|gasPrice|Long|【可选】Gas单位价格,该gas设置对jingtum公链不起效|  
- 请求示例图：
---
![image](./pics/account_transfer.jpg?raw=true)

- 请求示例代码：
---
```
/v1/account/transfer?srcAccount=XXX&privateKey=XXX&destAccount=0x08bad40508a3169a3a2e9caa36fa20ef7bd98535&amount=0.00001&chainCode=moac&tokenCode=MOAC&bizId=ce876b5b-faad-4fe3-a820-6646ee8cc7ef
```

- 结果返回参数： 
  
| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|code|String|请求结果|
|message|String|返回信息|
|success|boolean|是否成功（true：成功）---链返回正确的结果，但交易可能还未写入区块，所以仍在执行中|
|data|Object|数据|
|data.hash|String|交易返回的hash值|  
- 返回示例图：
---
![image](./pics/account_transfer_result.jpg?raw=true)

- 返回示例代码：
---
```json
{
    "code": "200",
    "data": {
        "hash": "0xe8f6b9636bcb5bc27f3b86cd81cceb446b180ed8e4ba9224b56e0f9fa6cf2f27"
    },
    "message": "",
    "success": true
}
```



### <a name="4.4. 获取账户余额">4.4. 获取账户余额</a>  
[回到顶部](#目录)

获取账户的余额。  
- 接口地址：/v1/account/balance  

- 请求方式：GET/POST  

- 接口参数： 
 
| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|account|String|账户|
|chainCode|String|区块链编码（比如：jingtum）|
|tokenCode|String|通证编码（比如：jingtum的通证是SWT）|
- 请求示例图：
--- 
![image](./pics/account_balance.jpg?raw=true) 
- 请求示例代码：
--- 
```
/v1/account/balance?account=0x08bad40508a3169a3a2e9caa36fa20ef7bd98535&chainCode=moac&tokenCode=MOAC
```

- 结果返回参数：  

| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|code|String|请求结果|
|message|String|返回信息|
|success|boolean|是否成功（true：成功）|
|data|Object|返回数据|
|data.address|String|账户地址|
|data.balance|String|余额|
|data.tokenCode|String|通证编码|
|data.freezed|String|冻结金额|

- 返回示例图：
--- 
![image](./pics/account_balance_result.jpg?raw=true)

- 返回示例代码：
--- 
```json
{
    "code": "200",
    "data": {
        "tokenCode": "MOAC",
        "address": "0x08bad40508a3169a3a2e9caa36fa20ef7bd98535",
        "balance": "0.00101",
        "freezed": ""
    },
    "message": "",
    "success": true
}

```

### <a name="4.5. 获取账户支付信息">4.5. 获取账户支付信息</a>  
[回到顶部](#目录)

直接从链上获取账户支付交易的详细信息。
- 接口地址：/v1/account/transInfo  

- 请求方式：GET/POST  

- 接口参数：  

| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|account|String|账户|
|chainCode|String|【可选】区块链编码（比如：jingtum）|
|tranHash|String|支付交易的hash|
- 请求示例图：
---
![image](./pics/account_transInfo.jpg?raw=true)

- 请求示例代码：
---
```
/v1/account/transInfo?account=jDjnjvH62qbFy2SWFZX8S373wqoLJ8MWRa&chainCode=jingtum&tranHash=D56E173D86B904669A6C3F913CD5B386EE9A3A9F732D2086337EF5404AC24A0A
```

- 结果返回参数：  

| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|code|String|请求结果|
|message|String|返回信息|
|success|boolean|是否成功（true：成功）|
|data|Object|返回数据|
|data.tradeTime|Date|交易时间|
|data.tokenCode|String|通证编码|
|data.gasFee|String|交易费用|
|data.amount|String|支付金额|
|data.memos|String|记录内容|
|data.blockNumber|String|区块编号（交易记录一旦写入区块后才返回该字段）|
|data.state|String|状态（-1：失败；0：初始；1：等待；8：同步成功；9：成功）||
|data.tranHash|String|交易的hash|
|data.destAccount|String|接受方的账户|
|data.srcAccount|String|支付方的账户|
- 返回示例图：
---
![image](./pics/account_transInfo_result.jpg?raw=true)

- 返回示例代码：
---
``` json
{
    "code": "200",
    "data": {
        "tradeTime": 1532072250000,
        "tokenCode": "SWT",
        "gasFee": "0.01",
        "amount": "0.2",
        "memos": "accout transfer 0.2swt",
        "blockNumber": "10222917",
        "destAccount": "jEWtMWS6FBXyj1U1c4X8u5VnFpZtZ3rUMZ",
        "state": "9",
        "tranHash": "D56E173D86B904669A6C3F913CD5B386EE9A3A9F732D2086337EF5404AC24A0A",
        "srcAccount": "jDjnjvH62qbFy2SWFZX8S373wqoLJ8MWRa"
    },
    "message": "",
    "success": true
}
```

### <a name="4.6. 同步流水">4.6. 同步流水</a> 
[回到顶部](#目录)

当链上的流水和接入平台的流水不同步时，比如其账户在外部进行转入或转出的交易，这时需要调用同步流水，把链上的流水同步到火花接入平台中，然后就可以根据各种不同的条件来进行查询。只支持jingtum公链的同步流水。
- 接口地址：/v1/account/syncWaterflow  

- 请求方式：GET/POST  

- 接口参数：  

| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|account|String|账户|
|chainCode|String|区块链编码|
|tokenCode|String|通证编码|
|syncSize|Integer|【可选】同步大小 （默认为10）|  
- 请求示例图：
---
![image](./pics/account_syncWaterflow.jpg?raw=true)

- 请求示例代码：
---  
```
/v1/account/syncWaterflow?account=jsG1CeTng2HJamTTjQiUsk1Jak9pn228Ey&chainCode=jingtum&tokenCode=SWT
```
- 结果返回参数：

| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|code|String|请求结果|
|message|String|返回信息|
|success|boolean|是否成功（true：成功）|
|data|list|返回数据|
|data.destinyPublicKey|String|目标方的公钥|
|data.tokenCode|String|通证编码|
|data.tradeTime|Date|交易时间|
|data.createtime|Date|创建时间|
|data.payTime|Date|支付时间|
|data.chainCode|String|区块链编码|
|data.memos|String|记录内容|
|data.dispAmount|String|显示的转账金额|
|data.srcPublicKey|String|发起方的公钥|
|data.state|Integer|状态（8：同步状态）|
|data.tradeHash|String|交易的hash|


- 返回示例图：
---  
![image](./pics/account_syncWaterflow_result.jpg?raw=true)

- 返回示例代码：
---  
```  json
{
    "code": "200",
    "data": [
        {
            "destinyPublicKey": "jsG1CeTng2HJamTTjQiUsk1Jak9pn228Ey",
            "tokenCode": "SWT",
            "tradeTime": 1529481680000,
            "createtime": 1529481680000,
            "payTime": 1529481680000,
            "chainCode": "jingtum",
            "memos": "hello world",
            "dispAmount": "22.1",
            "srcPublicKey": "jDjnjvH62qbFy2SWFZX8S373wqoLJ8MWRa",
            "state": 8,
            "tranHash": "0B4E6F24DA7C3594162FC6535AB67E163FBDA64F8DF30A5C48F2D417FE2CF7D9"
        }
    ],
    "message": "",
    "success": true
}
```
### <a name="4.7. 获取账户支付历史">4.7. 获取账户支付历史</a>  
[回到顶部](#目录)

获取指定账户在链上的支付历史。  
- 接口地址：/v1/account/transInfoList  

- 请求方式：GET/POST  

- 接口参数：  
  
| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|account|String|账户|
|pagesize|Integer|【可选】返回的每页数据量，默认每页10项|
|pagenum|Integer|【可选】返回第几页的数据，默认从1开始|
|starttime|String|【可选】开始时间，比如：2018-01-01 00:00:00|
|endtime|String|【可选】结束时间，比如： 2018-06-01 23:59:59|  
 
- 请求示例图：
---
![image](./pics/account_transInfoList.jpg?raw=true) 

- 请求示例代码：
---
```
/v1/account/transInfoList?account=jfhEuQiJ5f8y8F8hR57JVUnXxYavn9FM3h
```

- 结果返回参数： 

| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|code|String|请求结果|
|message|String|返回信息|
|success|boolean|是否成功（true：成功）|
|data|Object|返回数据|
|data.total|Integer|总记录数|
|data.pageSize|Integer|每页数据量|
|data.pageNum|Integer|第几页|
|data.results|list|结果数据|
|data.results.tokenCode|String|通证编码|
|data.results.tradeTime|Date|交易时间|
|data.results.amount|String|支付金额|
|data.results.gasFee|String|交易费用|
|data.results.chainCode|String|区块链编码|
|data.results.memos|String|记录内容|
|data.results.bizId|String|业务Id|
|data.results.destAccount|String|接受方的账户|
|data.results.state|String|状态（-1：失败；0：初始；1：等待；8：同步成功；9：成功）|
|data.results.srcAccount|String|支付方的账户|
|data.results.tranHash|String|交易的hash|
- 返回示例图： 
---
![image](./pics/account_transInfoList_result.jpg?raw=true) 

- 返回示例代码： 
---

```  json
{
    "code": "200",
    "data": {
        "total": 1,
        "pageSize": 10,
        "pageNum": 1,
        "results": [
            {
                "tokenCode": "SWT",
                "tradeTime": 1532861464000,
                "amount": "35",
                "gasFee": "0.01",
                "chainCode": "jingtum",
                "memos": "SWT INIT",
                "bizId": "1532861464298",
                "destAccount": "jfhEuQiJ5f8y8F8hR57JVUnXxYavn9FM3h",
                "state": 9,
                "tranHash": "EB7658318F2D3DC3D94A3F32F92945AC204523FDB8FCBF4EC9264E72CE22305B",
                "srcAccount": "jB4MR8wQAbSECLtTXnaerFEoYvE7qvHNrh"
            }
        ]
    },
    "message": "",
    "success": true
}
```

### <a name="4.8. 账户的文本上链">4.8. 账户的文本上链</a>  
[回到顶部](#目录)

该接口提供账户的文本上链。
  

- 接口地址：/v1/account/record  

- 请求方式：POST  

- 接口参数：  
  
| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|srcAccount|String|支付方的账户|
|srcPrivateKey|String|支付方的账户私钥|
|destAccount|String|接受方的账户|
|amount|String|支付金额|
|chainCode|String|区块链编码|
|tokenCode|String|通证编码|
|memo|String|记录内容（提供了敏感词过滤功能，上链时敏感词会转换为*）|
|bizId|String|业务Id（每次操作都不能重复，保证事务）|
|gasLimit|Long|【可选】Gas数的上限值,该gas设置对jingtum公链不起效|
|gasPrice|Long|【可选】Gas单位价格,该gas设置对jingtum公链不起效|  
- 请求示例图：
---
![image](./pics/account_record.jpg?raw=true)

- 请求示例代码：
---
```
srcAccount:0x9d7f35ae9271b12f7f8dd210bb1764612c974b41
srcPrivateKey:1158781120b45f6b323ae137a2625ec70c8ed8844ad0d125af8d106767f1d1c0
destAccount:0x35676148123ebbad169921af6083437f44fcb15b
amount:0.0000001
chainCode:moacTest
tokenCode:MOAC
memo:account transfer 0.0000001 moac
bizId:{{$guid}}
gasLimit:40000
gasPrice:20000000000
```

- 结果返回参数： 
  
| 参数         | 类型       | 说明   |
| :------------- |:-------------| :-----|
|code|String|请求结果|
|message|String|返回信息|
|success|boolean|是否成功（true：成功）---链返回正确的结果，但是交易有可能还在运行中|
|data|Object|数据|
|data.hash|String|交易返回的hash值|  
- 返回示例图：
---
![image](./pics/account_record_result.jpg?raw=true)

- 返回示例代码：
---
```json
{
    "code": "200",
    "data": {
        "hash": "0xfa4f2017670c476616725c305c770d842eb103c3d216d706e5bd62927309bd1b"
    },
    "message": "",
    "success": true
}
```




## 官方联系方式

### 官方QQ群

![QQ群：594629943](../sp.png)


## 第三方合作伙伴

 - <a href="https://www.jingtum.com/">井通科技</a>,github地址为：https://github.com/swtcpro ，开发者文档地址：http://developer.jingtum.com/  浏览器地址：http://state.jingtum.com

 - <a href="http://www.moac.io/">MOAC</a>,github地址为：ttps://github.com/MOACChain/,开发者文档地址：https://github.com/MOACChain/moac-core/wiki/Commands ,https://github.com/MOACChain/moac-core/wiki/Chain3 ,浏览器地址：http://explorer.moac.io/home

