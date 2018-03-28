---
layout: page
render_title: true
title: Thoughts - Archive
---

{% assign sorted_thoughts = site.thoughts | sort: "date" | reverse %}
{% for thought in sorted_thoughts %}
  {% if thought.title != "Thoughts - Archive" %}
## [{{ thought.title }}]({{ thought.url }})
{{ thought.content }}
  {% endif %}
{% endfor %}
