# 正向代理服务 (ForwardProxy)

启动本地正向代理或隧道代理服务，支持多协议中继与用户鉴权。

## 运行参数

* **监听端口号 (`port`)**
  * 类型: `number`
  * 默认值: `1080`
  * 描述: 本地代理服务监听的端口号。
* **监听绑定IP (`bind_address`)**
  * 类型: `string`
  * 默认值: `"0.0.0.0"`
  * 描述: 代理服务绑定的网络接口 IP 地址。
* **代理协议 (`protocol`)**
  * 类型: `string`
  * 默认值: `"ALL"`
  * 描述: 支持的代理协议类型。可选值：`ALL`, `HTTP`, `SOCKS5`。
* **认证用户名 (`auth_username`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 客户端连接代理所需的鉴权用户名（留空免密）。
* **认证密码 (`auth_password`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 客户端连接代理所需的鉴权密码（可选）。
* **服务句柄标识 (`handle_name`)**
  * 类型: `string`
  * 默认值: `"default_proxy"`
  * 描述: 代理服务唯一句柄标识，供 NetworkClose 联动释放。
* **空闲超时时间 (秒，0为不限) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `60`
  * 描述: 连接空闲无数据流转时的超时断开时长。

## 输出

* 类型: `string`
* 描述: 代理服务启动运行状态提示。

## 使用示例

```json
{
  "tag": "ForwardProxy",
  "port": 1080,
  "bind_address": "127.0.0.1",
  "protocol": "ALL",
  "auth_username": "",
  "auth_password": "",
  "handle_name": "my_proxy",
  "timeout_secs": 60
}
```