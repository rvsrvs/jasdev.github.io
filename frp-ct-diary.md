---
layout: page
title: "Functional (Reactive) Programming and Category Theory Diary"
permalink: frp-ct-diary
render_title: true
---

I’ve always admired folks who learn publicly through diaries. Brent Simmons did so with his [series on Swift](https://inessential.com/swiftdiary) and I’ll follow suit as I wade through material on functional (reactive) programming and category theory.

To view all entries on a single page or be notified of updates, check out the diary’s [archive](/frp-ct-diary/all) and [RSS feed](/frp-ct-diary.xml).

- Note about AFP
- PF solution set PRs
- Approach in studying for AFP
- Add CT links from Pinboard to diary

{% assign sorted_diary_entries = site.frp-ct-diary | sort: "date" | reverse %}
{% for entry in sorted_diary_entries %}
{% if entry.title != "F(R)P and CT Diary Archive" %}
- [{{ entry.title }}]({{ entry.url }})
{% endif %}
{% endfor %}
