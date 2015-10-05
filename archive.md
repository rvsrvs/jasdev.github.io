---
layout: page
title: Archive
---

I usually [tweet](https://twitter.com/jasdev) out each post. However, if you prefer RSS, you can find my feed [here](/atom.xml)!

## Posts

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}

## External Posts

  * 29 Sep 2014 &raquo; [Bitcasa CloudFS Hackathon Judge ](https://twitter.com/bitcasa/status/516729203920998400)

  * 25 Sep 2014 &raquo; [Why Stripe is Becoming the Transaction Layer of the Internet](http://blog.thinkful.com/post/98406708378/why-stripe-is-becoming-the-transaction-layer-of)

  * 02 Sep 2014 &raquo; [Tech Tuesday: Speeding Up Client-Side Applications with ETags](http://imgur.com/blog/2014/09/02/tech-tuesday-speeding-up-client-side-applications-with-etags/)

  * 28 April 2014 &raquo; [UVa Engineering Unbound Magazine](http://www.seas.virginia.edu/pubs/unbound/pdfs/spring14.pdf) (Pages 8 - 9)

  * 17 Sep 2013 &raquo; [UVa Class of 2014 Feature](https://twitter.com/UVA/status/379975813581377536)

  * 10 Sep 2013 &raquo; [Mentioned in Philly.com article about PennApps](http://articles.philly.com/2013-09-10/news/41906700_1_app-store-swap-programmer)
