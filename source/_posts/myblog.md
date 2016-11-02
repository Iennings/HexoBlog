---
title: 我的博客搭建过程
date: 2016-05-02 11:29:25
categories:
tags: blog
---
# 今天基本把我的博客给搞好了,就差域名备好案了
我是用hexo 在阿里云服务器(CentOS 7系统)上用nginx 运行的 然后用push钩子,在本地用git push完后自动部署到你的服务器上,我在网上看到很多教程，但是有一些比较久了,自己看的也迷迷糊糊的.今天就讲一下我的搭建过程,希望有用.
## hexo 使用
先下载nodejs,在你自己的本地电脑,还有阿里云服务器都要下载,我本机系统是ubuntu15.10,具体怎么下载google吧
下载完后是npm安装
```
npm install hexo -g
```
用下面语句,测试是不是成功
```
hexo help
```
接着就是在~/目录里建立一个文件夹(blog)
```
cd ~/blog
```
```
hexo init
```
<!--more-->
然后看[官方文档](https://hexo.io/zh-cn/docs/)对blog目录下的文件_config.yml进行配置
选择主题:
可以看[知乎上推荐的](https://www.zhihu.com/question/24422335)
具体操作就不说了
### 云服务器部署你的hexo博客
顺便教一下阿里云CentOS 7安装nodejs的方法
首先在阿里云服务器下载nodejs,我的是CentOs 7系统,用`yum install nodejs`下载成功了,但是在安装hexo时失败了,而且很慢,淘宝源还是慢，包括用wget 去获取最新版本,但是编译一直卡在那里,用 yum groupinstall "Developments Tools" 也不行,自己时间也不够所以就查了一下没有找到好的答案就换了一种方法,用nvm 安装
首先
```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash
```
```
source ~/.bash_profile
```
然后就可以用nvm 了
```
nvm list-remote
```
列出所有版本,选择最新版本或者合适的版本
```
nvm install v4.4.3
```
接着就是查看所安装的版本
参考网页[阿里云ESC-CenntOS安装Node.js](https://www.janecc.com/ecs-centos-node-js-v4-2-6.html)
```
nvm list
```
## 阿里云服务器当做git的远程仓库,实现hexo自动部署
至于怎么做的,我直接推荐我参考的网页[用git hoooks, 实现hexo 在阿里云自动部署](http://www.imys.net/20160303/hexo-nginx-auto-deploy.html)
这里说注意几点
1 本地公匙新建立一个
```
cd ~/.shh
```
```
ssh-keygen -t rsa -f ~/.ssh/id_rsa.aliyun -C "myblog"
```
上面的id_rsa.aliyun是我们指定的文件名，这时~/.ssh目录下会多出id_rsa.aliyun和id_rsa.aliyun.pub两个文件，id_rsa.aliyun.pub里保存的就是我们要使用的公匙
新增并配置config文件
如果在.ssh/文件夹下没有config
就新建一个config文件
```
touch ~/.ssh/config
```
在config里添加如下内容
`
Host xxxxxx(你的域名或者云服务器的ip地址)
    IdentityFile ~/.ssh/id_rsa.aliyun
	    User git
`
测试是否配置成功
```
ssh -T git@xxxxx
```
参考网站[git 生成多个ssh key及解决多个ssh key的问题](http://riny.net/2014/git-ssh-key/)
2 本地clone的git时的问题
当本地`git clone git@xxxxx` 第一个git空的文件时，你上传第一个文件后就而且是设置完`unset GIT_DIR`后，直接把你的刚刚clone 的文件夹` .git `复制到你的public文件夹就可以了,不然初始化是要处理分支问题,在hooks的自动部署上处理这问题(给我显示的错误原因打???,都不清楚到底是到底问题),我花了一个晚上也没搞好,所以就放弃了,一切从来(心痛...),对自己git学的有自信的人可以看看能不能解决
3 至于nginx问题
就是在 `/etc/nginx/` 下,到`conf`文件夹里新建一个配置文件,添加配置吧
`
有什么问题基本上goole 就可以解决了,有什么问题也可以问我一起学习,我一直觉得,我们的很多知识来自互联网大家的分享,那么我们要贡献出我们知识进行分享，这也是我写博客的目的所在!希望各位同学也能够分享出你们的知识:)
`
