# 下蛊状态栏 · di-lao 图床归档说明

> 对象：酒馆正则脚本「状态栏」`regex-下蛊状态栏.json`
> 原图床：`files.catbox.moe`（131 张）+ `i.postimg.cc`（27 张）+ `lib.baomitu.com`（jQuery/lodash）+ `fonts.googleapis.com`（3 款中文字体）
> 现图床：`Yehehua1311/di-lao`（jsDelivr CDN + raw 双通道），前缀 `下蛊/`
> 迁移日期：2026-10-15　执行脚本：`_xia_gu/`

---

## 1. 目录结构

```
下蛊/
├─ 01_常规立绘_SFW/        37 张  <名字>_N1.png … 卡面普通态（SFW）
├─ 02_情欲立绘_NSFW/       54 张  <名字>_S1.png … 情欲态（NSFW）
├─ 03_深堕立绘_Deep/       67 张  <名字>_D1.png … 深堕/沉沦态（Deep）
├─ 04_占位图_Placeholder/   1 张  默认占位图（新引入，卡片原件未引用）
├─ 05_依赖库_Libs/          2 个  jQuery 3.6.0 / lodash 4.17.21（镜像存档）
├─ 06_字体_Fonts/         285 个  google-fonts.css + 284 个 woff2 分片（完全离线）
└─ README.md                     本文件
```

合计 **446 个文件 / 266.1 MB**（PNG 159 + JS 2 + 字体 285）。

## 2. 仓库路径与链接规则

| 项 | 值 |
|---|---|
| 仓库 | `Yehehua1311/di-lao`（public，与「深渊地牢」「寄生虫状态栏」同仓共存） |
| 分支 | `main` |
| 前缀 | `下蛊/` |
| CDN | `https://cdn.jsdelivr.net/gh/Yehehua1311/di-lao@main/<按段编码路径>` |
| raw | `https://raw.githubusercontent.com/Yehehua1311/di-lao/main/<按段编码路径>` |
| 备用节点 | 把 `cdn.jsdelivr.net` 换成 `gcore.jsdelivr.net` |

**编码约定**：路径按 `/` 切段后逐段 `encodeURIComponent` 再 `join('/')`。
注意前缀 `下蛊` 本身也要编码为 `%E4%B8%8B%E8%9B%8A`；整条路径一次性编码会把分隔斜杠变成 `%2F`。

## 3. 图床对应关系

卡片里是单一常量 `window.charAvatars`，结构为 `{ 角色名: [{ url, type }] }`，`type` 取 `sfw` / `nsfw` / `deep`：

| 键名 | 是否在 `defaultCharNames` | 张数 | sfw / nsfw / deep |
|---|---|---|---|
| 姬无双 | ✔ | 9 | 2 / 3 / 4 |
| 萧凤鸾 | ✔ | 9 | 2 / 3 / 4 |
| 冷清秋 | ✔ | 9 | 2 / 3 / 4 |
| 夜幽兰 | ✔ | 8 | 2 / 3 / 3 |
| 独孤雪 | ✔ | 8 | 2 / 3 / 3 |
| 梅傲雪 | ✔ | 9 | 2 / 3 / 4 |
| 白芷 | ✔ | 11 | 2 / 4 / 5 |
| 赵明月 | ✔ | 9 | 2 / 3 / 4 |
| 柳如月 | ✔ | 9 | 2 / 3 / 4 |
| 燕飞飞 | ✔ | 8 | 2 / 3 / 3 |
| 曲灵 | ✔ | 9 | 2 / 3 / 4 |
| 南宫婉 | ✔ | 8 | 2 / 3 / 3 |
| 苏婉柔 | ✘（外室/扩展角色） | 3 | 1 / 1 / 1 |
| 云间月 | ✘ | 9 | 2 / 3 / 4 |
| 梵音 | ✘ | 9 | 2 / 3 / 4 |
| 千叶琉璃 | ✘ | 4 | 2 / 1 / 1 |
| 阿依莎 | ✘ | 9 | 2 / 3 / 4 |
| 汤娇娇 | ✘ | 9 | 2 / 3 / 4 |
| 千叶凛 | ✘ | 9 | 2 / 3 / 4 |

合计 **158 个立绘槽位 / 158 个唯一 URL**，分布在 19 个角色键下（12 个在默认名册内，7 个是外室/扩展角色）。

**源图规格两种**（沿用原图，未做压缩/换算）：

| 来源 | 规格 | 张数 |
|---|---|---|
| `files.catbox.moe` | 832×1216 | 131 |
| `i.postimg.cc` | 547×799 | 27 |
| 占位图（新引入） | 443×647 | 1 |

> 两种规格并存是原卡片本身就有的（前 3 张一组的 catbox 与后 3~5 张一组的 postimg 混排）。
> 卡片渲染用 `object-fit: cover` 的固定容器，因此两种比例都会被裁切填满，不影响布局。

## 4. 迁移时同步修掉的 1 处代码问题

卡片的图片缓存模块（IndexedDB + MutationObserver）原本按**旧图床域名**过滤待缓存的 `<img>`：

```js
// 原件
if (!src.includes("postimg.cc") && !src.includes("catbox.moe")) return;
```

迁移后这条判断永远不成立，会导致 **预缓存后台任务全部空转**（`startPrecache` 会遍历
`charAvatars` 的 158 条 URL 走一遍 `fetch`，但 `applyImgCacheToNode` / `getCachedImageUrl`
的 blob 兜底链路彻底失效），表现为弱网下立绘反复重新加载。

