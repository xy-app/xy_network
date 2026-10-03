# HTTP 文件下载 (HttpDownload)

通过 HTTP 网址下载文件，支持覆盖判断、超时设置与忽略证书校验。

## 运行参数

* **下载链接 (URL) (`url`)**
  * 类型: `string`
  * 默认值: `"https://"`
  * 描述: 远程资源下载 URL 地址。
* **保存目录 (`folder`)**
  * 类型: `DirectoryPicker`
  * 默认值: `""`
  * 描述: 本地保存目标目录路径。
* **保存文件名 (可选) (`filename`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 自定义保存的文件名（留空则自动从响应头或 URL 推导）。
* **覆盖已有文件 (`overwrite`)**
  * 类型: `bool`
  * 默认值: `true`
  * 描述: 若目标文件已存在是否直接覆盖。
* **超时时间 (秒) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `120`
  * 描述: 下载操作的最大超时时长。
* **忽略 SSL 证书错误 (`ignore_ssl_errors`)**
  * 类型: `bool`
  * 默认值: `false`
  * 描述: 是否跳过无效或自签名证书校验。

## 输出

* 类型: `string`
* 描述: 下载完成后的本地绝对路径。

## 使用示例

```json
{
  "tag": "HttpDownload",
  "url": "https://example.com/file.zip",
  "folder": "D:/downloads",
  "filename": "file.zip",
  "overwrite": true,
  "timeout_secs": 120,
  "ignore_ssl_errors": false
}
```