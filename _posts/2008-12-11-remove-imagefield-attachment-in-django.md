---
layout: post
title: Remove ImageField attachment in Django
date: '2008-12-11 07:42:00'
---

I use Django's `ImageField` to upload images to my posts/pages. It's easy to use and does what it's supposed to do. However, I figured that Django doesn't provide an easy way to delete an Image (I guess) after you have uploaded it on the Django Admin site. After some [googling](http://www.djangosnippets.org/snippets/894/ "Title"), I found a really easy way to add this functionality to my project. I simply added a checkbox to my Post model, which I called `remove_image()`. Afterwards, I created a `save()` method within the Post model class, which adds some functionality to the `save()` method that was inherited from the `models.Model` class. In there, I simply check whether `remove_image()` was set to True. If so, the Image will be removed from the file system and an empty string will be assigned to the `ImageField`. Lastly, I call the `save()` method from the base class, to make sure Django saves ervything correctly. Here's the code snipped:

```python
import os
...
class Post(models.Model):
    image = models.ImageField(upload_to='uploads/', blank=True)
    remove_image = models.BooleanField('Remove Image')
    ...

def save(self):
    if self.remove_image and self.image != "":
        if os.path.exists(self.image.path):
            os.remove(self.image.path)
            self.image = ""
            self.remove_image = False
		
    super(Post, self).save()
    ...
```