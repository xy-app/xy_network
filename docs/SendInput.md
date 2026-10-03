# 发送远程输入事件 (SendInput)

将本地操作捕获的键鼠动作与控制指令向远端受控设备传输。

## 运行参数

* **远端受控机地址与端口 (`host_address`)**
  * 类型: `string`
  * 默认值: `"127.0.0.1:9988"`
  * 描述: 远端目标接收服务的 IP 地址与端口号。
* **坐标偏移量校准 (x,y) (`offset_pos`)**
  * 类型: `string`
  * 默认值: `"0,0"`
  * 描述: 发送时对输入坐标执行的像素偏移预调。
* **输入事件载荷 (数据/指令) (`input_data`)**
  * 类型: `string`
  * 默认值: `"click:left"`
  * 描述: 准备向受控机透传的输入动作指令。
* **传输协议 (`protocol`)**
  * 类型: `string`
  * 默认值: `"TCP"`
  * 描述: 底层通信协议。可选值：`TCP`, `UDP`。
* **安全握手凭据 Token (可选) (`auth_token`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 远端服务要求的身份认证加密口令。

## 输出

* 类型: `string`
* 描述: 输入事件网络投递状态。

## 使用示例

```json
{
  "tag": "SendInput",
  "host_address": "192.168.1.50:9988",
  "offset_pos": "0,0",
  "input_data": "key:enter",
  "protocol": "TCP",
  "auth_token": "safe_pass"
}
```