---
title: 联机托管说明
permalink: index.html
layout: home
ms.openlocfilehash: 7997ca57a9f8cff5e7f4a03c7de0eaf0c9e436ba
ms.sourcegitcommit: 2ef4734dc1cea1e600b967de23c86716f8fd779e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2022
ms.locfileid: "138095641"
---
# <a name="content-directory"></a>内容目录

可在[此处下载](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/archive/master.zip)所需的练习文件

下面列出了每个练习的超链接。

## <a name="exercise"></a>练习

{% assign Exercise = site.pages | where_exp:"page", "page.url contains '/Instructions/Exercises'" %}
| 模块 | 练习 |
| --- | --- | 
{% for activity in Exercise %}| {{ activity.Exercise.module }} | [{{ activity.Exercise.title }}{% if activity.Exercise.type %} - {{ activity.Exercise.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

