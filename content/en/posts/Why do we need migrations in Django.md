---
title: "Why do we need migrations in Django"
date: 2023-05-27T11:25:00+03:00
draft: false
---

I have three iterations of my relationship with migrations in Django. When saw it at first time, I don't understand why do we need it. It seems like a extra step for me. When I starts to use it, I understand - it is for saving changes for database in VCS like git.

After some time I change my mind, and I asked myself this question again. Look, we have file `models.py`, and migrations just represent changes of this file. For schema migrations all we need is a store changes of `models.py` and that is! Why we need add extra step with file migrations?

And now, in a third iteration, I understand it. There are three main reasons:
1. Django don't know about your VCS. It knows nothing about different versions `models.py`. So, the management of DB schema changes is now your area of responsibility. With migrations Django allows you to move backward and forward on DB schema changes history, without any  VCS, and apply any historical screenshot of your DB to current DB. Migrations is a VCS for your DB!
2. With migrations Django can control migration process. Django checks your changes and find conflicts or highlight some bad moments. For example, if you add a new field withoud default value, Django will notify you about it. Migration gives you additional protection from erros. 
3. With the scheme-migration, there is also the data-migration. Data migration helps you made changes with data after you change schema. With scheme-migration + data-migrations you can revert your whole DB (schema+data) to previous state. There is no any tool to do this without migrations. VCS can't help you, and looking for changes of `models.py` also doesn't make sence.

Using migrations is a safer and more controlled way to manage the database structure and prevent errors. Now I hope you now with me, at third iteration :)