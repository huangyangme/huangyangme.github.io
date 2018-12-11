---
layout: post
title: "Ghost 如何修改文章路由地址？"
comments: true
keywords: "Ghost"
---

Ghost 默认文章地址是如： `www.macpai.cn/xxx`

希望改成： `www.macpai.cn/post/xxx`

可以在 `content/settings/routes.yaml` 中修改 `collections.permalink` 为 `/post/{slug}/`
