---
layout: page
title: A CT and F(R)P Diary 
render_title: true
---

A diary of notes on category theory and functional (reactive) programming. Here’s an [RSS feed](/notes.xml), for the nerds. <3

- [All entries.](/notes/all)
{% assign sorted_entries = site.notes | sort: "date" | reverse %}
{% for entry in sorted_entries %}
{% if entry.title != "A CT and F(R)P Diary - Archive" %}
- [{{ entry.title }}]({{ entry.url }})
{% endif %}
{% endfor %}
