# beilypay-api-doc
Welcome to the beilypay-api-doc !
# 签名算法


第一步，设所有发送或者接收到的数据为集合M，将集合M内非空参数值的参数按照参数名ASCII码从小到大排序（字典序），使用URL键值对的格式（key1=value1&key2=value2…）拼接成字符串stringA。
特别注意以下重要规则：

- 参数名ASCII码从小到大排序（字典序）；
- 参数名区分大小写；
- 如果参数的值为空不参与签名；
- 验证调用返回或平台主动通知签名时，传送的sign参数不参与签名，将生成的签名与该sign值作校验。
- 接口可能增加字段，验证签名时必须支持增加的扩展字段。

第二步，在stringA最后拼接上key得到stringSignTemp字符串，并对stringSignTemp进行MD5运算，再将得到的字符串所有字符转换为大写，得到sign值signValue。
​

待签名字符串：stringSignTemp= a=123&b=123&c=123&key=key
签名:  sign = MD5(stringSignTemp) .toUpperCase();


# 环境信息

**接口响应 code 200为成功  其他为失败，失败原因见msg描述**

**测试环境域名 http://dev.beilypay.com （测试环境不发起真正的代付、代收。只做API验证。）**

**正式环境域名  http://service.beilypay.com**


# 代付
## 代付下单


**接口描述**:


**接口地址**:`/v2/trans/create`


**请求方式**：`POST`


**consumes**:`["application/json"]`


**produces**:`["*/*"]`


**请求示例**：


```json
{
	"account": "",
	"accountOwner": "",
	"accountType": "",
	"address": "",
	"appId": 0,
	"bankCode": "",
	"email": "",
	"ifsc": "",
	"merchantId": 0,
	"mobile": "",
	"notifyUrl": "",
	"outOrderNo": "",
	"payAmount": 0,
	"sign": ""
}
```


**请求参数属性说明**
**TransReq**

| 参数名称 | 参数说明 | 是否必须 | 数据类型 |
| --- | --- | --- | --- |
| account | 对应的收款账户 | true | string |
| accountOwner | 账户持有者姓名 | true | string |
| accountType | 收款账户类型 Card: 代付到银行卡 | true | string |
| address | 收款人地址 | true | string |
| appId | 应用id | true | long |
| bankCode | 账户类型为Card,对应的银行编码 | true | string |
| email | 收款人邮箱 | true | string |
| ifsc | 账户类型为Card,分行的IFSC代码 | true | string |
| merchantId | 商户id | true | integer(int32) |
| mobile | 收款人手机号 印度10位手机号 | true | string |
| notifyUrl | 通知回调地址 | true | string |
| outOrderNo | 商户单号 | true | string |
| payAmount | 代付金额, 整数 单位分 | true | integer(int32) |
| sign | 数据签名 | true | string |



**响应示例**:


```json
{
	"code": 0,
	"data": {
		"appId": 0,
		"merchantId": 0,
		"outOrderNo": "",
		"payAmount": 0,
		"sign": "",
		"status": 0,
		"orderNo": "",
		"transTime": ""
	},
	"msg": ""
}
```


**响应参数**:

| 参数名称 | 参数说明 | 类型 | schema |
| --- | --- | --- | --- |
| code | 200成功 其他失败 | integer(int32) | integer(int32) |
| data |  | 代付结果返回 | 代付结果返回 |
| msg |  | string |  |



**代付结果返回属性说明**

| 参数名称 | 参数说明 | 类型 |
| --- | --- | --- |
| appId | 应用id | long |
| merchantId | 商户id | integer(int32) |
| outOrderNo | 商户订单号 | string |
| payAmount | 代付金额, 整数 单位分 | integer(int32) |
| sign | 数据签名 | string |
| status | 代付单状态 1等待回调；2成功；3失败； | integer(int32) |
| orderNo | 交易号 | string |
| transTime | 交易时间  yyyy-MM-dd HH:mm:ss | string |







## 代付异步通知


**接口描述**:
该接口是平台方调用客户后台将代付结果通知给客户方的异步回调接口,回 调地址由代付接口中 notifyUrl 指定,请确保 notifyUrl 地址是可访问的，系统会在代付结果更新时立即回调客户方,在客户方收到回调后,如果处理成功,请返回 包含”SUCC”的内容,格式不限,平台认为是回调成功的.
​

**请求方式**：`POST`


**consumes**:`["application/json"]`
​


