# 数据库启动与停止
1. 通过管路员身份启动命令提示符（cmd）  
2. 输入net stop `mySQL name` （停止）  
3. 输入net start `mySQL name` （启动）

# 数据库登录与退出
1. 登录：在命令提示符中输入 mysql 【-h localhost -P 3306】 -u root -p  
2. 退出：exit or ctrl+c

# 常见命令
1. 查看当前所有的数据库  
  show databases;  
2. 打开指定的库  
  use 库名;  
3. 查看当前库的所有表  
  show tables;  
4. 查看其它库的所有表  
  show tables from 库名;  
5. 创建表  
  create table 表名（  
    列名 列类型，  
    列名 列类型，  
    。。。  
  ）  
6. 查看表结构  
  desc 表名；  
7. 查看服务器版本  
  方法一：登录到mysql  
    select version();  
  方法二：没有登录到mysql服务器  
    mysql --version or mysql -V  

# MySQL语法规范
1. 不区分大小写，但建议关键字大写，表名、列名小写  
2. 每条命令最好用分号结尾  
3. 每条命令根据需要可以进行缩进或者换行  
4. 注释  
  单行注释：#注释文字  
  单行注释：-- 注释文字  
  多行注释：/* 注释文字 */  
  
  
