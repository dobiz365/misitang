---
layout: page
title: About
description: 计算机改变世界
keywords: 唐明, 米斯唐
comments: false
menu: 关于
permalink: /about/
---

我是唐明，一个喜欢计算机的极客。

仰慕「优雅编码的艺术」。

坚信熟能生巧，努力改变人生。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
