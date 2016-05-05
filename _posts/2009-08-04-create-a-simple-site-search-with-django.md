---
layout: post
title: Create a simple site search with Django
date: '2009-08-04 12:51:00'
---

It's been a while since I wrote my last post. I've been rather busy the last couple of months. Today I finally found some time to write down some stuff. Initially I wanted to write about Django's Q object and later about how to create a simple site-search with Django. I decided to combine these two topcis and just write one post.Let me first say, I don't know if that's a good way to build a site search with Django. It works great for me as I don't have a lot of content that has to be searched. If you are looking for a more flexible and more reliable Django-Search, you should check out [Haystack](http://haystacksearch.org).

Having said this, let's get started.

First we need to create a Form in our 'views.py' file.
To do so, add the following code to your views.py file

```language-python
import django.forms as forms

class SearchForm(forms.Form):
    query = forms.CharField(label="")
```

Now we can pass this form to our template

```language-python
f = SearchForm()
myform = {'form':f}
```