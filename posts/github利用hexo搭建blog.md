---
title: 在github上使用hexo搭建blog教程
date: 2016-5-4 14:34:14
categories: Hexo
toc: true
---
# GitHub上使用Hexo搭建Blog
**本文内容从多处整理搬运，以下为原文链接**
[史上最详细的Hexo博客搭建图文教程](https://xuanwo.org/2015/03/26/hexo-intor/)
[使用GitHub和Hexo搭建免费静态Blog](http://wsgzao.github.io/post/hexo-guide/#Hexo简写命令)

## 前言
本来自己整理的资料都是存放在OneNote上，后来也算是一个程序员的信仰，于是在GtiHub上建站，由于不是做web前端这方面的工作的，所以收集了一些资料和教程，完成了个人Blog的创建。
现在把资料和过程整理后放出。

## 准备
需要安装以下软件
- **[Node.js](https://nodejs.org/en/download/)**
- **[Git](https://git-scm.com/downloads)(或者[GitHub Desktop](https://desktop.github.com/))**

### 安装Node.js
安装基本一路Next
**注意**安装完成后在CMD中通过
``` bash
node -v
npm -v
```
来查看环境变量是否成功添加。
MacOS下在终端中输入查看即可。

成功添加的应该输出正确版本信息：
![node.js install](http://ww4.sinaimg.cn/large/826cbe25gw1f3j7a2cq40j20it0camxx.jpg)
否则需要添加Path环境变量，使`npm`命令生效。(**具体环境变量视安装路径而定**)
``` bash
;C:\Program Files\nodejs\node_modules\npm
```
MacOS下将以下地址添加到$PATH即可。
``` bash
/usr/local/bin
```

### 安装Git
安装基本安装默认一路Next
同样，安装完成后通过在CMD中查询
``` bash
git -v
```
MacOS同上。
来验证是否成功安装并添加了环境变量，否则同样需要添加环境变量

### 在GitHub中创建新repository
**如果没有账号，先去注册账号**
在代码库创建页面：
**注意，*yourname*这里指的是你在GitHub注册的名称**
在Repository name下填写yourname.github.io，Description (optional)下填写一些简单的描述（不写也没有关系），如图所示：
![github info](http://ww1.sinaimg.cn/large/826cbe25gw1f3j9sslyrfj20kw0g00vj.jpg)

### 开启gh-pages功能
点击界面上侧的Settings，你将会打开这个库的setting页面，向下拖动，直到看见Github Pages，如图：
![github pages](http://ww1.sinaimg.cn/large/826cbe25gw1f3ja0qgcopj20ki04u752.jpg)
点击Lanuch automatic page generator，Github将会自动替你创建出一个gh-pages的页面。
如果你的配置没有问题，那么大约15分钟之后，yourname.github.io这个网址就可以正常访问了~
如果yourname.github.io已经可以正常访问了，那么Github一侧的配置已经全部结束了。

## 配置Hexo
### 安装Hexo
在自己认为合适的地方创建一个文件夹，然后在文件夹空白处按住Shift+鼠标右键，然后点击在此处打开命令行窗口。（同样要记住啦，下文中会使用在当前目录打开命令行来代指上述的操作）
在命令行中输入：
``` bash
npm install hexo-cli -g
```
**注意:**MacOS下需要命令在root权限下运行，所以需要在前面加上`sudo`

然后你将会看到:
![hexo-cli](http://ww3.sinaimg.cn/large/826cbe25gw1f3jabnx9h3j20it0ca76z.jpg)
可能你会看到一个WARN，但是不用担心，这不会影响你的正常使用。
然后输入
``` bash
npm install hexo --save
```
然后你会看到命令行窗口刷了一大堆白字，下面我们来看一看Hexo是不是已经安装好了。
在命令行中输入：
``` bash
hexo -v
```
如果能正确显示版本信息，则安装完成。

### 初始化Hexo
接着上面的操作，输入：
``` bash
hexo init
```
如图：
![hexo init](http://ww4.sinaimg.cn/large/826cbe25gw1f3jagna5p6j20it0cagod.jpg)

然后输入：
``` bash
npm install
```
之后npm将会自动安装你需要的组件，只需要等待npm操作即可。

### 首次体验Hexo
继续操作，同样是在命令行中，输入：
``` bash
hexo g
```
如图:
![hexo g](http://ww3.sinaimg.cn/large/826cbe25gw1f3janunzxjj20it0cagqs.jpg)

然后输入：
``` bash
hexo s
```
然后会看到：
![hexo s](http://ww4.sinaimg.cn/large/826cbe25gw1f3jap61coaj20it0ca0xq.jpg)

在浏览器中打开 http://localhost:4000/ 你将会看到：
![hexo page](http://ww2.sinaimg.cn/large/826cbe25gw1f3jaql0zukj211y0k8dli.jpg)

到目前为止，Hexo在本地的配置已经全都结束了。

## 使用Hexo
在配置过程中请使用*yamllint*来保证自己的*yaml*语法正确
### 修改全局配置文件
具体配置信息引用自**[Hexo官方文档](http://hexo.io/zh-cn/docs/configuration.html)**
您可以在 _config.yml 中修改大部份的配置。

## 上传到GitHub
有两种方法，根据之前安装的软件选择
- **Git**
- **GitHub**

### Git方法
#### 配置Deployment
首先，你需要为自己配置身份信息，打开命令行，然后输入：
``` bash
git config --global user.name "yourname"
git config --global user.email "youremail"
```
同样在_config.yml文件中，找到Deployment，然后按照如下修改：
``` yaml
deploy:
  type: git
  repo: git@github.com:yourname/yourname.github.io.git
  branch: master
```

#### 上传
执行
``` bash
npm install hexo-deployer-git --save
```
来安装所需的插件

然后在当前目录打开命令行，输入：
``` bash
hexo d
```
随后按照提示，分别输入自己的Github账号用户名和密码，开始上传。
然后通过 http://yourname.github.io/ 来访问自己刚刚上传的网站。

### GitHub方法
设置Local path如D:\No_Energy\Projects\GitHub\
运行Git Shell切换到如D:\No_Energy\Projects\GitHub\MyBlog路径下
执行hexo g命令生成public文件夹
把生成的内容全部拷贝到Local path或其子目录
运行GitHub确认修改信息后执行右上角的Sync同步
最后访问主页观察效果

或者在GitHub中Clone之前创建的repository(yourname.github.io)
执行hexo g命令生成public文件夹
把生成的内容全部拷贝到Clone的目录
运行GitHub确认修改信息后执行右上角的Sync同步
最后访问主页观察效果

## 添加新文章
打开Hexo目录下的source文件夹，所有的文章都会以md形式保存在_post文件夹中，只要在_post文件夹中新建md类型的文档，就能在执行hexo g的时候被渲染。
新建的文章头需要添加一些yml信息，如下所示：
``` yaml
---
title: hello-world   //在此处添加你的标题。
date: 2016-5-4 14:10:21   //在此处输入你编辑这篇文章的时间。
categories: Exercise   //在此处输入这篇文章的分类。
toc: true  //在此处设定是否开启目录，需要主题支持。
---
```
### **[MarkDown语法说明](http://wowubuntu.com/markdown/#list)**
### 添加图片、音频和视频
参考资料 [Hexo添加图片、音乐和视频](http://starsky.gitcafe.io/2015/05/05/Hexo%E6%B7%BB%E5%8A%A0%E5%9B%BE%E7%89%87%E3%80%81%E9%9F%B3%E4%B9%90%E5%92%8C%E8%A7%86%E9%A2%91/)

#### 插入图片
##### 添加外部链接图片
代码样式为：
``` md
![“图片描述”（可以不写）](“图片地址”)
```
这里推荐[新浪微博图床](https://chrome.google.com/webstore/detail/%E6%96%B0%E6%B5%AA%E5%BE%AE%E5%8D%9A%E5%9B%BE%E5%BA%8A/fdfdnfpdplfbbnemmmoklbfjbhecpnhf)，当得到图片外链后，直接粘贴到小括号中即可。
该Chrome应用为开源项目，[地址](https://github.com/Suxiaogang/WeiboPicBed)
例如：
插入以下代码
``` md
![](https://camo.githubusercontent.com/c9128303f7e52d640b64b1cdd2a7948c924b3bcf/687474703a2f2f7777332e73696e61696d672e636e2f6c617267652f35666433373831386a77316577313732717378626f6732306c7a30633271636e2e676966)
```
结果如下
![how to use](https://camo.githubusercontent.com/c9128303f7e52d640b64b1cdd2a7948c924b3bcf/687474703a2f2f7777332e73696e61696d672e636e2f6c617267652f35666433373831386a77316577313732717378626f6732306c7a30633271636e2e676966)

##### 添加本地图片
在本地网站目录下新建文件夹，命名为images或者其他你喜欢的名字，然后编辑你的md博文，插入下面的代码样式：
``` md
![“图片描述”（可以不写）](/images/你的图片名字.JPG)
```
例如我在D:\No_Energy\Projects\GitHub\MyBlog\images放入了一个smile01.gif图片，然后写下：
``` md
![](/images/smile01.gif)
```
重新生成，上传后就能看到了。
**注意：**图片路径和图片名称必须为英文，否则不能显示图片。

#### 插入音乐
比如网易云音乐，找到喜欢的歌曲，点击分享按钮，生成外链播放器，把里面的代码复制下来，直接粘贴到博文中即可。
比如插入以下代码：
``` html
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="66" src="http://music.163.com/outchain/player?type=2&id=28481818&auto=0&height=66"></iframe>
```

#### 插入视频
视频也和音乐类似，先输入视频标题，回车换一行插入代码即可。一般代码都可在视频页面的分享上找到。
``` html
这是你们要的合唱版《孔明&王朗》
<embed height="415" width="544" quality="high" allowfullscreen="true" type="application/x-shockwave-flash" src="http://static.hdslb.com/miniloader.swf" flashvars="aid=982443&page=1" pluginspage="http://www.adobe.com/shockwave/download/download.cgi?P1_Prod_Version=ShockwaveFlash"></embed>
```
注意代码中的允许全屏要写上：*allowfullscreen*

## 更换主题
可以在**[Themes](https://github.com/hexojs/hexo/wiki/Themes)**寻找自己喜欢的主题
下载所有的主题文件，保存到Hexo目录下的themes文件夹下。然后在_config.yml文件中修改：
``` yaml
# Extensions
## Plugins: http://hexo.io/plugins/
## Themes: http://hexo.io/themes/
theme: landscape //themes文件夹中对应文件夹的名称
```
然后先执行`hexo clean`，然后重新`hexo g`，然后按照之前选择的方式上传，就能看到新主题的效果了。
或者在`hexo clean`，`hexo g`后选择`hexo s`然后在 http://localhost:4000/ 预览效果。