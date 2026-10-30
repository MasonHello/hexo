---
title: Mason Blog 搭建与配置说明
date: 2026-10-30 15:20:00
author: Mason
top: true
cover: true
coverImg: /medias/featureimages/9.jpg
img: /medias/featureimages/9.jpg
summary: 记录本站基于 Hexo 8 + hexo-theme-matery 的完整配置，方便日后维护与迁移。
categories: 工程实践
tags:
  - Hexo
  - Matery
  - 建站
---

## 一、站点信息

站点名称、作者、描述等都在根目录 `_config.yml` 中配置：

```yaml
title: Mason
subtitle: '宁静致远'
description: 'Mason 的个人博客，记录网络安全研究、渗透测试、代码审计与工程实践。'
author: Mason
language: zh-CN
timezone: 'Asia/Shanghai'
url: https://masonhello.github.io/hexo
root: /hexo/
```

## 二、页面清单

| 页面 | 路径 | 文件 |
| :--- | :--- | :--- |
| 首页 | `/` | 由 `hexo-generator-index` 生成 |
| 标签 | `/tags/` | `source/tags/index.md` |
| 分类 | `/categories/` | `source/categories/index.md` |
| 归档 | `/archives/` | 自动生成 |
| 关于 | `/about/` | `source/about/index.md` |
| 留言板 | `/contact/` | `source/contact/index.md` |
| 友链 | `/friends/` | `source/friends/index.md` + `source/_data/friends.json` |
| 404 | `/404.html` | `source/404.md` |

### 导航结构

顶部导航为五个分组，父级只作分组不跳转（`children` 二级菜单）：

| 分组 | 子项 | 路径 |
| :--- | :--- | :--- |
| HOME | — | `/` |
| SEC | WEB / CTF | `/categories/WEB/`、`/categories/CTF/` |
| CODE | JAVA / Python | `/categories/Java/`、`/categories/Python/` |
| Internet | HCIP / Linux | `/categories/HCIP/`、`/categories/Linux/` |
| Others | 标签 / 分类 / 归档 | `/tags/`、`/categories/`、`/archives/` |
| 关于 | 留言板 / 友情链接 | `/contact/`、`/friends/` |

> 六个技术分类各有一篇 `hide: true` 的「栏目说明」占位文章（`column-*.md`），用于让分类页存在且不出现在首页；有真实文章后可删除。
## 三、已安装插件

```bash
hexo-generator-search      # 站内搜索
hexo-wordcount             # 字数统计 / 阅读时长
hexo-permalink-pinyin      # 中文标题转拼音链接
hexo-generator-feed        # RSS（atom.xml）
```

## 四、写作模板

新建文章后会自动带入 `scaffolds/post.md` 中的 Front-matter：

```yaml
---
title: {{ title }}
date: {{ date }}
author: Mason
categories:
tags:
summary:
---
```

常用 Front-matter 选项：

- `top: true` — 首页置顶 / 推荐
- `cover: true` + `coverImg` — 加入首页轮播
- `img` — 文章特色图
- `toc: false` — 关闭本篇目录
- `mathjax: true` — 开启公式渲染

## 五、自定义部分

### 5.1 版权信息

底栏版权在 `themes/hexo-theme-matery/layout/_partial/footer.ejs` 中，已统一改为 Mason：

```html
Copyright © 2026 Mason | All Rights Reserved.
Powered by Hexo | Theme by Mason
```

文章正文复制时的版权声明在主题 `_config.yml` 的 `copyright` 配置中。

### 5.2 友链与 t00ls

所有站外入口统一收敛到「友情链接」页，数据在 `source/_data/friends.json`：
顶部导航、首页 banner 按钮、社交图标、底栏都不再放 t00ls 入口。

### 5.3 配色

整体主色调在 `themes/hexo-theme-matery/source/css/matery.css` 中搜索 `.bg-color` 修改。

### 5.4 打字副标题（一言 API）

banner 下方的打字文案来自「一言」接口 <https://hitokoto.cn>，配置在主题 `_config.yml`：

```yaml
subtitle:
  enable: true
  source: hitokoto
  api: https://v1.hitokoto.cn/?encode=text&charset=utf-8
  category: 'd,i,k'     # 文学/诗词/哲学；留空 = 全部分类随机
  loop: true
  refreshInterval: 4000 # 一句展示完后换下一句的间隔
```

- 接口不可用（被墙 / 离线）时会自动轮换 `sub` 里的兜底文案，页面不会空着
- 把 `source` 改成 `static` 即可只用本地文案轮播
- 首页轮播间隔 `cover.intervalTime` 建议 ≥ 8000ms，否则一句话还没打完就被切走
### 5.5 文章时效性提示

