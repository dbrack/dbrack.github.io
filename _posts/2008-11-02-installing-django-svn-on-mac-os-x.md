---
layout: post
title: Installing Django SVN on Mac OS X
date: '2008-11-02 08:35:00'
---

So you wanna use Django SVN on Mac OS X (Leopard)? Here’s how:
Open a Terminal and create a home for Django. You can do that by typing

```language-bash
sudo mkdir /usr/local/django
```

to the Terminal.
Now change to this directory by entering

```language-bash
cd /usr/local/django
```

Ready to check out the Django Source? Ok, then enter

```language-bash
sudo svn co http://code.djangoproject.com/svn/django/trunk/
```

Ok, we’re almost done. The next thing we have to do is to tell Python where Django lives.

```language-bash
sudo ln -s /usr/local/django/trunk/django /Library/Python/2.5/site-packages/django
```

Alright, one last thing you have to do. You obviously wanna be able to create a Django project pretty much evrywhere on your system. So we have to create yet another symlink. But this time it will link to the django-admin.py script

```language-bash
sudo ln -s /usr/local/django/trunk/django/bin/django-admin.py /usr/local/bin/
```

Done! Now you're ready to role. You should now be able to start a project by using the

```language-bash
django-admin.py startproject myproject
```

command.
Next time you want to update your Django SVN code, simply go to the trunk directory in your Terminal

```language-bash
cd /usr/local/django/trunk/
```

and then enter

```language-bash
sudo svn up
```

That’s basically it. Have fun using Django SVN on your Mac OS X.