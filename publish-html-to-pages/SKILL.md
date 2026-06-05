---
name: publish-html-to-pages
description: |
  将本地 HTML 文件发布为 GitHub Pages 网页。当用户说"把这个 HTML 发布到网页"、"渲染为网页"、
  "放到 GitHub Pages 上"、"在线看这个页面"、"publish html to pages"时触发。
  也适用于用户想把本地 HTML 变成可在线访问的链接。
---

# Publish HTML to GitHub Pages

将本地文件夹内的指定 HTML 文件重命名为 `index.html`，自动推送到 GitHub，通过 Pages 渲染为网页。

## 工作流程

### 步骤 1：确认文件位置

用户需要提供要发布的 HTML 文件路径。确认该文件所在目录是一个 git 仓库，并且已关联 GitHub 远程。

### 步骤 2：重命名为 index.html

将 HTML 文件重命名为 `index.html`（保留在同一目录下）：

```bash
mv "原始文件名.html" "所在目录/index.html"
```

如果该目录已存在 `index.html`，先询问用户是否覆盖。

### 步骤 3：提交并推送

```bash
cd "<repo-root>"
git add "<path-to-index.html>"
git commit -m "Publish <folder-name>/index.html for GitHub Pages"
git push
```

### 步骤 4：输出访问链接

推送成功后，根据远程仓库地址推断 GitHub Pages 链接：

| 远程 | Pages 根域名 |
|------|-------------|
| `git@github.com:<user>/<repo>.git` | `https://<user>.github.io/<repo>/` |

目标 URL = `https://<user>.github.io/<repo>/<子目录>/`

### 前提条件

- 仓库已开启 GitHub Pages（Settings → Pages → 选择分支保存）
- 该仓库只需开启一次，之后所有子目录的 `index.html` 都能自动渲染

### URL 规则

| 本地路径 | 网页地址 |
|---------|---------|
| `repo/paper/index.html` | `https://<user>.github.io/<repo>/paper/` |
| `repo/index.html` | `https://<user>.github.io/<repo>/` |
