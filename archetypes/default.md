---
title: "{{ replace .File.ContentBaseName `-` ` ` }}"
description: 
date: {{ .Date }}
lastmod: {{ .Date }}
slug: 
image: 
categories: 
    - 
tags: 
    - 
# 数学公式：当前全局已开启（params.toml 里 article.math = true），
# 这一行是为将来改成"按文章加载"时准备的，留着不影响。
math: true
draft: true
---

在这里写正文。

<!--
常用字段说明：
  description  首页卡片上显示的一句话摘要
  slug         URL 里的名字，不填则用文件夹名
  image        封面图，放同目录下写文件名即可
  categories   分类（会出现在右侧栏和归档页）
  tags         标签（会出现在标签云）
  draft        true = 草稿，只有 hugo server -D 时才显示
  math         设为 true 才加载数学公式渲染，正文有公式时记得打开
-->
