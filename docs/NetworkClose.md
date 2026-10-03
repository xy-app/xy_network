# 关闭网络连接 (NetworkClose)

安全释放已打开的网络连接句柄或代理服务资源。

## 运行参数

* **连接或代理句柄标识符 (`socket`)**
  * 类型: `string`
  * 默认值: `"default_socket"`
  * 描述: 准备关闭的句柄标识符，留空或填写 `'all'` 可关闭全部。

## 输出

* 类型: `string`
* 描述: 操作执行结果提示。

## 使用示例

```json
{
  "tag": "NetworkClose",
  "socket": "default_socket"
}
```