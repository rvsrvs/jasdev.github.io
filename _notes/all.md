---
layout: page
render_title: true
title: Diary Archive
---

{% assign sorted_entries = site.notes | sort: "date" | reverse %}

(Note: This is my technical diary, a place where I exhale from longer-form work. There will likely be typos, mistakes, or wider logical leaps since I’m writing for myself—the intent is to learn in public.)

<hr>

{% for entry in sorted_entries %}
  {% if entry.title != "Diary Archive" %}
## [{{ entry.title }}]({{ entry.url }})
<span class="post-date">{{ entry.date | date_to_string }}</span>
{{ entry.content }}
  {% endif %}
{% endfor %}
