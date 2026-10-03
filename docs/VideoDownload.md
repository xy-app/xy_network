# 流媒体视频下载 (VideoDownload)

高速解析并下载网页流媒体音视频，支持浏览器登录态 Cookie 读取与格式转码配置。

## 运行参数

* **视频网页/流媒体地址 (URL) (`url`)**
  * 类型: `string`
  * 默认值: `"https://"`
  * 描述: 待解析下载的视频页面或流媒体播放链接。
* **保存目录或命名模板 (`output`)**
  * 类型: `DirectoryPicker`
  * 默认值: `""`
  * 描述: 下载完成的音视频文件存放路径。
* **高级选项 (JSON配置) (`options`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 自定义格式过滤与高级参数字典配置。
* **Cookie 文件路径 (可选) (`cookiefile`)**
  * 类型: `FilePicker`
  * 默认值: `""`
  * 描述: 用于会员或免登录验证的 Cookie 文件。
* **提取 Cookie 的浏览器 (`browser`)**
  * 类型: `string`
  * 默认值: `"none"`
  * 描述: 自动从本地已登录浏览器中提取会话凭据。可选值：`none`, `chrome`, `edge`, `firefox`。
* **浏览器用户配置目录 (可选) (`profile_directory`)**
  * 类型: `DirectoryPicker`
  * 默认值: `""`
  * 描述: 指定浏览器的多用户 Profile 文件夹路径。

## 输出

* 类型: `string`
* 描述: 视频下载与转码合成结果。

## 使用示例

```json
{
  "tag": "VideoDownload",
  "url": "https://www.bilibili.com/video/BV1xx411c7mD",
  "output": "D:/videos/",
  "options": "",
  "cookiefile": "",
  "browser": "edge",
  "profile_directory": ""
}
```