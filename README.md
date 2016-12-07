# Win7 手工搭建 Apache+Mysql+PHP
1.打开D:\Program Files\Apache Software Foundation\Apahce2.4\conf\httpd.conf
2.搜素httpd-vhosts.conf,去掉前面注释

3.打开D:\Program Files\Apache Software Foundation\Apahce2.4\htdocs 新建一个文件夹Sites(emile.com)作为虚拟主机目录
4.打开D:\Program Files\Apache Software Foundation\Apahce2.4\conf\extra\httpd-vhosts.conf

5.修改DocumentRoot为你的虚拟主机目录

		DocumentRoot "D:/Program Files/Apache Software Foundation/Apahce2.4/htdocs/Sites"
    	ServerName emile.com

6.前往 C:\Windows\system32\driver\etcs\hosts
7.在最后增加词条

	 127.0.0.1       emile.com 

 (emile.com是你的虚拟主机域名ServerName)
8.重启Apache


Apache - php
9.解压php
10.前往解压好的php文件夹
11.复制一份  php.ini-development  取名为php.ini
12.搜索  extension_dir = "ext" 去掉前面的注释 ; 
13.修改 extension_dir = "D:/Program Files/php/ext（你的php/ext文件的绝对路径）"
14.打开D:\Program Files\Apache Software Foundation\Apache2.4\conf\httpd.conf
15.搜索><IfModule mime_module>
16.在><IfModule mime_module>加上
>   # Add Handler allows you to analyze the php file
    AddType application/x-httpd-php .php
17.再加上

###①php5版本		
		PHPIniDir "D:/Program Files/php"
		LoadModule php5_module "D:/Program Files/php/php5apache2_4.dll"
		LoadFile "D:/Program Files/php/libeay32.dll"
		LoadFile "D:/Program Files/php/ssleay32.dll"
		<IfModule mime_module>
    		    TypesConfig conf/mime.types
    		    AddType application/x-httpd-php .php
		</IfModule>

###②php7版本	
		PHPIniDir "D:/AMP/php"
		LoadModule php7_module "D:/AMP/php/php7apache2_4.dll"
		<IfModule mime_module>
		    TypesConfig conf/mime.types
		    AddType application/x-httpd-php .php
		</IfModule>

18.新建一个php文件 放在虚拟主机
19.重启Apache
20.链接到php文件

21.安装mysql
22.打开 php.ini 搜索 extension=php_mysqli.dll  
23.去掉 ;extension=php_mysql.dll  前面的注释  ;
24.重启apache
