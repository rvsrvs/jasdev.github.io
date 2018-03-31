---
layout: post
title: The Interplay Between High-Cadence and Well-Formed Thoughts
permalink: high-cadence-versus-well-formed
type: essay
---

Back in January, I started a public, high-cadence log of [what’s on my mind](/thoughts)[^1]. Along the spectrum between my [Twitter account](http://twitter.com/jasdev) and this blog, the journal sits in-between. _Unpolished_ thoughts, developed over time. Six months and 46 entries later, I wanted to take a moment to reflect on my experience so far.

## Why Keep One?

Two reasons. First, to create a distillery for my thoughts. By regularly thinking in public, I can better spot themes, [reconsolidate memories](/memory-reconsolidation), and [orbit around future posts](/thoughts/2017-1-7). Second, it felt like the most natural solution to the following question: “What single document should you read before meeting me to know what’s on my mind?” Or as [Ryan](http://twitter.com/ryandawidjan) succinctly put it, a “one-way coffee with my brain.“

## What Went Well?

One of the subtle—yet subconscious—lenses I _accidentally_ use when thinking about my favorite creators (writers, artists, engineers, etc.) is through their volumes of work[^2]. “She is the creator of A and B and wrote a wonderful post on C.” It’s [too easy to forget](/value-of-conferences#humanizing-heroes) that __we’re not our work__.

This journal helped reveal my inner gears. More specifically, it allowed others to see beyond the blog posts, [side projects](https://twitter.com/parrots/status/779014268905816064), and [(bad) puns](https://twitter.com/jasdev/status/791701214664790016).

<blockquote class="twitter-tweet" data-conversation="none" data-lang="en"><p lang="en" dir="ltr">^ personally wish blogging was more about peeking behind the curtain into one&#39;s mind rather than shipping a polished contained unit</p>&mdash; ryan (@ryandawidjan) <a href="https://twitter.com/ryandawidjan/status/601601021471825920">May 22, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">This message absolutely made my day &lt;3 <a href="https://t.co/BxWQMUI6oF">pic.twitter.com/BxWQMUI6oF</a></p>&mdash; Jasdev Singh (@jasdev) <a href="https://twitter.com/jasdev/status/854301253714759681">April 18, 2017</a></blockquote>

## What Didn’t Go So Well?

Counterintuitively, the journal almost acted like an _escape valve_ for my ambition to write longer-form posts. Instead of taking the extra time to crystallize a cohesive thesis, I’d sometimes fire off an entry and call it a day[^3]. Doing so caused me to miss out on arguably the most important part of writing: the final 20% involved in publishing. Editing, _n_-ary drafts, and trimming the cruft are the metaphorical [sticking points](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4887540/) where most of my cognitive growth happens.

A second, more concrete aspect that could have been improved was tooling. Below are the steps[^4]—read friction—around updating the thought journal:

- Open [Tower](https://www.git-tower.com) and pull the latest changes
- Open `thoughts.md` in [iA Writer](https://twitter.com/jasdev/status/882070849389555714)
- “Spend some time writing new entry”
- Make a commit in Tower and push the changes up to GitHub

The fact that this process had four steps (and involved a `git` commit) impeded my cadence quite a bit. Ryan’s approach of deferring to [Quip](http://quip.com/jgBUALiGBjwp) to focus on writing is better in this regard. However, I leaned towards a lack of tooling (and external hosting à la Quip) in an attempt to avoid the “configuration trap“ of [writing software instead of prose](http://jxf.me/entries/in-the-beginning/) and to carve out [my own little corner](http://blog.semilshah.com/2016/04/30/medium-rare/) of the Internet.

## What’s Next?

Moving forward, I’ve decided to make a few changes:

#### Thoughts Archive

The original version of the journal was simply one gigantic Markdown file. I’ve since migrated it to a Jekyll [collection](/thoughts) with permalinks to each post. This will allow me to point others to specific entries and concretely trace themes across posts.

#### Feed

While I thought the _lack_ of notifications would be an asset[^5], it ended up adding friction for [those who would otherwise love to read the journal](https://twitter.com/wahoo/status/854365871443107840). As an initial option, I’ve opened up an [RSS feed](/thoughts.xml) for my thoughts—which I might pipe to a separate Twitter account in the future.

#### Routine

Inspired by [Justin Duke](https://twitter.com/justinmduke)’s [keyboard shortcut to start new blog posts](https://twitter.com/justinmduke/status/882434291258376193), I made a port for my setup. Now, starting a new journal entry is as simple as ⌘⌥⌃T.

![](/public/images/thoughts_workflow.png)

Noticing that I [wrote most of my entries in the evening](https://github.com/Jasdev/jasdev.github.io/graphs/punch-card), public journaling started naturally capping off my days. [Evening routines](https://twitter.com/jasdev/status/780141465917947904) warrant a post in itself. However, one habit that has stuck recently is [being somewhat religious](https://twitter.com/jasdev/status/877709265070284801) about my [OmniFocus Daily Reviews](http://irace.me/gtd#review-everything-every-week). Looking ahead, I’m going use this existing momentum to [stack](http://jamesclear.com/habit-stacking) my journal entries—public and [private](/small-moments)—after the review.

![](/public/images/daily_review.png)

---

This is all an experiment. So, I’ll check back in six month’s time. Until then, here’s what’s been [on my mind this past week](/thoughts/2017-7-25).

---

Special thanks to [Claire](https://twitter.com/_eeclaire), [John](https://twitter.com/jxxf), [Justin](https://twitter.com/justinmduke), and [Felix](https://twitter.com/krausefx) for reading early drafts of this essay.

## Footnotes:

[^1]: This experiment was inspired by Ryan’s attempt at this: [High-Cadence Thoughts](http://quip.com/jgBUALiGBjwp).

[^2]: On the note of “volumes of work,” an example that still blows me away is [Above & Beyond](https://twitter.com/aboveandbeyond)’s radio show: [Group Therapy](https://itunes.apple.com/us/podcast/above-beyond-group-therapy/id286889904?mt=2) (formerly [Trance Around the World](https://itunes.apple.com/us/podcast/above-beyond-trance-around-the-world/id993499023?mt=2)). The group has run the podcast every week for 688 weeks (13+ years) so far!

[^3]: I have noticed [this dynamic](https://twitter.com/ryandawidjan/status/739490348553150464) with tweets too.

[^4]: I’d repeat the same process on iOS with [Git2Go](https://git2go.com) replacing Tower.

[^5]: i.e. I _thought_ the journal would be more effective when visits stemmed from curiosity, instead of pushes.
