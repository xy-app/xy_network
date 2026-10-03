# 网络数据接收 (NetworkReceive)

从已连接的网络通道中按定界符、固定长度或全部可用流读取数据。

## 运行参数

* **句柄标识符 (`socket`)**
  * 类型: `string`
  * 默认值: `"default_socket"`
  * 描述: 待读取数据的目标网络连接标识符。
* **读取模式 (`read_mode`)**
  * 类型: `string`
  * 默认值: `"AllAvailable"`
  * 描述: 数据读取分包模式。可选值：`AllAvailable`, `UntilDelimiter`, `FixedLength`。
* **定界结束符 (`delimiter`)**
  * 类型: `string`
  * 默认值: `"\\n"`
  * 描述: 当模式为 UntilDelimiter 时的包结尾分隔符。
* **固定读取字节数 (`length`)**
  * 类型: `number`
  * 默认值: `1024`
  * 描述: 当模式为 FixedLength 时期望读取的字节长度。
* **输出格式模式 (`data_mode`)**
  * 类型: `string`
  * 默认值: `"Text"`
  * 描述: 输出内容显示格式：纯文本 (`Text`) 或十六进制字符串 (`Hex`)。
* **读取超时时间 (毫秒) (`timeout_ms`)**
  * 类型: `number`
  * 默认值: `3000`
  * 描述: 单次读取等待数据的最大毫秒数。

## 输出

* 类型: `string`
* 描述: 读取解析后的数据内容。

## 使用示例

```json
{
  "tag": "NetworkReceive",
  "socket": "plc_conn",
  "read_mode": "UntilDelimiter",
  "delimiter": "\\r\\n",
  "length": 1024,
  "data_mode": "Text",
  "timeout_ms": 3000
}
```