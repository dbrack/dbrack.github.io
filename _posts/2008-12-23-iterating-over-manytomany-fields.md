---
layout: post
title: Iterating over ManyToMany fields
date: '2008-12-23 16:15:00'
---

If you have a Django model with a ManyToMany field, it's not obvious for a beginner (like me) how to display the content of the field in a template. The solution is actually pretty easy. Let's assume you have model class Actor:

```python
class Actor(models.Model):
    name = models.CharField('Actor name', max_length=60)
    ...
```
and a Movie class:

```python
class Movie(models.Model):
    title = models.CharField('Movie title', max_length=100)
    actors = models.ManyToManyField(Actor)
    ...
```

In order to get all movies for a specific actor, you can query the `ManyToMany` field in your controller (views.py) like this:

```python
movies = Movie.objects.filter(actors__name=name_received)
```

In your template, you can iterate over the ManyToMany field using the `object.ManyToManyField.all()` method. In my case it looks like this:

```python
{% for movie in movies %}
    Title: {{ movie.title }}
    Actors:
    {% for actor in movie.actors.all %}
        {% if not forloop.last %}
            {{ actor.name }},
        {% else %}
            {{ actor.name }}
        {% endif %}
    {% endfor %}
{% endfor %}
```
