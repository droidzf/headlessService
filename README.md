
## ubuntu安装依赖

```sh
sudo apt-get install -y nodejs
sudo ln -s /usr/bin/nodejs /usr/sbin/node
sudo apt-get install npm
npm install -g n
n lts
npm install -g pm2
sudo apt-get install sqlite3
```

### 更改配置

更改node需要调用的接口地址路径和端口参数为本地java接口的路径和端口
`play/rpc_service.js中的function notifyserver(unit, data)`

### 第一次运行设置devicename和密码
```sh
./setinfo.sh devicename passphrase
```

## 运行

```sh
node start.js
```

## 后台运行

```sh
pm2 start start.js
```
运行后控制台会打印一个地址 记下此地址(云牛币将转到此地址，后期转币使用)

Witness SingleAddress --------------> 

#
#

## 接口说明
接口协议：rpc

[rpc示例文档](https://github.com/dagxio/Bsure-api-wal)

接口地址 http://localhost:6332

**以post方式发送json串**
```json
{"jsonrpc":"2.0","id":1,"method":"methodname","params":[]}
```
**返回格式**
```json
{
"jsonrpc": "2.0",
"result": true,
"id": 1
}
```

```json
//错误
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32700,
        "message": "Invalid Request"
    },
    "id": null
}
```

##### 查询某个收款地址历史
`参数address为商品地址`
```json
{"jsonrpc":"2.0","id":1,"method":"listtransactions","params":{"address": "VXA5Q27TPFZAO4DGKRR3W2D62YIM7GPD"}}
```

**返回：**
```json
{
"jsonrpc": "2.0",
"result": [{
    "action": "received",
    "amount": 100000,//金额
    "my_address": "VXA5Q27TPFZAO4DGKRR3W2D62YIM7GPD",//商品地址
    "arrPayerAddresses": [
        null
    ],
    "confirmations": 1,
    "unit": "DKEqxD6E93TEm+6P088U+0g2eCh7PRp/xLC55YfVohc=",
    "fee": 954,//手续费
    "time": "1536807729",
    "level": null,
    "mci": 977739,
    "asset": "jH12XQGk0JJxYAO7j/lK0jrRjKVuEFc9mfTUc14mx1g="//资产
}],
"id": 1
}
```

##### 生成地址
```json
{"jsonrpc":"2.0","id":1,"method":"getnewaddress","params":[]}
```
**返回:**
```json
{
    "jsonrpc": "2.0",
    "result": "",
    "id": 0
}
```
#####生成地址后等待付款
`第一个参数为商品地址，第二个参数为商品金额`
```json
{"jsonrpc":"2.0","id":1,"method":"waitstable","params":[ "VXA5Q27TPFZAO4DGKRR3W2D62YIM7GPD",100]}
```
**返回:**
```json
{
    "jsonrpc": "2.0",
    "result": true,
    "id": 1
}
```
##### 生成付款码
`第一个参数为商品地址，第二个参数为商品金额`
```json
{"jsonrpc":"2.0","id":1,"method":"getpaycode","params":[ "VXA5Q27TPFZAO4DGKRR3W2D62YIM7GPD",100]}
```
**返回：**
```josn
{
    "jsonrpc": "2.0",
    "result": "byteball:VXA5Q27TPFZAO4DGKRR3W2D62YIM7GPD?amount=100&asset=jH12XQGk0JJxYAO7j%2FlK0jrRjKVuEFc9mfTUc14mx1g%3D",
    "id": 1
}
```
#### 充值后转币
##### 1.转云牛币
`第一个参数为用户地址，第二个参数为金额，第三个参数为资产`
```json
{"jsonrpc":"2.0","id":1,"method":"sendtoaddress","params":["LDLIJOWMGJVLPCVXKZ6KP7JILQHYVEI6",100,"jH12XQGk0JJxYAO7j/lK0jrRjKVuEFc9mfTUc14mx1g="]}
```
##### 2.转base资产(用于支付时的手续费，每笔交易大概1000左右，首次转入数量和后期不足可领取需java端限制)
```只传2个参数即可 同上```
```json
{"jsonrpc":"2.0","id":1,"method":"sendtoaddress","params":["LDLIJOWMGJVLPCVXKZ6KP7JILQHYVEI6",100]}
```
**返回：**
```json
{
    "jsonrpc": "2.0",
    "result": "7tf3+cH+A3bL9T9SCA2E/oZJzK+kUDekxFM50RbuCrI=",
    "id": 1
}
```
