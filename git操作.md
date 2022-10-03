#### github代码下载
1. 选择对应的文件夹(最好为空)
2. 快捷键右键(git bash here) 或者 手动打开git命令窗口跳转到指定文件夹目录下
3. 在命令行窗口中输入git init，创建.git文件
4. 使用git clone xxx ，命令，完成下载。 （xxx是代码库的下载链接）

#### 本地代码推送
```
初始化：
git init ：会创建一个.git文件，初始化本地git仓库；
git add .：将本地仓库内所有文件添加到缓冲区；
git commit -m "你的提交备注"：把文件提交到版本库，输入提交备注；
git remote add origin 远程库地址：把本地仓库与远程仓库管理；
git push -u origin master：把本地代码推送到远程代码仓库。
 

更新代码：
git add .：将本地仓库内的所有文件添加到缓冲区；
git commit -m "提交备注"：把文件提交到版本库；
git pull origin master：把远程仓库的代码拉下来合并到本地；
git push origin master：把本地代码推送到远程代码仓库。
```