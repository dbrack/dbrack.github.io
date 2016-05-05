---
layout: post
title: Back After Some Downtime
date: '2009-04-02 05:33:00'
---

Over the last weekend I migrated my Webserver from [FreeBSD](http://www.freebsd.org) to [Gentoo](http://www.gentoo.org). Most of the things worked flawlessly. Since I was in an updatey mood, I decided to change the way the Django site is served too. I used Apache & mod_python before which was a rather slow solution. I now use [lighttpd](http://www.lighttpd.net) aka lighty and fcgi to serve this site. I can say, it's definitely a lot faster now.
I'm gonna write a detailed post about serving Django with lighttpd on Gentoo later on. I'm kind of busy at the moment so I don't find much time to write down all the stuff I want to.
Here are some posts I'm planning to write in the near future:

- Serve Django with lighty on Gentoo
- Complex Queries with Django's Q object
- Create a simple site search with Django

So ya, I guess that's pretty much it for this post.