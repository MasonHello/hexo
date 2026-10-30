# Mason

> 宁静致远
>
> 基于 [Hexo 8](https://hexo.io/) + [hexo-theme-matery](https://github.com/blinkfox/hexo-theme-matery) 搭建的个人技术博客。

- 站点地址：<https://masonhello.github.io/hexo>
- 站点名称：**Mason**
- 作者 / 版权：**Mason**
- GitHub：<https://github.com/MasonHello>

---

## 一、功能一览

| 模块 | 说明 |
| :--- | :--- |
| 首页 | 全屏 banner 轮播 + 「一言」API 逐字打印副标题 + "我的信条"卡片 + 推荐置顶 + 文章水瀑流列表 |
| 标签页 | 标签词云（`/tags/`） |
| 分类页 | 分类雷达图（`/categories/`） |
| 归档页 | 文章日历热力图 + 时间轴（`/archives/`） |
| 关于页 | 个人资料 + 文章统计 + 我的项目 + 我的技能 + Markdown 正文（`/about/`） |
| 留言板 | 联系方式 + 留言入口（`/contact/`） |
| 友链页 | 卡片式友链（`/friends/`，数据在 `source/_data/friends.json`） |
| 404 | 自定义 404 页面（`/404.html`） |
| 站内搜索 | 顶栏搜索图标，基于 `hexo-generator-search` |
| RSS | `/atom.xml`，基于 `hexo-generator-feed` |
| 其他 | 深色模式、文章目录、代码高亮/复制/折叠、复制正文版权提示、文章时效性提示（发布于 X 前）、字数/阅读时长/访问统计（不蒜子）、中文标题转拼音链接 |

## 二、目录结构

```
.
├── _config.yml                 # 站点主配置（站名、作者、url、插件、部署）
├── package.json                # 依赖与脚本
├── scaffolds/                  # 新建文章 / 页面的默认模板（已带 author: Mason）
├── source/
│   ├── _posts/                 # 文章
│   ├── _data/friends.json      # 友链数据
│   ├── 404.md                  # 404 页面
│   ├── about/index.md          # 关于页
│   ├── categories/index.md     # 分类页
│   ├── tags/index.md           # 标签页
│   ├── contact/index.md        # 留言板
│   └── friends/index.md        # 友链页
└── themes/hexo-theme-matery/
    ├── _config.yml             # 主题配置（菜单、社交、项目、技能、评论等）
    ├── layout/                 # EJS 模板（已按 Mason 定制）
    └── source/                 # 主题静态资源（含 Mason 的 logo / avatar / favicon）
```

## 三、导航结构

顶部导航是六个分组（父级只作分组、点击不跳转，桌面端悬停展开，移动端抽屉内点击展开）：

| 分组 | 子项 | 目标 |
| :--- | :--- | :--- |
| HOME | — | `/` |
| SEC | WEB / CTF | `/categories/WEB/`、`/categories/CTF/` |
| CODE | JAVA / Python | `/categories/Java/`、`/categories/Python/` |
| Internet | HCIP / Linux | `/categories/HCIP/`、`/categories/Linux/` |
| Others | 标签 / 分类 / 归档 | `/tags/`、`/categories/`、`/archives/` |
| 关于 | 留言板 / 友情链接 | `/contact/`、`/friends/` |

配置位置：主题 `_config.yml` 的 `menu`（用 `children` 写二级菜单）。

> WEB / CTF / Java / Python / HCIP / Linux 六个分类目前各有一篇 `hide: true` 的「栏目说明」占位文章（`source/_posts/column-*.md`），**只为让分类页存在并列出栏目**，不会出现在首页。等对应分类有真实文章后，删掉占位文章即可。
## 四、站点主配置（`_config.yml`）

```yaml
title: Mason
subtitle: '宁静致远'
description: 'Mason 的个人博客，记录网络安全研究、渗透测试、代码审计与工程实践。'
keywords: 'Mason, 网络安全, 渗透测试, 代码审计, 漏洞分析, 安全研究'
author: Mason
language: zh-CN
timezone: 'Asia/Shanghai'

url: https://masonhello.github.io/hexo   # GitHub Pages 项目站点写法
root: /hexo/                              # 与 url 中的仓库名保持一致
permalink: :year/:month/:day/:title/
```

> **换域名怎么办？** 把 `url` 改成 `https://your-domain.com`，`root` 改成 `/` 即可，
> 其余链接全部由 `url_for()` 自动生成，不需要逐页修改。

已启用插件：

- `hexo-generator-search` —— 站内搜索（生成 `search.xml`）
- `hexo-generator-feed` —— RSS（生成 `atom.xml`）
- `hexo-wordcount` —— 字数、阅读时长、站点总字数
- `hexo-permalink-pinyin` —— 中文标题转拼音路径

代码高亮使用 Hexo 内置 `prismjs`（`highlight.enable: false` + `prismjs.enable: true`）。

## 五、Mason 定制点

1. **版权统一为 Mason**
   - 底栏：`themes/.../layout/_partial/footer.ejs` → `Copyright © 2026 Mason | All Rights Reserved.`
   - 主题署名：`Powered by Hexo | Theme by Mason`（链接 <https://github.com/MasonHello>）
   - 文章末尾授权声明：主题 `_config.yml` 的 `reprint`（默认 `cc_by_nc_sa`）
   - 复制正文时的版权提示：主题 `_config.yml` 的 `copyright`
   - `_config.yml` 的 `author: Mason`、`scaffolds/*.md` 的 `author: Mason`
2. **品牌视觉**
   - `themes/.../source/medias/logo.png`、`avatar.jpg`、`favicon.png` 均已替换为 Mason 标识
   - 头像、关于页资料、我的项目、我的技能均改为 Mason 的内容
3. **打字副标题走「一言」API**
   - 主题 `_config.yml` 的 `subtitle`：`source: hitokoto`，接口 `https://v1.hitokoto.cn/?encode=text&charset=utf-8`
   - 每次加载预取 `quoteCount`（默认 5）条，交给 Typed.js 循环轮播；每条停留 `refreshInterval`（默认 4000ms）
   - 分类 `category` 默认 `'d,i,k'`（文学 / 诗词 / 哲学），可用英文逗号组合；留空 = 官方全部分类随机；`l` 为抖机灵
   - 接口 2 秒未返回或失败时自动使用 `sub` 里的兜底文案，页面不会空着
   - `source: static` 可切回纯本地文案轮播
4. **文章时效性提示（按发布时间分级）**
   - 文章页正文顶部会按「发布至今的天数」自动决定是否提示、提示强度：
     | 发布时长 | 表现 | 文案 |
     | :--- | :--- | :--- |
     | < 30 天 | 不提示 | — |
     | ≥ 30 天 | 琥珀色 + info 图标 | 本文发布于 {ago}（{date}），文中部分细节可能已有变化，请留意甄别。 |
     | ≥ 180 天 | 橙色 + 警告三角 | 本文发布于 {ago}（{date}），部分内容可能已不再适用，请酌情参考。 |
     | ≥ 365 天 | 红色 + 圆形叹号 | 本文发布于 {ago}（{date}），技术更新较快，内容大概率已过时，请谨慎参考。 |
   - 位置：`themes/.../layout/_partial/post-detail.ejs`；样式：`themes/.../source/css/my.css` 的 `.post-outdated-notice`
   - 「{ago}」由浏览器按当前时间实时计算（N 天前 / N 个月前 / N 年前），静态站点久不重建也不会算错
   - 主题 `_config.yml` 的 `postNotice.levels` 可自由增删分级：`days` 天数门槛、`icon` 图标、`text` 文案（支持 `{ago}` `{date}` `{days}` 占位符）
5. **样式全部对齐 https://jwt1399.top/ **
   - 参考站同样是 Matery，其定制集中在 `matery.css` 与 `my.css`。本项目把两边 `matery.css` 做**结构化逐条比对**，只把「取值不同」的声明提取成 `themes/.../source/css/reference-skin.css`（47 条规则），在 `matery.css` 之后、`my.css` 之前加载，从而不改模板结构即可完全套用其外观
   - 皮肤里包含（均取自参考站）：
     | 选择器 | 取值 |
     | :--- | :--- |
     | `.bg-color` | `linear-gradient(to right, #4a85e9, #EC6EAD)` + `opacity:.9`（导航/页脚/标签/按钮） |
     | `blockquote` | 左边框 `#de5448` |
     | `code` / `pre` | `#de5448` 行内代码色、`#272822` 代码块底 |
     | `.text-color` / `.progress-bar` | `#ed4264` / `#BBFFFF→#4b8af4` |
     | `.container`（≥993px） | `85%` 宽 |
     | 滚动条 | 蓝色渐变拇指 + 灰色轨道 |
     | 表格 `th/td` | `1px solid #dfe2e5`、`12px 13px` 内边距 |
   - **banner / 封面不套参考站皮肤**：首页大图、关于页、404、文章页封面统一使用 Matery 默认样式（与 <https://memme.cn/> 一致）——背景图 + `rainbow` 紫色渐变遮罩动画、标题 4rem、胶囊按钮、社交图标、轮播指示器
   - **全局字体＝参考站方案**（`reference-skin.css` 末尾的 `--site-font` / `--site-code-font`）：
     - 正文/标题/导航/卡片/按钮/表单/页脚/TOC 全部 `Verdana`，中文由系统字体（微软雅黑 / 苹方）承接
     - 代码块与行内代码用参考站的等宽栈：`Inconsolata → Monaco → Consolas → Courier New`
     - 参考站自托管了 `Verdana.ttf` / `Monaco.ttf`（其 `my.css` 的 `@font-face JVer/JMon`）；本项目默认用系统字体，若要跨平台像素级一致，把两个 ttf 放进 `source/medias/fonts/` 并启用 skin 里注释的 `@font-face`，再把 `'JVer'`/`'JMon'` 加到字体栈首位（这两个字体为商业字体，需自行授权）
     - 第三方「分享」组件（`share.min.css`）自身用 `socialshare` 图标字体，且有中文回退，不做覆盖以免图标变方块
   - `my.css` 只剩两件事：保证代码块等宽字体、文章时效性提示样式
   - 卡片圆角/阴影、按钮胶囊、chip 圆角、页面底色 `#eaeaea` 等本来就在主题 `matery.css` 中，与参考站同源，无需再覆盖
   - 想调色：直接改 `reference-skin.css` 里对应声明即可（与参考站一一对应）
6. **t00ls 只保留在友情链接**
   - 已从顶部导航、首页 banner 按钮、社交图标、底栏、关于页、留言板移除 t00ls 入口
   - 唯一入口：友链页卡片（`source/_data/friends.json` 中的 `t00ls` 项），以及友链页 `/friends/`

   | 位置 | 是否保留 | 文件 |
   | :--- | :--- | :--- |
   | 顶部导航 | 已移除 | 主题 `_config.yml` → `menu` |
   | 首页 banner 按钮 | 已移除（改为 GitHub） | 主题 `_config.yml` → `indexbtn` |
   | 社交图标 | 已移除 | `social-link.ejs` |
   | 底栏 | 已移除 | `footer.ejs` |
   | 关于页 / 留言板 / 我的项目 | 已移除 | `source/about`、`source/contact`、`myProjects` |
   | **友情链接** | **保留** | `source/_data/friends.json` |

7. **清理**
   - 移除了 landscape 主题与 `_config.landscape.yml`
   - 关闭了演示性质的音乐、视频、打赏、樱花/飘雪/彩带等特效
   - 评论系统默认全部关闭（`gitalk` / `valine` / `waline` / `twikoo` 等仅保留配置位）
   - 修复 `contact.ejs` 中指向不存在资源的引用，修复 404 页硬编码首页链接
   - 修复 `reprint-statement.ejs` 在子目录部署下文章链接重复的问题

## 六、常用命令

```bash
pnpm install                  # 安装依赖
pnpm run clean                # 清理缓存与 public
pnpm run build                # 生成静态站点
pnpm run server               # 本地预览 http://localhost:4000/hexo/
pnpm run deploy               # 部署（需先在 _config.yml 配置 deploy）
```

新建文章：`pnpm run new "文章标题"`，会自动套用 `scaffolds/post.md` 中带 `author: Mason` 的模板。

## 七、部署到 GitHub Pages

1. 安装部署插件：

   ```bash
   pnpm add hexo-deployer-git
   ```

2. 在 `_config.yml` 中打开部署配置：

   ```yaml
   deploy:
     type: git
     repo: https://github.com/MasonHello/hexo.git
     branch: gh-pages
   ```

3. 执行 `pnpm run pub`（清理 + 生成 + 部署）。
4. 在仓库 Settings → Pages 中选择 `gh-pages` 分支作为站点源。

## 八、开启评论（可选）

本站为纯净版，评论默认关闭。以 Waline 为例：

```yaml
waline:
  enable: true
  serverURL: https://your-waline-server.vercel.app
  avatar: 'mp'
```

开启后留言板与文章页会自动出现评论区。

## 九、版权

本站所有原创内容（文章、图片、代码示例）著作权均归 **Mason** 所有。
欢迎转载，请保留作者署名与原文链接，禁止商业使用。

站点与主题署名：`Powered by Hexo | Theme by Mason` → <https://github.com/MasonHello>
（主题基于开源项目 hexo-theme-matery 二次定制。）
