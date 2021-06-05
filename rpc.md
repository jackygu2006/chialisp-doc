## 通过RPC与链交互

chia节点和服务带有一个支持JSON的RPC API服务，允许访问Chia链。可以通过HTTP、WebSockets或python客户端访问。

这些端口可以在`~/.chia/mainnet/config/config.yaml`中配置。`RPC`端口不会被暴露在互联网上，`TLS`证书被用来保证通信安全。

### 全球开放的Chia节点查询：
```
North Asia introducer-apne.chia.net:8444
South Asia introducer-apse.chia.net:8444
Western North America: introducer-or.chia.net:8444
Eastern North America introducer-va.chia.net:8444
Europe: introducer-eu.chia.net:8444
```
访问：https://chia.keva.app 查询更新

所有开放节点端口8444，测试网58444

### 主网默认端口：
```
Daemon: 55400
Full Node: 8555
Farmer: 8559
Harvester: 8560
Wallet: 9256
```
节点配置文档在`~/.chia/testnet_7/config/config.yaml`，主网在`~/.chia/mainnet/config/config.yaml`

### HTTP/JSON方式
从命令行调用`RPC`时必须使用证书。所有的节点都是用`JSON`格式的`POST`进行的。响应是一个带有`success`为`True/False`字段的`JSON`字典。

### JavaScript方式
参考js客户端：
https://github.com/chiaforce/chia-client

#### WebSocket方式
调用格式：
```
{
    "command": "get_blockchain_state",
    "ack": false,
    "data": {},
    "request_id": "123456",
    "destination": "wallet",
    "origin": "ui",
}
```

### Python方式
参考客户端源码，目录在：`chia/rpc`。

或者查看命令行源码，目录在: `chia/cmds`
