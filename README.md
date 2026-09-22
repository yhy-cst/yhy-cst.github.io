# 个人网站

基于 **Hugo** + **Stack 主题** 的静态博客，风格参照 [ovideros.site](https://ovideros.site/)。
卡片式布局、明暗模式、归档、搜索、标签云、友链页都已经配好，开箱即用。

**线上地址：https://yhy-cst.github.io/**
仓库：https://github.com/yhy-cst/yhy-cst.github.io

推送代码到 `main` 分支会自动构建部署（GitHub Actions）。

> ⚠️ 本机直连 github.com 不通，本仓库已配置走本地代理（Clash 等，端口 7890）。
> **推送/拉取前请确保代理软件在运行**，否则会卡住后报连接超时。
> 该代理是仓库级配置，不影响其他项目；如需检查：`git config --get http.proxy`

---

## 一、先跑起来

### 1. 安装 Hugo（extended 版）

必须用 **extended** 版本，主题的 SCSS 需要它。

- **Windows**：`winget install Hugo.Hugo.Extended`，或从 [Releases](https://github.com/gohugoio/hugo/releases) 下载 `hugo_extended_*_windows-amd64.zip` 解压后把 `hugo.exe` 放进 PATH。
- **macOS**：`brew install hugo`
- **Linux**：`sudo apt install hugo`（注意 apt 源的旧版可能不是 extended，建议用 Release 压缩包）

验证：

```bash
hugo version
# 输出里要有 "extended" 字样，例如 hugo v0.166.0+extended
```

> 本机已经装好了，在 `C:\Users\18834\.local\hugo\hugo.exe`。可以直接用这个路径，或者把它加到系统 PATH。

### 2. 本地预览

在站点根目录（就是本文件所在的目录）执行：

```bash
hugo server -D
```

浏览器打开 http://localhost:1313 。`-D` 表示连草稿一起显示。改文件会自动刷新。

### 3. 构建静态文件

```bash
hugo --minify
```

产物在 `public/` 目录，整个目录就是可以部署的网站。

---

## 二、改成你自己的内容

按重要性排序，改完这几处基本就是你的站了。

| 要改的东西 | 文件 |
| --- | --- |
| 域名、站点标题、分页数量 | `config/_default/hugo.toml` |
| 头像、副标题、小挂件 emoji、右侧栏部件、首页横幅 | `config/_default/params.toml` |
| 左侧的 GitHub / 邮箱 / RSS 图标 | `config/_default/menu.toml` |
| 站点标题（多语言时） | `config/_default/languages.toml` |
| 头像图片 | `assets/img/avatar.jpg`（正方形最佳，会裁成圆形显示） |
| 首页横幅大图 | `assets/img/home-banner.jpg`（横版，16:9 左右最合适） |
| 浏览器标签图标 | `assets/img/favicon.png` |
| 关于页 | `content/page/about/index.md` |
| 友链页 | `content/page/links/index.md` |
| 分类的名称、简介、配色 | `content/categories/<分类名>/_index.md` |
| 中文界面文案 | `i18n/zh-cn.toml` |
| 配色、圆角、字体等全部视觉 | `assets/scss/custom.scss` |

### 配色在哪里改

站点的整套视觉（底色、主色、卡片圆角、阴影、字号）都集中在 `assets/scss/custom.scss`
开头的两个变量块里：一个是浅色，一个是深色。想换主色只改 `--accent-color` 就行，
例如换成墨绿 `#0f766e` 或绛红 `#b91c1c`。

> 为什么不用主题自带的 `[colorScheme]` 配置？主题只支持在几个预设里选，
> 而 `custom.scss` 可以完全自定义，且升级主题不会冲突。

### 首页横幅与图片

首页顶部的大图在 `config/_default/params.toml` 的 `[homeBanner]` 里配置：

```toml
[homeBanner]
    image    = "img/home-banner.jpg"   # 放在 assets/img/ 里；留空则不显示横幅
    height   = 260                     # 桌面端高度（px），手机上会自动变矮
    position = "center"                # 裁切焦点：center / top / bottom
    title    = ""                      # 图上标题，留空用站点标题
    subtitle = ""                      # 图上副标题，留空不显示
```

图片文件放在 `assets/img/` 下即可，不需要手动压缩，Hugo 会处理。

**图片建议尺寸**

| 用途 | 文件 | 建议 |
| --- | --- | --- |
| 头像 | `assets/img/avatar.jpg` | 正方形，400×400 以上。界面上裁成圆形显示，主体尽量居中 |
| 首页横幅 | `assets/img/home-banner.jpg` | 横版，1440×810（16:9）左右 |
| 浏览器图标 | `assets/img/favicon.png` | 正方形 192×192。**别用整张复杂插画缩到 32px**，会糊成一团，裁个主体特写更好 |

换图后如果浏览器还显示旧图，`Ctrl + F5` 强制刷新（浏览器对 favicon 缓存很顽固）。

> ⚠️ **改 `params.toml` 时注意 TOML 语法**：顶层键必须写在所有 `[表头]` 之前。
> 比如 `favicon` 要放在 `[dateFormat]` 上面，否则会被解析成 `dateFormat.favicon`，
> 站点图标就会静默失效、且不报任何错。同理 `mainSections`、`rssFullContent` 也在最上面。

### 分类的配色

主题默认按分类名的哈希值随机生成颜色，分类一多就会撞色。所以本站改成在分类页里显式指定：

```yaml
# content/categories/技术/_index.md
---
title: 技术
description: 折腾与踩坑
style:
    background: "hsl(168, 60%, 87%)"
    color: "hsl(172, 72%, 22%)"
---
```

背景和文字色用同一色相（这里是 168）保证协调，深浅分别调明度即可。

### 关于域名

`config/_default/hugo.toml` 第一行：

```toml
baseURL = "https://你的域名/"
```

部署到 GitHub Pages 项目仓库时，工作流会自动覆盖这个值，本地不用改也能构建。

---

## 三、写文章

### 新建

```bash
hugo new post/我的新文章/index.md
```

会生成 `content/post/我的新文章/index.md`。为什么用文件夹而不是单个 `.md`？
因为文件夹形式（Page Bundle）可以把图片放在一起，方便管理：

```
content/post/我的新文章/
├── index.md
├── cover.png     ← 封面图
└── diagram.png   ← 正文里的图
```

### frontmatter 字段

```yaml
---
title: 文章标题
description: 显示在首页卡片上的一句话摘要
date: 2026-09-22
lastmod: 2026-09-22          # 可选，会显示"最后更新于"
slug: my-post                # 可选，URL 用 /p/my-post/，不填用文件夹名
image: cover.png             # 可选，封面图
categories:
    - 技术
tags:
    - Hugo
    - 指南
draft: true                  # 改成 false 才会正式发布
---
```

### 正文常用语法

- **提示框**（GitHub 风格）：

  ```markdown
  > [!TIP]
  > 标题写在这里
  >
  > 内容。
  ```

  类型可选 `NOTE` / `TIP` / `IMPORTANT` / `WARNING` / `CAUTION`。

- **代码块**：三个反引号加语言名，自带行号和复制按钮。

- **数学公式**：见下面单独一节。

- **图片**：`![说明](cover.png)`，同目录文件直接写文件名。

- **视频**：内置短代码 `{{</* bilibili "BV号" */>}}`、`{{</* youtube "视频ID" */>}}`。

- **目录**：文章页右侧会自动生成，来自二级和三级标题。

目前 `content/post/` 是空的，站点处于刚建好的状态：首页只有横幅，右侧栏的归档 / 分类 / 标签云
会**等到第一篇带对应信息的文章发布后自动出现**，不需要手动配置。写第一篇文章就会填充它们。

### 数学公式

已配置好，**离线可用**，写 `$...$` 行内、`$$...$$` 块级即可：

```markdown
质能方程 $E = mc^2$ 写在这里。

$$
\int_{0}^{1} x^{2}\,dx = \frac{1}{3}
$$
```

**唯一的坑**：公式块里不要让 `=` 或 `-` 单独占一行。因为 Markdown 会把
"一行文字 + 单独一行 `=`" 解析成一级标题，导致公式被拆开。

```markdown
<!-- 错：= 单独一行 -->
$$
a
=
b
$$
```

```markdown
<!-- 对：= 与内容同行 -->
$$
a = b
$$
```

如果确实需要 `=` 独占一行，用 `<div>` 包起来绕过解析：

```markdown
<div>
$$
a
=
b \\
c
$$
</div>
```

**KaTeX 是自托管的**（`static/vendor/katex/`），不走 CDN。这样国内网络或断网时公式都能显示。
升级版本时下载新版 dist 覆盖该目录即可。

**加载策略**：默认全局开启（`config/_default/params.toml` 里 `article.math = true`）。
如果文章大多没有公式，想加快加载，改成 `false`，再在需要的文章 frontmatter 里写 `math: true`。

### 字体

阅读区（正文、文章标题）用 **Times New Roman + 仿宋**：西文走 Times，中文走仿宋。
界面元素（菜单、侧栏、标签、元信息）保持无衬线字体，代码用等宽字体，公式交给 KaTeX 自己的数学字体。

都集中在 `assets/scss/custom.scss` 顶部的 `--reading-font` / `--ui-font` / `--mono-font` 三个变量里，
想换字体改这三处即可。注意西文字体要写在中文前面，否则中文会先被西文字体匹配走。

---

## 四、部署上线

生成的都是静态文件，托管选择很多。下面三种都免费且够用。

### 方案 A：Cloudflare Pages（推荐，国内访问相对好）

1. 把代码推到 GitHub 仓库。
2. 打开 [Cloudflare Dashboard](https://dash.cloudflare.com/) → Workers & Pages → Create → Pages → 连接 Git 仓库。
3. 构建配置：
   - **Framework preset**：Hugo
   - **Build command**：`hugo --minify`
   - **Build output directory**：`public`
   - **环境变量**：`HUGO_VERSION` = `0.166.0`
4. 保存后自动构建，之后每次 push 都会重新部署。
5. 在 Custom domains 里绑定自己的域名。

### 方案 B：Vercel

1. 导入 GitHub 仓库。
2. Framework 选 Hugo（会自动识别），Output Directory 填 `public`。
3. 环境变量里设 `HUGO_VERSION=0.166.0`。

### 方案 C：GitHub Pages

仓库里已经放好了工作流 `.github/workflows/deploy.yml`，推到 GitHub 后自动生效：

1. 在 GitHub 建一个**空仓库**（不要勾选 Add README / .gitignore / license）。
2. 本地关联并推送（分支名必须是 `main`）。
3. push 后 Actions 会自动构建部署，无需手动开 Pages
   （工作流里配了 `enablement: true`，会自动把 Source 设为 GitHub Actions）。

**两种仓库类型，选一种：**

| 类型 | 仓库名 | 访问地址 | baseURL |
| --- | --- | --- | --- |
| 用户/组织根站点 | 必须叫 `用户名.github.io` | `https://用户名.github.io/` | `https://用户名.github.io/` |
| 普通项目站点 | 任意，如 `blog` | `https://用户名.github.io/blog/` | `https://用户名.github.io/blog/` |

两种都能正常工作——工作流会用真实地址自动覆盖 baseURL。区别只是访问地址长相。
根站点更短，而且每个账号只能有一个这种仓库。

> **注意**：GPL-3.0 的主题要求保留 `themes/hugo-theme-stack/LICENSE`，
> 并且站点代码需要公开。所以仓库请设为 **Public**——
> 免费账号的 Private 仓库本来也用不了 Pages。
> 仓库本身就是公开的，页脚也保留了主题署名，已满足协议要求。

#### 常见问题：Actions 里每次都有个 "pages build and deployment" 失败

**症状**：每次 push 后出现两条工作流，其中 `pages build and deployment` 失败，
报错位置是 `Build with Jekyll`；自己的 `Deploy Hugo site to Pages` 却是成功的，网站也能访问。

**原因**：这是 GitHub 内置的 Pages 构建器在跑（Jekyll），它不认识 Hugo 项目，必然失败。
说明 Pages 的构建源还停留在旧模式（`build_type = legacy`），也就是「Deploy from a branch」。

**解决**：把 Pages 构建源改成 GitHub Actions。两种办法：

- 网页操作：仓库 **Settings → Pages → Build and deployment → Source** 选 **GitHub Actions**
- 命令行（需要 token，`<user>` 换成你的用户名，`<token>` 换成有 repo 权限的 PAT）：

  ```bash
  curl -X PUT \
    -H "Authorization: Bearer <token>" \
    -H "Accept: application/vnd.github+json" \
    https://api.github.com/repos/<user>/<user>.github.io/pages \
    -d '{"build_type":"workflow"}'
  ```

检查当前是哪种模式：

```bash
curl -H "Authorization: Bearer <token>" \
  https://api.github.com/repos/<user>/<user>.github.io/pages
# build_type 为 workflow 就是对的；为 legacy 就会出现上面那个失败
```

改完之后再 push 一次，就不会再出现那条失败的 Jekyll 构建了。

### 方案 D：自己的服务器

```bash
hugo --minify
# 然后把 public/ 里的内容传到服务器，例如：
rsync -avz --delete public/ user@your-server:/var/www/blog/
```

配 Nginx：

```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /var/www/blog;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

---

## 五、可选功能

### 评论

`config/_default/params.toml` 里默认关着。推荐两种免费方案：

**Waline**（需要单独部署一个服务端，[官方文档](https://waline.js.org/guide/get-started/)）：

```toml
[comments]
    enabled  = true
    provider = "waline"

    [comments.waline]
        serverURL = "https://你的-waline-地址.vercel.app"
```

**Giscus**（基于 GitHub Discussions，最简单）：

1. 给仓库开启 Discussions，装上 [giscus App](https://github.com/apps/giscus)。
2. 在 [giscus.app](https://giscus.app/) 填仓库信息，拿到 `repoID` 和 `categoryID`。
3. 填进配置：

```toml
[comments]
    enabled  = true
    provider = "giscus"

    [comments.giscus]
        repo       = "用户名/仓库名"
        repoID     = "R_xxxx"
        category   = "Announcements"
        categoryID = "DIC_xxxx"
```

### 统计分析

在 `layouts/_partials/head/custom.html`（新建）里贴统计代码，例如 Umami 或 Google Analytics：

```html
<script defer src="https://你的统计服务/script.js" data-website-id="xxxx"></script>
```

### 备案

如果用国内服务器托管，记得在页脚加备案号。改 `config/_default/params.toml`：

```toml
[footer]
    since = 2026
    customText = '我的博客 · <a href="https://beian.miit.gov.cn/">京ICP备xxxxxxxx号</a>'
```

`customText` 会显示在版权行的年份后面，用 `safeHTML` 渲染，所以可以写链接。

---

### 页脚年份

`since` 是建站年份，显示规则由主题控制：

- 与当前年份相同 → 只显示一个（`© 2026`）
- 与当前年份不同 → 自动变成区间（`© 2026 - 2027`）

跨年后不用手动改，年份会自动更新。

---

## 六、目录说明

```
个人网站/
├── config/_default/
│   ├── hugo.toml         站点核心配置（域名、标题、分页、高亮、公式）
│   ├── params.toml       主题参数（头像、副标题、小部件、评论）
│   ├── menu.toml         左侧社交图标
│   ├── languages.toml    语言与副标题
│   └── related.toml      相关文章推荐规则
├── content/
│   ├── _index.md         首页（只定义菜单项）
│   ├── post/             所有文章
│   ├── page/             归档 / 搜索 / 关于 / 友链 独立页
│   ├── categories/       分类的展示信息（标题、简介、配色）
│   └── tags/             标签列表页的标题
├── assets/
│   ├── img/
│   │   ├── avatar.jpg        头像
│   │   ├── home-banner.jpg   首页横幅大图
│   │   ├── favicon.png       浏览器标签图标
│   │   └── favicon-32.png    小尺寸图标（备用）
│   └── scss/custom.scss  整套视觉（配色、字体、圆角、卡片、横幅）
├── static/vendor/katex/  KaTeX 公式渲染库（自托管，不依赖 CDN）
├── layouts/
│   ├── home.html         首页（在主题基础上加了横幅大图）
│   └── _partials/
│       ├── article/components/math.html  公式加载（改为站内路径）
│       ├── data/title.html   覆盖主题的页面标题（让 404 显示中文）
│       └── head/custom.html  插入统计代码等 head 内容
├── i18n/zh-cn.toml       中文界面文案
├── archetypes/default.md 新文章模板
├── themes/hugo-theme-stack/  Stack 主题（已随仓库一起提交）
├── .github/workflows/    GitHub Pages 自动部署
└── public/               构建产物（自动生成，不用管）
```

---

## 七、常见问题

**Q：页面样式全乱了 / 报错 `TOCSS: failed to transform`**
没用 extended 版 Hugo。跑 `hugo version` 确认输出里有 `extended`。

**Q：中文摘要、字数统计不对**
`config/_default/hugo.toml` 里 `hasCJKLanguage = true` 要打开。

**Q：文章有公式但不渲染**
在该文章的 frontmatter 里加 `math: true`，公式才会加载渲染脚本。

**Q：改了主题文件，升级主题会冲突**
不要直接改 `themes/hugo-theme-stack/` 里的文件。要改样式写在 `assets/scss/custom.scss`；
要改页面模板，把对应文件复制到站点 `layouts/` 下的相同路径再改，Hugo 会优先用站点里的。

**Q：如何更新主题版本**

```bash
cd themes/hugo-theme-stack
git pull
```

**Q：改了颜色但页面没变**
浏览器缓存。用 `Ctrl + F5` 强制刷新；如果还不行，删掉 `resources/_gen/` 再重新构建。

**Q：404 页面的浏览器标签显示英文**
这是 Hugo 内置行为（`.Title` 固定为英文）。已经用 `layouts/_partials/data/title.html` 覆盖成中文，
不需要额外处理。若升级主题后标题行为有变，对照主题同名文件检查即可。

---

## 技术栈

- [Hugo](https://gohugo.io/) — 静态网站生成器
- [Stack](https://github.com/CaiJimmy/hugo-theme-stack) — 卡片式主题，by [Jimmy Cai](https://jimmycai.com)
