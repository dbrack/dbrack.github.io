---
layout: post
title: Extend Django's manage.py command
date: '2009-11-16 12:09:00'
---

In Django 1.0 it's very easy to extend the default functionality of the manage.py command.
I use it to delete the comments that have been marked as spam. The cool thing is that you can access the data model using the ORM. So, let's see how this whole thing works.

First off, we need to create a 'management' folder within our application. In there, we need to create a folder called 'commands'. This is the place where we can put our Python scripts. Don't forget to create the `__init__.py` files in both folders. The directory structure should look something like this

```
app/
    __init__.py
    models.py
    management/
        __init__.py
        commands/
            __init__.py
            greatcmd.py
    views.py
```

As you can see above, I have already created a file called `greatcmd.py` within the commands directory. The name of the file will also be the name of our command. In this example, we would execute the command like this

```bash
./manage.py greatcmd
```

Now let's take a look inside the greatcmd.py file

```python
from django.core.management.base import BaseCommand
class Command(BaseCommand):
	help = "This doesn't do anything yet"
	def handle(self, *args, **options):
		print 'greatcmd was called'
		# other fancy code here
```

That's basically it. You can add pretty much anything in the handle method. It's also possible to import your models etc. Pretty convenient and straightforward. Please note that you have to be in your application's directory when executing the manage.py command. Otherwise you won't be able to run the custom command.

If you wanna know more about it, take a look at the official [Django Documentation](http://docs.djangoproject.com/en/dev/howto/custom-management-commands/#howto-custom-management-commands). If you're interested in how I fight spam in my Django Apps, feel free to read my other post [Preventing Spam In Dango](/preventing-spam-in-django/).