{% for p in site.patterns %}
* [{{p.title}}]({{p.url | relative_url}})
{% endfor %}
