# Win7 手工配置 Apache+Mysql+PHP

1.打开
		D:\AMP\Apahce2.4\conf\httpd.conf
2.在D盘新建一个文件夹Sites作为虚拟主机目录
3.搜索DocumentRoot，修改	
		Directory "D:/Sites"
4.打开
		D:\AMP\Apahce2.4\conf\extra\httpd-vhosts.conf
5.修改DocumentRoot为你的虚拟主机目录
		DocumentRoot "D:/Sites"
6.重启Apache
7.解压php
8.前往解压好的php文件夹
9.复制一份  php.ini-development  取名为php.ini
10.搜索  
		extension_dir = "ext" 
去掉前面的注释 ; 
11.修改 
		extension_dir = "./ext"
12.打开
		D:\AMP\Apache2.4\conf\httpd.conf

## Apache To php
13.搜索><IfModule mime_module>
14.在><IfModule mime_module>加上
>   # Add Handler allows you to analyze the php file
    AddType application/x-httpd-php .php
15.再加上

###php5版本		
		PHPIniDir "D:/Program Files/php"
		LoadModule php5_module "D:/Program Files/php/php5apache2_4.dll"
		LoadFile "D:/Program Files/php/libeay32.dll"
		LoadFile "D:/Program Files/php/ssleay32.dll"
		<IfModule mime_module>
    		    TypesConfig conf/mime.types
    		    AddType application/x-httpd-php .php
		</IfModule>

###php7版本	
		PHPIniDir "D:/AMP/php"
		LoadModule php7_module "D:/AMP/php/php7apache2_4.dll"
		<IfModule mime_module>
		    TypesConfig conf/mime.types
		    AddType application/x-httpd-php .php
		</IfModule>

16.新建一个php文件 放在虚拟主机
17.重启Apache
18.链接到php文件

19.安装mysql
20.打开 php.ini 搜索 extension=php_mysqli.dll  
21.去掉 ;extension=php_mysql.dll  前面的注释  ;
22.重启apache
