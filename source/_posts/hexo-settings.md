---
title: hexo_settings
date: 2025-05-22 23:46:49
tags:
---

## 文章的存放和静态资源的处理

### 静态资源的处理

对于我这种习惯于将文档引用的静态资源存放在一个单独的文件中的人, 则需要使用更组织化的方式来管理资源

需要在_config.yml启用配置

```yaml
post_asset_folder: true
marked:
  prependRoot: true
  postAsset: true
```

## skip static sources render

```yaml
skip_render: /static/*/**
```

- '*': Any file or directory in parent folder.
- '**': All the file and directory in parent folder and its child folder.

## 关于文本文件乱码

将乱码文件的编码从UTF-8改为ANSI就不乱码了。
