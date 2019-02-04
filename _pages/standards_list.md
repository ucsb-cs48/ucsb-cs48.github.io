---
title: Standards
permalink: "/standards/"
---

## Standards

Standards for code, user stories, issues, documentation, etc.

<ul>
{% for s in site.standards %}
  <li {% if s.indent %} class="indent" {% endif %}>
   <a href="{{ s.url | relative_url}}">{{s.topic}}</a>&mdash;{{s.desc}}
  </li>
{% endfor %}
</ul>







