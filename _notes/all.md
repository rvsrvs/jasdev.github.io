---
layout: page
render_title: true
title: Diary Archive
---

{% assign sorted_entries = site.notes | sort: "date" | reverse %}

(Note: these are entries from my technical diary. There will likely be typos, mistakes, or wider logical leaps—the intent here is to “[[let] others look over my shoulder while I figure things out.](https://austinkleon.com/2015/06/14/to-be-a-teacher-and-remain-a-student/)”)

<hr>

{% for entry in sorted_entries %}
  {% if entry.title != "Diary Archive" %}
## [{{ entry.title }}]({{ entry.url }})
<span class="post-date">{{ entry.date | date_to_string }}</span>
{{ entry.content }}
  {% endif %}
{% endfor %}
