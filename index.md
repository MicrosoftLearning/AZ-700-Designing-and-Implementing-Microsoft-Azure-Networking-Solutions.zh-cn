---
title: 联机托管说明
permalink: index.html
layout: home
---

# 内容目录

可在[此处下载](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/archive/master.zip)所需的练习文件

向下滚动以获取演示。 

## 练习

{% assign Exercise = site.pages | where_exp:"page", "page.url contains '/Instructions/Exercises'" %}
| 模块 | 练习 |
| --- | --- | 
{% for activity in Exercise  %}| {{ activity.Exercise.module }} | [{{ activity.Exercise.title }}{% if activity.Exercise.type %} - {{ activity.Exercise.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

<!-- ## Demonstrations (under construction)

{% assign demos = site.pages | where_exp:"page", "page.url contains '/Instructions/Demos'" %}
| Demonstration |
| --- | 
{% for activity in demos  %}| [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
-->


