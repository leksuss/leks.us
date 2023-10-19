---
title: "Query Manager in Django ORM"
date: 2023-04-09T23:48:00+03:00
draft: false
---

When you write some query in Django ORM you always see this word `objects`. For example:
```python
Post.objects.all()
```

Okay, `Post` is the model, `all()` is a method which say - give me all records. But what is the `objects`? This is a **query manager**. This is an interface through which our instructions goes to SQL queries. It serves our queries like `all()`, `get()`, `filter()`, etc.. and returns `QuerySet` or an object (record). `objects` is a method of class, not object. You can read this code `Post.objects.all()` like this: give me `all()` `objects` from `Post` model. Or like this: from `Post` table we ask to get `objects`. Which `objects`? `all()` objects.

Few people know that you can rename `objects` to any free name you want. But I think it is a bad idea in common cases. Everybody used to with `objects` and I think there is almost no reasons to change default query manager's name. Instead of renaming you can create your own query manager!

Django creates default query manager for you automatically for each model named `objects`. Here is example to create two new query managers ([from Django Docs](https://docs.djangoproject.com/en/3.2/topics/db/managers/#django.db.models.Manager)):
```python
class AuthorManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(role='A')

class EditorManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(role='E')

class Person(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    role = models.CharField(max_length=1, choices=[('A', _('Author')), ('E', _('Editor'))])
    people = models.Manager()
    authors = AuthorManager()
    editors = EditorManager()
```

Now `authors` and `editors` is a query managers, and you can access it like this:
`Person.authors.all()`, `Person.editors.all()`.  Note, we rename default `objects` to `people` in this case. This is one rare case when it's justified. Anyway, be carefull with this.