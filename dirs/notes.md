---
title: Notes
---

<ul>
{% for item in site.data.notes-list%}
    <li><a href="{{ item._link }}">{{ item.title }}</a></li>
{% endfor %}
</ul>
