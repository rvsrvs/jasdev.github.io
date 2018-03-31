---
layout: page
title: Etceteras
render_title: true
---

### [Link Blog](https://twitter.com/whatjasdevreads)

Instapaper is my tool of choice in navigating web content. To approximate a link blog, I’ve made my account favorites public. A few folks have reached out mentioning that they [enjoy the feed](https://twitter.com/jasdev/status/975422925825179648)—if you want to follow along, here are some ways to do so: [web](http://instapaper.com/p/jasdev), [RSS](http://instapaper.com/starred/rss/4750197/tes3ESKY5E01Sx4DKAfUYK9rUQ), and [Twitter](https://twitter.com/whatjasdevreads).

### Interludes

Writing that doesn’t quite fit the theme of Distillations. This space let’s me procrastinate on the current essay I’m drafting or dive into technical topics. I’ve kept some older—and _very embarrassing_—posts around to allow for nostalgic explorations of past work (and hopefully mile mark my progress?).

<ul>
{% assign interludes = site.posts | where:"type","interlude" %}
{% for post in interludes %}
<li>
  <a href="{{ post.url }}">{{ post.title }}</a>
</li>
{% endfor %}
</ul>

### External Posts

- [Featured Follow Kickoff Post](https://medium.com/featuredfollow/featured-follow-jasdev-singh-2c5042abe3f6#.ve0gnopzq)
- [Swift Compilation Reporting at Tumblr](https://engineering.tumblr.com/post/144151794436/swift-compilation-reporting-at-tumblr)
- [Guest on the SwiftCoders Podcast](https://overcast.fm/+GCc7vT_Uw)
- [Multithreading in iOS CodePath Guide](https://github.com/codepath/ios_guides/wiki/Multithreading-in-iOS)
- [Why Stripe is Becoming the Transaction Layer of the Internet](http://blog.thinkful.com/post/98406708378/why-stripe-is-becoming-the-transaction-layer-of)
- [Tech Tuesday: Speeding Up Client-Side Applications with ETags](https://blog.imgur.com/2014/09/02/tech-tuesday-speeding-up-client-side-applications-with-etags/)
- [UVA Engineering Unbound Magazine (Pages 8–9)](http://www.seas.virginia.edu/pubs/unbound/pdfs/spring14.pdf)

### [Thoughts](/thoughts) ([RSS](/thoughts.xml))

An effort to create a public, high-cadence log of what’s on my mind. Along the spectrum between [my Twitter account](https://twitter.com/jasdev) and Distillations, [this journal](/thoughts) sits in-between. Unpolished thoughts, developed over time. For more context, here’s my reflection on the [project](/high-cadence-versus-well-formed).
