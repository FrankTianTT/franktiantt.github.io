# Frank Tian's Technical Blog

基于 Hugo + PaperMod 主题的双语技术博客。

## 项目概览

```
franktiantt.github.io/
├── hugo.yaml              # 全局配置（修改站点设置、语言、菜单等）
├── CNAME                  # 自定义域名 franktian.top
├── content/               # 所有页面和文章
│   ├── _index.en.md       #   英文首页
│   ├── _index.zh.md       #   中文首页
│   ├── about.en.md        #   英文「关于我」
│   ├── about.zh.md        #   中文「关于我」
│   ├── search.en.md       #   英文搜索页
│   ├── search.zh.md       #   中文搜索页
│   ├── archives.en.md     #   英文归档页
│   ├── archives.zh.md     #   中文归档页
│   └── posts/             #   所有文章（每篇一个文件夹）
│       └── 2024-06-14-bellman-equation/
│           ├── index.en.md    #   英文版
│           ├── index.zh.md    #   中文版
│           └── images/        #   配图
├── assets/
│   └── css/extended/
│       └── math.css       # 数学公式样式
├── static/img/            # 静态资源（头像、配图等）
├── layouts/               # 自定义模板
│   ├── _default/_markup/
│   │   └── render-passthrough.html  # 数学公式服务端渲染
│   └── partials/
│       └── comments.html  # Giscus 双语评论区
├── themes/PaperMod/       # Hugo 主题（git submodule）
└── .github/workflows/
    └── hugo-deploy.yml    # 自动部署配置
```

## 快速开始

### 本地运行

```bash
# 安装 Hugo（仅首次）
brew install hugo

# 克隆项目（含主题子模块）
git clone --recurse-submodules git@github.com:franktiantt/franktiantt.github.io.git
cd franktiantt.github.io

# 启动开发服务器
hugo server --bind 0.0.0.0 --port 1313
```

浏览器打开 http://localhost:1313/ 即可预览。修改文件后自动刷新。

## 如何修改站点信息

### 修改头像

替换 `static/img/avatar.svg`（或放入 `avatar.jpg` / `avatar.png`）。

如果使用其他文件名或格式，同步修改 `hugo.yaml` 中的 `imageUrl` 字段（共 2 处，分别在 `languages.en.params.profileMode` 和 `languages.zh.params.profileMode`）。

### 修改自我介绍

编辑 `content/about.en.md`（英文版）和 `content/about.zh.md`（中文版）。

- `title` 字段控制页面标题
- `showToc: false` 表示不显示目录
- 正文使用标准 Markdown

### 修改站点标题和副标题

编辑 `hugo.yaml`，在 `languages.en` 和 `languages.zh` 下分别修改：

```yaml
languages:
  en:
    title: "Frank Tian's Blog"          # 英文站点标题
    params:
      description: "Technical blog..."   # 英文站点描述（SEO）
      profileMode:
        title: Frank Tian                # 首页大标题
        subtitle: "ML · RL · Robotics"   # 首页副标题
  zh:
    title: "Frank Tian 的技术博客"        # 中文站点标题
    params:
      description: "机器学习、强化学习..."
      profileMode:
        title: Frank Tian
        subtitle: "机器学习 · 强化学习 · 机器人"
```

### 修改社交链接

编辑 `hugo.yaml` 的 `params.socialIcons`：

```yaml
params:
  socialIcons:
    - name: github
      url: "https://github.com/franktiantt"
    - name: email
      url: "mailto:franktian424@qq.com"
    # 更多图标见 PaperMod 文档
```

## 如何写文章

### 文章目录结构

每篇文章是一个独立的文件夹（Hugo Page Bundle），放在 `content/posts/` 下：

```
content/posts/
└── YYYY-MM-DD-slug/
    ├── index.en.md           # 英文文章
    ├── index.zh.md           # 中文文章
    └── images/               # 文章配图
        ├── diagram.png
        └── photo.jpg
```

**关键规则：**
- `index.en.md` 和 `index.zh.md` —— 文件名固定，靠后缀区分语言
- `slug` 在 front matter 中设置，中英文版必须相同（否则语言切换器无法关联）
- 文件夹名称 `YYYY-MM-DD-slug` 中的日期控制发布时间，slug 部分建议和 front matter 中一致
- 图片放 `images/` 下，文章中用相对路径引用：`![描述](images/diagram.png)`

### 文章 Front Matter 模板