文章正文顶部会按「发布至今多久」自动决定提示强度，太新的文章不提示：

| 发布时长 | 表现 | 文案 |
| :--- | :--- | :--- |
| < 30 天 | 不提示 | — |
| ≥ 30 天 | 琥珀色 + info | 本文发布于 {ago}（{date}），文中部分细节可能已有变化，请留意甄别。 |
| ≥ 180 天 | 橙色 + 警告三角 | 本文发布于 {ago}（{date}），部分内容可能已不再适用，请酌情参考。 |
| ≥ 365 天 | 红色 + 圆形叹号 | 本文发布于 {ago}（{date}），技术更新较快，内容大概率已过时，请谨慎参考。 |

- 模板：`themes/.../layout/_partial/post-detail.ejs`；样式：`themes/.../source/css/my.css` 的 `.post-outdated-notice`
- 「{ago}」由浏览器按当前时间实时计算（N 天前 / N 个月前 / N 年前），静态站点久不重建也不会算错
- 主题 `_config.yml` 中可自由增删分级：

```yaml
postNotice:
  enable: true
  levels:
    - days: 30
      icon: fas fa-info-circle
      text: 本文发布于 {ago}（{date}），文中部分细节可能已有变化，请留意甄别。
    - days: 180
      icon: fas fa-exclamation-triangle
      text: 本文发布于 {ago}（{date}），部分内容可能已不再适用，请酌情参考。
    - days: 365
      icon: fas fa-exclamation-circle
      text: 本文发布于 {ago}（{date}），技术更新较快，内容大概率已过时，请谨慎参考。
```
### 5.6 样式来源：jwt1399.top

全站外观对齐 <https://jwt1399.top/>（同样是 Matery 主题）。做法不是手抄，而是**逐条比对**两边 `matery.css`，把取值不同的声明抽成独立皮肤文件：

```
themes/hexo-theme-matery/source/css/reference-skin.css   # 参考站差异样式（47 条）
themes/hexo-theme-matery/source/css/my.css               # 仅保留代码等宽 + 时效性提示
```

加载顺序（`layout/_partial/main-style.ejs`）：

```html
matery.css  →  reference-skin.css  →  my.css  →  dark.css
```

皮肤里包含：`.bg-color` 蓝粉渐变 + 90% 不透明度、`blockquote` 红边框、代码块 `#272822` 底与 `#de5448` 行内代码色、进度条、滚动条、表格边框、`.container` 85% 宽度等。

**banner / 封面**不套参考站皮肤，保持 Matery 默认样式（与 <https://memme.cn/> 一致）：背景图 + `rainbow` 紫色渐变遮罩动画、标题 4rem、胶囊按钮、社交图标、轮播指示器，文章页封面也用文章特色图。

全局字体完全按参考站设置（`reference-skin.css` 末尾）：

```css
:root {
    --site-font: Verdana, Geneva, "DejaVu Sans", "Microsoft YaHei", "PingFang SC",
        "Hiragino Sans GB", "Heiti SC", "WenQuanYi Micro Hei", sans-serif;
    --site-code-font: Inconsolata, Monaco, Consolas, "Courier New", Courier, monospace;
}

html, body, button, input, select, textarea,
h1, h2, h3, h4, h5, h6, p, a, li, td, th, blockquote,
.btn, .chip, .card, .post-title, .article-content,
.bg-cover .title, .nav-menu, .copy-right, .toc-title, … {
    font-family: var(--site-font);
}

code, pre, .code_lang, pre[class*="language-"], code[class*="language-"] {
    font-family: var(--site-code-font) !important;
}
```

即全站西文/数字用 Verdana、中文交给系统字体，代码用参考站的 Inconsolata/Monaco 栈。
参考站还自托管了 `Verdana.ttf` / `Monaco.ttf`（`@font-face JVer/JMon`）；本项目默认用系统字体，若要跨平台一致，把 ttf 放到 `source/medias/fonts/` 并启用 skin 中注释的 `@font-face`。

要改配色，直接编辑 `reference-skin.css` 中对应声明即可（每一条都能在参考站 CSS 里找到出处）。
## 六、常用命令

```bash
hexo clean && hexo g && hexo s   # 清理、生成、本地预览
hexo new "文章标题"               # 新建文章
hexo new page "页面名"            # 新建页面
```

> 本地预览地址：<http://localhost:4000/hexo/>
