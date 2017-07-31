---
layout: page
title: Thoughts - All
---

{% assign sorted_thoughts = site.thoughts | sort: "date" | reverse %}
{% for thought in sorted_thoughts %}
{% if thought.title != "Thoughts - All" %}
## [{{ thought.title }}]({{ thought.url }})
{{ thought.content }}
{% endif %}
{% endfor %}
