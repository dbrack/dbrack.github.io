---
layout: post
title: Django fieldset crazyness
date: '2008-11-01 08:35:00'
---

I’ve been messing around with [Django](http://www.djangoproject.com/ "Django") for quite a while now. All in all it’s a great framework. There’s just this one small thing that drives me crazy. The fieldsets with all those braces and parentheses. It’s really confusing and makes the code kinda hard to read. However,  here’s just a small code snipped to show you what I mean:

```language-python
fieldsets = (
(None, {
    'fields': ('title', 'pub_date', 'tags', 'category', 'content', 'creator',)
}),
    ('Attachments', {
        'fields' : ('image',),
        'classes' : ('collapse',),
    }),
    ('Advanced options', {
        'fields' : ('slug',),
        'classes' : ('collapse',),
    }),
)
```
    
Crazy stuff, right? But oh well, the result however, is great!