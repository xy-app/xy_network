# BT/种子做种分发 (BtSeed)

将本地完整文件或目录作为种子进行持续做种与分发共享。

## 运行参数

* **种子文件路径或监听目录 (`torrent`)**
  * 类型: `FilePicker`
  * 默认值: `""`
  * 描述: 单个 `.torrent` 种子文件路径或监控种子变动的文件夹。
* **完整数据资源目录 (`resource_dir`)**
  * 类型: `DirectoryPicker`
  * 默认值: `""`
  * 描述: 本地完整数据资源所在的文件夹路径。
* **自动监听目录变动 (`watch_folder`)**
  * 类型: `bool`
  * 默认值: `false`
  * 描述: 是否自动监听种子目录变动以动态增减做种任务。
* **做种持续时间 (秒，0为长效) (`seed_duration`)**
  * 类型: `number`
  * 默认值: `0`
  * 描述: 做种持续时间，0 表示后台长效做种。

## 输出

* 类型: `string`
* 描述: 做种服务初始化状态与任务标识。

## 使用示例

```json
{
  "tag": "BtSeed",
  "torrent": "D:/torrents/dataset.torrent",
  "resource_dir": "D:/datasets/",
  "watch_folder": false,
  "seed_duration": 3600
}
```