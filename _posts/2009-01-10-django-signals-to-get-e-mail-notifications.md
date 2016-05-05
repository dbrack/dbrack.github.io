---
layout: post
title: Django signals to get E-Mail notifications
date: '2009-01-10 14:20:00'
---

Django includes a signal dispatcher which allows you to get notified if some actions have taken place. So what does that mean? Let's say you are using Django's [comments framework](http://docs.djangoproject.com/en/dev/ref/contrib/comments/) and you would like to receive an E-Mail if someone has posted a comment. That's pretty easy to do actually.
Add the following code to your `__init__.py` file in your application folder:

```python
from django.contrib.comments.signals import comment_will_be_posted
from django.core.mail import send_mail
def comment_notification(sender, **kwargs):
    comment = kwargs['comment']
    subject = 'New Comment on %s' % comment.content_object.title
    message = 'Author: %s (%s)\n %s\n\nComment:\n%s' % (comment.user_name, comment.user_email,comment.user_url, comment.comment)
    send_mail(subject, message, 'sender', ['receivers',]) 
comment_will_be_posted.connect(comment_notification)
```

There are different kind of signals, I use the `comment_will_be_posted` signal which is sent just before a comment will be saved, but after it’s been sanity checked and submitted.
You can find out more about Django's signal dispatcher in the official [documentation](http://docs.djangoproject.com/en/dev/topics/signals/).