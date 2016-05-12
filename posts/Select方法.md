---
title: Select方法
date: 2015-7-14 11:43:19
categories: .Net
toc: true
---

# Select方法
``` c
Select();//全部查出来   
Select(过滤条件);//根据过滤条件进行过滤，如Select("columnname1   like   '%xx%'");   
Select(过滤条件,排序字段);//过滤，并排序，如Select("columnname1   like   '%xx%'",columnname2); 
```
完成一个查询，返回一个DataTable后，很多时候都想在查询结果中继续搜索。这时可以使用Select方法对结果进行再查询。 
Select方法有4个重载，我们经常用到的就是`DataTable.Select(String);`。

下面就说说带一个参数的`DataTable.Select(String)`： 
这个`String`的参数是查询的限定式。相当于SQL查询语言中的WHERE语句（不含WHERE），其语法符合SQL语言语法。 
我觉得就是类似sql的语法而已。 

不过我试了试，不支持BETWEEN AND，举个成功的例子: 
``` c
//FromTime 和ToTime 是两个DateTime类型的变量；occurTime是dTable里面的列名； 
DataRow[] datarows = dTable.Select("occurTime >= '" + FromTime + "' and occurTime <= '" + ToTime+"'"); 
```
DataTable.Select()方法里面支持简单的过滤和排序，不支持复杂的条件过滤和排序。里面的字符串必须是列名和数据，以及>,<,=,<>等关系运算符。举几个例子： 
``` c
DataRow[]   row   =   Detailtb.Select("WZMC='"+MaterialName+"' and   CZ='"+MaterialTexture+"   and   GG='"+MaterialSpecs+"'");    
DataTable.Select("City Like 'B%'"); 
DataTable.Select("name='" + a +"'"); 
```
一定要注意单引号的问题；我之前就是把变量用双引号括起来了，一直出错，后来在网上查，发现要先有双引号，再用单引号；即‘“变量”’；
