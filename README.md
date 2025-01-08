<p align="center"><img alt="live.fanmingming.com" src="logo.png"></p>
<h1 align="center"> ✯ 一个可直连访问的电视/广播图标库与相关工具项目 ✯ </h1>
<h3 align="center">🔕 永久免费 直连访问 完整开源 不断完善的台标 支持IPv4/IPv6双栈访问 🔕</h3>

---

## 🤹‍♂️使用方法:

### 🌇电视/广播图标库：

| 类 别  | 调用路径                                       | 最后更新   |
|-------|------------------------------------------------|------------|
| 📺电视  | [https://live.fanmingming.cn/tv/{name}.png](https://github.com/fanmingming/live/tree/main/tv) | 2024.12.01    |
| 📻广播  | [https://live.fanmingming.cn/radio/{name}.png](https://github.com/fanmingming/live/tree/main/radio) | 2024.8.29   |

### ⛓️创建您的m3u订阅链接：
 - 下载 `demo.m3u` 空白示例文件并使用文本编辑软件打开。
   - [https://live.fanmingming.cn/tv/m3u/demo.m3u](https://live.fanmingming.cn/tv/m3u/demo.m3u)

 - 参考下方示例代码将`可用的CCTV1节目源`替换为您当地可用的直播源链接，依此类推逐个替换。

```
#EXTM3U x-tvg-url="https://live.fanmingming.cn/e.xml"
#EXTINF:-1 tvg-name="CCTV1" tvg-logo="https://live.fanmingming.cn/tv/CCTV1.png" group-title="央视",CCTV-1 综合
可用的CCTV1节目源
此处省略...
```

 - 将编辑完成的m3u文件上传到您的Github仓库。
 - 为您的Github仓库开启Pages。
 - 通过播放器订阅您的m3u链接。

> 关于Github Pages：[https://docs.github.com/en/enterprise-cloud@latest/pages/quickstart](https://docs.github.com/en/enterprise-cloud@latest/pages/quickstart)

## 🛠️工具
- 📆**EPG接口地址**：
  -  [https://live.fanmingming.cn/e.xml](https://live.fanmingming.cn/e.xml)
- 🏞️**Bing每日图片**：
  -  [https://fanmingming.com/bing](https://fanmingming.com/bing)
- 🎞️**m3u8在线下载**：
  -  [https://live.fanmingming.cn/m3u8](https://live.fanmingming.cn/m3u8)
- 🆕**TXT转M3U格式**：
  - [https://live.fanmingming.cn/txt2m3u](https://live.fanmingming.cn/txt2m3u)
- 📄**在线M3U转TXT**：
  - Demo🔗 [https://fanmingming.com/txt?url=https://live.fanmingming.com/tv/m3u/ipv6.m3u](https://fanmingming.com/txt?url=https://live.fanmingming.cn/tv/m3u/ipv6.m3u)
- 🌐**M3U8 Web Player**:
  - Demo🔗 [https://live.fanmingming.cn/player/?vurl=https://0472.org/hls/cgtn.m3u8](https://live.fanmingming.cn/player/?vurl=https://0472.org/hls/cgtn.m3u8)


