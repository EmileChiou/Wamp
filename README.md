# Win7 手工配置 Apache+Mysql+PHP

前往 `http://www.apachehaus.com/cgi-bin/download.plx`，下载你需要的Apache2.4版本

解压到你的目录

打开`Path:/to/Apahce2.4/conf/httpd.conf`

搜索`ServerRoot`，修改`ServerRoot: "Path:/to/Apahce2.4"`

搜索`DocumentRoot`,修改`DocumentRoot "Path:/to/VirtualHost"`，修改`Directory: "Path:/to/VirtualHost"`


##让apache开启.htaccess

搜索`rewrite_module`,去除前面注释符号`#`

找到 

		<Directory /> 
		　　Options FollowSymLinks 
		　　AllowOverride None 
		</Directory>

修改为 

		<Directory /> 
		　　Options FollowSymLinks 
		　　AllowOverride All 
		</Directory> 
		

## Apache To php

搜索  `<IfModule mime_module>`

在  `<IfModule mime_module>` 加上

`# Add Handler allows you to analyze the php file`

`AddType application/x-httpd-php .php`

再加上

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
重启apache



安装PHP,解压php

##配置php.ini

1.复制一份`php.ini-development`  OR  `php.ini-production` 更名为 `php.ini`

2.搜索  `extension_dir = "ext"`去掉前面的注释 ; 

3.修改 `extension_dir = "./ext"`

4.开启相应的扩展库功能，找到下面的几行，把前面的注释符号“;”去掉

`extension=php_curl.dll`      		-----curl库

`extension=php_gd2.dll`        		-----GD库

`extension=php_openssl.dll`    		-----ssl  github经常用到

`extension=php_mbstring.dll`		-----mbstring库

`extension=php_pdo_mysql.dll`     	-----mysql-PDO库

`extension=php_xmlrpc.dll`     		-----XML库

##php-mysql

搜索

`;extension=php_mysql.dll`		-----php5-mysql数据库

`;extension=php_mysqli.dll`		-----php7-mysql数据库

去掉前面的注释  ;


5.配置PHP的Session功能

在使用session功能时，必须配置session文件在服务器上的保存目录，否则无法使用session，需要建一个可读写的目录文件夹，那么我们在WAMP文件夹里phpSessionTmp目录，然后在`php.ini`文件中找到

`;session.save_path = "/tmp"`

 修改为：
 
`session.save_path = "Path:/to/phpSessionTmp"`

6.配置PHP的文件上传功能

在使用PHP文件上传功能时，必须指定一个临时文件夹以完成文件上传功能。下面在`Path:/to`文件夹里创建一个`phpFileUploadTmp`文件夹，然后在`php.ini`文件中找到

`;upload_tmp_dir =`

修改为：

`upload_tmp_dir = "Path:/to/phpFileUploadTmp"`

7.修改`date.timezone`，默认为美国时间

找到

`;date.timezone =`

修改为：

`date.timezone = Asia/Shanghai`


		
配置好环境变量，打开终端，`httpd -k install;`

## 安装Mysql

下载mysql压缩包 `https://dev.mysql.com/downloads/mysql/`

解压到目录，进入目录，

复制一份`my-default.ini`为`my.ini`

```
		basedir = Path:/to/mysql
		datadir = Path:/to/mysql/data
		port = 3306
		character-set-server = utf8
		skip-grant-tables
```

设置环境变量Path里，添加一条：`Path：\to\mysql\bin;`。打开终端，先后输入:

命令：`mysqld --initialize`      直接初始化mysql，生成data文件夹中的文件.

命令：`mysqld -install`          安装mysql.

命令：`net start mysql`          启动服务器.

命令：`mysql -u root -p`         回车，回车，进入mysql服务.

命令：`use mysql`          先选择一个表.

命令：`update user set authentication_string=password('123456') where user='root' and Host = 'localhost';` mysql服务设置密码.

命令：`mysql> flush privileges;`  更新权限.

命令：`quit`                     退出mysql.

命令：`net stop mysql`         停止服务器.

服务打开`my.ini`,把`skip-grant-tables`注释掉.

命令：`net start mysql`          启动服务器.

命令：`mysql -u root -p`         回车，`123456`,回车进入mysql服务.

命令：`set password for 'root'@'localhost'=password('123456');`

命令：`quit`                     退出mysql.




# 删除MYSQL

`mysql -remove`
或者
`sc delete mysql`
