---
title: hexo+github+next搭建个人博客
date: 2016-03-16 10:13:39
tags:
- 博客
- Hexo
- GitHub
- Next
categories: 教程
---

# 安装准备软件 #
- [Node.js](https://nodejs.org/en/)

- [Git](https://git-scm.com/)

# git #
## 生成新的SSH Key ##
下载安装好git后，可通过右键桌面打开git bash命令行：

	ssh-keygen -t rsa -C "邮件地址@youremail.com"  #一直回车即可
	
	
## 添加 SSH Key 到 GitHub ##
	cd ~/.ssh 检查本机的ssh密钥
	cat id_rsa.pub
登陆github系统。点击右上角的 Account Settings—>SSH Public keys —> add another public keys

## 测试 ##
	ssh -T git@github.com  # 输入yes

## 设置用户信息 ##
Git 会根据用户的名字和邮箱来记录提交。

	git config --global user.name "aierui"//用户名
	git config --global user.email  "imland@outlook.com"//填写自己的邮箱

# github #
注册[github](https://github.com/)过程就不说了。
新建一个仓库，名字必须为 "github用户名.github.io",如图所示(我的已经存在):
![](/images/hexo+github+next/g_1.png)

打开仓库，点击设置：
![](/images/hexo+github+next/g_2.png)

在设置页面拖到最下面，点击图示按钮：
![](/images/hexo+github+next/g_3.png)

至此，github基本弄完。

然后将自动生成的文件全部删掉，可通过git clone到本地删掉，再提交。

# hexo #
## 安装 ##
	npm install -g hexo-cli #使用 npm 安装 Hexo。

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

	hexo init <folder>
	cd <folder>
	npm install

此时，执行：
	
    hexo s #启动本地服务器测试
可以启动本地服务器测试，http://localhost:4000 。

## 配置 ##


> _config.yml 全局配置文件。要注意的是，该文件格式要求极为严格，缺少一个空格都会导致运行错误。小提示：不要用Tab缩进，两个空格符， 冒号：后面只用一个空格即可 。

> themes 存放主题的文件夹

> source 博客资源文件夹

> source/_drafts 草稿文件夹

> source/_posts 文章文件夹

> themes/landscape 默认皮肤文件夹

> ……


> [官方文档](https://hexo.io/zh-cn/docs/setup.html)中较详细。

这里可以先不修改，在next主题部分详细说明

## 部署到github ##
修改 _config.yml，文件底部：

	# Deployment
	## Docs: https://hexo.io/docs/deployment.html
	deploy:
	  type: git
	  repository: git@github.com:chhzhangs/chhzhangs.github.io.git
	  branch: master

执行命令:

	hexo clean #静态网页清除生成的
	hexo g  # 生成静态网页
	hexo d # 同步到github上

就将hexo部署在github上了，通过chhzhangs.github.io就可以访问个人博客了。

# next #

next的官方网站有很详细的说明：http://theme-next.iissnan.com/getting-started.html

在本地修改完这一连串的配置，(包括：语言设置、财产、菜单设置、侧栏设置、头像设置、作者名称、站点描述、标签云页面、分类页面、统计系统、评论系统等等)。


# hexo不同电脑同步 #
转载：
> 一、关于搭建的流程
> 
> 1. 创建仓库，http://CrazyMilk.github.io；
> 2. 创建两个分支：master 与 hexo；
> 3. 设置hexo为默认分支（因为我们只需要手动管理这个分支上的Hexo网站文件）；
> 4. 使用git clone git@github.com:CrazyMilk/CrazyMilk.github.io.git拷贝仓库；
> 5. 在本地http://CrazyMilk.github.io文件夹下通过Git bash依次执行npm install hexo、hexo init、npm install 和 npm install hexo-deployer-git（此时当前分支应显示为hexo）;
> 6. 修改_config.yml中的deploy参数，分支应为master；
> 7. 依次执行git add .、git commit -m "..."、git push origin hexo提交网站相关的文件；
> 8. 执行hexo g -d生成网站并部署到GitHub上。
> 
> 这样一来，在GitHub上的http://CrazyMilk.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。完美( •̀ ω •́ )y！
> 
> 二、关于日常的改动流程
> 在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理。
> 
> 1. 依次执行git add .、git commit -m "..."、git push origin hexo指令将改动推送到GitHub（此时当前分支应为hexo）；
> 2. 然后才执行hexo g -d发布网站到master分支上。
> 
> 虽然两个过程顺序调转一般不会有问题，不过逻辑上这样的顺序是绝对没问题的（例如突然死机要重装了，悲催....的情况，调转顺序就有问题了）。
> 
> 三、本地资料丢失后的流程
> 当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：
> 
> 1. 使用git clone git@github.com:CrazyMilk/CrazyMilk.github.io.git拷贝仓库（默认分支为hexo）；
> 2. 在本地新拷贝的http://CrazyMilk.github.io文件夹下通过Git bash依次执行下列指令：npm install hexo、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令）。
> 
> 作者：CrazyMilk
> 链接：https://www.zhihu.com/question/21193762/answer/79109280
> 来源：知乎
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

步骤一的第五步会出现问题：

hexo init会删除.git目录，需要重新建立本地仓库与远程仓库的链接。
重新git init，然后再建立一个新的分支hexo，同步之后再执行，再push就成功了。

# 参考资料 #
1. [如何使用10个小时搭建出个人域名而又Geek的独立博客？](http://blog.shijinrong.cn/2016/01/03/2016-01-03-how-to-build-blog/)
2. [next官方网站](http://theme-next.iissnan.com/getting-started.html)
3. [hexo官方网站](https://hexo.io/zh-cn/docs/setup.html)
4. [GitHub Pages + Hexo搭建博客](http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more)