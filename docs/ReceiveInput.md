# 接收远程输入事件 (ReceiveInput)

本地监听接收来自远端设备的输入动作与协同控制指令。

## 运行参数

* **本地监听地址与端口 (`host_address`)**
  * 类型: `string`
  * 默认值: `"0.0.0.0:9988"`
  * 描述: 本地监听入站事件的网络地址与端口。
* **坐标偏移量校准 (`offset_pos`)**
  * 类型: `string`
  * 默认值: `"0,0"`
  * 描述: 接收鼠标坐标时自动叠加的偏移像素校准 (x,y)。
* **传输协议 (`protocol`)**
  * 类型: `string`
  * 默认值: `"TCP"`
  * 描述: 底层数据通信协议。可选值：`TCP`, `UDP`。
* **安全握手凭据 Token (可选) (`auth_token`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 验证客户端发包合法性的加密口令。
* **等待超时时间 (秒，0为无限) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `60`
  * 描述: 等待远程输入包的最大超时秒数。

## 输出

* 类型: `string`
* 描述: 捕获并回放的输入动作摘要。

## 使用示例

```json
{
  "tag": "ReceiveInput",
  "host_address": "0.0.0.0:9988",
  "offset_pos": "0,0",
  "protocol": "TCP",
  "auth_token": "safe_pass",
  "timeout_secs": 60
}
```