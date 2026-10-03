# 网络数据发送 (NetworkSend)

向指定的网络连接发送纯文本内容或十六进制字节报文。

## 运行参数

* **句柄标识符 (`socket`)**
  * 类型: `string`
  * 默认值: `"default_socket"`
  * 描述: 目标网络连接句柄标识符。
* **待发送数据 (`value`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 准备向网络通道写入的内容载荷。
* **数据格式模式 (`data_mode`)**
  * 类型: `string`
  * 默认值: `"Text"`
  * 描述: 输入数据的解析格式：纯文本 (`Text`) 或十六进制字节码 (`Hex`)。
* **自动追加回车换行 (`append_crlf`)**
  * 类型: `bool`
  * 默认值: `false`
  * 描述: 是否在发送内容尾部自动追加 `\r\n` 换行符。

## 输出

* 类型: `string`
* 描述: 实际发送的字节数与执行状态。

## 使用示例

```json
{
  "tag": "NetworkSend",
  "socket": "plc_conn",
  "value": "01 03 00 00 00 0A C5 CD",
  "data_mode": "Hex",
  "append_crlf": false
}
```