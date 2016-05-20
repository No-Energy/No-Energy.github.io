---
title: C#反射Assembly详细说明
date: 2016-2-11 00:11:19
categories: .Net
toc: true
---

# C#反射Assembly详细说明
[参考资料](http://blog.csdn.net/lyncai/article/details/8621880)

- 对C#反射机制的理解
- 概念理解后，必须找到方法去完成，给出管理的主要语法
- 最终给出实用的例子，反射出来dll中的方法

反射是一个程序集发现及运行的过程，通过反射可以得到*.exe或*.dll等程序集内部的信息。使用反射可以看到一个程序集内部的接口、类、方法、字段、属性、特性等等信息。
在System.Reflection命名空间内包含多个反射常用的类，下面表格列出了常用的几个类。

|类型|作用|
|---|---|
|Assembly|通过此类可以加载操纵一个程序集，并获取程序集内部信息|
|EventInfo|该类保存给定的事件信息|
|FieldInfo|该类保存给定的字段信息|
|MethodInfo|该类保存给定的方法信息|
|MemberInfo|该类是一个基类，它定义了EventInfo、FieldInfo、MethodInfo、PropertyInfo的多个公用行为|
|Module|该类可以使你能访问多个程序集中的给定模块|
|ParameterInfo|该类保存给定的参数信息|
|PropertyInfo|该类保存给定的属性信息|
 
## System.Reflection.Assembly类
通过Assembly可以动态加载程序集，并查看程序集的内部信息，其中最常用的就是`Load()`这个方法。
``` c
Assembly assembly=Assembly.Load("MyAssembly");
```
利用Assembly的`object CreateInstance(string)`方法可以反射创建一个对象，参数0为类名。

## System.Type类
Type是最常用到的类，通过Type可以得到一个类的内部信息，也可以通过它反射创建一个对象。一般有三个常用的方法可得到Type对象。
利用`typeof()`得到Type对象
``` c
Type type=typeof(Example);
```
利用`System.Object.GetType()`得到Type对象
``` c
Example example=new Example();
Type type=example.GetType();
```
利用`System.Type.GetType()`得到Type对象
``` c
Type type=Type.GetType("MyAssembly.Example",false,true);
```
注意参数0是类名，参数1表示若找不到对应类时是否抛出异常，参数1表示类名是否区分大小写
例子：
我们最常见的是利用反射与Activator结合来创建对象。
``` c
Assembly assembly= Assembly.Load("MyAssembly");
Type type=assembly.GetType("Example");
object obj=Activator.CreateInstance(type);
```

## 反射方法
### 通过 System.Reflection.MethodInfo能查找到类里面的方法
``` c
Type type=typeof(Example);  
MethodInfo[] listMethodInfo=type.GetMethods();  
foreach(MethodInfo methodInfo in listMethodInfo)  
Cosole.WriteLine("Method name is "+methodInfo.Name);  
```

### 我们也能通过反射方法执行类里面的方法
``` c
Assembly assembly= Assembly.Load("MyAssembly");  
Type type=assembly.GetType("Example");  
object obj=Activator.CreateInstance(type);  
MethodInfo methodInfo=type.GetMethod("Hello World");  //根据方法名获取MethodInfo对象  
methodInfo.Invoke(obj,null);  //参数1类型为object[]，代表Hello World方法的对应参数，输入值为null代表没有参数  
```

## 反射属性
### 通过 System.Reflection.PropertyInfo 能查找到类里面的属性
常用的方法有GetValue（object,object[]) 获取属性值和 SetValue(object,object,object[]) 设置属性值
``` c
Type type=typeof(Example);  
PropertyInfo[] listPropertyInfo=type.GetProperties();  
foreach(PropertyInfo propertyInfo in listPropertyInfo)  
Cosole.WriteLine("Property name is "+ propertyInfo.Name);  
```

### 我们也可以通过以下方法设置或者获取一个对象的属性值
``` c
Assembly assembly=Assembly.Load("MyAssembly");  
Type type=assembly.GetType("Example");  
object obj=Activator.CreateInstance(type);  
PropertyInfo propertyInfo=obj.GetProperty("Name");    //获取Name属性对象  
var name=propertyInfo.GetValue(obj,null）;                //获取Name属性的值  
PropertyInfo propertyInfo2=obj.GetProperty("Age");     //获取Age属性对象  
propertyInfo.SetValue(obj,34,null);                              //把Age属性设置为34  
```

## 反射字段
通过 System.Reflection.FieldInfo 能查找到类里面的字段
它包括有两个常用方法`SetValue（object,object)`和`GetValue(object)`使用方法与反射属性非常相似

