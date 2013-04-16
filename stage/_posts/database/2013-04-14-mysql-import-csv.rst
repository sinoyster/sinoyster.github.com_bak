---
layout: post
title: "MySQL文本导入小结"
description: "mysql导入文本文件小结"
category: database
tags: ["mysql","csv"]
---
{% include JB/setup %}


对一个数据人来说，mysql是数据分析的利器，本文总结几种常用数据导入问题的解决办法

1.命令
============
MySQL 文档copy来的语法, 是不是很头大， 直接调几条带注释的来看吧

.. code :: mysql

 LOAD DATA [LOW_PRIORITY | CONCURRENT] 
     [LOCAL]              #标识是从服务器端还是客户端的文件系统进行数据导入
     INFILE 'file_name'   #指定导入文件
     [REPLACE | IGNORE]   #重复情况是覆盖还是忽略
     INTO TABLE tbl_name  #导入的表名
     [CHARACTER SET charset_name]  #导入文件的字符集
     [{FIELDS | COLUMNS}  #描述每个导入字段的属性
         [TERMINATED BY 'string']             #指字段定分隔符
         [[OPTIONALLY] ENCLOSED BY 'char']    #字段包围符号
         [ESCAPED BY 'char']
     ]
     [LINES               #行属性描述
         [STARTING BY 'string']      #行起始字符
         [TERMINATED BY 'string']    #行分割 unix: \n  ;  windows: \r\n
     ]
     [IGNORE number LINES]           #跳过前几行
     [(col_name_or_user_var,...)]    #指定文本域与表的映射，
                                     #可以直接映射到表字段，
                                     #也可以映射到某变量, 
                                     #映射为变量转换后再赋给表字段
     [SET col_name = expr,...]       #对导入映射的变量进行加工处理，
                                     #最后映射到字段上


从下面的几个案例来看Load DATA infile 是如何使用的. 
假定:在没有特殊说明的情况下，案例中的数据库和导入文本都是 **utf8** 编码,在windows下实验保存
注：在启动mysql命令时候要指定 **--local-infile** 否则 LOAD DATA 中的LOCAL不会生效

2.AUTO_INCREMENT字段
======================

大多数表都会有个自增的id(AUTO_INCREMENT)字段作为主键，导入时候需要自动生成,
我们在通过sql插入这种类型的字段时，赋值为null就可以自动生成自增的主键,
导入的时候我们也可以如法炮制。

假定有如下表和导入数据::

   CREATE TABLE imp_null (
     id INT NOT NULL AUTO_INCREMENT ,
     name   VARCHAR(16) ,
     addr   VARCHAR(64) ,
     PRIMARY KEY(id)
   );

数据文件：imp_null.txt::

   张三,地址一
   李四,地址二
   王五,地址三
   赵六,地址四

导入命令::

   LOAD DATA LOCAL INFILE 'imp_null.txt'
   INTO TABLE imp_null
   FIELDS TERMINATED BY ','  #字段之间是','分隔
   LINES TERMINATED BY '\n'  #根据你的系统选择,windows是'\r\n'; unix是'\n'
   (name, addr)              #数据文件的字段顺序,以及对应的表映射字段
   SET id=NULL;              #指定每个导入的id为NULL，数据库自动生成

3.跳过指定列
===================

跳过指定列很简单，只需要把文件映射到变量上，而不是表字段上即可

假定有如下表和导入数据::

   CREATE TABLE imp_skip (
     name   VARCHAR(16) ,
     addr   VARCHAR(64) 
   );

数据文件：imp_skip.txt::

   张三,字段2,地址一,字段4
   李四,字段2,地址二,字段4
   王五,字段2,地址三,字段4
   赵六,字段2,地址四,字段4


导入命令::

   LOAD DATA LOCAL INFILE 'imp_skip.txt'
   INTO TABLE imp_skip
   FIELDS TERMINATED BY ','  #字段之间是','分隔
   LINES TERMINATED BY '\n'  #根据你的系统选择,windows是'\r\n'; unix是'\n'
   (name,@f1,addr,@f2)       #直接把字段数据映射到变量上，
                             #变量是用@开头,对变量不做处理,就跳过相应的字段了



4.日期处理
==================

MySQL中导入的日期日期字符串格式固定是YYYY-MM-DD,如果不是这种格式就得做转换， 解决的思路就是把文本中的字符串先保存在变量中再把变量转换成日期格式

假定有如下表和导入数据::

   CREATE TABLE imp_date (
     name   VARCHAR(16) ,
     bday   DATE
   );

数据文件：imp_date.txt::

   张三,12/22/1981
   李四,05/15/1980
   王五,06/04/1989
   赵六,09/28/1990


导入命令::

   LOAD DATA LOCAL INFILE 'imp_date.txt'
   INTO TABLE imp_date
   FIELDS TERMINATED BY ','  #字段之间是','分隔
   LINES TERMINATED BY '\n'  #根据你的系统选择,windows是'\r\n'; unix是'\n'
   (name,@bday)              #把日期字段先映射到变量上，
   SET bday = STR_TO_DATE(@bday, '%m/%d/%Y');    
                             #通过转换函数把字符串转换成日期格式



5.中文处理
==================

对于linux，系统和数据库默认都是utf8编码，一般导入导出都没有太大问题。
而对于中文windows系统默认的文本文件采用的是GBK编码, 需要在导入的时候指定:
**character set gbk**

建议编码的问题还是通过iconv命令来处理, 一是数据库不一定支持指定编码，二是有编码错误会导致导入失败

6.金额处理
====================

对于很多电子表格导出的金额数据会包含\ **','**\ 号,如：'123,456.00', 导入的时候需要把','去掉，注意在导出文本的时候不要选择','分隔

假定有如下表和导入数据::

   CREATE TABLE imp_curr (
     name   VARCHAR(16) ,
     amount DECIMAL(13,2) 
   );

数据文件(\\t分隔)：imp_curr.txt::

   张三	123,456.89
   李四	123,456.89
   王五	123,456.89
   赵六	123,456.89


导入命令::

   LOAD DATA LOCAL INFILE 'imp_curr.txt'
   INTO TABLE imp_curr
   FIELDS TERMINATED BY '\t'  
   LINES TERMINATED BY '\n'  #根据你的系统选择,windows是'\r\n'; unix是'\n'
   (name,@amount)            #直接把字段数据映射到变量上，
   SET amount=REPLACE(@amount,',','')         
                             #通过REPLACE直接替换','号



$.其他
==================

跳过开头几行:: 

     IGNORE 1 LINES           #跳过第一行

总的来说不建议在导入阶段做过于复杂的转换，建议在导入之前对数据先进行预处理，所以awk、python也需要了解一些
