# Cannot Connect as Root in MySQL
MySQL 5.7 changed the secure model: now MySQL root login requires a sudo.

i.e., phpMyAdmin will be not able to use root credentials.

The simplest (and safest) solution will be create a new user and grant required privileges.

## 1. Connect to mysql
```console
$ sudo mysql --user=root mysql
```

## 2. Create a user for MySQL
Run the following commands (replacing `new_user` by the desired password and `some_password` by the desired password):

```console
> CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'some_password';
> GRANT ALL PRIVILEGES ON *.* TO 'new_user'@'localhost' WITH GRANT OPTION;  
> FLUSH PRIVILEGES;
```  

## 3. Optional: allow remote connections
> **Remember:** allow a remote user to have all privileges is a security concern.

```console
> CREATE USER 'new_user'@'%' IDENTIFIED BY 'some_password';
> GRANT ALL PRIVILEGES ON *.* TO 'new_user'@'%' WITH GRANT OPTION;
> FLUSH PRIVILEGES;
```

## 4. Update phpMyAdmin
Follow step 1 and 2 then before this step.  

Using `sudo`, edit `/etc/dbconfig-common/phpmyadmin.conf ` file updating user/password values in the following sections (replacing `some_password` by the password used in step 2):

```console
# dbc_dbuser: database user
#       the name of the user who we will use to connect to the database.
dbc_dbuser='phpmyadmin'

# dbc_dbpass: database user password
#       the password to use with the above username when connecting
#       to a database, if one is required
dbc_dbpass='some_password'
```

## References
- [https://askubuntu.com/questions/763336/cannot-enter-phpmyadmin-as-root-mysql-5-7](https://askubuntu.com/questions/763336/cannot-enter-phpmyadmin-as-root-mysql-5-7)