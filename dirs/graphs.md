---
title: Graphs

_graphs: # start with _ to ignore the filter for this attribute
  - _link: ./../graphs/knowledge-graph-subdirs.html
    title: Graph with Categories
  - _link: ./../graphs/knowledge-graph.html
    title: Graph without Categories
---

<ul>
   {% for item in page._graphs %}
      <li><a href="{{ item._link }}">{{ item.title }}</a></li>
   {% endfor %}
</ul>
