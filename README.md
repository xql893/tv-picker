# TV 选台工具（实时源）

一个纯前端的电视直播选台网页：每次打开实时从 [iptv-org](https://iptv-org.github.io) 拉取最新频道列表，
支持按分类 / 国家 / 关键词筛选，手机内直接播放（hls.js），也可导出 `.m3u` 给电脑 VLC 用。

## 文件说明

| 文件 | 作用 |
|------|------|
| `index.html` | 线上主页面（自包含，数据由浏览器实时拉取，无需后端） |
| `server.py` | 可选本地后端（FastAPI），用于缓存 M3U 并提供搜索/导出 API |
| `gen.py` | 旧版生成器：把 M3U 解析后内联进 HTML 生成自包含单文件（现已被实时版取代） |
| `templates/index.html` | 早期模板（已被根目录 `index.html` 取代，仅供追溯） |

> 提示：`data/` 目录（含 `index.m3u`、`countries.json`）是下载缓存，非源码，
> 需要本地跑后端时按下方命令重新拉取即可。

## 直接用（最简单）

把 `index.html` 用任意浏览器打开即可，无需服务器、无需构建。

## 本地起后端（可选）

```bash
pip install fastapi uvicorn
# 先拉取数据
curl -s -o data/index.m3u https://iptv-org.github.io/iptv/index.m3u
curl -s -o data/countries.json https://iptv-org.github.io/api/countries.json
# 启动
python3 -m uvicorn server:app --host 0.0.0.0 --port 8000
# 浏览器打开 http://localhost:8000/tv
```

## 导出部署（GitHub Pages / 任意静态托管）

只需把 `index.html` 作为站点入口（根目录 `index.html`）发布即可，它是自包含的纯静态文件。

## 数据来源

频道数据来自社区公开索引 [iptv-org](https://github.com/iptv-org/iptv)，
仅索引播放链接、不托管视频；公开 IPTV 源可能随时失效或受地域限制。