已改为按新图床判定（含备用节点与 raw 通道）：

```js
// 迁移版
if (!src.includes("cdn.jsdelivr.net") && !src.includes("gcore.jsdelivr.net") && !src.includes("raw.githubusercontent.com")) return;
```

除这一处外，`replaceString` 与原件**逐字节一致**（把 URL 剥离并把这处适配归一化后比对）。

## 5. 字体离线化

原卡片顶部是 1 条 Google Fonts `@import`（一次请求 3 款中文字体：`Ma Shan Zheng` / `Noto Serif SC` wght 400·700 / `Zhi Mang Xing`）。

| 项 | 结果 |
|---|---|
| 抓到的 `@font-face` 块 | 386 个 |
| 本地化改写 | **382 个块** → 同目录相对文件名 |
| 下载分片 | **284 个 woff2，11.6 MB**，魔数全部合法 |
| 悬空引用 | **0** |
| 绝对地址兜底 | 2 处（0.5%） |

那 2 处保留 `fonts.gstatic.com` 绝对地址的原因：它们的 `url()` 与 `unicode-range`
同时被传输层注入打坏，跨 10 次采样也无法在其他样本里认回真值（详见 §7）。
影响仅限于对应 unicode-range 的极少数生僻字，会走 CDN 首次回源，其余全部本地命中。

## 6. 校验结果

| 项 | 结果 |
|---|---|
| 本地素材 | 159/159 魔数合法，IHDR 宽高与清单全一致 |
| 图床链接 | 161/161 实测 HTTP 200 + `Content-Type: image/png` / `javascript` / `text/css` |
| 字体分片 | 284/284 `wOF2` 魔数合法，CSS 无悬空引用 |
| 成品 JSON | 13 个字段除 `replaceString` 外逐字段一致；无 BOM |
| 差异纯净性 | 剥离 URL + 归一化 1 处刻意适配后，与原件**逐字节一致** |
| 内嵌 JS | 3 个 `<script>` 块全部通过 V8 `vm.compileFunction` |
| 标签配对 | 与原版逐项一致且全部平衡（div 229/229，script 5/5） |
| 原有功能 | 23 项关键片段在位；19 个角色键、158 条 `url/type` 条目 |
| 旧图床残留 | catbox 0、postimg 0、baomitu 0、googleapis 0 |
| 校验总计 | **35/35 通过** |

详细报告见 `_xia_gu/_验证报告.json`。

## 7. 已知坑（复用本流程时重点看这几条）

### 7.1 传输层会往 HTTP 响应里注入 4 字节垃圾

抓 `fonts.googleapis.com` 时，响应正文**每 32768 字节**被插入「4 个随机十六进制字符 + CRLF」，
落点是硬偏移、与内容无关：`0 / 63926 / 96688 / 129450 / 162212 / 194974 / 227736 / 260498 / 293260 / 391554`。

后果是 CSS 里既有 `src: ur<CRLF>7967<CRLF>l(...)` 这种被劈开的 `url(`，
也有 `'N7ff2oto Serif SC'` 这种被打坏的字体族名，还有
`.../H4ch…tJDWv<CRLF>7ff2<CRLF>3z-us4bxD8F5og.40.woff2` 这种被劈开的 URL。

**因此**：

- 「同一位置跨样本比对」无效 —— 同一个字节偏移会把**同一个位置**打坏；
- 「删掉固定串 `7ff2`」也无效 —— 注入字符是随机的（见过 `7ff2` / `96c6` / `6c6c` / `63fa` / `8000` / `3b3e`）；
- 可靠做法：多次采样取并集 + **以「磁盘上确实存在该文件的合法 wOF2」为硬判据**认领真值；
- 改 `url()` 只能用单遍正则回调；`split(绝对URL).join(文件名)` 循环替换时，
  文件名是绝对 URL 的子串，多遍替换会互相吃掉，产出 `.../fonts.gstatic.c7ff2om/...` 畸形域名。

### 7.2 上传前的「源目录隔离」

`gh-upload.mjs` 直接按目录名映射仓库路径，如果同一个本地目录里混着上一个项目的素材，
会一并推到新前缀下。本次就把「寄生虫状态栏」的 19 个旧文件扫了进来，已中止并移至
`_xia_gu/../_寄生虫旧素材备份/` 后再重传。

### 7.3 jsDelivr 冷缓存

新推送的文件首次访问可能返回 503 或短暂 404，探测需带退避重试；raw 通道实时可用。
CDN 对 `@main` 有缓存，改同名文件后可能短暂返回旧图，需立即生效可换成具体 commit sha。

### 7.4 导入后状态栏不显示

`<StatusPlaceHolderImpl/>` 是浏览器不认识的自定义标签，**没被正则替换掉时不可见**，
表现为「整条状态栏消失」。除了脚本未启用/作用域不对/看的还是旧消息之外，
本仓库「寄生虫状态栏」那套排查里已经定位过一个易踩的引擎语义：**同一角色卡内嵌正则按数组顺序执行**，
若前面有一条 `replaceString` 为空串的脚本抢同一个锚点，后面的状态栏脚本就永远匹配不到。
本次交付沿用原件 `id` 与元数据（`placement: [2]` / `markdownOnly: true` / `runOnEdit: true`），
未改动导入语义。
