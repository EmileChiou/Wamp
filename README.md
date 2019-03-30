# Win7 手动搭配 Apache+Mysql+PHP

0.`Visual C++ `很重要，前往你的电脑上的 `控制面板\所有控制面板项\程序和功能`查看你电脑已安装的`Visual C++`版本

不同版本的php和apache对`Visual C++`的版本要求不一样

官方下载地址

Visual C++ Redistributable for Visual Studio 2012 （即VC11）

`https://www.microsoft.com/en-us/download/details.aspx?id=30679`

Visual C++ Redistributable for Visual Studio 2013 （即VC12）

`https://www.microsoft.com/zh-cn/download/confirmation.aspx?id=40784`

Visual C++ Redistributable for Visual Studio 2015 （即VC14）

`https://www.microsoft.com/en-us/download/details.aspx?id=53840`

1.安装PHP, 前往 `https://windows.php.net/download/`，下载你需要的php版本

下载完成后，解压到你的目录

2.前往 `http://www.apachehaus.com/cgi-bin/download.plx`，下载你需要的Apache版本

下载完成后，解压到你的目录

3.下载mysql压缩包 `https://dev.mysql.com/downloads/mysql/`，下载你需要的mysql版本

下载完成后，解压到你的目录

# 配置Apache

打开`Path:/to/Apahce2.4/conf/httpd.conf`

1.配置Apache服务的目录

搜索`Define SRVROOT`，修改`Define SRVROOT "Path:/to/Apahce2.4"`

搜索`ServerRoot`，修改`ServerRoot "Path:/to/Apahce2.4"`

2.配置虚拟主机目录，即浏览器访问`localhost`时指向的目录

搜索`DocumentRoot`,修改`DocumentRoot "Path:/to/VirtualHost"`，修改`Directory: "Path:/to/VirtualHost"`


## 让apache开启.htaccess

搜索`rewrite_module`,去除前面注释符号`#`

找到 
```
	<Directory /> 
	  Options FollowSymLinks 
	  AllowOverride None 
	</Directory>
```
修改为 
```
	<Directory /> 
	  Options FollowSymLinks 
	  AllowOverride All 
	</Directory> 
```

## Apache To php

搜索  `<IfModule mime_module>`

在  `<IfModule mime_module>` 加上

`# Add Handler allows you to analyze the php file`

`AddType application/x-httpd-php .php`

在`<IfModule mime_module>` 处加上

###php5版本	
```
		PHPIniDir "Path:/to/php"
		LoadModule php5_module "Path:/to/php/php5apache2_4.dll"
		LoadFile "Path:/to/php/libeay32.dll"
		LoadFile "Path:/to/php/ssleay32.dll"
```
###php7版本	
```
		PHPIniDir "Path:/to/php"
		LoadModule php7_module "Path:/to/php/php7apache2_4.dll"
```
## 配置默认解析文件

搜索  `<IfModule dir_module>`

``
<IfModule dir_module>
    DirectoryIndex index.php index.html index.htm
</IfModule>
``

保存，安装（在已经配置好环境变量的前提下，打开命令提示符，键入`httpd -k install`）


# 配置php

1.复制一份`php.ini-development`  OR  `php.ini-production` 更名为 `php.ini`

2.搜索  `extension_dir = "ext"`去掉前面的注释 ; 

3.修改 `extension_dir = "./ext"`

4.开启相应的扩展库功能，找到下面的几行，把前面的注释符号“;”去掉

`extension=php_curl.dll`      		-----curl库

`extension=php_gd2.dll`        		-----GD库

`extension=php_mbstring.dll`		-----mbstring库

`extension=php_openssl.dll`    		-----ssl  github经常用到

`extension=php_pdo_mysql.dll`     	-----mysql-PDO库

`extension=php_xmlrpc.dll`     		-----XML库

##php-mysql

搜索

`;extension=php_mysql.dll`		-----php5-mysql数据库

`;extension=php_mysqli.dll`		-----php7-mysql数据库

去掉前面的注释符号 `;`


5.配置PHP的Session功能（非必要配置）

在使用session功能时，必须配置session文件在服务器上的保存目录，否则无法使用session，需要建一个可读写的目录文件夹，那么我们在WAMP文件夹里phpSessionTmp目录，然后在`php.ini`文件中找到

`;session.save_path = "/tmp"`

 修改为：
 
`session.save_path = "Path:/to/phpSessionTmp"`

6.配置PHP的文件上传功能（非必要配置）

