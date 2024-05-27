---
title: "Permission denied after import DB in PostgreSQL"
date: 2023-08-20T19:03:00+03:00
draft: true
---

This post extends previous with the [similar title](/posts/permission-denied-after-grand-all-in-postgresql/). 

When you transfer database from one machine to another, postgres privileges needed to be re-granted to the db user. Usually it is made by command:
```sql
GRANT ALL PRIVILEGES ON DATABASE mydb TO mydb_user;
```

But in my case it's not working. So you need to explicitly grant privileges for each sql-object:

```sql
GRANT ALL ON ALL TABLES IN SCHEMA public TO mydb_user;
GRANT ALL ON ALL FUNCTIONS IN SCHEMA public TO mydb_user;
GRANT ALL ON ALL SEQUENCES IN SCHEMA public TO mydb_user;
```

Making this solving error "django.db.utils.ProgrammingError: permission denied for relation django_migrations".