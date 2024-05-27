---
title: "Alter all tables to set owner in PSQL"
date: 2023-09-12T17:22:00+03:00
draft: false
---

It is not enough to set grant all for the imported database. You have to change ownership to all tables in the database. And there is no simple, built-in method to do this.

I found this solution, read all table names from information schema and alter each table via this SQL query:

```sql
SELECT format(
          'ALTER TABLE public.%I OWNER TO <user_name>',
          table_name
       )
FROM information_schema.tables
WHERE table_schema = 'public'
  AND table_type = 'BASE TABLE' \gexec
```

Some notes:
1) you need to have sufficient rights (be `postgres` or other user with  the same rights)
2) change <user_name> to yours
3) `\gexec` will execute each line of the query result as a statement