| 参数名称 | 参数说明 | 类型 |
| --- | --- | --- |
| appId | 应用id | long |
| merchantId | 商户id | integer(int32) |
| outOrderNo | 商户订单号 | string |
| payAmount | 代付金额, 整数 单位分 | integer(int32) |
| sign | 数据签名 | string |
| status | 代付单状态 2成功；3失败； | integer(int32) |
| orderNo | 交易号 | string |
| payTime| 交易时间   | Date |

​

## 代付状态查询


**接口描述**:


**接口地址**:`/v2/trans/query`


**请求方式**：`POST`


**consumes**:`["application/json"]`


**produces**:`["*/*"]`


**请求示例**：


```json
{
	"appId": 0,
	"merchantId": 0,
	"sign": "",
	"orderNo": ""
}
```


**请求参数属性说明**

| 参数名称 | 参数说明 | in | 是否必须 | 数据类型 |
| --- | --- | --- | --- | --- |
| appId | 应用id | body | true | long |
| merchantId | 商户id | body | true | integer(int32) |
| sign | 数据签名 | body | true | string |
| orderNo | 交易号 | body | true | string |



**响应示例**:


```json
{
	"code": 0,
	"data": {
		"appId": 0,
		"merchantId": 0,
		"outOrderNo": "",
		"payAmount": 0,
		"sign": "",
		"status": 0,
		"orderNo": ""
	},
	"msg": ""
}
```


**响应参数**:

| 参数名称 | 参数说明 | 类型 | schema |
| --- | --- | --- | --- |
| code | 200成功 其他失败  | integer(int32) | integer(int32) |
| data |  | 代收订单详情查询 | 代收订单详情查询 |
| msg |  | string |  |



**代收订单详情查询属性说明**

| 参数名称 | 参数说明 | 类型 |
| --- | --- | --- |
| appId | 应用id | long |
| merchantId | 商户id | integer(int32) |
| outOrderNo | 商户订单号 | string |
| payAmount | 代付金额, 整数 单位分 | integer(int32) |
| sign | 数据签名 | string |
| status | 代付单状态 1等待回调；2成功；3失败； | integer(int32) |
| orderNo | 交易号 | string |



# 代收
## 代收下单


**接口描述**:


**接口地址**:`/v2/payment/create`


**请求方式**：`POST`


**consumes**:`["application/json"]`


**produces**:`["*/*"]`


**请求示例**：


```json
{
	"appId": 0,
	"email": "",
	"frontCallback": "",
	"merchantId": 0,
	"mobile": "",
	"notifyUrl": "",
	"outOrderNo": "",
	"payAmount": 0,
	"sign": "",
	"userId": "",
	"userName": ""
}
```


**代收下单参数属性说明**

| 参数名称 | 参数说明 | in | 是否必须 | 数据类型 |
| --- | --- | --- | --- | --- |
| appId | 应用id | body | true | long |
| email | 邮箱 |  | true | string |
| frontCallback | 前端支付成功跳转地址 | body | true | string |
| merchantId | 商户id | body | true | integer(int32) |
| mobile | 手机号 印度10位 |  | true | string |
| notifyUrl | 通知回调地址 | body | true | string |
| outOrderNo | 商户单号 | body | true | string |
| payAmount | 代收金额, 整数 单位分 | body | true | integer(int32) |
| sign | 请求签名 | body | true | string |
| userId |  | body | true | string |
| userName |  | body | true | string |



**响应示例**:


```json
{
	"code": 0,
	"data": {
		"outOrderNo": "",
		"payAmount": 0,
		"payUrl": "",
		"sign": "",
		"orderNo": ""
	},
	"msg": ""
}
```


**响应参数**:

| 参数名称 | 参数说明 | 类型 | schema |
| --- | --- | --- | --- |
| code | 200成功 其他失败  | integer(int32) | integer(int32) |
| data |  | 代收单支付信息 | 代收单支付信息 |
| msg |  | string |  |



**代收单支付信息属性说明**

| 参数名称 | 参数说明 | 类型 |
| --- | --- | --- |
| outOrderNo | 平台单号 | string |
| payAmount | 支付金额 | integer(int32) |
| payUrl | 支付链接 | string |
| sign | 签名 | string |
| orderNo | 交易号 | string |

## 
测试环境下，payUrl非支付链接，访问该链接系统自动修改为支付成功，并触发回调



## 代收异步通知


**接口描述**:
该接口是平台方调用客户后台将代收结果通知给客户方的异步回调接口,回 调地址由代收接口中 notifyUrl 指定,请确保 notifyUrl 地址是可访问的，系统会在代付结果更新时立即回调客户方,在客户方收到回调后,如果处理成功,请返回 包含”SUCC”的内容,格式不限,平台认为是回调成功的.
​

