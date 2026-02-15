#     MYSQL  
## 基本  
|连接|使用|关闭|
|:--:|:--:|:--:|
  |net start mysql96|mysql -u root -p|net stop mysql96|

## 解释说明相关操作  
|创建用户|给予权限|查看权限|
|:--:|:--:|:--:|
|CREATE USER 'user1'@'localhost' IDENTIFIED BY '123456';|GRANT XXXXXX ON xxxxx.* TO 'march'@'localhost';<br>FLUSH PRIVILEGES;|SHOW GRANTS FOR 'user1'@'localhost';|

## 数据库
mysql相当于一个大厦，root用户是超级管理员，可以创建新的管理者（user1），也可以创建新的房间（database：game）
|创建数据库|使用数据库|给予权限|
|:--:|:--:|:--:|
|CREATE DATABASE game;|use game;|GRANT ALL PRIVILEGES ON game.* TO 'user1'@'localhost';|

>给予权限时，**给user1管理game数据库所有的权限**