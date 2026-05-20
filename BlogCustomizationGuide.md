# Hexo + Fluid 博客完全自定义指南

> 适用于当前项目 `LIYBlog`（Hexo 8.1.2 + Fluid 1.9.9），所有路径和配置项均基于项目实际结构。

---

## 目录

1. [基础站点信息配置](#1-基础站点信息配置)
2. [静态资源的管理与使用](#2-静态资源的管理与使用)
3. [博文写作与内容管理](#3-博文写作与内容管理)
4. [利用 AI 助手进行深度自定义](#4-利用-ai-助手进行深度自定义)

---

## 1. 基础站点信息配置

你的博客有两个核心配置文件：

| 文件 | 路径 | 作用 |
|------|------|------|
| Hexo 主配置 | `e:\LIYBlog\_config.yml` | 网站全局设定、插件、部署 |
| Fluid 主题配置 | `e:\LIYBlog\_config.fluid.yml` | 主题外观、布局、功能开关 |

> **重要**：`_config.fluid.yml` 是你的项目根目录下的那个文件（不是 `node_modules` 里面的），它会覆盖主题自带的默认配置。

### 1.1 修改网站标题、副标题、作者名

打开 **Hexo 主配置文件** `_config.yml`（位于项目根目录），找到 `# Site` 部分：

```yaml
# Site
title: Hexo                    # ← 改成你的博客名，如 "LIY 的技术笔记"
subtitle: ''                   # ← 副标题，如 "乐于分享，勤于记录"
description: ''                # ← 网站简介/SEO描述，如 "一个关于前端与生活的小站"
keywords:                      # ← 网站关键词，如 "博客,前端,JavaScript"
author: John Doe               # ← 改成你的名字，如 "LIY"
language: zh-CN                # ← 语言保持 zh-CN 即可
timezone: ''                   # ← 时区，如 "Asia/Shanghai"
```

修改示例：

```yaml
title: LIY 的技术笔记
subtitle: '终身学习，持续进步'
description: '一个专注前端开发与效率工具的个人博客'
keywords: 前端, JavaScript, React, 博客
author: LIY
language: zh-CN
timezone: 'Asia/Shanghai'
```

### 1.2 导航栏标题与菜单

打开 **Fluid 主题配置文件** `_config.fluid.yml`，找到 `navbar` 部分：

```yaml
navbar:
  blog_title: "Fluid"          # ← 改成你的博客名（为空则自动使用 _config.yml 的 title）
  menu:
    - { key: "home", link: "/", icon: "iconfont icon-home-fill" }
    - { key: "archive", link: "/archives/", icon: "iconfont icon-archive-fill" }
    - { key: "category", link: "/categories/", icon: "iconfont icon-category-fill" }
    - { key: "tag", link: "/tags/", icon: "iconfont icon-tags-fill" }
    - { key: "about", link: "/about/", icon: "iconfont icon-user-fill" }
    #- { key: "links", link: "/links/", icon: "iconfont icon-link-fill" }  # 去掉 # 即可开启友链页
```

`key` 对应的中文文本来自主题的语言文件 `node_modules/hexo-theme-fluid/languages/zh-CN.yml`。你也可以通过添加 `name` 字段强制指定显示名：

```yaml
    - { key: "home", link: "/", icon: "iconfont icon-home-fill", name: "首页" }
```

### 1.3 网站 URL 与部署配置

仍在 `_config.yml` 中：

```yaml
# URL
url: https://github.com/LIYRock/LIYRock.github.io   # ← 改成你的实际域名
root: /
permalink: :year/:month/:day/:title/                 # 文章链接格式，可不动

# Deployment
deploy:
  type: ''   # 如果你需要从本地部署，可配置为 git 等类型
```

当前项目使用 GitHub Actions 自动部署（见 `.github/workflows/deploy.yml`），因此 `deploy` 留空即可。

### 1.4 关于页个人信息

关于页的信息在 `_config.fluid.yml` 的 `about:` 部分：

```yaml
about:
  enable: true
  banner_img: /img/default.png      # ← 改成你自己的头图
  banner_img_height: 60
  banner_mask_alpha: 0.3
  avatar: /img/avatar.png           # ← 改成你的头像路径
  name: "Fluid"                     # ← 改成你的名字
  intro: "An elegant theme for Hexo"  # ← 改成你的简介
  icons:                            # ← 社交图标链接
    - { class: "iconfont icon-github-fill", link: "https://github.com", tip: "GitHub" }
    - { class: "iconfont icon-douban-fill", link: "https://douban.com", tip: "豆瓣" }
    - { class: "iconfont icon-wechat-fill", qrcode: "/img/favicon.png" }
```

关于页的正文内容在 `source/about/index.md` 中，支持 Markdown 和 HTML。

---

## 2. 静态资源的管理与使用

### 2.1 资源存放位置一览

| 资源类型 | 存放路径 | 说明 |
|----------|----------|------|
| 图片 | `e:\LIYBlog\source\img\` | 头像、Logo、Banner 图等 |
| 随机头图 | `e:\LIYBlog\source\img\random\` | 用于 Banner 随机切换 |
| 自定义 CSS | `e:\LIYBlog\source\css\` | 覆盖主题样式的 CSS 文件 |
| 自定义 JS | `e:\LIYBlog\source\js\` | 自定义脚本文件 |

> **重要**：如果 `source/img/` 目录不存在，你需要手动创建（在资源管理器中右键新建文件夹即可）。

### 2.2 更换头像

**步骤一**：将你的头像图片放到 `source/img/` 目录下，例如命名为 `my-avatar.png`。

**步骤二**：在 `_config.fluid.yml` 中修改 `about.avatar` 字段：

```yaml
about:
  avatar: /img/my-avatar.png
```

### 2.3 更换网站 Logo / Favicon

在 `_config.fluid.yml` 的全局区域：

```yaml
favicon: /img/my-favicon.png          # 浏览器标签页图标
apple_touch_icon: /img/my-favicon.png  # Apple 设备图标
```

将你的图标文件放到 `source/img/`，然后修改上述路径即可。

### 2.4 更换首页大图（Banner）

Fluid 为每个页面都提供了独立的 Banner 图设置，在 `_config.fluid.yml` 中：

```yaml
index:
  banner_img: /img/default.png           # ← 首页头图，改成你的图片路径
  banner_img_height: 100                  # 头图高度占比（0-100）
  banner_mask_alpha: 0.3                  # 蒙版不透明度（0-1）

post:
  banner_img: /img/default.png            # ← 文章页头图
  banner_img_height: 70

archive:
  banner_img: /img/default.png            # ← 归档页头图
  banner_img_height: 60

category:
  banner_img: /img/default.png            # ← 分类页头图

tag:
  banner_img: /img/default.png            # ← 标签页头图

about:
  banner_img: /img/default.png            # ← 关于页头图
```

**步骤**：

1. 将你想要的 Banner 图片放到 `source/img/` 目录。
2. 修改对应页面的 `banner_img` 为 `/img/你的图片名.png`。
3. 调整 `banner_img_height` 控制图片显示高度。

**高级玩法：随机头图**

```yaml
banner:
  random_img: true   # 设为 true
  parallax: true     # 视差滚动效果
```

然后将多张图片放到 `source/img/random/` 目录下，Banner 图片会在这些图片中随机选取。

### 2.5 启用自定义 CSS 文件

在 `_config.fluid.yml` 中找到：

```yaml
custom_css:
```

假设你要添加自己的样式文件：

**步骤一**：在 `source/css/` 目录下创建 CSS 文件，例如 `my-style.css`。

**步骤二**：在配置中引用它：

```yaml
custom_css: /css/my-style.css
```

你也可以引用多个 CSS 文件（列表格式）：

```yaml
custom_css:
  - /css/my-style.css
  - /css/dark-mode-tweak.css
```

### 2.6 启用自定义 JS 文件

同理，在 `_config.fluid.yml` 中：

```yaml
custom_js:
```

**步骤一**：在 `source/js/` 目录下创建 JS 文件，例如 `my-script.js`。

**步骤二**：在配置中引用它：

```yaml
custom_js: /js/my-script.js
```

也可以列表形式引用多个：

```yaml
custom_js:
  - /js/scroll-animation.js
  - /js/click-effect.js
```

---

## 3. 博文写作与内容管理

### 3.1 文章存放与创建

所有 Markdown 文章存放在：

```
e:\LIYBlog\source\_posts\
```

**创建新文章**：在终端中进入项目目录，执行：

```bash
hexo new "我的第一篇文章"
```

这会在 `source/_posts/我的第一篇文章.md` 创建一个新文件。

### 3.2 Front-matter 完整示例

打开刚创建的文章，你会在文件顶部看到 `---` 包裹的区域，这就是 Front-matter。以下是一个完整的示例：

```yaml
---
title: 我的第一篇文章                    # 文章标题（必填）
date: 2026-05-20 10:00:00               # 发布日期
updated: 2026-05-21 15:30:00            # 更新日期（可选）
author: LIY                             # 作者名（可选，覆盖全局 author）
categories:
  - 前端                                # 主分类
  - JavaScript                          # 子分类
tags:
  - JavaScript                          # 标签 1
  - React                               # 标签 2
  - 教程                                # 标签 3
excerpt: 这是一篇关于 React 组件的入门教程   # 自定义摘要（可选）
sticky: 100                             # 数值越大越靠前（置顶）
index_img: /img/post-cover.jpg          # 文章封面图（可选）
lazyload: true                          # 是否启用懒加载（可选）
comments: true                          # 是否开启评论（可选）
toc: true                               # 是否显示目录（可选）
math: false                             # 是否启用数学公式（可选）
mermaid: false                          # 是否启用流程图（可选）
---
```

**字段说明**：

| 字段 | 必填 | 说明 |
|------|------|------|
| `title` | 是 | 文章标题 |
| `date` | 否 | 发布日期，不写则用文件创建日期 |
| `updated` | 否 | 更新日期，不写则用文件修改日期 |
| `categories` | 否 | 分类，支持层级（如 `[前端, JavaScript]` 表示"前端 > JavaScript"） |
| `tags` | 否 | 标签，可以写多个 |
| `excerpt` | 否 | 手动设置摘要，不写则自动截取 |
| `sticky` | 否 | 置顶，数值越大越靠前 |
| `index_img` | 否 | 文章在首页显示的封面图 |
| `comments` | 否 | 单独控制本篇文章是否显示评论 |
| `toc` | 否 | 单独控制本篇文章是否显示目录 |

### 3.3 创建分类页和标签页

Fluid 主题默认**已自动启用**分类页和标签页，你不需要手动创建页面！只需确认 `_config.fluid.yml` 中配置正确：

```yaml
category:
  enable: true      # 分类页开关，已默认开启

tag:
  enable: true      # 标签页开关，已默认开启
```

只要你在文章 Front-matter 中填写了 `categories` 和 `tags`，这些页面就会自动展示所有分类和标签的聚合。

如果你的导航栏中没有显示分类和标签入口，检查 `_config.fluid.yml` 中 `navbar.menu` 的这两行没有被注释：

```yaml
navbar:
  menu:
    - { key: "category", link: "/categories/", icon: "iconfont icon-category-fill" }
    - { key: "tag", link: "/tags/", icon: "iconfont icon-tags-fill" }
```

### 3.4 草稿（Draft）的用法

当你写了一部分但不想发布时，可以使用草稿功能：

**创建草稿**：

```bash
hexo new draft "一篇还没写完的文章"
```

这会在 `source/_drafts/` 目录下创建文件，该目录的文件**不会**被生成到最终站点。

**本地预览草稿**（调试用）：

```bash
hexo server --draft
```

**发布草稿**（将草稿转为正式文章）：

```bash
hexo publish "一篇还没写完的文章"
```

发布后文件会从 `source/_drafts/` 移动到 `source/_posts/`。

### 3.5 修改默认文章模板

如果你想新建文章时自动带上某些 Front-matter 字段，可以修改脚手架文件：

- `scaffolds/post.md` — 文章的默认模板
- `scaffolds/draft.md` — 草稿的默认模板
- `scaffolds/page.md` — 页面的默认模板

例如，修改 `scaffolds/post.md`：

```markdown
---
title: {{ title }}
date: {{ date }}
categories:
tags:
---
```

这样每次 `hexo new` 时，这些字段就会自动出现在新文件中。

---

## 4. 利用 AI 助手进行深度自定义

我是你的专属 AI 编程助手，可以帮你实现几乎所有你能想到的博客定制需求。以下是一些实例，供你参考你可以让我做什么。

### 4.1 自定义 CSS 样式

Fluid 提供了丰富的颜色变量配置（在 `_config.fluid.yml` 的 `color:` 部分），但很多细节需要通过 CSS 来覆盖。

**常见需求示例**：

> **"帮我让所有卡片的圆角更大一些"**

我会分析 Fluid 主题的 CSS 结构（位于 `node_modules/hexo-theme-fluid/source/css/`），然后为你生成一段自定义 CSS：

```css
/* 增大卡片圆角 */
.card,
#board .card {
  border-radius: 16px;
}

/* 修改正文字体 */
.markdown-body {
  font-family: "LXGW WenKai", "Noto Serif SC", Georgia, serif;
  font-size: 17px;
}

/* 暗色模式下卡片背景 */
[data-user-color-scheme='dark'] .card {
  background-color: #1e2430;
}
```

你只需要把这个 CSS 保存到 `source/css/my-style.css`，然后在 `_config.fluid.yml` 中引用：

```yaml
custom_css: /css/my-style.css
```

**你可以让我做的事情**：
- 修改字体（引入 Google Fonts 或中文字体）
- 调整卡片阴影、圆角、间距
- 修改暗色模式的配色细节
- 改变导航栏的透明度和样式
- 美化滚动条样式
- 修改代码块的颜色主题

### 4.2 JavaScript 交互功能

Fluid 主题提供了 `custom_js` 配置，你可以注入自己的 JS 脚本来实现各种交互效果。

**示例：让返回顶部按钮更平滑**

> **"帮我写一个更平滑的返回顶部动画"**

我会查看你已经有哪些配置（当前你的 `scroll_top_arrow` 已启用），然后为你写：

```javascript
// 保存到 source/js/smooth-scroll.js
(function() {
  var scrollBtn = document.querySelector('.scroll-top-button');
  if (!scrollBtn) return;

  scrollBtn.addEventListener('click', function(e) {
    e.preventDefault();
    var currentScroll = document.documentElement.scrollTop || document.body.scrollTop;
    if (currentScroll > 0) {
      window.requestAnimationFrame(scrollToTop);
    }
  });

  function scrollToTop() {
    var currentScroll = document.documentElement.scrollTop || document.body.scrollTop;
    if (currentScroll > 0) {
      window.requestAnimationFrame(scrollToTop);
      window.scrollTo(0, currentScroll - currentScroll / 8);
    }
  }
})();
```

然后在 `_config.fluid.yml` 中启用：

```yaml
custom_js: /js/smooth-scroll.js
```

**你可以让我做的事情**：
- 编写返回顶部动画（平滑滚动）
- 实现阅读进度条
- 添加点击特效（爱心、烟花等）
- 实现页面底部"一言"动态文字
- 让评论框更美观
- 编写图片点击放大增强

### 4.3 修改主题模板（EJS）

Fluid 主题使用 EJS 模板引擎，所有布局文件在 `node_modules/hexo-theme-fluid/layout/` 中。如果你需要更底层的修改，我可以通过分析模板文件来帮你。

> **重要提示**：直接修改 `node_modules` 中的文件会在下次 `npm install` 时丢失。推荐使用 Hexo 的 **主题文件覆盖** 机制：
>
> 将你修改的布局文件复制一份到项目根目录下，保持相同的相对路径结构。

**示例：修改页脚版权信息**

> **"帮我改一下页脚的版权文字"**

实际上 Footer 文字可以直接在 `_config.fluid.yml` 中配置：

```yaml
footer:
  content: '
    <a href="https://hexo.io" target="_blank"><span>Hexo</span></a>
    <i class="iconfont icon-love"></i>
    <a href="https://github.com/fluid-dev/hexo-theme-fluid" target="_blank"><span>Fluid</span></a>
  '
```

你可以直接在这里修改 HTML，比如改成：

```yaml
footer:
  content: '<span>&copy; 2026 LIY Blog</span>'
```

**你可以让我做的事情**：
- 修改特定页面的布局结构
- 调整文章页的元素排列顺序
- 在页面特定位置插入自定义 HTML

### 4.4 Hexo 扩展脚本（scripts 目录）

Hexo 支持在项目根目录的 `scripts/` 文件夹中放置 JavaScript 扩展脚本，这些脚本会在 Hexo 启动时自动加载。

**示例：自动为所有外链添加 `target="_blank"`**

> **"帮我写一个 Hexo 脚本，让所有外链在新标签页打开"**

我会为你生成如下脚本（`scripts/external-link.js`）：

```javascript
// 保存到 scripts/external-link.js
hexo.extend.filter.register('after_post_render', function(data) {
  data.content = data.content.replace(
    /<a href="(https?:\/\/[^"]+)"/g,
    '<a href="$1" target="_blank" rel="noopener noreferrer"'
  );
  return data;
});
```

**你可以让我做的事情**：
- 创建自定义的 Hexo 标签插件
- 编写生成器（自动生成 RSS、Sitemap 等）
- 实现文章过滤器（自动处理图片、链接等）
- 集成第三方服务脚本

### 4.5 更多高级定制示例

以下是你可以随时向我提出的请求类型：

1. **"帮我给博客添加一个暗色模式定时切换功能（比如晚上 6 点自动变暗）"**
2. **"我想在所有文章底部自动添加一个'阅读更多'的推荐区块"**
3. **"帮我写一个打字机效果的页脚签名"**
4. **"我想让代码块在暗色模式下也有好看的配色"**
5. **"帮我调整手机端的排版间距"**
6. **"我想给博客添加一个回到顶部的悬浮按钮，并带进度环"**
7. **"帮我在首页文章列表的每篇文章加上阅读量统计"**
8. **"我想把代码块的复制按钮改成中文提示"**

---

## 附录：常用命令速查

| 命令 | 说明 |
|------|------|
| `hexo server` | 启动本地预览服务器（默认 `http://localhost:4000`） |
| `hexo server --draft` | 本地预览（含草稿） |
| `hexo new "文章名"` | 创建新文章 |
| `hexo new draft "草稿名"` | 创建草稿 |
| `hexo publish "草稿名"` | 发布草稿 |
| `hexo clean` | 清除缓存和已生成文件 |
| `hexo generate` | 生成静态文件（到 `public/` 目录） |
| `hexo deploy` | 部署到远程服务器 |

---

**最后的话**

以上是本指南的全部内容。你的博客项目位于 `e:\LIYBlog\`，使用 **Hexo 8.1.2 + Fluid 1.9.9**，通过 GitHub Actions 自动部署。

无论你遇到任何定制难题——无论是调整一个颜色值、编写一段复杂的 JS 交互动效、还是修改主题的底层模板——都欢迎直接向我描述你的需求。我会结合你当前项目的实际配置和文件结构，给你提供**可直接复制粘贴的代码**和**一步步的操作指令**，让你从零基础也能轻松完成高级定制。

现在就试试吧！你可以问我例如："帮我修改首页 Banner 图"、"帮我把文章字体改成思源黑体"、或者任何你想实现的功能。
