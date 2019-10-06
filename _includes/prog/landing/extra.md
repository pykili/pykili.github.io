{% for section in site.data.links.extra %}
#### {{ section.title }}

{% if section.desc %}
{{ section.desc }}
{% endif %}

{% for item in section.items %}
- {% if item.lang %}`{{ item.lang }}`{% endif %} [{{ item.text }}]({{ item.url }}) {% if item.desc %} — {{ item.desc }} {% endif %}
{% endfor %}
<br>
{% endfor %}