**请求方式**：`POST`


**consumes**:`["application/json"]`
​


| 参数名称 | 参数说明 | 类型 |
| --- | --- | --- |
| outOrderNo | 商户订单号 | string |
| paid | 代付金额, 整数 单位分 | integer(int32) |
| sign | 数据签名 | string |
| status | 代付单状态 2成功；3失败； | integer(int32) |
| orderNo | 交易号 | string |
| payTime | 交易时间   | Date |

## 
## 代收单状态查询


**接口描述**:


**接口地址**:`/v2/payment/query`


**请求方式**：`POST`


**consumes**:`["application/json"]`


**produces**:`["*/*"]`


**请求示例**：


```json
{
	"appId": 0,
	"merchantId": 0,
	"sign": "",
	"orderNo": ""
}
```


**请求参数查询属性说明**

| 参数名称 | 参数说明 | 是否必须 | 数据类型 |
| --- | --- | --- | --- |
| appId | 应用id | true | long |
| merchantId | 商户id | true | integer(int32) |
| sign | 数据签名 | true | string |
| orderNo | 交易号 | true | string |



**响应示例**:


```json
{
	"code": 0,
	"data": {
		"outOrderNo": "",
		"paid": 0,
		"payAmount": 0,
		"sign": "",
		"status": 0,
		"orderNo": "",
		"transTime": ""
	},
	"msg": ""
}
```


**响应参数**:

| 参数名称 | 参数说明 | 类型 | schema |
| --- | --- | --- | --- |
| code | 200成功 其他失败  | integer(int32) | integer(int32) |
| data |  | 代收单信息 | 代收单信息 |
| msg |  | string |  |



**schema属性说明**


**代收单信息**

| 参数名称 | 参数说明 | 类型 |
| --- | --- | --- |
| outOrderNo | 商户单号 | string |
| paid | 实付金额 单位 分 | integer(int32) |
| payAmount | 代收单金额 单位 分 | integer(int32) |
| sign | 数据签名 | string |
| status | 代收单状态 0 代收创建失败  1等待回调；2成功；3失败； | integer(int32) |
| orderNo | 交易号 | string |
| transTime | 交易完成时间 yyyy-MM-dd HH:mm:ss | string |

# 商户管理
<a name="IV43Z"></a>
## 商户余额查询


**接口地址** `/merchant/accountInfo`


**请求方式** `POST`


**consumes** `["application/json"]`


**请求参数**


**AccountQueryReq**

| 参数名称 | 参数说明 | 是否必须 | 数据类型 |
| --- | --- | --- | --- |
| appId | 应用id | true | integer(int64) |
| merchantId | 商户id | true | integer(int32) |
| sign | 数据签名 | true | string |



**请求示例**

```java
{
"appId": 1235677,
"merchantId": 1123,
"sign": "ASDAFASGTHJFJFGJFGHFG"
}
```


**响应参数**

| 参数名称 | 参数说明 | 类型 |
| --- | --- | --- |
| code | 响应码 | integer(int32) |
| data | 业务数据 | MerchantAccountVO |
| msg |  | string |



**MerchantAccountVO**

| 参数名称 | 参数说明 | 类型 |
| --- | --- | --- |
| merchantId | 商户id | integer(int32) |
| settlementAmount | 账户可用金额，单位INR | number |



**响应示例**


```java
{
"code": 200,
"data": {
	"merchantId": 111,
	"settlementAmount"99.99:
	},
"msg": "SUCC"
}
```
# 系统错误码

| 错误码 | 错误说明 |
| --- | --- |
| 10001 | merchantId not exist（商户不存在） | 
| 10002 | appId not exist（app未注册）|
| 10003 | orderNo not exist （订单号不存在）| 
| 10004 | sign check error（签名校验失败）|
| 10005 | account balance not enough （代付账户余额不足）| 
| 10006 | your requests are too frequent（请求过于频繁）|
| 10007 | your account setting error （商户账户设置异常，联系平台处理）| 
| 10008 | transaction fail（交易创建失败，联系平台处理）|
| 10009 | order finished（交易已完成） | 
| 10010 | order expire(订单已失效，请重新下单)|
| 10011 | trans account type not support(代付账户类型不支持) | 
| 10012 | pay tool error(银行交易失败)|
| 10013 | param error（参数异常，具体见真实错误返回） | 
| 20001 | unknow error(未知异常)|


