---
layout: page
render_title: true
title: "F(R)P and CT Diary Archive"
---

{% assign sorted_diary_entries = site.frp-ct-diary | sort: "date" | reverse %}
{% for entry in sorted_diary_entries %}
  {% if entry.title != "F(R)P and CT Diary Archive" %}
        ## [{{ entry.title }}]({{ entry.url }})
        {{ entry.content }}
  {% endif %}
{% endfor %}