在使用PHP文件上传功能时，必须指定一个临时文件夹以完成文件上传功能。下面在`Path:/to`文件夹里创建一个`phpFileUploadTmp`文件夹，然后在`php.ini`文件中找到

`;upload_tmp_dir =`

修改为：

`upload_tmp_dir = "Path:/to/phpFileUploadTmp"`

7.修改`date.timezone`，默认为美国时间（非必要配置）

找到

`;date.timezone =`

修改为：

`date.timezone = Asia/Shanghai`


保存，重启apache（在已经配置好环境变量的前提下，打开命令提示符，键入`httpd -k restart`）		


## 安装Mysql

官方安装参考手册`https://dev.mysql.com/doc/refman/5.7/en/windows-install-archive.html`（链接为5.7.18以上版本查看其他版本可以网页在右上角切换） 


### 1.配置my.ini

进入mysql的目录

#### mysql 5.7.18及以上版本

直接在根目录新建一个名为 `my.ini` 的文件（因为mysql 5.7.18 zip包不再自带默认ini文件，需要手动创建），把下面代码放进去并保存
```
	[mysqld]
	basedir = Path:/to/mysql
	datadir = Path:/to/mysql/data
	port = 3306
	character-set-server = utf8
	skip-grant-tables
```
#### mysql 5.7.18以下版本

复制一份 `my-default.ini` 另存为 `my.ini`

```
	basedir = Path:/to/mysql
	datadir = Path:/to/mysql/data
	port = 3306
	character-set-server = utf8
	skip-grant-tables
```

### 2.初始化+安装mysql

打开命令提示符（在已经配置好环境变量的前提下），先后输入:

命令：`mysqld --initialize`      直接初始化mysql，生成data文件夹中的文件.

命令：`mysqld -install`          安装mysql.

命令：`net start mysql`          启动服务器.

命令：mysql> `mysql -u root -p`         回车，回车，进入mysql服务.

命令：mysql> `use mysql;`          先选择一个表.

命令：mysql> `update user set authentication_string=password('123456') where user='root' and Host = 'localhost';` mysql服务设置密码.

命令：mysql> `flush privileges;`  更新权限.

命令：mysql> `quit;`                     退出mysql.

命令：`net stop mysql`         停止服务器.

重新打开`my.ini`编辑,把`skip-grant-tables`注释掉.

命令：`net start mysql`          启动服务器.


至此，你已经成功搭建一个WAMP环境


# 附注、其他问题

## 删除MYSQL

`mysql -remove`
或者
`sc delete mysql`



## 配置环境变量

1.前往`控制面板\所有控制面板项\系统`，或者直接在桌面上，我的电脑右键 -> 属性

2.点击左边侧边栏`高级系统设置`,点击`环境变量`

3.在第二个框，找到名为`Path`的变量，选中，点击编辑按钮

4.在变量值后面添加你的安装目录内的exe程序的位置（mysql和apache一般是bin文件夹，php时根目录），变量之间用半角分号`;`隔开

例如：`D:/AMP/apache/bin;D:/AMP/mysql/bin;D:/AMP/php;`


## 部分Apache命令行提示符

启动Apache	 	`httpd -k start`
停止Apache	 	`httpd -k stop`
重启Apache		`httpd -k restart`
卸载Apache Service	`httpd -k uninstall`
查看Apache版本	      `httpd -V`
命令行列表		     `httpd -h`

## MSVCR120.dll丢失错误，查看对应丢失的VC++版本并下载安装

msvcp、msvcr、vcomp140.dll属于VC++2015版

msvcp、msvcr、vcomp120.dll属于VC++2013版

msvcp、msvcr、vcomp110.dll属于VC++2012版

msvcp、msvcr、vcomp100.dll属于VC++2010版

msvcp、msvcr、vcomp90.dll属于VC++2008版


### 官方下载地址

Visual C++ Redistributable for Visual Studio 2010 

`https://www.microsoft.com/en-us/download/details.aspx?id=5555`

Visual C++ Redistributable for Visual Studio 2012 （即VC11）

`https://www.microsoft.com/en-us/download/details.aspx?id=30679`

Visual C++ Redistributable for Visual Studio 2013 （即VC12）

`https://www.microsoft.com/zh-cn/download/confirmation.aspx?id=40784`

Visual C++ Redistributable for Visual Studio 2015 （即VC14）

`https://www.microsoft.com/en-us/download/details.aspx?id=53840`
