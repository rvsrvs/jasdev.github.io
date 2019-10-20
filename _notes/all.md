---
layout: page
render_title: true
title: A CT and F(R)P Diary - Archive
---

{% assign sorted_entries = site.notes | sort: "date" | reverse %}
{% for entry in sorted_entries %}
  {% if entry.title != "A CT and F(R)P Diary - Archive" %}
## [{{ entry.title }}]({{ entry.url }})
<span class="post-date">{{ entry.date | date_to_string }}</span>
{{ entry.content }}
  {% endif %}
{% endfor %}
