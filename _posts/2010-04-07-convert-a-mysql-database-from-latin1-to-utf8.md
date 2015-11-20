---
layout: post
title: Convert a MySQL database from latin1 to UTF8
date: 2010-04-07 00:00:00
---

I was working on Mysql Server Version 5.0.75 at Ubuntu to convert a MySQL DataBase from latin1 to UTF8. For massive size (more than 15GB) of the database, it takes a big amount of time if we ALTER all the tables and individual columns. But if we make our changes directly in the SQL backup file and then restore the data, it works much faster. Here is a brief description of the entire process.

1. First, backup the current latin1 database.

```
 $ mysqldump -uroot -p --opt --default-character-set=latin1 --skip-set-charset DBNAME_latin1 &gt; DBNAME_latin1.sql 
```

2. run the following queries to see the current character-set settings,

```
mysql> show variables like "%character";

+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | latin1                     |
| character_set_connection | latin1                     |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | latin1                     |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.04 sec)

mysql> show variables like "%collation%";

+----------------------+-------------------+
| Variable_name        | Value             |
+----------------------+-------------------+
| collation_connection | latin1_swedish_ci |
| collation_database   | latin1_swedish_ci |
| collation_server     | latin1_swedish_ci |
+----------------------+-------------------+
3 rows in set (0.00 sec)
```

3. Stop mySql server using the command bellow,

```
$ sudo /etc/init.d/mysql stop
```

4. We have to add the following settings to my.cnf/my.ini file under [mysqld] and [client] sections.

```
$ sudo gedit /etc/mysql/my.cnf

[mysqld]
default-character-set=utf8
default-collation=utf8_general_ci
character-set-server=utf8
collation-server=utf8_general_ci
init-connect='SET NAMES utf8'

[client]
default-character-set=utf8
```

5. start the database server,

```
$ sudo /etc/init.d/mysql start

```

6. run the following queries to confirm your database server state,

```
mysql> show variables like "%character%";

+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)

mysql> show variables like "%collation%";

+----------------------+-------------------+
| Variable_name        | Value             |
+----------------------+-------------------+
| collation_connection | utf8_general_ci   |
| collation_database   | utf8_general_ci   |
| collation_server     | utf8_general_ci   |
+----------------------+-------------------+
3 rows in set (0.00 sec)
```

7. use **replace** command for replacing latin1 character set and collation to utf8.

```
$ replace "CHARSET=latin1" "CHARSET=utf8" "character_set_client = latin1" "character_set_client = utf8" "set latin1" "set utf8" "latin1_general_cs" "utf8_bin" &lt; DBNAME_latin1.sql &gt; DBNAME_utf8.sql
```

8. Restore the dump file to MySQL,

```
mysql -uroot -p --default-character-set=utf8 DBNAME < DBNAME_utf8.sql 
```

It is one of the ways to converting the latin1 database to utf8. We can also do it via direct MySQL queries, but it may slow down if the database size is too large and may create a problem with foreign keys. You can also write a shell script for the entire process.

N.B.:

1. If you use any varchar datatype as key then you have to ensure that it must be less than or equal 255 character in create table statement. Otherwise you may get error like "Specified key was too long; max key length is 767 bytes".

Here,

1 latin char = 1 byte

1 utf8 char = 3 byte

So, you have to use 767/3 = 255 char maximum for key.

2. You may also get errors like "Got a packet bigger than 'max_allowed_packet' bytes". For its solution you have to edit my.cnf file and increase the value of 'max_allowed_packet'.

3. After conversion you may found some data loss. For handling such situation you can write a program which will check your latin1 old database for fields which contains latin1 character (ASCII > 127) and generate a update script for those fields.

4. If you use AES_ENCRYPT to any field (ex. for password), you should decrypt them first before the whole process. Otherwise, it may create a big problem.





