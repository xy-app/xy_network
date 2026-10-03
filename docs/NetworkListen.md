# 网络服务监听 (NetworkListen)

本地绑定并监听指定端口上的网络连接或入站数据报文。

## 运行参数

* **监听绑定地址与端口 (`socket_address`)**
  * 类型: `string`
  * 默认值: `"0.0.0.0:8080"`
  * 描述: 本地监听的服务绑定地址与端口。
* **传输协议 (`protocol`)**
  * 类型: `string`
  * 默认值: `"TCP"`
  * 描述: 底层网络传输协议类型。可选值：`TCP`, `UDP`。
* **接入句柄标识符 (`handle_name`)**
  * 类型: `string`
  * 默认值: `"default_socket"`
  * 描述: 接入连接注册的唯一句柄标识符。
* **等待接入超时时间 (秒，0为无限) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `60`
  * 描述: 等待客户端接入的最大等待时长。

## 输出

* 类型: `string`
* 描述: 客户端接入状态及对端 IP 端口信息。

## 使用示例

```json
{
  "tag": "NetworkListen",
  "socket_address": "0.0.0.0:9000",
  "protocol": "TCP",
  "handle_name": "listener_01",
  "timeout_secs": 60
}
```