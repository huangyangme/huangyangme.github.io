#Terminal 管理 MySQL



##配置环境变量与启动/终止 mysql

设置环境变量： 通过运行“sudo vi /etc/bashrc”，在bash配置文件中加入mysqlstart、mysql和mysqladmin的别名（注意：修改完毕之后需要退出“终端（Terminal）”之后重新进入，这些命令才会生效）：

```
alias mysqlstart='sudo /Library/StartupItems/MySQLCOM/MySQLCOM restart'
alias mysql='/usr/local/mysql/bin/mysql'
alias mysqladmin='/usr/local/mysql/bin/mysqladmin'
```

从终端启动/停止 mysql

```
启动：sudo /Library/StartupItems/MySQLCOM/MySQLCOM start
停止：sudo /Library/StartupItems/MySQLCOM/MySQLCOM stop
重启：sudo /Library/StartupItems/MySQLCOM/MySQLCOM restart
```

修改默认密码

`mysqladmin -u root password '要设置的密码'`

终端登陆

`mysql:mysql -u root -p`

---

##MySQL服务状态

### 启动MySQL

    $ sudo /Library/StartupItems/MySQLCOM/MySQLCOM start

需要输入管理员密码。

###停止MySQL服务

    $ sudo /Library/StartupItems/MySQLCOM/MySQLCOM stop

###重启MySQL服务

    $ sudo /Library/StartupItems/MySQLCOM/MySQLCOM restart

###查看当前MySQL版本

    mysql> select version();

##更改MySQL的root管理员密码

例：把root账号的密码改成'123456'：

    /usr/local/mysql/bin/mysqladmin -u root -p password 123456
    Enter password: 
    Warning: Using a password on the command line interface can be insecure.

注意：需要知道账户的原密码才能进行修改。

##MySQL终端登录

###终端登录（繁琐）

首先使用以下命令查看路径，是否有有添加MySQL的路径：

    $ echo $PATH

MySQL的运行路径：`/usr/local/mysql/bin`，如果你能在查询结果中找到这段字符，那么就是已添加进路径里。如没有，则需要把MySQL的运行路径添加进去。

添加MySQL运行路径：

    $ PATH="$PATH":/usr/local/mysql/bin 

添加是否成功，我们可以使用which使用来查看：

    $ which mysql
    /usr/local/mysql/bin/mysql

若存在路径，则会输出mysql的运行路径，若不存在，则什么都不输出。

添加后就能正常登录了：

    $ mysql -u root -p

这里会要求输入MySQL的登录密码。

注意：每次关闭终端后，再重新打开终端，都要重新添加路径，你可以把这些命令当作是临时的。也就是说，这些命令会在终端关闭后失效。 

###终端登录（简化）

可以使用alias命令简化MySQL的终端登录操作，如果只是想要临时的话，可以直接在终端输入 `alias <简化后的名字> <执行的命令>`，关闭终端后，刚刚进行过简化的命令就会失效。如果想要让它始终存在，那么需要把alias指令添加到 `~/.bashrc`(Ubuntu) 或者 `~/.bash_profile`(MacOS)。

除了上述的终端登录方法外，还可以使用MySQL的运行路径进行登录：

    $ /usr/local/mysql/bin/mysql -u root -p
    Enter password: 

然而在终端里，可以使用 alias 命令去简化： 

    $ alias mysql=/usr/local/mysql/bin/mysql

它的格式是：alias <简化后的名字>=<'具体的指令> 

使用时就可以很简单：

    $ mysql -u root -p
    Enter password: 

但是这样做还不够，因为这个是暂时性的，只要关闭当前的终端窗口，所有简化的指令便会失效。所以需要把alias定义为全局的，可以在~/.bash_profile添加指令，前提是进入~/.bash_profile文件：

    $ vi ~/.bash_profile 

编辑前：

    export PATH="/Users/baijiawei/Library/Application Support/GoodSync":$PATH

编辑后：

    export PATH="/Users/baijiawei/Library/Application Support/GoodSync":$PATH
     
    # MySQL
    alias mysql='/usr/local/mysql/bin/mysql';

`#`那一行代表是注释，一般还会在具体的命令加上单引号，就是前面提及到的“alias <简化后的名字>=<'具体的指令>”。

最后，要使~/.bash_profile文件生效，必须使用 source 命令：

    $ source ~/.bash_profile

以后使用时，就不需要再输入那么多麻烦的指令了。

可以在终端上直接输入alias查看已有的简化命令：

    $ alias 
    alias mysql='/usr/local/mysql/bin/mysql'

##MySQL数据库的导入和导出

要想导入和导出数据库，需要用到mysqldump工具。需要找到它的运行路径：

    /usr/local/mysql/bin/mysqldump

先使用alias简化一下命令：

    alias mysqldump='/usr/local/mysql/bin/mysqldump';

###导出数据库

导出数据库时的格式：

    mysqldump -u root -p <数据库名> <表名> > <导出的名字>.sql

实例说明：

    $ mysqldump -u root -p test CLASS > class.sql

导出带删除格式的数据库，还原时能够覆盖已有数据库而不用删除原有数据库：

    mysqldump --add-drop-table e -u root -p testDB > TESTDB.sql 

###导入数据库

在已有的数据库导入数据，首先使用use命令进入到该数据库，然后：

    mysql> source /Users//Documents/Code/class.sql

导入数据库的格式：source /<路径>/. <sql>.sql，你也可以导出为.dump文件 

###还原数据库

    $ mysql -u root -p testDB < testDB.sql

---

- 原文链接：[http://www.cnblogs.com/GarveyCalvin/p/4297221.html](http://www.cnblogs.com/GarveyCalvin/p/4297221.html)
- 下一篇：[Terminal 数据库、数据表、数据操作](http://wiki.huangyang.me/read/docs/1-gong-ju/terminal-mysqlshu-ju-ku-cao-zuo)