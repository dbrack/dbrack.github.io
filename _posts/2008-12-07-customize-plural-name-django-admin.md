---
layout: post
title: Customize plural name Django Admin
date: '2008-12-07 08:35:00'
---

Django automatically generates a plural verbose name for your data models. However, it happens that the plural name is wrong. In my case, the model is called 'Category'. On the admin site it was listed as 'Categorys' which is obviously wrong. This can be fixed very easily. Just add a Meta class to your model class and override the 'verbose_name_plural' variable.
In my case it looks like that:

```python
class Category(models.Model):
...
class Meta:
    verbose_name_plural = "Categories"
    ...
```
