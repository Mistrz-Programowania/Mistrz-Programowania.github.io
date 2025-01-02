---
layout: default
---

Kliknij w link aby zobaczyć zadania dotyczące danego tematu.

<ul>
{% for tag in site.data.tags %}
<li><a href="/tags/{{ tag[0] }}">{{ tag[1] | capitalize }}</a></li>
{% endfor %}
</ul>
