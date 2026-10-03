# BT/种子下载 (BtDownload)

通过 BT 种子文件或磁力链接高速下载网络资源。

## 运行参数

* **种子文件/磁力链接 (`file`)**
  * 类型: `FilePicker`
  * 默认值: `""`
  * 描述: Torrent 文件路径或 Magnet 磁力链接 (如 `magnet:?xt=...`)。
* **保存目录 (`save_path`)**
  * 类型: `DirectoryPicker`
  * 默认值: `""`
  * 描述: 下载数据保存的目标文件夹路径。
* **等待全部下载完成 (`wait_finish`)**
  * 类型: `bool`
  * 默认值: `true`
  * 描述: 是否同步等待所有数据块下载完成。
* **超时时间 (秒，0为无限) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `300`
  * 描述: 最大等待下载完成的超时时长，0 为无限等待。

## 输出

* 类型: `string`
* 描述: 下载任务执行状态或目标存储文件路径。

## 使用示例

```json
{
  "tag": "BtDownload",
  "file": "D:/downloads/sample.torrent",
  "save_path": "D:/downloads/",
  "wait_finish": true,
  "timeout_secs": 600
}
```