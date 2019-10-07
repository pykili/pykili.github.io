{% for lesson in site.data.prog-lessons %}
{% if page.url contains lesson.link %}

{% if lesson.hw.group1.deadline or lesson.hw.group2.deadline or lesson.hw.group3.deadline or lesson.hw.group4.deadline %}

---

Выполните задание пройдя по ссылке в GitHub Classroom:

{% if lesson.hw.group1.deadline %}
- 1 группа: дедлайн **{{ lesson.hw.group1.deadline }}** — <{{ lesson.hw.group1.classroom }}>
{% endif %}
{% if lesson.hw.group2.deadline %}
- 2 группа: дедлайн **{{ lesson.hw.group2.deadline }}** — <{{ lesson.hw.group2.classroom }}>
{% endif %}
{% if lesson.hw.group3.deadline %}
- 3 группа: дедлайн **{{ lesson.hw.group3.deadline }}** — <{{ lesson.hw.group3.classroom }}>
{% endif %}
{% if lesson.hw.group4.deadline %}
- 4 группа: дедлайн **{{ lesson.hw.group4.deadline }}** — <{{ lesson.hw.group4.classroom }}>
{% endif %}

{% endif %}

{% endif %}
{% endfor %}
