---
title: 友链
slug: links
description: 一些我常逛的站点
links:
  - title: Hugo
    description: 世界上最快的网站构建框架
    website: https://gohugo.io/
    image: https://gohugo.io/favicon-32x32.png
  - title: Stack 主题
    description: 本站使用的卡片式 Hugo 主题
    website: https://github.com/CaiJimmy/hugo-theme-stack
    image: https://avatars.githubusercontent.com/u/23092395?v=4
  - title: GitHub
    description: 全球最大的代码托管平台
    website: https://github.com
    image: https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png
menu:
    main:
        name: 友链
        weight: -50
        params:
            icon: link
comments: false
---

在页面的 frontmatter 里加 `links` 字段即可生成上面的卡片列表：

```yaml
links:
  - title: 站点名
    description: 一句话介绍
    website: https://example.com
    image: 图片地址（本地或外链都行）
```

想加友链可以在下面留言，或者邮件联系我。
