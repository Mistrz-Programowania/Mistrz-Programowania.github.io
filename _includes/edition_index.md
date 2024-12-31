<ul>
{% for solution in site.solutions %}
  {% if solution.edition == include.edition %}
    <li><a href="{{ solution.url }}">{{ solution.title }}</a></li>
  {% endif %}
{% endfor %}
</ul>
