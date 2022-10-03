### Hexo的搭建 ###

```in
安装Nodejs
node -v	#查看node版本
npm -v	#查看npm版本
上述步骤查看环境变量是否配置成功（下同）
npm install -g cnpm --registry=http://registry.npm.taobao.org	#安装淘宝的cnpm 管理器
cnpm -v	#查看cnpm版本
cnpm install -g hexo-cli    #安装hexo框架
hexo -v	#查看hexo版本
mkdir blog	#创建blog目录
cd blog	 #进入blog目录
sudo hexo init 	#生成博客 初始化博客
hexo s	#启动本地博客服务
http://localhost:4000/	#本地访问地址
hexo n "我的第一篇文章" #创建新的文章 
#返回blog目录
hexo clean #清理
hexo g #生成
#Github创建一个新的仓库 YourGithubName.github.io
cnpm install --save hexo-deployer-git #在blog目录下安装git部署插件
----
#配置_config.yml 
-----
	# Deployment
	## Docs: https://hexo.io/docs/deployment.html
	deploy:
  		type: git
 		repo: https://github.com/YourGithubName/YourGithubName.github.io.git
  		branch: master
-----
hexo d	#部署到Github仓库里
https://YourGithubName.github.io/  #访问这个地址可以查看博客

 git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia  #下载yilia主题到本地

#修改hexo根目录下的 _config.yml 文件 ： theme: yilia

1.hexo c	#清理一下
2.hexo g	#生成
3.hexo d	#部署到远程Github仓库
注:2和3步可简写 hexo g -d
https://YourGithubName.github.io/  #查看博客
```



### Hexo 部署到github ### 

一. 在Git bash界面下设置ssh key

```in
#这将按照你提供的邮箱地址，创建一对密钥，后续用于github中ssh的绑定
ssh-keygen -t rsa -C "your_email@example.com"
输入后会出现key的存放位置，直接默认回车即可。
接下来回车两次，即默认空密码。结果如下图所示。
我这里是默认路径C:\Users\Administrator\.ssh，
复制id_rsa.pub这个文件里面的所有内容就是我们要的ssh key
```

![image-20220426204826810](G:\学习笔记\pic\image-20220426204826810.png)

二 . 复制ssh key，绑定到github账户中去

![image-20220426205214838](G:\学习笔记\pic\image-20220426205214838.png)

![image-20220426205316896](G:\学习笔记\pic\image-20220426205316896.png)

三. 测试能否成功连接

```in
Git bash界面下输入该命令： ssh -T git@github.com
如下图所示，出现Hi XXXX! You've successfully authenticated, but GitHub does not provide shell access.
即成功。
```

![image-20220426205536711](G:\学习笔记\pic\image-20220426205536711.png)

四.设置账户信息

```in
git config --global user.name "you_name"//github的用户名
git config --global user.email  "you@exmaple.com"//github的邮箱
```

![image-20220426205643158](G:\学习笔记\pic\image-20220426205643158.png)



### Hexo主题配置 ###

```in
一.选择主题
github中找主题，例如https://github.com/fluid-dev/hexo-theme-fluid

二.主题下载
方式一：git clone git项目地址 安装地址
例, git clone https://github.com/fluid-dev/hexo-theme-fluid themes/fluid
注意：fatal: unable to access 'https://github.com/DIYgod/hexo-theme-sagiri/': OpenSSL SSL_read: Connection was reset, errno 10054
这个错误可能是连接git的网络不稳定，多试几次。
方式二：npm install 安装
例，npm install --save hexo-theme-fluid
具体见各主题说明

三.修改hexo站点配置文件
在hexo生成的文件夹下，修改config.yml文件，找到theme,对应主题的名称。
例，theme：fluid

四.重新生成运行
1.hexo clean
2.hexo g
3.hexo s
```



### Hexo网页标题乱码问题 ###

```in
1.打开hexo站点的config配置文件，修改language: zh-CN.
2.保存文件的格式需要以utf-8来保存，这里推荐以vscode或者editplus等软件编辑。
```



### Hexo页面部署成功，打开界面缺显示404 ###

```in
因为页面缓存的问题,用Ctrl+F5刷新成功完成.
参照该博客，https://chenpt.cc/hexo-Error/ 
```



### hexo 缺少git组件 ###

```in
原因：少了hexo针对git的deploy组件
解决方法：npm install --save hexo-deployer-git
```

