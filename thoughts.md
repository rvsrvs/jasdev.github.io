---
layout: page
title: Thoughts
---

[Unpolished, developed over time](/high-cadence-versus-well-formed).

- [All](/thoughts/all)
{% assign sorted_thoughts = site.thoughts | sort: "date" | reverse %}
{% for thought in sorted_thoughts %}
{% if thought.title != "Thoughts - All" %}
- [{{ thought.title }}]({{ thought.url }})
{% endif %}
{% endfor %}