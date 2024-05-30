You might have faced the following error:

```pymysql.err.OperationalError: (1118, 'Row size too large. The maximum row size for the used table type, not counting BLOBs, is 65535. This includes storage overhead, check the manual. You have to change some columns to TEXT or BLOBs')```

In such case you can do 2 things:
1. Modifying the my.cnf file.
2. Trim tables.

<h1>Modifying the my.cnf file:</h1>

1. First open my.cnf file using ```sudo nano /etc/mysql/my.cnf```.
2. It should contain the following:
```plaintext
#
# The MariaDB/MySQL tools read configuration files in the following order:
# 0. "/etc/mysql/my.cnf" symlinks to this file, reason why all the rest is read.
# 1. "/etc/mysql/mariadb.cnf" (this file) to set global defaults,
# 2. "/etc/mysql/conf.d/*.cnf" to set global options.
# 3. "/etc/mysql/mariadb.conf.d/*.cnf" to set MariaDB-only options.
# 4. "~/.my.cnf" to set user-specific options.
#
# If the same option is defined multiple times, the last one will apply.
#
# One can use all long options that the program supports.
# Run program with --help to get a list of available options and with
# --print-defaults to see which it would actually understand and use.
#
# If you are new to MariaDB, check out https://mariadb.com/kb/en/basic-mariadb-articles/

#
# This group is read both by the client and the server
# use it for options that affect everything
#
[client-server]
# Port or socket location where to connect
# port = 3306
socket = /run/mysqld/mysqld.sock

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
```

3. Add these lines below [mysqld]:
   ```
   innodb-file-format=barracuda
   innodb-file-per-table=1
   innodb-large-prefix=1
   innodb_log_file_size = 512M
   innodb_strict_mode = 0
   ```
4. The final text should look something like this:
```plaintext
#
# The MariaDB/MySQL tools read configuration files in the following order:
# 0. "/etc/mysql/my.cnf" symlinks to this file, reason why all the rest is read.
# 1. "/etc/mysql/mariadb.cnf" (this file) to set global defaults,
# 2. "/etc/mysql/conf.d/*.cnf" to set global options.
# 3. "/etc/mysql/mariadb.conf.d/*.cnf" to set MariaDB-only options.
# 4. "~/.my.cnf" to set user-specific options.
#
# If the same option is defined multiple times, the last one will apply.
#
# One can use all long options that the program supports.
# Run program with --help to get a list of available options and with
# --print-defaults to see which it would actually understand and use.
#
# If you are new to MariaDB, check out https://mariadb.com/kb/en/basic-mariadb-articles/

#
# This group is read both by the client and the server
# use it for options that affect everything
#
[client-server]
# Port or socket location where to connect
# port = 3306
socket = /run/mysqld/mysqld.sock

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
innodb_log_file_size = 512M
innodb_strict_mode = 0

[mysql]
default-character-set = utf8mb4
```

5. Then do ```sudo service mysql restart```
6. This should fix the issue in most cases. But in my case it didn't work. If you still face the same issue as me then follow the next instruction.

<h1> Trim tables </h1>

1. In MySQL, there is a hard limit of 4096 columns per table, but the effective maximum may be less for a given table. The exact limit depends on several interacting factors. Every table (regardless of storage engine) has a maximum row size of 65,535 bytes. Storage engines may place additional constraints on this limit, reducing the effective maximum row size. You can check here for more info: https://docs.erpnext.com/docs/user/manual/en/maximum-number-of-fields-in-a-form
2. When we delete a custom field it does not get deleted from the database so basically we need to trim out all the unneccessary fields from database table that are just redundunt.
3. From your bench folder do ```bench --site site_name trim-tables```. This will delete the redundant fields.
4. Now migrate site with ```bench --site site_name migrate``` just in case.

Hope this helped.
