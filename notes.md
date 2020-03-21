---
layout: page
title: A technical diary
render_title: true
---

Notes on category theory, functional (reactive) programming, and other etceteras—this is an attempt to learn in public, so, if there are typos or mistakes (there definitely will be), please [reach out](https://twitter.com/jasdev).

Here’s an [RSS feed](/notes.xml), for the nerds. <3

- [All entries](/notes/all)
{% assign sorted_entries = site.notes | sort: "date" | reverse %}
{% for entry in sorted_entries %}
{% if entry.title != "Diary Archive" %}
- [{{ entry.title }}]({{ entry.url }})
{% endif %}
{% endfor %}
