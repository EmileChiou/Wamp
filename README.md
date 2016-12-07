# Win7 手工配置 Apache+Mysql+PHP

打开`D:\AMP\Apahce2.4\conf\httpd.conf`
在D盘新建一个文件夹Sites作为虚拟主机目录
搜索`DocumentRoot`，修改`Directory "D:/Sites"`
打开`D:\AMP\Apahce2.4\conf\extra\httpd-vhosts.conf`
修改DocumentRoot为你的虚拟主机目录`DocumentRoot "D:/Sites"`

解压php
复制一份`php.ini-development`OR`php.ini-production`取名为`php.ini`
搜索  `extension_dir = "ext"`去掉前面的注释 ; 
修改 `extension_dir = "./ext"`
打开`D:\AMP\Apache2.4\conf\httpd.conf`

## Apache To php
搜索  `<IfModule mime_module>`
在  `<IfModule mime_module>` 加上
    # Add Handler allows you to analyze the php file
    AddType application/x-httpd-php .php
再加上

###php5版本		
		PHPIniDir "D:/Program Files/php"
		LoadModule php5_module "D:/AMP/php/php5apache2_4.dll"
		LoadFile "D:/AMP/php/libeay32.dll"
		LoadFile "D:/AMP/php/ssleay32.dll"
###php7版本	
		PHPIniDir "D:/AMP/php"
		LoadModule php7_module "D:/AMP/php/php7apache2_4.dll"
		
## 安装Mysql
打开 php.ini 搜索 extension=php_mysqli.dll  
去掉 ;extension=php_mysql.dll  前面的注释  ;
重启apache
