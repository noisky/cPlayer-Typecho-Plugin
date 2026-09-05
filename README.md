# cPlayer for Typecho（本地播放版）

基于 [journey-ad/cPlayer-Typecho-Plugin](https://github.com/journey-ad/cPlayer-Typecho-Plugin)
的 Typecho 维护版本，底层播放器基于 [MoePlayer/cPlayer](https://github.com/MoePlayer/cPlayer)。

本版本只保留自定义音频播放能力，适合播放博客本地托管或其他可直接访问的音频资源。
同时保留 LRC 歌词、翻译歌词、封面、播放列表、缓存和播放器脚本 CDN 配置。

## 主要改动

- 移除网易云音乐单曲、歌单、专辑、艺人和每日推荐解析
- 移除网易云音乐 API、`MUSIC_U`、音质和歌单缓存配置
- 编辑器只保留本地音乐插入界面
- 修复当前 Typecho 版本下清空缓存后的页面跳转问题
- 移除旧版 SRI 残留逻辑
- 增加播放器脚本 CDN 前缀配置
- 保持 `[player]`、`[mp3]`、`[lrc]`、`[tlrc]` 短代码兼容

## 安装

1. 确保服务器支持 PHP cURL 扩展，并确保插件 `cache` 目录可写。
2. 将插件目录命名为 `cPlayer`，上传到博客的 `/usr/plugins` 目录。
3. 在 Typecho 后台启用 cPlayer 插件。
4. 在文章编辑器中点击“插入本地音乐”，或手动编写短代码。

## CDN 配置

进入后台的 cPlayer 设置页面，配置“播放器脚本 CDN 前缀”。

留空时使用插件本地资源：

```text
/usr/plugins/cPlayer/assets/dist/cplayer.js
```

例如填写：

```text
https://cdn.example.com/cPlayer/
```

实际加载地址为：

```text
https://cdn.example.com/cPlayer/cplayer.js?v=2.0.0
```

CDN 前缀应指向 `cplayer.js` 所在目录。播放器脚本使用跨域加载，CDN
需要允许站点跨域请求。

## 使用方法

### 单曲播放

音频 URL 是必填项，歌词、翻译歌词和封面均为可选项：

```text
[player url="https://example.com/music/song.mp3" artist="歌手" name="歌曲名称" cover="https://example.com/music/cover.jpg"/]
```

### 使用歌词 URL

```text
[player url="https://example.com/music/song.mp3" artist="歌手" name="歌曲名称" lrc="https://example.com/music/song.lrc"/]
```

翻译歌词需要在同一首歌曲上同时提供 `lrc` 和 `tlrc`：

```text
[player url="https://example.com/music/song.mp3" artist="歌手" name="歌曲名称" lrc="https://example.com/music/song.lrc" tlrc="https://example.com/music/song-tlrc.lrc"/]
```

### 直接嵌入歌词文本

```text
[player url="https://example.com/music/song.mp3" artist="歌手" name="歌曲名称"]
[lrc]
[00:00.00]第一句歌词
[00:05.00]第二句歌词
[/lrc]
[tlrc]
[00:00.00]Translation
[00:05.00]Translation
[/tlrc]
[/player]
```

### 播放列表

```text
[player autoplay="false"]
[mp3 url="https://example.com/music/song-1.mp3" artist="歌手一" name="歌曲一" lrc="https://example.com/music/song-1.lrc"/]
[mp3 url="https://example.com/music/song-2.mp3" artist="歌手二" name="歌曲二" cover="https://example.com/music/cover-2.jpg"/]
[/player]
```

## 短代码属性

### 播放器和歌曲属性

```text
url: 音频资源 URL，必填
name: 歌曲名称，未填写时显示 Unknown
artist: 艺术家，未填写时显示 Unknown
cover: 封面图片 URL；填写 false 可禁用封面；填写 search 可按歌曲信息自动查找封面
lrc: LRC 歌词 URL
tlrc: LRC 翻译歌词 URL，需要同时提供 lrc
lrcoffset: 歌词整体偏移时间，单位为 ms
autoplay: 是否自动播放，可用 true 或 false，默认为 false
```

本版本不再支持旧版的 `id`、`type`、`MUSIC_U` 等网易云音乐参数，请使用 `url`
指定音频资源。

`lrc` 和 `tlrc` URL 由服务器端读取并缓存，因此 PHP-FPM 必须能够访问对应地址。
如果歌词 URL 曾经获取失败，需要在插件设置中点击“清空歌词和封面缓存”后重试。

## 缓存与故障排查

- `cache` 目录用于保存歌词和封面缓存，必须可写。
- 点击插件设置中的“清空歌词和封面缓存”可以删除已有缓存。
- 如果音频可以播放但歌词不显示，优先检查服务器是否能访问歌词 URL，以及 URL 是否直接返回 LRC 文本。
- 文章中应填写原始 URL，例如 `lrc="https://example.com/song.lrc"`，不要填写 Markdown 链接格式。

## 许可证与致谢

本维护版基于 [journey-ad/cPlayer-Typecho-Plugin](https://github.com/journey-ad/cPlayer-Typecho-Plugin)，
底层播放器基于 [MoePlayer/cPlayer](https://github.com/MoePlayer/cPlayer)。

制作过程中参考了[zgq354](https://github.com/zgq354/APlayer-Typecho-Plugin)的代码，特此感谢。

本项目遵循 MIT License。原作者版权归 [journey.ad](https://github.com/journey-ad/)，维护版由 noisky 维护。
