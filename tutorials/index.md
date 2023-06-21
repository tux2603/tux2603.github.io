---
layout: base
---

{% for post in site.posts limit:10 %}
{% if post.categories contains 'tutorials' %}
# [{{ post.title }} - {{ post.date | date_to_string }}]({{ post.url }}) 
{{ post.description }}
> {{ post.excerpt }}
{% endif %}
{% endfor %}