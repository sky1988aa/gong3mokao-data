# GONG3MOKAO 远程数据

公共营养师考试题库的远程数据，供微信小程序按需加载（规避主包体积限制）。

托管：GitHub Pages / jsDelivr CDN
来源：`GONG3MOKAO/dist/cloudbase/`（由 `tools/build.py` 自动构建，**勿手动编辑本仓库文件**）

## 文件

| 文件 | 大小 | 用途 | 消费方 |
|------|------|------|--------|
| `explanations.json` | ~1.9M | 题目解析（id → 解析文本） | `study` / `review` / `favorite` 页 |
| `chapter-decks.json` | ~433K | 章节闪卡数据（19 章 1120 张） | `chapter` 页（阶段二） |

## 引入方式

小程序通过 `app.js` 的 `globalData` 配置 URL（**commit 固定引用**，规避 CDN 缓存延迟）：

```js
explanationUrl: 'https://cdn.jsdelivr.net/gh/sky1988aa/gong3mokao-data@<commit>/explanations.json',
explanationCacheVersion: 12,
chapterDeckUrl: 'https://cdn.jsdelivr.net/gh/sky1988aa/gong3mokao-data@<commit>/chapter-decks.json',
chapterDeckCacheVersion: 1,
```

## 更新流程

1. 在主仓库改数据 → `python tools/build.py` 产出新文件
2. `cp dist/cloudbase/*.json` 到本仓库
3. commit + push（记录新 commit hash）
4. 把新 hash 写入 `app.js` 的 URL，并 bump 对应的 cache version
5. 小程序端检测到 version 变化后自动重新拉取
