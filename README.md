# 组会 Touying 模板
**后续转入 [typst-packages](https://github.com/Chenyang-Wang-1999/typst-packages) 开发，此仓库不再更新**

根据 `组会PPT模版.pptx` 重建。原稿为 16:9、960 × 540 pt，采用清华紫 `#5B2F7C`、灰色封面标题框、目录竖线及正文页上下横条。

## 文件

- `main.typ`：可直接修改的组会示例，含封面、自动目录、正文、双栏公式、表格和结束页。
- `main.pdf`：示例预览，共 10 页，含每个章节前的高亮目录。
- `theme.typ`：可复用的主题和页面函数。
- `replica.typ` / `replica.pdf`：保留原 PPT 的 5 页内容与顺序，使用当前模板样式。
- `assets/`：原稿标识、校园剪影及反白 SVG。复制模板时请保留整个目录。

## 开始使用

直接编辑 `main.typ`。VS Code 中可用现有的 Tinymist 预览和导出 PDF；也可在本目录运行：

```powershell
typst compile main.typ main.pdf
typst watch main.typ main.pdf
```

本次使用本机 Tinymist 0.15.6 编译验证，依赖固定为 `@preview/touying:0.7.4`。第一次在新设备编译需要下载 Touying 及其依赖。全局中文字体为黑体（`SimHei`），英文为 Arial，公式独立使用 `New Computer Modern Math`。页脚副标题的中文使用华文新魏（`STXinwei`），英文仍使用 Arial。换设备时请确保这些字体可用，或通过对应参数指定替代字体。

最小示例：

```typst
#import "@preview/touying:0.7.4": *
#import "theme.typ": *

#show: group-meeting-theme.with(
  config-info(
    title: [报告标题],
    subtitle: [博士生论坛报告],
    author: [姓名],
    institution: [清华大学物理系],
    date: datetime.today(),
  ),
)

#title-slide()
#outline-slide()

= 研究背景
== 研究问题

- 第一个问题
- 第二个问题

= 本周进展
== 主要结果

这里填写正文。

#closing-slide()
```

`= 一级标题`定义章节，`== 二级标题`开始新的一页。左上角的 `Part 01`、`Part 02` 自动随章节更新。每个章节开始前自动显示一次目录，当前章节行（含方块）为清华紫，其余行为灰色。封面、目录参与逻辑页码计数，但不显示页码。动画的多个步骤显示同一页码。

## 主题设置

下面这些参数都写在 `group-meeting-theme.with(...)` 中：

| 参数 | 默认值 | 用途 |
| --- | --- | --- |
| `primary` | `rgb("5b2f7c")` | 横条、竖线、标题与强调文字的颜色 |
| `brand` | `"physics"` | 封面和目录标识，可改为 `"university"` |
| `font` | `("Arial", "SimHei")` | 英文与中文字体 |
| `math-font` | `"New Computer Modern Math"` | 公式字体，独立于全局字体 |
| `subtitle-font` | `("Arial", "STXinwei")` | 页脚副标题的英文与中文字体 |
| `body-size` | `20pt` | 正文字号 |
| `title-size` | `28pt` | 正文页顶部标题字号，长标题会适当缩小 |
| `section-slides` | `true` | 自动插入当前章节的高亮目录，`false` 可关闭 |
| `outline-v-spacing` | `auto` | 所有目录的条目间距，默认均匀分布，可设为 `32pt` 等长度 |
| `show-contents` | `true` | 是否显示目录，`false` 可隐藏 |
| `part-prefix` | `"Part"` | 左上角章节编号的前缀 |
| `footer` | `auto` | 默认显示日期和副标题；自定义内容可覆盖，`none` 可隐藏 |
| `show-page-number` | `true` | 是否显示正文页页码 |
| `cover-logo` | `auto` | 覆盖封面/目录标识，可传入 `image(...)` 或 `none` |
| `header-logo` | 透明反白 SVG | 覆盖正文页右上角标识，可传入内容或 `none` |
| `campus` | 原稿校园剪影 | 覆盖封面/目录右下角剪影，`none` 可隐藏 |

例如，更改横条背景色：

```typst
#show: group-meeting-theme.with(
  primary: rgb("194f6b"),
  brand: "university",
  config-info(title: [报告标题]),
)
```

正文校徽为透明背景的白色矢量路径，可随横条背景换色。封面、目录上的紫色标识和校园剪影保留原图颜色，不随 `primary` 重新着色；需要时可替换或隐藏。

另外导出四个颜色变量，可直接用于 `text`、线条或图表：

```typst
#text(red)[红色]       // #D62728
#text(blue)[蓝色]      // #005795
#text(green)[绿色]     // #1A5F1A
#text(orange)[橙色]    // #C45C00
```

## 页面与内容

### 封面

```typst
// 标题、作者、单位居中，日期和副标题放在左下角。
#title-slide()

// 或单独指定这一页的标题、标识和补充内容。
#title-slide(title: [标题], subtitle: [博士生论坛报告],
  logo: university-logo, extra: [课题组名称])
```

封面标题建议控制在两行内；标题框尺寸固定，长标题需要主动换行或精简。

`config-info.subtitle` 默认仅出现在页脚日期后面，例如 `2026-09-07    博士生论坛报告`。日期和副标题（包括中英文）均为 18pt，中间间隔 18pt。封面页脚使用清华紫，左端与标题框左端（47.18pt）对齐，文字下缘与右下角校园图下缘（515.25pt）对齐；结束页沿用这一位置。正文页脚使用白色，并在底部横条内与页码共用基线、垂直居中。总目录和章节高亮目录均不显示页脚。若需要保留原稿的并排标题形式，仍可显式使用 `#title-slide(inline-subtitle: true)`。

### 目录

`#outline-slide()` 自动收集一级标题并生成可点击的目录。目录文本框以整张 slide 的垂直中线（270pt）为中心，上下留白相等，避开顶部标题、标识和底部校园剪影。未指定间距时，多行条目均匀铺满整个可用文本区（120–420pt）；只有一个章节时，该行居于 slide 正中。也可手动指定条目和垂直间距：

```typst
#outline-slide(title: [目录], v-spacing: 32pt, items: (
  [研究背景],
  [本周进展],
  [后续计划],
))
```

默认目录适合约 3–5 个章节；条目过长或过多时，应拆成多页并分别传入 `items`。

单页的 `v-spacing` 覆盖全局 `outline-v-spacing`；两者均为 `auto` 时自动分配空白。手动间距表示相邻条目之间的净空白，此时将所有条目组成的整个文本组垂直居中，而非从顶部开始排列。`active: 2` 可手动高亮第 2 行，`active: auto` 高亮当前章节，`active: none` 为不高亮的总目录。自动章节目录无需手动调用。

### 普通页和双栏

```typst
// 二级标题方式，自动读取章节编号。
== 实验结果

这里填写正文。

// 显式创建页面并指定顶部文字。
#slide(title: [实验结果], part: [Part 02])[
  这里填写正文。
]

// 双栏比例可修改。
#slide(title: [理论模型], composer: (1fr, 1fr))[
  左栏内容。
][
  $ E = m c^2 $
]
```

正文可直接使用 Typst 的列表、数学公式、`image`、`table`、`figure` 等。为了保留一页对应一张幻灯片的关系，主题关闭自动溢出分页，并启用 Touying 的正文溢出警告。出现警告时请减少内容或手动拆页。

### 动画与讲义

```typst
== 逐步显示

第一步。

#pause

第二步。
```

导出讲义时，在主题参数中加入 `config-common(handout: true)`，每张幻灯片只保留最终步骤。`#meanwhile`、演讲者备注等继续使用 Touying 提供的接口。

### 结束页

```typst
#closing-slide(body: [谢谢！], subtitle: [欢迎讨论])
```

## 素材说明

物理系标识和紫色清华大学标识直接从原 PPT 提取。校园剪影从原 PPT 渲染结果中提取，以保留 PowerPoint 对图片应用的双色效果。封面框、目录竖线和正文横条均由 Typst 原生图形构成，文字、表格和公式可直接编辑。

`assets/university-logo-white.svg` 来自用户提供的 `校徽反白.svg`。仅在模板内的副本中移除了紫色背景矩形，将裁剪组的变换移到路径上以兼容 Typst，并收紧画布留白；白色校徽路径保持原样。上一级目录的原 PPT 和原 SVG 未修改。标识与图片的权利归原权利人，不因模板转换而改变。

字体渲染、段落间距和 SVG 校徽与 PowerPoint 原稿存在细微差异，本模板保留原稿的主要几何尺寸和设计。

Touying 的接口可参考 [官方文档](https://touying-typ.github.io/zh/docs/intro/) 和 [0.7.4 包页面](https://typst.app/universe/package/touying/)。

## 保存到本地库
`typst` 的本地库默认保存在 `~\AppData\Roaming\typst\packages\local`。在该文件夹下创建文件夹 `thu-Touying\0.1.0`，然后将`assets`, `theme.typ`与`typst.toml` 复制到该文件夹下即可。

目录结构:
```
~\AppData\Roaming\typst\packages\local\thu-Touying\0.1.0
├── assets
│   ├── university-logo-white.svg
│   ├── campus.png
│   ├── university-logo.jpeg
│   └── physics-logo.png
├── theme.typ
└── typst.toml
```
