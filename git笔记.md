初始化	git init

让git管理	git add . (.表示所有未被管理的文件，也可以 git add fileName)

配置个人信息

git config --global user.email " 邮箱"

git config --global user.name "姓名"

生成版本	git commit -m 'description'

查看版本记录

git log

ps： 在reset 回到之前版本时，在时间线上，HEAD指向的点之后的 版本记录都看不到

如果想知道所有版本变更历史，

改用

git reflog



切换到指定版本：（ v3回滚到v2，以及v2再回滚到v3）

先用 git reflog 查看版本变更记录，

然后用 git reset --hard 版本号， 这里的版本号在git reflog执行后会显示，16进制的大数

![image-20210608021918211](git%E7%AC%94%E8%AE%B0.assets/image-20210608021918211.png)



查看分支

git branch

创建分支

git branch 分支名

切换分支

git checkout 分支名

分支合并

git merge  要合并的分支

注意:切换分支后再合并,合并后可能要解决冲突

删除分支

git branch -d 分支名





2.5.4 git 工作流

![image-20210610233522984](git%E7%AC%94%E8%AE%B0.assets/image-20210610233522984.png)

## 2.6   与远端协同

​		在家里上传代码

```
1. 给远程仓库起别名	(仅一次)
	git remote add origin 远程仓库地址
2. 向远程推送代码
	git push -u origin 分支名称
```

​		到公司新电脑上第一次获取代码

```
1. 克隆远程仓库地址
	git clone 远程仓库地址 (内部已实现git remote add origin 远程仓库地址, 即起别名为origin)
2. 切换分支
	git checkout 分支
```

​		在公司进行开发

```
1. 切换到dev分支进行开发
	git checkout dev
2. dev代码有点老,先把 master分支合并到dev (仅一次)
	git merge master
3. 修改代码
4. 提交代码, 下班
	git add .
	git commit -m "xx"
	git push origin dev
```

​		到家里继续写代码

```
1. 切换到dev分支进行开发
	git checkout dev
2. 拉代码
	git pull origin dev
3. 继续开发
4. 提交代码
	git add .
	git commit -m "xx"
	git push origin dev
```

### 重复如上

开发完毕, 要上线

```
1.将dev分支合并到master,进行上线
	git checkout master
	git merge dev
	git push origin master
2. 把dev分支有也推送到远端
	git checkout dev
	git merge master
	git push orig
```









