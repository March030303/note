# VSCode MySQL Shell插件连接说明

## 1. New Connection 的作用

`New Connection`是用来**创建新的数据库连接配置**，它的作用是让 VSCode 插件连接到你已经存在的 MySQL 服务器中的用户账号，而不是用来创建新的 MySQL 用户账号。你需要在这个配置里填写已有的 MySQL 用户的信息，来建立 VSCode 和 MySQL 数据库的连接。

## 2. 能否使用之前的密码

可以使用之前`march`用户的密码，前提是这个密码是正确的，并且`march`用户拥有`localhost`的访问权限（如果之前还存在`Access denied`的错误，需要先按照之前的解决方案修复该问题）。

## 3. 连接的基本步骤

在点击`New Connection`后，你需要填写以下信息来完成连接配置：

1. **Hostname**：填写`localhost`（如果是连接本地的 MySQL 服务器）

2. **Port**：默认是`3306`，如果你的 MySQL 修改了端口，填写对应的端口号

3. **Username**：填写`march`

4. **Password**：填写`march`用户的密码（之前的密码，或者重置后的新密码）

5. 可以选择填写连接名称（方便区分多个连接）

6. 填写完成后，点击测试连接，确认可以连接成功后，保存该连接配置。

## 4. 注意事项

如果测试连接时仍然出现`Access denied for user 'march'@'localhost'`的错误，需要先解决该权限问题，你可以参考之前生成的《MySQL 访问错误解决方案》文档来修复。

## 5. 初始安装MySQL时设置的密码对应账户说明

你一开始安装MySQL时设置的密码，默认对应的是MySQL的**超级管理员账户——root账户**（用户名固定为`root`），这个账户是MySQL安装后自动创建的默认管理员账户，拥有MySQL服务器的全部操作权限（比如创建其他用户、修改权限、管理数据库等），和你之前尝试使用的`march`账户是两个完全不同的账户。

补充说明（贴合你的使用场景，帮你理清关联）：

- root账户：安装MySQL时手动设置的密码，就对应这个账户，是MySQL的最高权限账户，可用于管理所有数据库和其他用户（包括`march`账户）；

- march账户：是一个普通MySQL用户（非默认创建），它的密码和root账户密码无关，需要单独设置或重置，你之前出现的访问拒绝错误，就是因为march账户的密码错误，或该账户没有localhost访问权限，和root账户密码无关；

- 特殊情况：若安装时未提示设置密码（如部分Linux系统、Homebrew安装），root账户可能初始无密码，或系统生成了临时随机密码（临时密码会记录在MySQL日志文件中，可通过日志查询），具体取决于你的MySQL版本和安装方式。

结合你使用VSCode MySQL Shell插件的需求：你可以先用root账户和安装时设置的密码登录MySQL，检查并重置march账户的密码、授予其localhost访问权限，之后再用march账户的正确密码，通过插件的New Connection完成连接配置。

## 6. root账户新建MySQL用户（含密码设置）步骤

由于你并未手动设置过`march`账户（该账户可能是误操作生成或插件默认提示），下面教你用**root超级管理员账户**，新建任意数量的MySQL普通账户，并设置密码，操作完成后即可用新账户通过VSCode插件连接，步骤全程简单可操作：

### 第一步：用root账户登录MySQL（两种常用方式，选一种即可）

- 方式1：VSCode插件内登录（最便捷，贴合你的使用场景）
  
1. 打开VSCode MySQL Shell插件，点击`New Connection`；
  
2. Hostname填`localhost`，Port填`3306`；
  
3. Username填`root`，Password填你**安装MySQL时设置的root密码**；
  
4. 点击测试连接，连接成功后保存配置，即可在插件内用root登录MySQL。


- 方式2：电脑终端/CMD登录（通用，适合插件登录失败时备用）

1. 打开电脑终端（Windows用CMD，Mac/Linux用终端）；
 
2. 输入命令：`mysql -u root -p`，按回车；
  
3. 提示“Enter password:”时，输入root密码（输入时不显示，输完按回车），即可登录MySQL。


### 第二步：新建MySQL用户+设置密码（核心步骤）

登录root账户后，复制下面的命令（按需修改参数），逐行执行（每行输完按回车），即可新建账户并设密码，以新建2个常用账户为例（可无限新增）：

1. 新建账户1（示例：用户名`user1`，密码`123456`，仅允许本地localhost访问，适合自己使用）
  
命令：`CREATE USER 'user1'@'localhost' IDENTIFIED BY '123456';`
说明：把`user1`改成你想要的用户名（如test、dev），`123456`改成你想设的密码（建议复杂一点，如Test@123）。


2. 新建账户2（示例：用户名`user2`，密码`Test@456`，同样仅本地访问）
  
命令：`CREATE USER 'user2'@'localhost' IDENTIFIED BY 'Test@456';`

3. （可选）新建允许远程访问的账户（若需在其他电脑连接，按需添加）
  
命令：`CREATE USER 'user3'@'%' IDENTIFIED BY 'Remote@789';`
说明：`%`表示允许所有远程IP访问，仅用于多设备连接时，日常本地使用不建议开。


### 第三步：给新账户授权（必须做，否则无法连接数据库）

新建的账户默认没有任何权限（无法查看、操作数据库），需给账户授予基础权限，命令如下（按需选择）：

- 基础权限（推荐，适合日常使用）：授予查看、操作所有数据库的权限，输完按回车
  
命令：`GRANT ALL PRIVILEGES ON *.* TO 'user1'@'localhost' WITH GRANT OPTION;`
说明：把`user1`改成你新建的用户名（如user2），逐个账户执行即可。


- 有限权限（按需设置）：仅授予某个特定数据库的操作权限（更安全）
  
命令：`GRANT ALL PRIVILEGES ON 数据库名.* TO 'user1'@'localhost' WITH GRANT OPTION;`

- 刷新权限（关键一步，必须执行）：让授权生效
  
命令：`FLUSH PRIVILEGES;`

### 第四步：验证新建账户（可选，确保能用）

1. 退出root登录：输入命令`quit;`（终端登录时），或在VSCode插件内右键root连接选择“Disconnect”；

2. 按照“第三步 连接的基本步骤”，在插件内新建连接，Username填你新建的用户名（如user1），Password填你设置的密码；

3. 点击测试连接，显示连接成功，即为新建账户生效，可正常使用。

补充提示：后续若想修改账户密码、删除账户，仍需用root账户登录，执行对应命令即可（如需，可补充相关操作）。
> （注：文档部分内容可能由 AI 生成）