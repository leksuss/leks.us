---
title: "Permission denied after GRAND ALL in PostgreSQL"
date: 2023-04-16T14:19:00+03:00
draft: false
---

The usual behavior if you need to create a new database with a new user in PostgreSQL is to do something like this:
```sql
sudo -u postgres psql  # or docker exec -it postgres_container psql -Upostgres

postgres=# CREATE DATABASE mydb;
postgres=# CREATE USER mydb_user WITH ENCRYPTED PASSWORD 'mydb_user_pass';
postgres=# GRANT ALL PRIVELEGES ON DATABASE mydb to mydb_user;
GRANT
```

And it works all the time I have worked with PostgreSQL. But not now. With trying to create a table I receive the following error: **permission denied for schema public**:
```sql
postgres=# \c mydb mydb_user;  # switch user and database;
You are now connected to database "mydb" as user "mydb_user".
mydb=> CREATE TABLE foo (id int);
ERROR:  permission denied for schema public
LINE 1: CREATE TABLE foo (id int);
                     ^
```

WTF? Maybe I should `GRANT ALL` to `Public` schema? Ok, let's try:

```sql
mydb=> \c mydb postgres;  # switch user to postgres
You are now connected to database "mydb" as user "postgres".
mydb=# GRANT ALL ON SCHEMA public TO mydb_user;
GRANT
mydb=# \c mydb mydb_user;  # switch user to mydb_user
You are now connected to database "mydb" as user "mydb_user".
mydb=> CREATE TABLE foo (id int);
ERROR:  permission denied for schema public
LINE 1: CREATE TABLE foo (id int);
                     ^
```
Hmmm... After some googling, I found that `CREATE` command is eligible for only database owners! This feature was introduced in October 2022 with PostgreSQL version 15. You can find it [here](https://www.postgresql.org/about/news/postgresql-15-released-2526/#:%7E:text=PostgreSQL%2015%20also%20revokes%20the%20CREATE%20permission%20from%20all%20users%20except%20a%20database%20owner%20from%20the%20public) in "Other Notable Chanes". 

So now, to create tables, user should be the owner of the database in PostgreSQL 15. To set ownership you need to execute this command:
```sql
ALTER DATABASE mydb OWNER TO myuser;
```

Or, in my case:
```sql
mydb=> \c mydb postgres  # switch user to postgres
You are now connected to database "mydb" as user "postgres".
mydb=# ALTER DATABASE mydb OWNER TO mydb_user;
mydb=> \c mydb mydb_user  # switch user to mydb_user
You are now connected to database "mydb" as user "mydb_user".
mydb=> CREATE TABLE foo (id int);
CREATE TABLE
```

Wow! It Works!