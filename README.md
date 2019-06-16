# configdb
## 解决8.0安全问题
* alias mysql=/usr/local/mysql/bin/mysql
* mysql -u root -p
* use mysql;
* alter user 'root'@'localhost' identified with mysql_native_password by 'root123';
