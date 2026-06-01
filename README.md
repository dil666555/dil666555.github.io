# dre 的个人博客 - 源码项目

本分支 (`source`) 包含了博客的 Hexo 源码与原始 Markdown 文档，你可以通过它轻松维护博客、撰写和发布新文章。而生成的静态 HTML 网页会自动编译并发布到 `master` 分支（即 GitHub Pages）。

---

## 🛠️ 本地开发与维护指南

### 1. 安装与准备工作
在开始之前，请确保本地已安装 [Node.js](https://nodejs.org/) (推荐 LTS 版本)。
然后在项目根目录下运行以下命令安装所需依赖：
```bash
npm install
```

### 2. 编写新文章
使用 Hexo CLI 新建一篇文章：
```bash
npx hexo new post "我的新文章"
```
这会在 `source/_posts/` 目录下生成 `我的新文章.md`。

用你的 Markdown 编辑器（如 VS Code、Typora）打开该文件，你可以看到以下元数据头（Front Matter）：
```markdown
---
title: 我的新文章
date: 2026-06-01 13:30:00
tags:
  - 学习笔记
categories:
  - 技术分享
---
这里写你的文章正文内容...
```

### 3. 本地预览
启动本地 Hexo 开发服务器：
```bash
npm run server
```
启动后在浏览器中打开 [http://localhost:4000](http://localhost:4000) 即可进行实时预览。修改 Markdown 文件后，页面会自动刷新预览效果。

### 4. 编译与发布
当你写完文章或修改了站点配置，可以通过以下一键脚本将网站静态文件构建并部署到 GitHub 上的 `master` 分支：
```bash
npm run deploy
```
*提示：该命令会自动依次运行 `hexo clean`（清理缓存）、`hexo generate`（生成静态文件）以及 `hexo deploy`（部署到 master 分支）。*

---

## ⚙️ 常见配置修改

### 1. 修改站点基本信息
打开根目录下的 `_config.yml` 文件：
* 站点名称：修改 `title`
* 作者名字：修改 `author`
* 网站描述：修改 `description`

### 2. 修改主题样式与社交链接
打开根目录下的 `_config.butterfly.yml` 文件：
* **头像**：更换 `source/images/profile.jpg` 并修改 `avatar.img`
* **Favicon（站点小图标）**：更换 `source/img/favicon.ico` 并修改 `favicon`
* **社交链接**：在 `social` 项下添加或修改你的 GitHub/邮箱等链接

---

## 📁 目录结构说明

```
blog-source/
  ├── _config.yml             # Hexo 核心配置文件
  ├── _config.butterfly.yml   # Butterfly 主题配置文件
  ├── package.json            # 项目依赖和脚本
  ├── source/                 # 源文件存放目录
  │   ├── _posts/             # 所有 Markdown 格式的文章原文 (*.md)
  │   ├── images/             # 存放个人头像 (profile.jpg)
  │   └── img/                # 存放小图标 (favicon.ico)
  └── README.md               # 项目使用说明
```
