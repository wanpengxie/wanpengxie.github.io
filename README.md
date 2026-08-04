# atoll-site

Atoll 的 GitHub Pages 站点（Jekyll，GitHub Pages 原生构建，无需任何 CI 配置：
push markdown 即自动发布）。独立 git 仓，被 coagent 主仓 .gitignore。

## 结构

```
_config.yml      站点配置（标题、主题 minima、baseurl、免 front matter 插件）
index.md         首页（系列目录，只列已放出的篇目）
architecture/    Atoll Architecture Series（当前放出 01/02，全系列共 8 篇）
```

`architecture/` 下的文件与 `../docs/architecture/`（atoll-docs 仓）**逐字节相同**，
不带 front matter——靠 GitHub Pages 默认启用的
optional-front-matter / titles-from-headings / relative-links 三插件直接成页。
放出后续篇目 = 从 docs 仓 `cp` 对应文件过来 + 在 `index.md` 目录里加一行；
docs 仓修订已放出篇目后重新 `cp` 覆盖即同步，不需要改写任何内容。

新增独立文章：直接放带 front matter 的 markdown 页面，并在 `index.md` 挂链接。

## 发布

个人主页形：仓库名 `wanpengxie.github.io`，站点即 <https://wanpengxie.github.io>，
`_config.yml` 的 `baseurl` 保持 `""`。GitHub 对该名字的仓库自动启用 Pages，
push main 后约 1 分钟生效。

```bash
gh repo create wanpengxie.github.io --public --source . --push
```

## 本地预览（可选）

```bash
gem install github-pages
jekyll serve   # http://127.0.0.1:4000
```

不装 ruby 也没关系——GitHub Pages 云端构建，push 即所见。

## 内容来源

`architecture/` 系列来自 atoll-docs 仓（`coagent/docs/architecture/`，
2026-07-14 讨论整理稿）。事实变化时先更新 docs 仓，再同步到这里。
