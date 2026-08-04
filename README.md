# wanpengxie.github.io — Atoll blog

介绍 Atoll 的 blog 站（Jekyll + minima，GitHub Pages 原生构建，无 CI 配置：
push main 即自动发布到 <https://wanpengxie.github.io>，约 1 分钟生效）。
独立 git 仓，被 coagent 主仓 .gitignore。

## 结构

```
_config.yml   站点配置
index.md      首页引言（文章列表由 home layout 自动生成）
_posts/       blog 文章，文件名 YYYY-MM-DD-slug.md
```

发一篇新文章：

```markdown
---
layout: post
title: "文章标题"
date: 2026-08-05 10:00:00 +0800
---

正文……
```

首页按日期倒序自动列出，无需手工维护目录。

## 内容来源

首两篇来自 atoll-docs 仓的 Atoll Architecture Series
（`coagent/docs/architecture/01、02`，全系列 8 篇），转为 post 时仅加
front matter、去掉首行 H1（标题由 front matter 承担），正文未改。
放出后续篇目同样从 docs 仓拷入 `_posts/` 加 front matter 即可。
事实变化时先更新 docs 仓，再同步到这里。

## 本地预览（可选）

```bash
gem install github-pages
jekyll serve   # http://127.0.0.1:4000
```
