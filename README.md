# di-lao · 图床

深渊地牢（角色卡）状态栏素材图床，公开仓库，供 SillyTavern 正则「状态栏」引用。

## 目录结构

| 目录 | 内容 | 数量 |
|---|---|---|
| `01_女主立绘/` | 4 位女主立绘（547×799，对应卡片 `aspect-ratio: 13/19`） | 4 |
| `02_NPC随机池/` | 普通 NPC 通用随机立绘池 | 5 |
| `03_固定NPC立绘/` | 按姓名硬绑定的 NPC 立绘 | 2 |
| `04_CG画廊/` | 每位女主 3 张 CG（800×800），按 女主 分子目录 | 12 |

CG 解锁条件：CG1 = 女主专属值 > 333，CG2 = 专属值 > 666，CG3 = 生命刻印结成。

## 引用方式

jsDelivr CDN（推荐，有缓存加速）：

```
https://cdn.jsdelivr.net/gh/Yehehua1311/di-lao@main/<路径>
```

raw 直连（实时，无缓存）：

```
https://raw.githubusercontent.com/Yehehua1311/di-lao/main/<路径>
```

路径中的中文与特殊字符需做 URL 编码，例如
`01_女主立绘/法莲娜_拜伦西娅.png` →
`https://cdn.jsdelivr.net/gh/Yehehua1311/di-lao@main/01_%E5%A5%B3%E4%B8%BB%E7%AB%8B%E7%BB%98/%E6%B3%95%E8%8E%B2%E5%A8%9C_%E6%8B%9C%E4%BC%A6%E8%A5%BF%E5%A8%85.png`

## 备注

仓库参考 `mu-pig` 图床的用法（GitHub 仓库 + jsDelivr）。GitHub 对单仓库有 1 GB 软限制，图片增多时需留意。
