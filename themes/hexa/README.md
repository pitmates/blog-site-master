# hexa

It's another version of `HeXadillaX`, the theme of `HeXadillaX`.

## Usage

### CONFIG

修改_config.yml。

### TAGS && CATEGORIES

在 `blog` 下的 `source` 文件夹下创建 `tags` 文件夹、`categories` 文件夹，`links` 文件夹、`vita` 文件夹。
在创建的文件夹下创建 `index.md`。

在 `tags/index.md` 中写入:

```markdown
layout: tags
title: tags
---
```

在 `categories/index.md` 中写入:

```markdown
layout: categories
title: categories
---
```
其它文件夹自己设计。

### DUOSHUO

Register your own [duoshuo](http://dev.duoshuo.com/docs/501e6ce1cff715f71800000d) account and replace `duoshuo short_name` in `_config.yml`.


### 单篇文章开启 Mathjax

向 `_config.yml` 文章源码文件头部添加 `mathjax: true`


