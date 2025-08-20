---
title: Directories # you may put the course name here
---

<ul>
   {% for item in site.data.dirs-list%}
      <li><a href="{{ item._link }}">{{ item.title }}</a></li>
   {% endfor %}
</ul>