## 反射特性
通过`System.Reflection.MemberInfo的GetCustomAttributes(Type,bool)`就可反射出一个类里面的特性,以下例子可以反射出一个类的所有特性
```c
Type type=typeof("Example");  
object[] typeAttributes=type.GetCustomAttributes(false);       //获取Example类的特性  
foreach(object attribute in typeAttributes)  
Console.WriteLine("Attributes description is "+attribute.ToString());  
```
通过下面例子，可以获取Example类Name属性的所有特性通过下面例子，可以获取Example类Name属性的所有特性
``` c
public class Example  
{  
    [DataMemberAttribute]  
    publics string Name  
    {get;set;}  
    ..................  
}  
Type type = typeof(Example);          
PropertyInfo propertyInfo=type.GetProperty("Name");    //获取Example类的Name属性  
foreach (object attribute in propertyInfo.GetCustomAttributes(false))        //遍历Name属性的所有特性  
    Console.WriteLine("Property attribute: "+attribute.ToString());  
```

## 总结：
### `Assembly.Load()`方法，`Assembly.LoadFrom()`方法，`Assembly.LoadFile()`方法的区别
在C#中，我们要使用反射，首先要搞清楚以下命名空间中几个类的关系:
**System.Reflection命名空间**
1. AppDomain:应用程序域，可以将其理解为一组程序集的逻辑容器
2. Assembly:程序集类
3. Module:模块类
4. Type:使用反射得到类型信息的最核心的类
他们之间是一种从属关系，也就是说，一个AppDomain可以包含N个Assembly,一个Assembly可以包含N个Module,而一个Module可以包含N个Type.

#### Assembly.Load()
这个方法通过程序集的长名称（包括程序集名，版本信息，语言文化，公钥标记）来加载程序集的，会加载此程序集引用的其他程序集，
一般情况下都应该优先使用 这个方法，他的执行效率比LoadFrom要高很多，而且不会造成重复加载的问题（原因在第2点上说明）
使用这个方法的时候， CLR会应用一定的策略来查找程序集，实际上CLR按如下的顺序来定位程序集：
- 如果程序集有强名称，在首先在全局程序集缓（GAC）中查找程序集。
- 如果程序集的强名称没有正确指定或GAC中找不到，那么通过配置文件中的<codebase>元素指定的URL来查找
- 如果没有指定强名称或是在GAC中找不到，CLR会探测特定的文件夹：
假设你的应用程序目录是C:\AppDir,<probing>元素中的privatePath指定了一个路径Path1,你要定位的程序集是AssemblyName.dll则CLR将按照如下顺序定位程序集
```
C:\AppDir\AssemblyName.dll
C:\AppDir\AssemblyName\AssemblyName.dll
C:\AppDir\Path1\AssemblyName.dll
C:\AppDir\Path1\AssemblyName\AssemblyName.dll
```

如果以上方法不能找到程序集，会发生编译错误，如果是动态加载程序集，会在运行时抛出异常！
    
#### Assembly.LoadFrom()
这个方法从指定的路径来加载程序集，实际上这个方法被调用的时候，CLR会打开这个文件，获取其中的程序集版本，语言文化，公钥标记等信息，把他们传递给 Load方法，
接着，Load方法采用上面的策略来查找程序集。如果找到了程序集，会和LoadFrom方法中指定的路径做比较，如果路径相同，该程序集 会被认为是应用程序的一部分，
如果路径不同或Load方法没有找到程序集，那该程序集只是被作为一个"数据文件"来加载，不会被认为是应用程序的一部分。 
这就是在第1点中提到的Load方法比LoadFrom方法的执行效率高的原因。另外，由于可能把程序集作为"数据文件"来加载，所以使用 LoadFrom从不同路径加载相同程序集的时候会导致重复加载。
当然这个方法会加载此程序集引用的其他程序集。

#### Assembly.LoadFile()
这个方法是从指定的文件来加载程序集，和上面方法的不同之处是这个方法不会加载此程序集引用的其他程序集！
结论：一般大家应该优先选择Load方法来加载程序集，如果遇到需要使用LoadFrom方法的时候，最好改变设计而用Load方法来代替！

### 另：`Assembly.LoadFile`与`Assembly.LoadFrom`的区别
1. Assembly.LoadFile只载入相应的dll文件，比如Assembly.LoadFile（"abc.dll"），则载入abc.dll,假如abc.dll中引用了def.dll的话，def.dll并不会被载入。
Assembly.LoadFrom则不一样，它会载入dll文件及其引用的其他dll,比如上面的例子，def.dll也会被载入。

2. 用Assembly.LoadFrom载入一个Assembly时，会先检查前面是否已经载入过相同名字的Assembly,比如abc.dll有两个版本（版本1在目录1下，版本2放在目录2下），程序一开始时载入了版本1,当使用Assembly.LoadFrom（"2\\abc.dll"）载入版本2时，不能载入，而是返回版本1.Assembly.LoadFile的话则不会做这样的检查，比如上面的例子换成Assembly.LoadFile的话，则能正确载入版本2.
LoadFile:加载指定路径上的程序集文件的内容。LoadFrom: 根据程序集的文件名加载程序集文件的内容。

区别：
- LoadFile 方法用来来加载和检查具有相同标识但位于不同路径中的程序集。但不会加载程序的依赖项。
- LoadFrom 不能用于加载标识相同但路径不同的程序集。

