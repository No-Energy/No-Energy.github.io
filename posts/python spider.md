---
title: Python 爬虫相关
date: 2016-5-3 08:55:29
categories: Python
toc: true
---
# Python爬虫
代码位于 [GitHub](https://github.com/No-Energy/PythonLesson)
下的
WebSpide.py
WebSpide2.py
WebSpide3.py

[参考资料](http://www.cnblogs.com/fnng/p/3576154.html)

## 主要用到第三方库
requests #第三方库，类似于urllib.request，是Python较好用的http client
bs4 #第三方库，BeautifulSoup

urllib.request #自带库，2.#版本引用 urllib即可
re #正则库

## 注意点：
``` py
def getHtml(url):
    page = urllib.request.urlopen(url)
    html = page.read()
    return html.decode('utf-8')
```
直接read后编码为byte object形式，需要转换为string object

``` py
def get_data(url):
  dataList = ''
  response = requests.get(url)
  if response.status_code != 200:
    return 'noValue'
  soup = bs4.BeautifulSoup(response.text,"html.parser")
  dataList += soup.title.string[4:7]
  for meta in soup.select('meta'):
    if meta.get('name') == 'description':
      dataList += meta.get('content')
  dataList += soup.find_all('img')[1]['src']
  return dataList
```
BeautifulSoup可以解析html语言，使用BeautifulSoup可以建立规则读取网页内容，或者使用
正则表达式读取获取需要内容：
``` py
def getImg(html):
    reg = r'src="(.+?\.jpg)" pic_ext'
    imgre = re.compile(reg)
    imglist = re.findall(imgre, html)
```

urllib.request.urlretrieve()方法可以下载需要的内容
```py
pool = Pool(4)
pool.map(func, listValue)
```
线程池的使用，func为使用的方法，listValue为对应每一个方法的参数的集合
