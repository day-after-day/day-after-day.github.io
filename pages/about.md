---
layout: page
title: About
description: 会当凌绝顶，一览众山小。
keywords: 码缘, gaofangshang ,day-after-day
comments: true
menu: 关于
permalink: /about/
---

为自己的每一天都安排好计划

告别浑噩的人生

不要让自己在睡前觉得：自己今天都没有做

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
