# 网络建立连接 (NetworkConnect)

创建 TCP 或 UDP 客户端网络连接并注册到全局连接池。

## 运行参数

* **主机地址 (IP或域名) (`host_address`)**
  * 类型: `string`
  * 默认值: `"127.0.0.1"`
  * 描述: 目标服务器的 IP 地址或域名。
* **端口号 (`port_number`)**
  * 类型: `number`
  * 默认值: `80`
  * 描述: 目标网络服务端口号。
* **传输协议 (`protocol`)**
  * 类型: `string`
  * 默认值: `"TCP"`
  * 描述: 底层网络传输协议类型。可选值：`TCP`, `UDP`。
* **连接句柄标识符 (`handle_name`)**
  * 类型: `string`
  * 默认值: `"default_socket"`
  * 描述: 注册到连接池中的唯一句柄名称。
* **超时时间 (毫秒) (`timeout_ms`)**
  * 类型: `number`
  * 默认值: `3000`
  * 描述: 建立握手连接的最大超时毫秒数。

## 输出

* 类型: `string`
* 描述: 连接成功状态提示与注册的句柄名称。

## 使用示例

```json
{
  "tag": "NetworkConnect",
  "host_address": "192.168.1.100",
  "port_number": 502,
  "protocol": "TCP",
  "handle_name": "plc_conn",
  "timeout_ms": 3000
}
```