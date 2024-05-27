---
title: "Understanding intermediate model with many-to-many relationship in Django"
date: 2023-06-10T23:35:00+03:00
draft: false
---

Many-to-many relationship in Django generate third (intermediate) model which store key-to-key pairs from each of many-to-many models. Because there is no other way to store many-to-many relationship.

If your many-to-many pair don't need to have additional information, you can just add `ManyToManyField` to one of the model. Which one? To anyone, because there is no difference: many-to-many relationship is symmetric. For example, you have `Post` and `Tag` models:

```python
class Post(models.Model):  
    title = models.CharField()
    text = models.TextField()

class Tag(models.Model):  
    title = models.CharField()
    posts = models.ManyToManyField(Post, related_name='posts')
```

You can put `ManyToManyField` in `Post` class, or in `Tag` class. And in example above intermediate model don't have any additional options. There is no "post-tag" object you can manage or use somehow. It's just relationship you want to designate and that's all.  Note, you don't need to add `on_delete` param because key-to-key pair in intermediate models will be deleted automatically when you delete row in one of the many-to-many relationship.

Another examlpe, we have `Artist`, `Movie` and intermediate `Role` models:
```python
class Artist(models.Model):  
    name = models.CharField()

class Movie(models.Model):  
    title = models.CharField()
    artists = models.ManyToManyField(artist, related_name = 'actor')
```

And we need to add some custom fields to intermediate model. Every artist in movie has it's own role, so the intermediate model is a artist's role. Role has a lot of additional info, for example, role name. Django allows you to manually create intermediate model with custom fields. First, you should add `through` param in `ManyToManyField` and set it's value to name of intermediate model. Second, in this third model you need to specify two one-to-many fields for each of many-to-many models:
```python
class Artist(models.Model):  
    name = models.CharField()

class Movie(models.Model):  
    title = models.CharField()
    artists = models.ManyToManyField(artist, related_name = 'actor', through='Role')

class Role(models.Model):  
    role_name = models.CharField()
    artist = models.ForeignKey(artist, on_delete=models.CASCADE)  
    movie = models.ForeignKey(movie, on_delete=models.CASCADE)
```

Note, here we should set `on_delete` param for third model because it's one-to-many fields ,not many-to-many.

Now we have intermediate model with custom fields and we can add roles in admin panel using `Movie` and `Artist` models. And this is complete different example, because `Role` is independent entity having it's own properties. While "post-tag" is something like more technical relationship, in this example "artist-movie" relationship create new object - role with it's own properties.