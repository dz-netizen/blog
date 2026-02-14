---
title: github.io blog 使用说明
date: 2026-02-11 20:52:17
tags:
	- Hexo
	- GitHub Pages
categories:
	- 博客
---

这篇文章记录：用 **Hexo** 在本地写作与预览，并部署到 **GitHub Pages（username.github.io）** 的完整步骤。

## 你需要先了解的几个东西

### Node.js / npm / npx 是什么

- **Node.js**：运行 JavaScript 的运行时环境。Hexo 是基于 Node.js 的。
- **npm**：Node.js 的包管理器（安装依赖、运行脚本）。安装 Node.js 时通常会自带 npm。
- **npx**：npm 自带的命令执行器，用来“临时/就地运行”某个包的命令。你看到的 `npx hexo ...` 就是用 npx 去运行 Hexo。

### Hexo 是什么

**Hexo** 是一个静态博客生成器：

- 你写的是 Markdown（在 `source/_posts/`）
- Hexo 把 Markdown 渲染成静态 HTML/CSS/JS（生成到 `public/`）
- 再把 `public/` 的内容部署到 GitHub Pages 仓库

## 本地环境准备（只需要做一次）

### 1）安装 Node.js

到 Node.js 官网下载 LTS 版本并安装。安装后打开 PowerShell，确认版本：

```bash
node -v
npm -v
```

能输出版本号就说明安装成功。

### 2）安装项目依赖

在你的博客项目根目录（包含 `package.json` 的目录）执行：

```bash
npm install
```

这会把 Hexo 以及部署插件等依赖安装到 `node_modules/`。

## 写你的第一篇文章

### 1）新建文章

在项目根目录执行：

```bash
npx hexo new post "标题随便取"
```

比如：

```bash
npx hexo new post "github.io blog 使用说明"
```

它会在 `source/_posts/` 下生成一个同名的 `.md` 文件。

### 2）编辑文章（Front-matter + Markdown 正文）

文章顶部的 `---` 包住的部分叫 **Front-matter**，常用字段：

```yaml
---
title: 文章标题
date: 2026-02-11 21:00:00
tags:
	- 标签1
	- 标签2
categories:
	- 分类
---
```

常用字段在_config.yml文件中设置。

正文就是正常 Markdown。例子：

```md
## 小节标题

这里写内容。

- 列表项 1
- 列表项 2

插入链接：https://hexo.io/
```

## 本地预览（写作时最常用）

在项目根目录执行：

```bash
npx hexo server
```

它会启动本地服务，默认地址通常是：

- http://localhost:4000

你改完文章保存后，刷新浏览器就能看到变化。

停止预览：在终端按 `Ctrl + C`。

## 生成静态文件（public）

当你准备发布时，先清理再生成：

```bash
npx hexo clean
npx hexo generate
```

说明：

- `hexo clean`：清理缓存与旧的生成产物（会影响 `public/`）
- `hexo generate`：把 `source/` 渲染成静态站点到 `public/`

重要提醒：**不要直接修改 `public/` 里的文件来“长期改样式/图片”**，因为下次 `generate` 会重新生成并覆盖。应修改主题源文件（通常在 `themes/`）。

## 部署到 GitHub Pages

### 1）确认部署配置（只需检查一次）

打开根目录的 `_config.yml`，确保 `deploy` 配置类似下面这样：

```yaml
deploy:
	type: git
	repo: https://github.com/<你的用户名>/<你的用户名>.github.io.git
	branch: master
```

如果你用的是 `main` 分支，把 `branch` 改成 `main`。

另外建议把 `url` 改成你真实站点地址，例如：

```yaml
url: https://<你的用户名>.github.io
```

### 2）一键部署

发布时执行：

```bash
npx hexo deploy
```

常用发布流程（推荐顺序）：

```bash
npx hexo clean
npx hexo generate
npx hexo deploy
```

第一次部署如果提示权限问题，一般是 Git 认证：

- 用 HTTPS 仓库：需要配置 GitHub Token
- 或改用 SSH 仓库地址（前提是你已配置 SSH key）

## 常见问题

### 1）我替换了图片，但 clean/generate 后又变回去了

原因通常是：你替换的是 `public/` 里的生成产物；Hexo 每次生成都会覆盖。

正确做法：到主题的源文件里替换（例如 banner 在主题的 `source/css/images/banner.jpg` 这类位置）。

### 2）命令太长，能不能用 npm scripts

项目根目录的 `package.json` 里一般会有脚本，你可以直接：

```bash
npm run server
npm run clean
npm run build
npm run deploy
```

（不同项目脚本名可能不一样，以 `package.json` 为准。）

## 最小工作流（记住这三条就够）

```bash
# 写作预览
npx hexo server

# 发布
npx hexo clean
npx hexo g
npx hexo d
```
