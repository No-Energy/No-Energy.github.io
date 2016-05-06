---
title: Python urllib的urlretrieve()函数解析
date: 2016-5-5 00:12:23
categories: Python
toc: true
---
# Python urllib的urlretrieve()函数解析

下面我们再来看看 urllib 模块提供的 urlretrieve() 函数。urlretrieve() 方法直接将远程数据下载到本地。
``` py
>>> help(urllib.urlretrieve)
Help on function urlretrieve in module urllib:
urlretrieve(url, filename=None, reporthook=None, data=None)
```

- 参数 finename 指定了保存本地路径（如果参数未指定，urllib会生成一个临时文件保存数据。）
- 参数 reporthook 是一个回调函数，当连接上服务器、以及相应的数据块传输完毕时会触发该回调，我们可以利用这个回调函数来显示当前的下载进度。
- 参数 data 指 post 到服务器的数据，该方法返回一个包含两个元素的(filename, headers)元组，filename 表示保存到本地的路径，header 表示服务器的响应头。
下面通过例子来演示一下这个方法的使用，这个例子将 google 的 html 抓取到本地，保存在 D:/google.html 文件中，同时显示下载的进度。

``` py
import urllib
def cbk(a, b, c):  
    ''回调函数 
    @a: 已经下载的数据块 
    @b: 数据块的大小 
    @c: 远程文件的大小 
    ''
    per = 100.0 * a * b / c  
    if per > 100:  
        per = 100 
    print '%.2f%%' % per
url = 'http://www.google.com'
local = 'd://google.html'
urllib.urlretrieve(url, local, cbk)
```

下面是 urlretrieve() 下载文件实例，可以显示下载进度。
``` py
#!/usr/bin/python
#encoding:utf-8
import urllib
import os
def Schedule(a,b,c):
    ''
    a:已经下载的数据块
    b:数据块的大小
    c:远程文件的大小
   ''
    per = 100.0 * a * b / c
    if per > 100 :
        per = 100
    print '%.2f%%' % per
url = 'http://www.python.org/ftp/python/2.7.5/Python-2.7.5.tar.bz2'
#local = url.split('/')[-1]
local = os.path.join('/data/software','Python-2.7.5.tar.bz2')
urllib.urlretrieve(url,local,Schedule)
######output######
#0.00%
#0.07%
#0.13%
#0.20%
#....
#99.94%
#100.00%
```
通过上面的练习可以知道，urlopen() 可以轻松获取远端 html 页面信息，然后通过 python 正则对所需要的数据进行分析，匹配出想要用的数据，在利用urlretrieve() 将数据下载到本地。对于访问受限或者对连接数有限制的远程 url 地址可以采用 proxies（代理的方式）连接，如果远程数据量过大，单线程下载太慢的话可以采用多线程下载，这个就是传说中的爬虫。