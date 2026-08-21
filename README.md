# XiX1ong.github.io

使用 Hugo 和 Stack v4 构建的个人博客，部署到 GitHub Pages。

## macOS 首次安装

安装 Homebrew 后，在终端运行：

```bash
brew install go hugo
```

确认版本：

```bash
go version
hugo version
```

Stack v4 要求 Hugo 0.157.0 或更高。本项目的 GitHub Actions 固定使用 Hugo 0.165.0、Go 1.26.6 和 Stack v4.0.3。

## 本地预览

```bash
hugo mod download
hugo server --buildDrafts
```

访问 <http://localhost:1313/>。停止预览时按 `Control-C`。

## 新建文章

每篇文章使用一个 Page Bundle，让 Markdown 与照片保存在同一目录：

```bash
hugo new content post/2026-trip-name/index.md
```

目录结构示例：

```text
content/post/2026-trip-name/
├── index.md
├── cover.jpg
├── photo-01.jpg
└── photo-02.jpg
```

在 `index.md` 中使用相对路径：

```markdown
![洱海边的傍晚](photo-01.jpg "大理，2026 年 5 月")
```

Hugo 会把正文图片转换为 WebP，并生成 480、800、1200、1600、2400 像素的响应式版本。发布前请将 front matter 中的 `draft = true` 改为 `draft = false`。

## 构建与发布

本地生产构建：

```bash
hugo --gc --minify
```

`public/` 是构建产物，已被 Git 忽略，不要手工提交。推送 `main` 后，GitHub Actions 会自动构建并部署到 GitHub Pages。

首次推送源码后，需要在 GitHub 仓库中打开 **Settings → Pages**，将 **Source** 设置为 **GitHub Actions**。

## 更新依赖

更新本地 Hugo 与 Go：

```bash
brew update
brew upgrade hugo go
```

更新 Stack v4 时先查看其发布说明，然后运行：

```bash
hugo mod get github.com/CaiJimmy/hugo-theme-stack/v4@latest
hugo mod tidy
hugo --gc --minify
```

确认本地构建无误后再提交 `go.mod` 和 `go.sum`。
