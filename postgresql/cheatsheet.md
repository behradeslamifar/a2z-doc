ساختن کاربر با SQL
```
su postgres 
postgres@ubuntu:~$ psql
postgres=# create database mydb;
postgres=# create user myuser with encrypted password 'mypass';
postgres=# grant all privileges on database mydb to myuser;
```

ساختن کاربر با دستورات postgres
```
su postgres 
postgres@ubuntu:~$ createuser --pwprompt <username>
postgres@ubuntu:~$ createdb <dbname>
```

لیست کردن دیتابیس ها
```
su postgres 
postgres@ubuntu:~$ psql
postgres=# \l
```