英文版 `content/posts/2024-06-14-my-post/index.en.md`：

```yaml
---
title: "Your English Title"
date: 2024-06-14T12:00:00+08:00
slug: my-post
draft: false
categories: ["Reinforcement Learning"]
tags: ["RL", "MDP", "Value Function"]
math: true                       # 启用数学公式
summary: "A brief description."
---
```

中文版 `content/posts/2024-06-14-my-post/index.zh.md`：

```yaml
---
title: "你的中文标题"
date: 2024-06-14T12:00:00+08:00
slug: my-post                      # slug 必须相同
draft: false
categories: ["强化学习"]           # 分类用中文
tags: ["RL", "MDP", "价值函数"]    # 标签按语言翻译
math: true
summary: "简短描述。"
---
```

### 数学公式写法

行内公式：`$E = mc^2$` 或 `$\mathcal{N}(0, 1)$`

块级公式：
```
$$
V^*(s) = \max_{a \in \mathcal{A}} \left[ R(s, a) + \gamma \sum_{s'} P(s'|s, a) V^*(s') \right]
$$
```

公式在构建时由 Hugo 服务端渲染为 MathML，无需浏览器加载 KaTeX JS。

### 分类和标签

- 分类 (`categories`) 和标签 (`tags`) 是**语言感知**的
- 英文页面只显示英文分类/标签，中文页面只显示中文分类/标签
- 技术缩写（如 RL、MDP）可以跨语言保持不变
- 无需提前注册，在文章 front matter 中写新分类/标签即可自动创建
- 建议保持分类数量少（3-5 个大类），标签可以更细粒度

### 草稿

设置 `draft: true` 可以让文章不在生产环境显示。本地开发时加上 `-D` 参数查看草稿：

```bash
hugo server -D
```

## 如何修改导航菜单

编辑 `hugo.yaml` 的 `menu.main`（或各语言下的 `menu.main`）：

```yaml
menu:
  main:
    - name: Posts       # 菜单显示文字
      url: /posts/      # 链接地址
      weight: 10        # 排序（数字越小越靠前）
    - name: About
      url: /about/
      weight: 50
```

如需中英文菜单显示不同文字，在各语言下单独配置。

## 评论系统 (Giscus)

评论使用 Giscus，数据存储在 GitHub Discussions 中。

1. 确保 GitHub 仓库已启用 Discussions（Settings → Features → Discussions）
2. 访问 https://giscus.app 生成配置
3. 将 `repoId` 和 `categoryId` 填入 `hugo.yaml` 的 `params.giscus`

## 部署

### 自动部署

推送到 `main` 分支即可。GitHub Actions 自动构建并部署到 GitHub Pages。

### 手动部署

```bash
hugo --minify                      # 构建到 public/ 目录
# public/ 的内容即部署产物
```

### DNS 配置

1. 在域名管理商处添加 CNAME 记录：`franktian.top` → `franktiantt.github.io`
2. GitHub 仓库 Settings → Pages → Custom domain 填入 `franktian.top`
3. 启用 Enforce HTTPS

## 项目原理

### 双语系统

- Hugo 原生多语言模式，默认语言为英文
- 内容文件通过 `.en.md` / `.zh.md` 后缀区分语言
- 相同 `slug` 的文件自动关联为彼此的翻译
- 导航栏自动显示语言切换器（English | 中文）
- 分类、标签、界面文字全部跟随当前语言

### 数学公式渲染

- Hugo 的 Goldmark 解析器遇到 `$...$` 和 `$$...$$` 时，通过 `passthrough` 扩展跳过 Markdown 处理
- `render-passthrough.html` 模板调用 `transform.ToMath` 在构建时渲染为 MathML
- 无需浏览器加载 KaTeX JS，页面加载更快，不会出现公式闪烁

### PaperMod 主题自定义

- `layouts/partials/extend_head.html`：插入到 `<head>` 的额外内容（PaperMod 钩子）
- `layouts/partials/comments.html`：覆盖默认评论区
- `assets/css/extended/*.css`：自动合并到主题样式（PaperMod 钩子）
- `static/` 下的文件直接复制到站点根目录

## 常用命令

```bash
hugo server                        # 启动开发服务器
hugo server -D                     # 包含草稿
hugo                               # 构建站点
hugo --minify                      # 构建并压缩（生产环境）
hugo new posts/my-post.en.md       # 创建新文章骨架
```
